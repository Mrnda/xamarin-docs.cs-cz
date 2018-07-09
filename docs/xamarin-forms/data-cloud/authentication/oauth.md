---
title: Ověřování uživatelů pomocí zprostředkovatelů Identity
description: Tento článek vysvětluje, jak používat Xamarin.Auth ke správě procesu ověřování aplikace Xamarin.Forms.
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: 504b2789ef61b0339d1c32e92c852a779a193b52
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854763"
---
# <a name="authenticating-users-with-an-identity-provider"></a>Ověřování uživatelů pomocí zprostředkovatelů Identity

_Xamarin.Auth je multiplatformní SDK pro ověřování uživatelů a ukládání svých účtů. Zahrnuje ověřovací data OAuth, které poskytují podporu pro používání zprostředkovatelů identity, například Google, Microsoft, Facebook a Twitter. Tento článek vysvětluje, jak používat Xamarin.Auth ke správě procesu ověřování aplikace Xamarin.Forms._

OAuth je otevřený standard pro ověřování a umožňuje vlastníka prostředku upozornit poskytovatele prostředků, že mají oprávnění udělit nepředáme žádné třetí straně Pokud chcete přistupovat ke svým informacím bez sdílení identity vlastníky prostředků. Příklad tohoto by povolení uživatele upozornit zprostředkovatele identity (jako jsou Google, Microsoft, Facebook nebo Twitter), mají udělit oprávnění k aplikaci pro přístup k datům bez sdílení identitu uživatele. Běžně se používá jako přístup pro uživatele k přihlášení k webům a aplikacím pomocí zprostředkovatele identity, ale bez vystavení své heslo k webu nebo aplikaci.

Přehled toku ověřování při využívání poskytovatel identit OAuth vypadá takto:

1. Aplikace přejde prohlížeče na adresu URL zprostředkovatele identity.
1. Zprostředkovatel identity zpracovává ověřování uživatelů a vrátí autorizační kód do aplikace.
1. Aplikace vymění autorizační kód pro přístupový token od zprostředkovatele identity.
1. Aplikace používá přístupový token pro přístup k API na zprostředkovatele identity, jako je například rozhraní API umožňující získat základní údaje.

