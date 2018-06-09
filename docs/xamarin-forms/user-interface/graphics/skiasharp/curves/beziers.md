---
title: Tři typy Bézierových křivek
description: Tento článek vysvětluje, jak chcete použít k vykreslení krychlový, kvadratické a conic Bézierových křivek v aplikacích Xamarin.Forms SkiaSharp a to ukazuje s ukázkový kód.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: charlespetzold
ms.author: chape
ms.date: 05/25/2017
ms.openlocfilehash: 4a1b86035f9ce31b6e9fafac06cd0090a516b542
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244003"
---
# <a name="three-types-of-bzier-curves"></a>Tři typy Bézierových křivek

_Použití SkiaSharp k vykreslení krychlový, kvadratické a conic Bézierových křivek_

Bézierovy křivky jmenuje po Pierre Bézierovy (1910 – 1999), francouzštině pracovníkem v automobilu společnosti Renault, kdo používá křivku návrh s asistencí počítače car subjektů.

Bézierovy křivky se ví, že pro se dobře hodí pro interaktivní návrhu: jsou dobře behaved &mdash; jinými slovy, nejsou k dispozici singularities, které způsobí křivku k nekonečné nebo nepraktické &mdash; a jsou obecně vkusnou . Obsahuje přehled znak na počítači, jaká písma jsou obvykle definovány s Bézierových křivek:

![](beziers-images/beziersample.png "Ukázka Bézierovy křivky")

Článek Wikipedia na [Bézierovy křivky](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) obsahuje některé užitečné informace. Termín *Bézierovy křivky* ve skutečnosti odkazuje na řadu podobné křivek. SkiaSharp podporuje tři typy Bézierových křivek, volá se *krychlový*, *kvadratické*a *conic*. Conic je také označován jako *rozumné Kvadratická*.

## <a name="the-cubic-bzier-curve"></a>Krychlový Bézierovy křivky

Krychlový je typ Bézierovy křivky, který Většina vývojářů zamyslet nad po předmět Bézierových křivek.

Můžete přidat krychlový Bézierovy křivky do `SKPath` pomocí [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) metoda s třemi `SKPoint` parametry, nebo [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/System.Single/) přetížení s samostatné `x` a `y` parametry:

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

Křivku začne k aktuálnímu bodu Kontury. Dokončení krychlový Bézierovy křivky je definováno čtyři body:

- počátečního bodu: aktuální příkaz v průběhu, nebo (0, 0), pokud `MoveTo` nebyla zavolána
- nejprve řídicí bod: `point1` v `CubicTo` volání
- druhý řídicí bod: `point2` v `CubicTo` volání
- koncový bod: `point3` v `CubicTo` volání

Výsledná křivky začíná na počáteční bod a končí na koncový bod. Křivku obecně nepředává prostřednictvím dvou kontrolních bodů; Místo toho fungují mnohem like magnets vyžádání křivky směrem je.

Nejlepší způsob, jak podívat krychlový Bézierovy křivky je experimenty. Toto je účelem **Bézierovu křivku** stránky, která je odvozena z `InteractivePage`. [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) soubor vytvoří `SKCanvasView` a `TouchEffect`. [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) souboru kódu vytvoří čtyři `TouchPoint` objekty v jeho konstruktoru. `PaintSurface` Vytvoří obslužnou rutinu události `SKPath` k vykreslení Bézierovy křivky založené na čtyři `TouchPoint` objektů a také nevykresluje desítkovém tečný řádky z kontrolních bodů do koncových bodů:

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

Zde je spuštěn na všechny tři platformy:

