---
title: Ověřování uživatelů pomocí zprostředkovatele Identity
description: Xamarin.Auth je SDK a platformy pro ověřování uživatelů a ukládání svých účtů. Zahrnuje ověřovací data OAuth, které poskytují podporu pro použití poskytovatelů identit, jako je například Google, Microsoft, Facebook či Twitter. Tento článek vysvětluje, jak používat ke správě procesu ověřování v aplikaci Xamarin.Forms Xamarin.Auth.
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: 26e85a37cfd36b5d4f045273548efafccca79e1a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="authenticating-users-with-an-identity-provider"></a>Ověřování uživatelů pomocí zprostředkovatele Identity

_Xamarin.Auth je SDK a platformy pro ověřování uživatelů a ukládání svých účtů. Zahrnuje ověřovací data OAuth, které poskytují podporu pro použití poskytovatelů identit, jako je například Google, Microsoft, Facebook či Twitter. Tento článek vysvětluje, jak používat ke správě procesu ověřování v aplikaci Xamarin.Forms Xamarin.Auth._

OAuth je otevřený standard pro ověřování a umožňuje vlastník prostředku oznámení zprostředkovatele prostředků, musí udělit oprávnění třetích stran pro přístup k jejich informace bez sdílení identity vlastníky prostředků. Příkladem by povolení uživatelům oznámit zprostředkovatele identity (například Google, Microsoft, Facebook nebo Twitter), musí udělit oprávnění k aplikaci přistupovat ke svým datům bez sdílení identitu uživatele. Běžně se používá jako přístup pro uživatele k přihlášení k webům a aplikacím pomocí zprostředkovatele identity, ale bez vystavení své heslo k webu nebo aplikaci.

Přehled tok ověřování při využívání poskytovatel identit OAuth vypadá takto:

1. Aplikace přejde prohlížeč na adresu URL zprostředkovatele identity.
1. Poskytovatel identity zpracuje ověření uživatele a vrátí autorizační kód do aplikace.
1. Aplikace výměny autorizační kód pro přístupový token od zprostředkovatele identity.
1. Aplikace používá přístupový token pro přístup k rozhraní API na poskytovatele identit, jako je například rozhraní API pro požadování základní uživatelská data.

