---
title: "Sociální Framework"
description: "Sociální Framework poskytuje jednotné rozhraní API pro interakci se sociálními sítěmi, včetně Twitteru a Facebooku, jakož i SinaWeibo pro uživatele v Číně."
ms.topic: article
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 9892577d7e0ed3d3f622f881cc51db09eb44a8fd
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="social-framework"></a>Sociální Framework

_Sociální Framework poskytuje jednotné rozhraní API pro interakci se sociálními sítěmi, včetně Twitteru a Facebooku, jakož i SinaWeibo pro uživatele v Číně._


Pomocí rozhraní sociálních umožňuje aplikacím komunikovat se sociálními sítěmi z jediného rozhraní API bez nutnosti ke správě ověřování. Obsahuje systém poskytuje řadiče zobrazení pro vytváření příspěvků, jakož i abstrakci, která umožňuje využívání rozhraní API každý sociální sítě prostřednictvím protokolu HTTP.

> [!IMPORTANT]
> **Poznámka:** pro různé platformy API pro připojení k různým sociálních sítí, najdete v článku [Xamarin.Social](http://components.xamarin.com/view/xamarin.social/) součásti v úložišti součástí Xamarin.

## <a name="connecting-to-twitter"></a>Připojování ke službě Twitter

### <a name="twitter-account-settings"></a>Nastavení účtu služby Twitter.

Pro připojení k Twitter, pomocí rozhraní sociálních, účet je potřeba nakonfigurovat v nastavení zařízení, jak je uvedeno níže:

 [![](social-framework-images/twitter01.png "Nastavení účtu služby Twitter.")](social-framework-images/twitter01.png#lightbox)

Jakmile je účet zadaný a ověření u služby Twitter, bude tento účet používat všechny aplikace na zařízení, které používá sociálních Framework třídy pro přístup k Twitter.

### <a name="sending-tweets"></a>Odesílání Tweetů

Sociální Framework zahrnuje řadič názvem `SLComposeViewController` , uvede systému podle zobrazení pro úpravy a odesílání tweet. Následující snímek obrazovky ukazuje příklad v tomto zobrazení:

 [![](social-framework-images/twitter02.png "Tento snímek obrazovky ukazuje příklad SLComposeViewController")](social-framework-images/twitter02.png#lightbox)

Používat `SLComposeViewController` službou Twitter, musí být vytvořena instance řadiče voláním `FromService` metoda s `SLServiceType.Twitter` jak je uvedeno níže:

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

Po `SLComposeViewController` instance je vrácena, může sloužit k dispozici uživatelské rozhraní k vystavování příspěvků na Twitteru. Prvním krokem je však v takovém případě zkontrolujte dostupnost dané sociální síti Twitter voláním `IsAvailable`:

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController` nikdy odešle tweet přímo bez zásahu uživatele. Však může být inicializována pomocí následujících metod:

-   `SetInitialText` – Přidá úvodní text, který se zobrazí v tweet. 
-  `AddUrl` – Přidá do tweet adresu Url.
-  `AddImage` – Přidá bitovou kopii tweet.


Jakmile inicializován, volání `PresentVIewController` zobrazení vytvořené `SLComposeViewController`. Uživatele můžete pak můžete upravit a odeslat tweet nebo zrušte odesláním. V obou případech by měl být řadičem zavře v `CompletionHandler`, kde výsledek můžete také zkontrolovat, najdete v části Pokud tweet byl odeslán nebo zrušit, jak je uvedeno níže:

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>Příklad tweet

Následující kód ukazuje, jak pomocí `SLComposeViewController` nabídne zobrazení používá k odeslání tweet:

```csharp
using System;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _twitterComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
        #endregion

        #region Computed Properties
        public bool isTwitterAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Twitter); }
        }

        public SLComposeViewController TwitterComposer {
            get { return _twitterComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            SendTweet.Enabled = isTwitterAvailable;
        }
        #endregion

        #region Actions
        partial void SendTweet_TouchUpInside (UIButton sender)
        {
            // Set initial message
            TwitterComposer.SetInitialText ("Hello Twitter!");
            TwitterComposer.AddImage (UIImage.FromFile ("Icon.png"));
            TwitterComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (TwitterComposer, true, null);
        }
        #endregion
    }
}
```

### <a name="calling-twitter-api"></a>Volání rozhraní API služby Twitter.

Sociální Framework zahrnuje taky podporu požadavků protokolu HTTP na sociálních sítích. Zapouzdřit požadavku v `SLRequest` třídu, která se používá k cílové rozhraní API konkrétní sociálních sítí.

Například následující kód vytvoří žádost služby Twitter. Chcete-li získat veřejný časovou osu (rozšířením na kódu uvedeného výše):

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _twitterAccount;
#endregion

#region Computed Properties
public ACAccount TwitterAccount {
    get { return _twitterAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    SendTweet.Enabled = isTwitterAvailable;
    RequestTwitterTimeline.Enabled = false;

    // Initialize Twitter Account access 
    var accountStore = new ACAccountStore ();
    var accountType = accountStore.FindAccountType (ACAccountType.Twitter);

    // Request access to Twitter account
    accountStore.RequestAccess (accountType, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestTwitterTimeline.Enabled = true;
            });
        }
    });
}
#endregion

#region Actions
partial void RequestTwitterTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
    var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = TwitterAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Podívejme se na tento kód podrobně. Nejdřív získá přístup k účtu úložiště a získá typ účtu sítě Twitter:

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

V dalším kroku požádá uživatele, pokud vaše aplikace může mít přístup k účtu služby Twitter, a pokud je přístup povolen, účet je načten do paměti a aktualizovat uživatelské rozhraní:

```csharp
// Request access to Twitter account
accountStore.RequestAccess (accountType, (granted, error) => {
    // Allowed by user?
    if (granted) {
        // Get account
        _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
        InvokeOnMainThread (() => {
            // Update UI
            RequestTwitterTimeline.Enabled = true;
        });
    }
});
```

Když si uživatel vyžádá data časové osy (klepnutím na tlačítko v uživatelském rozhraní), tvoří aplikaci nejprve žádost o přístup k datům z Twitteru:

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```
V tomto příkladu je omezení vrácených výsledků pro posledních deset položky zahrnutím `?count=10` v adrese URL. Nakonec připojí požadavek na účet služby Twitter, (který byl načten výše) a provádí volání služby Twitter načíst data:

