---
title: Přístup k rozhraní API grafu
description: Tento dokument popisuje, jak přidat do mobilních aplikací vytvořených pomocí Xamarinu ověřování Azure Active Directory.
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c43dfa79831f22e55490b27c3c360602ae717627
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781127"
---
# <a name="accessing-the-graph-api"></a>Přístup k rozhraní API grafu

Použijte následující postup použijte rozhraní Graph API z v aplikaci Xamarin:

1. [Probíhá registrace ve službě Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) na *windowsazure.com* portál, pak
2. [Konfigurace služby](~/cross-platform/data-cloud/active-directory/get-started/configure.md).

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>Krok 3. Přidání ověřování služby Active Directory do aplikace

V aplikaci přidejte odkaz na **Azure Active Directory Authentication Library (Azure ADAL)** pomocí Správce balíčků NuGet v sadě Visual Studio nebo Visual Studio for Mac.
Zkontrolujte, zda jste vybrali **zobrazit předběžné verze balíčků** zahrnout tohoto balíčku, protože je stále ve verzi preview.

> [!IMPORTANT]
> Poznámka: Azure ADAL 3.0 právě náhled a může být nejnovější změny před vydání konečné verze. 


![](graph-images/06.-adal-nuget-package.jpg "Přidat odkaz na Azure Active Directory Authentication Library (Azure ADAL)")

V aplikaci bude nyní musíte přidejte následující úrovně proměnné třídy, které jsou požadovány pro tok ověřování.

```csharp
//Client ID
public static string clientId = "25927d3c-.....-63f2304b90de";
public static string commonAuthority = "https://login.windows.net/common"
//Redirect URI
public static Uri returnUri = new Uri("http://xam-demo-redirect");
//Graph URI if you've given permission to Azure Active Directory
const string graphResourceUri = "https://graph.windows.net";
public static string graphApiVersion = "2013-11-08";
//AuthenticationResult will hold the result after authentication completes
AuthenticationResult authResult = null;
```

Poznámka, zde je `commonAuthority`. Pokud je koncový bod ověřování `common`, stane se aplikace **víceklientské**, což znamená, že každý uživatel, můžete použít přihlášení pomocí přihlašovacích údajů služby Active Directory. Po ověření tohoto uživatele bude fungovat v kontextu vlastní služby Active Directory – tj uvidí podrobnosti související s jeho služby Active Directory.

### <a name="write-method-to-acquire-access-token"></a>Write – metoda získat přístup k tokenu

Následující kód (pro Android) se spustil ověřování a po dokončení přiřadit výsledek v `authResult`. IOS a Windows Phone implementace mírně lišit: druhý parametr (`Activity`) se liší v systému iOS a chybí na Windows Phone.

```csharp
public static async Task<AuthenticationResult> GetAccessToken
            (string serviceResourceId, Activity activity)
{
    authContext = new AuthenticationContext(Authority);
    if (authContext.TokenCache.ReadItems().Count() > 0)
        authContext = new AuthenticationContext(authContext.TokenCache.ReadItems().First().Authority);
    var authResult = await authContext.AcquireTokenAsync(serviceResourceId, clientId, returnUri, new AuthorizationParameters(activity));
    return authResult;
}  
```

Ve výše uvedeném kódu `AuthenticationContext` zodpovídá za ověření s commonAuthority. Má `AcquireTokenAsync` metodu, která trvat parametry jako prostředek, který musí mít přístup, v takovém případě `graphResourceUri`, `clientId`, a `returnUri`. Aplikace se vrátí do `returnUri` po dokončení ověřování. Tento kód zůstane stejný pro všechny platformy, ale parametr poslední `AuthorizationParameters`, se budou lišit na různých platformách a je odpovědná za kterými se řídí tok ověřování.

V případě Android nebo iOS jsme předat `this` parametru `AuthorizationParameters(this)` sdílet kontext, zatímco v systému Windows je předáván bez zadání parametru jako nový `AuthorizationParameters()`.

### <a name="handle-continuation-for-android"></a>Pokračování popisovač pro Android

Po dokončení ověřování toku by měl vrátit do aplikace. V případě Android se zpracovává souborem následující kód, který má být přidán do **MainActivity.cs**:


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);

    
}
```

### <a name="handle-continuation-for-windows-phone"></a>Pokračování popisovač pro Windows Phone

Pro Windows Phone upravit `OnActivated` metoda v **App.xaml.cs** soubor s níže uvedeného kódu:

```csharp
protected override void OnActivated(IActivatedEventArgs args)
{
#if WINDOWS_PHONE_APP
  if (args is IWebAuthenticationBrokerContinuationEventArgs)
  {
     WebAuthenticationBrokerContinuationHelper.SetWebAuthenticationBrokerContinuationEventArgs(args as IWebAuthenticationBrokerContinuationEventArgs);
  }
#endif
  base.OnActivated(args);
}
```

Nyní když aplikaci spouštíte, měli byste vidět ověřovací dialog.
Po úspěšném ověření požádá vaše oprávnění pro přístup k prostředkům (v našem případě rozhraní Graph API):

![](graph-images/08.-authentication-flow.jpg "Po úspěšném ověření požádá vaše oprávnění pro přístup k prostředkům v našem případě rozhraní Graph API")

Pokud je ověření úspěšné a jste udělili oprávnění aplikace pro přístup k prostředkům, měli byste obdržet `AccessToken` a `RefreshToken` pole se seznamem v `authResult`. Tyto tokeny jsou nutné pro další volání rozhraní API a autorizace s Azure Active Directory na pozadí.

![](graph-images/07.-access-token-for-authentication.jpg "Tyto tokeny jsou nutné pro další volání rozhraní API a autorizace s Azure Active Directory na pozadí")

Následující kód například umožňuje získat seznam uživatelů ze služby Active Directory. Adresa URL webové rozhraní API můžete nahradit své webové rozhraní API, který je chráněn službou Azure AD.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

