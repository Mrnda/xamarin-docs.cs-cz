---
title: Vize Framework
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2016
ms.openlocfilehash: 698bf829128cff1263e98b49d29a77b75ec32ad9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="vision-framework"></a>Vize Framework

Rozhraní framework vize přidá počet novou bitovou kopii zpracování funkcí do systému iOS 11, včetně:

- [Detekce obdélníku](#rectangles)
- [Vzhled detekce](#faces)
- Počítač Learning Image Analysis (popsané v [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Detekce čárový kód
- Analýza zarovnání obrázku
- Detekce textu
- Detekce horizon
- Objekt detekce & sledování

![Nešlo se tři obdélníků zjistil](vision-images/found-rectangles-tiny.png) ![Nešlo se dvěma řezy zjistil](vision-images/xamarin-home-faces-tiny.png)

Rámeček zjišťování a zjišťování vzhled jsou podrobněji popsána níže.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Detekce obdélníku

[VisionRects ukázka](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) ukazuje, jak zpracovat bitovou kopii a kreslení obdélníků zjištěných na něm.

### <a name="1-initialize-the-vision-request"></a>1. Inicializace vize požadavku

V `ViewDidLoad`, vytvoření `VNDetectRectanglesRequest` odkazující `HandleRectangles` metoda, která bude volána na konci každého požadavku:

`MaximumObservations` By měla být nastavena také vlastnost, jinak bude použita výchozí 1 a bude vrácen pouze jeden výsledek.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. Spuštění zpracování vize

Následující kód spustí zpracování požadavku. V **VisionRects** ukázku, tento kód spustí po uživatel vybral obrázku:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Tato obslužná rutina předá `ciImage` Framework vize `VNDetectRectanglesRequest` který byl vytvořen v kroku 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Zpracování výsledků vize zpracování

Po dokončení detekce obdélníku rozhraní provede `HandleRectangles` metoda, souhrn, které jsou uvedeny níže:

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

### <a name="4-display-the-results"></a>4. Zobrazit výsledky

`OverlayRectangles` Metoda v **VisionRectangles** ukázka má tři funkce:

- Vykreslování se zdrojovou bitovou kopií,
- Kreslení obdélníků označíte, kde byla zjištěna každé z nich, a
- Přidání textový popisek pro každý rámeček pomocí CoreGraphics.

Zobrazení [na ukázkový zdroj](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) pro konkrétní metody CoreGraphics.

![Nešlo se tři obdélníků zjistil](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Další zpracování

Detekce obdélníku je často jenom první krok v řetězu operací, jako třeba s [v tomto příkladu CoreMLVision](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision), kde se budou předávat obdélníků model CoreML analyzovat psané číslic.


<a name="faces" />

## <a name="face-detection"></a>Vzhled detekce

[VisionFaces ukázka](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) funguje podobně jako ochrana **VisionRectangles** ukázkové použití různých třídy žádost o vizi.

### <a name="1-initialize-the-vision-request"></a>1. Inicializace vize požadavku

V `ViewDidLoad`, vytvoření `VNDetectFaceRectanglesRequest` odkazující `HandleRectangles` metoda, která bude volána na konci každého požadavku.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. Spuštění zpracování vize

Následující kód spustí zpracování požadavku. V **VisionFaces** ukázkový kód spustí po uživatel vybral obrázku:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Tato obslužná rutina předá `ciImage` Framework vize `VNDetectFaceRectanglesRequest` který byl vytvořen v kroku 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Zpracování výsledků vize zpracování

Po dokončení detekce vzhled obslužná rutina spustí `HandleRectangles` metodu, která provádí zpracování chyb a zobrazí hranice zjištěné řezy a volání `OverlayRectangles` kreslení obdélníků ohraničující na původní obrázek:

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

### <a name="4-display-the-results"></a>4. Zobrazit výsledky

`OverlayRectangles` Metoda v **VisionFaces** ukázka má tři funkce:

- Vykreslování se zdrojovou bitovou kopií,
- Kreslení obdélníků pro každý řez detekuje, a
- Přidání textový popisek pro každý řez pomocí CoreGraphics.

Zobrazení [na ukázkový zdroj](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) pro konkrétní metody CoreGraphics.

![Nešlo se dvěma řezy zjistil](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Další zpracování

Rozhraní framework vize zahrnuje další funkce pro zjištění funkce rozpoznávání obličeje, jako je například očí a úst. Použití `VNDetectFaceLandmarksRequest` typu, který vrátí `VNFaceObservation` výsledky jako v kroku 3 výše, ale s další `VNFaceLandmark` data.


## <a name="related-links"></a>Související odkazy

- [Vize obdélníků (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [Vize řezy (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Pokroky v základní Image - filtry, operačního systému, vize a více (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/510/)
