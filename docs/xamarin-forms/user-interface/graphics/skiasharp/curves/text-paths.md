---
title: Cesty a Text v SkiaSharp
description: V tomto článku jsou zde popsány průnik SkiaSharp cesty a text a to ukazuje s ukázkový kód.
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/01/2017
ms.openlocfilehash: 305ee2946d3a291e6237d5a2860eda7331193b23
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243902"
---
# <a name="paths-and-text-in-skiasharp"></a>Cesty a Text v SkiaSharp

_Prozkoumejte průnik cesty a text._

V systémech moderní grafiky jsou písma textu kolekce obrysy znaků, obvykle definované kvadratických Bézierových křivek. V důsledku toho mnoho moderní grafické systémy zahrnují umožňuje převést do cesty grafiky textových znaků.

Už jste se seznámili, vám může obtažení jsou podrobněji popsány dále textových znaků, stejně jako je vyplnění. To umožňuje zobrazit tyto obrysy znaků s šířku tahu konkrétní a i efekt cestu, jak je popsáno v [ **cesta důsledky** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) článku. Je také možné převést na řetězec znaků, ale `SKPath` objektu. To znamená, že text jsou podrobněji popsány dále lze použít pro výstřižek pomocí technik, které bylo popsané v [ **výstřižek s cestami a oblasti** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md) článku.

Kromě použití efektu cesta k obtažení outline znak, můžete také vytvořit cestu, kterou účinky, které jsou založená na cestách jsou odvozené z řetězce znaků, a dokonce můžete kombinovat dvě důsledky:

![](text-paths-images/pathsandtextsample.png "Efekt cesta textu")

V [předchozí článek](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) jste viděli, jak [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) metodu `SKPaint` můžete získat přehled vytažené cesty. Tuto metodu můžete použít také pomocí cesty, které jsou odvozené od obrysy znaků.

Nakonec tento článek ukazuje další průnik cesty a text: [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) metodu `SKCanvas` umožňuje zobrazit textový řetězec tak, aby účaří text odpovídá zakřivené cesty.

## <a name="text-to-path-conversion"></a>Text, který má cesta převod

