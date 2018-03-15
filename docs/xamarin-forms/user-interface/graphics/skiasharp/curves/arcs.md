---
title: "Tři způsoby, jak nakreslit oblouk"
description: "Naučte se používat k definování oblouky třemi různými způsoby SkiaSharp"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: charlespetzold
ms.author: chape
ms.date: 05/10/2017
ms.openlocfilehash: 739efa994f172a7a1de82ac02d1c10b0d80f4c30
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="three-ways-to-draw-an-arc"></a>Tři způsoby, jak nakreslit oblouk

_Naučte se používat k definování oblouky třemi různými způsoby SkiaSharp_

Oblouk je křivku na obvodu elipsy, jako je například zaokrouhlené části infinity registrace:

![](arcs-images/arcsample.png "Infinity přihlášení")

Bez ohledu jednoduchost definice, neexistuje žádný způsob, jak definovat vykreslování oblouk funkce, která splňuje všechny potřeby a proto shoda mezi grafické systémy nejlepší způsob, jak nakreslit oblouk. Z tohoto důvodu `SKPath` třída neomezuje sám sebe na právě jeden z přístupů.

`SKPath` definuje `AddArc` metoda, pět různých `ArcTo` metody a dvě relativní `RArcTo` metody. Tyto metody spadají do tří kategorií, reprezentující tři velmi různý přístup k určení oblouk. Který, kterou použijete, závisí na informacích k dispozici pro definování oblouk a jak tato oblouk zapadá do dalších grafiky, který kreslení.

## <a name="the-angle-arc"></a>Úhel oblouku

Úhel oblouku přístupu k vykreslování oblouky je potřeba zadat obdélníku bounds elipsy. Oblouk na obvodu tento elipsy je indikován úhly z centra se třemi tečkami provedení začátek oblouk a jeho délka. Dvě různé metody kreslení oblouky úhlu. Jedná se o [ `AddArc` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) metoda a [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKRect/System.Single/System.Single/System.Boolean/) metoda:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Tyto metody jsou stejné jako pro Android [ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/) a [ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/) metody. IOS [ `AddArc` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArc/p/System.Boolean/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) metoda je podobný, ale je omezen na oblouky na obvodu kruhu místo zobecněny, aby elipsy.

Obě metody začínat `SKRect` hodnotu, která určuje umístění a velikost elipsy:

![](arcs-images/anglearcoval.png "Oval, která začíná úhel oblouku")

Oblouk je součástí obvodu tento třemi tečkami.

`startAngle` Argument je po směru hodinových ručiček úhel ve stupních relativně k na vodorovném řádku vykreslovány z centra se třemi tečkami vpravo. `sweepAngle` Argument je vzhledem k `startAngle`. Tady jsou `startAngle` a `sweepAngle` hodnoty 60 a 100 stupňů, v uvedeném pořadí:

![](arcs-images/anglearcangles.png "Úhly, které definují úhel oblouku")

Oblouk začíná na počáteční úhel. Úhel oblouku se řídí jeho délka:

![](arcs-images/anglearchighlight.png "Zvýrazněná úhel oblouku")

Křivky přidán do cesty s `AddArc` nebo `ArcTo` metoda je jednoduše ta část obvodu se třemi tečkami, tady zobrazené červeně:

![](arcs-images/anglearc.png "Úhel oblouku samostatně")

`startAngle` Nebo `sweepAngle` argumenty může být záporné: oblouk je po směru hodinových ručiček pro kladné hodnoty `sweepAngle` a proti směru hodinových ručiček pro záporné hodnoty.

Ale `AddArc` nemá *není* definovat uzavřené obrysem. Při volání `LineTo` po `AddArc`, řádek do bodu v vykreslením od konce oblouk `LineTo` metoda a stejné platí pro `ArcTo`.

`AddArc` automaticky spustí novou obrysem a je funkčně srovnatelný volání `ArcTo` s posledním argumentem `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Aby se nazývá poslední argument `forceMoveTo`, a efektivně způsobuje, že `MoveTo` volání na začátku oblouk. Nové obrysem, která začíná. To nemá s argumentem poslední ve `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Tato verze `ArcTo` nevykresluje řádek na začátek oblouk z aktuální pozici. To znamená, že oblouk mohou být někde uprostřed větší obrysem.

