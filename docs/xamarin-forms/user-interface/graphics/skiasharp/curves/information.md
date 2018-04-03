---
title: Informace o cestě a – výčet
description: Získat informace o cestách a výčet obsahu
ms.topic: article
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/12/2017
ms.openlocfilehash: 9c7e8a44a9bb4ee8cb131ffa25e4a595b4225ed6
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/03/2018
---
# <a name="path-information-and-enumeration"></a>Informace o cestě a – výčet

_Získat informace o cestách a výčet obsahu_

[ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) Třída definuje několik vlastnosti a metody, které vám umožní získat informace o cestě. [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) a [ `TightBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.TightBounds/) vlastnosti (a související metody) získat metrical dimenze cesty. [ `Contains` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Contains/p/System.Single/System.Single/) Metody můžete zjistit, jestli do konkrétního bodu v rámci cestu.

Někdy je užitečné k určení celková délka čar a křivek, které tvoří cestu. Toto není algorithmically jednoduchou úlohou, takže celou třídu s názvem [ `PathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) je věnována.

Je také někdy užitečné k získání kreslení operace a body, které tvoří cestu. Na první, pokud tuto funkci zdát, nepotřebné: Pokud váš program vytvoří cestu, program již zná obsah. Ale už víte, že cesty lze vytvořit také pomocí [cesta důsledky](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) a převedením [textové řetězce do cesty](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md). Můžete také získat kreslení operace a body, které tvoří tyto cesty. Jednou z možností je použít algoritmické transformace pro všechny body. To umožňuje obtékání textu kolem jedna polokoule, jako jsou:

![](information-images/pathenumerationsample.png "Text zabalené na polokoule")

## <a name="getting-the-path-length"></a>Získávání délky cesty

V článku [ **cesty a Text** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) jste viděli, jak používat [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) metoda kreslení textový řetězec, jehož základní následuje během cesty. Ale co když budete chtít velikost text tak, aby odpovídal cesta přesněji? Pro kreslení textu kolem kruh, totiž snadno obvodu kruhu je jednoduchá k výpočtu. Ale obvodu elipsy nebo délka Bézierovy křivky není tak jednoduché. 

