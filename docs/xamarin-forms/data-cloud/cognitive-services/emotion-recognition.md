---
title: "Rozpoznávání emocí úrovně rozpoznávání pomocí rozpoznávání emocí úrovně rozhraní API"
description: "Rozhraní API pro rozpoznávání emocí úrovně přebírá výraz obličeje bitovou kopii jako vstup a vrátí úrovní spolehlivosti mezi sadu emoce pro každý řez v bitové kopii. Tento článek vysvětluje, jak použít rozhraní API pro rozpoznávání emocí úrovně rozpoznat rozpoznávání emocí úrovně, abyste aplikaci Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 159bd1b23eb7505c5d5629570a34d54e0525567e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="emotion-recognition-using-the-emotion-api"></a>Rozpoznávání emocí úrovně rozpoznávání pomocí rozpoznávání emocí úrovně rozhraní API

_Rozhraní API pro rozpoznávání emocí úrovně přebírá výraz obličeje bitovou kopii jako vstup a vrátí úrovní spolehlivosti mezi sadu emoce pro každý řez v bitové kopii. Tento článek vysvětluje, jak použít rozhraní API pro rozpoznávání emocí úrovně rozpoznat rozpoznávání emocí úrovně, abyste aplikaci Xamarin.Forms._

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně předběžné verze")

> [!NOTE]
> Rozhraní API pro rozpoznávání emocí úrovně je stále ve verzi preview. Existuje může být nejnovější změny do rozhraní API před finální verzi.

## <a name="overview"></a>Přehled

Rozhraní API pro rozpoznávání emocí úrovně může zjistit anger, cestou, působící, obavy, štěstí, neutrální, sadness a při neočekávaném, obličeje výrazu. Tyto emoce se všeobecně a ve všech jazykových verzích předávají prostřednictvím stejné základní obličeje výrazy. A vrátí výsledek rozpoznávání emocí úrovně pro výraz obličeje, rozhraní API pro rozpoznávání emocí úrovně také vrátí hodnotu ohraničující pole pro zjištěné řezy pomocí rozhraní API řez. Pokud uživatel již volána rozhraní API řez, kteří odesílají vzhled rámečku jako volitelné vstup. Všimněte si, že klíč rozhraní API musí být získána používat rozhraní API pro rozpoznávání emocí úrovně. To lze získat v [Začínáme zdarma](https://www.microsoft.com/cognitive-services/sign-up) na webu microsoft.com.

Rozpoznávání emocí úrovně rozpoznávání lze provést pomocí klientské knihovny a pomocí rozhraní REST API. Tento článek zaměřuje na provádění rozpoznávání emocí úrovně rozpoznávání prostřednictvím [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) klientské knihovny, která si můžete stáhnout z NuGet.

Rozhraní API pro rozpoznávání emocí úrovně lze také rozpoznat obličeje výrazy lidem v video a vrátí souhrn jejich emoce. Další informace najdete v tématu [rozpoznávání emocí úrovně v Video](https://www.microsoft.com/cognitive-services/emotion-api/documentation#emotion-in-video) na webu microsoft.com.

Další informace o rozhraní API pro rozpoznávání emocí úrovně najdete v tématu [dokumentaci k rozhraní API pro rozpoznávání emocí úrovně](https://www.microsoft.com/cognitive-services/emotion-api/documentation) na webu microsoft.com.

## <a name="performing-emotion-recognition"></a>Provádění rozpoznávání rozpoznávání emocí úrovně

Rozpoznávání rozpoznávání emocí úrovně je dosaženo tím, že nahrajete do rozhraní API pro rozpoznávání emocí úrovně stream bitové kopie. Velikost souboru bitové kopie nesmí být větší než 4MB, a podporované formáty souborů jsou JPEG, GIF, PNG nebo BMP. V rámci bitové kopie rozsah velikost rozpoznat řez je 36 x 36 do 4096 x 4096 pixelů. Žádné řezy mimo tento rozsah nebudou zjištěna.

Následující příklad kódu ukazuje proces rozpoznávání rozpoznávání emocí úrovně:

```csharp
using Microsoft.ProjectOxford.Emotion;
using Microsoft.ProjectOxford.Emotion.Contract;

var emotionClient = new EmotionServiceClient(Constants.EmotionApiKey);

using (var photoStream = photo.GetStream())
{
  Emotion[] emotionResult = await emotionClient.RecognizeAsync(photoStream);
  if (emotionResult.Any())
  {
    // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
    emotionResultLabel.Text = emotionResult.FirstOrDefault().Scores.ToRankedList().FirstOrDefault().Key;
  }
  // Store emotion as app rating
  ...
}
```

`EmotionServiceClient` Instance musí být vytvořený provést klíčem rozhraní API pro rozpoznávání emocí úrovně, které jsou předávány jako argument pro rozpoznávání emocí úrovně rozpoznávání `EmotionServiceClient` konstruktor.

`RecognizeAsync` Metodu, která je volána `EmotionServiceClient` instance, odešle obrázek rozhraní API pro rozpoznávání emocí úrovně, jako `Stream`. Klíč rozhraní API bude odeslat do rozhraní API pro rozpoznávání emocí úrovně, pokud tato operace je volána. Způsobí selhání odeslat platný klíč rozhraní API `Microsoft.ProjectOxford.Common.ClientException` hlášeny s výjimka zpráva označující, že byl odeslán neplatný klíč rozhraní API.

`RecognizeAsync` Metoda vrátí `Emotion` pole, za předpokladu, že byla rozpoznána řez. Pro každé bitové kopie maximální počet řezy, které lze vyhledat je 64 a řezy jsou seřazeny podle velikosti obdélníku řez v sestupném pořadí. Pokud je zjištěn žádné řez, prázdnou `Emotion` pole bude vrácen.

Při interpretaci výsledky z rozhraní API pro rozpoznávání emocí úrovně, zjištěné rozpoznávání emocí úrovně by měl interpretovat jako rozpoznávání emocí úrovně s nejvyšší skóre, jako jsou normalized skóre mají sečíst na jedno. Proto ukázkovou aplikaci zobrazí rozpoznaný rozpoznávání emocí úrovně s nejvyšší skóre pro největší zjištěné písmo v bitové kopii, jak je vidět na následujících snímcích obrazovky:

![](emotion-recognition-images/emotion-recognition.png "Rozpoznávání rozpoznávání emocí úrovně")

## <a name="summary"></a>Souhrn

Tento článek vysvětluje jak rozpoznat rozpoznávání emocí úrovně, abyste aplikaci na platformě Xamarin.Forms pomocí rozhraní API pro rozpoznávání emocí úrovně. Rozhraní API pro rozpoznávání emocí úrovně přebírá výraz obličeje bitovou kopii jako vstup a vrátí důvěru mezi sadu emoce pro každý řez v bitové kopii.


## <a name="related-links"></a>Související odkazy

- [Dokumentace k rozpoznávání emocí úrovně rozhraní API](https://www.microsoft.com/cognitive-services/emotion-api/documentation)
- [TODO kognitivní služby (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/)
- [Rozpoznávání emocí úrovně rozhraní API](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)
