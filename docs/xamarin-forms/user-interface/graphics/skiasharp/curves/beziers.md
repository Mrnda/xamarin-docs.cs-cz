---
title: Tři typy Bézierových křivek
description: Tento článek vysvětluje, jak ve Skiasharpu použít k vykreslení conic, kvadratické a kubické Bézierovy křivky v aplikacích Xamarin.Forms a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: charlespetzold
ms.author: chape
ms.date: 05/25/2017
ms.openlocfilehash: 0ad722f22cf5ed8dc06fdf0d1e063d285e2ddb2f
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615337"
---
# <a name="three-types-of-bzier-curves"></a>Tři typy Bézierových křivek

_Podívejte se, jak ve Skiasharpu použitých k vykreslování conic, kvadratické a kubické Bézierovy křivky_

Bézierovy Pierre (1910 – 1999), francouzština inženýr ve společnosti automobilový průmysl Renault, který používá křivky pro návrh s asistencí počítače subjektů car má stejný název Bézierovy křivky.

Jsou známy se dobře hodí pro interaktivní návrhu Bézierových křivek: jsou dobře behaved &mdash; jinými slovy, nejsou k dispozici singularities, které způsobují křivka se nekonečné nebo nepraktické &mdash; a jsou obecně vkusnou . Jsou podrobněji popsány dále znak z písma založené na počítačích se obvykle definují s Bézierových křivek:

![](beziers-images/beziersample.png "Ukázka Bézierovu křivku")

Článku na wikipedii o [Bézierovy křivky](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) obsahuje pár užitečných informací. Termín *Bézierovy křivky* ve skutečnosti odkazuje na řadu podobné křivky. Ve Skiasharpu podporuje tři typy Bézierových křivek, volá se, *kubické*, *kvadratické*a *conic*. Conic se také označuje jako *racionální kvadratické*.

## <a name="the-cubic-bzier-curve"></a>Kubické Bézierovy křivky

Volání cubic je typ Bézierovy křivky, Většina vývojářů představit po předmětem Bézierovy křivky.

Můžete přidat kubické Bézierovy křivky do `SKPath` pomocí [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) metoda se třemi `SKPoint` parametry, nebo [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/System.Single/) přetížení samostatné `x` a `y` parametry:

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

Křivka začne do aktuálního místa obrysu. Kompletní kubické Bézierovy křivky se definuje čtyři body:

- počáteční bod: aktuální příkaz ve obrysu, nebo (0, 0) Pokud `MoveTo` nevolala
- nejprve řídicí bod: `point1` v `CubicTo` volání
- za druhé řídicí bod: `point2` v `CubicTo` volání
- koncový bod: `point3` v `CubicTo` volání

Výsledná křivky začíná u počáteční bod a končí na koncovém bodu. Křivka obecně neprochází přes dvě kontrolních bodů; Místo toho fungují mnohem like magnets přetahování křivku na ně.

