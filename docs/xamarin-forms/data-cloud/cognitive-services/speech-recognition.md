---
title: "Rozpoznávání řeči rozpoznávání řeči Bing rozhraní API"
description: "Rozhraní API pro rozpoznávání řeči Bing je rozhraní API založené na cloudu, které poskytuje algoritmy zpracovat mluvené jazyk. Tento článek vysvětluje, jak převést na text v aplikaci Xamarin.Forms zvuk pomocí rozhraní API REST rozpoznávání řeči Bing."
ms.topic: article
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 186ea6277ec7bd4ceb52855186e6fd88344b1b86
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="speech-recognition-using-the-bing-speech-api"></a>Rozpoznávání řeči rozpoznávání řeči Bing rozhraní API

_Rozhraní API pro rozpoznávání řeči Bing je rozhraní API založené na cloudu, které poskytuje algoritmy zpracovat mluvené jazyk. Tento článek vysvětluje, jak převést na text v aplikaci Xamarin.Forms zvuk pomocí rozhraní API REST rozpoznávání řeči Bing._

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně předběžné verze")

> [!NOTE]
> Rozhraní API pro rozpoznávání řeči Bing je stále ve verzi preview. Existuje může být nejnovější změny do rozhraní API před finální verzi.

## <a name="overview"></a>Přehled

Rozhraní API pro rozpoznávání řeči Bing má dvě součásti:

- Rozpoznávání řeči rozhraní API pro převod na text mluvené slovo. Rozpoznávání řeči lze provést pomocí rozhraní REST API, klientské knihovny nebo knihovna service.
- Převod textu na řeč rozhraní API pro převod textu na mluvené slovo. Převod textu na řeč se provádí prostřednictvím rozhraní REST API.

Tento článek se zaměřuje na provádění rozpoznávání řeči přes REST API. Zatímco knihovny klienta a služby podporují vrácení částečné výsledky, REST API může vrátit pouze jeden rozpoznávání výsledek, bez jakékoli částečné výsledky.

Klíč rozhraní API musí být získána používat rozhraní API pro rozpoznávání řeči Bing. To lze získat v [Začínáme zdarma](https://www.microsoft.com/cognitive-services/sign-up?ReturnUrl=/cognitive-services/subscriptions?productId=%2fproducts%2fBing.Speech.Preview) na webu microsoft.com.

Další informace o rozhraní API pro rozpoznávání řeči Bing najdete v tématu [Bing řeči dokumentaci](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview) na webu microsoft.com.

## <a name="authentication"></a>Ověřování

Každý požadavek do rozhraní API REST rozpoznávání řeči Bing vyžaduje přístupový token JSON Web Token (JWT), který můžete získat od služby tokenů kognitivní služby v `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Nelze získat token tím, že požadavek POST do tokenů služby zadání `Ocp-Apim-Subscription-Key` záhlaví, který obsahuje klíč rozhraní API jako jeho hodnotu.

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

Přístupový token musí být zadány v každé Bing rozpoznávání řeči REST API volat jako `Authorization` záhlaví řetězec s předponou `Bearer`, jak ukazuje následující příklad kódu:

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Chyba 403 odpovědi způsobí selhání předat token platný přístup ke službě Bing rozpoznávání řeči REST API.

## <a name="performing-speech-recognition"></a>Provádění rozpoznávání řeči

Rozpoznávání řeči je dosaženo tím, že požadavek POST do `recognize` rozhraní API v `https://speech.platform.bing.com/recognize`. Jeden požadavek nesmí obsahovat více než 10 sekund zvuk a doba trvání celkový požadavek nesmí překročit 14 sekund.

Zvukový obsah musí být umístěny v textu POST žádosti ve formátu wav. Informace o podporovaných kodeky najdete v tématu [podporované kodeky](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-codecs) na webu microsoft.com.

V ukázkové aplikaci `RecognizeSpeechAsync` metoda vyvolá proces rozpoznávání řeči:

```csharp
public async Task<SpeechResult> RecognizeSpeechAsync(string filename)
{
    ...

    // Read audio file to a stream
    var file = await PCLStorage.FileSystem.Current.LocalStorage.GetFileAsync(filename);
    var fileStream = await file.OpenAsync(PCLStorage.FileAccess.Read);

    // Send audio stream to Bing and deserialize the response
    string requestUri = GenerateRequestUri(Constants.SpeechRecognitionEndpoint);
    string accessToken = authenticationService.GetAccessToken();
    var response = await SendRequestAsync(fileStream, requestUri, accessToken, Constants.AudioContentType);
    var speechResults = JsonConvert.DeserializeObject<SpeechResults>(response);

    fileStream.Dispose();
    return speechResults.results.FirstOrDefault();
}
```

Zvuk se zaznamená do každého projektu specifických pro platformy jako PCM wav data a `RecognizeSpeechAsync` metoda používá `PCLStorage` balíček NuGet otevřít zvukový soubor jako datový proud. Žádost o rozpoznávání řeči, kterou se vygeneruje URI a přístupový token se načtou z tokenu služby. Žádost o rozpoznávání řeči je odeslána do `recognize` rozhraní API, které vrátí odpověď JSON obsahující výsledek. Odpověď JSON je deserializovat s výsledkem nevrátila volání metody pro zobrazení.

### <a name="configuring-speech-recognition"></a>Konfigurace rozpoznávání řeči

Proces rozpoznávání řeči lze nakonfigurovat tak, že zadáte parametry dotazu HTTP. Jsou povinné a nepovinné parametry, pomocí ukazuje povinné parametry, které musí být nastavena následující metody:

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    string requestUri = speechEndpoint;
    requestUri += @"?scenarios=ulm";                                    // websearch is the other option
    requestUri += @"&appid=D4D52672-91D7-4C74-8AD8-42B1D98141A5";       // You must use this ID
    requestUri += @"&locale=en-US";                                     // Other languages supported
    requestUri += string.Format("&device.os={0}", operatingSystem);     // Open field
    requestUri += @"&version=3.0";                                      // Required value
    requestUri += @"&format=json";                                      // Required value
    requestUri += @"&instanceid=fe34a4de-7927-4e24-be60-f0629ce1d808";  // GUID for device making the request
    requestUri += @"&requestid=" + Guid.NewGuid().ToString();           // GUID for the request
    return requestUri;
}
```

Hlavní konfigurace provádí `GenerateRequestUri` metodou je nastavení národního prostředí zvukového obsahu. Seznam podporovaných národních prostředí najdete v tématu [podporované národní prostředí ](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-locales) na webu microsoft.com.

Další informace o možných hodnot pro povinné parametry najdete v tématu [požadované parametry](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#required-parameters) na webu microsoft.com. Informace o volitelné parametry najdete v tématu [volitelné parametry](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition) na webu microsoft.com.

### <a name="sending-the-request"></a>Odesílání požadavku

`SendRequestAsync` Metoda provede požadavek POST do rozhraní API REST rozpoznávání řeči Bing a vrátí odpověď:

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);

    using (var httpClient = new HttpClient())
    {
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
        var response = await httpClient.PostAsync(url, content);

        return await response.Content.ReadAsStringAsync();
    }
}
```

