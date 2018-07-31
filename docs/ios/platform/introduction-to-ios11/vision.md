---
title: Pro zpracování obrazu rozhraní Xamarin.iOS
description: Tento dokument popisuje, jak používat iOS 11 Vision rozhraní Xamarin.iOS. Konkrétně prodiskutování obdélník detekci a rozpoznávání tváře.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2017
ms.openlocfilehash: 4746de2f351e866fd72946b204f97e997c3e88c4
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350661"
---
# <a name="vision-framework-in-xamarinios"></a>Pro zpracování obrazu rozhraní Xamarin.iOS

Rozhraní pro zpracování obrazu přidává několik nových funkcí do systému iOS 11, včetně pro zpracování obrázků:

- [Detekce obdélník](#rectangles)
- [Rozpoznávání tváře](#faces)
- Machine Learning Analýza obrázků (popsáno v [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Detekce čárového kódu
- Analýza zarovnání obrázku
- Rozpoznávání textu
- Detekce horizon
- Detekce a sledování

![Fotografie s tři obdélníky zjistil](vision-images/found-rectangles-tiny.png) ![Fotografie se zjistil dvě tváře](vision-images/xamarin-home-faces-tiny.png)

Obdélník detekci a rozpoznávání tváře jsou popsány podrobněji níže.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Detekce obdélník

[VisionRects ukázka](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) ukazuje, jak zpracování obrazu a nakreslit zjištěné obdélníky.

### <a name="1-initialize-the-vision-request"></a>1. Inicializujte žádost o zpracování obrazu

V `ViewDidLoad`, vytvořit `VNDetectRectanglesRequest` odkazující `HandleRectangles` metodu, která bude volána na konci každého požadavku:

`MaximumObservations` By měla být nastavena také vlastnost, jinak to bude použita výchozí hodnota 1 a vrátí se pouze jeden výsledek.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. Spuštění zpracování pro zpracování obrazu

Následující kód spustí zpracování požadavku. V **VisionRects** ukázku, tento kód se spustí poté, co uživatel vybral obrázku:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Tato obslužná rutina předá `ciImage` Framework pro zpracování obrazu `VNDetectRectanglesRequest` , který byl vytvořen v kroku 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Zpracování výsledků zpracování pro zpracování obrazu

Po dokončení detekce obdélník rozhraní provede `HandleRectangles` metoda souhrn, které jsou uvedené níže:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  bool atLeastOneValid = false;
  foreach (var o in observations){
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  if (!atLeastOneValid) return;
  // Show the pre-processed image
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Zobrazení výsledků

`OverlayRectangles` Metodu **VisionRectangles** ukázka má tři funkce:

- Vykreslování zdrojového obrázku
- Vykreslování rámečku označuje, kde každý z nich byl zjištěn, a
- Přidání textový popisek pro každý obdélníku pomocí CoreGraphics.

Zobrazení [na ukázkový zdroj](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) pro konkrétní metody CoreGraphics.

![Fotografie s tři obdélníky zjistil](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Další zpracování

Detekce obdélníku je často jenom prvním krokem v řetězci operací, jako třeba s [v tomto příkladu CoreMLVision](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision), kde jsou předány obdélníků modelů CoreML analyzovat rukou psaný číslic.


<a name="faces" />

## <a name="face-detection"></a>Rozpoznávání tváře

[VisionFaces ukázka](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) funguje podobně jako ochrana čipem **VisionRectangles** vzorkování, pomocí jiné třídy žádost o zpracování obrazu.

### <a name="1-initialize-the-vision-request"></a>1. Inicializujte žádost o zpracování obrazu

V `ViewDidLoad`, vytvořit `VNDetectFaceRectanglesRequest` odkazující `HandleRectangles` metodu, která bude volána na konci každého požadavku.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. Spuštění zpracování pro zpracování obrazu

Následující kód spustí zpracování požadavku. V **VisionFaces** tento kód se spustí poté, co uživatel vybral bitovou kopii ukázky:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Tato obslužná rutina předá `ciImage` Framework pro zpracování obrazu `VNDetectFaceRectanglesRequest` , který byl vytvořen v kroku 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Zpracování výsledků zpracování pro zpracování obrazu

Po dokončení rozpoznávání tváře obslužná rutina provede `HandleRectangles` metodu, která provede zpracování chyb a zobrazí hranice zjištěné tváří a volání `OverlayRectangles` kreslení obdélníků ohraničující na původní obrázek:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNFaceObservation>();
  // ... omitted error handling...
  var summary = "";
  var imageSize = InputImage.Extent.Size;
  bool atLeastOneValid = false;
  Console.WriteLine("Faces:");
  summary += "Faces:";
  foreach (var o in observations) {
    // Verify detected rectangle is valid. omitted
    var boundingBox = o.BoundingBox.Scaled(imageSize);
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  // Show the pre-processed image (on UI thread)
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Zobrazení výsledků

`OverlayRectangles` Metodu **VisionFaces** ukázka má tři funkce:

- Vykreslování zdrojového obrázku
- Kreslení obdélníku pro každou tvář zjistil, a
- Přidání textový popisek pro každý obličej pomocí CoreGraphics.

Zobrazení [na ukázkový zdroj](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) pro konkrétní metody CoreGraphics.

![Fotografie se zjistil dvě tváře](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Další zpracování

Rozhraní pro zpracování obrazu zahrnuje další funkce pro detekci funkce rozpoznávání obličeje, jako je například oči a úst. Použití `VNDetectFaceLandmarksRequest` typ, který vrátí `VNFaceObservation` výsledky jako v kroku 3 výše, ale s požadovanými dodatečnými `VNFaceLandmark` data.


## <a name="related-links"></a>Související odkazy

- [Pro zpracování obrazu obdélníky (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [Pro zpracování obrazu tváří (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Zálohy v základní Image - filtry, počítačů, pro zpracování obrazu a další (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/510/)
