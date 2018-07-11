---
title: Ověřování uživatelů pomocí Azure Active Directory B2C
description: Azure Active Directory B2C je cloudové řešení správy identit pro zákaznické webové a mobilní aplikace. Tento článek ukazuje, jak integrovat správu identit uživatelů do mobilní aplikace pomocí knihovny Microsoft Authentication Library a Azure Active Directory B2C.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: b6989782c438ec41911cc9317d9f911d6518132d
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38872705"
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Ověřování uživatelů pomocí Azure Active Directory B2C

_Azure Active Directory B2C je cloudové řešení správy identit pro zákaznické webové a mobilní aplikace. Tento článek ukazuje, jak integrovat správu identit uživatelů do mobilní aplikace pomocí knihovny Microsoft Authentication Library a Azure Active Directory B2C._

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně předběžné verze")

> [!NOTE]
> [Knihovna Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) je stále ve verzi preview, ale je vhodný pro použití v produkčním prostředí. Ale existuje může být rozbíjející změny v rozhraní API, formát vnitřní mezipaměti a další mechanismy knihovny, který může mít vliv na vaše aplikace.

## <a name="overview"></a>Přehled

Azure Active Directory B2C je služba pro správu identit pro zákaznické aplikace, která umožňuje uživatelům přihlásit se do vaší aplikace pomocí:

