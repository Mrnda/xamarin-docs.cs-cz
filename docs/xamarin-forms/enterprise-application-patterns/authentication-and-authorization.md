---
title: Ověřování a autorizace
description: Tato kapitola popisuje, jak aplikaci eShopOnContainers mobilní aplikace provádí ověřování a autorizace pro kontejnerizované mikroslužby.
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: beb9e8f351a1cecc6017a08345f7cfc5e207ba35
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996214"
---
# <a name="authentication-and-authorization"></a>Ověřování a autorizace

Ověřování je proces získání identifikace přihlašovací údaje, jako jsou jména a hesla od uživatele a ověření těchto přihlašovacích údajů, kteří autoritu. Pokud jsou pověření platná, entity, která je odeslána přihlašovací údaje se považuje za ověřená identita. Po ověření identity autorizačního procesu Určuje, zda tato identita má přístup k danému prostředku.

Existuje celá řada přístupů k integraci ověřování a autorizace v aplikaci Xamarin.Forms, která komunikuje s webovou aplikaci ASP.NET MVC, včetně použití zprostředkovatele externího ověřování jako je Microsoft, Google, ASP.NET Core Identity Middlewaru Facebook, nebo Twitter a ověřování. V aplikaci eShopOnContainers mobilní aplikaci provádí ověřování a autorizace pomocí identity kontejnerizované mikroslužby, který používá IdentityServer 4. Mobilní aplikace vyžaduje tokeny zabezpečení z IdentityServer, pro ověřování uživatele nebo pro přístup k prostředku. Pro IdentityServer problém tokeny jménem uživatele uživatel musíte se přihlásit k IdentityServer. Ale IdentityServer neposkytuje uživatelského rozhraní nebo databáze pro ověřování. V aplikaci eShopOnContainers odkaz na aplikaci, proto ASP.NET Core Identity se používá pro tento účel.

## <a name="authentication"></a>Ověřování

Ověřování je vyžadováno, pokud aplikace potřebuje znát identitu aktuálního uživatele. ASP.NET Core primární mechanismus pro identifikaci uživatelů je systém členství technologie ASP.NET Core Identity, který ukládá informace o uživateli v úložišti dat nakonfigurované vývojářem. Toto úložiště dat obvykle bude úložiště objektu EntityFramework, i když se balíčky třetích stran nebo vlastní úložiště lze použít k ukládání informací o identitě v Azure storage, Azure Cosmos DB nebo jiné umístění.

Scénáře ověřování služby, které využívají úložiště dat místního uživatele a které ukládají informace o identitě mezi požadavky pomocí souborů cookie (což je typické ve webových aplikacích ASP.NET MVC), ASP.NET Core Identity je vhodné řešení. Ale soubory cookie nejsou vždy přirozené prostředky uchování a přenášet data. Například webové aplikace ASP.NET Core, která zveřejňuje koncové body RESTful, které jsou přístupné z mobilní aplikace se obvykle třeba použít ověřování pomocí tokenu nosiče, protože soubory cookie nelze používat v tomto scénáři. Však nosné tokeny můžete snadno načíst a zahrnutý v hlavičce autorizace webové žádosti z mobilní aplikace.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Vydávání nosné tokeny pomocí IdentityServer 4

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) je open source platforma OpenID Connect a OAuth 2.0 pro ASP.NET Core, který lze použít pro řadu scénářů ověřování a autorizaci, včetně vystavování tokenů zabezpečení pro místní uživatele ASP.NET Core Identity.

> [!NOTE]
> OpenID Connect a OAuth 2.0 jsou velmi podobné, při mají rozdílné povinnosti.

OpenID Connect se vrstvu ověřování přes protokol OAuth 2.0. OAuth 2 je protokol, který umožňuje aplikacím požádat o přístupové tokeny od služby tokenů zabezpečení a použít je ke komunikaci s rozhraními API. Toto delegování snižuje složitost v klientské aplikace a rozhraní API, protože může být centralizované ověřování a autorizace.