```csharp
// Request data
request.Account = TwitterAccount;
request.PerformRequest ((data, response, error) => {
    // Was there an error?
    if (error == null) {
        // Was the request successful?
        if (response.StatusCode == 200) {
            // Yes, display it
            InvokeOnMainThread (() => {
                Results.Text = data.ToString ();
            });
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", response.StatusCode);
            });
        }
    } else {
        // No, display error
        InvokeOnMainThread (() => {
            Results.Text = string.Format ("Error: {0}", error);
        });
    }
});
```

Pokud data byla úspěšně načtena, zobrazí se nezpracovaná data JSON (jako následující příklad výstupu):

[![](social-framework-images/twitter03.png "Příklad zobrazení nezpracovaná data JSON")](social-framework-images/twitter03.png#lightbox)

V reálné aplikaci může výsledky JSON pak analyzovat jako normální a výsledky uživateli. V tématu [Úvod webové služby](~/cross-platform/data-cloud/web-services/index.md) informace o tom, jak analyzovat JSON.

## <a name="connecting-to-facebook"></a>Připojení k síti Facebook

### <a name="facebook-account-settings"></a>Nastavení účtu služby Facebook

Připojování ke službě Facebook s sociálních Framework je téměř stejný jako proces použitý pro Twitter uvedené výše. Uživatelský účet Facebook musí být nakonfigurován v nastavení zařízení, jak je uvedeno níže:

[![](social-framework-images/facebook01.png "Nastavení účtu služby Facebook")](social-framework-images/facebook01.png#lightbox)

Po nakonfigurování všech aplikací na zařízení, které používá rozhraní sociálních použije tento účet pro připojení k síti Facebook.

### <a name="posting-to-facebook"></a>Publikování na Facebooku

Sociální Framework je jednotné rozhraní API, navržená tak, aby přístup k několika sociálních sítí, zůstane kód skoro stejné bez ohledu na dané sociální síti používá.

Například `SLComposeViewController` lze přesně jako v příkladu Twitter uvedena výše, jenom jiné přepíná na možnosti a nastavení specifických pro Facebook. Příklad:

```csharp
using System;
using Foundation;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _facebookComposer = SLComposeViewController.FromService (SLServiceType.Facebook);
        #endregion

        #region Computed Properties
        public bool isFacebookAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Facebook); }
        }

        public SLComposeViewController FacebookComposer {
            get { return _facebookComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            PostToFacebook.Enabled = isFacebookAvailable;
        }
        #endregion

        #region Actions
        partial void PostToFacebook_TouchUpInside (UIButton sender)
        {
            // Set initial message
            FacebookComposer.SetInitialText ("Hello Facebook!");
            FacebookComposer.AddImage (UIImage.FromFile ("Icon.png"));
            FacebookComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (FacebookComposer, true, null);
        }
        #endregion
    }
}
```

Při použití s Facebook, `SLComposeViewController` zobrazí zobrazení, která vypadá téměř stejný jako příklad Twitter zobrazující **Facebook** jako nadpis v tomto případě:

[![](social-framework-images/facebook02.png "Zobrazení SLComposeViewController")](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Volání rozhraní API grafu služby Facebook

Podobně jako v příkladu Twitter, sociálních Framework `SLRequest` objekt lze použít s graph API sítě Facebook společnosti. Například následující kód vrací informace z grafu rozhraní API o účtu Xamarin (rozšířením na kód výše uvedené):

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _facebookAccount;
#endregion

#region Computed Properties
public ACAccount FacebookAccount {
    get { return _facebookAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    PostToFacebook.Enabled = isFacebookAvailable;
    RequestFacebookTimeline.Enabled = false;

    // Initialize Facebook Account access 
    var accountStore = new ACAccountStore ();
    var options = new AccountStoreOptions ();
    var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
    accountType = accountStore.FindAccountType (ACAccountType.Facebook);

    // Request access to Facebook account
    accountStore.RequestAccess (accountType, options, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _facebookAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestFacebookTimeline.Enabled = true;
            });
        }
    });

}
#endregion

