---
title: Cesty a Text ve Skiasharpu
description: Tento článek zkoumá je určena průsečíkem cesty ve Skiasharpu a text a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 08/01/2017
ms.openlocfilehash: 3576af56d48eec58f3fe5fee42aef143e2edea70
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615454"
---
# <a name="paths-and-text-in-skiasharp"></a>Cesty a Text ve Skiasharpu

_Prozkoumejte je určena průsečíkem cesty a text_

V systémech moderních grafických písma textu jsou kolekce znaku jsou podrobněji popsány dále, většinou definovaný pomocí kvadratické Bézierovy křivky. Řada moderních grafických systémů v důsledku toho obsahovat zařízení, které chcete převést textové znaky do cesty grafiky.

Už víte, že je můžete vytáhnout obrysy textové znaky i vyplnit. Díky tomu můžete zobrazit tyto znaky jsou podrobněji popsány dále s šířku tahu konkrétní a dokonce i mohou mít vliv cestu, jak je popsáno v [ **efekty cest** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) článku. Je také možné převést na řetězec znaků, ale `SKPath` objektu. To znamená, že jsou podrobněji popsány dále text lze použít pro oříznutí s techniky, které jsou popsány v [ **Ořezy cestami a oblastmi** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md) článku.

Kromě použití mohou mít vliv cestu k obtažení osnovy znak, můžete také vytvořit cestu, která efekty, které jsou založeny na cesty jsou odvozeny z řetězce znaků a dokonce je možné kombinovat dva důsledky:

![](text-paths-images/pathsandtextsample.png "Vliv cestu text")

V [předchozím článku](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) jste viděli, jak [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) metoda `SKPaint` můžete získat přehled vytažené cestu. Tuto metodu můžete také pomocí cesty odvozené z znaku jsou podrobněji popsány dále.

A konečně, tento článek popisuje další průsečík cesty a text: [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) metodu `SKCanvas` umožňuje zobrazit textový řetězec tak, aby základní text následuje zakřivené cesty.

## <a name="text-to-path-conversion"></a>Text, který se cesta převodu

[ `GetTextPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetTextPath/p/System.String/System.Single/System.Single/) Metoda `SKPaint` převede řetězec znaků na `SKPath` objektu:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x` a `y` argumenty označuje počáteční bod základních hodnot v levé části textu. Hrají je stejný atribut role zde jako v `DrawText` metoda `SKCanvas`. V této cestě bude mít základních hodnot v levé části textu souřadnic (x, y).

`GetTextPath` Metoda je přehnaně, pokud chcete pouze pro výplň nebo tah Výsledná cesta. Normální `DrawText` metody umožňují udělat. `GetTextPath` Metoda je užitečnější pro další úkoly týkající se cesty.

Jednu z těchto úloh je oříznutí. **Oříznutí textu** stránka vytvoří ořezovou cestu podle jsou podrobněji popsány dále znak slova "Kód". Tato cesta je roztažen tak, aby velikost stránky má oříznout rastrový obrázek, který obsahuje bitovou kopii **oříznutí textu** zdrojový kód:

[![](text-paths-images/clippingtext-small.png "Trojitá snímek obrazovky stránky oříznutí textu")](text-paths-images/clippingtext-large.png#lightbox "Trojitá snímek obrazovky stránky oříznutí textu")

[ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) Konstruktoru třídy načte bitovou mapu, která je uložena jako vložený prostředek v **média** složku řešení:

```csharp
public class ClippingTextPage : ContentPage
{
    SKBitmap bitmap;

    public ClippingTextPage()
    {
        Title = "Clipping Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.PageOfCode.png";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

`PaintSurface` Obslužná rutina začíná tím, že vytvoříte `SKPaint` objekt vhodný pro text. `Typeface` Je nastavena také `TextSize`, i když se pro tuto konkrétní aplikaci `TextSize` vlastnost je čistě arbirtrary. Všimněte si také neexistuje žádné `Style` nastavení.

`TextSize` a `Style` nastavení vlastnosti nejsou nutné protože to `SKPaint` objekt se používá pouze pro `GetTextPath` zavolat pomocí textový řetězec "Kód". Obslužná rutina pak změří výsledné `SKPath` objektu a použije tři transformace na střed a změňte jeho velikost na velikost stránky. Cestu můžete nastavit jako ořezové cesty:

```csharp
public class ClippingTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Blue);

        using (SKPaint paint = new SKPaint())
        {
            paint.Typeface = SKTypeface.FromFamilyName(null, SKTypefaceStyle.Bold);
            paint.TextSize = 10;

            using (SKPath textPath = paint.GetTextPath("CODE", 0, 0))
            {
                // Set transform to center and enlarge clip path to window height
                SKRect bounds;
                textPath.GetTightBounds(out bounds);

                canvas.Translate(info.Width / 2, info.Height / 2);
                canvas.Scale(info.Width / bounds.Width, info.Height / bounds.Height);
                canvas.Translate(-bounds.MidX, -bounds.MidY);

                // Set the clip path
                canvas.ClipPath(textPath);
            }
        }

        // Reset transforms
        canvas.ResetMatrix();

        // Display bitmap to fill window but maintain aspect ratio
        SKRect rect = new SKRect(0, 0, info.Width, info.Height);
        canvas.DrawBitmap(bitmap,
            rect.AspectFill(new SKSize(bitmap.Width, bitmap.Height)));
    }
}
```

Po nastavení ořezové cesty rastrového obrázku je možné zobrazit a bude oříznut znak přehledy. Všimněte si použití [ `AspectFill` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRect.AspectFill/p/SkiaSharp.SKSize/) metoda `SKRect` , která vypočítá obdélníku pro naplnění stránky při zachování poměru stran.

**Vliv cestu Text** stránky převede jeden znak do cesty k vytvoření vliv cestu 1 D. Objekt malby se tato cesta se pak použije k obtažení osnovy větší verze stejného znaku:

[![](text-paths-images/textpatheffect-small.png "Trojitá snímek obrazovky stránky vliv cestu Text")](text-paths-images/textpatheffect-large.png#lightbox "Trojitá snímek obrazovky stránky vliv cestu Text")

Velká část práce v [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) třída vyvolá se v polích a konstruktor. Dva `SKPaint` objekty definované jako pole se používají pro dva různé účely: první (s názvem `textPathPaint`) se používá k převodu ampersand s `TextSize` 50 na cestu pro vliv cestu 1 D. Druhý (`textPaint`) slouží k zobrazení větší verzi ampersand pomocí tohoto vliv cestu. Z tohoto důvodu `Style` tento druhý malířského objektu se nastaví na `Stroke`, ale `StrokeWidth` vlastnost není nastavená, protože tato vlastnost není nezbytné, při použití mohou mít vliv cestu 1 D:

```csharp
public class TextPathEffectPage : ContentPage
{
    const string character = "@";
    const float littleSize = 50;