[ `GetTextPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetTextPath/p/System.String/System.Single/System.Single/) Metodu `SKPaint` převede na řetězec znaků `SKPath` objektu:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x` a `y` argumenty označuje počáteční bod základních hodnot v levé části textu. Přehrávání na stejný atribut role v tomto poli jako v `DrawText` metodu `SKCanvas`. V této cestě bude mít základních hodnot v levé části textu souřadnic (x, y).

`GetTextPath` Metoda je přehnaně, pokud chcete jenom vyplnění nebo obtažení výsledné cestu. Normální `DrawText` metoda umožňuje udělat. `GetTextPath` Je užitečnější pro další úkoly týkající se cesty.

Jeden z těchto úloh je výstřižek. **Výstřižek Text** stránky vytvoří výstřižek cestu podle jsou podrobněji popsány dále znak slova "Kód." Tato cesta je roztažen tak, aby velikost stránky oříznutí rastrového obrázku, který obsahuje bitovou kopii **výstřižek Text** zdrojový kód:

[![](text-paths-images/clippingtext-small.png "Trojitá snímek obrazovky stránky výstřižek Text")](text-paths-images/clippingtext-large.png#lightbox "Trojitá snímek obrazovky stránky výstřižek textu")

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

`PaintSurface` Obslužná rutina začíná vytvořením `SKPaint` objekt vhodný pro text. `Typeface` Vlastnost nastavena a taky `TextSize`, i když pro tuto konkrétní aplikaci `TextSize` vlastnost je čistě arbirtrary. Všimněte si také neexistuje žádné `Style` nastavení.

`TextSize` a `Style` nastavení vlastností nejsou nutné protože to `SKPaint` objekt se používá výhradně pro `GetTextPath` volat pomocí textový řetězec "Kódu". Obslužná rutina pak měří výsledné `SKPath` objektu a použije tři transformace na střed a změňte jeho velikost na velikost stránky. Cesta pak můžete nastavit jako cestu výstřižek:

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

Po nastavení cesty výstřižek bitmapy lze zobrazit a bude oříznuto obrysy znaků. Všimněte si použití [ `AspectFill` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRect.AspectFill/p/SkiaSharp.SKSize/) metodu `SKRect` , vypočítá obdélníku pro naplnění stránky při zachování poměru stran.

**Efektu cesta Text** stránky převádí znak znak ampersand na cestu k vytvoření efektu cesta 1 D. Objekt Malování s efektu tato cesta se pak použije k obtažení obrys větší verze tento stejný znak:

[![](text-paths-images/textpatheffect-small.png "Trojitá snímek obrazovky stránky efektu cesta Text")](text-paths-images/textpatheffect-large.png#lightbox "Trojitá snímek obrazovky stránky efekt cesta textu")

Velká část práce při [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) třída dojde v pole a konstruktor. Dva `SKPaint` objekty definované jako pole se používají pro dva různé účely: první (s názvem `textPathPaint`) se používá k převodu ampersand s `TextSize` 50 na cestu k efektu cesta 1 D. Druhý (`textPaint`) se používá k zobrazení větší verze ampersand s platnost této cesty. Z tohoto důvodu `Style` z této druhé Malování objektu na hodnotu `Stroke`, ale `StrokeWidth` není nastavena vlastnost, protože tuto vlastnost není nezbytné při použití efektu cesta 1 D:

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

Nejdřív pomocí konstruktoru `textPathPaint` objektu k měření ampersand s `TextSize` 50. Negativy se center souřadnice obdélníku jsou předána do `GetTextPath` metoda pro převod textu na cestu. Výsledná cesta obsahuje (0, 0) bodu ve středu znaku, který je ideální pro cestu efekt 1D.

Může si myslíte, který `SKPathEffect` vytvořen na konci tohoto konstruktoru objekt může být nastaven na `PathEffect` vlastnost `textPaint` místo uložena jako pole. Ale tento není zapnuté na velmi dobře pracovat, protože je poškozený, výsledky `MeasureText` volání v `PaintSurface` obslužné rutiny:

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

Aby `MeasureText` hovor se používá k centru znak na stránce. Aby nedocházelo k problémům, `PathEffect` je nastavena na objekt Malování po byla měřena text, ale předtím, než se zobrazí.

## <a name="outlines-of-character-outlines"></a>Obrysy obrysy znaků

Obvykle [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) metodu `SKPaint` převede jednu cestu do jiné použitím vlastnosti Malování, zejména tahu šířky a cestu účinek. Při použití bez cesty důsledky `GetFillPath` efektivně vytváří cestu, která popisuje jiné cesty. Tento postup je znázorněn v **klepněte sem a Outline cesta** stránky v [ **cesta důsledky** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) článku.

Můžete také volat `GetFillPath` na cestu, kterou vrátil `GetTextPath` ale zpočátku nemusí být zcela jisti jaké, který chcete vzhled.

**Znak Outline jsou podrobněji popsány dále** stránky ukazuje techniku. Všechny relevantní kód je v `PaintSurface` obslužnou rutinu [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) třídy.

Konstruktor začíná vytvořením `SKPaint` objekt s názvem `textPaint` s `TextSize` vlastností na základě velikosti stránky. To je převést na cestu pomocí `GetTextPath` metoda. Souřadnice argumenty, které mají `GetTextPath` efektivně center cestu na obrazovce:

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

`PaintSurface` Obslužná rutina vytvoří novou cestu s názvem `outlinePath`. To se stane cílovou cestu ve volání `GetFillPath`. `StrokeWidth` Vlastnost 25 příčiny `outlinePath` k popisu obrys 25. pixelů celou cestu vytažení textových znaků. Tato cesta se následně zobrazí červeně s šířku tahu 5:

[![](text-paths-images/characteroutlineoutlines-small.png "Trojitá snímek obrazovky stránky jsou podrobněji popsány dále znak Outline")](text-paths-images/characteroutlineoutlines-large.png#lightbox "Trojitá snímek obrazovky stránky jsou podrobněji popsány dále znak obrysu")

Podrobněji a uvidíte překrytí, kde obrysu cesty díky sharp rohu. Jedná se o normální artefakty tohoto procesu.

## <a name="text-along-a-path"></a>Text podél cesty

Text se zobrazí za normálních okolností na vodorovné směrného plánu. Text lze spustit ve svislém směru nebo šikmo otáčet, ale směrného plánu je stále přímka.

Čas od času, kdy chcete text, který má spusťte společně křivka nejsou k dispozici. Toto je účelem [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) metodu `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

Text zadaný v prvním argumentu Přišla žádost o spuštění v cestě zadané jako druhý argument. Můžete začít text v posun od začátku pomocí cestu `hOffset` argument. Za normálních okolností cesta forms účaří text: Text horních jsou na jedné straně cesty a dolní dotahy text na straně druhé. Ale můžete odsadit základní text z cesty s `vOffset` argument.

Tato metoda nemá žádné zařízení pro poskytovat pokyny k nastavení `TextSize` vlastnost `SKPaint` Chcete-li text, velikost perfektně spustit od začátku cesty na konec. Někdy může rozmyslete si, že velikost textu sami. Jinou dobu, budete muset být popsané v článku na budoucí pomocí funkce měření cestu.

**Cyklické Text** program obtéká text kruh. Je snadné tak, aby byl snadno velikost text, který se nevejde přesně určit obvodu kruhu. `PaintSurface` Obslužnou rutinu [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) třída vypočítá radius kruhu na základě velikosti stránky. Že kroužek se změní na `circularPath`:

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

`TextSize` Vlastnost `textPaint` se pak upraví tak, aby odpovídalo obvodu kruhu šířka textu:

[![](text-paths-images/circulartext-small.png "Trojitá snímek obrazovky stránky cyklické Text")](text-paths-images/circulartext-large.png#lightbox "Trojitá snímek obrazovky stránky cyklické textu")

Vlastní text jste vybrali bude poněkud cyklické: slova "Kruh" je předmět věty i objekt prepositional frázi.

## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
