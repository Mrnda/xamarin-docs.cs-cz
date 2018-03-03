---
title: "Úvod do CoreML"
description: "Strojového učení pro mobilní aplikace v systému iOS 11"
ms.topic: article
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 0932130275cd037b3d4414c9c7a421dbee360eeb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-coreml"></a>Úvod do CoreML

_Strojového učení pro mobilní aplikace v systému iOS 11_

CoreML přináší machine learning k iOS – aplikace mohou využít výhod vyškolení machine learning modely k provádění nejrůznějším úloh, od řešení problémů k rozpoznávání bitové kopie.

Tento úvod obsahuje následující:

- [Začínáme s CoreML](#coreml)
- [Pomocí rozhraní vize CoreML](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>Začínáme s CoreML

Tyto kroky popisují, jak přidat CoreML do projektu iOS. Odkazovat [Pricer prostředí Mars ukázka](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) praktické příklad.

![Snímek obrazovky ukázkové předpověď ceny MARS prostředí](coreml-images/marspricer-heading.png)

### <a name="1-add-the-model-to-the-project"></a>1. Přidání modelu do projektu

Přidání kompilované modelu (adresář s **.modelc** rozšíření) k **prostředky** adresáři projektu. Obsah adresáře by měly mít akci s sestavení **BundleResource**:

![Složky zdrojů by měl obsahovat kompilované modelu](coreml-images/resources-modelc.png)

[Ukázky](https://developer.xamarin.com/samples/monotouch/ios11/) použití modelů kompilovat v Xcode 9 nebo ručně pomocí následujícího příkazu terminálu:

```csharp
xcrun coremlcompiler compile {model.mlmodel} {outputFolder}
```

> [!NOTE]
> **.model –** soubory _musí_ kompilovány do **.modelc** před použitím podle CoreML

### <a name="2-load-the-model"></a>2. Načíst model

Před použitím modelu, načíst pomocí `MLModel.FromUrl` statickou metodu:

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.FromUrl(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. Nastavte parametry

Parametry modelu jsou předaná a odhlašování pomocí třídu kontejneru, který implementuje `IMLFeatureProvider`.

Třídy zprostředkovatele funkce chovají jako slovník řetězce a `MLFeatureValue`s, kde každé funkce hodnotou může být jednoduchý řetězec nebo číslo, pole nebo data nebo vyrovnávací paměti pixelů obsahující bitovou kopii.

Kód pro zprostředkovatele funkce s jednou hodnotou je zobrazena níže:

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

Pomocí třídy jako to, vstupní parametry lze zadat tak, aby se rozumí CoreML. Názvy funkcí (například `myParam` v příkladu kódu) co očekává modelu se musí shodovat.

### <a name="4-run-the-model"></a>4. Spustit model

Pomocí modelu vyžaduje, aby se zprostředkovatel funkci chcete vytvořit instanci a parametry nastavené, potom, `GetPrediction` volat metodu:

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. Extrahování výsledky

Výsledek předpovědi `outFeatures` je také instanci `IMLFeatureProvider`; výstupní hodnoty lze přistupovat pomocí `GetFeatureValue` s názvu každého výstupního parametru (například `theResult`), protože v tomto příkladu:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>Pomocí rozhraní vize CoreML

CoreML můžete také použít ve spojení s rozhraní vize k provádění operací na bitovou kopii, jako je rozpoznávání tvaru, identifikace objektu a další úlohy.

Níže uvedené kroky popisují, jak CoreML a vize používá společně [CoreMLVision ukázka](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/). Ukázka kombinuje [obdélníků rozpoznávání](~/ios/platform/introduction-to-ios11/vision.md#rectangles) z rozhraní framework vize s _MNINSTClassifier_ CoreML modelu k identifikaci psané číslice v fotografie.

![Obrázek rozpoznávání čísla 3](coreml-images/vision3.png) ![Obrázek rozpoznávání čísla 5](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. Vytvoření modelu vize CoreML

CoreML model _MNISTClassifier_ je načíst a pak uzavřen do `VNCoreMLModel` takže modelu k dispozici pro úlohy vize. Tento kód také vytvoří dva požadavky vizi: nejdřív pro hledání obdélníky v bitovou kopii a potom pro zpracování obdélníku s modelem CoreML:

```csharp
// Load the ML model
var assetPath = NSBundle.MainBundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
var mlModel = MLModel.FromUrl(assetPath, out NSError mlErr);
var vModel = VNCoreMLModel.FromMLModel(mlModel, out NSError vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(vModel, HandleClassification);
```

Třída musí implementovat `HandleRectangles` a `HandleClassification` metody pro žádosti o vizi, zobrazí v krocích 3 a 4 níže.

### <a name="2-start-the-vision-processing"></a>2. Spuštění zpracování vize

Následující kód spustí zpracování požadavku. V **CoreMLVision** ukázku, tento kód spustí po uživatel vybral obrázku:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Tato obslužná rutina předá `ciImage` Framework vize `VNDetectRectanglesRequest` který byl vytvořen v kroku 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Zpracování výsledků vize zpracování

Po dokončení detekce obdélníku se provede `HandleRectangles` metodu, která ořízne obrázek, který se extrahuje první obdélníku, převede obrázek obdélníku stupně šedi a předává je pro model CoreML pro klasifikaci.

`request` Parametr předaná této metodě obsahuje podrobnosti o vizi požadavku a pomocí `GetResults<VNRectangleObservation>()` metoda, vrátí seznam obdélníků najít v bitové kopii. První rámeček `observations[0]` je extrahována a předaný CoreML modelu:

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] { ClassificationRequest }, out NSError err);
  });
}
```

`ClassificationRequest` Byl inicializován v kroku 1 používat `HandleClassification` metoda definované v dalším kroku.

### <a name="4-handle-the-coreml"></a>4. Zpracování CoreML

`request` Parametr předaná této metodě obsahuje podrobnosti o CoreML požadavku a pomocí `GetResults<VNClassificationObservation>()` metoda, vrátí seznam možných výsledky seřazené podle spolehlivosti (nejvyšší spolehlivosti první):

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```



## <a name="samples"></a>Ukázky kódu

Existují tři CoreML ukázky můžete vyzkoušet:

* [Předpověď ceny prostředí Mars ukázka](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) má jednoduché číselné vstupy a výstupy.

* [Vize & CoreML ukázka](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/) přijme parametrem bitové kopie a používá rozhraní vize k identifikaci odmocnina oblasti v bitovou kopii, které jsou předávány CoreML modelu, který rozpoznává jeden číslic.

* Nakonec [CoreML Image rozpoznávání ukázka](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/) CoreML používá k identifikaci funkcí v fotografie. Ve výchozím nastavení používá menší **SqueezeNet** modelu (5MB), ale bylo zapsáno tak, aby si můžete stáhnout a začlenit delší **VGG16** modelu (553 MB). Další informace najdete v tématu [tohoto příkladu readme](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md).


## <a name="related-links"></a>Související odkazy

- [Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [Příklad CoreML (Mars prostředí) (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML a zpracování obrazu (číslo rozpoznávání) (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [Rozpoznávání CoreML bitové kopie (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [CoreML s Azure vlastní vize (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [Představení CoreML (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/703/)