[![](beziers-images/beziercurve-small.png "Trojitá snímek obrazovky stránky Bézierovu křivku")](beziers-images/beziercurve-large.png#lightbox "Trojitá snímek obrazovky stránky Bézierovu křivku")

Matematický křivka je krychlový polynomu. Křivku maximálně protíná přímku na tři body. U počáteční bod křivka je vždy tečný chcete a ve stejném směru jako přímka od začátku, přejděte na první kontrolního bodu. Na koncový bod křivka je vždy tečný chcete a ve stejném směru jako přímka z ovládacího prvku druhý přejděte na koncový bod.

Krychlový Bézierovy křivky je vždy ohraničené konvexní čtyřúhelník připojení čtyři body. Tento postup se nazývá *konvexní trupu*. Pokud kontrolní body leží na přímku mezi počátečním a koncovým bodem, Bézierovy křivky vykreslí jako přímka. Ale křivku můžete také mezi samostatně, protože třetí snímek obrazovky ukazuje.

Obrysem cesta může obsahovat více Bézierových křivek krychlový připojené, ale bude připojení mezi dvěma krychlový Bézierových křivek smooth pouze v případě, že následující tři body jsou colinear (tedy leží na přímku):

- druhý řídicí bod první křivky
- koncový bod první křivky, což je také počáteční bod druhý křivky
- První řídicí bod druhý křivky

V následující článek na [ **Data cesty SVG** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) dozvíte budovy k usnadnění definici hladké připojené Bézierovy křivky.

Někdy je užitečné znát základní čištění vzorce, které vykreslení krychlový Bézierovy křivky. Pro *t* rozsahu od 0 do 1, čištění vzorce jsou následující:

x(t) = (1 – t) ³x₀ + 3t (1 – t) ²x₁ + 3t² (1 – t) x₂ + t³x₃

y(t) = (1 – t) ³y₀ + 3t (1 – t) ²y₁ + 3t² (1 – t) y₂ + t³y₃

Nejvyšší exponent 3 potvrdí, že jsou krychlový polynomials. Je snadné ověřte, že když `t` rovná 0, je bod je (x₀, y₀), což je počáteční bod a kdy `t` hodnotu 1, bod je (x₃, y₃), což je koncový bod. V blízkosti počáteční bod (pro nízké hodnoty `t`), první řídicí bod (x₁, y₁) má silné ovlivňuje a v blízkosti koncového bodu (vysoké hodnoty, které se ") druhý řídicí bod (x₂, y₂) má silné vliv.

## <a name="bzier-curve-approximation-to-circular-arcs"></a>Kruhové oblouky aproximace Bézierovy křivky

Někdy je vhodnější použít k vykreslení kruhového oblouku Bézierovy křivky. Krychlový Bézierovy křivky můžete Přibližná kruhového oblouku velmi dobře až kruh čtvrtletí, čtyři připojené Bézierových křivek můžete definovat celou kruh. Tato aproximace část dva články publikované před více než 25 let:

> Tor Dokken, a další "Dobré aproximace kroužky podle zakřivení průběžné Bézierových křivek," *počítače podporovaná geometrickou návrh 7* (1990), 33 41.

> Michael Goldapp, "Aproximace Kruhové oblouky podle krychlový Polynomials" *počítače podporovaná geometrickou návrh 8* (1991), 227 238.

Následující diagram znázorňuje čtyři body s názvem bez přípony `pto`, `pt1`, `pt2`, a `pt3` definování Bézierovy křivky (zobrazené červeně), který se blíží kruhového oblouku:

![](beziers-images/bezierarc45.png "Aproximace kruhového oblouku s Bézierovy křivky")

Řádky od počáteční a koncové body, které se kontrolní body jsou tangens na kruh a Bézierovy křivky a mají délku *L*. První článek citovalo výše označuje, že nejlepší Bézierovy křivky blíží kruhového oblouku při dlouhou *L* se počítá takto:

L = 4 × tan(α / 4) / 3

Na obrázku úhlu 45 stupňů, takže L rovná 0.265. V kódu by tato hodnota vynásobí požadované radius kruhu.

**Kruhový oblouk na Bézierovu** stránce můžete experimentovat s definování Bézierovy křivky sblížit kruhového oblouku pro úhly rozsahu až o 180 stupňů. [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) soubor vytvoří `SKCanvasView` a `Slider` pro výběr úhel. `PaintSurface` Obslužné rutiny událostí v [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) souboru kódu použije transformace pro nastavení bodu (0, 0) na střed plátna. Nakreslí zarovnaný na střed v tomto bodě pro porovnání a pak vypočítá dvě kontrolních bodů pro Bézierovy křivky:

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

Počáteční a koncový bod (`point0` a `point3`) se vypočítává podle normálního čištění vzorce pro kruhu. Protože je umístěn na střed kruhu v (0, 0), tyto body lze také zacházet jako paprskového vektory z střed kruhu k obvodu. Kontrolní body jsou na řádky, které jsou tangens na kruh, tak, aby byly v pravém úhlu tyto paprskového Vektorům. Vektor v pravém úhlu je jednoduše původní vektoru s souřadnice X a Y, které jsou vzájemně zaměněny a jeden z nich provedené záporné.

Tady je programy spuštěné na tři platforem pomocí tří různých úhlů:

[![](beziers-images/beziercirculararc-small.png "Trojitá snímek obrazovky stránky kruhový oblouk na Bézierovu")](beziers-images/beziercirculararc-large.png#lightbox "Trojitá snímek obrazovky stránky Bézierovy kruhový oblouk")

Prohlédněte si blíže třetí snímek obrazovky a uvidíte, že Bézierovy křivky zejména odchylují od polokruhu když úhel je 180 stupňů, ale na obrazovce iOS ukazuje, že nejspíš vyhovoval čtvrtletí kruh stejně dobře, když úhel je 90 stupňů.

Výpočet souřadnice dvě kontrolních bodů je poměrně snadné, když čtvrtletí kroužek je orientované takto:

![](beziers-images/bezierarc90.png "Aproximace čtvrtletí kroužkem Bézierovy křivky")

Pokud radius kruhu je 100, *L* 55, a představuje počet snadno pamatovat.

**Umocněním kruhu** stránky animuje obrázek až čtverce kruh. Kruhu je sblížit podle jehož souřadnice jsou uvedeny v první sloupec tuto definici pole v čtyři Bézierových křivek [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) třídy:

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

Druhý sloupec obsahuje souřadnice čtyři Bézierových křivek, které definují čtverce, jehož je přibližně stejné jako oblasti kruhu. (Kreslení čtverce s *přesný* oblast jako dané kroužek je classic nevyřešené geometrickou problém [umocněním kruhu](https://en.wikipedia.org/wiki/Squaring_the_circle).) Pro vykreslování čtverce s Bézierových křivek, dva kontrolní body pro každý křivky jsou stejné a jsou colinear s počáteční a koncový bod, takže Bézierovy křivky je vykreslen jako přímka.

Třetí sloupec pole je interpolované hodnoty pro animace. Stránka nastaví časovač pro 16 milisekund a `PaintSurface` obslužná rutina je volána v tomto kurzu:

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

Body interpolace podle sinusoidally provozních hodnotu `t`. Interpolované body se pak používají k vytvoření řadu čtyři připojené Bézierových křivek. Tady je animace spuštěna na tři platformách zobrazující průběh z kruhu na čtverce:

[![](beziers-images/squaringthecircle-small.png "Trojitá snímek obrazovky Squaring stránce kruh")](beziers-images/squaringthecircle-large.png#lightbox "Trojitá snímek obrazovky Squaring stránce kruhu.")

Takové animace bude možné bez křivek, které jsou algorithmically dostatečně flexibilní, aby se vykresluje jako Kruhové oblouky a rovné čáry.

**Bézierovy Infinity** stránky také využívá výhod možnost Bézierovy křivky Přibližná kruhového oblouku. Tady je `PaintSurface` obslužnou rutinu na základě [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) třídy:

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

Může to být dobrým cvičení k vykreslení tyto souřadnice na graf dokumentu chcete zobrazit, jak spolu souvisí. Infinity přihlášení je zaměřená na bod (0, 0) a dvě smyčky mít středy (–150, 0) a (150, 0) a poloměr 100. V řadě `CubicTo` příkazy, uvidíte X souřadnice kontrolních bodů s ohledem na hodnoty –95 a –205 (tyto hodnoty jsou –150 plus a minus 55), 205 a 95 (150 plus a minus 55), a také 250 a –250 pro pravé a levé strany. Jedinou výjimkou je, když přihlašovací infinity protíná sám sebe v centru. V takovém případě kontrolní body mají souřadnice s kombinací 50 a -50, vyrovnejte křivku téměř centru.

Tady je přihlašovací infinity na všech tří platformách:

[![](beziers-images/bezierinfinity-small.png "Trojitá snímek obrazovky stránky Bézierovy Infinity")](beziers-images/bezierinfinity-large.png#lightbox "Trojitá snímek obrazovky stránky Bézierovy Infinity")

Je poněkud hladší směrem k centru než infinity přihlašovací poskytnutý **oblouk Infinity** stránku z [ **tři způsoby nakreslit oblouk** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) článku.

## <a name="the-quadratic-bzier-curve"></a>Kvadratické Bézierovy křivky

Kvadratické Bézierovy křivky má pouze jeden prvek bod a křivka je definovaný jenom tři body: počáteční bod, bod řízení a koncový bod. Kromě toho, že nejvyšší exponent je 2, takže křivka je kvadratické polynomu jsou velmi podobné krychlový Bézierovy křivky čištění vzorce:

x(t) = (1 – t) ²x₀ + 2t (1 – t) x₁ + t²x₂

y(t) = (1 – t) ²y₀ + 2t (1 – t) y₁ + t²y₂

Chcete-li přidat kvadratické Bézierovy křivky na cestu, použijte [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) metoda nebo [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/System.Single/System.Single/System.Single/System.Single/) přetížení s samostatné `x` a `y` souřadnice:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

Metody přidat křivku z aktuální pozici k `point2` s `point1` jako řídicí bod.

Můžete experimentovat s kvadratických Bézierových křivek **kvadratické křivky** stránky, což je velmi podobné **Bézierovu křivku** stránky s výjimkou má jenom tři body dotykového ovládání. Tady je `PaintSurface` obslužné rutiny v [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) souboru kódu na pozadí:

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

A zde je spuštěn na všechny tři platformy:

[![](beziers-images/quadraticcurve-small.png "Trojitá snímek obrazovky stránky kvadratické křivky")](beziers-images/quadraticcurve-large.png#lightbox "Trojitá snímek obrazovky stránky kvadratické křivky")

Čáry s koncovými body jsou tangens na křivku na počáteční a koncový bod a splňují okamžiku ovládacího prvku.

Kvadratické Bézierovy je vhodný, pokud potřebujete křivky obecné tvaru, ale přednost pohodlí jenom jeden řídicí bod, místo dvou. Kvadratické Bézierovy vykreslí efektivnější než jakékoli jiné křivku, proto ji se používá interně v Skia k vykreslení eliptické oblouky.

Ale obrazec kvadratické Bézierovy křivky není eliptické, proto více kvadratické Béziers jsou nutné k Přibližná eliptické oblouk. Kvadratické Bézierovy se místo toho segment parabolicky.

## <a name="the-conic-bzier-curve"></a>Conic Bézierovy křivky

Conic Bézierovy křivky &mdash; také označované jako rozumné kvadratické Bézierovy křivky &mdash; je poměrně poslední přidání do rodiny Bézierových křivek. Jako kvadratické Bézierovy křivky rozumné kvadratické Bézierovy křivky zahrnuje počáteční bod, koncový bod a bod jeden prvek. Ale je potřeba rozumné kvadratické Bézierovy křivky *váhy* hodnotu. Je volána *rozumné* kvadratické totiž čištění vzorce poměry.

Čištění vzorce pro X a Y jsou poměr, které sdílejí stejnou jmenovatel. Tady je vztah pro jmenovatel pro *t* rozsahu od 0 do 1 a hodnota váhy *w*:

d(t) = (1 – t) ² + 2wt(1 – t) + t²

Teoreticky rozumné Kvadratická zahrnuje tři samostatné váhy hodnoty, jeden pro každou tři podmínky, ale tyto můžete zjednodušit pouze jednu hodnotu váhy výraz střední.

Čištění vzorce pro souřadnice X a Y jsou podobné čištění vzorce pro kvadratické Bézierovy s tím rozdílem, že výraz střední také zahrnuje hodnota váhy a výraz se vydělí jmenovatel:

x(t) = ((1 – t) ²x₀ + 2wt (1 – t) x₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t) ²y₀ + 2wt (1 – t) y₁ + t²y₂)) ÷ d(t)

Rozumné kvadratických Bézierových křivek se také označují jako *conics* protože mohou přesně reprezentovat segmenty každé části conic &mdash; hyperboly, paraboly, tři tečky a kroužky.

Chcete-li přidat rozumné kvadratické Bézierovy křivky na cestu, použijte [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) metoda nebo [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) přetížení s samostatné `x` a `y` souřadnice:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

Všimněte si, že posledních `weight` parametr.

**Conic křivky** stránce můžete experimentovat s tyto křivky. `ConicCurvePage` Třída odvozená z `InteractivePage`. [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) vytvoří soubor `Slider` vyberte hodnotu váhy mezi – 2 a 2. [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) souboru kódu vytvoří tři `TouchPoint` objekty a `PaintSurface` obslužná rutina jednoduše vykreslí výsledné křivky tečný řádků pro ovládací prvek body:

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

Zde je spuštěn na všechny tři platformy:

[![](beziers-images/coniccurve-small.png "Trojitá snímek obrazovky stránky Conic křivky")](beziers-images/coniccurve-large.png#lightbox "Trojitá snímek obrazovky stránky Conic křivky")

Jak vidíte, řídicí bod zdá se, že když vyšší váhou pro vyžádání obsahu křivky směrem ho Další. Když váhu nulová, stane se křivku přímky z počáteční bod pro koncový bod.

Teoreticky záporné váhu jsou povolené a způsobit křivku na ohybem *rychle* z kontrolního bodu. Ale provede – 1 nebo pod příčina jmenovatel v čištění vzorce se záporné pro konkrétní hodnoty *t*. Z tohoto důvodu pravděpodobně záporné vah se ignorují v `ConicTo` metody. **Conic křivky** program umožňuje nastavit záporné váhy, ale jak můžete vidět pomocí experimentování, záporné váhu nemají stejného efektu jako váhu nula a způsobit přímku k vykreslení.

Je velmi snadné odvození kontrolního bodu a váhy používat `ConicTo` metoda Kreslení kruhových oblouků až (s výjimkou) polokruhu. V následujícím diagramu tečný řádky z počátečního a koncového bodu schází kontrolního bodu.

![](beziers-images/conicarc.png "Vykreslení conic oblouk na kruhový oblouk")

Trigonometrické můžete použít k určení vzdálenost kontrolního bodu z centra na kruh: je radius dělený kosinus poloviční úhlu α kruhu. Kreslení kruhových oblouků mezi počáteční a koncový bod, nastavte váhu na tento stejný kosinus poloviční úhlu. Všimněte si, že pokud úhel 180 stupňů, pak tečný řádky nikdy nesplní a váhu je nulová. Ale pro úhly menší než 180 stupňů, výpočty funguje bez problémů.

**Conic kruhového oblouku** stránky ukazuje to. [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) vytvoří soubor `Slider` pro výběr úhel. `PaintSurface` Obslužné rutiny v [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) souboru kódu vypočítá kontrolního bodu a váhu:

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

Jak můžete vidět, není žádný visual rozdíl mezi `ConicTo` cestu zobrazené červeně a základní kruhu zobrazí pro referenci:

[![](beziers-images/coniccirculararc-small.png "Trojitá snímek obrazovky stránky Conic kruhového oblouku")](beziers-images/coniccirculararc-large.png#lightbox "Trojitá snímek obrazovky stránky Conic kruhový oblouk")

Ale nastavit úhel 180 stupňů a matematika selhání.

Je v tomto případě velice nepříjemná který `ConicTo` nepodporuje záporné váhu, protože teoreticky (podle čištění vzorce), můžete dokončit kruhu s jiným voláním `ConicTo` s stejné body ale zápornou hodnotu váhy. To by umožnilo vytváření celý kruh se právě dvěma `ConicTo` křivek založené na libovolný úhel mezi (s výjimkou) nula stupňů a o 180 stupňů.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
