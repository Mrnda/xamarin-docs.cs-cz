---
title: Překlad text použití překladač rozhraní API
description: Rozhraní API služby Microsoft překladač slouží k převodu řeč a textu přes REST API. Tento článek vysvětluje, jak používat rozhraní API služby Microsoft překladač Text k převodu textu z jednoho jazyka do druhého v aplikaci Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 5c1001335fb030f9a91ec72456042316864ccf5c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="text-translation-using-the-translator-api"></a>Překlad text použití překladač rozhraní API

_Rozhraní API služby Microsoft překladač slouží k převodu řeč a textu přes REST API. Tento článek vysvětluje, jak používat rozhraní API služby Microsoft překladač Text k převodu textu z jednoho jazyka do druhého v aplikaci Xamarin.Forms._

## <a name="overview"></a>Přehled

Rozhraní API překladač má dvě součásti:

- Text překlad rozhraní REST API převede text z jednoho jazyka na jiný jazyk textu. Rozhraní API automaticky rozpozná jazyk textu, který vám byl zaslán před jeho překladu.
- Rozpoznávání řeči překlad rozhraní REST API transcribe řeči z jednoho jazyka do textu jiný jazyk. Rozhraní API se také integruje je funkce, které řeči přeložený text.

Tento článek se zaměřuje na překlad textu z jednoho jazyka na jiný pomocí rozhraní API Text překladač.

Klíč rozhraní API musí být získána používat rozhraní API Text překladač. To lze získat v [jak se přihlásit k rozhraní API služby Microsoft překladač Text](/azure/cognitive-services/translator/translator-text-how-to-signup/).

Další informace o rozhraní API služby Microsoft překladač Text najdete v tématu [dokumentaci k rozhraní API Text překladač](/azure/cognitive-services/translator/).

## <a name="authentication"></a>Ověřování

Každý požadavek do rozhraní API Text překladač vyžaduje přístupový token JSON Web Token (JWT), který můžete získat od služby tokenů kognitivní služby v `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Nelze získat token tím, že požadavek POST do tokenů služby zadání `Ocp-Apim-Subscription-Key` záhlaví, který obsahuje klíč rozhraní API jako jeho hodnotu.

Následující příklad kódu ukazuje, jak požádat o přístup tokenu od služby tokenů:

```csharp
public AuthenticationService(string apiKey)
{
    subscriptionKey = apiKey;
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
}
...
async Task<string> FetchTokenAsync(string fetchUri)
{
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";
    var result = await httpClient.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
}
```

Vrácený přístupový token, který je Base64 text, má čas vypršení platnosti 10 minut. Proto ukázkovou aplikaci obnovuje se přístupový token každých 9 minut.

Přístupový token musí být zadány v každé rozhraní API Text překladač volání jako `Authorization` záhlaví řetězec s předponou `Bearer`, jak ukazuje následující příklad kódu:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Další informace o tokenu služby kognitivní services najdete v tématu [rozhraní API tokenů ověřování](http://docs.microsofttranslator.com/oauth-token.html).

## <a name="performing-text-translation"></a>Provádí překlad textu

Překlad textu dosáhnout tím, že požadavek GET na `translate` rozhraní API v `https://api.microsofttranslator.com/v2/http.svc/translate`. V ukázkové aplikaci `TranslateTextAsync` metoda vyvolá proces překladu text:

```csharp
public async Task<string> TranslateTextAsync(string text)
{
  ...
  string requestUri = GenerateRequestUri(Constants.TextTranslatorEndpoint, text, "en", "de");
  string accessToken = authenticationService.GetAccessToken();
  var response = await SendRequestAsync(requestUri, accessToken);
  var xml = XDocument.Parse(response);
  return xml.Root.Value;
}
```

`TranslateTextAsync` Metoda vygeneruje URI požadavku a získá přístupový token od služby tokenů. Požadavek překlad textu je poté odeslán do `translate` rozhraní API, které vrátí odpověď XML obsahující výsledek. Je-li analyzovat odpověď XML a vrácením výsledku překlad volání metody pro zobrazení.

Další informace o rozhraní API REST překlad Text najdete v tématu [rozhraní API služby Microsoft překladač Text](http://docs.microsofttranslator.com/text-translate.html).

### <a name="configuring-text-translation"></a>Konfigurace překlad textu

Proces překladu textu, můžete nakonfigurovat tak, že zadáte parametry dotazu HTTP:

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

Tato metoda nastaví text, který má být přeložit a jazyk, který chcete převede text, který se. Seznam jazyků podporovaných v Microsoft Translator najdete v tématu [podporované jazyky v rozhraní API služby Microsoft překladač Text](/azure/cognitive-services/translator/languages/).

> [!NOTE]
> Pokud aplikace potřebuje vědět, v jakém jazyce, je text, `Detect` zjistit jazyk textový řetězec nelze volat rozhraní API.

### <a name="sending-the-request"></a>Odesílání požadavku

`SendRequestAsync` Metoda provede požadavek GET REST API pro překlad Text a vrátí odpověď:

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Tato metoda vytváří požadavek GET přidáváním přístupový token `Authorization` záhlaví, řetězec s předponou `Bearer`. Potom posílá požadavek GET `translate` rozhraní API pomocí adresu URL požadavku zadání text k převodu a jazyk, který chcete převede text, který se. Odpověď je pak číst a vrátí volání metody.

`translate` Rozhraní API bude odesílat stavový kód HTTP 200 (OK) v odpovědi, za předpokladu, že žádost je platná, což naznačuje, že požadavek byl úspěšný a že požadované informace je v odpovědi. Seznam možná chyba odpovědí najdete v tématu zprávy odpovědi na [získat převede](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate).

### <a name="processing-the-response"></a>Zpracování odpovědi

Rozhraní API odpověď se vrátí ve formátu XML. Následující data XML zobrazí zprávu s typické úspěšné odpovědi:

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

V ukázkové aplikaci se analyzovat odpověď XML do `XDocument` instance s hodnotou kořenový XML nevrátila volání metody pro zobrazení, jak je vidět na následujících snímcích obrazovky:

![](text-translation-images/text-translation.png "Překlad textu němčina")

## <a name="summary"></a>Souhrn

Tento článek vysvětlené jak převede text z jednoho jazyka do textu jiný jazyk v aplikaci Xamarin.Forms pomocí rozhraní API služby Microsoft překladač Text. Kromě překlad textu, rozhraní API služby Microsoft překladač také transcribe řeči z jednoho jazyka do textu jiný jazyk.

## <a name="related-links"></a>Související odkazy

- [Dokumentace rozhraní API Text překladač](/azure/cognitive-services/translator/).
- [Využívání RESTful webová služba](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO kognitivní služby (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Překladač Microsoft Text rozhraní API](http://docs.microsofttranslator.com/text-translate.html).
