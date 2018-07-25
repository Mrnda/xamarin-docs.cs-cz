---
title: Základní Image Xamarin.iOS
description: Základní Image je novou architekturou zavedena v systému iOS 5 a poskytují zpracování obrázků a živého videa vylepšení funkcí. Tento článek přináší tyto funkce pomocí ukázky Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 7af57856079813e8cb1831a7f22a0a098a6be771
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242163"
---
# <a name="core-image-in-xamarinios"></a>Základní Image Xamarin.iOS

_Základní Image je novou architekturou zavedena v systému iOS 5 a poskytují zpracování obrázků a živého videa vylepšení funkcí. Tento článek přináší tyto funkce pomocí ukázky Xamarin.iOS._

Základní Image je novou architekturou zavedená v systému iOS 5, která poskytuje řadu předdefinovaných filtrů a efekty, které chcete použít pro obrázky a videa, včetně rozpoznávání tváře.

Tento dokument obsahuje jednoduché příklady:

-  Rozpoznávání tváře.
-  Použití filtrů pro bitovou kopii
-  Seznam dostupných filtrů.


Tyto příklady by vám pomůžou začít začlenění funkcí základní Image do aplikace Xamarin.iOS.

## <a name="requirements"></a>Požadavky

Musíte použít nejnovější verzi Xcode.

## <a name="face-detection"></a>Rozpoznávání tváře

Funkce rozpoznávání tváře základní Image nemá právě co stavu – se pokusí identifikovat fotografie tváře a vrací souřadnice žádné tváře, které rozpozná. Tyto informace je možné zjistit počet uživatelů v obrázku, nakreslete ukazatele na obrázku (např.) "přidáváním značek" lidé v fotografie), nebo cokoli jiného si můžete představit.

Tento kód z CoreImage\SampleCode.cs ukazuje, jak vytvořit a používat rozpoznávání tváře na vložený obrázek:

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

Funkce pole se vyplní `CIFaceFeature` objekty (pokud nebyly zjištěny žádné tváře). Je `CIFaceFeature` pro každou plochu. `CIFaceFeature` má následující vlastnosti:

-  HasMouthPosition – Určuje, zda byl zjištěn přidržte pro tento rozpoznávání tváře.
-  HasLeftEyePosition – Určuje, zda byl zjištěn levého oka pro tento rozpoznávání tváře.
-  HasRightEyePosition – Určuje, zda byl zjištěn pravého pro tento rozpoznávání tváře. 
-  MouthPosition – souřadnice ústí pro tento rozpoznávání tváře.
-  LeftEyePosition – souřadnice levého oka pro tento rozpoznávání tváře.
-  RightEyePosition – souřadnice pravého oka pro tuto plošku.


Souřadnice pro všechny tyto vlastnosti mají původ v levé dolní – na rozdíl od UIKit, který používá jako původ vlevo nahoře. Při použití souřadnice na `CIFaceFeature` nezapomeňte "překlopit". Toto zobrazení velmi základní vlastní image v CoreImage\CoreImageViewController.cs ukazuje, jak nakreslit obrázek trojúhelníky "indikátor pro rozpoznávání tváře. (Poznámka: `FlipForBottomOrigin` metoda):

```csharp
public class FaceDetectImageView : UIView
{
    public Xamarin.iOS.CoreImage.CIFeature[] Features;
    public UIImage Image;
    public FaceDetectImageView (RectangleF rect) : base(rect) {}
    CGPath path;
    public override void Draw (RectangleF rect) {
        base.Draw (rect);
        if (Image != null)
            Image.Draw(rect);

        using (var context = UIGraphics.GetCurrentContext()) {
            context.SetLineWidth(4);
            UIColor.Red.SetStroke ();
            UIColor.Clear.SetFill ();
            if (Features != null) {
                foreach (var feature in Features) { // for each face
                    var facefeature = (CIFaceFeature)feature;
                    path = new CGPath ();
                    path.AddLines(new PointF[]{ // assumes all 3 features found
                        FlipForBottomOrigin(facefeature.LeftEyePosition, 200),
                        FlipForBottomOrigin(facefeature.RightEyePosition, 200),
                        FlipForBottomOrigin(facefeature.MouthPosition, 200)
                    });
                    path.CloseSubpath();
                    context.AddPath(path);
                    context.DrawPath(CGPathDrawingMode.FillStroke);
                }
            }
        }
    }
    /// <summary>
    /// Face recognition coordinates have their origin in the bottom-left
    /// but we are drawing with the origin in the top-left, so "flip" the point
    /// </summary>
    PointF FlipForBottomOrigin (PointF point, int height)
    {
        return new PointF(point.X, height - point.Y);
    }
}
```

Potom v souboru SampleCode.cs image a funkce jsou přiřazeny předtím, než se překreslí image:

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

