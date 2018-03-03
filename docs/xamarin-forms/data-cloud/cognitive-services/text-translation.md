---
title: "Překlad text použití překladač rozhraní API"
description: "Rozhraní API služby Microsoft překladač slouží k převodu řeč a textu přes REST API. Tento článek vysvětluje, jak používat rozhraní API služby Microsoft Text překlad k převodu text z jednoho jazyka do druhého v aplikaci Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: f403ebaffdf742c22e61b73aee7a42648fe597dc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="text-translation-using-the-translator-api"></a>Překlad text použití překladač rozhraní API

_Rozhraní API služby Microsoft překladač slouží k převodu řeč a textu přes REST API. Tento článek vysvětluje, jak používat rozhraní API služby Microsoft Text překlad k převodu text z jednoho jazyka do druhého v aplikaci Xamarin.Forms._

## <a name="overview"></a>Přehled

Rozhraní API překladač má dvě součásti:

- Text překlad rozhraní REST API převede text z jednoho jazyka na jiný jazyk textu. Rozhraní API automaticky rozpozná jazyk textu, který vám byl zaslán před jeho překladu.
- Rozpoznávání řeči překlad rozhraní REST API transcribe řeči z jednoho jazyka do textu jiný jazyk. Rozhraní API se také integruje je funkce, které řeči přeložený text.

Tento článek se zaměřuje na překlad textu z jednoho jazyka na jiný pomocí rozhraní API pro překlad textu.

Klíč rozhraní API musí být získána používat rozhraní API pro překlad textu. To můžete získat podle těchto pokynů [Začínáme](http://docs.microsofttranslator.com/text-translate.html) na [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

Další informace o rozhraní API překladač Microsoft najdete v tématu [dokumentace společnosti Microsoft překladač](https://www.microsoft.com/cognitive-services/translator-api/documentation/TranslatorInfo/overview) na webu microsoft.com.

## <a name="authentication"></a>Ověřování

Každý požadavek do rozhraní API pro překlad Text vyžaduje přístupový token JSON Web Token (JWT), který můžete získat od služby tokenů kognitivní služby v `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Nelze získat token tím, že požadavek POST do tokenů služby zadání `Ocp-Apim-Subscription-Key` záhlaví, který obsahuje klíč rozhraní API jako jeho hodnotu.

Následující příklad kódu ukazuje, jak požádat o přístup tokenu od služby tokenů:

```csharp
async Task<string> FetchTokenAsync(string fetchUri, string apiKey)
{
  using (var client = new HttpClient())
  {
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";

    var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
  }
}
```

Vrácený přístupový token, který je Base64 text, má čas vypršení platnosti 10 minut. Proto ukázkovou aplikaci obnovuje se přístupový token každých 9 minut.

Přístupový token musí být zadány v každé rozhraní API překlad Text volání jako `Authorization` záhlaví řetězec s předponou `Bearer`, jak ukazuje následující příklad kódu:

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Další informace o tokenu služby kognitivní services najdete v tématu [rozhraní API tokenů ověřování](http://docs.microsofttranslator.com/oauth-token.html) na [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

## <a name="performing-text-translation"></a>Provádí překlad textu

Překlad textu dosáhnout tím, že požadavek GET na `Translate` rozhraní API v `https://api.microsofttranslator.com/v2/http.svc/Translate`. V ukázkové aplikaci `TranslateTextAsync` metoda vyvolá proces překladu text:

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

`TranslateTextAsync` Metoda vygeneruje URI požadavku a získá přístupový token od služby tokenů. Požadavek překlad textu je poté odeslán do `Translate` rozhraní API, které vrátí odpověď XML obsahující výsledek. Je-li analyzovat odpověď XML a vrácením výsledku překlad volání metody pro zobrazení.

Další informace o rozhraní API REST překlad Text najdete v tématu [ukázkový kód](http://docs.microsofttranslator.com/text-translate.html#/default) na [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="configuring-text-translation"></a>Konfigurace překlad textu

Proces překladu text se dá nakonfigurovat tak, že zadáte parametry dotazu HTTP. Jsou povinné a nepovinné parametry, pomocí ukazuje povinné parametry, které musí být nastavena následující metody:

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

Tato metoda nastaví text, který má být přeložit a jazyk, který chcete převede text, který se. Seznam jazyků podporovaných v Microsoft Translator najdete v tématu [jazyky](https://www.microsoft.com/translator/languages.aspx) na webu microsoft.com.

> [!NOTE]
> Pokud aplikace potřebuje vědět, v jakém jazyce, je text, `Detect` zjistit jazyk textový řetězec nelze volat rozhraní API.

Další informace o povinných a volitelných parametrů najdete v tématu [Text překlad API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate) na [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="sending-the-request"></a>Odesílání požadavku

`SendRequestAsync` Metoda provede požadavek GET REST API pro překlad Text a vrátí odpověď:

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
  using (var httpClient = new HttpClient())
  {
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
  }
}
```

Tato metoda vytváří požadavek GET přidáváním přístupový token `Authorization` záhlaví, řetězec s předponou `Bearer`. Potom posílá požadavek GET `Translate` rozhraní API pomocí adresu URL požadavku zadání text k převodu a jazyk, který chcete převede text, který se. Odpověď je pak číst a vrátí volání metody.

`Translate` Rozhraní API bude odesílat stavový kód HTTP 200 (OK) v odpovědi, za předpokladu, že žádost je platná, což naznačuje, že požadavek byl úspěšný a že požadované informace je v odpovědi. Seznam možná chyba odpovědí najdete v tématu zprávy odpovědi na [získat převede](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate) na [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="processing-the-response"></a>Zpracování odpovědi

Rozhraní API odpověď se vrátí ve formátu XML. Následující data XML zobrazí zprávu s typické úspěšné odpovědi:

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

V ukázkové aplikaci se analyzovat odpověď XML do `XDocument` instance s hodnotou kořenový XML nevrátila volání metody pro zobrazení, jak je vidět na následujících snímcích obrazovky:

![](text-translation-images/text-translation.png "Překlad textu němčina")

## <a name="summary"></a>Souhrn

Tento článek vysvětlené jak převede text z jednoho jazyka do textu jiný jazyk v aplikaci Xamarin.Forms pomocí rozhraní API služby Microsoft Text překlad. Kromě překlad textu, rozhraní API služby Microsoft překladač také transcribe řeči z jednoho jazyka do textu jiný jazyk.



## <a name="related-links"></a>Související odkazy

- [Překladač Microsoft dokumentace](https://www.microsoft.com/cognitive-services/translator-api/documentation/TranslatorInfo/overview)
- [Využívání RESTful webová služba](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO kognitivní služby (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Text Translation API](http://docs.microsofttranslator.com/text-translate.html)
