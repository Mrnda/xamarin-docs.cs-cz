---
title: Informace o cestě a výčet
description: Tento článek vysvětluje, jak získat informace o cesty ve Skiasharpu a spočítat obsahy a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 09/12/2017
ms.openlocfilehash: 65c614e9a6eb26bc0d027a4a67bec19b036d0a70
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615272"
---
# <a name="path-information-and-enumeration"></a>Informace o cestě a výčet

_Získejte informace o cestách a spočítat obsahy_

[ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) Třída definuje několik vlastností a metod, které umožňují získat informace o cestě. [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) a [ `TightBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.TightBounds/) vlastnosti (a související metody) získat metrical dimenze cesty. [ `Contains` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Contains/p/System.Single/System.Single/) Metoda vám umožňuje zjistit, jestli konkrétní bod v rámci cesty.

Někdy je užitečné k určení celková délka čar a křivek, které tvoří cestu. Toto není algorithmically jednoduchého úkolu, takže celou třídu s názvem [ `PathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) je věnována.

Je také někdy užitečné získat kreslicí operace a body, které tvoří cestu. Tato zařízení můžou zdát, zbytečné: Pokud váš program vytvořil cestu, program již zná obsah. Ale už víte, že cesty mohou také vytvořit [efekty cest](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) a převodem [textové řetězce do cesty](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md). Můžete také získat kreslicí operace a body, které tvoří tyto cesty. Jednou z možností je použít na všechny body vylepšením transformace. To umožňuje techniky, jako je například zalamování textu kolem polokoule:

![](information-images/pathenumerationsample.png "Text zalomený na polokoule")

## <a name="getting-the-path-length"></a>Získávání délky cesty

V následujícím článku [ **cesty a Text** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) jste viděli, jak používat [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) metoda nakreslete textový řetězec, jehož základní následuje kurzu cestu. Ale co když budete chtít velikost textu, tak, aby přesně odpovídal cestu? Pro kreslení textu kruh, totiž snadno obvod kruhu je jednoduché pro výpočet. Ale není tak snadné obvod elipsa nebo délka Bézierovy křivky.