    SKPathEffect pathEffect;

    SKPaint textPathPaint = new SKPaint
    {
        TextSize = littleSize
    };

    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black
    };

    public TextPathEffectPage()
    {
        Title = "Text Path Effect";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Get the bounds of textPathPaint
        SKRect textPathPaintBounds = new SKRect();
        textPathPaint.MeasureText(character, ref textPathPaintBounds);

        // Create textPath centered around (0, 0)
        SKPath textPath = textPathPaint.GetTextPath(character,
                                                    -textPathPaintBounds.MidX,
                                                    -textPathPaintBounds.MidY);
        // Create the path effect
        pathEffect = SKPathEffect.Create1DPath(textPath, littleSize, 0,
                                               SKPath1DPathEffectStyle.Translate);
    }
    ...
}
```

Nejdřív pomocí konstruktoru `textPathPaint` objektu k měření ampersand s `TextSize` 50. Negativní center souřadnice obdélníku jsou potom předány `GetTextPath` metodu pro převod textu na cestu. Výsledná cesta má (0, 0) přejděte v centru znaku, který je ideální pro vliv cestu 1D.

Si možná myslíte, že `SKPathEffect` objekt vytvořený na konci konstruktoru může být nastaven na `PathEffect` vlastnost `textPaint` spíše než uložit jako pole. Ale tento nastavená na Ne dobře pracovat, protože zkreslený výsledky `MeasureText` volání `PaintSurface` obslužné rutiny:

```csharp
public class TextPathEffectPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set textPaint TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Do not measure the text with PathEffect set!
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(character, ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Set the PathEffect property and display text
        textPaint.PathEffect = pathEffect;
        canvas.DrawText(character, xText, yText, textPaint);
    }
}
```

Že `MeasureText` hovor se používá k center znak na stránce. Abyste se vyhnuli potížím, `PathEffect` je nastavena na objekt malířského po se měří textu, ale než se zobrazí.

## <a name="outlines-of-character-outlines"></a>Přehledy jsou podrobněji popsány dále znak

Obvykle [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) metodu `SKPaint` převede jednu cestu na jiný použitím vlastnosti programu Malování, zejména stroke šířku a cestu efekt. Při použití bez efekty cest `GetFillPath` účinně vytvoří cestu, která popisuje jinou cestu. To je ukázáno v **klepnutím osnovy cestu** stránku [ **efekty cest** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) článku.

Můžete také volat `GetFillPath` v cestě vrácená `GetTextPath` , ale zpočátku nemusí být úplně jistí tom, jak to bude vypadat.

**Znak osnovy jsou podrobněji popsány dále** stránce ukazuje postup. Důležitý kód je ve `PaintSurface` obslužná rutina [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) třídy.

Konstruktor začíná tím, že vytvoříte `SKPaint` objekt s názvem `textPaint` s `TextSize` vlastností na základě velikosti stránky. To je převeden na cestu pomocí `GetTextPath` metody. Souřadnice argumenty, které mají `GetTextPath` efektivně center cestu na obrazovce:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint())
    {
        // Set Style for the character outlines
        textPaint.Style = SKPaintStyle.Stroke;

        // Set TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Measure the text
        SKRect textBounds = new SKRect();
        textPaint.MeasureText("@", ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Get the path for the character outlines
        using (SKPath textPath = textPaint.GetTextPath("@", xText, yText))
        {
            // Create a new path for the outlines of the path
            using (SKPath outlinePath = new SKPath())
            {
                // Convert the path to the outlines of the stroked path
                textPaint.StrokeWidth = 25;
                textPaint.GetFillPath(textPath, outlinePath);

                // Stroke that new path
                using (SKPaint outlinePaint = new SKPaint())
                {
                    outlinePaint.Style = SKPaintStyle.Stroke;
                    outlinePaint.StrokeWidth = 5;
                    outlinePaint.Color = SKColors.Red;

                    canvas.DrawPath(outlinePath, outlinePaint);
                }
            }
        }
    }
}
```

