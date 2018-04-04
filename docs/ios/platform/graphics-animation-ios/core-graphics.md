---
title: Základní grafiky
description: Tento článek popisuje architektury iOS základní grafické prvky. Ukazuje, jak používat základní grafiky k vykreslení geometry, Image a soubory PDF.
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: e7b28ae8014928d82628bd8069d30ca88a4be05f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="core-graphics"></a>Základní grafiky

_Tento článek popisuje architektury iOS základní grafické prvky. Ukazuje, jak používat základní grafiky k vykreslení geometry, Image a soubory PDF._

zahrnuje iOS [ *základní grafické prvky* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) framework k podpoře nízké úrovně kreslení. Co povolit bohaté grafické možnosti v rámci UIKit jsou tyto rozhraní. 

Základní grafické prvky je nízké úrovně 2D grafické rozhraní, které umožňuje kreslení nezávislé grafiky zařízení. Kreslení v UIKit všechny 2D interně používá základní grafické prvky.

Kreslení v různých scénářích včetně podporuje grafiky jádra:

-  [Kreslení na obrazovku prostřednictvím `UIView` ](#Drawing_in_a_UIView_Subclass) .
-  [Vykreslování obrázků v paměti nebo na obrazovce](#Drawing_Images_and_Text).
-  Vytváření a kreslení do PDF.
-  Čtení a kreslení existujícího PDF.


## <a name="geometric-space"></a>Geometrickou místa

Bez ohledu na to scénář všechny kreslení provést s základní grafiky provádí v geometrické místa, což znamená, že funguje v abstraktní bodů namísto pixelů. Popisují, co chcete vykresleného z hlediska geometrie a kreslení stavu například barvy, styly řádku atd. a zpracovává základní grafické prvky překladu všechno, co do pixelů. Takový stav vkládá grafiky kontext, který si můžete představit jako plátno kopírovat.

Existuje několik výhod tohoto přístupu:

-  Kreslení kódu se stane dynamické a následně můžete upravit grafiky za běhu.
-  Snižuje potřebu statických bitových kopií v sadě aplikací můžete snížit velikost aplikace.
-  Grafika stát odolnější vůči změny rozlišení mezi zařízeními.

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>Kreslení v UIView podtřídy

Každý `UIView` má `Draw` metoda, která je volána v systému, když je potřeba vykreslit. Přidat zobrazení, kreslení kód podtřídami `UIView` a přepsání `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Kreslení by měla být volána nikdy přímo. V systému je volána při zpracování spuštění smyčky. Při prvním prostřednictvím spuštění smyčky po přidání zobrazení do zobrazení hierarchie, jeho `Draw` metoda je volána. Následující volání `Draw` dojít, když zobrazení je označen jako museli být vykresleny voláním buď `SetNeedsDisplay` nebo `SetNeedsDisplayInRect` v zobrazení.

### <a name="pattern-for-graphics-code"></a>Vzor pro kód grafiky

Kód `Draw` implementace by měl popisují, co chce vykresleného. Kreslení kód pracuje podle vzoru, ve kterém nastaví některé kreslení stavu a volá metodu pro žádosti, je nutné vykreslit. Tento vzor může být zobecněn následujícím způsobem:

1. Získáte grafiky kontextu.

2. Nastavte atributy kreslení.

3. Vytvořte některé geometrie z kreslení primitiv.

4. Volání metody kreslení nebo tahu.

### <a name="basic-drawing-example"></a>Příklad základní kreslení

Zvažte například následující fragment kódu:

```csharp
//get graphics context
using (CGContext g = UIGraphics.GetCurrentContext ()) {
            
    //set up drawing attributes
    g.SetLineWidth (10);
    UIColor.Blue.SetFill ();
    UIColor.Red.SetStroke ();

    //create geometry
    var path = new CGPath ();

    path.AddLines (new CGPoint[]{
    new CGPoint (100, 200),
    new CGPoint (160, 100), 
    new CGPoint (220, 200)});

    path.CloseSubpath ();

    //add geometry to graphics context and draw it
    g.AddPath (path);       
    g.DrawPath (CGPathDrawingMode.FillStroke);
}
```

Pojďme rozdělíme tento kód:

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
V řádku nejdřív získá aktuální kontext grafiky, který má použít pro kreslení. Si můžete představit kontextu grafiky jako na plátno, kreslení se stane, obsahující všechny stavy o výkresu, jako je například tahu a barvy výplně, jakož i geometrie k vykreslení.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

Po získání kontextu grafiky kód nastaví některých atributů pro použití při kreslení uvedené výše. V takovém případě se nastavují šířku tahu a výplně barvy řádku. Všechny následné kreslení pak bude používat tyto atributy, protože jsou zachována ve stavu grafiky kontextu.

K vytvoření geometrie kód používá `CGPath`, což umožňuje cesty grafiky popsat z čar a křivek. V takovém případě cesta přidá řádky pole bodů tvoří trojúhelníku připojování. Podle následující používá grafiky základní tabulky souřadnicový systém zobrazení kreslení, jejichž zdrojem je v levém horním kladné x-Direct kladnou y směrem dolů a doprava:

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

Jakmile je cesta vytvořena, je přidán do kontextu grafiky tak, aby volání `AddPath` a `DrawPath` v uvedeném pořadí můžete kreslení ho.

Výsledné zobrazení je zobrazena níže:

 ![](core-graphics-images/00-bluetriangle.png "Ukázka výstupu trojúhelníku")

## <a name="creating-gradient-fills"></a>Vytvoření přechodu výplně

Širší formy kreslení jsou k dispozici. Například základní grafické prvky umožňuje vytvoření přechodu výplně a použití ořezové cesty. Kreslení přechodu výplně uvnitř cesty z předchozího příkladu, nejprve cestu je třeba nastavit jako cestu výstřižek:

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

Nastavení aktuální cesty jako cesty výstřižek omezí všechny následné kreslení v geometrické cesty, jako je například následující kód, který nevykresluje lineárního přechodu:

```csharp
// the color space determines how Core Graphics interprets color information
    using (CGColorSpace rgb = CGColorSpace.CreateDeviceRGB()) {
        CGGradient gradient = new CGGradient (rgb, new CGColor[] {
        UIColor.Blue.CGColor,
        UIColor.Yellow.CGColor
    });

// draw a linear gradient
    g.DrawLinearGradient (
        gradient, 
        new CGPoint (path.BoundingBox.Left, path.BoundingBox.Top), 
        new CGPoint (path.BoundingBox.Right, path.BoundingBox.Bottom), 
        CGGradientDrawingOptions.DrawsBeforeStartLocation);
    }
```

Tyto změny vytvoření přechodu výplně, jak je uvedeno níže:

 ![](core-graphics-images/01-gradient-fill.png "Příklad s přechodu výplně")

## <a name="modifying-line-patterns"></a>Úpravy vzorů řádku

Atributy kreslení čar můžete také změnit základní grafické prvky. To zahrnuje, změnit barvu čáry šířky a tahu, jakož i vzoru řádku samostatně, jak je vidět v následujícím kódu:

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

Přidávání tento kód před kreslení výsledky operace v jednotkách přerušované tahy 10 dlouhý a s 4 jednotky mezery mezi pomlčky, jak je uvedeno níže:

 ![](core-graphics-images/02-dashed-stroke.png "Přidání tohoto kódu před kreslení výsledky operace v přerušované tahy")
 
Všimněte si, že při použití unifikované API v Xamarin.iOS, typ pole, je potřeba `nfloat`a také musí být explicitně převést na Math.PI.

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>Kreslení obrázky a Text

Kromě kreslení cest v kontextu zobrazení grafiky, základní grafické prvky také podporuje vykreslení obrázky a text. Kreslení obrázku, stačí vytvořit `CGImage` a předejte jej `DrawImage` volání:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

To však vytvoří bitovou kopii vykreslovat opačně, jak je uvedeno níže:

 ![](core-graphics-images/03-upside-down-monkey.png "Obrázek vykreslovat obráceně")

Důvodem je základní grafické prvky počátek kreslení obrázku je v levém dolním při zobrazení má původ v levé horní části. Proto ke správnému zobrazení bitovou kopii, počátek má být změněn, jehož lze dosáhnout úpravou *aktuální transformační matice* *(CTM)*. CTM definuje, kde body za provozu, také známé jako *uživatele místo*. Převrácení CTM ve směru osy y a posunem podle výšky hranice ve směru osy y záporné můžete překlopit bitovou kopii.

Kontext grafiky obsahuje pomocné metody pro transformaci CTM. V takovém případě `ScaleCTM` "převrátí" kreslení a `TranslateCTM` přesouvá vlevo nahoře, jak je uvedeno níže:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
    
    using (CGContext g = UIGraphics.GetCurrentContext ()) {

        // scale and translate the CTM so the image appears upright
        g.ScaleCTM (1, -1);
        g.TranslateCTM (0, -Bounds.Height);
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
}   
```

Výsledný obraz se následně zobrazí výšku:

 ![](core-graphics-images/04-upright-monkey.png "Svisle obrázku zobrazovaného ukázka")

> [!IMPORTANT]
> Změny v kontextu grafiky platí pro všechny následné kreslení operace. Proto je převede CTM, bude mít vliv žádné další kreslení. Například pokud jste po transformaci CTM obrázek trojúhelníku, by se zobrazilo obráceně.

### <a name="adding-text-to-the-image"></a>Přidávání textu do bitové kopie

Jako s cesty a bitové kopie, zahrnuje kreslení textu pomocí základní grafické prvky stejné základní vzor nastavení některé grafiky stavu a voláním metody k vykreslení. V případě text, metoda pro zobrazení textu je `ShowText`. Při přidání do bitové kopie kreslení příklad, následující kód nakreslí některé textu s použitím základní grafické prvky:

```csharp
public override void Draw (RectangleF rect)
{
    base.Draw (rect);
    
    // image drawing code omitted for brevity ...

    // translate the CTM by the font size so it displays on screen
    float fontSize = 35f;
    g.TranslateCTM (0, fontSize);

    // set general-purpose graphics state
    g.SetLineWidth (1.0f);
    g.SetStrokeColor (UIColor.Yellow.CGColor);
    g.SetFillColor (UIColor.Red.CGColor);
    g.SetShadow (new CGSize (5, 5), 0, UIColor.Blue.CGColor);

    // set text specific graphics state
    g.SetTextDrawingMode (CGTextDrawingMode.FillStroke);
    g.SelectFont ("Helvetica", fontSize, CGTextEncoding.MacRoman);

    // show the text
    g.ShowText ("Hello Core Graphics");
}
```

Jak vidíte, nastavení stavu grafiky pro vykreslování textu je podobná kreslení geometrie. Pro text kreslení ale, jsou také použít vykreslování režim a písmo textu. V takovém případě stín platí také, i když použití stínů funguje stejně pro cestu kreslení.

Výsledný text se zobrazí s bitovou kopii, jak je uvedeno níže:

 ![](core-graphics-images/05-text-on-image.png "Výsledný text se zobrazí Image")

## <a name="memory-backed-images"></a>Zálohovaná paměti bitové kopie

Kromě kreslení na kontext zobrazení grafiky zálohovaný podporuje základní grafické prvky kreslení paměti bitové kopie, také známé jako kreslení mimo obrazovku. Díky tomu vyžaduje:

-  Vytvoření kontextu grafiky, který je zálohovaný díky v paměti rastrového obrázku
-  Nastavení kreslení stavu a vydání příkazy pro kreslení
-  Získávání bitovou kopii z kontextu
-  Odebrání kontextu


Na rozdíl od `Draw` metoda, kde poskytuje kontext zobrazení, v tomto případě vytvoříte kontextu v jednom ze dvou způsobů:

1. Při volání `UIGraphics.BeginImageContext` (nebo `BeginImageContextWithOptions`)

2. Vytvořením nového `CGBitmapContextInstance`

 `CGBitmapContextInstance` je užitečné, když pracujete přímo s bity bitové kopie, například pro případy, kdy používáte se vlastní image manipulaci s algoritmem. Ve všech ostatních případech, měli byste použít `BeginImageContext` nebo `BeginImageContextWithOptions`.

Až budete mít kontextu obrázku, přidání kódu kreslení je stejně, jako je v `UIView` podtřídy. Například příklad kódu dříve použitá k vykreslení trojúhelník slouží k vykreslení na bitovou kopii v paměti místo v `UIView`, jak je uvedeno níže:

```csharp
UIImage DrawTriangle ()
{
    UIImage triangleImage;

    //push a memory backed bitmap context on the context stack
    UIGraphics.BeginImageContext (new CGSize (200.0f, 200.0f));

    //get graphics context
    using(CGContext g = UIGraphics.GetCurrentContext ()){

        //set up drawing attributes
        g.SetLineWidth(4);
        UIColor.Purple.SetFill ();
        UIColor.Black.SetStroke ();

        //create geometry
        path = new CGPath ();

        path.AddLines(new CGPoint[]{
            new CGPoint(100,200),
            new CGPoint(160,100), 
            new CGPoint(220,200)});

        path.CloseSubpath();

        //add geometry to graphics context and draw it
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);

        //get a UIImage from the context
        triangleImage = UIGraphics.GetImageFromCurrentImageContext ();
    }
    
    return triangleImage;
}
```

Běžně se používají kreslení na bitmapu zálohovaná paměti je pro zachycení image z libovolného `UIView`. Například následující kód vykreslí vrstvy zobrazení na kontext rastrového obrázku a vytvoří `UIImage` z něj:

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Kreslení soubory PDF

Kromě bitové kopie podporuje základní grafické prvky kreslení PDF. Jako obrázky, vám může vykreslit PDF v paměti, stejně jako čtení souborů PDF pro vykreslování v `UIView`.

### <a name="pdf-in-a-uiview"></a>PDF v UIView

Základní grafické prvky také podporuje čtení ze souboru PDF a vykreslování v zobrazení pomocí `CGPDFDocument` třídy. `CGPDFDocument` Třída reprezentuje PDF v kódu a slouží ke čtení a vykreslení stránky.

Například následující kód v `UIView` podtřídami čtení ze souboru do PDF `CGPDFDocument`:

```csharp
public class PDFView : UIView
{
    CGPDFDocument pdfDoc;

    public PDFView ()
    {
        //create a CGPDFDocument from file.pdf included in the main bundle
        pdfDoc = CGPDFDocument.FromFile ("file.pdf");
    }
  
     public override void Draw (Rectangle rect)
    {
        ...
    }
}
```

`Draw` Metoda pak můžete použít `CGPDFDocument` k načtení stránky do `CGPDFPage` a vykreslit ho voláním `DrawPDFPage`, jak je uvedeno níže:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
        
    //flip the CTM so the PDF will be drawn upright
    using (CGContext g = UIGraphics.GetCurrentContext ()) {
        g.TranslateCTM (0, Bounds.Height);
        g.ScaleCTM (1, -1);
        
        // render the first page of the PDF
        using (CGPDFPage pdfPage = pdfDoc.GetPage (1)) {
            
        //get the affine transform that defines where the PDF is drawn
        CGAffineTransform t = pdfPage.GetDrawingTransform (CGPDFBox.Crop, rect, 0, true);
        
        //concatenate the pdf transform with the CTM for display in the view
        g.ConcatCTM (t);
        
        //draw the pdf page
        g.DrawPDFPage (pdfPage);
        }
    }
}
```

### <a name="memory-backed-pdf"></a>Paměť záložních souborů PDF

Pro v paměti PDF, je potřeba vytvořit PDF kontextu voláním `BeginPDFContext`. Kreslení do PDF je granulární na stránky. Každé stránce spuštění voláním `BeginPDFPage` a dokončí voláním `EndPDFContent`, pomocí grafického kódu v rozmezí. Také jako s kreslení obrázku paměti zálohovaný PDF kreslení používá počátek v levém dolním, který může být účtována změnou CTM stejně jako s obrázky.

Následující kód ukazuje, jak kreslení textu do PDF:

```csharp
//data buffer to hold the PDF
NSMutableData data = new NSMutableData ();

//create a PDF with empty rectangle, which will configure it for 8.5x11 inches
UIGraphics.BeginPDFContext (data, CGRect.Empty, null);

//start a PDF page
UIGraphics.BeginPDFPage ();
       
using (CGContext g = UIGraphics.GetCurrentContext ()) {
    g.ScaleCTM (1, -1);
    g.TranslateCTM (0, -25);      
    g.SelectFont ("Helvetica", 25, CGTextEncoding.MacRoman);
    g.ShowText ("Hello Core Graphics");
    }
    
//complete a PDF page
UIGraphics.EndPDFContent ();
```

Výsledný text vykreslením do PDF, který je pak obsažené v `NSData` , lze uložit, nahrané, poslaného e-mailem atd.


## <a name="summary"></a>Souhrn

V tomto článku jsme se podívali na možnosti grafiky poskytované prostřednictvím *základní grafické prvky* framework. Jsme viděli, jak používat základní grafiky k vykreslení geometry, Image a soubory PDF v kontextu `UIView,` stejně jako na záložních paměti grafiky kontexty.

## <a name="related-links"></a>Související odkazy

- [Ukázka základní grafiky](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Grafika a návod animace](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Core Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Základní animace recepty](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
