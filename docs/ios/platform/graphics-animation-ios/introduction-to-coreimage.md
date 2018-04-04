---
title: CoreImage
description: CoreImage je novou architekturou zavedena v systému iOS 5 a zadejte zpracování obrázků a live video vylepšení funkce. Tento článek představuje tyto funkce o ukázky Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 0bb2c3b8b563da53e432ad16e6518ada67a4655e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="coreimage"></a>CoreImage

_CoreImage je novou architekturou zavedena v systému iOS 5 a zadejte zpracování obrázků a live video vylepšení funkce. Tento článek představuje tyto funkce o ukázky Xamarin.iOS._

CoreImage je novou architekturou, zavedená v iOS 5, který poskytuje řadu předdefinovaných filtrů a efekty uplatňovat na obrázky a videa, včetně detekce řez.

Tento dokument obsahuje jednoduché příklady, jak:

-  Čelí detekce.
-  Použití filtrů pro bitovou kopii
-  Výpis k dispozici tyto filtry.


Tyto příklady by vám pomůžou začít začlenění CoreImage funkce do aplikace Xamarin.iOS.

## <a name="requirements"></a>Požadavky

Musíte použít nejnovější verzi Xcode.

## <a name="face-detection"></a>Vzhled detekce

Funkce rozpoznávání vzhled CoreImage neobsahuje právě co zobrazuje – se pokusí identifikovat tyto řezy v fotografie a vrátí souřadnice žádné řezy, které ji rozpoznává. Tyto informace lze zjistit, kolik lidí do image, kreslení ukazatele na bitovou kopii (např. pro "označování, osoby v fotografie), nebo cokoliv jiného, si můžete představit.

Tento kód z CoreImage\SampleCode.cs ukazuje, jak vytvořit a použít vzhled detekce na vložený obrázek:

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

Vyplní pole funkce s `CIFaceFeature` objekty (pokud nebyly zjištěny žádné řezy). Došlo `CIFaceFeature` pro každý řez. `CIFaceFeature` má následující vlastnosti:

-  HasMouthPosition – jestli byla zjištěna úst Tento řez.
-  HasLeftEyePosition – jestli byla zjištěna levého oka Tento řez.
-  HasRightEyePosition – jestli byla zjištěna pravého Tento řez. 
-  MouthPosition – souřadnice úst pro tento řez.
-  LeftEyePosition – souřadnice levého oka pro tento řez.
-  RightEyePosition – souřadnice správné oko pro tento řez.


Souřadnice pro tyto vlastnosti mají původ v levé dolní – na rozdíl od UIKit, který používá levém horním jako počátek. Při použití souřadnice na `CIFaceFeature` nezapomeňte 'překlopit' je. Toto zobrazení velmi základní vlastní image ve CoreImage\CoreImageViewController.cs ukazuje, jak k vykreslení trojúhelníčky 'vzhled ukazatele, na bitovou kopii (Poznámka: `FlipForBottomOrigin` metoda):

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

Potom v souboru SampleCode.cs bitové kopie a funkce jsou přiřazeny předtím, než bude překreslen bitovou kopii:

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

Na snímku obrazovky vidíte ukázkový výstup: umístění zjištěných funkce rozpoznávání obličeje se zobrazují v UITextView a vykreslovány na zdrojové bitové kopie pomocí CoreGraphics.

Kvůli způsobu práce rozpoznávání obličeje, se někdy zjistí operace než lidské řezy (např. Tyto opice hračka!).

## <a name="filters"></a>Filtry

Existuje více než 50 různé integrované filtry a rozhraní je rozšiřitelný, tak, aby nové filtry se dají implementovat.

## <a name="using-filters"></a>Pomocí filtrů

Použití filtru pro bitovou kopii má čtyři kroky odlišné: načítání bitovou kopii, vytváření filtru, použití filtru a ukládání (nebo zobrazení) výsledek.

Nejdřív načíst bitovou kopii do `CIImage` objektu.

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

Druhý vytvořte třídu filtru a nastavit jeho vlastnosti.

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

Třetí, přístup `OutputImage` vlastnost a volání `CreateCGImage` metoda k vykreslení konečný výsledek.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

Nakonec přiřadíte bitovou kopii k zobrazení zobrazíte výsledek. V reálné aplikaci může uložit výsledný obraz systému souborů, alba fotografií, Tweet nebo e-mailu.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

Tyto snímky obrazovky zobrazit výsledek `CISepia` a `CIHueAdjust` filtry, které je ukázán v CoreImage.zip ukázkový kód.

Najdete v článku [upravit kontraktu a také Průraznost o tajný recept bitovou kopii](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) příklad `CIColorControls` filtru.

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

Tento kód z CoreImage\SampleCode.cs výstupy úplný seznam předdefinovaných filtrů a jejich parametrů.

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

[Referenci třídy CIFilter](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) popisuje 50 integrované filtry a jejich vlastnosti. Tento kód výše můžete můžete dotazovat třídy filtru, včetně výchozí hodnoty pro parametry a maximální a minimální povolené hodnoty (které by bylo možné ověřit vstupy před použitím filtru).

Seznam kategorií výstup vypadá takto v simulátoru – můžete procházet v seznamu zobrazíte všechny filtry a jejich parametrů.

 [![](introduction-to-coreimage-images/coreimage05.png "Seznam kategorií výstup vypadá takto v simulátoru")](introduction-to-coreimage-images/coreimage05.png#lightbox)

Každý filtr uvedené má byla zpřístupněná jako třídu v Xamarin.iOS, tak můžete také zkoumat rozhraní API Xamarin.iOS.CoreImage v prohlížeči sestavení nebo pomocí automatického dokončování v sadě Visual Studio pro Mac nebo Visual Studio. 

## <a name="summary"></a>Souhrn

Tento článek ukazuje, jak používat některé z nových funkcí framework CoreImage systém iOS 5 jako detekce vzhled a použití filtrů pro bitovou kopii. Nejsou k dispozici v rámci budete moci použít desítek filtry jinou bitovou kopii.

## <a name="related-links"></a>Související odkazy

- [Základní Image (ukázka)](https://developer.xamarin.com/samples/CoreImage/)
- [Upravit kontraktu a také Průraznost složení bitové kopie](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [Použití filtrů CoreImage](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [Odkaz na CIFilter – třída](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