`PaintSurface` Obslužná rutina vytvoří novou cestu s názvem `outlinePath`. Toto řešení je cílová cesta ve volání `GetFillPath`. `StrokeWidth` Vlastnost 25 příčiny `outlinePath` k popisu osnovy 25 pixel celou cestu vytažení textové znaky. Tato cesta se následně zobrazí červeně s šířku tahu 5:

[![](text-paths-images/characteroutlineoutlines-small.png "Trojitá snímek obrazovky stránky jsou podrobněji popsány dále znak osnovy")](text-paths-images/characteroutlineoutlines-large.png#lightbox "Trojitá snímek obrazovky stránky jsou podrobněji popsány dále znak osnovy")

Prohlédněte si blíže a zobrazí se vám překrytí kde obrysu cesty díky ostrý roh. Jedná se o normální artefakty tohoto procesu.

## <a name="text-along-a-path"></a>Text podél cesty

Text se normálně zobrazí pro vodorovné základnu. Text lze otočit spouštět svisle nebo šikmo směrný plán je však stále rovné čáry.

Existují však situace, kdy se má text spusťte společně křivky. Toto je účelem [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) metoda `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

Textem zadaným v prvním argumentu dojde ke spuštění na cestě zadané jako druhý argument. Můžete začít text na posunu od začátku do cesty `hOffset` argument. Obvykle cestu tvoří základní text: na jedné straně cesty jsou Text horní a dolní dotahy text na druhé. Ale můžete posun základní text z cesty s `vOffset` argument.

Tato metoda nemá žádné zařízení a přidal se návod na nastavení `TextSize` vlastnost `SKPaint` velikosti dokonale spustit od začátku cesty na konec textu. Někdy můžete zjistit velikost tohoto textu na vlastní. Jindy je potřeba pomocí služby functions měření cestu najdete v některém z budoucích článků.

**Cyklické Text** program zalomí text kruh. Je snadné k určení obvod kruhu, takže je snadné pro nastavení velikosti textu podle přesně. `PaintSurface` Obslužná rutina [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) třídy vypočítá poloměr kruhu na základě velikosti stránky. Tento kruh stane `circularPath`:

```csharp
public class CircularTextPage : ContentPage
{
    const string text = "xt in a circle that shapes the te";
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circularPath = new SKPath())
        {
            float radius = 0.35f * Math.Min(info.Width, info.Height);
            circularPath.AddCircle(info.Width / 2, info.Height / 2, radius);

            using (SKPaint textPaint = new SKPaint())
            {
                textPaint.TextSize = 100;
                float textWidth = textPaint.MeasureText(text);
                textPaint.TextSize *= 2 * 3.14f * radius / textWidth;

                canvas.DrawTextOnPath(text, circularPath, 0, 0, textPaint);
            }
        }
    }
}
```

`TextSize` Vlastnost `textPaint` potom upraví tak, aby šířka textu odpovídá obvod kruhu:

[![](text-paths-images/circulartext-small.png "Trojitá snímek obrazovky stránky cyklické Text")](text-paths-images/circulartext-large.png#lightbox "Trojitá snímek obrazovky stránky cyklické Text")

Vlastní text byl zvolen bude poněkud cyklické: slovo "Kruh" je předmětem věty i objekt prepositional frázi.

## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
