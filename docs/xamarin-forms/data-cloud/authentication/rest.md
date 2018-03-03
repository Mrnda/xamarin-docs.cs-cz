---
title: "Ověřování RESTful webová služba"
description: "HTTP podporuje několik ověřovací mechanismy pro řízení přístupu k prostředkům. Základní ověřování poskytuje přístup k prostředkům jenom klienty, kteří mají správné přihlašovací údaje. Tento článek ukazuje, jak používat základní ověřování k ochraně přístupu k prostředkům RESTful webová služba."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 7aea74f95e8738cc415eaac3a5ac4f86b069d0f7
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="authenticating-a-restful-web-service"></a>Ověřování RESTful webová služba

_HTTP podporuje několik ověřovací mechanismy pro řízení přístupu k prostředkům. Základní ověřování poskytuje přístup k prostředkům jenom klienty, kteří mají správné přihlašovací údaje. Tento článek ukazuje, jak používat základní ověřování k ochraně přístupu k prostředkům RESTful webová služba._

Doprovodné ukázkovou aplikaci Xamarin.Forms využívá službu REST hostované Xamarin, která poskytuje přístup jen pro čtení k webové službě. Proto operace, které vytvářet, aktualizovat a odstraňovat data nemění data využívat v aplikaci. Však je k dispozici v IIS s možnostmi hostitele verzi služby REST *TodoRESTService* existuje můžete najít složku v ukázkové aplikaci a pokyny k nastavení služby. Tato verze IIS s možnostmi hostitele služby REST poskytuje úplné vytvoření, aktualizace, čtení a odstranění přístupu k datům.

> [!NOTE]
> Zabezpečení přenosu aplikace (ATS) vynucuje v iOS 9 a vyšší, zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikace, a tím prevence náhodného zpřístupnění citlivých informací. Protože ATS je povolené ve výchozím nastavení v aplikacích pro iOS 9, všechna připojení budou platit ATS požadavky na zabezpečení. Pokud připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.
> ATS můžete vyjádřit výslovný souhlas mimo Pokud není možné použít `HTTPS` protokolu a zabezpečení komunikace pro prostředků z Internetu. Toho lze dosáhnout aktualizací aplikace **Info.plist** souboru. Další informace najdete v části [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md).

## <a name="authenticating-users-over-http"></a>Ověřování uživatelů pomocí protokolu HTTP

Základní ověřování je nejjednodušší ověřovací mechanismus, který podporuje protokol HTTP a zahrnuje klient pro odeslání uživatelského jména a hesla jako text nezašifrované kódováním base64. Funguje takto:

- Pokud webová služba přijme požadavek na chráněný prostředek, odmítne žádost s kódem stavu HTTP 401 (odepření přístupu) a nastaví hlavičku WWW-Authenticate odpovědi, jak je znázorněno v následujícím diagramu:

![](rest-images/basic-authentication-fail.png "Selhání základní ověřování")

- Pokud webová služba přijme požadavek pro chráněný prostředek, se `Authorization` záhlaví správně nastavit, odpoví služby web s kódem stavu HTTP 200, které označuje, že požadavek byl úspěšný a že požadované informace se v odpovědi. Tento scénář je zobrazen v následujícím diagramu:

![](rest-images/basic-authentication-success.png "Základní ověřování úspěšné")

> [!NOTE]
> Základní ověřování lze používat pouze přes připojení HTTPS. Při použití připojení HTTP, <code>Authorization</code> záhlaví lze snadno dekódovat pokud útočník zachycenou přenos HTTP.

## <a name="specifying-basic-authentication-in-a-web-request"></a>Určení základní ověřování v požadavku webu

Použití základního ověřování je určena následujícím způsobem:

1. Řetězec "Základní" je přidán do `Authorization` hlavičky žádosti.
1. Uživatelské jméno a heslo jsou sloučeny do řetězec ve formátu "řetězec", který je pak base64 kódování a přidat do `Authorization` hlavičky žádosti.

Proto s 'XamarinUser' uživatelské jméno a heslo, XamarinPassword' záhlaví se změní na:

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

`HttpClient` Třídy můžete nastavit `Authorization` hodnota hlavičky na `HttpClient.DefaultRequestHeaders.Authorization` vlastnost. Protože `HttpClient` existuje instance pro víc požadavků `Authorization` záhlaví potřebuje pouze jednou, nastavit místo při provádění každého požadavku, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    var authData = string.Format ("{0}:{1}", Constants.Username, Constants.Password);
    var authHeaderValue = Convert.ToBase64String (Encoding.UTF8.GetBytes (authData));

    client = new HttpClient ();
    ...
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue ("Basic", authHeaderValue);
  }
  ...
}
```

Potom po odeslání žádosti s operací webové služby je podepsán požadavek `Authorization` záhlaví, která určuje, zda má uživatel oprávnění k vyvolání operace.

> [!NOTE]
> Když službu REST ukázka ukládá přihlašovací údaje jako konstanty, by neměly být uloženy ve formátu nezabezpečené v publikované aplikace. [Xamarith.Auth](https://www.nuget.org/packages/Xamarin.Auth/) NuGet poskytuje funkce týkající se bezpečného ukládání přihlašovacích údajů. Další informace najdete v části [ukládání a načítání informací o účtu na zařízení](~/xamarin-forms/data-cloud/authentication/oauth.md).


## <a name="processing-the-authorization-header-server-side"></a>Zpracování na straně serveru hlavičku autorizace

Službu REST doprovodné ukázka upraví každou akci s `[BasicAuthentication]` atribut. Tento atribut je implementováno modulem `BasicAuthenticationAttribute` třídy v řešení a slouží k analýze `Authorization` záhlaví a určení, zda jsou pověření kódováním base64 platný jeho porovnáním hodnot uložených v *Web.config*. Tento přístup je vhodný pro službu ukázkové, vyžaduje rozšíření pro veřejné webové služby.

V modulu Základní ověřování používaný službou IIS jsou uživatelé ověřování proti přihlašovacích údajů Windows. Proto musí uživatelé mají účty v doméně serveru. Základní ověřování modelu lze však nastavit povolit vlastní ověřování, uživatelské účty, kde se ověřují vůči externího zdroje, například do databáze. Další informace najdete v části [základní ověřování v rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/security/basic-authentication) na webu technologie ASP.NET.

> [!NOTE]
> Základní ověřování není navržená ke správě odhlašuje. Standardní základní ověřování přístupu pro protokolování tedy k ukončení relace.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak přidat základní ověřování do webových požadavků, které jsou vytvářeny pomocí aplikace Xamarin.Forms `HttpClient` třídy. Základní ověřování poskytuje přístup k prostředkům jenom klienty, kteří mají správné přihlašovací údaje. Informace o tom, jak používat [Xamarin.Auth](https://www.nuget.org/packages/Xamarin.Auth/) tak, aby uživatelé mohli sdílet back-end přitom jenom má přístup k jejich datům, spravovat proces ověřování v aplikaci Xamarin.Forms, najdete v části [ověřování uživatelů pomocí zprostředkovatele Identity](~/xamarin-forms/data-cloud/authentication/oauth.md).


## <a name="related-links"></a>Související odkazy

- [TodoREST (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [Využívání RESTful webová služba](~/xamarin-forms/data-cloud/consuming/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