Experimentování ve službě, je nejlepší způsob, jak získat představu kubické Bézierovy křivky. Toto je účelem **Bézierovu křivku** stránky, která je odvozena z `InteractivePage`. [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) vytvoří soubor `SKCanvasView` a `TouchEffect`. [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) soubor kódu na pozadí vytvoří čtyři `TouchPoint` objekty 've svém konstruktoru. `PaintSurface` Vytvoří obslužnou rutinu události `SKPath` k vykreslení Bézierovy křivky založené na čtyři `TouchPoint` objekty a také nakreslí tečkovaná tečny z kontrolních bodů do koncových bodů:

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
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[2].Center.X,
                    touchPoints[2].Center.Y,
                    touchPoints[3].Center.X,
                    touchPoints[3].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
       touchPoint.Paint(canvas);
    }
}
```

Tady je spuštěn na všech třech platformách:

[![](beziers-images/beziercurve-small.png "Trojitá snímek obrazovky stránky Bézierovu křivku")](beziers-images/beziercurve-large.png#lightbox "Trojitá snímek obrazovky stránky Bézierovu křivku")

Matematický křivka je kubické polynomial. Rovné čáry na třech místech protíná křivka nejvíce. V okamžiku spuštění křivka je vždy tečný do a také ve stejném směru jako rovnou linii od začátku, přejděte na první řídicí bod. Na koncovém bodu křivka je vždy tečný do a také ve stejném směru jako rovnou linii od druhý ovládací prvek, přejděte na koncový bod.

Kubické Bézierovy křivky je vždy ohraničené konvexní čtyřúhelník připojení čtyři body. Tento postup se nazývá *konvexní trupu*. Pokud kontrolní body leží na rovné čáry mezi počáteční a koncový bod, Bézierovy křivky vykreslí jako rovné čáry. Ale křivka můžete také různé samostatně, jak třetí snímek obrazovky ukazuje.

Cesta rozvrh může obsahovat více připojených kubické Bézierovy křivky, ale bude připojení mezi dvěma kubické Bézierovy křivky smooth pouze v případě, že jsou následující tři body colinear (to znamená, leží na rovné čáry):

- druhý řídicí bod křivky první
- koncový bod křivky první, což je také počáteční bod druhý křivky
- První řídicí bod křivky druhý

V následujícím článku na [ **Data cesty SVG** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) zjistíte zařízení k usnadnění definice technologie smooth připojených Bézierovy křivky.

Někdy je užitečné vědět základní parametrické rovnice, které vykreslují kubické Bézierovy křivky. Pro *t* od 0 do 1, parametrické rovnice jsou následující:

x(t) = (1 – t) ³x₀ + 3t (1 – t) ²x₁ + 3t² (1 – t) x₂ + t³x₃

y(t) = (1 – t) ³y₀ + 3t (1 – t) ²y₁ + 3t² (1 – t) y₂ + t³y₃

Nejvyšší exponent 3 potvrdí, že jde o kubické polynomials. Je snadné k ověření, že `t` rovná 0, je bod je (x₀ y₀), což je počáteční bod a kdy `t` rovná 1, je bod je (x₃ y₃), což je koncový bod. U počáteční bod (pro nízké hodnoty `t`), první řídicí bod (x₁, y₁) má silné projeví a téměř koncový bod (vysoké hodnoty, které nejde ") druhý řídicí bod (x₂, y₂) má silný účinek.

## <a name="bzier-curve-approximation-to-circular-arcs"></a>Aproximace Bézierovy křivky do Kruhové oblouky

Někdy je vhodné použít k vykreslení na kruhový oblouk Bézierovy křivky. Kubické Bézierovy křivky můžete ji odhadnout kruhového oblouku velmi dobře až čtvrtletí kruh, aby čtyři připojených Bézierových křivek můžete definovat celý kruh. Této aproximace je podrobněji popsána dvě články publikované před více než 25 let:

> Tor Dokken, et al. "Dobrá aproximace kruhy tak zaoblení průběžné Bézierových křivek" *počítače spouštějte geometrické návrh 7* (1990), 33 41.

> Michael Goldapp, "Aproximace Kruhové oblouky podle Kubické Polynomials" *počítače projektování geometrické 8* (1991), 227 238.

Následující diagram znázorňuje čtyři body s popiskem `pto`, `pt1`, `pt2`, a `pt3` definování Bézierovy křivky (zobrazené červeně), který aproximuje kruhového oblouku:

![](beziers-images/bezierarc45.png "Aproximace kruhového oblouku s Bézierovy křivky")

Řádky z počátečního a koncového bodu kontrolním bodům jsou arkustangens kruhu a Bézierovy křivky a mít délku *L*. První článku uvedeném výše označuje, že blíží nejlepší Bézierovy křivky na kruhový oblouk při takto dlouhou *L* se vypočítává takto:

L = 4 × tan(α / 4) / 3

Na obrázku ukazuje úhel 45 stupňů, takže L rovná 0.265. Tato hodnota by v kódu, vynásobený požadované poloměr kruhu.

**Kruhový oblouk na Bézierovu** stránce můžete experimentovat s definováním Bézierovy křivky aproximace kruhového oblouku úhlů rozsahu až o 180 stupňů. [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) vytvoří soubor `SKCanvasView` a `Slider` pro výběr úhlu. `PaintSurface` Obslužné rutině událostí ve [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) soubor kódu na pozadí používá transformace nastavit bod (0, 0) na střed plátna. Nakreslí na střed v tomto bodě pro porovnání a vypočítá jeho dvou kontrolních bodů pro Bézierovy křivky:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 3;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate length of control point line
    float length = radius * 4 * (float)Math.Tan(Math.PI * angle / 180 / 4) / 3;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the end points
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point3 = new SKPoint(radius * sin, radius * cos);

    // Find the control points
    SKPoint point0Normalized = Normalize(point0);
    SKPoint point1 = point0 + new SKPoint(length * point0Normalized.Y,
                                          -length * point0Normalized.X);

    SKPoint point3Normalized = Normalize(point3);
    SKPoint point2 = point3 + new SKPoint(-length * point3Normalized.Y,
                                          length * point3Normalized.X);

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);
    canvas.DrawCircle(point3.X, point3.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point3.X, point3.Y, point2.X, point2.Y, dottedStroke);

    // Draw the Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.CubicTo(point1, point2, point3);
        canvas.DrawPath(path, redStroke);
    }
}

// Vector methods
SKPoint Normalize(SKPoint v)
{
    float magnitude = Magnitude(v);
    return new SKPoint(v.X / magnitude, v.Y / magnitude);
}

float Magnitude(SKPoint v)
{
    return (float)Math.Sqrt(v.X * v.X + v.Y * v.Y);
}

```