**Úhel oblouku** stránky umožňuje používat dvě posuvníky a určuje počáteční úhel oblouku. Vytvoří dvě souboru XAML instance `Slider` elementy a `SKCanvasView`. `PaintCanvas` Obslužné rutiny v [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) souboru nevykresluje elipsy a oblouk pomocí dvou `SKPaint` objekty definované jako pole:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
    float startAngle = (float)startAngleSlider.Value;
    float sweepAngle = (float)sweepAngleSlider.Value;

    canvas.DrawOval(rect, outlinePaint);

    using (SKPath path = new SKPath())
    {
        path.AddArc(rect, startAngle, sweepAngle);
        canvas.DrawPath(path, arcPaint);
    }
}
```

Jak vidíte, může trvat počáteční úhel a úhel oblouku na záporné hodnoty:

[![](arcs-images/anglearc-small.png "Trojitá snímek obrazovky stránky úhel oblouku")](arcs-images/anglearc-large.png#lightbox "Trojitá snímek obrazovky stránky úhel oblouku")

Tento přístup k generování oblouku je algorithmically nejjednodušší a je snadné odvození čištění vzorce, které popisují oblouk. Znalost, velikost a umístění se třemi tečkami a úhly počáteční a oblouku, počáteční a koncové body oblouku, lze vypočítat pomocí jednoduchého trigonometrické:

x = oval. MidX + (oval. Šířka / 2) * cos(angle)

y = oval. MidY + (oval. Výška / 2) * sin(angle)

`angle` Hodnota je buď `startAngle` nebo `startAngle + sweepAngle`.

Použití dvou úhlů k definování oblouku je nejvhodnější pro případy, pokud víte, úhlová délka oblouk, který chcete k vykreslení, třeba, aby výsečového grafu. **v kolekci rozložen výsečového grafu** stránky ukazuje to. [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) Třída používá interní třída pro definování některé kovodělných dat a barev:

```csharp
class ChartData
{
    public ChartData(int value, SKColor color)
    {
        Value = value;
        Color = color;
    }

    public int Value { private set; get; }

    public SKColor Color { private set; get; }
}

ChartData[] chartData =
{
    new ChartData(45, SKColors.Red),
    new ChartData(13, SKColors.Green),
    new ChartData(27, SKColors.Blue),
    new ChartData(19, SKColors.Magenta),
    new ChartData(40, SKColors.Cyan),
    new ChartData(22, SKColors.Brown),
    new ChartData(29, SKColors.Gray)
};

```

`PaintSurface` Obslužná rutina nejprve prochází položky k výpočtu `totalValues` číslo. Od ho můžete určit velikost jednotlivých položek, jako podíl celkového počtu a převést, úhel:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int totalValues = 0;

    foreach (ChartData item in chartData)
    {
        totalValues += item.Value;
    }

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float explodeOffset = 50;
    float radius = Math.Min(info.Width / 2, info.Height / 2) - 2 * explodeOffset;
    SKRect rect = new SKRect(center.X - radius, center.Y - radius,
                             center.X + radius, center.Y + radius);

    float startAngle = 0;

    foreach (ChartData item in chartData)
    {
        float sweepAngle = 360f * item.Value / totalValues;

        using (SKPath path = new SKPath())
        using (SKPaint fillPaint = new SKPaint())
        using (SKPaint outlinePaint = new SKPaint())
        {
            path.MoveTo(center);
            path.ArcTo(rect, startAngle, sweepAngle, false);
            path.Close();

            fillPaint.Style = SKPaintStyle.Fill;
            fillPaint.Color = item.Color;

            outlinePaint.Style = SKPaintStyle.Stroke;
            outlinePaint.StrokeWidth = 5;
            outlinePaint.Color = SKColors.Black;

            // Calculate "explode" transform
            float angle = startAngle + 0.5f * sweepAngle;
            float x = explodeOffset * (float)Math.Cos(Math.PI * angle / 180);
            float y = explodeOffset * (float)Math.Sin(Math.PI * angle / 180);

            canvas.Save();
            canvas.Translate(x, y);

            // Fill and stroke the path
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, outlinePaint);
            canvas.Restore();
        }

        startAngle += sweepAngle;
    }
}
```

Nový `SKPath` objekt se vytvoří pro každou řezu výseče. Cesta se skládá z řádku z centra, pak `ArcTo` k vykreslení oblouk a další čáru zpět do center výsledků `Close` volání. Tento program zobrazí "rozložený" výsečový řezy podle jejich přesunutí ven z centra × 50 pixelů. Tento úkol vyžaduje vektoru ve směru středový úhel oblouku pro každý řez:

[![](arcs-images/explodedpiechart-small.png "Trojitá snímek obrazovky stránky v kolekci rozložen výsečového grafu")](arcs-images/explodedpiechart-large.png#lightbox "Trojitá snímek obrazovky stránky v kolekci rozložen výsečového grafu")

Pokud chcete zobrazit, jak vypadá bez "nárůst", jednoduše komentář `Translate` volání:

[![](arcs-images/explodedpiechartunexploded-small.png "Trojitá snímek obrazovky stránky v kolekci rozložen výsečového grafu bez rozbalení")](arcs-images/explodedpiechartunexploded-large.png#lightbox "Trojitá snímek obrazovky stránky v kolekci rozložen výsečového grafu bez rozbalení")

## <a name="the-tangent-arc"></a>Tečný oblouk

Druhý typ oblouk nepodporuje `SKPath` je *tečný oblouk*, proto volat, protože oblouk je obvodu kruh, který je tangens dva připojené řádcích.

Tečný oblouk se přidá do cesty s volání [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) metoda se dvěma `SKPoint` parametry, nebo [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) přetížení s samostatné `Single` parametry pro body:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

To `ArcTo` metoda je podobná PostScript [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) – funkce (stránka 532 v dokumentu PDF) a iOS [ `AddArcToPoint` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArcToPoint/p/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) metoda.

`ArcTo` Metoda zahrnuje tři body:

- Aktuální příkaz Kontury nebo bodu (0, 0), pokud `MoveTo` nebyla zavolána
- První argument bodu `ArcTo` metodu, s názvem *rohu bodu*
- Druhý argument bodu `ArcTo`, zavolat *cílového bodu*:

![](arcs-images/tangentarcthreepoints.png "Tři body, které začínají tečný oblouk")

Tyto tři body definovat připojené dva řádky:

![](arcs-images/tangentarcconnectinglines.png "Řádky připojení tři body tečný oblouk")

Pokud jsou tři body colinear &mdash; to znamená, pokud jsou na stejné přímce v &mdash; žádné oblouk budou vykreslovat.

`ArcTo` Metoda také zahrnuje `radius` parametr. Definuje vlastnosti radius kruh:

![](arcs-images/tangentarccircle.png "Na kruh tečný oblouk")

Tečný oblouk není zobecněný pro elipsy.

Pokud dva řádky splňují v jakékoli úhlu, můžete tento kruh vložen mezi tyto řádky tak, aby se tangens i řádcích:

![](arcs-images/tangentarctangentcircle.png "Mezi dvěma čárami kruhu tečný oblouk")

Křivky, který je přidán do Kontury nemění buď bodů zadaných ve `ArcTo` metoda. Skládá se z přímky z aktuální bod prvního tečný bodu a oblouk, který končí v druhém tečný bodu:

![](arcs-images/tangentarchighlight.png "Zvýrazněná tečný oblouk mezi dvěma čárami")

Tady je poslední lineární a oblouk, který je přidán do Kontury:

![](arcs-images/tangentarc.png "Zvýrazněná tečný oblouk mezi dvěma čárami")

Kontury lze pokračovat z druhé tečný bodu.

**Tangens oblouk** stránce můžete experimentovat s tečný oblouk. Toto je první několik stránek, které jsou odvozeny od [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/InteractivePage.cs), která definuje několik užitečný `SKPaint` objekty a provádí `TouchPoint` zpracování:

```csharp
public class InteractivePage : ContentPage
{
    protected SKCanvasView baseCanvasView;
    protected TouchPoint[] touchPoints;

    protected SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3
    };

    protected SKPaint redStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 15
    };

    protected SKPaint dottedStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    };

    protected void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = baseCanvasView.CanvasSize.Width / (float)baseCanvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            baseCanvasView.InvalidateSurface();
        }
    }
}
```

`TangentArcPage` Třída odvozená z `InteractivePage`. Konstruktor v [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) souboru je odpovědná za vytváření instancí a inicializace `touchPoints` pole a nastavení `baseCanvasView` (v `InteractivePage`) do `SKCanvasView` vytvořena instance v objektu [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) souboru:

```csharp
public partial class TangentArcPage : InteractivePage
{
    public TangentArcPage()
    {
        touchPoints = new TouchPoint[3];

        for (int i = 0; i < 3; i++)
        {
            TouchPoint touchPoint = new TouchPoint
            {
                Center = new SKPoint(i == 0 ? 100 : 500,
                                     i != 2 ? 100 : 500)
            };
            touchPoints[i] = touchPoint;
        }

        InitializeComponent();

        baseCanvasView = canvasView;
        radiusSlider.Value = 100;
    }

    void sliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

`PaintSurface` Obslužná rutina používá `ArcTo` na základě metod k vykreslení oblouk na body touch a `Slider`, ale také algorithmically vypočítá kruhu úhel podle:

```csharp
public partial class TangentArcPage : InteractivePage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw the two lines that meet at an angle
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.LineTo(touchPoints[1].Center);
            path.LineTo(touchPoints[2].Center);
            canvas.DrawPath(path, dottedStrokePaint);
        }

        // Draw the circle that the arc wraps around
        float radius = (float)radiusSlider.Value;

        SKPoint v1 = Normalize(touchPoints[0].Center - touchPoints[1].Center);
        SKPoint v2 = Normalize(touchPoints[2].Center - touchPoints[1].Center);

        double dotProduct = v1.X * v2.X + v1.Y * v2.Y;
        double angleBetween = Math.Acos(dotProduct);
        float hypotenuse = radius / (float)Math.Sin(angleBetween / 2);
        SKPoint vMid = Normalize(new SKPoint((v1.X + v2.X) / 2, (v1.Y + v2.Y) / 2));
        SKPoint center = new SKPoint(touchPoints[1].Center.X + vMid.X * hypotenuse,
                                     touchPoints[1].Center.Y + vMid.Y * hypotenuse);

        canvas.DrawCircle(center.X, center.Y, radius, this.strokePaint);

        // Draw the tangent arc
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.ArcTo(touchPoints[1].Center, touchPoints[2].Center, radius);
            canvas.DrawPath(path, redStrokePaint);
        }

        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
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
}
```

Tady je **tangens oblouk** stránky běžící na všechny tři platformách:

[![](arcs-images/tangentarc-small.png "Trojitá snímek obrazovky stránky tangens oblouk")](arcs-images/tangentarc-large.png#lightbox "Trojitá snímek obrazovky stránky tangens oblouk")

Na zařízení Windows Mobile tři body jsou téměř colinear a oblouk je velmi malé.

Tečný oblouk je ideální pro vytváření zaoblenými hranami, jako je například zaoblený obdélník. Protože `SKPath` již obsahuje `AddRoundedRect` metoda, **zaokrouhlené pro sedmiúhelník** stránky demonstruje použití `ArcTo` pro zaokrouhlení rozích zachytávání sedm mnohoúhelníku. (Kód je zobecněn pro všechny regulární mnohoúhelníku.)

`PaintSurface` Obslužnou rutinu [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) třída obsahuje jednu `for` smyčky k výpočtu souřadnice sedm vrcholy pro sedmiúhelník a druhý k výpočtu střední sedm postranní z nich vrcholy. Tyto středních bodů jsou pak používány k vytváření cesta:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float cornerRadius = 100;
    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPoint[] vertices = new SKPoint[numVertices];
    SKPoint[] midPoints = new SKPoint[numVertices];

    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    // Coordinates of the midpoints of the sides connecting the vertices
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        int prevVertex = (vertex + numVertices - 1) % numVertices;
        midPoints[vertex] = new SKPoint((vertices[prevVertex].X + vertices[vertex].X) / 2,
                                        (vertices[prevVertex].Y + vertices[vertex].Y) / 2);
    }

    // Create the path
    using (SKPath path = new SKPath())
    {
        // Begin at the first midpoint
        path.MoveTo(midPoints[0]);

        for (int vertex = 0; vertex < numVertices; vertex++)
        {
            SKPoint nextMidPoint = midPoints[(vertex + 1) % numVertices];

            // Draws a line from the current point, and then the arc
            path.ArcTo(vertices[vertex], nextMidPoint, cornerRadius);

            // Connect the arc with the next midpoint
            path.LineTo(nextMidPoint);
        }
        path.Close();

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);
        }
    }
}

```