Kombinace OpenID Connect a OAuth 2.0 sloučit dva základní bezpečnostní otázky o ověřování a přístup k rozhraní API a IdentityServer 4 je implementace těchto protokolů.

V aplikacích používajících přímou komunikaci klienta mikroslužeb, jako jsou například aplikace odkaz na aplikaci eShopOnContainers vyhrazené ověřování mikroslužeb, který funguje jako Token služby zabezpečení (STS) slouží k ověřování uživatelů, jak je znázorněno na obrázku 9-1. Další informace o přímé komunikaci klienta mikroslužbách najdete v tématu [komunikace mezi klientem a Mikroslužby](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices).

![](authentication-and-authorization-images/authentication.png "Ověřování pomocí vyhrazené ověřování mikroslužby")

**Obrázek 9-1:** ověřování pomocí vyhrazené ověřování mikroslužby

Aplikaci eShopOnContainers mobilní aplikace komunikuje se službou identit mikroslužeb, která používá IdentityServer 4 k provedení ověřování a řízení přístupu pro rozhraní API. Mobilní aplikace požaduje proto tokeny z IdentityServer, pro ověřování uživatele nebo pro přístup k prostředku:

-   Ověřování uživatelů pomocí IdentityServer se dosahuje prostřednictvím mobilní aplikace požaduje *identity* token, který představuje výsledek ověřovacího procesu. Minimálně obsahuje identifikátor pro uživatele a informace o tom, jak a kdy ověření uživatele. Může také obsahovat data další identity.
-   Přístup k prostředku s IdentityServer se dosahuje prostřednictvím mobilní aplikace požaduje *přístup* token, který umožňuje přístup k prostředku rozhraní API. Klienti požádat o přístupové tokeny, které předávají do rozhraní API. Přístupové tokeny obsahují informace o klientovi a uživatele (pokud existuje). Rozhraní API pak pomocí těchto informací můžete autorizovat přístup ke svým datům.

> [!NOTE]
> Klient musí zaregistrovat IdentityServer může požádat o tokeny.

### <a name="adding-identityserver-to-a-web-application"></a>Přidání IdentityServer do webové aplikace

