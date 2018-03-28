---
title: Rozpoznávání řeči rozpoznávání řeči Microsoft rozhraní API
description: Rozhraní API pro rozpoznávání řeči Microsoft je rozhraní API založené na cloudu, které poskytuje algoritmy zpracovat mluvené jazyk. Tento článek vysvětluje, jak převést na text v aplikaci Xamarin.Forms zvuk pomocí rozhraní API REST rozpoznávání řeči společnosti Microsoft.
ms.topic: article
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 2230b11f9553fb779a86d7504ed5507d2e7cbaa7
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/28/2018
---
# <a name="speech-recognition-using-the-microsoft-speech-api"></a>Rozpoznávání řeči rozpoznávání řeči Microsoft rozhraní API

_Rozhraní API pro rozpoznávání řeči Microsoft je rozhraní API založené na cloudu, které poskytuje algoritmy zpracovat mluvené jazyk. Tento článek vysvětluje, jak převést na text v aplikaci Xamarin.Forms zvuk pomocí rozhraní API REST rozpoznávání řeči společnosti Microsoft._

## <a name="overview"></a>Přehled

Rozhraní API pro rozpoznávání řeči Microsoft má dvě součásti:

- Rozpoznávání řeči rozhraní API pro převod na text mluvené slovo. Rozpoznávání řeči lze provést pomocí rozhraní REST API, klientské knihovny nebo knihovna service.
- Převod textu na řeč rozhraní API pro převod textu na mluvené slovo. Převod textu na řeč se provádí prostřednictvím rozhraní REST API.

Tento článek se zaměřuje na provádění rozpoznávání řeči přes REST API. Zatímco knihovny klienta a služby podporují vrácení částečné výsledky, REST API může vrátit pouze jeden rozpoznávání výsledek, bez jakékoli částečné výsledky.

Klíč rozhraní API musí být získána používat rozhraní API pro rozpoznávání řeči společnosti Microsoft. To lze získat v [zkuste kognitivní služby](https://azure.microsoft.com/try/cognitive-services/).

Další informace o rozhraní API pro rozpoznávání řeči Microsoft najdete v tématu [dokumentaci k rozhraní API pro rozpoznávání řeči Microsoft](/azure/cognitive-services/speech/home/).

## <a name="authentication"></a>Ověřování

Každou žádost odeslanou REST API pro rozpoznávání řeči Microsoft vyžaduje přístupový token JSON Web Token (JWT), který můžete získat od služby tokenů kognitivní služby v `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Nelze získat token tím, že požadavek POST do tokenů služby zadání `Ocp-Apim-Subscription-Key` záhlaví, který obsahuje klíč rozhraní API jako jeho hodnotu.

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

Přístupový token musí být zadány v každé rozhraní API REST řeči Microsoft volání jako `Authorization` záhlaví řetězec s předponou `Bearer`, jak ukazuje následující příklad kódu:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Chyba 403 odpovědi způsobí selhání předat token platný přístup ke službě Microsoft řeči REST API.

## <a name="performing-speech-recognition"></a>Provádění rozpoznávání řeči

Rozpoznávání řeči je dosaženo tím, že požadavek POST do `recognition` rozhraní API v `https://speech.platform.bing.com/speech/recognition/`. Jeden požadavek nesmí obsahovat více než 10 sekund zvuk a doba trvání celkový požadavek nesmí překročit 14 sekund.

Zvukový obsah musí být umístěny v textu POST žádosti ve formátu wav.

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
    var speechResult = JsonConvert.DeserializeObject<SpeechResult>(response);

    fileStream.Dispose();
    return speechResult;
}
```

Zvuk se zaznamená do každého projektu specifických pro platformy jako PCM wav data a `RecognizeSpeechAsync` metoda používá `PCLStorage` balíček NuGet otevřít zvukový soubor jako datový proud. Žádost o rozpoznávání řeči, kterou se vygeneruje URI a přístupový token se načtou z tokenu služby. Žádost o rozpoznávání řeči je odeslána do `recognition` rozhraní API, které vrátí odpověď JSON obsahující výsledek. Odpověď JSON je deserializovat s výsledkem nevrátila volání metody pro zobrazení.

### <a name="configuring-speech-recognition"></a>Konfigurace rozpoznávání řeči

Proces rozpoznávání řeči, můžete nakonfigurovat tak, že zadáte parametry dotazu HTTP:

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    // To build a request URL, you should follow:
    // https://docs.microsoft.com/en-us/azure/cognitive-services/speech/getstarted/getstartedrest
    string requestUri = speechEndpoint;
    requestUri += @"dictation/cognitiveservices/v1?";
    requestUri += @"language=en-us";
    requestUri += @"&format=simple";
    System.Diagnostics.Debug.WriteLine(requestUri.ToString());
    return requestUri;
}
```

Hlavní konfigurace provádí `GenerateRequestUri` metodou je nastavení národního prostředí zvukového obsahu. Seznam podporovaných národních prostředí najdete v tématu [podporované jazyky ](/azure/cognitive-services/speech/api-reference-rest/supportedlanguages/).

### <a name="sending-the-request"></a>Odesílání požadavku

`SendRequestAsync` Metoda provede požadavek POST REST API pro rozpoznávání řeči společnosti Microsoft a vrátí odpověď:

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);
    var response = await httpClient.PostAsync(url, content);
    return await response.Content.ReadAsStringAsync();
}
```

Tato metoda vytvoří požadavku POST pomocí:

- Zabalení zvukový datový proud v `StreamContent` instance, která poskytuje obsah HTTP na základě na datový proud.
- Nastavení `Content-Type` hlavičky požadavku `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Přidání přístupový token `Authorization` záhlaví, řetězec s předponou `Bearer`.

Požadavek POST je poté odeslán do `recognition` rozhraní API. Odpověď je pak číst a vrátí volání metody.

`recognition` Rozhraní API bude odesílat stavový kód HTTP 200 (OK) v odpovědi, za předpokladu, že žádost je platná, což naznačuje, že požadavek byl úspěšný a že požadované informace je v odpovědi. Seznam možná chyba odpovědí najdete v tématu [Poradce při potížích s](/azure/cognitive-services/speech/troubleshooting).

### <a name="processing-the-response"></a>Zpracování odpovědi

Rozhraní API odpověď se vrátí ve formátu JSON, s rozpoznaný text se součástí `name` značky. Následující data JSON zobrazí zprávu s typické úspěšné odpovědi:

```json
{  
   "RecognitionStatus":"Success",
   "DisplayText":"Go shopping tomorrow.",
   "Offset":16000000,
   "Duration":17100000
}
```

V ukázkové aplikaci se deserializovat JSON odpovědi do `SpeechResult` instance s výsledkem nevrátila volání metody pro zobrazení, jak je vidět na následujících snímcích obrazovky:

![](speech-recognition-images/speech-recognition.png "Rozpoznávání řeči")

## <a name="summary"></a>Souhrn

Tento článek vysvětlené jak zvuk převedeno na text v aplikaci Xamarin.Forms pomocí REST API pro rozpoznávání řeči společnosti Microsoft. Kromě provádění rozpoznávání řeči, rozhraní API pro rozpoznávání řeči Microsoft také převést textu na mluvené slovo.

## <a name="related-links"></a>Související odkazy

- [Dokumentace k Microsoft řeči rozhraní API](/azure/cognitive-services/speech/home/).
- [Využívání RESTful webová služba](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO kognitivní služby (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