Tady je program běžící na třech platformách:

[![](arcs-images/roundedheptagon-small.png "Trojitá snímek obrazovky stránky pro zaokrouhlené sedmiúhelník")](arcs-images/roundedheptagon-large.png#lightbox "Trojitá snímek obrazovky stránky pro zaokrouhlené sedmiúhelník")

## <a name="the-elliptical-arc"></a>Eliptické oblouk

Eliptické oblouk se přidá do cesty s volání [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/SkiaSharp.SKPoint/) metoda, která má dva `SKPoint` parametry, nebo [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/System.Single/System.Single/) přetížení se samostatnou X a Y souřadnice:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

Je v souladu s eliptické oblouk [eliptické oblouk](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) součástí škálovatelné grafiky SVG (Vector) a univerzální platformu Windows [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) – třída.

Tyto `ArcTo` metody nakreslit oblouk mezi dvěma body, které je aktuální bod Kontury, a parametr poslední `ArcTo` – metoda ( `xy` parametr nebo samostatné `x` a `y` parametry):

![](arcs-images/ellipticalarcpoints.png "Dva body, které definované eliptické oblouk")

První parametr bodu `ArcTo` – metoda (`r`, nebo `rx` a `ry`) není bod vůbec, ale místo toho Určuje poloměr vodorovného a svislého elipsy;

![](arcs-images/ellipticalarcellipse.png "Elipsy, který definován eliptické oblouk")

`xAxisRotate` Parametr je po směru hodinových ručiček stupních otočení tento elipsy:

![](arcs-images/ellipticalarctiltedellipse.png "Nakloněné elipsy, který definován eliptické oblouk")

Pokud tato nakloněné elipsy je pak umístěna tak, aby dotýká se dva body, ve dvou různých oblouky připojeni body:

![](arcs-images/ellipticalarcellipse1.png "První sadu eliptické oblouky")

Tyto dvě oblouky dají rozlišovat dvěma způsoby: nejvyšší oblouk je větší než oblouk dolní a jako oblouk vykreslením zleva doprava, nejvyšší oblouk se vykresluje v směru hodinových ručiček, zatímco dolní oblouk se vykresluje v proti směru hodinových ručiček.

Je také možné použít se třemi tečkami mezi dvěma body jiným způsobem:

![](arcs-images/ellipticalarcellipse2.png "Druhá sada eliptické oblouky")

Nyní je menší oblouk na horní, který je vykreslen po směru hodinových ručiček a větší oblouk na dolní, který je vykreslen proti směru hodinových ručiček.

Tyto dva body můžete proto musí být připojená pomocí oblouk definovaný nakloněné elipsy v celkem čtyři způsoby:

![](arcs-images/ellipticalarccolors.png "Všechny čtyři eliptické oblouky")

Tyto čtyři oblouky jsou rozlišené čtyři kombinace [ `SKPathArcSize` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathArcSize/) a [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/) argumentů typu výčtu `ArcTo` metoda:

- Red: SKPathArcSize.Large a SKPathDirection.Clockwise
- zelená: SKPathArcSize.Small a SKPathDirection.Clockwise
- modré: SKPathArcSize.Small a SKPathDirection.CounterClockwise
- Purpurová: SKPathArcSize.Large a SKPathDirection.CounterClockwise

Pokud se třemi tečkami nakloněné není dostatečně velké na to, aby vyhovovaly mezi dvěma body je jednotná škálovat, dokud není dostatečně velké na to. Pouze dva jedinečné oblouky v takovém případě připojit dva body. Ty lze rozlišit pomocí `SKPathDirection` parametr.

I když tento přístup k definování oblouk vyznívá komplexní na první dojde, je jediným přístup, který umožňuje definovat oblouk s otočený elipsy a je často Nejsnadnějším když potřebujete integrovat s dalšími částmi Kontury oblouky.

**Eliptické oblouk** stránky umožňuje interaktivně nastavit dva body a velikosti a oběh se třemi tečkami. `EllipticalArcPage` Třída odvozená z `InteractivePage`a `PaintSurface` obslužné rutiny v [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) souboru kódu na pozadí nevykresluje čtyři oblouky:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        int colorIndex = 0;
        SKPoint ellipseSize = new SKPoint((float)xRadiusSlider.Value,
                                          (float)yRadiusSlider.Value);
        float rotation = (float)rotationSlider.Value;

        foreach (SKPathArcSize arcSize in Enum.GetValues(typeof(SKPathArcSize)))
            foreach (SKPathDirection direction in Enum.GetValues(typeof(SKPathDirection)))
            {
                path.MoveTo(touchPoints[0].Center);
                path.ArcTo(ellipseSize, rotation,
                           arcSize, direction,
                           touchPoints[1].Center);

                strokePaint.Color = colors[colorIndex++];
                canvas.DrawPath(path, strokePaint);
                path.Reset();
            }
    }

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}

