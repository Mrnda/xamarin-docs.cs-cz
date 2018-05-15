---
title: Rozpoznávání emocí úrovně rozpoznávání pomocí tučné rozhraní API
description: Rozhraní API vzhled přebírá výraz obličeje bitovou kopii jako vstup a vrátí data, která zahrnuje úrovní spolehlivosti mezi sadu emoce pro každý řez v bitové kopii. Tento článek vysvětluje, jak používat rozhraní API vzhled rozpoznat rozpoznávání emocí úrovně, abyste aplikaci Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 4dc04cb077b894b255eb496b2cb2983626573897
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="emotion-recognition-using-the-face-api"></a>Rozpoznávání emocí úrovně rozpoznávání pomocí tučné rozhraní API

_Rozhraní API vzhled přebírá výraz obličeje bitovou kopii jako vstup a vrátí data, která zahrnuje úrovní spolehlivosti mezi sadu emoce pro každý řez v bitové kopii. Tento článek vysvětluje, jak používat rozhraní API vzhled rozpoznat rozpoznávání emocí úrovně, abyste aplikaci Xamarin.Forms._

## <a name="overview"></a>Přehled

Rozhraní API vzhled můžete provádět emocí ke zjištění anger, cestou, působící, obavy, štěstí neutrální, sadness a neočekávaném, obličeje výrazu. Tyto emoce se všeobecně a ve všech jazykových verzích předávají prostřednictvím stejné základní obličeje výrazy. A vrátí výsledek rozpoznávání emocí úrovně pro výraz obličeje, rozhraní API vzhled může taky vrátí ohraničující pole pro zjištěné řezy. Všimněte si, že klíč rozhraní API musí být získána používat rozhraní API řez. To lze získat v [zkuste kognitivní služby](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Rozpoznávání emocí úrovně rozpoznávání lze provést pomocí klientské knihovny a pomocí rozhraní REST API. Tento článek se zaměřuje na provádění rozpoznávání emocí úrovně rozpoznávání přes REST API. Další informace o rozhraní REST API najdete v tématu [vzhled REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Rozhraní API vzhled lze také rozpoznat obličeje výrazy lidem v video a souhrn jejich emoce se můžete vrátit. Další informace najdete v tématu [jak analyzovat videa v reálném čase](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Další informace o rozhraní API vzhled najdete v tématu [vzhled API](/azure/cognitive-services/face/overview/).

## <a name="authentication"></a>Ověřování

Každý požadavek do rozhraní API vzhled vyžaduje klíč rozhraní API, která musí být zadán jako hodnotu `Ocp-Apim-Subscription-Key` záhlaví. Následující příklad kódu ukazuje, jak přidat klíč rozhraní API do `Ocp-Apim-Subscription-Key` hlavičky požadavku:

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Chyba odpovědi 401 způsobí selhání předat do rozhraní API vzhled platný klíč rozhraní API.

## <a name="performing-emotion-recognition"></a>Provádění rozpoznávání rozpoznávání emocí úrovně

Rozpoznávání rozpoznávání emocí úrovně se provádí tak, že požadavek POST obsahující bitovou kopii `detect` rozhraní API v `https://[location].api.cognitive.microsoft.com/face/v1.0`, kde `[location]]` je oblast, můžete použít k získání klíče rozhraní API. Požadavek volitelné parametry jsou:

- `returnFaceId` – jestli se mají vracet faceIds zjištěné řezy. Výchozí hodnota je `true`.
- `returnFaceLandmarks` – jestli se mají vracet vzhled zajímavá zjištěné ploch. Výchozí hodnota je `false`.
- `returnFaceAttributes` – jestli k analýze a vrátit zadán jeden nebo více čelí atributy. Podporované vzhled atributy patří `age`, `gender`, `headPose`, `smile`, `facialHair`, `glasses`, `emotion`, `hair`, `makeup`, `occlusion`, `accessories`, `blur`, `exposure`, a `noise`. Upozorňujeme, že vzhled atribut analýzy má další náklady na výpočetní a čas.

Obsahu bitové kopie musí být umístěny v textu požadavku POST jako adresa URL, nebo binární data.

> [!NOTE]
> Jsou podporované formáty souborů obrázků JPEG, GIF, PNG nebo BMP a povolená velikost souboru je od 1KB do 4 MB volného místa.

V ukázkové aplikaci proces rozpoznávání rozpoznávání emocí úrovně vyvolané volání `DetectAsync` metoda:

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

Toto volání metody určuje datový proud obsahující data obrázku, který má být vrácen faceIds, že vzhled zajímavá neměly vracet, a že by měly být analyzovány rozpoznávání emocí úrovně bitové kopie. Také určuje, že budou vráceny výsledky jako pole `Face` objekty. Pak `DetectAsync` metoda vyvolá `detect` REST API, která provádí rozpoznávání rozpoznávání emocí úrovně:

```csharp
public async Task<Face[]> DetectAsync(Stream imageStream, bool returnFaceId, bool returnFaceLandmarks, IEnumerable<FaceAttributeType> returnFaceAttributes)
{
  var requestUrl =
    $"{Constants.FaceEndpoint}/detect?returnFaceId={returnFaceId}" +
    "&returnFaceLandmarks={returnFaceLandmarks}" +
    "&returnFaceAttributes={GetAttributeString(returnFaceAttributes)}";
  return await SendRequestAsync<Stream, Face[]>(HttpMethod.Post, requestUrl, imageStream);
}
```

Tato metoda generuje URI požadavku a pak odešle požadavek na `detect` rozhraní API prostřednictvím `SendRequestAsync` metoda.

> [!NOTE]
> Musíte použít stejné oblasti v voláními rozhraní API vzhled jako jste použili k získání klíče pro předplatné. Například, pokud jste získali vaše předplatné klíče z `westus` oblast, bude koncový bod zjišťování vzhled `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

### <a name="sending-the-request"></a>Odesílání požadavku

`SendRequestAsync` Metoda provede požadavek POST do rozhraní API vzhled a vrátí výsledek v podobě `Face` pole:

```csharp
async Task<TResponse> SendRequestAsync<TRequest, TResponse>(HttpMethod httpMethod, string requestUrl, TRequest requestBody)
{
  var request = new HttpRequestMessage(httpMethod, Constants.FaceEndpoint);
  request.RequestUri = new Uri(requestUrl);
  if (requestBody != null)
  {
    if (requestBody is Stream)
    {
      request.Content = new StreamContent(requestBody as Stream);
      request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
    }
    else
    {
      // If the image is supplied via a URL
      request.Content = new StringContent(JsonConvert.SerializeObject(requestBody, s_settings), Encoding.UTF8, "application/json");
    }
  }

  HttpResponseMessage responseMessage = await _client.SendAsync(request);
  if (responseMessage.IsSuccessStatusCode)
  {
    string responseContent = null;
    if (responseMessage.Content != null)
    {
      responseContent = await responseMessage.Content.ReadAsStringAsync();
    }
    if (!string.IsNullOrWhiteSpace(responseContent))
    {
      return JsonConvert.DeserializeObject<TResponse>(responseContent, s_settings);
    }
    return default(TResponse);
  }
  else
  {
    ...
  }
  return default(TResponse);
}
```

Pokud je zadaný bitovou kopii prostřednictvím datového proudu, metoda sestavení nástrojem pro zabalení stream bitové kopie v požadavku POST `StreamContent` instance, která poskytuje obsah HTTP na základě na datový proud. Případně, pokud je zadaný bitovou kopii prostřednictvím adresy URL, metodu sestavení požadavku POST nástrojem pro zabalení adresu URL v `StringContent` instance, která poskytuje obsah HTTP na základě řetězce.

Požadavek POST je poté odeslán do `detect` rozhraní API. Odpověď pro čtení, deserializovat a vrátil volání metody.

`detect` Rozhraní API bude odesílat stavový kód HTTP 200 (OK) v odpovědi, za předpokladu, že žádost je platná, což naznačuje, že požadavek byl úspěšný a že požadované informace je v odpovědi. Seznam možná chyba odpovědí najdete v tématu [vzhled REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

### <a name="processing-the-response"></a>Zpracování odpovědi

Rozhraní API odpověď se vrátí ve formátu JSON. Následující data JSON zobrazí zprávu s typické úspěšné odpovědi, který poskytuje data požadovaná společností ukázkovou aplikaci:

```json
[  
   {  
      "faceId":"8a1a80fe-1027-48cf-a7f0-e61c0f005051",
      "faceRectangle":{  
         "top":192,
         "left":164,
         "width":339,
         "height":339
      },
      "faceAttributes":{  
         "emotion":{  
            "anger":0.0,
            "contempt":0.0,
            "disgust":0.0,
            "fear":0.0,
            "happiness":1.0,
            "neutral":0.0,
            "sadness":0.0,
            "surprise":0.0
         }
      }
   }
]
```

Zprávu úspěšné odpovědi, která se skládá z pole vzhled položky seřazeny podle velikosti obdélníku řez v sestupném pořadí, zatímco prázdnou odpověď označuje žádné řezy zjištěna. Každý rozpoznána vzhled zahrnuje řadu volitelné vzhled atributy, které jsou určené `returnFaceAttributes` argument `DetectAsync` metoda.

V ukázkové aplikaci se deserializovat JSON odpovědi do pole `Face` objekty. Při interpretaci výsledky z rozhraní API řez, zjištěné rozpoznávání emocí úrovně by měl interpretovat jako rozpoznávání emocí úrovně s nejvyšší skóre, jako jsou normalized skóre mají sečíst na jedno. Ukázkovou aplikaci proto zobrazí rozpoznaný rozpoznávání emocí úrovně s nejvyšší skóre pro největší zjištěné písmo v bitové kopii. Můžete toho dosáhnout s následujícím kódem:

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

Následující snímek obrazovky ukazuje výsledek procesu rozpoznávání rozpoznávání emocí úrovně v ukázkové aplikaci:

![](emotion-recognition-images/emotion-recognition.png "Rozpoznávání rozpoznávání emocí úrovně")

## <a name="summary"></a>Souhrn

Tento článek vysvětlené jak rozpoznat rozpoznávání emocí úrovně, abyste aplikaci na platformě Xamarin.Forms pomocí rozhraní API řez. Rozhraní API vzhled přebírá výraz obličeje bitovou kopii jako vstup a vrátí data, která zahrnuje důvěru mezi sadu emoce pro každý řez v bitové kopii.

## <a name="related-links"></a>Související odkazy

- [Čelí rozhraní API](/azure/cognitive-services/face/overview/).
- [TODO kognitivní služby (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Vzhled rozhraní REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