[ `SKPathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) Třída může pomoci. [Konstruktor](https://developer.xamarin.com/api/constructor/SkiaSharp.SKPathMeasure.SKPathMeasure/p/SkiaSharp.SKPath/System.Boolean/System.Single/) přijímá `SKPath` argument a [ `Length` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPathMeasure.Length/) vlastnost odhalí jeho délky.

To je patrné **délka cesty** ukázka, která je založena na **Bézierovu křivku** stránky. [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) souboru je odvozen od `InteractivePage` a zahrnuje dotykové rozhraní:

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

[ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) soubor kódu na pozadí vám umožní přesunout čtyři dotykovými body k definování koncový bod a řízení body kubické Bézierovy křivky. Tři pole definovat textový řetězec `SKPaint` objektu a počítané šířku textu:

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

`baseTextWidth` Pole je šířku textu na základě `TextSize` nastavení 10.

`PaintSurface` Obslužná rutina kreslení Bézierovy křivky a potom velikosti přizpůsobení podél jeho úplnou délka textu:

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

`Length` Vlastnosti nově vytvořeného `SKPathMeasure` objektu získá délku cesty. To je rozdělena `baseTextWidth` hodnotu (což je šířku textu na základě velikosti textu 10) a potom vynásobí velikostí základního textu 10. Výsledkem je nové velikosti textu pro zobrazování textu na této cestě:

[![](information-images/pathlength-small.png "Trojitá snímek obrazovky stránky délka cesty")](information-images/pathlength-large.png#lightbox "Trojitá snímek obrazovky stránky délka cesty")

Protože Bézierovy křivky získá delší nebo kratší, zobrazí se změnit velikost textu.

## <a name="traversing-the-path"></a>Prochází cestou

`SKPathMeasure` můžete provést více než jen míry délka cesty. Pro libovolnou hodnotu mezi 0 a délka cesty `SKPathMeasure` objektu můžete získat od tohoto okamžiku pozice na cestě a tangens na křivku cestu. Tangens je k dispozici jako vektor ve formě `SKPoint` objektu, nebo jako otočení zapouzdřena v `SKMatrix` objektu. Tady jsou metody `SKPathMeasure` , který v různých a flexibilní způsob získání těchto informací:

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

**Polovině kanály kola** animuje obrázek na své blízké, co podél kubické Bézierovy křivky svézt vpřed a zpět na stránku:

[![](information-images/unicyclehalfpipe-small.png "Trojitá snímek obrazovky stránky polovině kanály kola")](information-images/unicyclehalfpipe-large.png#lightbox "Trojitá snímek obrazovky stránky kola polovině-kanálu")

`SKPaint` Objektu se používá pro vytažení polovině kanálu a kola je definován jako pole [ `UnicycleHalfPipePage` ]() třídy. Je také definováno `SKPath` objektu kola:

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

Třída obsahuje standardní přepsání `OnAppearing` a `OnDisappearing` metody pro animaci. `PaintSurface` Obslužná rutina vytvoří cesta pro polovině kanálu a pak vykreslí. `SKPathMeasure` Objekt se pak vytvoří podle tuto cestu:

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

`PaintSurface` Obslužná rutina vypočítá hodnotu `t` , které dostane od 0 do 1 každých pět sekund. Poté použije `Math.Cos` funkce pro převod, který na hodnotu `t` , který rozsah od 0 do 1 a zpět na 0, kde 0 odpovídá kola na začátku vlevo nahoře, během 1 odpovídá kola vpravo nahoře. Funkce kosinus způsobí, že rychlost nejpomalejší v horní části kanálu a nejrychlejší v dolní části.

Všimněte si, že tato hodnota `t` musí být vynásobena délkou cestu pro první argument `GetMatrix`. Matice se následně použije na `SKCanvas` objektů pro kreslení kola cesty.

## <a name="enumerating-the-path"></a>Vytváření výčtu cestu

Dvě vložené třídy `SKPath` bylo možné provést výčet obsahu cestu. Tyto třídy jsou [ `SKPath.Iterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+Iterator/) a [ `SKPath.RawIterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+RawIterator/). Dvě třídy jsou velmi podobné, ale `SKPath.Iterator` mohou odstranit prvky v cestě s nulovou délkou nebo blízko nulovou délku. `RawIterator` Se používá v následujícím příkladu.

Můžete získat objekt typu `SKPath.RawIterator` voláním [ `CreateRawIterator` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CreateRawIterator()/) metoda `SKPath`. Vytváření výčtů cesta se provádí pomocí opakovaného volání [ `Next` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.Next/p/SkiaSharp.SKPoint[]/) metody. Předat pole ze čtyř `SKPoint` hodnoty:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next` Metoda vrátí členem [ `SKPathVerb` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathVerb/) výčtu. Tyto hodnoty označují konkrétní příkaz vykreslování v cestě. Počet vložena do pole platné body, závisí na tento příkaz:

- [`Move`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Move/) s jediným bodem
- [`Line`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Line/) dva body
- [`Cubic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Cubic/) čtyři body.
- [`Quad`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Quad/) tři body
- [`Conic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Conic/) tři body (a také volat [ `ConicWeight` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.ConicWeight/) metodu váha)
- [`Close`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Close/) s jedním bodem
- [`Done`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Done/)

`Done` Příkaz označuje dokončení výčtu.

Všimněte si, že neexistují žádné `Arc` příkazů. To znamená, že všechny oblouky převedou na Bézierových křivek, když se přidá do cesty.

Některé z informací v `SKPoint` pole je redundantní. Například pokud `Move` příkaz následuje `Line` sloveso, pak první dva body, které nejsou poskytnuty `Line` je stejný jako `Move` bodu. V praxi je velmi užitečné tuto redundanci. Když se zobrazí `Cubic` příkaz, je přiložena všechny čtyři body, které definují kubické Bézierovy křivky. Není nutné zachovat aktuální pozici stanovené předchozí příkaz.

Problematické operace, ale `Close`. Tento příkaz Kreslení rovné čáry od aktuální pozice na začátek obrysu dříve podle navázat `Move` příkazu. V ideálním případě by `Close` operace by měla poskytnout tyto dva body spíše než jenom jeden bod. Co je horší je, že bod souvisejícím `Close` sloveso je vždycky (0, 0). To znamená, že při vytvoření výčtu prostřednictvím cestu, bude pravděpodobně nutné zachovat `Move` bod a aktuální pozici.

Statické [ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) třída obsahuje několik metod, které převést tři typy Bézierových křivek na řadu malý rovné čáry, které přibližný křivky. (Ukazatelů vzorce byly uvedené v článku [ **tři typy Bézierových křivek**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) `Interpolate` Metoda rozdělí do mnoha krátké řádky, které jsou pouze jednu jednotku v délce rovné čáry:

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

Všechny tyto metody jsou odkazovány z metody rozšíření `CloneWithTransform` vidíte níže. Tato metoda duplicity cestu tak, že výčet příkazů pro cesty a novou cestu na základě dat o sestavení. Ale nová cesta se skládá pouze z `MoveTo` a `LineTo` volání. Křivky a přímé čáry jsou zmenšeny na řadu malý řádky.

Při volání metody `CloneWithTransform`, předat metodě `Func<SKPoint, SKPoint>`, což je funkce s `SKPaint` parametr, který vrátí `SKPoint` hodnotu. Tato funkce je volána pro každý bod na použití vlastní vylepšením transformace:

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

Vzhledem k tomu, že klonovaný cesta je omezená na malý rovné čáry, transformační funkce má funkce převodu rovné čáry na křivky.

Všimněte si, že metoda zachová první bod každé obrysu v proměnné názvem `firstPoint` a aktuální pozice po každém z nich kreslení příkaz v proměnné `lastPoint`. To je nezbytné k sestavení kompletních poslední ukončovací řádku `Close` příkaz dochází.

**GlobularText** Ukázka používá tuto metodu rozšíření k zdánlivě Zalamovat text kolem polokoule 3D platná:

[![](information-images/globulartext-small.png "Trojitá snímek obrazovky stránky Globular Text")](information-images/globulartext-large.png#lightbox "Trojitá snímek obrazovky stránky Globular Text")

[ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) Konstruktoru třídy provádí Tato transformace. Vytvoří `SKPaint` objekt textu a potom získá `SKPath` objektu z `GetTextPath` metody. Toto je cesta předán `CloneWithTransform` – metoda rozšíření spolu s funkce transformace:

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

Funkce transformace vypočítá první dvě hodnoty s názvem `longitude` a `latitude` tohoto rozsahu od-pí/2 nahoře a vlevo od textu, do pí/2 na vpravo a dole textu. Rozsah těchto hodnot není vizuálně uspokojivé kvality, a proto jsou sníženy vynásobením 0,75. (Zkuste kódu bez tyto úpravy. Text se změní v Severní a Jižní hole příliš skrytého a po stranách příliš dynamického zajišťování.) Tyto trojrozměrného kulovité souřadnice jsou převedeny na dvourozměrné `x` a `y` souřadnice ve standardní vzorce.

Nová cesta je uložena jako pole. `PaintSurface` Obslužná rutina se pak pouze musí Centrování a škálovat cesty se zobrazí na obrazovce:

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

To je velmi flexibilní technika. Pokud pole efekty cest popsané v [ **efekty cest** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) článku nebude zahrnovat poměrně něco popisovač by měl být zahrnutý, toto je způsob, jak vyplnit mezery ve znalostech.

## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