Ukázková aplikace ukazuje, jak používat Xamarin.Auth implementovat tok nativní ověřování proti Google. I když Google je použít jako zprostředkovatele identity v tomto tématu, přístup se vztahuje rovněž na jiných zprostředkovatelů identity. Další informace o ověřování pomocí koncového bodu Google OAuth 2.0, naleznete v tématu [pomocí OAuth 2.0 pro přístup k rozhraní API Google](https://developers.google.com/identity/protocols/OAuth2) na webu společnosti Google.

> [!NOTE]
> V systému iOS 9 a vyšší zabezpečení přenosu aplikací (ATS) vynucuje zabezpečené připojení mezi internetové prostředky (například aplikace back endového serveru) a aplikací, tím zabrání náhodnému zveřejnění citlivých informací. Protože ATS je povolena ve výchozím nastavení aplikace vytvořené pro iOS 9, všechna připojení bude v souladu s požadavky na ATS zabezpečení. Pokud připojení tyto požadavky nesplňujete, že selže s výjimkou.
> ATS můžete vyjádřit výslovný souhlas z Pokud není možné použít `HTTPS` protokolu a zabezpečená komunikace pro internetové prostředky. Toho lze dosáhnout aktualizací aplikace **Info.plist** souboru. Další informace najdete v části [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="using-xamarinauth-to-authenticate-users"></a>Pomocí Xamarin.Auth k ověřování uživatelů

Xamarin.Auth podporuje dvě metody pro aplikace pro interakci s koncový bod autorizace zprostředkovatele identity:

1. Pomocí vložených webové zobrazení. Zatímco byla běžnou praxí, už se nedoporučuje z následujících důvodů:

    - Aplikace, který je hostitelem webové zobrazení můžete přístup k přihlašovací údaje úplného ověření uživatele, ne jenom OAuth udělení autorizace, která je určená pro aplikace. To porušuje principu nejnižší úrovně oprávnění, protože aplikace nemá přístup k přihlašovacím údajům výkonnější než vyžaduje, potenciálně zvýšení povrchu napadení aplikace.
    - Hostitelská aplikace může zaznamenat uživatelská jména a hesla, automaticky odesílání formulářů a obejít souhlas uživatele a zkopírujte soubory cookie relace a použít je k provádění ověřených akce jako uživatel.
    - Vložený webová zobrazení Nesdílejte stav ověřování pomocí jiných aplikací nebo webový prohlížeč zařízení, by uživatel musel přihlášení pro každou žádost o autorizaci, což se považuje za nižší než uživatelské prostředí.
    - Některé koncové body autorizaci provést kroky k detekovat a blokovat žádosti o autorizaci, které pocházejí z webového zobrazení.

1. Pomocí webového prohlížeče zařízení, což je doporučený postup. Pomocí prohlížeče zařízení pro žádosti OAuth se zvýší užitná hodnota aplikace, jak uživatelé potřebují jenom pro přihlášení ke zprostředkovateli identity jednou za zařízení, zvýšení míry úspěšnosti přihlašování a autorizaci toků v aplikaci. Prohlížeči v zařízení také poskytuje vylepšené zabezpečení aplikací budou moct prohlížet a upravovat obsah ve webovém zobrazení, ale ne obsah zobrazená v prohlížeči. Jedná se o postup provést v této článku a ukázkové aplikace.

Základní přehled o tom, jak ukázková aplikace používá k ověřování uživatelů a jejich základní data načíst Xamarin.Auth můžete vidět v následujícím diagramu:

![](oauth-images/google-auth.png "Použití Xamarin.Auth k ověřování pomocí Googlu")

Aplikace provede požadavek na ověření pomocí služby Google `OAuth2Authenticator` třídy. Odpověď ověřování, je vrácena, jakmile uživatel se úspěšně ověřil službou Google pomocí jejich přihlašovací stránku, který obsahuje přístupový token. Aplikace pak odešle požadavek do Googlu pro základní údaje, pomocí `OAuth2Request` třída s tímto tokenem přístupu nebudou zahrnuty do žádosti.

### <a name="setup"></a>Instalace

Projekt konzoly Google API musí být vytvořený přihlašovací Google integrovat aplikace Xamarin.Forms. To lze provést následujícím způsobem:

1. Přejděte [konzole rozhraní API Google](http://console.developers.google.com) web a přihlaste se pomocí přihlašovacích údajů k účtu Google.
1. Z projektu rozevíracího seznamu vyberte existující projekt nebo vytvořte novou.
1. Na bočním panelu v části "Rozhraní API Správce", vyberte **pověření**a pak **kartě obrazovky souhlas OAuth**. Zvolte **e-mailová adresa**, zadejte **název produktu, který se uživatelům zobrazí**a stiskněte klávesu **Uložit**.
1. V **pověření** kartu, vyberte možnost **Vytvořte přihlašovací údaje** rozevírací seznam a zvolte **ID klienta OAuth**.
1. V části **typ aplikace**, vyberte platformu, mobilní aplikace bude spuštěna na (**iOS** nebo **Android**).
1. Zadejte požadované podrobnosti a vyberte **vytvořit** tlačítko.

> [!NOTE]
> ID klienta umožňuje aplikaci přístup k povolených rozhraní API Google a pro mobilní aplikace je jedinečný pro jednu platformu. Proto **ID klienta OAuth** by měl být vytvořen pro každou platformu, která bude používat přihlášení Google.

Po provedení těchto kroků, Xamarin.Auth umožňuje spustit tok, který ověřování OAuth2 s Google.

### <a name="creating-and-configuring-an-authenticator"></a>Vytváření a konfiguraci ověřovací data

Společnosti Xamarin.Auth `OAuth2Authenticator` třídy je zodpovědná za zpracování tok ověřování OAuth. Následující příklad kódu ukazuje instance `OAuth2Authenticator` třídy při provádění ověřování přes webový prohlížeč zařízení:

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

`OAuth2Authenticator` Třídy vyžaduje několik parametrů, které jsou následující:

- **ID klienta** – určuje klienta, která odeslala žádost a můžete získat z projektu [konzole rozhraní API Google](http://console.developers.google.com).
- **Tajný kód klienta** – to by mělo být `null` nebo `string.Empty`.
- **Obor** – určuje přístup k rozhraní API žádá aplikaci, a hodnota informuje o tom, které se zobrazí uživateli, obrazovkami pro vyjádření souhlasu. Další informace o oborech najdete v tématu [autorizace rozhraní API požadavek](https://developers.google.com/+/web/api/rest/oauth) na webu společnosti Google.
- **Autorizace adresy URL** – Určuje adresu URL, kde bude získaných autorizační kód.
- **Adresa URL přesměrování** – Určuje adresu URL, kde se pošle odpověď. Hodnota tohoto parametru musí odpovídat jedné z hodnot, které se zobrazí v **pověření** projektu v kartě [konzole pro vývojáře Google](https://console.developers.google.com/).
- **Adresa AccessToken Url** – Určuje adresu URL používaná pro požádání o přístupové tokeny po získání autorizačního kódu.
- **GetUserNameAsync Func** – což je volitelné `Func` , který se použije pro asynchronní načtení uživatelské jméno účtu po je byl úspěšně ověřen.
- **Použijte nativní uživatelské rozhraní** – `boolean` hodnotu, která udává, jestli se má použít webový prohlížeč zařízení k provedení žádosti o ověření.

### <a name="setup-authentication-event-handlers"></a>Nastavení ověřování obslužných rutin událostí

Před prezentace uživatelského rozhraní, obslužná rutina události `OAuth2Authenticator.Completed` události musí být zaregistrovaný, jak je znázorněno v následujícím příkladu kódu:

```csharp
authenticator.Completed += OnAuthCompleted;
```

Tato událost se aktivuje, když uživatel úspěšně ověří nebo zruší přihlášení.

Volitelně obslužná rutina události `OAuth2Authenticator.Error` události lze také zaregistrovat.

### <a name="presenting-the-sign-in-user-interface"></a>Nabízí ten samý přihlášení uživatelského rozhraní

Přihlašovací uživatelské rozhraní lze zobrazit uživateli pomocí skládání Xamarin.Auth přihlášení, které musí být inicializován v každém projektu platformy. Následující příklad kódu ukazuje, jak inicializovat skládání přihlášení v `AppDelegate` třídu v projektu pro iOS:

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

Následující příklad kódu ukazuje, jak inicializovat skládání přihlášení v `MainActivity` třídu v projektu pro Android:

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

Projekt .NET Standard knihovny lze poté vyvolat skládání přihlášení následujícím způsobem:

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

Všimněte si, že argument `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` metoda je `OAuth2Authenticator` instance. Když `Login` vyvolání metody, přihlášení uživatelského rozhraní se budou zobrazovat uživateli na kartě z webového prohlížeče zařízení, která je znázorněna na následujících snímcích obrazovky:

![](oauth-images/login.png "Přihlášení Google")

### <a name="processing-the-redirect-url"></a>Zpracování adresy URL pro přesměrování

Po dokončení procesu ověřování uživatele ovládací prvek vrátí k aplikaci z kartu webového prohlížeče. Toho dosáhnete, když si zaregistrujete vlastní schéma adresy URL pro přesměrování adresu URL, která je vrácena z procesu ověřování a potom zjišťování a zpracování vlastní adresu URL, jakmile je odeslána.

Při výběru vlastní schéma adresy URL pro přidružení k aplikaci, aplikace musí používat schéma založené na názvu do pole pod kontrolou. Toho lze dosáhnout pomocí název identifikátoru sady prostředků v Iosu a název balíčku v Androidu a potom stornování je, aby schéma adresy URL. Někteří poskytovatelé identity, jako jsou Google, ale přiřadit identifikátory klientů na základě názvů domén, které jsou pak vrátit zpět a použít jako schéma adresy URL. Například, pokud Google vytvoří id klienta `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`, bude schéma adresy URL `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`. Mějte na paměti, který pouze jeden `/` se mohou objevit za součást schématu. Proto je kompletní příklad využívá vlastní schéma adresy URL adresy URL přesměrování `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`.

Když webový prohlížeč obdrží odpověď od poskytovatele identity, která obsahuje vlastní schéma adresy URL, pokusí se načíst adresu URL, která se nezdaří. Místo toho vlastní schéma adresy URL je ohlášena v operačním systému vyvolání události. Operační systém potom vyhledá registrovaná schémata, a pokud ho najde, operační systém spuštění aplikace, které registrované schéma a odeslat ji adresy URL pro přesměrování.

Mechanismus pro registraci vlastní schéma adresy URL s operačním systémem a zpracování schéma je specifické pro jednotlivé platformy.

#### <a name="ios"></a>iOS

V systémech iOS, je zaregistrovaný vlastní schéma adresy URL v **Info.plist**, jak je znázorněno na následujícím snímku obrazovky:

![](oauth-images/info-plist.png "Registrace schéma adresy URL")

**Identifikátor** hodnota může být cokoli a **Role** musí být nastaven na hodnotu **prohlížeč**. **Schémata Url** hodnotu, která začíná `com.googleusercontent.apps`, můžete získat id klienta iOS pro projekt na [konzole rozhraní API Google](http://console.developers.google.com).

Po dokončení zprostředkovatele identity žádost o autorizaci, přesměruje na adresu URL pro přesměrování aplikace. Vzhledem k tomu, že adresa URL používá vlastní schéma je výsledkem spuštění aplikace iOS, předá v adrese URL jako parametr spuštění, kde je zpracován programovacím modelem `OpenUrl` přepsání aplikace `AppDelegate` třída, která je znázorněna v následujícím příkladu kódu:

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

`OpenUrl` Metoda převede adresu URL přijatý z `NSUrl` k .NET `Uri`, před zpracováním adresy URL přesměrování se `OnPageLoading` metoda veřejné `OAuth2Authenticator` objektu. To způsobí, že Xamarin.Auth zavřete kartu webového prohlížeče a analyzovat přijatá data OAuth.

#### <a name="android"></a>Android

V systému Android, je tak, že zadáte zaregistrovaný vlastní schéma adresy URL [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) atribut na `Activity` , která zpracuje schéma. Po dokončení zprostředkovatele identity žádost o autorizaci, přesměruje na adresu URL pro přesměrování aplikace. Jako adresa URL používá vlastní schéma je výsledkem spuštění aplikace Android, předá v adrese URL jako parametr spuštění, kde je zpracován programovacím modelem `OnCreate` metodu `Activity` registrován pro obsluhu vlastní schéma adresy URL. Následující příklad kódu ukazuje třídu v ukázkové aplikaci, která zpracovává vlastní schéma adresy URL:

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

`DataSchemes` Vlastnost [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) musí být nastavena na identifikátor obrácený klienta, která se získá z id Android klienta pro projekt na [konzole rozhraní API Google](http://console.developers.google.com).

`OnCreate` Metoda převede adresu URL přijatý z `Android.Net.Url` k .NET `Uri`, před zpracováním adresy URL přesměrování se `OnPageLoading` metoda veřejné `OAuth2Authenticator` objektu. To způsobí, že Xamarin.Auth zavřete kartu webového prohlížeče a analyzovat přijatá data OAuth.

> [!IMPORTANT]
> V systému Android, používá Xamarin.Auth `CustomTabs` rozhraní API pro komunikaci pomocí webového prohlížeče a operační systém. Ale to není zaručeno, že `CustomTabs` prohlížeč kompatibilní se nainstaluje na zařízení uživatele.

### <a name="examining-the-oauth-response"></a>Zkoumání odpovědi OAuth

Po analýze přijatá data OAuth, bude vyvolána Xamarin.Auth `OAuth2Authenticator.Completed` událostí. V obslužné rutině události pro tuto událost `AuthenticatorCompletedEventArgs.IsAuthenticated` vlastnost lze použít k určení, zda bylo úspěšné ověřování, jak je znázorněno v následujícím příkladu kódu:

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

Je k dispozici v dat shromážděných z po provedení úspěšného ověření `AuthenticatorCompletedEventArgs.Account` vlastnost. Jedná se o přístupový token, který lze použít k podepisování žádostí pro data do rozhraní API od poskytovatele identity.

### <a name="making-requests-for-data"></a>Vytváření žádostí o Data

Jakmile aplikace získá přístupový token, používá se k vytvořit žádost o `https://www.googleapis.com/oauth2/v2/userinfo` rozhraní API požadavek základní uživatelské údaje od zprostředkovatele identity. Tento požadavek s vaší Xamarin.Auth `OAuth2Request` třídu, která představuje požadavek, který je ověřený pomocí účtu získaných `OAuth2Authenticator` instance, jak je znázorněno v následujícím příkladu kódu:

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

Metoda protokolu HTTP a adresy URL rozhraní API `OAuth2Request` instance také určuje `Account` instance, která obsahuje přístupový token, který podepisuje požadavku k adrese URL zadané hodnotou `Constants.UserInfoUrl` vlastnost. Zprostředkovatel identity vrátí základní údaje jako odpověď ve formátu JSON, včetně uživatelské jméno a e-mailovou adresu, za předpokladu, že rozpozná přístupový token jako neplatný. Odpověď JSON je přečtení a deserializovat do `user` proměnné.

Další informace najdete v tématu [volání rozhraní API Google](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) na portálu pro vývojáře Google.

### <a name="storing-and-retrieving-account-information-on-devices"></a>Ukládání a načítání informací o účtu na zařízení

Bezpečně uloží Xamarin.Auth `Account` objekty v rámci účtu úložiště tak, aby aplikace není vždy nutné opakované ověření uživatele. `AccountStore` Třídy je zodpovědná za ukládání informací o účtu a je zajištěná řetězce klíčů služby v iOS a `KeyStore` třídy v Androidu.

Následující příklad kódu ukazuje jak `Account` objekt je bezpečně uložit:

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

Uložené účty se jednoznačně identifikují pomocí klíče skládá z účtu `Username` vlastnost a ID služby, což je řetězec, který se používá při načítání účtů z účtu úložiště. Pokud `Account` byla uložena dříve, volání `Save` metoda znovu přepíše ho.

`Account` objekty pro služby může být načten voláním `FindAccountsForService` způsob, jak je znázorněno v následujícím příkladu kódu:

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

`FindAccountsForService` Vrátí metoda `IEnumerable` kolekce `Account` objektů s první položky v kolekci nastavena jako odpovídající účet.

## <a name="summary"></a>Souhrn

Tento článek vysvětlil, jak používat Xamarin.Auth ke správě procesu ověřování aplikace Xamarin.Forms. Poskytuje Xamarin.Auth `OAuth2Authenticator` a `OAuth2Request` třídy, které jsou používány aplikací Xamarin.Forms využívat zprostředkovatelů identity, například Google, Microsoft, Facebook a Twitter.


## <a name="related-links"></a>Související odkazy

- [OAuthNativeFlow (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/OAuthNativeFlow/)
- [OAuth 2.0 pro nativní aplikace](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Pomocí OAuth 2.0 pro přístup k rozhraní API Google](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