V pořadí pro webovou aplikaci ASP.NET Core používat IdentityServer 4 je nutné přidat do řešení webové aplikace Visual Studio. Další informace najdete v tématu [instalační program a přehled](https://identityserver4.readthedocs.io/en/release/quickstarts/0_overview.html) v dokumentaci k IdentityServer.

Jakmile IdentityServer je součástí řešení sady Visual Studio webové aplikace, musí být přidána zpracování kanálu žádostí protokolu HTTP webové aplikace, tak, aby mohl obsluhovat požadavky na koncové body OpenID Connect a OAuth 2.0. Toho můžete dosáhnout v `Configure` metoda ve webové aplikaci `Startup` třídy, jak je ukázáno v následujícím příkladu kódu:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Pořadí je důležité v kanálu zpracování požadavku HTTP webové aplikace. IdentityServer musí proto přidat do kanálu před architekturu uživatelského rozhraní, která implementuje přihlašovací obrazovky.

### <a name="configuring-identityserver"></a>Konfigurace IdentityServer

IdentityServer by měl být nakonfigurovaný v `ConfigureServices` metoda ve webové aplikaci `Startup` třídy voláním `services.AddIdentityServer` způsob, jak je ukázáno v následujícím příkladu kódu v aplikaci eShopOnContainers referenční aplikace:

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

Po volání `services.AddIdentityServer` metoda, se nazývají další rozhraní fluent API můžete nakonfigurovat tyto:

-   Přihlašovací údaje používané k podepisování.
-   Rozhraní API a identity prostředky, které uživatelé mohou požadovat při přístupu k.
-   Klienti, kteří se připojují k žádosti o tokeny.
-   ASP.NET Core Identity.

>💡 **Tip**: dynamicky načíst konfiguraci IdentityServer 4. Rozhraní API IdentityServer 4 umožňují nastavit IdentityServer ze seznamu objektů konfigurace v paměti. V aplikaci eShopOnContainers odkaz na aplikaci jsou tyto kolekce v paměti pevně zakódovaný do aplikace. Nicméně v produkčních scénářích je možné načíst dynamicky z konfiguračního souboru nebo z databáze.

Informace o konfiguraci IdentityServer používat ASP.NET Core Identity najdete v tématu [pomocí ASP.NET Core Identity](https://identityserver4.readthedocs.io/en/release/quickstarts/6_aspnet_identity.html) v dokumentaci k IdentityServer.

#### <a name="configuring-api-resources"></a>Konfigurace prostředků rozhraní API

Při konfiguraci prostředků rozhraní API `AddInMemoryApiResources` metoda očekává `IEnumerable<ApiResource>` kolekce. Následující příklad kódu ukazuje `GetApis` odkazovat na metodu, která poskytuje tuto kolekci na aplikaci eShopOnContainers aplikace:

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

Tato metoda určuje, že by měl IdentityServer chránit objednávky a košík rozhraní API. Proto IdentityServer spravovaný přístup tokeny se bude vyžadovat při volání těchto rozhraní API. Další informace o `ApiResource` zadejte naleznete v tématu [prostředku rozhraní API](https://identityserver4.readthedocs.io/en/release/reference/api_resource.html#refapiresource) v dokumentaci k IdentityServer 4.

#### <a name="configuring-identity-resources"></a>Konfigurace Identity prostředků

Při konfiguraci identity prostředky `AddInMemoryIdentityResources` metoda očekává `IEnumerable<IdentityResource>` kolekce. Identita prostředky jsou data, například ID uživatele, název nebo e-mailovou adresu. Každý prostředek identita má jedinečný název a typy deklarací identity libovolné je možné přiřadit k ní, které budou zahrnuty v tokenu identity pro uživatele. Následující příklad kódu ukazuje `GetResources` odkazovat na metodu, která poskytuje tuto kolekci na aplikaci eShopOnContainers aplikace:

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

Specifikace OpenID Connect určuje některé [standardní identity prostředky](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims). Požadavek na minimální je, že podpora se poskytuje pro generování jedinečné ID pro uživatele. Toho můžete dosáhnout zveřejněním `IdentityResources.OpenId` identity prostředku.

> [!NOTE]
> `IdentityResources` Třídy podporuje všechny obory definované ve specifikaci OpenID Connect (openid, e-mailu, profil, Telefon a adresa).

IdentityServer podporuje také definovat vlastní identitu zdroje. Další informace najdete v tématu [definování vlastní identitu zdroje](https://identityserver4.readthedocs.io/en/release/topics/resources.html#defining-custom-identity-resources) v dokumentaci k IdentityServer. Další informace o `IdentityResource` zadejte naleznete v tématu [Identity prostředku](https://identityserver4.readthedocs.io/en/release/reference/identity_resource.html) v dokumentaci k IdentityServer 4.

#### <a name="configuring-clients"></a>Konfigurace klientů

Klienti jsou aplikace, které můžete požádat o tokeny od IdentityServer. Následující nastavení musí být obvykle definovány pro každého klienta minimálně:

-   ID klienta jedinečný.
-   Povolené interakce s služby tokenů (označované jako typ udělení oprávnění).
-   Umístění, kam se posílají tokenů identit a přístupu k (označuje se jako identifikátor URI pro přesměrování).
-   Seznam prostředků, které klient je povolený přístup k (označuje se jako obory).

Při konfiguraci klientů, `AddInMemoryClients` metoda očekává `IEnumerable<Client>` kolekce. Následující příklad kódu znázorňuje konfiguraci pro aplikaci eShopOnContainers mobilní aplikace `GetClients` odkazovat na metodu, která poskytuje tuto kolekci na aplikaci eShopOnContainers aplikace:

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
-   `ClientName`: Klient zobrazovaný název, který se používá pro protokolování a obrazovkami pro vyjádření souhlasu.
-   `AllowedGrantTypes`: Určuje, jak chce pracovat s IdentityServer klienta. Další informace najdete v části [konfigurace tok ověřování](#configuring_the_authentication_flow).
-   `ClientSecrets`: Určuje tajných kódů pověření klienta, která se používají při žádosti o tokeny od koncového bodu tokenu.
-   `RedirectUris`: Určuje povolené identifikátory URI, pro které chcete vrátit tokeny nebo autorizační kódy.
-   `RequireConsent`: Určuje, zda je požadován obrazovce pro vyjádření souhlasu.
-   `RequirePkce`: Určuje, zda klienti, kteří používají autorizační kód musíte odeslat klíč důkazu.
-   `PostLogoutRedirectUris`: Určuje povolené identifikátory URI pro přesměrování po odhlášení.
-   `AllowedCorsOrigins`: Určuje původ klienta tak, aby IdentityServer můžete povolit volání mezi zdroji z původního zdroje.
-   `AllowedScopes`: Určuje prostředky, které má klient přístup. Ve výchozím nastavení klient nemá přístup k žádným prostředkům.
-   `AllowOfflineAccess`: Určuje, jestli může klient požadovat obnovovací tokeny.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>Konfigurace tok ověřování

Tok ověřování mezi klientem a IdentityServer lze nakonfigurovat tak, že zadáte typy udělení v `Client.AllowedGrantTypes` vlastnost. Specifikace OpenID Connect a OAuth 2.0 definovat všechny toky ověřování, včetně:

-   Implicitní. Tento tok je optimalizovaný pro aplikace založené na prohlížeči, by měla sloužit pro uživatele jenom pro ověřování nebo ověřování a přístup k žádosti o tokeny. Všechny tokeny jsou přenášena přes prohlížeč a proto pokročilých funkcí, jako jsou tokeny obnovení nejsou povolené.
-   Autorizační kód. Tento tok umožňuje načítání tokenů v používající back channel, na rozdíl od front-kanál prohlížeče současně také podporuje ověřování klientů.
-   Hybridní. Tento tok je kombinací implicitní a typy udělení autorizačního kódu. Identity token se přenášejí prostřednictvím kanálu prohlížeče a obsahuje odpověď podepsaný protokolu společně s další součásti, jako jsou autorizační kód. Po úspěšném ověření odpovědi by měla sloužit k načtení přístup a obnovovací token používající back channel.

> [!TIP]
> Tok ověřování hybridní použijte. Tok ověřování hybridní snižuje počet útoků, které se vztahují na kanál prohlížeče a představuje doporučené kroky pro nativní aplikace, které chcete načíst tokeny přístupu (a případně obnovovacích tokenů).

Další informace o toky ověřování najdete v tématu [typy udělení](https://identityserver4.readthedocs.io/en/release/topics/grant_types.html) v dokumentaci k IdentityServer 4.

### <a name="performing-authentication"></a>Provádí se ověřování

Pro IdentityServer problém tokeny jménem uživatele uživatel musíte se přihlásit k IdentityServer. Ale IdentityServer neposkytuje uživatelského rozhraní nebo databáze pro ověřování. V aplikaci eShopOnContainers odkaz na aplikaci, proto ASP.NET Core Identity se používá pro tento účel.

Mobilní aplikace aplikaci eShopOnContainers se ověří pomocí IdentityServer s hybridní tok ověřování, který je znázorněný v obrázku 9-2.

![](authentication-and-authorization-images/sign-in.png "Přehled procesu přihlášení")

**Obrázek 9 – 2:** podrobný přehled procesu přihlášení

Provedení požadavku na přihlášení k `<base endpoint>:5105/connect/authorize`. Po úspěšném ověření IdentityServer vrátí odpověď ověřování, který obsahuje autorizační kód a identity token. Autorizační kód se pak posílají do `<base endpoint>:5105/connect/token`, který odpoví přístup, identity a tokeny obnovení.

Aplikaci eShopOnContainers mobilní aplikace příznaky mimo IdentityServer odesláním požadavku do `<base endpoint>:5105/connect/endsession`, s další parametry. Po odhlášení, odpoví IdentityServer odesláním přesměrování po odhlášení URI zpět do mobilní aplikace. Tento proces je znázorněný na obrázku 9-3.

![](authentication-and-authorization-images/sign-out.png "Přehled procesu odhlášení")

**Obrázek 9-3:** podrobný přehled proces přihlášení

V aplikaci eShopOnContainers mobilní aplikaci, komunikace s IdentityServer provádí `IdentityService` třídy, která implementuje `IIdentityService` rozhraní. Toto rozhraní určuje, že implementující třída musí poskytovat `CreateAuthorizationRequest`, `CreateLogoutRequest`, a `GetTokenAsync` metody.

#### <a name="signing-in"></a>Přihlášení

Když uživatel klepne **přihlášení** tlačítko `LoginView`, `SignInCommand` v `LoginViewModel` třída provádí, která pak spustí `SignInAsync` metoda. Následující příklad kódu ukazuje tuto metodu:

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Tato metoda vyvolá `CreateAuthorizationRequest` metodu `IdentityService` třída, která je znázorněna v následujícím příkladu kódu:

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

Tato metoda vytvoří identifikátor URI pro jeho IdentityServer [koncový bod autorizace](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), s požadovanými parametry. Koncový bod autorizace je na `/connect/authorize` na portu 5105 základní koncový bod vystavený jako nastavení uživatele. Další informace o nastavení uživatele, najdete v části [Configuration Management](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Útok v aplikaci eShopOnContainers mobilní aplikaci se sníží implementace ověření klíče pro kód Exchange (PKCE) rozšíření OAuth. PKCE chrání autorizační kód z používán, pokud je zachytit. Toho dosáhnete pomocí klienta, generuje se tajný verifier hodnotu hash je předáno žádost o autorizaci, a který se zobrazí nezašifrované při uplatňování autorizační kód. Další informace o PKCE, naleznete v tématu [testování klíč pro výměnu kódu ve veřejných klientů OAuth](https://tools.ietf.org/html/rfc7636) na webové stránce Internet Engineering Task Force.

Vrácený identifikátor URI je uložená v `LoginUrl` vlastnost `LoginViewModel` třídy. Když `IsLogin` vlastnost se stane `true`, [ `WebView` ](xref:Xamarin.Forms.WebView) v `LoginView` stane viditelnou. `WebView` Vazby dat jeho [ `Source` ](xref:Xamarin.Forms.WebView.Source) vlastnost `LoginUrl` vlastnost `LoginViewModel` třídy a tak zajišťuje žádost o přihlášení IdentityServer při `LoginUrl` je nastavena na Koncový bod autorizace IdentityServer společnosti. Když IdentityServer obdrží tuto žádost a uživatel není ověřen, `WebView` budete přesměrováni na nakonfigurované přihlašovací stránku, která je znázorněna na obrázku 9-4.

![](authentication-and-authorization-images/login.png "Přihlašovací stránka zobrazí ve webové zobrazení")

**Obrázek 9-4:** přihlašovací stránky zobrazí ve webové zobrazení

Po dokončení přihlášení [ `WebView` ](xref:Xamarin.Forms.WebView) budete přesměrováni na návratové identifikátoru URI. To `WebView` způsobí, že navigace `NavigateAsync` metodu `LoginViewModel` třída má být spuštěna, která je znázorněna v následujícím příkladu kódu:

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

Tato metoda analyzuje odpověď ověřování, která je součástí návratový identifikátor URI a za předpokladu, že platný autorizační kód je k dispozici, odešle požadavek do vaší IdentityServer [koncový bod tokenu](https://identityserver4.readthedocs.io/en/release/endpoints/token.html), předávání autorizační kód Ověřovače PKCE tajného kódu a další požadované parametry. Koncový bod tokenu je v `/connect/token` na portu 5105 základní koncový bod vystavený jako nastavení uživatele. Další informace o nastavení uživatele, najdete v části [Configuration Management](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

>💡 **Tip**: ověření vrátit identifikátorů URI. I když v aplikaci eShopOnContainers mobilní aplikaci neověřuje návratový identifikátor URI, osvědčeným postupem je k ověření, že vrácená identifikátor URI odkazuje do vhodného umístění, aby se zabránilo útoků založených na open přesměrování.

Pokud koncový bod tokenu obdrží platný autorizační kód a tajného kódu verifier PKCE, odpoví přístupový token, identity token a token obnovení. Přístupový token (který umožňuje přístup k prostředkům rozhraní API) a identity token jsou pak uloženy jako nastavení aplikace a navigaci na stránce se provádí. Celkový efekt v aplikaci eShopOnContainers mobilní aplikaci proto je to: za předpokladu, že uživatelé budou moct úspěšně ověří IdentityServer, jsou přejde k `MainView` stránku, která je [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) který se zobrazí `CatalogView` jako jeho vybraná karta.

Informace o navigaci stránkami najdete v tématu [navigace](~/xamarin-forms/enterprise-application-patterns/navigation.md). Informace o tom, jak [ `WebView` ](xref:Xamarin.Forms.WebView) navigační způsobí, že metoda model zobrazení provést, najdete v tématu [vyvolání navigace pomocí chování](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Informace o nastavení aplikace najdete v tématu [Configuration Management](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Aplikaci eShopOnContainers také umožňuje mock přihlášení, když aplikace je nakonfigurován na použití mock služeb v `SettingsView`. V tomto režimu nebude aplikace komunikovat s IdentityServer, místo toho umožnit uživateli přihlášení pomocí přihlašovacích údajů pro všechny.

#### <a name="signing-out"></a>Podepisování navýšením kapacity

Když uživatel klepne **ODHLÁSIT** tlačítko `ProfileView`, `LogoutCommand` v `ProfileViewModel` třída provádí, která pak spustí `LogoutAsync` metoda. Tato metoda provádí navigaci na stránce pro `LoginView` stránky, předávání `LogoutParameter` instance nastavenou na `true` jako parametr. Další informace o předávání parametrů během navigace stránky najdete v tématu [předání parametrů během navigace](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Při zobrazení je vytvořen a přejde poté, `InitializeAsync` provedení metody zobrazení přidruženého zobrazení modelu, který potom provede `Logout` metodu `LoginViewModel` třída, která je znázorněna v následujícím příkladu kódu:

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

Tato metoda vyvolá `CreateLogoutRequest` metodu `IdentityService` třídy, prochází identity token načíst z nastavení aplikace jako parametr. Další informace o nastavení aplikace najdete v tématu [Configuration Management](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). Následující příklad kódu ukazuje `CreateLogoutRequest` metody:

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

Tato metoda vytvoří identifikátor URI pro IdentityServer [ukončit koncový bod relace](https://identityserver4.readthedocs.io/en/release/endpoints/endsession.html#refendsession), s požadovanými parametry. Koncového bodu end relace je v `/connect/endsession` na portu 5105 základní koncový bod vystavený jako nastavení uživatele. Další informace o nastavení uživatele, najdete v části [Configuration Management](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

Vrácený identifikátor URI je uložená v `LoginUrl` vlastnost `LoginViewModel` třídy. Zatímco `IsLogin` vlastnost `true`, [ `WebView` ](xref:Xamarin.Forms.WebView) v `LoginView` je viditelný. `WebView` Vazby dat jeho [ `Source` ](xref:Xamarin.Forms.WebView.Source) vlastnost `LoginUrl` vlastnost `LoginViewModel` třídy a tak zajišťuje žádost o odhlášení IdentityServer při `LoginUrl` je nastavena na Koncový bod relace end IdentityServer společnosti. Když IdentityServer obdrží tuto žádost, za předpokladu, že je uživatel přihlášený, dojde k odhlášení položky. Ověřování se sleduje pomocí souboru cookie spravuje middleware ověřování souborů cookie v ASP.NET Core. Proto odhlášení z IdentityServer odebere ověřovacího souboru cookie a přesměrování po odhlášení, identifikátor URI zpátky do klienta odesílá.

V mobilní aplikaci [ `WebView` ](xref:Xamarin.Forms.WebView) budete přesměrováni na identifikátor URI přesměrování po odhlášení. To `WebView` způsobí, že navigace `NavigateAsync` metodu `LoginViewModel` třída má být spuštěna, která je znázorněna v následujícím příkladu kódu:

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

Tato metoda vymaže identity token a přístupového tokenu z nastavení aplikace a nastaví `IsLogin` vlastnost `false`, které způsobí, že [ `WebView` ](xref:Xamarin.Forms.WebView) na `LoginView` stránka stane neviditelnou . Nakonec `LoginUrl` je nastavena identifikátor URI z IdentityServer společnosti [koncový bod autorizace](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), s požadovanými parametry při přípravě na dalším uživatel zahájí u přihlášení.

Informace o navigaci stránkami najdete v tématu [navigace](~/xamarin-forms/enterprise-application-patterns/navigation.md). Informace o tom, jak [ `WebView` ](xref:Xamarin.Forms.WebView) navigační způsobí, že metoda model zobrazení provést, najdete v tématu [vyvolání navigace pomocí chování](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Informace o nastavení aplikace najdete v tématu [Configuration Management](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Aplikaci eShopOnContainers také umožňuje model odhlášení při aplikace podle konfigurace používané firewallZobrazit mock služby. V tomto režimu aplikace nebude komunikovat s IdentityServer a místo toho vymaže všechny uložené tokeny z nastavení aplikace.

<a name="authorization" />

## <a name="authorization"></a>Autorizace

Po ověření, ASP.NET Core webové rozhraní API se často potřebují k autorizaci přístupu, který umožňuje službu. Ujistěte se, rozhraní API dostupné pro některé ověřeného uživatele, ale ne pro všechny.

Omezení přístupu ASP.NET Core MVC směrovací lze dosáhnout použitím atributu Authorize na kontroler nebo akce, která omezuje přístup na kontroler nebo akce pro ověřeného uživatele, jak je znázorněno v následujícím příkladu kódu:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Pokud neoprávněný uživatel pokusí o přístup k kontroler nebo akce, která je označena `Authorize` atribut, rozhraní MVC vrátí 401 (Neautorizováno) stavový kód HTTP.

> [!NOTE]
> Parametry můžete zadat na `Authorize` atribut k omezení rozhraní API pro konkrétní uživatele. Další informace najdete v tématu [autorizace](/aspnet/core/security/authorization/introduction/).

IdentityServer je možné integrovat do pracovní postup autorizace tak, aby přístupové tokeny poskytuje ovládací prvek autorizace. Tento přístup se zobrazí obrázek 9 – 5.

![](authentication-and-authorization-images/authorization.png "Ověření pomocí přístupového tokenu")

**Obrázek 9-5:** ověření pomocí přístupového tokenu

Aplikaci eShopOnContainers mobilní aplikace komunikuje se službou identit mikroslužeb a vyžádá přístupový token jako součást procesu ověřování. Přístupový token je předán rozhraní API podle řazení a nákupní košík mikroslužeb jako část žádosti o přístup. Přístupové tokeny obsahují informace o klientovi a uživatele. Rozhraní API pak pomocí těchto informací můžete autorizovat přístup ke svým datům. Informace o tom, jak nakonfigurovat IdentityServer ochrana rozhraní API najdete v tématu [konfigurace prostředkům API](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Konfigurace IdentityServer k autorizaci

K autorizaci s IdentityServer, musí jeho middleware ověřování přidat do kanál požadavků HTTP pro webovou aplikaci. Přidá middleware `ConfigureAuth` metoda ve webové aplikaci `Startup` třídu, která je vyvolána z `Configure` metoda a je znázorněn v následujícím příkladu kódu v aplikaci eShopOnContainers referenční aplikace:

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

Tato metoda zajišťuje, že rozhraní API je přístupný pouze s platným přístupovým tokenem. Middleware ověřuje příchozí tokeny k zajištění, že je odeslán z důvěryhodného vystavitele a ověří, že je token platný pro použití s rozhraním API, které obdrží. Procházení k řadiči řazení nebo nákupní košík proto vrátí 401 (Neautorizováno) stavový kód HTTP, označující, že přístupový token je povinný.

> [!NOTE]
> Middleware ověřování IdentityServer společnosti musí být přidané do kanálu požadavku protokolu HTTP webové aplikace před přidáním MVC s `app.UseMvc()` nebo `app.UseMvcWithDefaultRoute()`.

### <a name="making-access-requests-to-apis"></a>Vytváření žádosti o přístup k rozhraní API

Při zasílání požadavků na mikroslužby řazení a nákupní košík, přístup k tokenu, získané z IdentityServer během procesu ověřování musí být součástí žádosti, jak je znázorněno v následujícím příkladu kódu:

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Přístupový token se ukládá jako nastavení aplikace a je načten z úložiště specifické pro platformu a součástí volání `GetOrderAsync` metodu `OrderService` třídy.

Obdobně přístupový token musí být zahrnuty při odesílání dat do IdentityServer chráněné rozhraní API, jak je znázorněno v následujícím příkladu kódu:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Přístupový token je načten z úložiště specifické pro platformu a součástí volání `UpdateBasketAsync` metodu `BasketService` třídy.

`RequestProvider` Třídy, v aplikaci eShopOnContainers mobilní aplikaci, používá `HttpClient` třídy tak, aby žádosti vystavené aplikace odkaz na aplikaci eShopOnContainers rozhraní RESTful API. Při posílání požadavků na řazení a nákupní košík rozhraní API, které vyžadují autorizace, musí být platným přístupovým tokenem součástí požadavku. Toho dosáhnete přidáním přístupového tokenu do hlavičky `HttpClient` instance, jak je ukázáno v následujícím příkladu kódu:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`DefaultRequestHeaders` Vlastnost `HttpClient` třída zveřejňuje hlavičky, které se odesílají s každou žádostí a přístupový token se přidá do `Authorization` záhlaví s předponou řetězec `Bearer`. Odeslání požadavku na rozhraní RESTful API, hodnota `Authorization` záhlaví se extrahují a ověřena k zajištění, že se odeslal z důvěryhodných vystavitelů a používá k určení, zda má uživatel oprávnění k volání rozhraní API, která obdrží.

Další informace o jak aplikaci eShopOnContainers mobilní aplikace provede webové požadavky najdete v tématu [přístup ke vzdáleným datům](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Souhrn

Existuje celá řada přístupů k integraci ověřování a autorizace v aplikaci Xamarin.Forms, která komunikuje s webovou aplikaci ASP.NET MVC. V aplikaci eShopOnContainers mobilní aplikaci provádí ověřování a autorizace pomocí identity kontejnerizované mikroslužby, který používá IdentityServer 4. IdentityServer je open source platforma OpenID Connect a OAuth 2.0 pro ASP.NET Core, která se integruje s ASP.NET Core Identity provádět ověřování tokenu nosiče.

Mobilní aplikace vyžaduje tokeny zabezpečení z IdentityServer, pro ověřování uživatele nebo pro přístup k prostředku. Při přístupu k prostředku, musí být součástí požadavek na rozhraní API, které vyžadují autorizace přístupový token. Middleware pro IdentityServer ověřuje příchozí tokeny přístupu k zajištění, že jsou odesílány z důvěryhodného vystavitele a že jsou platné pro použití s rozhraním API, která přijímá, je.


## <a name="related-links"></a>Související odkazy

- [Stáhněte si elektronickou knihu (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [aplikaci eShopOnContainers (GitHub) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
