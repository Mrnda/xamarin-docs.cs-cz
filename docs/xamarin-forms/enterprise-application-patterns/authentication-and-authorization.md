---
title: Ověřování a autorizace
description: Tato kapitola vysvětluje, jak mobilní aplikace eShopOnContainers provádí ověřování a autorizaci proti kontejnerizované mikroslužeb.
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: 9e6cfa566ab455841b3f11e4a857dcf678083417
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242424"
---
# <a name="authentication-and-authorization"></a>Ověřování a autorizace

Ověřování je proces získání identifikačních pověření, jako je jméno a heslo od uživatele a ověření těchto pověření podle autoritu. Pokud jsou pověření platná, považována za typ entity, která je odeslána přihlašovací údaje ověřenou identitu. Po ověření identity autorizačního procesu Určuje, zda tuto identitu má přístup k danému prostředku.

Existuje mnoho přístupů k integraci do aplikace na platformě Xamarin.Forms, která komunikuje s webovou aplikaci ASP.NET MVC, včetně použití zprostředkovatele externího ověřování, jako je Microsoft, Google, ASP.NET Core Identity, ověřování a autorizace Middlewaru Facebook, nebo služby Twitter a ověřování. Mobilní aplikace eShopOnContainers provádí ověřování a autorizaci s mikroslužbu kontejnerizované identity, která používá IdentityServer 4. Mobilní aplikace vyžaduje tokeny zabezpečení z IdentityServer, pro ověření uživatele nebo pro přístup k prostředku. Pro IdentityServer problém tokeny jménem uživatele uživatel musí přihlásit k IdentityServer. IdentityServer není však poskytuje uživatelské rozhraní nebo databáze pro ověřování. Proto v eShopOnContainers odkaz na aplikaci ASP.NET Core Identity slouží pro tento účel.

## <a name="authentication"></a>Ověřování

Požaduje se ověření, když aplikace potřebuje znát identitu aktuálního uživatele. Primární mechanismus ASP.NET Core identifikace uživatelů je systém členství ASP.NET Core Identity, která uchovává informace o uživateli v úložišti dat konfigurovat tak, že vývojář. Tohle úložiště dat obvykle bude úložišti objektu EntityFramework, i když se balíčky třetích stran nebo vlastní úložiště můžete použít k ukládání informací o identitě úložiště Azure, Azure Cosmos DB nebo jiné umístění.

Pro scénáře ověřování, které využívají úložiště dat místních uživatelů a který zachování informací o identitě mezi požadavky pomocí souborů cookie (jako je typické v webových aplikací ASP.NET MVC), ASP.NET Core Identity je vhodné řešení. Soubory cookie jsou však není vždy fyzické prostředky k uchování a přenosu dat. Například webovou aplikaci ASP.NET Core, který zveřejňuje RESTful koncové body, které jsou přístupné z mobilní aplikace bude obvykle nutné použít ověřování tokenu nosiče, protože soubory cookie nelze použít v tomto scénáři. Nosné tokeny však snadno můžete být načtena a zahrnuty v hlavičce autorizace webové požadavky z mobilní aplikace.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Vydávání tokenů nosiče pomocí IdentityServer 4

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) je otevřeným zdrojem OpenID Connect a OAuth 2.0 framework pro ASP.NET Core, který můžete použít pro mnoho scénářů ověřování a autorizaci, včetně vystavování tokenů zabezpečení pro místní uživatele ASP.NET Core Identity.

> [!NOTE]
> OpenID Connect a OAuth 2.0 jsou velmi podobné přitom má jiné odpovědnosti.

OpenID Connect je k ověřování vrstva nad protokol OAuth 2.0. OAuth 2 je protokol, který umožňuje aplikacím požadovat přístupové tokeny od služby tokenů zabezpečení a použít je pro komunikaci s rozhraními API. Toto delegování snižuje složitost v klientské aplikace a rozhraní API, protože může být centralizované ověřování a autorizace.

Kombinace OpenID Connect a OAuth 2.0 sloučit dva základní zabezpečení se ověřování a přístup pomocí rozhraní API a IdentityServer 4 je implementace těchto protokolů.

V aplikacích používajících přímou komunikaci klienta mikroslužbu, jako je například aplikace odkaz eShopOnContainers vyhrazené ověřování mikroslužby, který funguje jako zabezpečení tokenu služby (STS) lze použít k ověřování uživatelů, jak je znázorněno na obrázku 9-1. Další informace o přímou komunikaci klienta mikroslužbu najdete v tématu [komunikace mezi klientem a Mikroslužeb](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices).

