---
title: Úvod do CoreML v Xamarin.iosu
description: Tento dokument popisuje CoreML, umožňující strojového učení v systému iOS. Tento dokument popisuje, jak začít pracovat s CoreML a jak pomocí rozhraní pro zpracování obrazu.
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: 13178d4530e3214c6cf31c1018b21815ccd2227f
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350681"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Úvod do CoreML v Xamarin.iosu

CoreML přináší strojového učení pro iOS – aplikace můžete využít výhod modelů vyškolených strojového učení k provádění nejrůznější úlohy, od problémů k rozpoznávání obrazu.

Tento úvod popisuje následující:

- [Začínáme s CoreML](#coreml)
- [Pomocí rozhraní pro zpracování obrazu CoreML](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>Začínáme s CoreML

Tyto kroky popisují, jak přidat CoreML na projekt iOS. Odkazovat [Mars Habitat Pricer ukázka](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) praktické příklad.

![Snímek obrazovky ukázkové prediktivní cena Habitat MARS](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1. Do projektu přidejte modelů CoreML

Přidání modelu CoreML (soubor s **.mlmodel** rozšíření) k **prostředky** adresáře projektu. 

Ve vlastnostech souboru modelu jeho **akce sestavení** je nastavena na **CoreMLModel**. To znamená, že se zkompiluje do **.mlmodelc** souboru, když je aplikace sestavená.

### <a name="2-load-the-model"></a>2. Načíst model

Načtení pomocí modelu `MLModel.Create` statickou metodu:

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. Nastavit parametry

Parametry modelu jsou předávány dovnitř a ven pomocí třídou kontejneru, která implementuje `IMLFeatureProvider`.

Třídy zprostředkovatele funkce chovají se jako slovník řetězce a `MLFeatureValue`s, kde každá hodnota funkce může být jednoduchý řetězec nebo číslo, pole nebo data nebo pixel vyrovnávací paměť obsahující bitovou kopii.

Kód pro funkce s jednou hodnotou zprostředkovatele je zobrazena níže:

```csharp
public class MyInput : NSObject, IMLFeatureProvider
{
  public double MyParam { get; set; }
  public NSSet<NSString> FeatureNames => new NSSet<NSString>(new NSString("myParam"));
  public MLFeatureValue GetFeatureValue(string featureName)
  {
    if (featureName == "myParam")
      return MLFeatureValue.FromDouble(MyParam);
    return MLFeatureValue.FromDouble(0); // default value
  }
```

Použití tříd tímto způsobem, vstupní parametry lze zadat tak, aby je srozumitelné CoreML. Názvy funkcí (jako například `myParam` v příkladu kódu) musí odpovídat hodnotám modelu očekává.

### <a name="4-run-the-model"></a>4. Spustit model

Pomocí modelu vyžaduje, aby se zprostředkovatel funkce má být vytvořena a parametry nastaven, pak, která `GetPrediction` volat metodu:

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. Extrahovat výsledky

Předpověď výsledků `outFeatures` je také instancí `IMLFeatureProvider`; výstupní hodnoty lze přistupovat pomocí `GetFeatureValue` pomocí názvu každého výstupního parametru (například `theResult`), jako v tomto příkladu:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>Pomocí rozhraní pro zpracování obrazu CoreML

CoreML lze také použít ve spojení s rozhraní pro zpracování obrazu k provádění operací na bitovou kopii, jako je například rozpoznávání tvar, identifikace objektů a další úlohy.

Níže uvedené kroky popisují, jak CoreML a zpracování obrazu se používají společně v [CoreMLVision ukázka](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/). Ukázka kombinuje [obdélníky rozpoznávání](~/ios/platform/introduction-to-ios11/vision.md#rectangles) z rozhraní pro zpracování obrazu pomocí _MNINSTClassifier_ modelů CoreML k identifikaci rukou psaný číslici v fotografie.

![Rozpoznávání obrazu čísla 3](coreml-images/vision3.png) ![Rozpoznávání obrazu čísla 5](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. Vytvoření modelu zpracování obrazu CoreML

Modelů CoreML _MNISTClassifier_ je načten a pak je obalen `VNCoreMLModel` které zpřístupňuje model pro úkoly pro zpracování obrazu. Tento kód také vytvoří dva požadavky pro zpracování obrazu: nejprve pro vyhledání obdélníků v obrázku a potom pro zpracování obdélníku s modelem CoreML:

```csharp
// Load the ML model
var bundle = NSBundle.MainBundle;
var assetPath = bundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
NSError mlErr, vnErr;
var mlModel = MLModel.Create(assetPath, out mlErr);
var model = VNCoreMLModel.FromMLModel(mlModel, out vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(model, HandleClassification);
```

Stále musí implementovat třídu `HandleRectangles` a `HandleClassification` metody pro zpracování obrazu požadavky, zobrazí v krocích 3 a 4 níže.

### <a name="2-start-the-vision-processing"></a>2. Spuštění zpracování pro zpracování obrazu

Následující kód spustí zpracování požadavku. V **CoreMLVision** ukázku, tento kód se spustí poté, co uživatel vybral obrázku:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Tato obslužná rutina předá `ciImage` Framework pro zpracování obrazu `VNDetectRectanglesRequest` , který byl vytvořen v kroku 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Zpracování výsledků zpracování pro zpracování obrazu

Po dokončení detekce obdélník se provede `HandleRectangles` metodu, která ořízne obrázek, který se extrahuje prvních obdélníku, převede obdélník obrázku do odstínů šedi a předá ho do modelů CoreML pro klasifikaci.

`request` Parametr předaný této metodě obsahuje podrobnosti o požadavku pro zpracování obrazu a použití `GetResults<VNRectangleObservation>()` metody, vrátí seznam obdélníky nalezen na obrázku. První obdélník `observations[0]` se extrahují a předat modelů CoreML:

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] {ClassificationRequest}, out NSError err);
  });
}
```

`ClassificationRequest` Byl inicializován v kroku 1 použít `HandleClassification` metody definované v dalším kroku.

### <a name="4-handle-the-coreml"></a>4. Popisovač CoreML

`request` Parametr předaný této metodě obsahuje podrobnosti o požadavku CoreML a pomocí `GetResults<VNClassificationObservation>()` metody, vrátí seznam možných výsledky seřazené podle spolehlivosti (nejvyšší spolehlivosti první):

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  // ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```

## <a name="samples"></a>Ukázky kódu

Existují tři CoreML vzorky vyzkoušet:

* [Ukázka prediktivní cena Habitat Mars](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) má jednoduché číselné vstupy a výstupy.

* [Vision a CoreML ukázka](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/) přijímá parametr image a používá rozhraní pro zpracování obrazu k identifikaci Čtvereček oblasti na obrázku, ve kterých jsou předány CoreML modelu, který rozpozná jednotné číslic.

* Nakonec [rozpoznávání obrazu CoreML ukázka](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/) CoreML používá k identifikaci funkce fotografie. Ve výchozím nastavení používá menší **SqueezeNet** model (5MB), ale bylo zapsáno tak, aby si můžete stáhnout a začlenit větší **VGG16** modelu (553 MB). Další informace najdete v tématu [vzorku readme](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md).

## <a name="related-links"></a>Související odkazy

- [Ve službě Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [Příklad CoreML (Mars Habitat) (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML a zpracování obrazu (číslo rozpoznávání) (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [Rozpoznávání obrazu CoreML (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [CoreML s Azure Custom Vision (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [Představujeme CoreML (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/703/)