Počáteční a koncový bod (`point0` a `point3`) se počítají na normální parametrické rovnice kruhu. Protože je na střed kruhu (0, 0), tyto body lze také považovat za paprskového vektory od středu kruhu k obvodu. Kontrolní body jsou na řádky, které jsou arkustangens kruh, tak, aby byly v pravém úhlu do těchto paprskového vektorů. Vektor v pravém úhlu je jednoduše původního vektoru se Prohodit souřadnice X a Y a jeden z nich provedli záporné.

Tady je program běžící na třech platformách pomocí tří různých úhlů:

[![](beziers-images/beziercirculararc-small.png "Trojitá snímek obrazovky stránky kruhový oblouk na Bézierovu")](beziers-images/beziercirculararc-large.png#lightbox "Trojitá snímek obrazovky stránky kruhového oblouku Bézierovy křivky")

Prohlédněte si blíže třetí snímek obrazovky a uvidíte, že Bézierovy křivky zejména odchylují od polokruhu při 180stupňový rozsah s orientací je úhel, ale iOS obrazovka ukazuje, že to vypadá podle čtvrtkruh zcela v pořádku, pokud je úhel 90 stupňů.

Výpočet souřadnice dvě kontrolních bodů je poměrně jednoduché, když čtvrtkruh je orientovaný takto:

![](beziers-images/bezierarc90.png "Aproximace čtvrtletí kroužek s Bézierovy křivky")

Pokud poloměr kruhu je 100, *L* je 55, tedy číslo snadno pamatovat.

**Umocňování kruhu** stránky animuje obrázek až kruh, čtverec. Kruhu je aproximována jehož souřadnice jsou uvedeny v prvním sloupci tuto definici pole v čtyři Bézierových křivek [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) třídy:

```csharp
public class SquaringTheCirclePage : ContentPage
{
    SKPoint[,] points =
    {
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() },
        { new SKPoint(  55,  100), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,   55), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,    0), new SKPoint(   125,      0), new SKPoint() },
        { new SKPoint( 100,  -55), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(  55, -100), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(   0, -100), new SKPoint(     0,   -125), new SKPoint() },
        { new SKPoint( -55, -100), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,  -55), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,    0), new SKPoint(  -125,      0), new SKPoint() },
        { new SKPoint(-100,   55), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint( -55,  100), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() }
    };
    ...
}
```

Druhý sloupec obsahuje souřadnice čtyři Bézierových křivek, které definují čtverec, jejichž oblasti je přibližně stejné jako obsah kruhu. (Kreslení čtverec se *přesné* oblast jako daný kruhu je classic neřešitelné geometrické problém [umocňování kruhu](https://en.wikipedia.org/wiki/Squaring_the_circle).) Pro vykreslení čtverec s Bézierových křivek dvě kontrolních bodů pro každé křivky jsou stejné a jsou colinear s počáteční a koncový bod, takže Bézierovy křivky se vykreslí jako rovné čáry.

Třetí sloupec pole je pro hodnoty interpolovaná pro animaci. Stránka nastaví časovače 16 milisekund a `PaintSurface` obslužná rutina je volána v tomto kurzu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    canvas.Translate(info.Width / 2, info.Height / 2);
    canvas.Scale(Math.Min(info.Width / 300, info.Height / 300));

    // Interpolate
    TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
    float t = (float)(timeSpan.TotalSeconds % 3 / 3);   // 0 to 1 every 3 seconds
    t = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;     // 0 to 1 to 0 sinusoidally

    for (int i = 0; i < 13; i++)
    {
        points[i, 2] = new SKPoint(
            (1 - t) * points[i, 0].X + t * points[i, 1].X,
            (1 - t) * points[i, 0].Y + t * points[i, 1].Y);
    }

    // Create the path and draw it
    using (SKPath path = new SKPath())
    {
        path.MoveTo(points[0, 2]);

        for (int i = 1; i < 13; i += 3)
        {
            path.CubicTo(points[i, 2], points[i + 1, 2], points[i + 2, 2]);
        }
        path.Close();

        canvas.DrawPath(path, cyanFill);
        canvas.DrawPath(path, blueStroke);
    }
}
```

Body jsou interpolovány na základě sinusoidally provozních hodnoty `t`. Interpolované body se pak používají k vytvořit sérii čtyři připojených Bézierovy křivky. Tady je animace spuštěna na třech platformách zobrazující průběh kruh čtverec:

[![](beziers-images/squaringthecircle-small.png "Trojitá snímek obrazovky Squaring stránce kruh")](beziers-images/squaringthecircle-large.png#lightbox "Trojitá snímek obrazovky Squaring stránce kruh")

Tyto animace by jinak nebylo možné bez algorithmically dostatečně flexibilní, aby se vykresluje jako Kruhové oblouky a rovné čáry, křivky.

**Bézierovy křivky nekonečno** stránka také využívá výhod schopnost Bézierovy křivky přibližný kruhového oblouku. Tady je `PaintSurface` obslužnou rutinu z [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) třídy:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.MoveTo(0, 0);                                // Center
        path.CubicTo(  50,  -50,   95, -100,  150, -100); // To top of right loop
        path.CubicTo( 205, -100,  250,  -55,  250,    0); // To far right of right loop
        path.CubicTo( 250,   55,  205,  100,  150,  100); // To bottom of right loop
        path.CubicTo(  95,  100,   50,   50,    0,    0); // Back to center  
        path.CubicTo( -50,  -50,  -95, -100, -150, -100); // To top of left loop
        path.CubicTo(-205, -100, -250,  -55, -250,    0); // To far left of left loop
        path.CubicTo(-250,   55, -205,  100, -150,  100); // To bottom of left loop
        path.CubicTo( -95,  100,  -50,   50,    0,    0); // Back to center
        path.Close();

        SKRect pathBounds = path.Bounds;
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.9f * Math.Min(info.Width / pathBounds.Width,
                                     info.Height / pathBounds.Height));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 5;

            canvas.DrawPath(path, paint);
        }
    }
}
```

Může být vhodné výkonu k vykreslení tyto souřadnice na papír grafu chcete zobrazit, jak spolu souvisí. Znaménko nekonečno je zaměřená na bod (0, 0), a dvě smyčky mají centra (–150, 0) a (150, 0) a poloměr 100. V řadě `CubicTo` příkazy, zobrazí se kontrolních bodů s ohledem na hodnoty –95 a –205 souřadnice X (tyto hodnoty jsou –150 plus a minus 55), 205 a 95 (150 plus a minus 55), stejně jako 250 a –250 pro levé a pravé strany. Jedinou výjimkou je při přihlašování nekonečno protíná sám v centru. V takovém případě kontrolní body mají souřadnice s kombinací 50 a -50, vyrovnejte křivky uprostřed.

Tady je znak nekonečno na všech třech platformách:

[![](beziers-images/bezierinfinity-small.png "Trojitá snímek obrazovky stránky Bézierovy nekonečno")](beziers-images/bezierinfinity-large.png#lightbox "Trojitá snímek obrazovky stránky nekonečno Bézierovy")

Je trochu hladší směrem do středu znaménko nekonečno vykreslený **oblouk nekonečno** stránku ze [ **tři způsoby, jak nakreslit oblouk** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) článku.

## <a name="the-quadratic-bzier-curve"></a>Kvadratické Bézierovy křivky

Kvadratické Bézierovy křivky má pouze jeden ovládací prvek bod a křivka je určené jenom tři body: počáteční bod, bod ovládacího prvku a koncový bod. Parametrické rovnice jsou velmi podobné kubické Bézierovy křivky, s tím rozdílem, že nejvyšší exponent je 2, tak křivka je kvadratické mnohočlenu:

x(t) = (1 – t) ²x₀ + 2t (1 – t) x₁ + t²x₂

y(t) = (1 – t) ²y₀ + 2t (1 – t) y₁ + t²y₂

Chcete-li přidat kvadratické Bézierovy křivky na cestu, použijte [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) metoda nebo [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/System.Single/System.Single/System.Single/System.Single/) přetížení samostatné `x` a `y` souřadnice:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

Metody přidat křivku z aktuální pozici, aby `point2` s `point1` jako řídicí bod.

Můžete experimentovat s kvadratické Bézierovy křivky **Kvadratická křivka** stránku, což je velmi podobný **Bézierovu křivku** stránky s tím rozdílem, obsahuje pouze tři dotykovými body. Tady je `PaintSurface` obslužné rutiny v [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) soubor kódu na pozadí:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with quadratic Bezier
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.QuadTo(touchPoints[1].Center,
                    touchPoints[2].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

A tady je spuštěn na všech třech platformách:

[![](beziers-images/quadraticcurve-small.png "Trojitá snímek obrazovky stránky Kvadratická křivka")](beziers-images/quadraticcurve-large.png#lightbox "Trojitá snímek obrazovky stránky Kvadratická křivka")

Tečkované čáry jsou arkustangens na křivku na počáteční a koncový bod a splňovat okamžiku ovládacího prvku.

Kvadratické Bézierovy je výhodné, pokud potřebujete obecné tvaru křivky, ale dáváte přednost pohodlí bodu pouze jeden ovládací prvek, místo dvou. Kvadratické Bézierovy vykreslí efektivnější než všechny ostatní křivky, což je důvod, proč používá se interně v Skia k vykreslení eliptické oblouky.

Ale tvar kvadratické Bézierovy křivky není elipsy, což je důvod, proč je potřeba více kvadratické Béziers přibližný oblouku elipsy. Kvadratické Bézierovy je místo toho segment parabolicky.

## <a name="the-conic-bzier-curve"></a>Conic Bézierovy křivky

Conic Bézierovy křivky &mdash; označované také jako rozumné kvadratické Bézierovy křivky &mdash; je poměrně nedávný dodatek řady Bézierovy křivky. Stejně jako kvadratické Bézierovy křivky racionální kvadratické Bézierovy křivky zahrnuje počáteční bod, koncový bod a bod jeden ovládací prvek. Ale také vyžaduje racionální kvadratické Bézierovy křivky *váha* hodnotu. Je volána *racionální* kvadratické, protože ukazatelů vzorce zahrnují poměry.

Parametrické rovnice X a Y jsou poměry, které sdílejí stejnou jmenovatel. Tady je rovnice faktorem pro *t* rozsahu od 0 do 1 a hodnota váhy *w*:

d(t) = (1 – t) ² + 2wt(1 – t) + t²

Teoreticky vzato racionální kvadratické může zahrnovat tří hodnot váhu samostatné, jeden pro každou tří podmínek, ale ty se dá zjednodušit na pouze jednu hodnotu váhy výraz střední.

Parametrické rovnice pro souřadnice X a Y jsou podobné parametrické rovnice pro kvadratické Bézierovy s tím rozdílem, že výraz střední také zahrnuje hodnota váhy a výrazu, je vyděleno hodnotou jmenovatele:

x(t) = ((1 – t) ²x₀ + 2wt (1 – t) x₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t) ²y₀ + 2wt (1 – t) y₁ + t²y₂)) ÷ d(t)

Racionální kvadratické Bézierovy křivky se také označují jako *conics* které přesně představují segmenty každé části conic &mdash; hyperboly, paraboly, symbol tří teček a kruzích.

Chcete-li přidat racionální kvadratické Bézierovy křivky na cestu, použijte [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) metoda nebo [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) přetížení samostatné `x` a `y` souřadnice:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

Všimněte si, že poslední `weight` parametru.

**Conic křivky** stránce můžete experimentovat s těmito křivky. `ConicCurvePage` Třída odvozena z `InteractivePage`. [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) vytvoří soubor `Slider` vyberte hodnotu váhy – 2 až 2. [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) soubor kódu na pozadí vytvoří tři `TouchPoint` objekty a `PaintSurface` obslužná rutina jednoduše vykreslí výsledná křivka s tečny do ovládacího prvku body:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with conic curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.ConicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     (float)weightSlider.Value);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

Tady je spuštěn na všech třech platformách:

[![](beziers-images/coniccurve-small.png "Trojitá snímek obrazovky stránky Conic křivky")](beziers-images/coniccurve-large.png#lightbox "Trojitá snímek obrazovky stránky Conic křivky")

Jak je vidět řídicí bod zdá se, že o přijetí změn křivky směrem k jeho další, když je váha vyšší. Když váha je nula, stane se křivku rovnou linii od počátečního bodu na koncový bod.

Teoreticky vzato negativní váhy jsou povolené a způsobit, že křivky ohybem *okamžitě* z bodu ovládacího prvku. Ale oceňuje – 1 nebo pod příčina jmenovatel v parametrické rovnice na záporný pro konkrétní hodnoty *t*. Z tohoto důvodu se pravděpodobně negativní vah se ignorují v `ConicTo` metody. **Conic křivky** program umožňuje nastavit váhu záporná, ale jak je vidět Experimentováním negativní váhy má stejný účinek jako váhu nula a způsobit, že mají být vykresleny rovné čáry.

Velice snadno se odvodit řídicí bod a váha používaná `ConicTo` metodu pro až nakreslit kruhového oblouku (ale bez zahrnutí) polokruhu. V následujícím diagramu schází tečného řádky z počátečního a koncového bodu řídicí bod.

![](beziers-images/conicarc.png "Vykreslení conic oblouk na kruhový oblouk")

Trigonometrické můžete použít k určení vzdálenost řídicí bod od středu kruhu: je poloměr kruhu dělený kosinus úhlu poloviční α. Chcete-li nakreslit na kruhový oblouk mezi počáteční a koncový bod, nastavte váhu na tento stejný kosinus poloviční úhlu. Všimněte si, pokud 180stupňový rozsah s orientací je úhel, pak nikdy nesplní tečný řádky a váhu je nula. Ale pro úhly menší než 180 stupňů, výpočty funguje správně.

**Conic kruhového oblouku** stránce ukazuje to. [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) vytvoří soubor `Slider` pro výběr úhlu. `PaintSurface` Obslužné rutiny v [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) soubor kódu na pozadí vypočítá řídicí bod a váhu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 4;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the points and weight
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point1 = new SKPoint(0, radius / cos);
    SKPoint point2 = new SKPoint(radius * sin, radius * cos);
    float weight = cos;

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point2.X, point2.Y, point1.X, point1.Y, dottedStroke);

    // Draw the conic
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.ConicTo(point1, point2, weight);
        canvas.DrawPath(path, redStroke);
    }
}
```

Jak je vidět, není žádný vizuální rozdíl mezi `ConicTo` cestu zobrazené červeně a základní kruh zobrazí pro referenci:

[![](beziers-images/coniccirculararc-small.png "Trojitá snímek obrazovky stránky Conic kruhového oblouku")](beziers-images/coniccirculararc-large.png#lightbox "Trojitá snímek obrazovky stránky Conic kruhový oblouk")

Ale nastavit úhel 180stupňový rozsah s orientací a matematiky selhání.

Je v tomto případě unfortunate, který `ConicTo` nepodporuje záporné váhy, protože teoreticky (založená na parametrické rovnice) můžete kruhu dokončeno s jiným voláním metody `ConicTo` stejné body, ale zápornou hodnotu váhy. To by umožnilo vytváření celý kruh právě dva `ConicTo` křivky založené na každý úhel mezi (ale nikoli včetně) nula stupňů a o 180 stupňů.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