- Pomocí jejich účtů na sociálních sítích (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Nově vytvořených přihlašovacích údajů (e-mailovou adresu a heslo, nebo uživatelské jméno a heslo). Tyto přihlašovací údaje se označují jako *místní* účty.

Postup pro integraci služby správy identit Azure Active Directory B2C do mobilní aplikace je následujícím způsobem:

1. Vytvoření tenanta Azure Active Directory B2C. Další informace najdete v tématu [vytvoření tenanta Azure Active Directory B2C na webu Azure Portal](/azure/active-directory-b2c/active-directory-b2c-get-started/).
1. Registrace mobilní aplikace pomocí tenanta Azure Active Directory B2C. Proces registrace přiřadí **ID aplikace** , který jednoznačně identifikuje vaši aplikaci a **adresy URL pro přesměrování** , který lze použít k cílení odpovědí zpět do vaší aplikace. Další informace najdete v tématu [Azure Active Directory B2C: Registrace vaší aplikace](/azure/active-directory-b2c/active-directory-b2c-app-registration/).
1. Vytvořte zásadu registrace a přihlášení. Tyto zásady se definují prostředí, kterými uživatelé budou procházet při registraci a přihlašování a také určuje obsah značek tokenů, které bude aplikace přijímat po úspěšné registraci nebo přihlášení. Další informace najdete v tématu [Azure Active Directory B2C: integrované zásady](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).
1. Použití [knihovna Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) v mobilní aplikaci pro ověřovací pracovní postup zahájení v tenantu Azure Active Directory B2C.

> [!NOTE]
> A integrace správy identit Azure Active Directory B2C v mobilních aplikacích, MSAL lze také integrovat správu identit Azure Active Directory do mobilní aplikace. Toho můžete docílit tak, že zaregistrujete mobilní aplikace s Azure Active Directory na [portál pro registraci aplikací](https://apps.dev.microsoft.com/). Proces registrace přiřadí **ID aplikace** , který jednoznačně identifikuje aplikaci, která se musí nastavit při použití MSAL. Další informace najdete v tématu [postup registrace aplikace s koncovým bodem v2.0](/azure/active-directory/develop/active-directory-v2-app-registration/), a [ověřit vaše mobilní aplikace pomocí knihovna Microsoft Authentication Library](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) na blogu Xamarinu.

Knihovna MSAL používá webový prohlížeč zařízení provádět ověření. Tím se zvýší užitná hodnota aplikace, jak uživatelé potřebují jenom přihlásit po každé zařízení, zvýšení míry úspěšnosti přihlašování a autorizaci toky v aplikaci. Prohlížeči v zařízení také poskytuje vyšší úroveň zabezpečení. Po dokončení procesu ověřování uživatele ovládací prvek vrátí k aplikaci z kartu webového prohlížeče. Toho dosáhnete, když si zaregistrujete vlastní schéma adresy URL pro přesměrování adresu URL, která je vrácena z procesu ověřování a potom zjišťování a zpracování vlastní adresu URL, jakmile je odeslána. Další informace o volbě vlastní schéma adresy URL, najdete v části [výběr na identifikátor URI přesměrování nativní aplikace](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/).

> [!NOTE]
> Mechanismus pro registraci vlastní schéma adresy URL s operačním systémem a zpracování schéma je specifické pro jednotlivé platformy.

Určuje každý požadavek zaslaný do tenanta služby Azure Active Directory B2C *zásady*. Zásady popisují prostředí pro spotřebitele identity jako je registrace nebo přihlášení. Například zásadu registrace umožňuje chování klienta Azure Active Directory B2C nakonfigurovat následující nastavení:

- Typy účtů, které uživatele můžete použít k přihlášení do aplikace.
- Shromažďování dat od uživatele během registrace.
- Ověřování službou Multi-Factor Authentication.
- Registrační stránku obsahu.
- Token deklarací identity, které mobilní aplikace obdrží, když zásady byl proveden.

Klient služby Azure Active Directory může obsahovat několik zásad různých typů, které lze použít v aplikaci podle potřeby. Kromě toho zásady můžete použít opakovaně napříč aplikacemi, abyste mohli definovat a upravovat činnosti identity příjemce beze změny kódu. Další informace o zásadách najdete v tématu [Azure Active Directory B2C: integrované zásady](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

## <a name="setup"></a>Instalace

Knihovna Microsoft Authentication Library (MSAL) NuGet musí být přidaný do projektu Přenosná knihovna tříd (PCL) a platforma projekty v řešení Xamarin.Forms. Následující části obsahují pokyny ke komunikaci s tenantem Azure Active Directory B2C z mobilních aplikací s využitím MSAL, další nastavení.

### <a name="portable-class-library"></a>Přenosná knihovna tříd

PCLs, které využívají MSAL muset změnilo Profile7 používat. Další informace o PCLs najdete v tématu [Úvod do knihovny přenosných tříd](~/cross-platform/app-fundamentals/pcl.md).

### <a name="ios"></a>iOS

V systémech iOS, musí být zaregistrovaný vlastní schéma adresy URL, který byl zaregistrován ve službě Azure Active Directory B2C v **Info.plist**, jak je znázorněno na následujícím snímku obrazovky:

![](azure-ad-b2c-images/customurl-ios.png "Registrace vlastní schéma adresy URL v Iosu")

Po dokončení požadavek ověřování Azure Active Directory B2C přesměruje na adresu URL pro přesměrování registrovaný. Vzhledem k tomu, že adresa URL používá vlastní schéma je výsledkem spuštění mobilní aplikace iOS, předá v adrese URL jako parametr spuštění, kde je zpracován programovacím modelem `OpenUrl` přepsání aplikace `AppDelegate` třída, která je znázorněna v následujícím kódu Příklad:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        ...
        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
            return true;
        }
    }
}
```

Kód v `OpenURL` metoda zajišťuje, že se ovládací prvek vrátí MSAL po interaktivní spotřebovanou část ověřovací pracovní postup byl ukončen.

### <a name="android"></a>Android

V Androidu, musí být zaregistrovaný vlastní schéma adresy URL, který byl zaregistrován ve službě Azure Active Directory B2C v **AndroidManifest.xml**, tak, že přidáte `<activity>` element v rámci existující `<application>` elementu. `<activity>` Určuje element `IntentFilter` na `Activity` , která zpracovává schéma a je znázorněno v následujícím příkladu:

```xml
<application ...>
  <activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
      <action android:name="android.intent.action.VIEW" />
      <category android:name="android.intent.category.DEFAULT" />
      <category android:name="android.intent.category.BROWSABLE" />
      <data android:scheme="INSERT_URL_SCHEME_HERE" android:host="auth" />
    </intent-filter>
  </activity>
</application>
```

Po dokončení požadavek ověřování Azure Active Directory B2C přesměruje na adresu URL pro přesměrování registrovaný. Vzhledem k tomu, že adresa URL používá vlastní schéma je výsledkem spuštění mobilní aplikace Android, předá v adrese URL jako parametr spuštění, kde je zpracován programovacím modelem `microsoft.identity.client.BrowserTabActivity`. Všimněte si, `data android:scheme` vlastnost musí být nastavena na vlastní schéma adresy URL, který je registrovaný pomocí aplikace Azure Active Directory B2C.

Kromě toho `MainActivity` třídy musí být upravena, jak je znázorněno v následujícím příkladu kódu:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            global::Xamarin.Forms.Forms.Init(this, bundle);
            Microsoft.WindowsAzure.MobileServices.CurrentPlatform.Init();
            LoadApplication(new App());
            App.UiParent = new UIParent(this);
        }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
        }
    }
}

```