Ukázkovou aplikaci ukazuje, jak použít Xamarin.Auth k implementaci tok nativní ověřování proti Google. Zatímco Google slouží jako zprostředkovatel identity v tomto tématu, je rovněž na jiných poskytovatelů identit přístupu. Další informace o ověřování pomocí služby Google OAuth 2.0 endpoint najdete v tématu [pomocí OAuth2.0 přístup k rozhraní API Google](https://developers.google.com/identity/protocols/OAuth2) na webu Google.

> [!NOTE]
> Zabezpečení přenosu aplikace (ATS) vynucuje v iOS 9 a vyšší, zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikace, a tím prevence náhodného zpřístupnění citlivých informací. Protože ATS je povolené ve výchozím nastavení v aplikacích pro iOS 9, všechna připojení budou platit ATS požadavky na zabezpečení. Pokud připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.
> ATS můžete vyjádřit výslovný souhlas mimo Pokud není možné použít `HTTPS` protokolu a zabezpečení komunikace pro prostředků z Internetu. Toho lze dosáhnout aktualizací aplikace **Info.plist** souboru. Další informace najdete v části [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md).

## <a name="using-xamarinauth-to-authenticate-users"></a>Pomocí Xamarin.Auth k ověřování uživatelů

Xamarin.Auth podporuje dva přístupy k interakci s koncovým bodem autorizace zprostředkovatele identity aplikací:

1. Pomocí embedded webové zobrazení. Pokud to bylo běžnou praxí, se doporučuje už z následujících důvodů:

    - Aplikace, který je hostitelem webového zobrazení mohou přistupovat k přihlašovací údaje úplného ověření uživatele, ne jenom autorizace udělení OAuth, který byl určen pro aplikaci. To je v rozporu Princip nejnižších nutných oprávnění, jako má aplikace přístup na výkonnější pověření, než se požaduje, potenciálně zvýšit prostor pro útoky aplikace.
    - Hostitelskou aplikaci může zaznamenat uživatelská jména a hesla, automaticky odesílání formulářů a nepoužívat souhlas uživatele a zkopírujte soubory cookie relace a použít je k provádění akcí ověřený jako uživatel.
    - Vložené webové zobrazení Nesdílejte stavu ověření s jinými aplikacemi nebo webový prohlížeč zařízení, vyžadování uživateli přihlásit se pro každý požadavek autorizace, která je považována za hodnotu menší než uživatelské prostředí.
    - Některé koncové body autorizace provést kroky ke zjištění a blokování autorizace požadavků, které pocházejí z webové zobrazení.

1. Pomocí webového prohlížeče v zařízení, což je doporučený postup. Pomocí prohlížeče zařízení pro požadavky OAuth zlepšuje použitelnost aplikaci, jak uživatelé potřebovat pouze k přihlášení k poskytovateli identity jednou za zařízení, vylepšení převod sazby toků přihlaste a autorizace v aplikaci. Prohlížeč zařízení také poskytuje lepší zabezpečení, jako jsou aplikace se můžou kontrolovat a upravovat obsah ve webovém zobrazení, ale ne obsah zobrazí v prohlížeči. Toto je přístup provedených v této článek a ukázkové aplikace.

Souhrnné informace o tom, jak ukázková aplikace používá k ověření uživatelů a jejich základní data načíst Xamarin.Auth je znázorněno v následujícím diagramu:

![](oauth-images/google-auth.png "Pomocí Xamarin.Auth k ověřování pro Google")

Požadavek na ověření pomocí služby Google díky aplikaci `OAuth2Authenticator` třídy. Vrátí odpověď ověřování, jakmile se uživatel byl úspěšně ověřen službou Google prostřednictvím jejich přihlašovací stránky, který obsahuje přístupový token. Aplikace se pak odešle požadavek Google pro základní uživatel data, pomocí `OAuth2Request` třídy s tímto tokenem přístupu nebudou zahrnuty v požadavku.

### <a name="setup"></a>Instalace

K integraci Google přihlásit se k aplikaci Xamarin.Forms musí být vytvořený projekt konzoly rozhraní Google API. To lze provést následujícím způsobem:

1. Přejděte na [konzoly rozhraní Google API](http://console.developers.google.com) webu a přihlaste se pomocí přihlašovacích údajů k účtu Google.
1. Z projektu rozevíracího seznamu vyberte existující projekt nebo vytvořte novou.
1. V části "Rozhraní API Správce" na bočním panelu, vyberte **pověření**, vyberte **OAuth souhlasu obrazovky karty**. Zvolte **e-mailová adresa**, zadejte **název produktu, který se uživatelům zobrazí**a stiskněte klávesu **Uložit**.
1. V **přihlašovací údaje** vyberte **vytvořit přihlašovací údaje** rozevírací seznam a vyberte **ID klienta OAuth**.
1. V části **typ aplikace**, vyberte platformy, na které bude mobilní aplikace spuštěna v (**iOS** nebo **Android**).
1. Zadejte požadované podrobnosti a vyberte **vytvořit** tlačítko.

> [!NOTE]
> ID klienta umožňuje aplikaci přístup k rozhraní API povoleno Google a mobilních aplikací jsou jedinečné pro jednu platformu. Proto **ID klienta OAuth** by měl být vytvořen pro každou platformu, který bude používat přihlášení Google.

Po provedení těchto kroků, Xamarin.Auth slouží k zahájení tok ověřování OAuth2 službou Google.

### <a name="creating-and-configuring-an-authenticator"></a>Vytváření a konfiguraci ověřovací data

Na Xamarin.Auth `OAuth2Authenticator` třída je zodpovědná za zpracování tok ověřování OAuth. Následující příklad kódu ukazuje instance `OAuth2Authenticator` třídy při ověřování pomocí webovému prohlížeči zařízení:

```csharp
var authenticator = new OAuth2Authenticator(
    clientId,
    null,
    Constants.Scope,
    new Uri(Constants.AuthorizeUrl),
    new Uri(redirectUri),
    new Uri(Constants.AccessTokenUrl),
    null,
    true);
```

`OAuth2Authenticator` Třída vyžaduje počet parametrů, které jsou následující:

- **ID klienta** – ten identifikuje klienta, která odeslala žádost a mohou být načteny z projektu v [konzoly rozhraní Google API](http://console.developers.google.com).
- **Tajný klíč klienta** – to by mělo být `null` nebo `string.Empty`.
- **Obor** – toto rozhraní API přístup požaduje aplikaci identifikuje a hodnota informuje obrazovce souhlasu, které se zobrazí uživateli. Další informace o oborech najdete v tématu [požadavku autorizace API](https://developers.google.com/+/web/api/rest/oauth) na webu Google.
- **Adresa URL pro autorizaci** – ten identifikuje adresu URL, kde se budou získávat autorizační kód z.
- **Přesměrování URL adresy** – ten identifikuje adresu URL, kde budou odeslány do odpovědi. Hodnota tohoto parametru musí shodovat s jedním z hodnoty, které se zobrazí v **pověření** kartu pro projekt v [konzole pro vývojáře Google](https://console.developers.google.com/).
- **Adresa AccessToken Url** – ten identifikuje adresu URL, které slouží k vyžádání přístupové tokeny po získání autorizační kód.
- **GetUserNameAsync Func** – na volitelné `Func` který se použije asynchronně načíst uživatelské jméno účtu po je byl úspěšně ověřen.
- **Použít nativní uživatelského rozhraní** – `boolean` hodnotu, která určuje, jestli se má používat webový prohlížeč v zařízení provést žádost o ověření.

### <a name="setup-authentication-event-handlers"></a>Nastavení ověřování obslužné rutiny událostí

Před nabízí uživatelské rozhraní, obslužné rutiny události pro `OAuth2Authenticator.Completed` události musí být zaregistrovaný, jak je znázorněno v následujícím příkladu kódu:

```csharp
authenticator.Completed += OnAuthCompleted;
```

Tato událost se aktivují, když uživatel úspěšně ověří nebo zruší přihlášení.

Volitelně obslužnou rutinu události pro `OAuth2Authenticator.Error` událost také lze zaregistrovat.

### <a name="presenting-the-sign-in-user-interface"></a>Prezentace přihlášení uživatelského rozhraní

Přihlašovací uživatelské rozhraní můžete zobrazovat uživateli pomocí přednášejícího Xamarin.Auth přihlášení, které musí být inicializován v každém projektu platformy. Následující příklad kódu ukazuje, jak k chybě při inicializaci přednášejícího přihlášení v `AppDelegate` třídy v projektu pro iOS:

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

Následující příklad kódu ukazuje, jak k chybě při inicializaci přednášejícího přihlášení v `MainActivity` třídy v projektu pro Android:

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

Potom přednášejícího přihlášení projektu přenosných třída knihovny PCL () může následujícím způsobem:

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

Všimněte si, že argument `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` metoda je `OAuth2Authenticator` instance. Když `Login` metoda je volána, zobrazí přihlašovací uživatelské rozhraní pro uživatele na kartě z webového prohlížeče v zařízení, která se zobrazí na následujících snímcích obrazovky:

![](oauth-images/login.png "Přihlášení Google")

### <a name="processing-the-redirect-url"></a>Zpracování adresy URL pro přesměrování

Po dokončení procesu ověřování uživatele bude ovládací prvek vrátit se do aplikace z kartu webového prohlížeče. Toho dosáhnete tak, že zaregistrujete vlastní schéma adresy URL pro přesměrování URL, která se vrátí z procesu ověřování a pak zjišťování a zpracování vlastní adresu URL, jakmile se odesílají.

Při výběru vlastní schéma adresy URL pro přidružení k aplikaci, aplikace musí používat schéma, na základě názvu řídí. Toho lze dosáhnout pomocí názvu identifikátor sady na iOS a název balíčku v systému Android, a pak Prohodit je, aby schéma adresy URL. Někteří poskytovatelé identity, jako je například Google, ale přiřadit identifikátorů klienta na základě názvů domén, které jsou pak vrátit zpět a použít jako schéma adresy URL. Například, pokud Google vytvoří id klienta `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`, bude schéma adresy URL `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`. Všimněte si, že pouze jeden `/` po komponentu schéma můžete zobrazit. Úplný příklad adresy URL přesměrování využitím vlastní schéma adresy URL je proto `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`.

Když webový prohlížeč obdrží odpověď od zprostředkovatele identity, který obsahuje vlastní schéma adresy URL, pokusí se načíst adresu URL, která se nezdaří. Místo toho vlastní schéma adresy URL je hlášených na operační systém vyvolání události. Operační systém potom kontroluje registrované schémat, a pokud ho najde, bude operační systém spuštění aplikace, které registrované schéma a odešle adresa URL pro přesměrování.

Tento mechanismus pro registraci vlastní schéma adresy URL s operačním systémem a zpracování schéma je specifická pro každou platformu.

#### <a name="ios"></a>iOS

V systému iOS, je zaregistrovaný vlastní schéma adresy URL v **Info.plist**, jak je znázorněno na následujícím snímku obrazovky:

![](oauth-images/info-plist.png "Registrace schéma adresy URL")

**Identifikátor** hodnota může být jakýkoli a **Role** musí být nastaven na hodnotu **prohlížeč**. **Schémata Url** hodnotu, která začíná `com.googleusercontent.apps`, můžete získat id klienta iOS pro projekt na [konzoly rozhraní Google API](http://console.developers.google.com).

Pokud poskytovatel identity dokončí požadavek ověřování, přesměruje na adresu URL pro přesměrování dané aplikace. Protože adresu URL používá vlastní schéma je výsledkem spuštění aplikace iOS, předávání v adrese URL jako parametr spuštění, kde je zpracován programovacím modelem `OpenUrl` přepsat aplikace `AppDelegate` třídy, která je znázorněno v následujícím příkladu kódu:

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    // Convert NSUrl to Uri
    var uri = new Uri(url.AbsoluteString);

    // Load redirectUrl page
    AuthenticationState.Authenticator.OnPageLoading(uri);

    return true;
}
```

`OpenUrl` Metoda převede adresu URL přijaté z `NSUrl` k .NET `Uri`, před zpracováním adresy URL přesměrování s `OnPageLoading` metoda veřejné `OAuth2Authenticator` objektu. To způsobí, že Xamarin.Auth zavřete kartu webového prohlížeče a analyzovat data přijaté OAuth.

#### <a name="android"></a>Android

V systému Android se vlastní schéma adresy URL je zaregistrován zadáním [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) atributu u `Activity` , který bude zpracovávat schéma. Pokud poskytovatel identity dokončí požadavek ověřování, přesměruje na adresu URL pro přesměrování dané aplikace. Jako adresu URL používá vlastní schéma je výsledkem Android spouštění aplikace, předávání v adrese URL jako parametr spuštění, kde je zpracován programovacím modelem `OnCreate` metodu `Activity` zaregistrovaná ke zpracování vlastní schéma adresy URL. Následující příklad kódu ukazuje třídu z ukázkové aplikace, která zpracovává vlastní schéma adresy URL:

```csharp
[Activity(Label = "CustomUrlSchemeInterceptorActivity", NoHistory = true, LaunchMode = LaunchMode.SingleTop )]
[IntentFilter(
    new[] { Intent.ActionView },
    Categories = new [] { Intent.CategoryDefault, Intent.CategoryBrowsable },
    DataSchemes = new [] { "<insert custom URL here>" },
    DataPath = "/oauth2redirect")]
public class CustomUrlSchemeInterceptorActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        // Convert Android.Net.Url to Uri
        var uri = new Uri(Intent.Data.ToString());

        // Load redirectUrl page
        AuthenticationState.Authenticator.OnPageLoading(uri);

        Finish();
    }
}
```

`DataSchemes` Vlastnost [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) musí být nastavena na identifikátor odstínech klienta, který se získávají z id Android klienta pro projekt na [konzoly rozhraní Google API](http://console.developers.google.com).

`OnCreate` Metoda převede adresu URL přijaté z `Android.Net.Url` k .NET `Uri`, před zpracováním adresy URL přesměrování s `OnPageLoading` metoda veřejné `OAuth2Authenticator` objektu. To způsobí, že Xamarin.Auth zavřete kartu webového prohlížeče a analyzovat data přijaté OAuth.

> [!IMPORTANT]
> V systému Android se používá Xamarin.Auth `CustomTabs` rozhraní API pro komunikaci se webový prohlížeč a operačního systému. Ale ji není zaručeno, že `CustomTabs` kompatibilní prohlížeč bude nainstalována na zařízení uživatele.

### <a name="examining-the-oauth-response"></a>Zkoumání odpovědi OAuth

Po analýze přijatá data OAuth, bude vyvolána Xamarin.Auth `OAuth2Authenticator.Completed` událostí. V obslužné rutiny události pro tuto událost `AuthenticatorCompletedEventArgs.IsAuthenticated` vlastnost lze použít k určení, zda byly úspěšné ověřování, jak je znázorněno v následujícím příkladu kódu:

```csharp
async void OnAuthCompleted(object sender, AuthenticatorCompletedEventArgs e)
{
  ...
  if (e.IsAuthenticated)
  {
    ...
  }
}
```

Data shromážděná z úspěšné ověření je k dispozici v `AuthenticatorCompletedEventArgs.Account` vlastnost. To zahrnuje přístupový token, který můžete použít k podepisování žádostí pro data do rozhraní API poskytované poskytovatelem identity.

### <a name="making-requests-for-data"></a>Provádění požadavky na Data

Po aplikace získá token přístupu, umožňuje požádat o `https://www.googleapis.com/oauth2/v2/userinfo` rozhraní API, data základní uživatel požadavku od zprostředkovatele identity. Tento požadavek se provádí s Xamarin.Auth na `OAuth2Request` třídy, která představuje požadavek, který slouží k ověření účtu načítají `OAuth2Authenticator` instance, jak je znázorněno v následujícím příkladu kódu:

```csharp
// UserInfoUrl = https://www.googleapis.com/oauth2/v2/userinfo
var request = new OAuth2Request ("GET", new Uri (Constants.UserInfoUrl), null, e.Account);
var response = await request.GetResponseAsync ();
if (response != null)
{
  string userJson = response.GetResponseText ();
  var user = JsonConvert.DeserializeObject<User> (userJson);
}
```

A také metodu protokolu HTTP a adresu URL rozhraní API `OAuth2Request` instance také určuje `Account` instance, který obsahuje přístupový token, který podepisuje požadavek na adresu URL zadanou v `Constants.UserInfoUrl` vlastnost. Poskytovatel identity vrátí základní uživatelská data jako odpověď JSON, včetně jména uživatelů a e-mailovou adresu, za předpokladu, že se přístupový token rozpozná, že je platný. Odpověď JSON je pak číst a deserializovat do `user` proměnné.

Další informace najdete v tématu [volání rozhraní API Google](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) na portálu pro vývojáře Google.

### <a name="storing-and-retrieving-account-information-on-devices"></a>Ukládání a načítání informací o účtu na zařízení

Xamarin.Auth bezpečně uloží `Account` objekty v účtu úložiště tak, aby aplikace není vždy nutné opakované ověření uživatele. `AccountStore` Třídy je zodpovědná za ukládání informací o účtu a je zajištěna podpora řetězce klíčů služby v iOS a `KeyStore` třídy v Android.

Následující příklad kódu ukazuje jak `Account` objekt je bezpečně uložen:

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

Uložené účty jsou jednoznačně identifikuje pomocí klíče skládá z účtu `Username` vlastnost a ID služby, které je řetězec, který se používá při načítání účty z účtu úložiště. Pokud `Account` byla uložena dříve, volání `Save` metoda znovu přepíše ho.

`Account` objekty pro služby může načíst volání `FindAccountsForService` metoda, jak je znázorněno v následujícím příkladu kódu:

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

`FindAccountsForService` Metoda vrátí `IEnumerable` kolekce `Account` objekty s první položka v kolekci se nastavuje jako odpovídající účet.

## <a name="summary"></a>Souhrn

Tento článek vysvětlení najdete postup používání Xamarin.Auth ke správě procesu ověřování v aplikaci Xamarin.Forms. Poskytuje Xamarin.Auth `OAuth2Authenticator` a `OAuth2Request` třídy, které jsou používány aplikací Xamarin.Forms využívat poskytovatelů identit, jako je například Google, Microsoft, Facebook či Twitter.


## <a name="related-links"></a>Související odkazy

- [OAuthNativeFlow (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/OAuthNativeFlow/)
- [OAuth 2.0 pro nativní aplikace](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Pomocí OAuth2.0 pro přístup k rozhraní API Google](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
