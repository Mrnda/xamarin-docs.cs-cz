---
title: Základní grafické prvky v Xamarin.iosu
description: Tento článek popisuje základní grafické prvky architektury iOS. Ukazuje, jak používat grafiku základní kreslení geometrie, obrázky a soubory PDF.
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 82c54074db722824c56ae3ae86620c804b8d109e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241946"
---
# <a name="core-graphics-in-xamarinios"></a>Základní grafické prvky v Xamarin.iosu

_Tento článek popisuje základní grafické prvky architektury iOS. Ukazuje, jak používat grafiku základní kreslení geometrie, obrázky a soubory PDF._

zahrnuje iOS [ *základní grafické prvky* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) framework k zajištění podpory nízké úrovně výkresu. Tyto architektury se, co povolit bohaté grafické možnosti v rámci UIKit. 

Základní grafiky je nízké úrovně 2D grafika rozhraní, která umožňuje kreslení nezávislé grafiky zařízení. Všechny 2D kreslení v Uikitu používá jádro grafiky interně.

Základní grafiky podporuje vykreslení v řadě scénářů, včetně:

-  [Kreslení na obrazovku prostřednictvím `UIView` ](#Drawing_in_a_UIView_Subclass) .
-  [Vykreslování obrázků v paměti nebo na obrazovce](#Drawing_Images_and_Text).
-  Vytváření a vykreslování do formátu PDF.
-  Čtení a kreslení existujícího souboru PDF.


## <a name="geometric-space"></a>Geometrické místa

Bez ohledu na scénář všechny kreslení provést s grafikou Core provádí v geometrické místa, to znamená, že to funguje v abstraktní body a ne v pixelech. Popište, co chcete vykresleného z hlediska geometrie a kreslení stavu, jako jsou barvy, styly řádku a podobně a základní grafické prvky zpracovává všechno, co překládá na pixelech. Takové stavu se přidá do grafiky kontext, který si můžete představit jako plátno kopírovat.

Existuje několik výhod tohoto přístupu:

-  Vykreslení kódu stane dynamické a následně upravit grafiky v době běhu.
-  Omezuje potřebu statické obrázky do sady prostředků aplikace může snížit velikost aplikace.
-  Grafika stát odolnější vůči změny rozlišení napříč zařízeními.

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>Kreslení v podtřídou UIView

Každý `UIView` má `Draw` metodu, která je volána v systému, když je potřeba vykreslit. Chcete-li přidat kód pro vykreslování na zobrazení podtřídy `UIView` a přepsat `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Draw by se nikdy volat přímo. Během zpracování spuštění smyčky je volána v systému. Při prvním průchodu spuštění smyčky po zobrazení se přidá do zobrazení hierarchie jeho `Draw` metoda je volána. Následující volání `Draw` dojít, když je zobrazení označilo jako vyžadující vykreslit pomocí volání buď `SetNeedsDisplay` nebo `SetNeedsDisplayInRect` pro zobrazení.

### <a name="pattern-for-graphics-code"></a>Vzor pro grafický kód

Kód v `Draw` implementace, měl by popisovat, co chce vystavené. Kód pro vykreslování probíhá podle vzoru, ve kterém nastaví stav některých vykreslování a volá metodu žádosti je potřeba vykreslit. Tento model můžete zobecnit následujícím způsobem:

1. Získáte kontext grafiky.

2. Nastavte atributy kreslení.

3. Vytvořte některé geometrie od kreslení primitiv.

4. Volání metody Draw nebo tah.

### <a name="basic-drawing-example"></a>Příklad základního kreslení

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

Pojďme rozdělit tento kód:

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
S tímto řádkem nejprve získá aktuální kontext grafiky má být použit pro kreslení. Si můžete představit grafiky kontextu jako na plátně, kreslení se stane, obsahující všechny stavy o výkresu, jako je například stroke a barvy výplně, jakož i geometrie, chcete-li nakreslit.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

Po získání kontextu grafiky kód nastaví některé atributy pro použití při kreslení, je uveden výše. V tomto případě jsou nastavené barvy čar šířku, protože byl zdvih a výplně. Všechny následné kreslení pak bude používat tyto atributy jsou uchovávány v kontextu grafiky stavu.

Vytvoření geometrie kód používá `CGPath`, což umožňuje cesty grafiky popsány z čar a křivek. V takovém případě přidá cestu čáry spojující body a společně tvoří trojúhelník pole. Jak se zobrazuje pod základní grafické prvky používá souřadnicový systém zobrazení kreslení, jejichž zdrojem je v levém horním rohu kladná x-Direct směru y pozitivní dolů a doprava:

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

Jakmile vytvoříte cestu, přidá se do kontextu grafiky tak, aby volání `AddPath` a `DrawPath` v uvedeném pořadí jej lze nakreslit.

Výsledné zobrazení je uveden níže:

 ![](core-graphics-images/00-bluetriangle.png "Trojúhelník ukázkový výstup")

## <a name="creating-gradient-fills"></a>Vytvoření přechodu výplně

Bohatší formy kresby jsou také k dispozici. Například základní grafické prvky umožňuje vytvoření přechodu výplně a použití ořezové cesty. Chcete-li nakreslit přechodovou výplní uvnitř cesty z předchozího příkladu, nejprve cesta musí být nastavený na ořezovou cestu:

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

Nastavení aktuální cestu jako ořezové cesty omezí všechny následné kreslení v rámci geometrické cesty, jako je například následující kód, který vykreslí lineárního přechodu:

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

Tyto změny vytvořit přechodové výplně, jak je znázorněno níže:

 ![](core-graphics-images/01-gradient-fill.png "V příkladu s přechodovou výplní")

## <a name="modifying-line-patterns"></a>Úprava řádku vzorců

Atributy kreslení čar můžete také upravit s grafikou Core. To zahrnuje změnu barvu čáry šířku a stroke, jakož i vzorek, samostatně, jak je znázorněno v následujícím kódu:

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

Přidání tohoto kódu před žádné výkresu výsledky operace v přerušované tahy 10 jednotek, long, 4 jednotky mezery mezi pomlčky, jak je znázorněno níže:

 ![](core-graphics-images/02-dashed-stroke.png "Přidání tohoto kódu před žádné výkresu výsledky operace v přerušované tahy")
 
Všimněte si, že při použití v Xamarin.iOS Unified API, typ pole musí být `nfloat`a také musí být explicitně přetypován na Math.PI.

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>Vykreslování obrázků a textu

Kromě kreslení cest v kontextu grafické zobrazení, základní grafické prvky také podporuje vykreslování obrázků a textu. Chcete-li nakreslit obrázek, jednoduše vytvořit `CGImage` a předat ho metodě `DrawImage` volání:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

Ale tímto se vytvoří bitovou kopii vykreslit vzhůru nohama, jak je znázorněno níže:

 ![](core-graphics-images/03-upside-down-monkey.png "Obrázek vykreslit vzhůru nohama")

Důvodem je, že počátek souřadných jádra pro kreslení obrázku je v levém dolním, zatímco zobrazení má původ v levém horním rohu. Proto a zobrazte obrázek správně, původ musí být upravena, jehož lze dosáhnout úpravou *aktuální transformační matice* *(CTM)*. CTM definuje, ve kterém body za provozu, označované také jako *uživatele místo*. Převrácení CTM ve směru osy y a posunu podle výšky na hranice ve směru osy y negativní přepnout na obrázku.

Kontext grafiky obsahuje pomocné metody pro transformaci CTM. V takovém případě `ScaleCTM` "převrátí" kreslení a `TranslateCTM` přesouvá na levém horním rohu, jak je znázorněno níže:

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

Výsledná bitová kopie se následně zobrazí svislé:

 ![](core-graphics-images/04-upright-monkey.png "Obrázek zobrazený sloupku vzorku")

> [!IMPORTANT]
> Změny v rámci grafiky platí pro všechny následné operace kreslení. Proto když se transformuje CTM, bude mít vliv na žádné další kreslení. Například pokud nakreslili trojúhelníku po transformaci CTM, měl by se zobrazit vzhůru nohama.

### <a name="adding-text-to-the-image"></a>Přidání textu do bitové kopie

Jako s cestami a obrázky, kreslení textu pomocí základní grafické prvky zahrnuje stejné základní vzor stav některých grafiky a volání metody, chcete-li nakreslit. V případě text, je metoda k zobrazení textu `ShowText`. Když se přidá do image kreslení příklad, následující kód vykreslí některé textu s použitím základní grafické prvky:

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

Jak je vidět, nastavení stavu grafiky pro vykreslování textu je nějak nakreslení geometrie. Pro text ale kreslení, se použijí i vykreslování režimu a písmo textu. V tomto případě stín platí také, ačkoli použití odstínů funguje stejně v případě cesty kreslení.

Výsledný text se zobrazí s imagí, jak je znázorněno níže:

 ![](core-graphics-images/05-text-on-image.png "Zobrazí se výsledný text s imagí")

## <a name="memory-backed-images"></a>Pamětí Imagí

Kromě kreslení do kontextu zobrazení grafické podporovaný Core grafiky podporuje vykreslení paměti imagí, označované také jako kreslení obrazovku. To vyžaduje:

-  Vytvoření kontextu grafiky, která je založená na v paměti rastrový obrázek
-  Nastavení vykreslení stavu a vydání příkazy pro kreslení
-  Načítání obrázku z kontextu
-  Odebrání kontextu


Na rozdíl od `Draw` metodu, pokud kontext je poskytnut pomocí zobrazení, v tomto případě vytvoříte kontextu v jednom ze dvou způsobů:

1. Voláním `UIGraphics.BeginImageContext` (nebo `BeginImageContextWithOptions`)

2. Vytvořit nový `CGBitmapContextInstance`

 `CGBitmapContextInstance` je užitečné při práci přímo s bity image, například pro případy, kdy používáte algoritmus manipulaci s vlastní image. Ve všech ostatních případech byste měli použít `BeginImageContext` nebo `BeginImageContextWithOptions`.

Jakmile budete mít objekt context obrázku, přidání vykreslení kódu je stejně jako v `UIView` podtřídy. Například v příkladu kódu předtím použili ke kreslení trojúhelník slouží k vykreslení obrázku v paměti namísto v `UIView`, jak je znázorněno níže:

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

Je běžné použití kresby na rastrový obrázek pamětí pro zachycení image z jakéhokoli `UIView`. Například následující kód vykreslí vrstva zobrazení na kontext rastrový obrázek a vytvoří `UIImage` z něj:

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Kreslení soubory PDF

Kromě bitové kopie podporuje základní grafické prvky kreslení PDF. Jako jsou obrázky, můžete můžete vykreslit PDF v paměti i čtení dokumentu PDF pro vykreslování `UIView`.

### <a name="pdf-in-a-uiview"></a>PDF v UIView

Základní grafické prvky také podporuje čtení ze souboru PDF a vykreslování v zobrazení pomocí `CGPDFDocument` třídy. `CGPDFDocument` Třída představuje PDF v kódu a slouží ke čtení a vykreslení stránky.

Například následující kód na `UIView` podtřídy čte ze souboru do formátu PDF `CGPDFDocument`:

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

`Draw` Pak můžete použít metodu `CGPDFDocument` k načtení stránky do `CGPDFPage` a vykreslit ho voláním `DrawPDFPage`, jak je znázorněno níže:

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

### <a name="memory-backed-pdf"></a>Pamětí PDF

Pro v paměti PDF, je potřeba vytvořit kontext PDF voláním `BeginPDFContext`. Kreslení do formátu PDF je detailní stránky. Spuštění každou stránku voláním `BeginPDFPage` a dokončí voláním `EndPDFContent`, pomocí grafického kódu mezi. Také jak s kreslení obrázku paměti zajišťuje PDF vykreslování používá původ v levém dolním, který může být zahrnuté úpravou CTM stejně jako s obrázky.

Následující kód ukazuje, jak vykreslení textu do formátu PDF:

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

Výsledný text je vykreslen do formátu PDF, které jsou pak obsaženy v `NSData` , který můžete uložit, nahrané, e-mailem, např.


## <a name="summary"></a>Souhrn

V tomto článku jsme se podívali na grafické funkce poskytuje prostřednictvím *základní grafické prvky* rozhraní framework. Jsme viděli, jak pomocí základní grafické prvky v rámci kontextu nakreslete geometry, obrázky a soubory PDF `UIView,` stejně jako na kontexty pamětí grafiky.

## <a name="related-links"></a>Související odkazy

- [Základní ukázka grafiky](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Grafika a animace návodu](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Core Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Základní animace recepty](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