Tato metoda vytvoří požadavku POST pomocí:

- Zabalení zvukový datový proud v `StreamContent` instance, která poskytuje obsah HTTP na základě na datový proud.
- Nastavení `Content-Type` hlavičky požadavku `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Přidání přístupový token `Authorization` záhlaví, řetězec s předponou `Bearer`.

Požadavek POST je poté odeslán do `recognize` rozhraní API. Odpověď je pak číst a vrátí volání metody.

`recognize` Rozhraní API bude odesílat stavový kód HTTP 200 (OK) v odpovědi, za předpokladu, že žádost je platná, což naznačuje, že požadavek byl úspěšný a že požadované informace je v odpovědi. Seznam možná chyba odpovědí najdete v tématu [chybové odpovědi](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#error-responses) na webu microsoft.com.

### <a name="processing-the-response"></a>Zpracování odpovědi

Rozhraní API odpověď se vrátí ve formátu JSON, s rozpoznaný text se součástí `name` značky. Následující data JSON zobrazí zprávu s typické úspěšné odpovědi:

```csharp
{
  "version": "3.0",
  "header": {
    "status": "success",
    "scenario": "ulm",
    "name": "go shopping tomorrow",
    "lexical": "go shopping tomorrow",
    "properties": {
      "requestid": "e06c059d-fa37-4bb1-843f-4914350279a8",
      "HIGHCONF": "1"
    }
  },
  "results": [
    {
      "scenario": "ulm",
      "name": "go shopping tomorrow",
      "lexical": "go shopping tomorrow",
      "confidence": "0.9493451",
      "properties": {
        "HIGHCONF": "1"
      }
    }
  ]
}
```

V ukázkové aplikaci se deserializovat JSON odpovědi do `SpeechResult` instance s výsledkem nevrátila volání metody pro zobrazení, jak je vidět na následujících snímcích obrazovky:

![](speech-recognition-images/speech-recognition.png "Rozpoznávání řeči")

Informace o hodnotách jednotlivé značky JSON najdete v tématu [odpovědí rozpoznávání řeči](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#speech-recognition-responses) na webu microsoft.com.

## <a name="summary"></a>Souhrn

Tento článek vysvětlené jak zvuk převedeno na text v aplikaci Xamarin.Forms pomocí rozhraní API REST rozpoznávání řeči Bing. Kromě provádění rozpoznávání řeči, rozhraní API pro rozpoznávání řeči Bing taky převést textu na mluvené slovo.



## <a name="related-links"></a>Související odkazy

- [Dokumentace řeči Bing](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview)
- [Využívání RESTful webová služba](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO kognitivní služby (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Rozpoznávání řeči Bing rozhraní REST API](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition)
