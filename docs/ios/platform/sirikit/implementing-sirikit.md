---
title: Implementace SiriKit
description: Tento článek popisuje kroky nutné k implementaci SiriKit podpory v aplikacích pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: a4f38e93cae3c9577a0b1e32067da2cfd2e4796d
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="implementing-sirikit"></a>Implementace SiriKit

_Tento článek popisuje kroky nutné k implementaci SiriKit podpory v aplikacích pro Xamarin.iOS._



Nové iOS 10 SiriKit povoluje aplikace Xamarin.iOS k poskytování služeb, které jsou dostupné pro uživatele na zařízení s iOS pomocí Siri a aplikace mapy. Tento článek popisuje kroky nutné k implementaci SiriKit podpory v aplikace pro Xamarin.iOS přidáním požadované rozšíření tříd Intent, rozšíření tříd Intent uživatelského rozhraní a termínů.

Siri pracuje s koncept **domény**, skupiny víte akce pro související úlohy. Každý interakce, který má aplikace s Siri musí spadat do jednoho z jeho domény známé služby následujícím způsobem:

- Zvuk nebo video volání.
- Rezervace pravé.
- Správa cvičení nebo.
- Zasílání zpráv.
- Hledání fotografie.
- Odesílání nebo přijímání platby.

Když uživatel odešle žádost siri jeden rozšíření aplikace služby, odešle SiriKit rozšíření **záměr** objekt, který popisuje žádost uživatele spolu s podpůrné daty. Rozšíření aplikace vygeneruje odpovídající **odpovědi** objekt pro v dané **záměr**, s podrobnostmi o tom, jak můžete rozšíření žádost zpracovat.

Tato příručka nabídne zběžný příklad včetně podpory SiriKit do stávající aplikace. Z důvodu v tomto příkladu budeme používat falešných MonkeyChat aplikace:

[![](implementing-sirikit-images/monkeychat01.png "Ikona MonkeyChat")](implementing-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat udržuje vlastní kontaktní adresáře uživatele přátel, každý přidružený název obrazovky (např. Bobo třeba) a umožňuje uživatelům poslat textové konverzace každý friend podle názvu jejich obrazovky.

## <a name="extending-the-app-with-sirikit"></a>Rozšíření aplikace pomocí SiriKit

Jak je znázorněno [Principy SiriKit koncepty](~/ios/platform/sirikit/understanding-sirikit.md) průvodce, se účastní rozšíření aplikace pomocí SiriKit tři hlavní části:

[![](implementing-sirikit-images/elements01.png "Rozšíření aplikace pomocí SiriKit diagram")](implementing-sirikit-images/elements01.png#lightbox)

Mezi ně patří:

1. **Rozšíření tříd Intent** -ověřuje uživatele odpovědi, potvrdí aplikace dokáže zpracovat žádost a ve skutečnosti provádí úlohy ke splnění požadavku uživatele.
2. **Rozšíření tříd Intent uživatelského rozhraní** - *volitelné*, obsahuje vlastní uživatelské rozhraní k odpovědím v prostředí Siri a můžete zahrnout aplikace uživatelského rozhraní a branding do Siri rozšířit možnosti pro uživatele.
3. **Aplikace** -poskytuje aplikace s konkrétní vocabularies uživatele Siri jako pomůcku při práci s ním. 

Všechny tyto prvky a kroky pro zahrnutí do aplikace se bude vztahovat podrobně v následujících částech.


## <a name="preparing-the-app"></a>Příprava aplikace

SiriKit je založený na rozšíření, ale před přidáním jakékoli rozšíření do aplikace, existuje pár věcí, které je potřeba udělat, aby s přijetím SiriKit vývojář.

### <a name="moving-common-shared-code"></a>Přesunutí společný kód sdílený 

Nejprve Vývojář můžete přesunout některé běžné kód, který bude sdílena mezi aplikace a rozšíření do buď _sdílených projektů_, _přenosné knihovny tříd (PCLs)_ nebo _nativní Knihovny_.

Rozšíření bude muset být moct provádět všechny možnosti, které aplikace nepodporuje. V podmínky MonkeyChat ukázkové aplikace, třeba hledání kontaktů, přidávání nových kontaktů, odesílání zpráv a načíst historii zpráv.

Přesunutím tento společný kód do sdílených projektů, PCL nebo nativní knihovny usnadňuje udržovat tento kód na jednom místě, běžné a zajistí, že rozšíření a nadřazená aplikace poskytují jednotné prostředí a funkce pro uživatele.

V případě aplikace příklad MonkeyChat datové modely a zpracování kód například sítě a databáze přístup přesunou se do nativní knihovny.

Postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Spuštění sady Visual Studio pro Mac a otevřete aplikaci MonkeyChat.
2. Klikněte pravým tlačítkem na název řešení v **řešení Pad** a vyberte **přidat** > **nový projekt...** : 

    [![](implementing-sirikit-images/prep01.png "Přidat nový projekt")](implementing-sirikit-images/prep01.png#lightbox)
3. Vyberte **iOS** > **knihovny** > **knihovny tříd** a klikněte na **Další** tlačítko: 

    [![](implementing-sirikit-images/prep02.png "Vyberte knihovnu – třída")](implementing-sirikit-images/prep02.png#lightbox)
4. Zadejte `MonkeyChatCommon` pro **název** a klikněte na **vytvořit** tlačítko: 

    [![](implementing-sirikit-images/prep03.png "Zadejte pro název MonkeyChatCommon")](implementing-sirikit-images/prep03.png#lightbox)
5. Klikněte pravým tlačítkem na **odkazy** složky hlavní aplikace v **Průzkumníku řešení** a vyberte **upravit odkazy...** . Zkontrolujte **MonkeyChatCommon** projektu a klikněte na **OK** tlačítko: 

    [![](implementing-sirikit-images/prep05.png "Zkontrolujte MonkeyChatCommon projektu")](implementing-sirikit-images/prep05.png#lightbox)
6. V **Průzkumníku**, přetáhněte společný kód sdílený z hlavní aplikace do nativní knihovny.
7. V případě MonkeyChat, přetáhněte **DataModels** a **procesory** složky z hlavní aplikace do nativní knihovny: 

    [![](implementing-sirikit-images/prep06.png "DataModels a procesory složky v Průzkumníku řešení")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Spuštění sady Visual Studio a otevřete aplikaci MonkeyChat.
2. Klikněte pravým tlačítkem na název řešení v **Průzkumníku řešení** a vyberte **přidat** > **nový projekt...** .
3. Vyberte **Visual C#** > **sdílený projekt** a klikněte na **Další** tlačítko: 

    [![](implementing-sirikit-images/prep02.w157-sml.png "Vyberte knihovnu – třída")](implementing-sirikit-images/prep02.w157.png#lightbox)
4. Zadejte `MonkeyChatCommon` pro **název** a klikněte na **vytvořit** tlačítko.
5. Klikněte pravým tlačítkem na **odkazy** složky hlavní aplikace v **Průzkumníku řešení** a vyberte **upravit odkazy...** . Zkontrolujte **MonkeyChatCommon** projektu a klikněte na **OK** tlačítko: 

    [![](implementing-sirikit-images/prep05w.png "Zkontrolujte MonkeyChatCommon projektu")](implementing-sirikit-images/prep05w.png#lightbox)
6. V **Průzkumníku**, přetáhněte společný kód sdílený z hlavní aplikace na sdílený projekt.
7. V případě MonkeyChat, přetáhněte **DataModels** a **procesory** složky z hlavní aplikace do nativní knihovny.

-----

Upravte žádné soubory, které byly přesunuty do nativní knihovny a změňte obor názvů odpovídat knihovny. Například změna `MonkeyChat` k `MonkeyChatCommon`:

```csharp
using System;
namespace MonkeyChatCommon
{
    /// <summary>
    /// A message sent from one user to another within a conversation.
    /// </summary>
    public class MonkeyMessage
    {
        public MonkeyMessage ()
        {
        }
        ...
    }
}
```

Dále přejděte zpět na hlavní aplikaci a přidejte `using` příkaz pro obor názvů nativní knihovny kdekoli aplikace používá jednu z tříd, které byly přesunuté:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using MonkeyChatCommon;

namespace MonkeyChat
{
    public partial class MasterViewController : UITableViewController
    {
        public DetailViewController DetailViewController { get; set; }

        DataSource dataSource;
        ...
    }
}
```

### <a name="architecting-the-app-for-extensions"></a>Pro rozšíření architektury aplikace

Obvykle bude aplikaci zaregistrovat pro více tříd Intent a vývojář musí zajistit, že aplikace je navržen pro příslušný počet záměr rozšíření.

V situaci, kdy aplikace vyžaduje více než jeden záměr má vývojář možnost uvedení všechny jeho záměrné zpracování jedno rozšíření záměr nebo vytvoření samostatné záměr přípony pro každý záměr.

Pokud se rozhodnete vytvořit samostatné záměr přípony pro každý záměr, může vývojář skončili duplikování velké množství často používaný kód v každé rozšíření a vytvořte velké nároky na procesor a paměť režii.

Pomoc při výběru mezi dvě možnosti, naleznete v článku Pokud žádné z záměry přirozeně patří společně. Například aplikace, která provedené audio a video volání chtít zahrnout obou těchto tříd Intent do jednoho rozšíření záměr, jako jsou jejich zpracování podobných úloh a může poskytovat většinu kódu opakovaného použití.

Záměr nebo skupiny záměry, které se nehodí do existující skupiny vytvořte v řešení aplikace tak, aby obsahovala je nové rozšíření záměr.


### <a name="setting-the-required-entitlements"></a>Nastavení požadovaná oprávnění

Jakékoli aplikaci Xamarin.iOS, která zahrnuje SiriKit integraci, musí mít správná oprávnění nastavený. Pokud vývojář není správně nastavena tato požadovaná oprávnění, nebudou moct instalovat nebo testování aplikace na 10 (nebo vyšší) skutečné iOS hardwaru, který je taky požadavek od iOS 10 simulátoru nepodporuje SiriKit.

Postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Dvakrát klikněte `Entitlements.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.
2. Přepnout **zdroj** kartě.
3. Přidat `com.apple.developer.siri` **vlastnost**, nastavte **typ** k `Boolean` a **hodnotu** k `Yes`: 

    [![](implementing-sirikit-images/setup01.png "Přidat vlastnost com.apple.developer.siri")](implementing-sirikit-images/setup01.png#lightbox)
4. Uložte změny do souboru.
5. Dvakrát klikněte **soubor projektu** v **Průzkumníku řešení** otevřete pro úpravy.
6. Vyberte **iOS podepisování sady** a ujistěte se, že `Entitlements.plist` je vybrán soubor v **vlastní oprávnění** pole: 

    [![](implementing-sirikit-images/setup02.png "Vyberte soubor Entitlements.plist v poli vlastní oprávnění")](implementing-sirikit-images/setup02.png#lightbox)
7. Klikněte **OK** tlačítko a uložte změny.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dvakrát klikněte `Entitlements.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.
3. Přidat `com.apple.developer.siri` **vlastnost**, nastavte **typ** k `Boolean` a **hodnotu** k `Yes`: 

    [![](implementing-sirikit-images/setup01w.png "Přidat vlastnost com.apple.developer.siri")](implementing-sirikit-images/setup01w.png#lightbox)
4. Uložte změny do souboru.
5. Dvakrát klikněte **soubor projektu** v **Průzkumníku řešení** otevřete pro úpravy.
6. Vyberte **iOS podepisování sady** a ujistěte se, že `Entitlements.plist` je vybrán soubor v **vlastní oprávnění** pole.

-----

Po dokončení aplikace `Entitlements.plist` soubor by měl vypadat takto (ve otevřen v editoru externí):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.siri</key>
    <true/>
</dict>
</plist>
```

### <a name="correctly-provisioning-the-app"></a>Správně zřizování aplikace

Vzhledem k striktní zabezpečení, které Apple umístěno kolem rozhraní SiriKit jakékoli aplikaci Xamarin.iOS, která implementuje SiriKit _musí_ správné ID aplikace a oprávnění (viz část výše) a musí být podepsané správné Profil pro zřizování.

Proveďte v počítači Mac:

1. Ve webovém prohlížeči, přejděte na [ http://developer.apple.com ](http://developer.apple.com) a přihlaste se k účtu.
2. Klikněte na **certifikáty**, **identifikátory** a **profily**.
3. Vyberte **profily zřizování** a vyberte **ID aplikace**, klikněte **+** tlačítko.
4. Zadejte **název** nového profilu.
5. Zadejte **ID sady** následující Apple názvů prvku doporučení.
6. Přejděte dolů k položce **App Services** vyberte **SiriKit** a klikněte na tlačítko **pokračovat** tlačítko: 

    [![](implementing-sirikit-images/setup03.png "Vyberte SiriKit")](implementing-sirikit-images/setup03.png#lightbox)
7. Zkontrolujte všechna nastavení, pak **odeslání** ID aplikace.
8. Vyberte **profily zřizování** > **vývoj**, klikněte **+** tlačítko, vyberte **Apple ID**, pak klikněte na tlačítko **pokračovat**.
9. Klikněte na vybrat **všechny**, pak klikněte na tlačítko **pokračovat**.
10. Klikněte na tlačítko **Vybrat vše** znovu, pak klikněte na tlačítko **pokračovat**.
11. Zadejte **název profilu** názvů pomocí Apple prvku návrhy, pak klikněte na tlačítko **pokračovat**.
12. Spusťte Xcode.
13. Vyberte z nabídky Xcode **předvolby...**
14. Vyberte **účty**, klikněte **zobrazit podrobnosti...** tlačítko: 

    [![](implementing-sirikit-images/setup04.png "Vyberte účty")](implementing-sirikit-images/setup04.png#lightbox)
15. Klikněte **stáhnout všechny profily** tlačítko v levém dolním: 

    [![](implementing-sirikit-images/setup05.png "Stáhnout všechny profily")](implementing-sirikit-images/setup05.png#lightbox)
16. Ujistěte se, že **profil zřizování** vytvořit vyšší nainstalován v Xcode.
17. Otevřete projekt přidat SiriKit podporu, aby v sadě Visual Studio for Mac.
18. Dvakrát klikněte `Info.plist` ve **Průzkumníku řešení**.
18. Ujistěte se, že **identifikátor svazku** odpovídá vytvořeném v portálu pro vývojáře Apple výše: 

    [![](implementing-sirikit-images/setup06.png "Identifikátor balíku")](implementing-sirikit-images/setup06.png#lightbox)
18. V **Průzkumníku řešení**, vyberte **projektu**.
19. Klikněte pravým tlačítkem na projekt a vyberte **možnosti**.
21. Vyberte **iOS podepisování sady**, vyberte **identitu podepisování** a **profil zřizování** vytvořili výše: 

    [![](implementing-sirikit-images/setup07.png "Vyberte identitu podepisování a profilu pro zřizování")](implementing-sirikit-images/setup07.png#lightbox)
22. Klikněte **OK** tlačítko a uložte změny.

> [!IMPORTANT]
> Testování SiriKit funguje pouze na skutečné iOS 10 hardwarové zařízení a nikoli v iOS 10 simulátoru. Pokud problémy instalace SiriKit s povolené aplikace Xamarin.iOS na skutečné hardwaru, ujistěte se, že požadovaná oprávnění, ID aplikace, identifikátor podepisování a profil zřizování byly správně nakonfigurovány v portálu pro vývojáře Apple i Visual Studio for Mac.

### <a name="requesting-siri-authorization"></a>Požaduje autorizaci Siri

Předtím, než aplikace přidá všechny termínů konkrétní uživatele nebo rozšíření tříd Intent připojí k Siri, se musí od uživatele pro přístup k Siri požaduje autorizaci.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Upravit aplikace `Info.plist` souboru, přepněte do **zdroj** zobrazení a přidat `NSSiriUsageDescription` klíče s hodnotou řetězce, které popisují, jak bude aplikace používat Siri a co typy dat budou odeslány. Řekněme například, MonkeyChat aplikace může "MonkeyChat kontakty do zašle Siri":

[![](implementing-sirikit-images/request01.png "NSSiriUsageDescription v editoru Info.plist")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Upravit aplikace `Info.plist` souboru a přidejte `NSSiriUsageDescription` klíče s hodnotou řetězce, které popisují, jak bude aplikace používat Siri a co typy dat budou odeslány. Řekněme například, MonkeyChat aplikace může "MonkeyChat kontakty do zašle Siri":

[![](implementing-sirikit-images/request01w.png "NSSiriUsageDescription v editoru Info.plist")](implementing-sirikit-images/request01w.png#lightbox)

-----

Volání `RequestSiriAuthorization` metodu `INPreferences` třídy při prvním spuštění aplikace. Upravit `AppDelegate.cs` třídy a ujistěte se, `FinishedLaunching` metoda vypadá takto:


```csharp
using Intents;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{

    // Request access to Siri
    INPreferences.RequestSiriAuthorization ((INSiriAuthorizationStatus status) => {
        // Respond to returned status
        switch (status) {
        case INSiriAuthorizationStatus.Authorized:
            break;
        case INSiriAuthorizationStatus.Denied:
            break;
        case INSiriAuthorizationStatus.NotDetermined:
            break;
        case INSiriAuthorizationStatus.Restricted:
            break;
        }
    });


    return true;
}
```

Při prvním tato metoda je volána, upozornění se zobrazí výzvy pro uživatele, aby mohla aplikace pro přístup k Siri. Zpráva, která vývojář přidán do `NSSiriUsageDescription` vyšší zobrazí se v této výstrahy. Pokud uživatel původně odmítne přístup, můžete použít **nastavení** aplikaci udělit přístup k aplikaci.

V každém okamžiku aplikace můžete zkontrolovat schopnost aplikace přistupovat Siri voláním `SiriAuthorizationStatus` metodu `INPreferences` třídy.

### <a name="localization-and-siri"></a>Lokalizace a Siri

Na zařízení s iOS je možné vybrat jazyk pro Siri, které se liší a výchozí systémové nastavení uživatele. Při práci s lokalizovaná data, aplikace bude muset použít `SiriLanguageCode` metodu `INPreferences` třídy z Siri získat kód jazyka. Příklad:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>Přidání uživatele konkrétní termínů

Slovník konkrétní uživatel bude poskytovat slova nebo fráze, které jsou jedinečné pro jednotlivé uživatele aplikace. Toto bude poskytnuta v době běhu z hlavní aplikace (ne rozšíření aplikace) jako seřazený sadu podmínek, seřazené nejvýznamnějších Priorita využití pro uživatele s podmínkami nejdůležitější na začátek seznamu.

Slovník konkrétní uživatel musí patřit do jedné z následujících kategorií:

- Obraťte se na názvy (které nejsou spravované přes rozhraní kontakty).
- Fotografie značky.
- Názvy alba fotografií.
- Cvičení názvy.

Když vyberete terminologie zaregistrovat jako vlastní slovník, vyberte pouze podmínky, může být nesprávně pochopeny někdo není obeznámeni s aplikací. Nikdy registrace běžné podmínky jako "Moje cvičení" nebo "Moje Album". Například aplikace MonkeyChat bude zaregistrovat přezdívky přidružené každý kontaktu v adresáři uživatele.

Aplikace obsahuje slovník konkrétní uživatel voláním `SetVocabularyStrings` metodu `INVocabulary` třídy a předávání v `NSOrderedSet` kolekce z hlavní aplikace. Aplikace by měly volat vždy `RemoveAllVocabularyStrings` metoda první, chcete-li odebrat existující podmínek před přidáním nové. Příklad:

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Foundation;
using Intents;

namespace MonkeyChatCommon
{
    public class MonkeyAddressBook : NSObject
    {
        #region Computed Properties
        public List<MonkeyContact> Contacts { get; set; } = new List<MonkeyContact> ();
        #endregion

        #region Constructors
        public MonkeyAddressBook ()
        {
        }
        #endregion

        #region Public Methods
        public NSOrderedSet<NSString> ContactNicknames ()
        {
            var nicknames = new NSMutableOrderedSet<NSString> ();

            // Sort contacts by the last time used
            var query = Contacts.OrderBy (contact => contact.LastCalledOn);

            // Assemble ordered list of nicknames by most used to least
            foreach (MonkeyContact contact in query) {
                nicknames.Add (new NSString (contact.ScreenName));
            }

            // Return names
            return new NSOrderedSet<NSString> (nicknames.AsSet ());
        }

        // This method MUST only be called on a background thread!
        public void UpdateUserSpecificVocabulary ()
        {
            // Clear any existing vocabulary
            INVocabulary.SharedVocabulary.RemoveAllVocabularyStrings ();

            // Register new vocabulary
            INVocabulary.SharedVocabulary.SetVocabularyStrings (ContactNicknames (), INVocabularyStringType.ContactName);
        }
        #endregion
    }
}
```

S tímto kódem na místě se může být například takto:

```csharp
using System;
using System.Threading;
using UIKit;
using MonkeyChatCommon;
using Intents;

namespace MonkeyChat
{
    public partial class ViewController : UIViewController
    {
        #region AppDelegate Access
        public AppDelegate ThisApp {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do we have access to Siri?
            if (INPreferences.SiriAuthorizationStatus == INSiriAuthorizationStatus.Authorized) {
                // Yes, update Siri's vocabulary
                new Thread (() => {
                    Thread.CurrentThread.IsBackground = true;
                    ThisApp.AddressBook.UpdateUserSpecificVocabulary ();
                }).Start ();
            }
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

> [!IMPORTANT]
> Siri zpracovává vlastní slovník jako pomocné parametry a bude obsahovat co nejvíc terminologie nejblíže. Však místo pro vlastní slovník je omezená, takže je potřeba zaregistrovat _pouze_ technologiím, které může být matoucí, proto zachování celkový počet registrovaných podmínky na minimum.

Další informace najdete v tématu naše [termínů odkaz na konkrétní uživatele](~/ios/platform/sirikit/understanding-sirikit.md) a společnosti Apple [zadání odkazu termínů vlastní](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

### <a name="adding-app-specific-vocabulary"></a>Přidání termínů konkrétní aplikace

Slovník konkrétní aplikace definuje konkrétní slova a slovní spojení, které bude známé pro všechny uživatele aplikace, jako jsou typy vozidel nebo názvy cvičení. Protože se jedná o část aplikace, jsou definovány v `AppIntentVocabulary.plist` souboru jako součást sady hlavní aplikace. Kromě toho se lokalizované tyto slova a slovní spojení.

Konkrétní aplikace termínů podmínky musí patřit do jedné z následujících kategorií:

- Přepravují možnosti.
- Cvičení názvy.

Soubor termínů konkrétní aplikace obsahuje dvě kořenové úrovně klíče:

- `ParameterVocabularies` **Požadované** -definuje vlastní podmínky a záměr parametry, které se vztahují na aplikace.
- `IntentPhrases` **Volitelné** – obsahuje příklad frází pomocí vlastní podmínky definovaný v `ParameterVocabularies`.

Každá položka v `ParameterVocabularies` musíte zadat řetězec ID, termín a záměr termín vztahující se k. Navíc jeden termín může použít pro více tříd Intent.

Úplný seznam přijatelných hodnot a požadovaný soubor strukturu, najdete v tématu společnosti Apple [referenční informace k aplikacím termínů soubor formátu](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

Chcete-li přidat `AppIntentVocabulary.plist` souboru do projektu aplikace, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Klikněte pravým tlačítkem myši na název projektu v **Průzkumníku řešení** a vyberte **přidat** > **nový soubor...**   >  **iOS**:

    [![](implementing-sirikit-images/plist01.png "Přidat seznam vlastností")](implementing-sirikit-images/plist01.png#lightbox)
2. Dvakrát klikněte `AppIntentVocabulary.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.
3. Klikněte na tlačítko **+** přidat klíč, nastavte **název** k `ParameterVocabularies` a **typ** k `Array`:

    [![](implementing-sirikit-images/plist02.png "Nastavte název na ParameterVocabularies a zadejte do pole")](implementing-sirikit-images/plist02.png#lightbox)
4. Rozbalte položku `ParameterVocabularies` a klikněte na tlačítko **+** tlačítko a nastavte **typ** k `Dictionary`:

    [![](implementing-sirikit-images/plist03.png "Nastavte typ slovníku")](implementing-sirikit-images/plist03.png#lightbox)
5. Klikněte na tlačítko **+** přidat nový klíč, nastavte **název** k `ParameterNames` a **typ** k `Array`:

    [![](implementing-sirikit-images/plist04.png "Nastavte název na ParameterNames a zadejte do pole")](implementing-sirikit-images/plist04.png#lightbox)
6. Klikněte **+** přidat nový klíč s **typ** z `String` a hodnotu jako jeden z dostupných názvy parametrů. Například `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05.png "Klíč INStartWorkoutIntent.workoutName")](implementing-sirikit-images/plist05.png#lightbox)
7. Přidat `ParameterVocabulary` klíče k `ParameterVocabularies` klíč s **typ** z `Array`:

    [![](implementing-sirikit-images/plist06.png "Přidejte klíč ParameterVocabulary ke klíči ParameterVocabularies s poli typu")](implementing-sirikit-images/plist06.png#lightbox)
8. Přidejte nový klíč s **typ** z `Dictionary`:

    [![](implementing-sirikit-images/plist07.png "Přidejte nový klíč s slovníku typů.")](implementing-sirikit-images/plist07.png#lightbox)
9. Přidat `VocabularyItemIdentifier` klíč s **typ** z `String` a zadejte jedinečné ID pro termín:

    [![](implementing-sirikit-images/plist08.png "Přidejte klíč VocabularyItemIdentifier s typu řetězec a zadejte jedinečné ID")](implementing-sirikit-images/plist08.png#lightbox)
10. Přidat `VocabularyItemSynonyms` klíč s **typ** z `Array`:

    [![](implementing-sirikit-images/plist09.png "Přidejte klíč VocabularyItemSynonyms s poli typu")](implementing-sirikit-images/plist09.png#lightbox)
11. Přidejte nový klíč s **typ** z `Dictionary`:

    [![](implementing-sirikit-images/plist10.png "Přidejte nový klíč s slovníku typů.")](implementing-sirikit-images/plist10.png#lightbox)
12. Přidat `VocabularyItemPhrase` klíč s **typ** z `String` a podmínek jsou definování aplikace:

    [![](implementing-sirikit-images/plist11.png "Přidejte klíč VocabularyItemPhrase s typu řetězec a termín definování aplikace")](implementing-sirikit-images/plist11.png#lightbox)
13. Přidat `VocabularyItemPronunciation` klíč s **typ** z `String` a výslovnosti výslovnosti podmínek:

    [![](implementing-sirikit-images/plist12.png "Přidejte klíč VocabularyItemPronunciation s typu řetězec a výslovnosti výslovnosti podmínek.")](implementing-sirikit-images/plist12.png#lightbox)
14. Přidat `VocabularyItemExamples` klíč s **typ** z `Array`:

    [![](implementing-sirikit-images/plist13.png "Přidejte klíč VocabularyItemExamples s poli typu")](implementing-sirikit-images/plist13.png#lightbox)
15. Přidat pár `String` klíče se používá příklad podmínek:

    [![](implementing-sirikit-images/plist14.png "Přidat pár klíčů řetězec se používá příklad podmínek.")](implementing-sirikit-images/plist14.png#lightbox)
16. Zopakujte výše uvedené kroky pro vlastní podmínky aplikace muset definovat.
17. Sbalit `ParameterVocabularies` klíč.
18. Přidat `IntentPhrases` klíč s **typ** z `Array`:

    [![](implementing-sirikit-images/plist15.png "Přidejte klíč IntentPhrases s poli typu")](implementing-sirikit-images/plist15.png#lightbox)
19. Přidejte nový klíč s **typ** z `Dictionary`:

    [![](implementing-sirikit-images/plist16.png "Přidejte nový klíč s slovníku typů.")](implementing-sirikit-images/plist16.png#lightbox)
20. Přidat `IntentName` klíč s **typ** z `String` a záměrné pro tento příklad:

    [![](implementing-sirikit-images/plist17.png "Přidejte klíč IntentName s řetězec typu a záměr pro tento příklad")](implementing-sirikit-images/plist17.png#lightbox)
21. Přidat `IntentExamples` klíč s **typ** z `Array`:

    [![](implementing-sirikit-images/plist18.png "Přidejte klíč IntentExamples s poli typu")](implementing-sirikit-images/plist18.png#lightbox)
22. Přidat pár `String` klíče se používá příklad podmínek:

    [![](implementing-sirikit-images/plist19.png "Přidat pár klíčů řetězec se používá příklad podmínek.")](implementing-sirikit-images/plist19.png#lightbox)
23. Výše uvedené kroky opakujte pro všechny záměry aplikace je potřeba zadat příklad použití.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Klikněte pravým tlačítkem myši na název projektu v **Průzkumníku řešení** a vyberte **Přidat > novou položku... > Apple > seznam vlastností > Info.plist**:

    [![](implementing-sirikit-images/plist01.w157-sml.png "Přidat nové Info.plist")](implementing-sirikit-images/plist01.w157.png#lightbox)

2. Dvakrát klikněte `AppIntentVocabulary.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.
3. Klikněte na tlačítko **+** přidat klíč, nastavte **název** k `ParameterVocabularies` a **typ** k `Array`:

    [![](implementing-sirikit-images/plist02w.png "Nastavte název na ParameterVocabularies a zadejte do pole")](implementing-sirikit-images/plist02w.png#lightbox)
4. Rozbalte položku `ParameterVocabularies` a klikněte na tlačítko **+** tlačítko a nastavte **typ** k `Dictionary`:

    [![](implementing-sirikit-images/plist03w.png "Nastavte typ slovníku")](implementing-sirikit-images/plist03w.png#lightbox)
5. Klikněte na tlačítko **+** přidat nový klíč, nastavte **název** k `ParameterNames` a **typ** k `Array`:

    [![](implementing-sirikit-images/plist04w.png "Nastavte název na ParameterNames a zadejte do pole")](implementing-sirikit-images/plist04w.png#lightbox)
6. Klikněte **+** přidat nový klíč s **typ** z `String` a hodnotu jako jeden z dostupných názvy parametrů. Například `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05w.png "Klíč INStartWorkoutIntent.workoutName")](implementing-sirikit-images/plist05w.png#lightbox)
7. Přidat `ParameterVocabulary` klíče k `ParameterVocabularies` klíč s **typ** z `Array`:

    [![](implementing-sirikit-images/plist06w.png "Přidejte klíč ParameterVocabulary ke klíči ParameterVocabularies s poli typu")](implementing-sirikit-images/plist06w.png#lightbox)
8. Přidejte nový klíč s **typ** z `Dictionary`:

    [![](implementing-sirikit-images/plist07w.png "Přidejte nový klíč s slovníku typů.")](implementing-sirikit-images/plist07w.png#lightbox)
9. Přidat `VocabularyItemIdentifier` klíč s **typ** z `String` a zadejte jedinečné ID pro termín:

    [![](implementing-sirikit-images/plist08w.png "Přidejte klíč VocabularyItemIdentifier s typu řetězec a zadejte jedinečné ID pro termín")](implementing-sirikit-images/plist08w.png#lightbox)
10. Přidat `VocabularyItemSynonyms` klíč s **typ** z `Array`:

    [![](implementing-sirikit-images/plist09w.png "Přidejte klíč VocabularyItemSynonyms s poli typu")](implementing-sirikit-images/plist09w.png#lightbox)
11. Přidejte nový klíč s **typ** z `Dictionary`:

    [![](implementing-sirikit-images/plist10w.png "Přidejte nový klíč s slovníku typů.")](implementing-sirikit-images/plist10w.png#lightbox)
12. Přidat `VocabularyItemPhrase` klíč s **typ** z `String` a podmínek jsou definování aplikace:

    [![](implementing-sirikit-images/plist11w.png "Přidejte klíč VocabularyItemPhrase s typu řetězec a termín definování aplikace")](implementing-sirikit-images/plist11w.png#lightbox)
13. Přidat `VocabularyItemPronunciation` klíč s **typ** z `String` a výslovnosti výslovnosti podmínek:

    [![](implementing-sirikit-images/plist12w.png "Přidejte klíč VocabularyItemPronunciation s typu řetězec a výslovnosti výslovnosti podmínek.")](implementing-sirikit-images/plist12w.png#lightbox)
14. Přidat `VocabularyItemExamples` klíč s **typ** z `Array`:

    [![](implementing-sirikit-images/plist13w.png "Přidejte klíč VocabularyItemExamples s poli typu")](implementing-sirikit-images/plist13w.png#lightbox)
15. Přidat pár `String` klíče se používá příklad podmínek:

    [![](implementing-sirikit-images/plist14w.png "Přidat pár klíčů řetězec se používá příklad podmínek.")](implementing-sirikit-images/plist14w.png#lightbox)
16. Zopakujte výše uvedené kroky pro vlastní podmínky aplikace muset definovat.
17. Sbalit `ParameterVocabularies` klíč.
18. Přidat `IntentPhrases` klíč s **typ** z `Array`:

    [![](implementing-sirikit-images/plist15w.png "Přidejte klíč IntentPhrases s poli typu")](implementing-sirikit-images/plist15w.png#lightbox)
19. Přidejte nový klíč s **typ** z `Dictionary`:

    [![](implementing-sirikit-images/plist16w.png "Přidejte nový klíč s slovníku typů.")](implementing-sirikit-images/plist16w.png#lightbox)
20. Přidat `IntentName` klíč s **typ** z `String` a záměrné pro tento příklad:

    [![](implementing-sirikit-images/plist17w.png "Přidejte klíč IntentName s řetězec typu a záměr pro tento příklad")](implementing-sirikit-images/plist17w.png#lightbox)
21. Přidat `IntentExamples` klíč s **typ** z `Array`:

    [![](implementing-sirikit-images/plist18w.png "Přidejte klíč IntentExamples s poli typu")](implementing-sirikit-images/plist18w.png#lightbox)
22. Přidat pár `String` klíče se používá příklad podmínek:

    [![](implementing-sirikit-images/plist19w.png "Přidat pár klíčů řetězec se používá příklad podmínek.")](implementing-sirikit-images/plist19w.png#lightbox)
23. Výše uvedené kroky opakujte pro všechny záměry aplikace je potřeba zadat příklad použití.

-----

> [!IMPORTANT]
> `AppIntentVocabulary.plist` Bude zaregistrována s Siri na testovací zařízení během vývoje a může trvat nějakou dobu Siri začlenit vlastní slovník. V důsledku toho tester muset Počkejte několik minut před pokusem o k testování aplikace konkrétní termínů, když se aktualizovala.

Další informace najdete v tématu naše [aplikace konkrétní termínů odkaz](~/ios/platform/sirikit/understanding-sirikit.md) a společnosti Apple [zadání odkazu termínů vlastní](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

## <a name="adding-an-intents-extension"></a>Přidávání tříd Intent rozšíření

Teď, když aplikace byl připraven přijmout SiriKit, vývojář bude nutné přidat jeden (nebo více) záměry rozšíření do řešení pro zpracování záměry požadované pro integraci Siri.

Pro každé rozšíření tříd Intent je nutné postupujte takto:

- Přidání rozšíření tříd Intent projektu k řešení aplikace Xamarin.iOS.
- Konfigurace rozšíření tříd Intent `Info.plist` souboru.
- Upravte hlavní třídy rozšíření tříd Intent.

Další informace najdete v tématu naše [odkazu rozšíření tříd Intent](~/ios/platform/sirikit/understanding-sirikit.md) a společnosti Apple [vytváření odkaz na rozšíření tříd Intent](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1).

### <a name="creating-the-extension"></a>Vytváření rozšíření

Chcete-li přidat rozšíření tříd Intent k řešení, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Klikněte pravým tlačítkem na **název řešení** v **řešení Pad** a vyberte **přidat** > **přidat nový projekt...** .
2. V dialogovém okně vyberte **iOS** > **rozšíření** > **záměr rozšíření** a klikněte na tlačítko **Další** tlačítko: 

    [![](implementing-sirikit-images/intents05.png "Vyberte záměrné rozšíření")](implementing-sirikit-images/intents05.png#lightbox)
3. Potom zadejte **název** záměr rozšíření a klikněte na tlačítko **Další** tlačítko: 

    [![](implementing-sirikit-images/intents06.png "Zadejte název pro záměrné rozšíření")](implementing-sirikit-images/intents06.png#lightbox)
4. Nakonec klikněte na **vytvořit** tlačítko Přidat rozšíření záměr řešení aplikace: 

    [![](implementing-sirikit-images/intents07.png "Přidejte rozšíření záměr do řešení aplikace")](implementing-sirikit-images/intents07.png#lightbox)
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **odkazy** složky nově vytvořený rozšíření záměr. Zkontrolujte název běžné projektu knihovny sdíleného kódu (který aplikaci vytvořili výše) a klikněte **OK** tlačítko: 

    [![](implementing-sirikit-images/intents08.png "Vyberte název běžné knihovny projektu sdíleného kódu")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Klikněte pravým tlačítkem na **název řešení** v **Průzkumníku řešení** a vyberte **přidat** > **přidat nový projekt...** .
2. V dialogovém okně vyberte **Visual C# > iOS rozšíření > rozšíření záměr** a klikněte na tlačítko **Další** tlačítko:

    [![](implementing-sirikit-images/intents05.w157-sml.png "Vyberte záměrné rozšíření")](implementing-sirikit-images/intents05.w157.png#lightbox)
3. Potom zadejte **název** záměr rozšíření a klikněte na tlačítko **OK** tlačítko.
1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **odkazy** záměry rozšíření nově vytvořené složky a vyberte **Přidat > odkaz**. Zkontrolujte název běžné projektu knihovny sdíleného kódu (který aplikaci vytvořili výše) a klikněte **OK** tlačítko:

    [![](implementing-sirikit-images/intents08w.png "Vyberte název běžné knihovny projektu sdíleného kódu")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

Opakujte tyto kroky pro počet záměr rozšíření (na základě [architektury aplikace pro rozšíření](#Architecting-the-App-for-Extensions) část výše) vyžadující aplikace.

### <a name="configuring-the-infoplist"></a>Konfigurace Info.plist

Pro každé rozšíření tříd Intent, který jste přidali do řešení aplikace, musí být konfigurované v `Info.plist` soubory pro práci s aplikací.

Stejně jako všechny typické rozšíření aplikace, aplikace bude mít existující klíče `NSExtension` a `NSExtensionAttributes`. Pro rozšíření tříd Intent existují dvě nové atributy, které musí být nakonfigurovaná:

[![](implementing-sirikit-images/intents01.png "Dva nové atributy, které musí být nakonfigurované")](implementing-sirikit-images/intents01.png#lightbox)

- **IntentsSupported** – požadované a skládá se z pole názvy záměr tříd, které aplikace chce podporovat z rozšíření záměr.
- **IntentsRestrictedWhileLocked** -je volitelné klíč pro aplikaci na konkrétní rozšíření zámek obrazovky chování. Skládá se z pole názvy záměr tříd, které aplikace chce vyžadovat, aby uživatel k přihlášení používat z rozšíření záměr.

Ke konfiguraci rozšíření záměr `Info.plist` souboru, klikněte dvakrát na jeho **Průzkumníku řešení** otevřete pro úpravy. Potom přepnout **zdroj** prohlédněte si potom rozbalte položku `NSExtension` a `NSExtensionAttributes` klíče v editoru:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents02.png "NSExtension a NSExtensionAttributes klíče v editoru")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents02w.png "NSExtension a NSExtensionAttributes klíče v editoru")](implementing-sirikit-images/intents02w.png#lightbox)

-----

Rozbalte `IntentsSupported` klíče a přidejte název třídy žádné záměr toto rozšíření bude podporovat. Pro aplikaci MonkeyChat příklad podporuje `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents09.png "Klíč INSendMessageIntent")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents09w.png "Klíč INSendMessageIntent")](implementing-sirikit-images/intents09w.png#lightbox)

-----

Pokud aplikace volitelně vyžaduje, aby uživatel přihlášen do zařízení pomocí daného záměr, rozbalte položku `IntentRestrictedWhileLocked` klíče a přidejte třídu názvy tříd Intent, které mají omezený přístup. Pro příklad MonkeyChat aplikaci, musíte být přihlášení uživatele k odeslání chatovací zprávy, takže jsme přidali `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents10.png "Přidaném klíči INSendMessageIntent")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents10w.png "Přidaném klíči INSendMessageIntent")](implementing-sirikit-images/intents10w.png#lightbox)

-----


Úplný seznam dostupných záměr domén, najdete v tématu společnosti Apple [záměr domén odkaz](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Konfigurace hlavní – třída

V dalším kroku vývojář muset nakonfigurovat hlavní třídy, který funguje jako hlavní vstupní bod pro rozšíření záměr do Siri. Musí být podtřídou třídy `INExtension` který odpovídá `IINIntentHandler` delegovat. Příklad:

```csharp
using System;
using System.Collections.Generic;

using Foundation;
using Intents;

namespace MonkeyChatIntents
{
    [Register ("IntentHandler")]
    public class IntentHandler : INExtension, IINSendMessageIntentHandling, IINSearchForMessagesIntentHandling, IINSetMessageAttributeIntentHandling
    {
        #region Constructors
        protected IntentHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override NSObject GetHandler (INIntent intent)
        {
            // This is the default implementation.  If you want different objects to handle different intents,
            // you can override this and return the handler you want for that particular intent.

            return this;
        }
        #endregion
        ...
    }
}
```

Neexistuje jeden solitary metoda, která aplikace musí implementovat na hlavní třídy rozšíření záměr `GetHandler` metoda. Tato metoda je předaná záměrem SiriKit a aplikace, musíte se vrátit **obslužná rutina záměr** odpovídající typ daného záměr.

Vzhledem k tomu, že aplikace MonkeyChat příklad zpracovává jenom jeden záměr, vrací sám v `GetHandler` metoda. Pokud rozšíření zpracovává více než jeden záměr, by vývojář přidejte třídu pro každý typ záměrné a vrátit instanci tady na základě `Intent` předaný metodě.

### <a name="handling-the-resolve-stage"></a>Zpracování fázi řešení

Fáze řešení je kde bude rozšíření záměr vysvětlení a ověřte parametry předané z Siri a nastavili prostřednictvím konverzace uživatele.

Pro každý parametr, který získá odesílá z Siri, je `Resolve` metoda. Aplikaci bude nutné k implementaci této metody pro všechny parametry, které aplikace potřebovat pomoc na Siri získat správnou odpověď od uživatele.

V případě aplikace MonkeyChat příklad rozšíření záměr bude vyžadovat nejméně jeden příjemce k odeslání zprávy do. Pro každého z příjemců v seznamu bude třeba provést hledání kontaktů, která může mít následující výsledek rozšíření:

- Přesně jeden odpovídající kontakt nachází.
- Dvě nebo více odpovídající kontakty nebyly nalezeny.
- Nebyly nalezeny žádné odpovídající kontakty.

Kromě toho MonkeyChat vyžaduje obsah pro tělo zprávy. Pokud uživatel nebyl zadaný to, je potřeba požádat uživatele o obsah Siri.

Rozšíření záměr potřebovat pro pohodlné zpracování jednotlivých případech.


```csharp
[Export ("resolveRecipientsForSearchForMessages:withCompletion:")]
public void ResolveRecipients (INSendMessageIntent intent, Action<INPersonResolutionResult []> completion)
{
    var recipients = intent.Recipients;
    // If no recipients were provided we'll need to prompt for a value.
    if (recipients.Length == 0) {
        completion (new INPersonResolutionResult [] { (INPersonResolutionResult)INPersonResolutionResult.NeedsValue });
        return;
    }

    var resolutionResults = new List<INPersonResolutionResult> ();

    foreach (var recipient in recipients) {
        var matchingContacts = new INPerson [] { recipient }; // Implement your contact matching logic here to create an array of matching contacts
        if (matchingContacts.Length > 1) {
            // We need Siri's help to ask user to pick one from the matches.
            resolutionResults.Add (INPersonResolutionResult.GetDisambiguation (matchingContacts));
        } else if (matchingContacts.Length == 1) {
            // We have exactly one matching contact
            resolutionResults.Add (INPersonResolutionResult.GetSuccess (recipient));
        } else {
            // We have no contacts matching the description provided
            resolutionResults.Add ((INPersonResolutionResult)INPersonResolutionResult.Unsupported);
        }
    }

    completion (resolutionResults.ToArray ());
}

[Export ("resolveContentForSendMessage:withCompletion:")]
public void ResolveContent (INSendMessageIntent intent, Action<INStringResolutionResult> completion)
{
    var text = intent.Content;
    if (!string.IsNullOrEmpty (text))
        completion (INStringResolutionResult.GetSuccess (text));
    else
        completion ((INStringResolutionResult)INStringResolutionResult.NeedsValue);
}
```

Další informace najdete v tématu naše [vyřešit odkazu fáze](~/ios/platform/sirikit/understanding-sirikit.md) a společnosti Apple [řešení a zpracování odkaz záměry](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1).

### <a name="handling-the-confirm-stage"></a>Zpracování fázi potvrzení

Fáze potvrďte je, kde zkontroluje záměr rozšíření, který obsahuje všechny informace ke splnění požadavku uživatele. Aplikace se chce send potvrzení společně se všechny podpůrné podrobnosti co je o dojít k Siri, na němž jej lze potvrdit s uživatelem že jde o zamýšlené akci.

```csharp
[Export ("confirmSendMessage:completion:")]
public void ConfirmSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Verify user is authenticated and the app is ready to send a message.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Ready, userActivity);
    completion (response);
}
```

Další informace najdete v tématu naše [potvrďte odkazu fáze](~/ios/platform/sirikit/understanding-sirikit.md).

### <a name="processing-the-intent"></a>Zpracování záměr

Toto je bod, kde rozšíření záměr ve skutečnosti provádí úlohy ke splnění požadavku uživatele a předejte výsledky zpět do Siri, takže uživatel může být informován.


```csharp
public void HandleSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Implement the application logic to send a message here.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, userActivity);
    completion (response);
}

public void HandleSearchForMessages (INSearchForMessagesIntent intent, Action<INSearchForMessagesIntentResponse> completion)
{
    // Implement the application logic to find a message that matches the information in the intent.
    ...

    var userActivity = new NSUserActivity (nameof (INSearchForMessagesIntent));
    var response = new INSearchForMessagesIntentResponse (INSearchForMessagesIntentResponseCode.Success, userActivity);

    // Initialize with found message's attributes
    var sender = new INPerson (new INPersonHandle ("sarah@example.com", INPersonHandleType.EmailAddress), null, "Sarah", null, null, null);
    var recipient = new INPerson (new INPersonHandle ("+1-415-555-5555", INPersonHandleType.PhoneNumber), null, "John", null, null, null);
    var message = new INMessage ("identifier", "I am so excited about SiriKit!", NSDate.Now, sender, new INPerson [] { recipient });
    response.Messages = new INMessage [] { message };
    completion (response);
}

public void HandleSetMessageAttribute (INSetMessageAttributeIntent intent, Action<INSetMessageAttributeIntentResponse> completion)
{
    // Implement the application logic to set the message attribute here.
    ...

    var userActivity = new NSUserActivity (nameof (INSetMessageAttributeIntent));
    var response = new INSetMessageAttributeIntentResponse (INSetMessageAttributeIntentResponseCode.Success, userActivity);
    completion (response);
}
```

Další informace najdete v tématu naše [zpracování odkazu fáze](~/ios/platform/sirikit/understanding-sirikit.md).

## <a name="adding-an-intents-ui-extension"></a>Přidání rozšíření tříd Intent uživatelského rozhraní

Volitelné rozšíření tříd Intent uživatelského rozhraní představuje možnost převést aplikace uživatelského rozhraní a značka do prostředí Siri a uživatelů působí připojený k aplikaci. S touto příponou aplikace můžete zahrnout značky, jakož i visual a dalších informací do zápis.

[![](implementing-sirikit-images/intentsui01.png "Příklad výstupu rozšíření tříd Intent uživatelského rozhraní")](implementing-sirikit-images/intentsui01.png#lightbox)

Stejně jako rozšíření tříd Intent bude vývojář proveďte následující krok pro rozšíření tříd Intent uživatelského rozhraní:

- Přidání projektu rozšíření tříd Intent uživatelského rozhraní k řešení aplikace Xamarin.iOS.
- Konfigurace uživatelského rozhraní rozšíření tříd Intent `Info.plist` souboru.
- Upravte hlavní třídy rozšíření tříd Intent uživatelského rozhraní.

Další informace najdete v tématu naše [záměry uživatelského rozhraní rozšíření odkazu](~/ios/platform/sirikit/understanding-sirikit.md) a společnosti Apple [poskytuje odkaz vlastní rozhraní](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1).

### <a name="creating-the-extension"></a>Vytváření rozšíření

Chcete-li přidat rozšíření tříd Intent uživatelského rozhraní k řešení, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Klikněte pravým tlačítkem na **název řešení** v **řešení Pad** a vyberte **přidat** > **přidat nový projekt...** .
2. V dialogovém okně vyberte **iOS** > **rozšíření** > **záměr uživatelského rozhraní rozšíření** a klikněte na tlačítko **Další** tlačítko: 

    [![](implementing-sirikit-images/intents11.png "Vyberte rozšíření záměrné uživatelského rozhraní")](implementing-sirikit-images/intents11.png#lightbox)
3. Potom zadejte **název** záměr rozšíření a klikněte na tlačítko **Další** tlačítko: 

    [![](implementing-sirikit-images/intents12.png "Zadejte název pro záměrné rozšíření")](implementing-sirikit-images/intents12.png#lightbox)
4. Nakonec klikněte na **vytvořit** tlačítko Přidat rozšíření záměr řešení aplikace: 

    [![](implementing-sirikit-images/intents13.png "Přidejte rozšíření záměr do řešení aplikace")](implementing-sirikit-images/intents13.png#lightbox)
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **odkazy** složky nově vytvořený rozšíření záměr. Zkontrolujte název běžné projektu knihovny sdíleného kódu (který aplikaci vytvořili výše) a klikněte **OK** tlačítko: 

    [![](implementing-sirikit-images/intents14.png "Vyberte název běžné knihovny projektu sdíleného kódu")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Klikněte pravým tlačítkem na **název řešení** v **Průzkumníku řešení** a vyberte **přidat** > **přidat nový projekt...**
2. V dialogovém okně vyberte **iOS** > **rozšíření** > **záměr uživatelského rozhraní rozšíření** a klikněte na tlačítko **Další** tlačítko.
3. Potom zadejte **název** záměr rozšíření a klikněte na tlačítko **OK** tlačítko.
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **odkazy** složky nově vytvořený rozšíření záměr. Zkontrolujte název běžné projektu knihovny sdíleného kódu (který aplikaci vytvořili výše) a klikněte **OK** tlačítko.
    
-----

### <a name="configuring-the-infoplist"></a>Konfigurace Info.plist

Konfigurace uživatelského rozhraní rozšíření tříd Intent `Info.plist` souboru fungovat s aplikací.

Stejně jako všechny typické rozšíření aplikace, aplikace bude mít existující klíče `NSExtension` a `NSExtensionAttributes`. Pro rozšíření tříd Intent je jeden nový atribut, který musí být nakonfigurovaná:

[![](implementing-sirikit-images/intents03.png "Jeden nový atribut, který musí být nakonfigurované")](implementing-sirikit-images/intents03.png#lightbox)

**IntentsSupported** požadované a skládá se z pole názvy záměr tříd, které chcete podporovat z rozšíření záměr aplikace.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Ke konfiguraci rozšíření uživatelského rozhraní záměr `Info.plist` souboru, klikněte dvakrát na jeho **Průzkumníku řešení** otevřete pro úpravy. Potom přepnout **zdroj** prohlédněte si potom rozbalte položku `NSExtension` a `NSExtensionAttributes` klíče v editoru:

[![](implementing-sirikit-images/intents04.png "NSExtension a NSExtensionAttributes klíče v editoru")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ke konfiguraci rozšíření uživatelského rozhraní záměr `Info.plist` souboru, klikněte dvakrát na jeho **Průzkumníku řešení** otevřete pro úpravy. Rozbalte `NSExtension` a `NSExtensionAttributes` klíče v editoru:

[![](implementing-sirikit-images/intents04w.png "Tnelze NSExtension a NSExtensionAttributes klíče v editoru")](implementing-sirikit-images/intents04w.png#lightbox)

-----

Rozbalte `IntentsSupported` klíče a přidejte název třídy žádné záměr toto rozšíření bude podporovat. Pro aplikaci MonkeyChat příklad podporuje `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents15.png "Klíč INSendMessageIntent")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents15w.png "Klíč INSendMessageIntent")](implementing-sirikit-images/intents15w.png#lightbox)

-----

Úplný seznam dostupných záměr domén, najdete v tématu společnosti Apple [záměr domén odkaz](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Konfigurace hlavní – třída

Nakonfigurujte hlavní třídy, který funguje jako hlavní vstupní bod pro rozšíření uživatelského rozhraní záměr do Siri. Musí být podtřídou třídy `UIViewController` který odpovídá `IINUIHostedViewController` rozhraní. Příklad:

```csharp
using System;
using Foundation;
using CoreGraphics;
using Intents;
using IntentsUI;
using UIKit;

namespace MonkeyChatIntentsUI
{
    public partial class IntentViewController : UIViewController, IINUIHostedViewControlling
    {
        #region Constructors
        protected IntentViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }

        public override void DidReceiveMemoryWarning ()
        {
            // Releases the view if it doesn't have a superview.
            base.DidReceiveMemoryWarning ();

            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Public Methods
        [Export ("configureWithInteraction:context:completion:")]
        public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
        {
            // Do configuration here, including preparing views and calculating a desired size for presentation.

            if (completion != null)
                completion (DesiredSize ());
        }

        [Export ("desiredSize:")]
        public CGSize DesiredSize ()
        {
            return ExtensionContext.GetHostedViewMaximumAllowedSize ();
        }
        #endregion
    }
}
```

Siri předá `INInteraction` instance třídy k `Configure` metodu `UIViewController` instance uvnitř rozšíření záměr uživatelského rozhraní.

`INInteraction` Objekt poskytuje tři důležité informace k rozšíření:

1. Záměrné objekt, který zpracovává.
2. Objekt odpovědi záměr z `Confirm` a `Handle` metody rozšíření záměr.
3. Interakce stav, který definuje stav interakci mezi aplikací a Siri.

`UIViewController` Instance je Princip třídou pro interakci s Siri a protože dědí z `UIViewController`, má přístup ke všem funkcím UIKit.

Při volání Siri `Configure` metodu `UIViewController` předává v kontextu zobrazení s oznámením, že řadiče zobrazení bude buď hostovány v Siri Snippit nebo karty mapy.

Siri, projdou také v dokončení obslužná rutina, která aplikaci zapotřebí vrátit požadovaná velikost zobrazení po dokončení činnosti aplikace její konfiguraci.

### <a name="design-the-ui-in-ios-designer"></a>Návrh uživatelského rozhraní v iOS návrháře

Rozložení uživatelského rozhraní rozšíření tříd Intent uživatelské rozhraní v iOS návrháře. Dvakrát klikněte na rozšíření `MainInterface.storyboard` v soubor **Průzkumníku řešení** otevřete pro úpravy. Přetáhněte všechny požadované prvky uživatelského rozhraní pro sestavení se uživatelské rozhraní a uložte změny.

> [!IMPORTANT]
> Když je možné přidat interaktivní prvky, jako `UIButtons` nebo `UITextFields` do uživatelského rozhraní rozšíření záměr `UIViewController`, jsou výhradně zakázané jako rozhraní záměr v neinteraktivním a uživatel nebude moci komunikovat s nimi.

### <a name="wire-up-the-user-interface"></a>Navázání uživatelského rozhraní

S uživatelským rozhraním uživatelského rozhraní rozšíření tříd Intent vytvořené v iOS Návrhář upravit `UIViewController` podtřídami a přepsání `Configure` metoda následujícím způsobem:

```csharp
[Export ("configureWithInteraction:context:completion:")]
public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
{
    // Do configuration here, including preparing views and calculating a desired size for presentation.
    ...

    // Return desired size
    if (completion != null)
        completion (DesiredSize ());
}

[Export ("desiredSize:")]
public CGSize DesiredSize ()
{
    return ExtensionContext.GetHostedViewMaximumAllowedSize ();
}
```

### <a name="overriding-the-default-siri-ui"></a>Přepsání rozhraní výchozí Siri

Rozšíření tříd Intent uživatelského rozhraní bude vždy zobrazí společně s další obsah Siri jako ikona aplikace a název v horní části uživatelského rozhraní nebo, podle úmyslu, tlačítka (například odeslání nebo zrušit), může se zobrazit v dolní části.

Existuje několik instancí, kde aplikace může nahradit informace, které je Siri zobrazení pro uživatele ve výchozím nastavení, jako je zasílání zpráv nebo mapy, kde aplikace může nahradit výchozí prostředí s jednou přizpůsobit aplikaci.

Pokud rozšíření tříd Intent uživatelského rozhraní musí přepsat prvky uživatelského rozhraní Siri, výchozí hodnota `UIViewController` podtřídami bude nutné implementovat `IINUIHostedViewSiriProviding` rozhraní a přihlásit k zobrazení elementu konkrétní rozhraní.

Přidejte následující kód, který `UIViewController` podtřídami Siri říct, že rozšíření uživatelského rozhraní záměr již zobrazuje obsah zprávy:

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>Důležité informace

Apple naznačuje, že vývojář trvat v úvahu při návrhu a implementace uživatelského rozhraní rozšíření záměr následující aspekty:

- **Být vědomá toho z využití paměti** – protože záměr uživatelského rozhraní rozšíření jsou dočasné a jen ukazuje po krátkou dobu, systém ukládá náročnější omezení paměti, než se používají s úplné aplikace.
- **Vezměte v úvahu minimální a maximální velikosti zobrazení** -zajistěte spokojeni rozšíření záměr uživatelského rozhraní na každý typ zařízení iOS, velikosti a orientace. Požadovaná velikost, která aplikace odešle zpět na Siri navíc nemusí být možné udělit oprávnění.
- **Flexibilní a adaptivní vzory rozložení** – znovu, přesvědčte se, že rozhraní zobrazena skvělé na každé zařízení.

## <a name="summary"></a>Souhrn

Tento článek má popsaná SiriKit a ukazuje, jak mohou být přidány do aplikace pro Xamarin.iOS k poskytování služeb, které jsou dostupné pro uživatele na zařízení s iOS pomocí Siri a mapy aplikace.




## <a name="related-links"></a>Související odkazy

- [Ukázka ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Průvodce programováním SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Záměry Framework – referenční informace](https://developer.apple.com/reference/intents)
- [Záměry uživatelského rozhraní Framework – referenční informace](https://developer.apple.com/reference/intentsui)
- [Referenční dokumentace záměrné domén](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