`OnCreate` Metoda je upraven na přiřazení `UIParent` instance na `App.UiParent` vlastnost. Tím se zajistí, že tok ověřování dochází v rámci aktuální aktivity.

Kód v `OnActivityResult` metoda zajišťuje, že se ovládací prvek vrátí MSAL po interaktivní spotřebovanou část ověřovací pracovní postup byl ukončen.

### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

Na univerzální platformu Windows je potřeba žádné další nastavení použití MSAL.

## <a name="initialization"></a>Inicializace

Knihovna Microsoft Authentication Library používá členy `PublicClientApplication` třídy zahájit ověřovací pracovní postup. Ukázková aplikace deklaruje a inicializuje `public` vlastnost tohoto typu s názvem `ADB2CClient`v `AuthenticationProvider` třídy. Následující příklad kódu ukazuje, jak inicializovat tuto vlastnost:

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

Pokud mobilní aplikace byla zaregistrovaný s tenantem Azure Active Directory B2C, proces registrace přiřazená **ID aplikace**. Toto ID musí být zadán v `PublicClientApplication` konstruktoru, společně s `Authority` konstantu, která zahrnuje základní adresu URL a zásad Azure Active Directory B2C má být proveden.

## <a name="signing-in"></a>Přihlášení

Na následujících snímcích obrazovky se zobrazí přihlašovací obrazovka v ukázkové aplikaci:

![](azure-ad-b2c-images/login.png "Přihlašovací stránka")

Přihlášení pomocí zprostředkovatele sociální identity nebo místní účet, jsou povolené. I Microsoft, Google a Facebook, jak je uvedeno výše, se používají jako zprostředkovatele sociální identity, můžete použít také jinými poskytovateli identity.

Následující příklad kódu ukazuje, jak je vyvolán proces přihlašování:

```csharp
using Microsoft.Identity.Client;

public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);
    ...
}

```

`AcquireTokenAsync` Metoda spustí webový prohlížeč zařízení a zobrazí možnosti ověřování, které jsou definovány v zásadách Azure Active Directory B2C, která je zadána zásad odkazuje prostřednictvím `Constants.Authority` konstantní. Tato zásada definuje prostředí, kterými uživatelé budou procházet při registraci a přihlášení a deklarací identity, že se že aplikace přijímat po úspěšné registraci nebo přihlášení.

Výsledek `AcquireTokenAsync` je volání metody `AuthenticationResult` instance. Pokud je ověření úspěšné, `AuthenticationResult` instance bude obsahovat token identity, který bude do mezipaměti místně. Pokud ověřování neproběhne úspěšně, `AuthenticationResult` instance bude obsahovat data, která označuje, proč ověřování se nezdařilo.

V ukázkové aplikaci, pokud je ověření úspěšné `TodoList` stránky přejde.

## <a name="silent-re-authentication"></a>Tiché opakované ověření

Když `LoginPage` v ukázce aplikace se zobrazí, je proveden pokus o načtení tokenu uživatele bez zobrazení uživatelského rozhraní ověřování. Tím se dosahuje prostřednictvím `AcquireTokenSilentAsync` způsob, jak je ukázáno v následujícím příkladu kódu:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult;

    if (useSilent)
    {
        authenticationResult = await ADB2CClient.AcquireTokenSilentAsync(
            Constants.Scopes,
            GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
            Constants.Authority,
            false);
    }
    ...
}
```

`AcquireTokenSilentAsync` Metoda se pokusí načíst token uživatele z mezipaměti, aniž by uživatel přihlásit. To řeší scénář, kde vhodný token může být již přítomny v mezipaměti z předchozí relace. Pokud je úspěšná, pokus o získání tokenu `TodoList` stránky přejde. Pokud se pokus o získání tokenu neproběhne úspěšně, nic se nestane, a uživatel bude mít možnost iniciovat nový pracovní postup ověřování.

## <a name="signing-out"></a>Odhlášení

Následující příklad kódu ukazuje, jak je vyvolán proces přihlášení:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

Zruší výběr všech ověřovacích tokenů z místní mezipaměti.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak integrovat správu identit uživatelů do mobilních aplikací pomocí Microsoft Authentication Library (MSAL) a Azure Active Directory B2C. Azure Active Directory B2C je cloudové řešení správy identit pro zákaznické webové a mobilní aplikace.


## <a name="related-links"></a>Související odkazy

- [AzureADB2CAuth (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Knihovna Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client)
