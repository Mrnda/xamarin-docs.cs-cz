---
title: Tři způsoby, jak nakreslit oblouk
description: Tento článek vysvětluje, jak ve Skiasharpu slouží k definování oblouky třemi různými způsoby a to demonstruje se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: charlespetzold
ms.author: chape
ms.date: 05/10/2017
ms.openlocfilehash: e862a663b35124c1470ae5239c93409c298b19ba
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615402"
---
# <a name="three-ways-to-draw-an-arc"></a>Tři způsoby, jak nakreslit oblouk

_Další informace o definování oblouky třemi různými způsoby použití ve Skiasharpu_

Oblouku je křivka na obvod elipsa, jako je například zakulacený částí tohoto symbolu nekonečna:

![](arcs-images/arcsample.png "Nekonečno přihlašování")

Bez ohledu na zjednodušení definice neexistuje žádný způsob, jak definovat oblouk vykreslování funkci, která splňuje každou potřebu, a proto žádná shoda mezi grafické systémy nejlepší způsob, jak nakreslit oblouk. Z tohoto důvodu `SKPath` třídy neomezuje samotné do právě jeden z přístupů.

`SKPath` definuje `AddArc` metody, pět různých `ArcTo` metody a dvě relativní `RArcTo` metody. Tyto metody spadají do tří kategorií, reprezentující tři velmi různé přístupy k určení oblouk. Ten, který jste použít závisí na informace k definování oblouk a jak tato oblouk zapadá grafiky, který kreslení k dispozici.

## <a name="the-angle-arc"></a>Úhel oblouku

Úhel oblouku přístup k vykreslení elipsy vyžaduje zadání obdélníku, který za rozsahem elipsu. Jedná o úhly v centru se třemi tečkami provádění začátek oblouk a jeho délka je označen oblouk na obvod tato tři tečky. Dva různé způsoby kreslení elipsy úhel. Jedná se o [ `AddArc` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) metoda a [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKRect/System.Single/System.Single/System.Boolean/) metody:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Tyto metody jsou stejné jako pro Android [ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/) a [ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/) metody. IOS [ `AddArc` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArc/p/System.Boolean/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) – metoda je podobné, ale je omezen na oblouky na obvod kruhu spíše než obecné, aby elipsu.

Obě metody začínat `SKRect` hodnotu, která definuje umístění a velikosti elipsy:

![](arcs-images/anglearcoval.png "Který začíná úhel oblouku elipsy")

Oblouku je součástí obvod tato tři tečky.

`startAngle` Argument je po směru hodinových ručiček úhel ve stupních vzhledem k vodorovnou horizontální čáru vykreslen v centru se třemi tečkami na pravé straně. `sweepAngle` Argument je vzhledem k `startAngle`. Tady jsou `startAngle` a `sweepAngle` hodnoty 60 a 100 stupňů, v uvedeném pořadí:

![](arcs-images/anglearcangles.png "Jedná o úhly, které definují úhel oblouku")

Oblouku začíná počáteční úhel. Úhel oblouku řídí jeho délka:

![](arcs-images/anglearchighlight.png "Zvýrazněný úhel oblouku")

Křivky přidány do cesty `AddArc` nebo `ArcTo` metoda je jednoduše, že část obvod se třemi tečkami, zobrazené červeně:

![](arcs-images/anglearc.png "Úhel oblouku sám o sobě")

`startAngle` Nebo `sweepAngle` argumenty mohou být záporná: oblouk je po směru hodinových ručiček pro kladné hodnoty `sweepAngle` a proti směru hodinových ručiček pro záporné hodnoty.

Ale `AddArc` nemá *není* definovat uzavřené obrysem. Při volání `LineTo` po `AddArc`, linie vychází z konce oblouku do bodu v `LineTo` metoda a stejné platí pro `ArcTo`.

`AddArc` automaticky spustí nová rozvrh a je funkčně ekvivalentní volání `ArcTo` s konečný argument `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Se nazývá posledním argumentem `forceMoveTo`, a efektivně způsobí, že `MoveTo` volání na začátku oblouk. Který začíná nové obrysem. To znamená ne v případě poslední argument metody `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Tato verze `ArcTo` nakreslí čáru od aktuální pozice na začátku oblouk. To znamená, že oblouku může být někde uprostřed větší obrysem.

**Úhel oblouku** stránky umožňuje používat dvě posuvníky a určuje počáteční úhel oblouku. Soubor XAML vytvoří dvě `Slider` elementy a `SKCanvasView`. `PaintCanvas` Obslužné rutiny v [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) souboru kreslení plné elipsy a oblouk pomocí dvou `SKPaint` objekty definované jako pole:

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

Jak je vidět, můžete provést počáteční úhel a úhel oblouku na záporné hodnoty:

[![](arcs-images/anglearc-small.png "Trojitá snímek obrazovky stránky úhel oblouku")](arcs-images/anglearc-large.png#lightbox "Trojitá snímek obrazovky stránky úhel oblouku")

Tento přístup k generování oblouku je algorithmically nejjednodušší a usnadňují odvození parametrické rovnice, které popisují oblouk. Znalost, velikost a umístění na tři tečky a úhlů počáteční a vyčištění, počáteční a koncový bod oblouku můžete vypočítat pomocí jednoduchého trigonometrické:

x = oval. MidX + (oval. Šířka / 2) * cos(angle)

y = oval. MidY + (oval. Výška / 2) * sin(angle)

`angle` Hodnota je buď `startAngle` nebo `startAngle + sweepAngle`.

Použití dvou úhlů k definování oblouku je nejvhodnější pro případy, kdy víte angular délka arc, který chcete nakreslit, třeba, aby výsečový graf. **Rozložený výsečový graf** stránce ukazuje to. [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) Třída používá interní třída pro definování některé kovodělných dat a barvy:

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

`PaintSurface` Obslužná rutina nejprve prochází položky, které chcete vypočítat `totalValues` číslo. Z té může určit velikost každé položky jako část celku a převést, kterým je úhel:

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

Nový `SKPath` objekt je vytvořen pro jednotlivé výseče. Cesta se skládá z řádku z centra, pak `ArcTo` Chcete-li nakreslit oblouk a další řádek zpět do center výsledků `Close` volání. Tento program zobrazí "rozložený" výsečí jejich přesunutím ven z centra o 50 pixelů. Tato úloha vyžaduje vektor ve směru středního úhel oblouku pro každý řez:

[![](arcs-images/explodedpiechart-small.png "Trojitá snímek obrazovky stránky rozložený výsečový graf")](arcs-images/explodedpiechart-large.png#lightbox "Trojitá snímek obrazovky stránky rozložený výsečový graf")

Chcete-li zjistit, jak to funguje bez "výbuchu", jednoduše komentář `Translate` volání:

[![](arcs-images/explodedpiechartunexploded-small.png "Trojitá snímek obrazovky stránky rozložený výsečový graf bez prudkého")](arcs-images/explodedpiechartunexploded-large.png#lightbox "Trojitá snímek obrazovky stránky rozložený výsečový graf bez rozbalení")

## <a name="the-tangent-arc"></a>Arkus tangens

Druhý typ oblouk nepodporuje `SKPath` je *Arkus tangens*, takže volat, protože je obvod kruhu, která je tangens na dva spojené čáry oblouk.

Tečný oblouk se přidá do cesty s volání [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) metodu se dvěma `SKPoint` parametry, nebo [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) přetížení samostatné `Single` parametry body:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

To `ArcTo` metoda je podobná PostScript [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) – funkce (stránka 532 v dokumentu PDF) a iOS [ `AddArcToPoint` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArcToPoint/p/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) metody.

`ArcTo` Metoda zahrnuje tři body:

- Aktuální příkaz obrysu nebo bod (0, 0) Pokud `MoveTo` nevolala
- První argument bodu `ArcTo` metodu s názvem *rohu bodu*
- Druhý argument bodu `ArcTo`, označované jako *cílový bod*:

![](arcs-images/tangentarcthreepoints.png "Tři body, které začínají na tečný oblouk")

Tyto tři body definují dva spojené čáry:

![](arcs-images/tangentarcconnectinglines.png "Čáry spojující tří bodů tangenty oblouk")

Pokud jsou tři body colinear &mdash; to znamená, pokud jsou na stejné přímce &mdash; bude vykreslen žádný oblouk.

`ArcTo` Metoda zahrnuje také `radius` parametru. Definuje poloměr kruhu:

![](arcs-images/tangentarccircle.png "Kruh tečný oblouk")

Arkus tangens se nezobecňuje pro elipsu.

Pokud splňují dva řádky na každý úhel, můžete tento kruh vložen mezi tyto řádky tak, aby se tangens pro obě čáry:

![](arcs-images/tangentarctangentcircle.png "Arkus tangens kruh mezi dvěma řádky")

Křivku, která se přidá do obrysu buď bodů určených v nemění `ArcTo` metody. Skládá se z první tečný bodu a oblouk končí druhý bod tečný rovnou linii od aktuálního místa:

![](arcs-images/tangentarchighlight.png "Zvýrazněný tečný oblouk mezi dvěma řádky")

Zde je poslední rovné čáry a arc, který je přidán do obrysu:

![](arcs-images/tangentarc.png "Zvýrazněný tečný oblouk mezi dvěma řádky")

Obrysu můžete pokračovat z druhého tečný bodu.

**Arkus tangens** stránce můžete experimentovat s Arkus tangens. Toto je první několik stránek, které jsou odvozeny z [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs), která definuje několik po ruce `SKPaint` objekty a provádí `TouchPoint` zpracování:

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

`TangentArcPage` Třída odvozena z `InteractivePage`. Konstruktor v [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) souboru je zodpovědný za vytváření instancí a inicializace `touchPoints` pole a nastavení `baseCanvasView` (v `InteractivePage`) k `SKCanvasView` v vytvořena instance objektu [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) souboru:

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

`PaintSurface` Obslužná rutina používá `ArcTo` metody, chcete-li nakreslit oblouk podle dotykovými body a `Slider`, ale také algorithmically vypočítá kruh, který je na základě úhel:

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

Tady je **Arkus tangens** stránky, které běží na všech třech platformách:

[![](arcs-images/tangentarc-small.png "Trojitá snímek obrazovky stránky Arkus tangens")](arcs-images/tangentarc-large.png#lightbox "Trojitá snímek obrazovky stránky Arkus tangens")

Arkus tangens je ideální pro vytvoření oblých rohů, jako je například zakulacený obdélník. Protože `SKPath` již obsahuje `AddRoundedRect` metody, **zaokrouhlí pro sedmiúhelník** stránce ukazuje, jak používat `ArcTo` pro zaoblení rohů mnohoúhelníku oboustranný sedm. (Kód je zobecněný pro jakékoli pravidelné mnohoúhelník.)

`PaintSurface` Obslužná rutina [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) třída obsahuje jeden `for` smyčky k výpočtu souřadnice sedm vrcholy pro sedmiúhelník a druhý k výpočtu středových bodů sedm stran z těchto vrcholy. Tyto středových bodů se následně použijí k vytvoření cesta:

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

[![](arcs-images/roundedheptagon-small.png "Trojitá snímek obrazovky stránky pro zaokrouhlí sedmiúhelník")](arcs-images/roundedheptagon-large.png#lightbox "Trojitá snímek obrazovky stránky pro sedmiúhelník zaokrouhleno")

## <a name="the-elliptical-arc"></a>Oblouku elipsy

Oblouku elipsy se přidá do cesty s volání [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/SkiaSharp.SKPoint/) metodu, která má dvě `SKPoint` parametry, nebo [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/System.Single/System.Single/) přetížení s samostatné X a Y souřadnice:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

Je v souladu s oblouku elipsy [oblouku elipsy](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) zahrnout grafiky SVG (Scalable Vector) a univerzální platformu Windows [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) třídy.

Tyto `ArcTo` metody nakreslit oblouk mezi dvěma body, které je do aktuálního místa obrysu, a poslední parametr `ArcTo` – metoda ( `xy` parametr nebo jako samostatná `x` a `y` parametry):

![](arcs-images/ellipticalarcpoints.png "Dva body, definované oblouku elipsy")

První parametr bodu `ArcTo` – metoda (`r`, nebo `rx` a `ry`) není bod vůbec, ale místo toho Určuje vodorovný a svislý poloměr elipsy;

![](arcs-images/ellipticalarcellipse.png "Elipsy, který definován oblouku elipsy")

`xAxisRotate` Parametr je počet stupňů po směru hodinových ručiček otočit tento elipsa:

![](arcs-images/ellipticalarctiltedellipse.png "Nakloněné elipsy, který definován oblouku elipsy")

Pokud tento nakloněné elipsa je pak umístěný tak, aby se dotkne dva body, body propojené pomocí dvou různých oblouky:

![](arcs-images/ellipticalarcellipse1.png "První sada eliptické oblouky")

Tyto dvě oblouky dají rozlišovat dvěma způsoby: nejvyšší oblouku je větší než dolní oblouk a jako oblouku vykreslením zleva doprava, horní oblouk vykreslením ve směru hodinových ručiček, zatímco dolní oblouku je vykreslen v proti směru hodinových ručiček.

Je také možné přizpůsobit se třemi tečkami mezi dvěma body jiným způsobem:

![](arcs-images/ellipticalarcellipse2.png "Druhá sada eliptické oblouky")

Nyní je menší oblouk v horní části, která je po směru hodinových ručiček a větší oblouk v dolní části, který je vykreslen proti směru hodinových ručiček.

Tyto dva body proto dá připojit pomocí oblouk definovaný nakloněné elipsa celkem čtyři způsoby:

![](arcs-images/ellipticalarccolors.png "Všechny čtyři eliptické oblouky")

Tyto čtyři oblouky se rozlišují výhradně podle čtyři kombinací [ `SKPathArcSize` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathArcSize/) a [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/) argumentů typu výčtu `ArcTo` metody:

- červené: SKPathArcSize.Large a SKPathDirection.Clockwise
- zelená: SKPathArcSize.Small a SKPathDirection.Clockwise
- modré: SKPathArcSize.Small a SKPathDirection.CounterClockwise
- Purpurová: SKPathArcSize.Large a SKPathDirection.CounterClockwise

Pokud se třemi tečkami nakloněné není dostatečně velký, aby mezi dvěma body je jednotně škálovat, dokud není dostatečně velký. Jenom dva jedinečné oblouky v takovém případě připojit dva body. Ty lze rozlišit pomocí `SKPathDirection` parametru.

Přestože tento přístup k definování oblouk zvuky komplexní na první dojde, je ale jediný možný přístup, který umožňuje definovat oblouk s otočený tři tečky a často je nejjednodušším přístupem při potřebný k integraci s jinými částmi obrysu elipsy.

**Oblouku elipsy** stránky umožňuje interaktivně nastavit dva body a velikosti a oběh elipsy. `EllipticalArcPage` Třída odvozena z `InteractivePage`a `PaintSurface` obslužné rutiny v [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) čtyři oblouky nakreslí soubor kódu na pozadí:

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

Tady je spuštěn na třech platformách:

[![](arcs-images/ellipticalarc-small.png "Trojitá snímek obrazovky stránky oblouku elipsy")](arcs-images/ellipticalarc-large.png#lightbox "Trojitá snímek obrazovky stránky oblouku elipsy")

**Oblouk nekonečno** stránka používá oblouku elipsy, chcete-li nakreslit znaménko nekonečno. Znaménko nekonečno je založena na dva kruhy s poloměry 100 jednotek oddělené 100 jednotek:

![](arcs-images/infinitycircles.png "Dva kruhy")

Přecházení mezi sebou mezi dvěma řádky se tangens na obou kruhy:

![](arcs-images/infinitycircleslines.png "Dva kruhy s tečny")

Podpis nekonečno je kombinace součástí těchto kruzích a dva řádky. Na kreslení nekonečno přihlašování pomocí oblouku elipsy, musí být určena souřadnice, kde dva řádky jsou arkustangens kruzích.

Sestavit správný obdélník v jednom z kruhů:

![](arcs-images/infinitytriangle.png "Dva kruhy s tečny a vložené kruh")

Poloměr kruhu je 100 jednotek a přepony část trojúhelníku je 150 jednotky, a proto úhel α Arkus sinus (inverzní sinus) 100 rozdělit podle 150 nebo 41.8 stupňů. Délka druhé straně část trojúhelníku je 150 časy kosinus 41.8 stupňů nebo 112, který se dá vypočítat pomocí Pythagorovy věty.

Souřadnice bodů tangenty lze vypočítat pak pomocí těchto informací:

x = 112·cos(41.8) = 83

y = 112·sin(41.8) = 75

Čtyř bodů tangenty jsou vše, co je potřeba kreslení pomocí poloměr kruhu 100 symbol nekonečna na střed v bodě (0, 0):

![](arcs-images/infinitycoordinates.png "Dva kruhy s tečný čáry a souřadnice")

`PaintSurface` Obslužné rutiny v [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) třídy umístí nekonečno přihlašování tak, aby (0, 0) bod je umístěn ve středu stránky a škáluje ji na velikost obrazovky:

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

Tento kód použije `Bounds` vlastnost `SKPath` určit dimenze sinus nekonečno škálovat na velikost plátna:

[![](arcs-images/arcinfinity-small.png "Trojitá snímek obrazovky stránky oblouk nekonečno")](arcs-images/arcinfinity-large.png#lightbox "Trojitá snímek obrazovky stránky nekonečno oblouk")

Výsledek vypadá trochu malé, což naznačuje, že `Bounds` vlastnost `SKPath` je větší než cestu pro generování sestav.

Interně Skia blíží oblouk pomocí více kvadratické Bézierovy křivky. Tyto křivky (Jak uvidíte v další části) obsahuje kontrolní body, které určují, jak je vykreslen křivku, ale nejsou součástí vykreslené křivky. `Bounds` Vlastnost zahrnuje tyto kontrolní body.

Pokud chcete získat přesnější vhodný, použijte `TightBounds` vlastnost, která nezahrnuje kontrolní body. Tady je program spuštěn v režimu na šířku a použití `TightBounds` vlastnost k získání cesty hranice:

[![](arcs-images/arcinfinitytightbounds-small.png "Trojitá snímek obrazovky stránky nekonečno oblouk s úzkou hranice")](arcs-images/arcinfinitytightbounds-large.png#lightbox "Trojitá snímek obrazovky stránky nekonečno oblouk s absolutní hranice")

I když jsou připojení mezi oblouky a rovné čáry matematicky smooth, změna oblouk rovné čáry zdát trochu náhlé. Na další stránce se zobrazí znaménko lepší nekonečno.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