![](authentication-and-authorization-images/authentication.png "Ověřování pomocí vyhrazené ověřování mikroslužby")

**Obrázek 9 – 1:** ověřování pomocí vyhrazené ověřování mikroslužby

EShopOnContainers mobilní aplikace komunikuje s mikroslužbu identity, který používá IdentityServer 4 k provedení ověřování a řízení přístupu pro rozhraní API. Mobilní aplikace požaduje proto tokeny z IdentityServer, pro ověření uživatele nebo pro přístup k prostředku:

-   Ověřování uživatelů s IdentityServer je dosaženo tím, že požádá mobilní aplikace *identity* token, který představuje výsledek ověřovacího procesu. Úplné minimálně obsahuje identifikátor pro uživatele a informace o tom, jak a kdy se uživatel ověřen. Může také obsahovat další identifikační údaje.
-   Přístup k prostředku s IdentityServer je dosaženo tím, že požádá mobilní aplikace *přístup* token, který umožňuje přístup k prostředku rozhraní API. Klienti požádat o přístupové tokeny, které předávají do rozhraní API. Přístupové tokeny obsahují informace o klientovi a uživatele (pokud existuje). Rozhraní API pak pomocí těchto informací k autorizaci přístupu k jejich datům.

> [!NOTE]
> Klient musí být zaregistrován u IdentityServer předtím, než ji může požádat o tokeny.

### <a name="adding-identityserver-to-a-web-application"></a>Přidání IdentityServer do webové aplikace