#region Actions
partial void RequestFacebookTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl ("https://graph.facebook.com/283148898401104");
    var request = SLRequest.Create (SLServiceKind.Facebook, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = FacebookAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Pouze skutečné rozdíl mezi tento kód a Twitter verze uvedené výš, je požadavek na Facebooku získat vývojáře nebo konkrétní ID aplikace (který můžete vygenerovat z portálu pro vývojáře pro Facebook) který musí být nastaven jako možnost, při vytváření požadavku:

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

Nepodařilo se nastavit tuto možnost (nebo pomocí neplatný klíč) způsobí chybu nebo je nevrátil žádná data.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak použít sociálních architekturu pro interakci s Twitteru a Facebooku. Je vám ukázal, kde má být konfigurace účtů pro každý sociálních sítí v nastavení zařízení. Popsány i způsob použití `SLComposeViewController` k dispozici jednotný pohled pro příspěvků na sociálních sítích. Kromě toho zkontrolován `SLRequest` třídu, která se používá k volání rozhraní API každý sociálních sítí.


## <a name="related-links"></a>Související odkazy

- [SocialFrameworkDemo (sample)](https://developer.xamarin.com/samples/SocialFrameworkDemo/)
- [Úvod k webovým službám](~/cross-platform/data-cloud/web-services/index.md)
