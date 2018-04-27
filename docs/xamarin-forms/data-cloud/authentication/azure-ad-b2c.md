---
title: Ověřování uživatelů s Azure Active Directory B2C
description: Azure Active Directory B2C je cloudové řešení správy identity pro určených webové a mobilní aplikace. Tento článek ukazuje, jak integrovat správu identit uživatelů do mobilních aplikací pomocí Microsoft knihovny pro ověřování a Azure Active Directory B2C.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 627c6773c099c9cf45f871a9bb73a201bf98271a
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Ověřování uživatelů s Azure Active Directory B2C

_Azure Active Directory B2C je cloudové řešení správy identity pro určených webové a mobilní aplikace. Tento článek ukazuje, jak integrovat správu identit uživatelů do mobilních aplikací pomocí Microsoft knihovny pro ověřování a Azure Active Directory B2C._

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně předběžné verze")

> [!NOTE]
> [Knihovny pro ověřování Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client) je stále ve verzi preview, ale je vhodná pro použití v provozním prostředí. Ale existuje může být narušující změny na rozhraní API, formát vnitřní mezipaměti a další mechanismy knihovny, která může mít vliv na vaše aplikace.

## <a name="overview"></a>Přehled

Azure Active Directory B2C je služba správy identity pro určených aplikace, která umožňuje příjemci k přihlášení do aplikace pomocí:

- Pomocí svých účtů na sociálních (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Vytvoření nové přihlašovací údaje (e-mailovou adresu a heslo, nebo uživatelské jméno a heslo). Tyto přihlašovací údaje jsou označovány jako *místní* účty.

Proces pro integraci služby Azure Active Directory B2C identity management do mobilní aplikace je následující:

1. Vytvoření klienta Azure Active Directory B2C. Další informace najdete v tématu [vytvoření klienta Azure Active Directory B2C na portálu Azure](/azure/active-directory-b2c/active-directory-b2c-get-started/).
1. Registrace mobilní aplikace pomocí klienta Azure Active Directory B2C. Proces registrace přiřadí **ID aplikace** vaší aplikace, které jednoznačně identifikuje a **adresy URL pro přesměrování** který lze použít k cílení odpovědí zpět do vaší aplikace. Další informace najdete v tématu [Azure Active Directory B2C: Registrace vaší aplikace](/azure/active-directory-b2c/active-directory-b2c-app-registration/).
1. Vytvoření zásad registrace a přihlášení. Tato zásada definují prostředí, které budou spotřebitelé projít během registrace a přihlášení a také určuje obsah aplikace bude přijímat tokeny na úspěšné registrace nebo přihlášení. Další informace najdete v tématu [Azure Active Directory B2C: integrovaných zásad](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).
1. Použití [knihovny pro ověřování Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) v mobilní aplikaci s inicializujte ověřovací pracovní postup u klienta služby Azure Active Directory B2C.

> [!NOTE]
> A také integraci správy identit Azure Active Directory B2C do mobilní aplikace, MSAL lze také integrovat správu identit Azure Active Directory do mobilní aplikace. To lze provést po registraci mobilních aplikací v Azure Active Directory [portálu pro registraci aplikace](https://apps.dev.microsoft.com/). Proces registrace přiřadí **ID aplikace** který jednoznačně identifikuje vaši aplikaci, které musí být zadán při použití MSAL. Další informace najdete v tématu [postup registrace aplikace s koncovým bodem v2.0](/azure/active-directory/develop/active-directory-v2-app-registration/), a [ověřit vaše mobilní aplikace pomocí ověřování knihovna Microsoft](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) na blogu Xamarin.

MSAL webovému prohlížeči zařízení používá k provedení ověřování. To zvyšuje použitelnost aplikace, jako jsou uživatelé pouze musí přihlášení po každé zařízení, vylepšení převod míry přihlašování a autorizaci toků v aplikaci. Prohlížeč zařízení také poskytuje vyšší úroveň zabezpečení. Po dokončení procesu ověřování uživatele bude ovládací prvek vrátit se do aplikace z kartu webového prohlížeče. Toho dosáhnete tak, že zaregistrujete vlastní schéma adresy URL pro přesměrování URL, která se vrátí z procesu ověřování a pak zjišťování a zpracování vlastní adresu URL, jakmile se odesílají. Další informace o volbě vlastní schéma adresy URL, najdete v části [výběr na identifikátor URI přesměrování nativní aplikaci](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/).

> [!NOTE]
> Tento mechanismus pro registraci vlastní schéma adresy URL s operačním systémem a zpracování schéma je specifická pro každou platformu.

Každý požadavek zaslaný do klienta Azure Active Directory B2C Určuje *zásad*. Zásady popisují činnosti identity uživatelů, jako je registrace nebo přihlášení. Například registrační zásadě umožňuje chování klienta Azure Active Directory B2C nakonfigurovat pomocí následující nastavení:

- Typy účtů, které příjemce může použít k přihlášení do aplikace.
- Data, které se mají shromažďovat od příjemce během registrace.
- Služba Multi-Factor authentication.
- Obsah na stránku pro přihlášení.
- Token deklarací identity, které mobilní aplikace obdrží při provedlo zásady.

Klient služby Azure Active Directory může obsahovat víc zásad různých typů, které se dají použít v aplikaci podle potřeby. Zásady můžete navíc opětovně použít napříč aplikací, což umožňuje definování a úprava činnosti identity příjemce beze změny kódu. Další informace o zásadách najdete v tématu [Azure Active Directory B2C: integrovaných zásad](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

## <a name="setup"></a>Instalace

Knihovna NuGet Microsoft ověřování knihovny (MSAL) musí být přidaný do projektu přenosných třída knihovny PCL () a platforma projekty v řešení Xamarin.Forms. Následující části obsahují další instalační pokyny pro použití MSAL pro komunikaci se klienta služby Azure Active Directory B2C z mobilní aplikace.

### <a name="portable-class-library"></a>Přenosná knihovna tříd

PCLs, které využívají MSAL bude nutné změnit cíl necílené používat Profile7. Další informace o PCLs najdete v tématu [Úvod do knihovny přenosných tříd](~/cross-platform/app-fundamentals/pcl.md).

### <a name="ios"></a>iOS

V systému iOS, musí být zaregistrovaný vlastní schéma adresy URL, která se zaregistrovala ve službě Azure Active Directory B2C ve **Info.plist**, jak je znázorněno na následujícím snímku obrazovky:

![](azure-ad-b2c-images/customurl-ios.png "Registrace vlastní schéma adresy URL v systému iOS")

Po dokončení požadavek ověřování Azure Active Directory B2C je přesměrován na adresu URL registrované přesměrování. Protože adresu URL používá vlastní schéma je výsledkem iOS mobilní aplikaci spustit, předávání v adrese URL jako parametr spuštění, kde je zpracován programovacím modelem `OpenUrl` přepsat aplikace `AppDelegate` třídy, která je znázorněno v následujícím kódu Příklad:

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

Kód `OpenURL` metoda zajišťuje, že ovládací prvek vrátí do MSAL po části interaktivní ověřování pracovního postupu byla ukončena.

### <a name="android"></a>Android

V systému Android se musí být zaregistrovaný vlastní schéma adresy URL, která se zaregistrovala ve službě Azure Active Directory B2C ve **AndroidManifest.xml**, tak, že přidáte `<activity>` element uvnitř existující `<application>` elementu. `<activity>` Určuje element `IntentFilter` na `Activity` který zpracovává schéma a je znázorněno v následujícím příkladu:

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

Po dokončení požadavek ověřování Azure Active Directory B2C je přesměrován na adresu URL registrované přesměrování. Protože adresu URL používá vlastní schéma je výsledkem Android mobilní aplikaci spustit, předávání v adrese URL jako parametr spuštění, kde je zpracován programovacím modelem `microsoft.identity.client.BrowserTabActivity`. Všimněte si, že `data android:scheme` musí být nastavena na vlastní schéma adresy URL, která je zaregistrovaná u aplikace Azure Active Directory B2C.

Kromě toho `MainActivity` třída je třeba upravit, jak je znázorněno v následujícím příkladu kódu:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
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

`OnCreate` Metoda je upraveném přiřazení `UIParent` instance k `App.UiParent` vlastnost. Tím se zajistí, že tok ověření dojde v kontextu aktuální aktivity.

Kód `OnActivityResult` metoda zajišťuje, že ovládací prvek vrátí do MSAL po části interaktivní ověřování pracovního postupu byla ukončena.

### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

Na univerzální platformy Windows je potřeba žádné další nastavení použít MSAL.

## <a name="initialization"></a>Inicializace

Knihovna ověřování Microsoft používá členy `PublicClientApplication` třída zahájíte ověřovací pracovní postup. Ukázkovou aplikaci deklaruje a inicializuje `public` vlastnosti tohoto typu, s názvem `ADB2CClient`v `AuthenticationProvider` třídy. Následující příklad kódu ukazuje, jak je tato vlastnost inicializovat:

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

Pokud mobilní aplikace se zaregistrovala ve službě klienta Azure Active Directory B2C, proces registrace přiřazené **ID aplikace**. Toto ID se musí určit v `PublicClientApplication` konstruktoru, společně s `Authority` konstanta, která tvoří základní adresu URL a Azure Active Directory B2C zásadu spouštění.

## <a name="signing-in"></a>Přihlášení

Obrazovka přihlášení v ukázkové aplikaci se zobrazí na následujících snímcích obrazovky:

![](azure-ad-b2c-images/login.png "Přihlašovací stránky")

Přihlášení pomocí poskytovatelů identit sociálních nebo místní účet, jsou povoleny. Při Microsoft, Google, Facebook, jako v příkladu nahoře, používají a jako poskytovatelé sociálních identity, můžete použít také jiných poskytovatelů identit.

Následující příklad kódu ukazuje, jak je vyvolána proces přihlášení:

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

`AcquireTokenAsync` Metoda spustí webový prohlížeč zařízení a zobrazí možnosti ověřování definované v zásadách Azure Active Directory B2C, který je určen zásadami odkazuje prostřednictvím `Constants.Authority` konstantní. Tato zásada definuje prostředí, které budou spotřebitelé projít během registrace a přihlášení a deklarací identity, že aplikace bude zasláno úspěšné registrace nebo přihlášení.

Výsledkem `AcquireTokenAsync` volání metody, které je `AuthenticationResult` instance. Pokud je ověření úspěšné, `AuthenticationResult` instance bude obsahovat identity token, který bude místně do mezipaměti. Pokud je ověření úspěšné, `AuthenticationResult` instance budou obsahovat data, která označuje, proč ověřování se nezdařilo.

V ukázkové aplikace, pokud je ověření úspěšné `TodoList` stránky přešli.

## <a name="silent-re-authentication"></a>Tichou opětovné ověření

Když `LoginPage` ve vzorku, aplikace se zobrazí, je proveden pokus o načtení tokenu uživatele bez zobrazení uživatelského rozhraní ověřování. Toho dosáhnete pomocí `AcquireTokenSilentAsync` metoda, jak je ukázáno v následujícím příkladu kódu:

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

`AcquireTokenSilentAsync` Metoda pokusí se načíst token uživatele z mezipaměti, aniž by uživatel přihlásit. To zpracovává scénář, kde mohou již být přítomné v mezipaměti z předchozí relace vhodný token. Pokud je úspěšné, pokus o získání tokenu `TodoList` stránky přešli. Pokud pokus o získání tokenu neproběhne úspěšně, nedojde k žádné akci a uživatel bude mít možnost spustit nový pracovní postup ověřování.

## <a name="signing-out"></a>Odhlásit

Následující příklad kódu ukazuje, jak je vyvolána proces přihlášení:

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

Zruší všechny tokeny ověřování z místní mezipaměti.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak integrovat správu identit uživatelů do mobilních aplikací pomocí Microsoft ověřování knihovny (MSAL) a Azure Active Directory B2C. Azure Active Directory B2C je cloudové řešení správy identity pro určených webové a mobilní aplikace.


## <a name="related-links"></a>Související odkazy

- [AzureADB2CAuth (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Knihovna Microsoft ověřování](https://www.nuget.org/packages/Microsoft.Identity.Client)