[ `SKPathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) Může pomoci třídy. [Konstruktor](https://developer.xamarin.com/api/constructor/SkiaSharp.SKPathMeasure.SKPathMeasure/p/SkiaSharp.SKPath/System.Boolean/System.Single/) přijímá `SKPath` argument a [ `Length` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPathMeasure.Length/) vlastnost zjistí jeho délka.

Tento postup je znázorněn v **délka cesty** ukázka, která je založena na **Bézierovu křivku** stránky. [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) souboru je odvozena z `InteractivePage` a zahrnuje touch rozhraní:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Curves.PathLengthPage"
                       Title="Path Length">
    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</local:InteractivePage>
```

[ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) souboru kódu umožňuje přesunout čtyři body touch definovat koncové body a body Bézierovy křivky krychlový ovládání. Tři pole definovat textového řetězce, `SKPaint` objekt a počítané šířka textu:

```csharp
public partial class PathLengthPage : InteractivePage
{
    const string text = "Compute length of path";

    static SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Black,
        TextSize = 10,
    };

    static readonly float baseTextWidth = textPaint.MeasureText(text);
    ...
}
```

`baseTextWidth` Pole je textu na základě `TextSize` nastavení 10.

`PaintSurface` Obslužná rutina nevykresluje Bézierovy křivky a pak velikostí umisťování podél jeho úplnou délka:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with cubic Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.CubicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     touchPoints[3].Center);

        canvas.DrawPath(path, strokePaint);

        // Get path length
        SKPathMeasure pathMeasure = new SKPathMeasure(path, false, 1);

        // Find new text size
        textPaint.TextSize = pathMeasure.Length / baseTextWidth * 10;

        // Draw text on path
        canvas.DrawTextOnPath(text, path, 0, 0, textPaint);
    }
    ...
}
```

`Length` Vlastnost nově vytvořený `SKPathMeasure` objekt získá délka cesty. To je rozdělena `baseTextWidth` hodnotu (což je textu na základě velikosti text 10) a potom vynásobena základního textu velikost 10. Výsledkem je nová velikost textu pro zobrazení textu podél této cesty:

[![](information-images/pathlength-small.png "Trojitá snímek obrazovky stránky délka cesty")](information-images/pathlength-large.png#lightbox "Trojitá snímek obrazovky stránky délka cesty")

Jako Bézierovy křivky získá delší nebo kratší, zobrazí se změnit velikost textu.

## <a name="traversing-the-path"></a>Procházení cesta

`SKPathMeasure` můžete provést více než jen měr délka cesty. Pro žádnou hodnotu mezi 0 a délka cesty `SKPathMeasure` objekt můžete získat od tohoto okamžiku pozici na cestu a tangens na křivku cestu. Tangens je k dispozici jako vektoru ve formě `SKPoint` objektu, nebo jako rotaci kolem zapouzdřené v `SKMatrix` objektu. Tady jsou metody `SKPathMeasure` , způsoby rozmanitých a flexibilní získat tyto informace:

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

[ `SKPathMeasureMatrixFlags` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasureMatrixFlags/) Jsou:

- [`GetPosition`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPosition/)
- [`GetTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)
- [`GetPositionAndTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)

**Kanálu polovině své blízké** stránky animuje Flash disk obrázek na své blízké, který se zdá se, že vozidlo a zpět podél krychlový Bézierovy křivky pro:

[![](information-images/unicyclehalfpipe-small.png "Trojitá snímek obrazovky stránky kanálu polovině své blízké")](information-images/unicyclehalfpipe-large.png#lightbox "Trojitá snímek obrazovky stránky své blízké půl kanálu")

`SKPaint` Objekt použitý pro vytažení půl kanálu a své blízké je definován jako pole v [ `UnicycleHalfPipePage` ]() třídy. Je také definován `SKPath` objekt pro své blízké:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };

    SKPath unicyclePath = SKPath.ParseSvgPathData(
        "M 0 0" + 
        "A 25 25 0 0 0 0 -50" +
        "A 25 25 0 0 0 0 0 Z" +
        "M 0 -25 L 0 -100" +
        "A 15 15 0 0 0 0 -130" +
        "A 15 15 0 0 0 0 -100 Z" +
        "M -25 -85 L 25 -85");
    ...
}
```

Třída obsahuje standardní přepsáními `OnAppearing` a `OnDisappearing` metody pro animace. `PaintSurface` Obslužná rutina vytvoří cesta pro půl kanálu a pak ho nevykresluje. `SKPathMeasure` Objektu je poté jste vytvořili podle tuto cestu:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath pipePath = new SKPath())
        {
            pipePath.MoveTo(50, 50);
            pipePath.CubicTo(0, 1.25f * info.Height, 
                             info.Width - 0, 1.25f * info.Height,
                             info.Width - 50, 50);

            canvas.DrawPath(pipePath, strokePaint);

            using (SKPathMeasure pathMeasure = new SKPathMeasure(pipePath))
            {
                float length = pathMeasure.Length;

                // Animate t from 0 to 1 every three seconds
                TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
                float t = (float)(timeSpan.TotalSeconds % 5 / 5);

                // t from 0 to 1 to 0 but slower at beginning and end
                t = (float)((1 - Math.Cos(t * 2 * Math.PI)) / 2);

                SKMatrix matrix;
                pathMeasure.GetMatrix(t * length, out matrix, 
                                      SKPathMeasureMatrixFlags.GetPositionAndTangent);

                canvas.SetMatrix(matrix);
                canvas.DrawPath(unicyclePath, strokePaint);
            }
        }
    }
}
```

`PaintSurface` Obslužná rutina vypočítá hodnotu `t` , přejde od 0 do 1 každých pět sekund. Poté použije `Math.Cos` funkce, převést na hodnotu `t` , rozsahy od 0 do 1 a zpět na hodnotu 0, kde 0 odpovídá své blízké na začátku nahoře vlevo, zatímco 1 odpovídá své blízké vpravo nahoře. Funkce kosinus způsobí, že rychlost nejpomalejší v horní části kanálu a nejrychlejší dole.

Všimněte si, že hodnota `t` musí být násobí hodnotou délka cesty pro první argument `GetMatrix`. Matice se potom použije k `SKCanvas` objekt pro vykreslení své blízké cestu.

## <a name="enumerating-the-path"></a>Vytváření výčtu cesta

Dva vložených třídy `SKPath` umožňují výčet obsahu cesty. Tyto třídy jsou [ `SKPath.Iterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+Iterator/) a [ `SKPath.RawIterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+RawIterator/). Dvě třídy jsou velmi podobné, ale `SKPath.Iterator` můžete eliminovat elementy v cestě s nulovou délkou nebo blízko nulové délky. `RawIterator` Se používá v následujícím příkladu.

Můžete získat objekt typu `SKPath.RawIterator` voláním [ `CreateRawIterator` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CreateRawIterator()/) metodu `SKPath`. Výčet prostřednictvím cestu lze provést opakovaně voláním [ `Next` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.Next/p/SkiaSharp.SKPoint[]/) metoda. Předat pole čtyři `SKPoint` hodnoty:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next` Metoda vrátí členem [ `SKPathVerb` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathVerb/) výčtu. Tyto hodnoty označují příkaz konkrétní kreslení v cestě. Počet platné body vložen do pole závisí na tento příkaz:

- [`Move`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Move/) s jediný bod
- [`Line`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Line/) s dva body.
- [`Cubic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Cubic/) s čtyři body
- [`Quad`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Quad/) s tři body
- [`Conic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Conic/) s tři body (a také zavolat [ `ConicWeight` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.ConicWeight/) metodu pro váhu)
- [`Close`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Close/) s jeden bod
- [`Done`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Done/)

`Done` Příkaz označuje, že výčtu je kompletní.

Všimněte si, že neexistují žádné `Arc` příkazy. To znamená, že všechny oblouky se převedou na Bézierových křivek při přidání k cestě.

Některé z informací v `SKPoint` pole je redundantní. Například pokud `Move` příkaz následuje `Line` příkaz a potom první dva body, které doprovází `Line` je stejný jako `Move` bodu. V praxi je velmi užitečné tento redundance. Když dojde `Cubic` operace, je přiložena všechny čtyři body, které definují krychlový Bézierovy křivky. Není nutné zachovat aktuální pozici vymezenému předchozí příkaz.

Problematické operace, je však `Close`. Tento příkaz vloží přímou čáru od aktuální pozice na začátek obrysem dříve nástrojem navázat `Move` příkaz. V ideálním případě `Close` operace by měl poskytovat tyto dva body spíše než jenom jeden bod. Co je zhoršení je, že bod doplňujícími `Close` je vždy (0, 0). To znamená, že při vytvoření výčtu prostřednictvím cestu, budete pravděpodobně muset zachovat `Move` bod a aktuální pozici.

Statické [ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) třída obsahuje několik metod, které převést tři typy Bézierových křivek na řadu jen nepatrnou přímých řádky, které Přibližná křivku. (Čištění vzorce byly uvedené v článku [ **tři typy Bézierových křivek**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) `Interpolate` Metoda rozpis přímku do mnoha krátké řádky, které jsou pouze jednu jednotku délka:

```csharp
static class PathExtensions
{
    ...
    static SKPoint[] Interpolate(SKPoint pt0, SKPoint pt1)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * pt0.X + t * pt1.X;
            float y = (1 - t) * pt0.Y + t * pt1.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenCubic(SKPoint pt0, SKPoint pt1, SKPoint pt2, SKPoint pt3)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2) + Length(pt2, pt3));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * (1 - t) * pt0.X +
                        3 * t * (1 - t) * (1 - t) * pt1.X +
                        3 * t * t * (1 - t) * pt2.X +
                        t * t * t * pt3.X;
            float y = (1 - t) * (1 - t) * (1 - t) * pt0.Y +
                        3 * t * (1 - t) * (1 - t) * pt1.Y +
                        3 * t * t * (1 - t) * pt2.Y +
                        t * t * t * pt3.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenQuadratic(SKPoint pt0, SKPoint pt1, SKPoint pt2)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenConic(SKPoint pt0, SKPoint pt1, SKPoint pt2, float weight)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float denominator = (1 - t) * (1 - t) + 2 * weight * t * (1 - t) + t * t;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * weight * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * weight * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            x /= denominator;
            y /= denominator;
        }

        return points;
    }

    static double Length(SKPoint pt0, SKPoint pt1)
    {
        return Math.Sqrt(Math.Pow(pt1.X - pt0.X, 2) + Math.Pow(pt1.Y - pt0.Y, 2));
    }
}
```

Všechny tyto metody se odkazuje z metody rozšíření `CloneWithTransform` vidíte níže. Tato metoda provede klonování cestu k vytváření výčtu příkazy cesty a vytváření na základě dat novou cestu. Ale nová cesta se skládá jenom z `MoveTo` a `LineTo` volání. Všechny křivek a úseček jsou omezeny na řadu jen nepatrnou řádky.

Při volání metody `CloneWithTransform`, předáte metodě `Func<SKPoint, SKPoint>`, což je funkce s `SKPaint` parametr, který vrátí `SKPoint` hodnotu. Tato funkce je volána pro každý bod použít vlastní algoritmické transformace:

```csharp
static class PathExtensions
{
    public static SKPath CloneWithTransform(this SKPath pathIn, Func<SKPoint, SKPoint> transform)
    {
        SKPath pathOut = new SKPath();

        using (SKPath.RawIterator iterator = pathIn.CreateRawIterator())
        {
            SKPoint[] points = new SKPoint[4];
            SKPathVerb pathVerb = SKPathVerb.Move;
            SKPoint firstPoint = new SKPoint();
            SKPoint lastPoint = new SKPoint();

            while ((pathVerb = iterator.Next(points)) != SKPathVerb.Done)
            {
                switch (pathVerb)
                {
                    case SKPathVerb.Move:
                        pathOut.MoveTo(transform(points[0]));
                        firstPoint = lastPoint = points[0];
                        break;

                    case SKPathVerb.Line:
                        SKPoint[] linePoints = Interpolate(points[0], points[1]);

                        foreach (SKPoint pt in linePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[1];
                        break;

                    case SKPathVerb.Cubic:
                        SKPoint[] cubicPoints = FlattenCubic(points[0], points[1], points[2], points[3]);

                        foreach (SKPoint pt in cubicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[3];
                        break;

                    case SKPathVerb.Quad:
                        SKPoint[] quadPoints = FlattenQuadratic(points[0], points[1], points[2]);

                        foreach (SKPoint pt in quadPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Conic:
                        SKPoint[] conicPoints = FlattenConic(points[0], points[1], points[2], iterator.ConicWeight());

                        foreach (SKPoint pt in conicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Close:
                        SKPoint[] closePoints = Interpolate(lastPoint, firstPoint);

                        foreach (SKPoint pt in closePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        firstPoint = lastPoint = new SKPoint(0, 0);
                        pathOut.Close();
                        break;
                }
            }
        }
        return pathOut;
    }
    ...
}
```

Vzhledem k tomu, že klonovaný cesta byla snížena jen nepatrnou rovné čáry, transformační funkce má možnost převodu rovné čáry na křivky.

Všimněte si, že metoda zachová první bod každý obrysem v proměnné názvem `firstPoint` a aktuální pozice po každém z nich kreslení příkaz v proměnné `lastPoint`. Toto jsou potřebné k vytvoření konečné ukončovací řádek, kdy `Close` zjistil se příkaz.

**GlobularText** ukázce se používá tato metoda rozšíření zdánlivě zalomení textu kolem polokoule v 3D vliv:

[![](information-images/globulartext-small.png "Trojitá snímek obrazovky stránky Globular Text")](information-images/globulartext-large.png#lightbox "Trojitá snímek obrazovky stránky Globular Text")

[ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) Konstruktoru třídy provede Tato transformace. Vytvoří `SKPaint` objekt pro text a pak získá `SKPath` objektu z `GetTextPath` metoda. Jedná se o cestu předaný `CloneWithTransform` metoda rozšíření spolu s transform funkce: 

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;

    public GlobularTextPage()
    {
        Title = "Globular Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.Typeface = SKTypeface.FromFamilyName("Times New Roman");
            textPaint.TextSize = 100;

            using (SKPath textPath = textPaint.GetTextPath("HELLO", 0, 0))
            {
                SKRect textPathBounds;
                textPath.GetBounds(out textPathBounds);

                globePath = textPath.CloneWithTransform((SKPoint pt) =>
                {
                    double longitude = (Math.PI / textPathBounds.Width) * 
                                            (pt.X - textPathBounds.Left) - Math.PI / 2;
                    double latitude = (Math.PI / textPathBounds.Height) * 
                                            (pt.Y - textPathBounds.Top) - Math.PI / 2;

                    longitude *= 0.75;
                    latitude *= 0.75;

                    float x = (float)(Math.Cos(latitude) * Math.Sin(longitude));
                    float y = (float)Math.Sin(latitude);

                    return new SKPoint(x, y);
                });
            }
        }
    }
    ...
}
```

Funkce transformace nejprve vypočítá dvě hodnoty s názvem `longitude` a `latitude` rozsahu od-pí/2 v horní a levé straně textu, až pí/2 v pravé a dolní části textu. Rozsah těchto hodnot není vizuálně vyhovující, proto jsou omezeny vynásobením 0,75. (Zkuste kód bez tyto úpravy. Text se změní na příliš skrytého v Severní a Jižní hole a příliš dynamické po stranách.) Tyto trojrozměrné kulovým souřadnice se převedou na dvourozměrná `x` a `y` souřadnice ve standardní vzorce.

Nová cesta je uložena jako pole. `PaintSurface` Obslužná rutina je jenom nutné, aby na střed a škálování cesta k zobrazení na obrazovce:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint pathPaint = new SKPaint())
        {
            pathPaint.Style = SKPaintStyle.Fill;
            pathPaint.Color = SKColors.Blue;
            pathPaint.StrokeWidth = 3;
            pathPaint.IsAntialias = true;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(0.45f * Math.Min(info.Width, info.Height));     // radius
            canvas.DrawPath(globePath, pathPaint);
        }
    }
}
```

To je velice flexibilní technika. Pokud pole cesty důsledky podrobněji [ **cesta důsledky** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) článku nebude zahrnovat poměrně něco popisovač by měl být zahrnutý, to je způsob k vyplnění mezer.

## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