V pořadí pro webovou aplikaci ASP.NET Core používat IdentityServer 4 musí být přidané na řešení webové aplikace Visual Studio. Další informace najdete v tématu [instalační program a přehled](https://identityserver4.readthedocs.io/en/release/quickstarts/0_overview.html) v dokumentaci k IdentityServer.

Jakmile IdentityServer je součástí řešení sady Visual Studio webové aplikace, musí být přidána do kanálu, se zpracováním požadavků HTTP webové aplikace, tak, aby mohl obsluhovat požadavky na koncové body OpenID Connect a OAuth 2.0. Můžete toho dosáhnout v `Configure` metoda ve webové aplikaci `Startup` třídy, jak je ukázáno v následujícím příkladu kódu:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Záleží na pořadí v kanálu zpracování požadavku HTTP webové aplikace. IdentityServer musí proto přidat do tohoto kanálu před uživatelského rozhraní framework, který implementuje přihlašovací obrazovku.

### <a name="configuring-identityserver"></a>Konfigurace IdentityServer

IdentityServer by měl být nakonfigurovaný v `ConfigureServices` metoda ve webové aplikaci `Startup` třída voláním `services.AddIdentityServer` metoda, jak ukazuje následující příklad kódu z eShopOnContainers referenční aplikace:

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

Po volání `services.AddIdentityServer` metoda, další rozhraní fluent API jsou volat za účelem proveďte následující konfiguraci:

-   Přihlašovací údaje používané k podepisování.
-   Rozhraní API a identity prostředky, které uživatelé si mohou vyžádat přístup.
-   Klienti, kteří se připojují k žádosti o tokeny.
-   Jádro ASP.NET Identity.

>💡 **Tip**: dynamicky načíst konfiguraci IdentityServer 4. Rozhraní API IdentityServer 4 umožňují nastavit IdentityServer ze seznamu v paměti objektů konfigurace. V aplikaci odkaz eShopOnContainers těchto kolekcí v paměti jsou pevně zakódovaná do aplikace. Ale v produkčních scénářích mohou být načíst dynamicky z konfiguračního souboru nebo z databáze.

Informace o konfiguraci IdentityServer na používání technologie ASP.NET Identity Core najdete v tématu [pomocí ASP.NET Identity Core](https://identityserver4.readthedocs.io/en/release/quickstarts/6_aspnet_identity.html) v dokumentaci k IdentityServer.

#### <a name="configuring-api-resources"></a>Konfigurace prostředků rozhraní API

Při konfiguraci zdroje rozhraní API `AddInMemoryApiResources` metoda očekává `IEnumerable<ApiResource>` kolekce. Následující příklad kódu ukazuje `GetApis` metoda, která poskytuje tuto kolekci na eShopOnContainers odkazovat aplikace:

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

Tato metoda určuje, že by měl IdentityServer chránit objednávek a košík rozhraní API. Proto IdentityServer spravovaný přístup tokeny se bude vyžadovat při volání těchto rozhraní API. Další informace o `ApiResource` zadejte najdete v tématu [rozhraní API prostředků](https://identityserver4.readthedocs.io/en/release/reference/api_resource.html#refapiresource) v dokumentaci k IdentityServer 4.

#### <a name="configuring-identity-resources"></a>Konfigurace prostředků Identity

Při konfiguraci identity prostředky `AddInMemoryIdentityResources` metoda očekává `IEnumerable<IdentityResource>` kolekce. Identity prostředky jsou data, jako je například ID uživatele, název nebo e-mailovou adresu. Každý prostředek, identity má jedinečný název a typy deklarací identity libovolné lze přiřadit k, které budou zahrnuty v tokenu identity pro uživatele. Následující příklad kódu ukazuje `GetResources` metoda, která poskytuje tuto kolekci na eShopOnContainers odkazovat aplikace:

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

Specifikace OpenID Connect určuje některé [standardní identity prostředky](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims). Minimální požadavek je, že podpora je k dispozici pro generování jedinečné ID pro uživatele. Toho se dosáhne vystavení `IdentityResources.OpenId` identity prostředků.

> [!NOTE]
> `IdentityResources` Třída podporuje všechny obory definované ve specifikaci OpenID Connect (openid, e-mailu, profil, Telefon a adresa).

IdentityServer podporuje také definovat vlastní identity prostředky. Další informace najdete v tématu [definování vlastní identity prostředky](https://identityserver4.readthedocs.io/en/release/topics/resources.html#defining-custom-identity-resources) v dokumentaci k IdentityServer. Další informace o `IdentityResource` zadejte najdete v tématu [Identity prostředků](https://identityserver4.readthedocs.io/en/release/reference/identity_resource.html) v dokumentaci k IdentityServer 4.

#### <a name="configuring-clients"></a>Konfigurace klientů

Klienti jsou aplikace, které můžete požádat IdentityServer tokeny. Následující nastavení musí být obvykle definovány pro každého klienta minimálně:

-   ID klienta jedinečný.
-   Povolené interakce s služba tokenu (označované jako typ udělení).
-   Umístění, kde tokeny přístupu a identit a posílají se na (označované jako identifikátor URI pro přesměrování).
-   Seznam prostředků, které klient je povolený přístup k (označované jako obory).

Při konfiguraci klientů, `AddInMemoryClients` metoda očekává `IEnumerable<Client>` kolekce. Následující příklad kódu ukazuje konfigurace pro mobilní aplikaci eShopOnContainers v `GetClients` metoda, která poskytuje tuto kolekci na eShopOnContainers odkazovat aplikace:

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

Tato konfigurace určuje data pro následující vlastnosti:

-   `ClientId`: Jedinečné ID klienta.
-   `ClientName`: Klient zobrazovaný název, který se používá k protokolování a na obrazovce souhlasu.
-   `AllowedGrantTypes`: Určuje, jak chce využívat IdentityServer klienta. Další informace najdete v části [konfigurace tok ověřování](#configuring_the_authentication_flow).
-   `ClientSecrets`: Určuje tajný pověření klienta, které se použijí při požaduje tokeny z koncového bodu token.
-   `RedirectUris`: Určuje povolené identifikátory URI, do kterého se mají vrátit tokeny nebo autorizačních kódů.
-   `RequireConsent`: Určuje, zda souhlasu obrazovky je povinný.
-   `RequirePkce`: Určuje, zda musí klienti, kteří používají autorizační kód odeslat doklad klíč.
-   `PostLogoutRedirectUris`: Určuje povolené identifikátory URI pro přesměrování po odhlášení.
-   `AllowedCorsOrigins`: Určuje počátek klienta tak, aby IdentityServer můžete povolit volání mezi zdroji z tohoto počátku.
-   `AllowedScopes`: Určuje prostředky, které má klient přístup k. Ve výchozím nastavení klient nemá přístup k žádným prostředkům.
-   `AllowOfflineAccess`: Určuje, zda může klient požadovat tokeny obnovení.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>Konfigurace tok ověřování

Tok ověření mezi klientem a IdentityServer lze nakonfigurovat tak, že zadáte typy udělení v `Client.AllowedGrantTypes` vlastnost. Specifikace OpenID Connect a OAuth 2.0 definovat všechny toky ověřování, včetně:

-   Implicitní. Tento tok je optimalizovaná pro aplikace založené na prohlížeči a slouží pro uživatele pouze ověřování nebo ověřování a přístup k žádosti o tokeny. Všechny tokeny jsou odeslány prostřednictvím prohlížeče a proto pokročilé funkce, jako jsou tokeny obnovení nejsou povoleny.
-   Autorizační kód. Tento postup umožňuje načíst tokeny na používající back channel, na rozdíl od front kanál prohlížeče, zároveň také podporuje ověřování klientů.
-   Hybridní. Tento tok je kombinací implicitní a typy udělení autorizačního kódu. Identity token se přenáší prostřednictvím kanálu prohlížeče a obsahuje odpověď podepsaný protokolu společně s další artefaktů, jako jsou autorizační kód. Po úspěšném ověření odpovědi používající back channel slouží k načtení přístup a aktualizovat token.

> [!TIP]
> Použijte hybridní tok ověření. Tok ověření hybridní snižuje počet útoků, které platí pro kanál prohlížeče a je doporučený postup pro nativní aplikace, které chcete načíst tokeny přístupu ke (a případně obnovovacích tokenů).

Další informace o ověřování toky najdete v tématu [typy udělení](https://identityserver4.readthedocs.io/en/release/topics/grant_types.html) v dokumentaci k IdentityServer 4.

### <a name="performing-authentication"></a>Ověřování

Pro IdentityServer problém tokeny jménem uživatele uživatel musí přihlásit k IdentityServer. IdentityServer není však poskytuje uživatelské rozhraní nebo databáze pro ověřování. Proto v eShopOnContainers odkaz na aplikaci ASP.NET Core Identity slouží pro tento účel.

Mobilní aplikace eShopOnContainers ověří IdentityServer s hybridní tok ověřování, což je znázorněno na obrázku 9-2.

![](authentication-and-authorization-images/sign-in.png "Přehled procesu přihlášení")

**Obrázek 9 – 2:** přehled procesu přihlášení

Přišla žádost o přihlášení `<base endpoint>:5105/connect/authorize`. Po úspěšném ověření IdentityServer vrátí odpověď ověřování obsahující autorizační kód a identity token. Autorizační kód se pak posílají do `<base endpoint>:5105/connect/token`, který odpoví přístup, identity a obnovovacích tokenů.

EShopOnContainers mobilní aplikace přihlásí mimo IdentityServer odesláním požadavku na `<base endpoint>:5105/connect/endsession`, s další parametry. Po odhlášení dojde, IdentityServer odpoví odesláním na post odhlášení identifikátor URI přesměrování zpět do mobilní aplikace. Obrázek 9-3 znázorňuje tento proces.

![](authentication-and-authorization-images/sign-out.png "Přehled procesu odhlášení")

**Obrázek 9 – 3:** přehled proces přihlášení

V mobilní aplikaci eShopOnContainers komunikaci s IdentityServer provádí `IdentityService` třídy, které implementuje `IIdentityService` rozhraní. Toto rozhraní určuje, že musíte zadat implementující třídu `CreateAuthorizationRequest`, `CreateLogoutRequest`, a `GetTokenAsync` metody.

#### <a name="signing-in"></a>Přihlášení

Když uživatel klepnutím **přihlášení** tlačítko `LoginView`, `SignInCommand` v `LoginViewModel` proveden třída, která zase provádí `SignInAsync` metoda. Následující příklad kódu ukazuje této metody:

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Tato metoda vyvolá `CreateAuthorizationRequest` metoda v `IdentityService` třídy, která je znázorněno v následujícím příkladu kódu:

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.IdentityCallback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

Tato metoda vytvoří identifikátor URI pro IdentityServer na [koncový bod autorizace](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), s požadovanými parametry. Koncový bod autorizace je na `/connect/authorize` na portu 5105 základní koncového bodu, které jsou zveřejněné jako uživatelské nastavení. Další informace o nastavení uživatele najdete v tématu [Configuration Management](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Prostor pro útoky eShopOnContainers mobilní aplikace se sníží o implementace ověření klíče pro rozšíření kódu Exchange (PKCE) pro OAuth. PKCE chrání autorizační kód z používá, pokud je zachycen. Toho se dosáhne klienta generování tajný verifier hodnotu hash, je předaná žádost o ověření, a který se zobrazí nezašifrované při uplatňuje autorizační kód. Další informace o PKCE najdete v tématu [kód ověření pro výměnu kódu OAuth veřejné klienty](https://tools.ietf.org/html/rfc7636) na webové stránce Internet Engineering Task Force.

Vrácený identifikátor URI je uložen v `LoginUrl` vlastnost `LoginViewModel` třídy. Když `IsLogin` vlastnost stane `true`, [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) v `LoginView` se zobrazí. `WebView` Vazby dat jeho [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) vlastnost, která má `LoginUrl` vlastnost `LoginViewModel` třídy a proto odešle přihlašovací požadavek IdentityServer při `LoginUrl` je nastavena na Koncový bod autorizace IdentityServer společnosti. Když IdentityServer obdrží tuto žádost a uživatel není ověřen, `WebView` bude přesměrován na nakonfigurované přihlašovací stránku, která se zobrazí v obrázek 9 – 4.

![](authentication-and-authorization-images/login.png "Přihlašovací stránka zobrazí webového zobrazení")

**Obrázek 9 – 4:** přihlašovací stránky zobrazí webového zobrazení

Po dokončení přihlášení [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) bude přesměrován na návratový identifikátor URI. To `WebView` způsobí, že navigace `NavigateAsync` metoda v `LoginViewModel` třídy mají být provedeny, která je znázorněno v následujícím příkladu kódu:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

Tato metoda analyzuje odpověď ověřování, obsažené v návratové URI a za předpokladu, že platný autorizační kód je k dispozici, se odešle požadavek na IdentityServer [koncový bod tokenu](https://identityserver4.readthedocs.io/en/release/endpoints/token.html), předávání autorizační kód PKCE tajný ověřovatele a další povinné parametry. Koncový bod token je na `/connect/token` na portu 5105 základní koncového bodu, které jsou zveřejněné jako uživatelské nastavení. Další informace o nastavení uživatele najdete v tématu [Configuration Management](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

>💡 **Tip**: ověření vrátit identifikátory URI. I když eShopOnContainers mobilní aplikace nemá ověřit návratový identifikátor URI, osvědčeným postupem je k ověření, že návratovým URI odkazuje do vhodného umístění, aby se zabránilo útokům otevřete přesměrování.

Pokud koncový bod token obdrží platný autorizační kód a PKCE tajný ověřovatele, odpoví přístupový token, tokenu identity a token obnovení. Přístupový token (který umožňuje přístup k prostředkům rozhraní API) a tokenu identity jsou pak uloženy jako nastavení aplikace a provádí navigaci na stránce. Proto v mobilní aplikaci eShopOnContainers celkový efekt je toto: za předpokladu, že se uživatelé moct úspěšně ověřit s IdentityServer, že jsou přešli `MainView` stránky, [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) který zobrazí `CatalogView` jako jeho vybraná karta.

Informace o navigaci na stránce najdete v tématu [navigační](~/xamarin-forms/enterprise-application-patterns/navigation.md). Informace o [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) navigační způsobí, že metoda modelu zobrazení provést, najdete v článku [vyvolání navigační pomocí chování](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Informace o nastavení aplikace najdete v tématu [Configuration Management](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> EShopOnContainers také umožňuje imitované přihlášení, když je aplikace nakonfigurovaná pro použití imitované služeb v `SettingsView`. V tomto režimu není aplikace komunikovat s IdentityServer, místo toho umožnit uživateli přihlášení pomocí všechny přihlašovací údaje.

#### <a name="signing-out"></a>Podepisování na více systémů

Když uživatel klepnutím **ODHLÁSIT** tlačítka na `ProfileView`, `LogoutCommand` v `ProfileViewModel` proveden třída, která zase provádí `LogoutAsync` metoda. Tato metoda provádí navigaci na stránce na `LoginView` stránky, předávání `LogoutParameter` instance nastavit na `true` jako parametr. Další informace o předávání parametrů při navigaci na stránce najdete v tématu [předání parametrů během navigační](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Při vytvoření a přešli, zobrazení `InitializeAsync` proveden metoda zobrazení přidruženého zobrazení modelu, který potom provede `Logout` metodu `LoginViewModel` třídy, která je znázorněno v následujícím příkladu kódu:

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

Tato metoda vyvolá `CreateLogoutRequest` metoda v `IdentityService` třídy a předejte identity token načíst z nastavení aplikace jako parametr. Další informace o nastavení aplikace najdete v tématu [Configuration Management](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). Následující příklad kódu ukazuje `CreateLogoutRequest` metoda:

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

Tato metoda vytvoří identifikátor URI, která je IdentityServer [ukončení koncový bod relace](https://identityserver4.readthedocs.io/en/release/endpoints/endsession.html#refendsession), s požadovanými parametry. Koncový bod end relace je v `/connect/endsession` na portu 5105 základní koncového bodu, které jsou zveřejněné jako uživatelské nastavení. Další informace o nastavení uživatele najdete v tématu [Configuration Management](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

Vrácený identifikátor URI je uložen v `LoginUrl` vlastnost `LoginViewModel` třídy. Když `IsLogin` vlastnost je `true`, [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) v `LoginView` je viditelná. `WebView` Vazby dat jeho [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) vlastnost, která má `LoginUrl` vlastnost `LoginViewModel` třídy a proto odešle požadavek odhlášení IdentityServer při `LoginUrl` je nastavena na Koncový bod relace na IdentityServer end. Když IdentityServer obdrží tuto žádost, za předpokladu, že uživatel je přihlášený, dojde k odhlášení položky. Ověřování je sledován pomocí souboru cookie spravuje middleware ověřování souborů cookie z ASP.NET Core. Proto při odhlášení IdentityServer odebere ověřovacího souboru cookie a odešle přesměrování odhlašovací post URI zpět do klienta.

V mobilní aplikaci [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) bude přesměrován na identifikátor URI přesměrování odhlašovací post. To `WebView` způsobí, že navigace `NavigateAsync` metoda v `LoginViewModel` třídy mají být provedeny, která je znázorněno v následujícím příkladu kódu:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

Tato metoda vymaže tokenu identity a tokenu přístupu z nastavení aplikace a nastaví `IsLogin` vlastnost `false`, který spustí [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) na `LoginView` stránky se neviditelná . Nakonec `LoginUrl` je nastavena URI z IdentityServer na [koncový bod autorizace](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), s požadovanými parametry, v rámci přípravy příštím uživatel spustí přihlášení.

Informace o navigaci na stránce najdete v tématu [navigační](~/xamarin-forms/enterprise-application-patterns/navigation.md). Informace o [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) navigační způsobí, že metoda modelu zobrazení provést, najdete v článku [vyvolání navigační pomocí chování](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Informace o nastavení aplikace najdete v tématu [Configuration Management](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> EShopOnContainers také umožňuje model odhlášení pokud aplikace je nakonfigurovaná pro použití imitované služby v firewallZobrazit. V tomto režimu aplikace nebude komunikovat s IdentityServer a místo toho vymaže všechny uložené tokeny z nastavení aplikace.

<a name="authorization" />

## <a name="authorization"></a>Autorizace

Po ověření ASP.NET Core webové rozhraní API často potřebují k autorizaci přístupu, která umožňuje službě aby rozhraní API, některé ověřeným uživatelům k dispozici, ale ne všechny.

Omezení přístupu k trasu ASP.NET MVC základní lze dosáhnout použitím atribut autorizovat do řadiče nebo akce, která omezuje přístup na kontroler nebo akce pro ověřené uživatele, jak je znázorněno v následujícím příkladu kódu:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Pokud neoprávněný uživatel pokusí o přístup k kontroler nebo akce, která je označena pomocí `Authorize` atribut rozhraní MVC vrátí 401 (Neautorizováno) stavový kód HTTP.

> [!NOTE]
> Parametry lze zadat u `Authorize` atribut omezit rozhraní API pro konkrétní uživatele. Další informace najdete v tématu [autorizace](/aspnet/core/security/authorization/introduction/).

IdentityServer můžete integrovat do pracovního postupu, autorizace, takže přístupových tokenů poskytuje řízení autorizace. Tento přístup je zobrazen v obrázek 9-5.

![](authentication-and-authorization-images/authorization.png "Autorizace pomocí přístupového tokenu")

**Obrázek 9 – 5:** autorizace pomocí přístupového tokenu

EShopOnContainers mobilní aplikace komunikuje s mikroslužbu identity a požadavky přístupový token jako součást procesu ověřování. Přístupový token je předán rozhraní API vystavené mikroslužeb řazení a košík v rámci žádosti o přístup. Přístupové tokeny obsahují informace o klientovi a uživatele. Rozhraní API pak pomocí těchto informací k autorizaci přístupu k jejich datům. Informace o tom, jak nakonfigurovat IdentityServer k ochraně rozhraní API najdete v tématu [konfigurace rozhraní API prostředky](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Konfigurace IdentityServer k autorizaci

K autorizaci s IdentityServer, musí být jeho middleware ověřování přidaný do kanálu požadavku HTTP webové aplikace. Middleware je přidaný do `ConfigureAuth` metoda ve webové aplikaci `Startup` třídy, který lze vyvolat pomocí `Configure` metoda a je ukázáno v následujícím příkladu kódu z eShopOnContainers referenční aplikace:

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

Tato metoda zajišťuje, že rozhraní API lze přistupovat pouze s tokenem platný přístup. Middleware ověří příchozí token k zajištění, že se odesílá z důvěryhodných vydavatelů a ověří, zda je token platný pro použití s rozhraním API, která přijímá ho. Procházení k řazení nebo košík řadič proto vrátí 401 (Neautorizováno) stavového kódu protokolu HTTP, označující, že je požadovaná přístupový token.

> [!NOTE]
> Middleware ověřování na IdentityServer musí být přidaný do kanálu požadavku HTTP webové aplikace před přidáním MVC s `app.UseMvc()` nebo `app.UseMvcWithDefaultRoute()`.

### <a name="making-access-requests-to-apis"></a>Provedení žádosti o přístup k rozhraní API

Při zasílání požadavků na řazení a košík mikroslužeb, přístup tokenu, získaný IdentityServer během procesu ověřování musí být součástí požadavku, jak je znázorněno v následujícím příkladu kódu:

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Přístupový token se ukládají jako nastavení aplikace a je načíst z úložiště specifické pro platformu a součástí volání `GetOrderAsync` metoda v `OrderService` třídy.

Podobně přístupový token musí být zahrnuty při odesílání dat do IdentityServer chráněné rozhraní API, jak je znázorněno v následujícím příkladu kódu:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Přístupový token je načtena z úložiště specifické pro platformu a součástí volání `UpdateBasketAsync` metoda v `BasketService` třídy.

`RequestProvider` Třída, v mobilní aplikaci eShopOnContainers používá `HttpClient` třída provádět požadavky na rozhraní RESTful API vystavené eShopOnContainers odkaz na aplikaci. Při provádění požadavků řazení a košík rozhraní API, které vyžadují ověřování, musí být součástí požadavku token platný přístup. Toho dosáhnete přidáním přístupový token do hlavičky `HttpClient` instance, jak je ukázáno v následujícím příkladu kódu:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`DefaultRequestHeaders` Vlastnost `HttpClient` třída zpřístupňuje hlavičky, které jsou odeslány s každým požadavkem a přístupový token se přidá `Authorization` záhlaví řetězec s předponou `Bearer`. Pokud je požadavek odeslán do rozhraní RESTful API, hodnota `Authorization` záhlaví je extrahována a ověřena k zajištění, že má odeslaný důvěryhodných vystavitelů a používá k určení, zda má uživatel oprávnění k volání rozhraní API, přijetí.

Další informace o způsobu eShopOnContainers mobilních aplikací umožňuje webových požadavků v tématu [přístup ke vzdáleným datům](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Souhrn

Existuje mnoho přístupů k integraci ověřování a autorizace do aplikace na platformě Xamarin.Forms, která komunikuje s webovou aplikaci ASP.NET MVC. Mobilní aplikace eShopOnContainers provádí ověřování a autorizaci s mikroslužbu kontejnerizované identity, která používá IdentityServer 4. IdentityServer je otevřeným zdrojem OpenID Connect a OAuth 2.0 framework pro ASP.NET Core, který se integruje s ASP.NET Identity Core k provedení ověřování tokenu nosiče.

Mobilní aplikace vyžaduje tokeny zabezpečení z IdentityServer, pro ověření uživatele nebo pro přístup k prostředku. Při přístupu k prostředku, musí být součástí požadavek na rozhraní API, které vyžadují autorizační token přístupu. Na IdentityServer middleware ověří příchozí tokeny přístupu k zajištění jejich odesláním z důvěryhodných vydavatelů, a že jsou platná pro použití s rozhraním API, která přijímá je.


## <a name="related-links"></a>Související odkazy

- [Stáhnout elektronická kniha (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (Githubu) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