Snímek obrazovky ukazuje příklad výstupu: umístění zjištěných funkce rozpoznávání obličeje se zobrazuje UITextView a vykreslit do zdrojového obrázku pomocí CoreGraphics.

Vzhledem ke způsobu rozpoznávání obličeje funguje to příležitostně zjistí věci kromě lidské tváře (např. Tyto opice slonovi!).

## <a name="filters"></a>Filtry

Existuje více než 50 různými integrované filtry a rozhraní je možné rozšířit tak, že je možné implementovat nové filtry.

## <a name="using-filters"></a>Pomocí filtrů

Použití filtrování podle obrázku má čtyři různé kroky: načtení obrázku, vytváření filtru, použití filtru a uložení (nebo zobrazení) výsledek.

Nejdřív načtěte do image `CIImage` objektu.

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

Za druhé vytvořte třídu filtr a nastavte jeho vlastnosti.

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

Třetí, přístup `OutputImage` vlastnosti a volání `CreateCGImage` metoda k vykreslení konečný výsledek.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

Nakonec přiřadíte bitovou kopii k zobrazení, abyste viděli výsledek. V reálné aplikaci může uložit výsledná bitová kopie systému souborů, fotoalba, Tweetu nebo e-mailu.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

Tyto snímky obrazovky ukazují výsledek `CISepia` a `CIHueAdjust` filtry, které je ukázán v CoreImage.zip ukázkový kód.

Najdete v článku [jasu recept Image a upravit smlouvu](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) příklad `CIColorControls` filtru.

```csharp
var uiimage = UIImage.FromFile("photo.JPG");
var ciimage = new CIImage(uiimage);
var hueAdjust = new CIHueAdjust();   // first filter
hueAdjust.Image = ciimage;
hueAdjust.Angle = 2.094f;
var sepia = new CISepiaTone();       // second filter
sepia.Image = hueAdjust.OutputImage; // output from last filter, input to this one
sepia.Intensity = 0.3f;
CIFilter color = new CIColorControls() { // third filter
    Saturation = 2,
    Brightness = 1,
    Contrast = 3,
    Image = sepia.OutputImage    // output from last filter, input to this one
};
var output = color.OutputImage;
var context = CIContext.FromOptions(null);
// ONLY when CreateCGImage is called do all the effects get rendered
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

```csharp
var context = CIContext.FromOptions (null);
```

```csharp
var context = CIContext.FromOptions(new CIContextOptions() {
    UseSoftwareRenderer = true  // CPU
});
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

### <a name="listing-filters-and-their-properties"></a>Seznam filtrů a jejich vlastnosti

Tento kód z CoreImage\SampleCode.cs vypíše úplný seznam předdefinovaných filtrů a jejich parametry.

```csharp
var filters = CIFilter.FilterNamesInCategories(new string[0]);
foreach (var filter in filters){
   display.Text += filter +"\n";
   var f = CIFilter.FromName (filter);
   foreach (var key in f.InputKeys){
     var attributes = (NSDictionary)f.Attributes[new NSString(key)];
     var attributeClass = attributes[new NSString("CIAttributeClass")];
     display.Text += "   " + key;
     display.Text += "   " + attributeClass + "\n";
   }
}
```

[Referenční třída CIFilter](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) popisuje 50 integrované filtry a jejich vlastnosti. Pomocí kódu výše si můžete dotazovat tříd filtru včetně výchozí hodnoty pro parametry a maximální a minimální povolené hodnoty (které by mohly ověřit vstupy před použitím filtru).

Seznam kategorií výstup vypadá takto v simulátoru – můžete procházet seznam zobrazíte všechny filtry a jejich parametry.

 [![](introduction-to-coreimage-images/coreimage05.png "Seznam kategorií výstup vypadá takto v simulátoru")](introduction-to-coreimage-images/coreimage05.png#lightbox)

Každý filtr uvedené byl zpřístupněn jako třída v Xamarin.iOS, takže můžete také prozkoumat rozhraní API Xamarin.iOS.CoreImage v prohlížeči sestavení nebo použití automatického dokončování v sadě Visual Studio for Mac nebo Visual Studio. 

## <a name="summary"></a>Souhrn

Tento článek ukazuje, jak používat některé z nových funkcí framework základní Image systém iOS 5 jako jsou rozpoznávání tváří a použití filtrů pro bitovou kopii. Nejsou k dispozici v rámci můžete použít desítky filtry jiný obrázek.

## <a name="related-links"></a>Související odkazy

- [Základní Image (ukázka)](https://developer.xamarin.com/samples/CoreImage/)
- [Nastavte smlouvy a jas recept obrázek](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [Pomocí základní Image filtry](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [Referenční třída CIFilter](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
