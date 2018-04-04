---
title: Rozpoznávání emocí úrovně rozpoznávání pomocí tučné rozhraní API
description: Rozhraní API vzhled přebírá výraz obličeje bitovou kopii jako vstup a vrátí data, která zahrnuje úrovní spolehlivosti mezi sadu emoce pro každý řez v bitové kopii. Tento článek vysvětluje, jak používat rozhraní API vzhled rozpoznat rozpoznávání emocí úrovně, abyste aplikaci Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 49e53425dbaf3aadd74d02ab030929e3311c7c8c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="emotion-recognition-using-the-face-api"></a>Rozpoznávání emocí úrovně rozpoznávání pomocí tučné rozhraní API

_Rozhraní API vzhled přebírá výraz obličeje bitovou kopii jako vstup a vrátí data, která zahrnuje úrovní spolehlivosti mezi sadu emoce pro každý řez v bitové kopii. Tento článek vysvětluje, jak používat rozhraní API vzhled rozpoznat rozpoznávání emocí úrovně, abyste aplikaci Xamarin.Forms._

## <a name="overview"></a>Přehled

Rozhraní API vzhled můžete provádět emocí ke zjištění anger, cestou, působící, obavy, štěstí neutrální, sadness a neočekávaném, obličeje výrazu. Tyto emoce se všeobecně a ve všech jazykových verzích předávají prostřednictvím stejné základní obličeje výrazy. A vrátí výsledek rozpoznávání emocí úrovně pro výraz obličeje, rozhraní API vzhled může taky vrátí ohraničující pole pro zjištěné řezy. Všimněte si, že klíč rozhraní API musí být získána používat rozhraní API řez. To lze získat v [zkuste kognitivní služby](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Rozpoznávání emocí úrovně rozpoznávání lze provést pomocí klientské knihovny a pomocí rozhraní REST API. Tento článek zaměřuje na provádění rozpoznávání emocí úrovně rozpoznávání prostřednictvím [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/) klientské knihovny, která si můžete stáhnout z NuGet.

Rozhraní API vzhled lze také rozpoznat obličeje výrazy lidem v video a souhrn jejich emoce se můžete vrátit. Další informace najdete v tématu [jak analyzovat videa v reálném čase](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Další informace o rozhraní API vzhled najdete v tématu [vzhled API](/azure/cognitive-services/face/overview/).

## <a name="performing-emotion-recognition"></a>Provádění rozpoznávání rozpoznávání emocí úrovně

Rozpoznávání rozpoznávání emocí úrovně je dosaženo tím, že nahrajete stream bitové kopie do rozhraní API řez. Velikost souboru bitové kopie nesmí být větší než 4MB, a podporované formáty souborů jsou JPEG, GIF, PNG nebo BMP.

Následující příklad kódu ukazuje proces rozpoznávání rozpoznávání emocí úrovně:

```csharp
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;

var faceServiceClient = new FaceServiceClient(Constants.FaceApiKey, Constants.FaceEndpoint);
// e.g. var faceServiceClient = new FaceServiceClient("a3dbe2ed6a5a9231bb66f9a964d64a12", "https://westus.api.cognitive.microsoft.com/face/v1.0/detect");

var faceAttributes = new FaceAttributeType[] { FaceAttributeType.Emotion };
using (var photoStream = photo.GetStream())
{
    Face[] faces = await faceServiceClient.DetectAsync(photoStream, true, false, faceAttributes);
    if (faces.Any())
    {
        // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
        emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
    }
    // Store emotion as app rating
    ...
}
```

`FaceServiceClient` Instance musí být vytvořený provést rozpoznávání rozpoznávání emocí úrovně, se setkávají klíč rozhraní API a předávány jako argumenty pro koncový bod `FaceServiceClient` konstruktor.

> [!NOTE]
> Musíte použít stejné oblasti v voláními rozhraní API vzhled jako jste použili k získání klíče pro předplatné. Například, pokud jste získali vaše předplatné klíče z `westus` oblast, bude koncový bod zjišťování vzhled `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

`DetectAsync` Metodu, která je volána `FaceServiceClient` instance, odešle obrázek rozhraní API vzhled jako `Stream`. Klíč rozhraní API bude odeslána do rozhraní API vzhled, když tato operace je volána. Způsobí selhání odeslat platný klíč rozhraní API `Microsoft.ProjectOxford.Face.FaceAPIException` hlášeny s výjimka zpráva označující, že byl odeslán neplatný klíč rozhraní API.

`DetectAsync` Metoda vrátí `Face` pole, za předpokladu, že byla rozpoznána řez. Každý vrátil vzhled obsahuje obdélníku indikující její umístění v kombinaci s řadou volitelné vzhled atributy, které jsou určené `faceAttributes` argument `DetectAsync` metoda. Pokud je zjištěn žádné řez, prázdnou `Face` pole bude vrácen.

Při interpretaci výsledky z rozhraní API řez, zjištěné rozpoznávání emocí úrovně by měl interpretovat jako rozpoznávání emocí úrovně s nejvyšší skóre, jako jsou normalized skóre mají sečíst na jedno. Proto ukázkovou aplikaci zobrazí rozpoznaný rozpoznávání emocí úrovně s nejvyšší skóre pro největší zjištěné písmo v bitové kopii, jak je vidět na následujících snímcích obrazovky:

![](emotion-recognition-images/emotion-recognition.png "Rozpoznávání rozpoznávání emocí úrovně")

## <a name="summary"></a>Souhrn

Tento článek vysvětlené jak rozpoznat rozpoznávání emocí úrovně, abyste aplikaci na platformě Xamarin.Forms pomocí rozhraní API řez. Rozhraní API vzhled přebírá výraz obličeje bitovou kopii jako vstup a vrátí data, která zahrnuje důvěru mezi sadu emoce pro každý řez v bitové kopii.

## <a name="related-links"></a>Související odkazy

- [Čelí rozhraní API](/azure/cognitive-services/face/overview/).
- [TODO kognitivní služby (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/)