```

Zde je spuštěn na tři platformy:

[![](arcs-images/ellipticalarc-small.png "Trojitá snímek obrazovky stránky eliptické oblouk")](arcs-images/ellipticalarc-large.png#lightbox "Trojitá snímek obrazovky stránky eliptické oblouk")

**Oblouk Infinity** stránka používá eliptické oblouk k vykreslení symbolem nekonečna. Infinity přihlášení je založena na dva kruhy s poloměr 100 jednotky oddělených 100 jednotky:

![](arcs-images/infinitycircles.png "Dva kruhy")

Dva řádky při překročení navzájem jsou tangens na obou kruhy:

![](arcs-images/infinitycircleslines.png "Dva kruhy tečný řádků")

Infinity přihlášení je kombinací těchto kružnice a dva řádky. Pokud chcete použít k vykreslení přihlašovací infinity eliptické oblouk, je třeba stanovit souřadnice, kde jsou dva řádky tangens k kroužky.

Vytvořte správné obdélníku v jednom z kroužky:

![](arcs-images/infinitytriangle.png "Dva kruhy s tečný čar a embedded kruhu.")

Radius kruhu je 100 jednotek a přepony trojúhelníku je 150 jednotek, úhel α je Arkus sinus (inverzní sinus) 100 rozdělit podle 150 nebo 41.8 stupňů. Délka druhé straně trojúhelníku je 150 časy kosinus 41.8 stupních nebo 112, které můžete také vypočítají jako Pythagorovy věty.

Souřadnice bodu tečný lze vypočítat potom pomocí těchto informací:

x = 112·cos(41.8) = 83

y = 112·sin(41.8) = 75

Čtyři tečný body jsou všechny, které jsou nezbytné k vykreslení symbolem infinity zarovnaný na střed v bodu (0, 0) se Kruh poloměr 100:

![](arcs-images/infinitycoordinates.png "Dva kruhy s tečný čar a souřadnice")

`PaintSurface` Obslužné rutiny v [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) třída umisťuje přihlašovací infinity tak, aby (0, 0) bod je umístěný v Centru pro stránky a škáluje cestu na velikost obrazovky:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.LineTo(83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.CounterClockwise, 83, -75);
        path.LineTo(-83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.Clockwise, -83, -75);
        path.Close();

        // Use path.TightBounds for coordinates without control points
        SKRect pathBounds = path.Bounds;

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / pathBounds.Width,
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

Kód používá `Bounds` vlastnost `SKPath` k určení dimenze sinus infinity škálování na velikost od plátna:

[![](arcs-images/arcinfinity-small.png "Trojitá snímek obrazovky stránky oblouk Infinity")](arcs-images/arcinfinity-large.png#lightbox "Trojitá snímek obrazovky stránky Infinity oblouk")

Výsledek vypadá trochu malé, což naznačuje, že `Bounds` vlastnost `SKPath` je generování sestav na velikost větší než cestu.

Interně Skia blíží oblouk pomocí více kvadratických Bézierových křivek. Tyto křivky (Jak uvidíte v další části) obsahovat kontrolní body, které řídí, jak se nevykreslí křivku, ale nejsou součástí vykreslené křivku. `Bounds` Vlastnost zahrnuje tyto kontrolní body.

Náročnější přizpůsobit, použijte `TightBounds` vlastnosti, které vylučují kontrolní body. Tady je program spuštěn v režimu na šířku a pomocí `TightBounds` vlastnost, která má získat cestu hranice:

[![](arcs-images/arcinfinitytightbounds-small.png "Trojitá snímek obrazovky stránky Infinity oblouk s úzkou hranice")](arcs-images/arcinfinitytightbounds-large.png#lightbox "Trojitá snímek obrazovky stránky Infinity oblouk s úzkou hranice")

I když jsou připojení mezi oblouky a přímky matematicky smooth, tato změna z oblouk přímku zdát trochu náhlému. Lepší infinity přihlášení se zobrazí na další stránce.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
