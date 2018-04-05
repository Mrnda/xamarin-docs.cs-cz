---
title: Základy cesta
description: Prozkoumat objekt SkiaSharp SKPath kombinování připojené čar a křivek
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: b2881148631435c9082b42cad0e784100b010b46
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/05/2018
---
# <a name="path-basics"></a>Základy cesta

_Prozkoumat objekt SkiaSharp SKPath kombinování připojené čar a křivek_

Jedním z nejdůležitějších funkcí cesty grafiky je možnost určit, kdy má být připojen více řádků a pokud by neměl být připojen. Rozdíl může být poměrně viditelné jako horní části tyto dvě trojúhelníčky ukázka:

![](paths-images/connectedlinesexample.png "Dva trojúhelníčky zobrazuje rozdíl mezi připojené a odpojené řádky")

Cesty grafiky je zapouzdřené pomocí [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) objektu. Cestu je kolekce jednoho či více *rozvrhy*. Každý obrysem je kolekce *připojené* přímých čar a křivek. Rozvrhy nejsou připojené k sobě navzájem, ale může vizuálně překrývat. Někdy může jeden obrysem překrývat sám sebe.

Obrysem obvykle začíná volání metodě následující `SKPath`:

- `MoveTo` Chcete-li začít nový obrysem

Argument této metody je jediný bod, který můžete buď jako express `SKPoint` hodnotu nebo jako samostatnou X a Y souřadnice. `MoveTo` Hovor zakládá Kontury a počáteční bod na začátku *aktuálnímu bodu*. Můžete volat pokračovat obrysem řádku nebo z aktuální bodu křivky mohutnosti na bod uvedený v metodu, která se pak stane aktuální nový bod následující metody:

- `LineTo` Chcete-li přidat přímku do cesty
- `ArcTo` Chcete-li přidat oblouk, což je řádek na obvodu kruh nebo třemi tečkami
- `CubicTo` Chcete-li přidat krychlový Bézierovy křivky
- `QuadTo` Chcete-li přidat kvadratické Bézierovy křivky
- `ConicTo` Chcete-li přidat rozumné kvadratické Bézierovy křivky, který může vykreslit přesně conic částech (tři tečky, paraboly a hyperboly)

Žádná z těchto pět metod neobsahuje všechny informace potřebné k popisu řádku nebo křivky. Každá z těchto pět metod funguje ve spojení s bodem aktuální vymezenému se bezprostředně před volání metody. Například `LineTo` metoda přidá přímku k obrysu založené na aktuální bodu, takže parametru `LineTo` je pouze jediný bod.

`SKPath` Třída také definuje metody, které mají stejné názvy jako těchto šest způsobů, ale `R` na začátku:

- `RMoveTo`
- `RLineTo`
- `RArcTo`
- `RCubicTo`
- `RQuadTo`
- `RConicTo`

`R` Znamená *relativní*. Mají stejnou syntaxi jako odpovídající metody bez `R` , ale jsou relativní vzhledem k aktuálnímu bodu. Toto jsou užitečné pro kreslení podobné části cesty v metoda, kterou je možné volat vícekrát.

Obrysem končí jiným voláním `MoveTo` nebo `RMoveTo`, začínající nové obrysem nebo volání `Close`, který zavře Kontury. `Close` Metoda automaticky připojí přímky z aktuální bodu první bodem Kontury a označí cestu jako zavřené, což znamená, že bude vykreslen bez jakékoli tahu CAP k vzdálené ploše.

Rozdíl mezi otevřené a uzavřené rozvrhů je znázorněn na následujícím **dva profily okrajů trojúhelníček** stránky, které používá `SKPath` objekt s dva profily okrajů k vykreslení dvě trojúhelníčky. První obrysem je otevřený a druhý je uzavřený. Tady je [ `TwoTriangleContours` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs) třídy:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create the path
    SKPath path = new SKPath();

    // Define the first contour
    path.MoveTo(0.5f * info.Width, 0.1f * info.Height);
    path.LineTo(0.2f * info.Width, 0.4f * info.Height);
    path.LineTo(0.8f * info.Width, 0.4f * info.Height);
    path.LineTo(0.5f * info.Width, 0.1f * info.Height);

    // Define the second contour
    path.MoveTo(0.5f * info.Width, 0.6f * info.Height);
    path.LineTo(0.2f * info.Width, 0.9f * info.Height);
    path.LineTo(0.8f * info.Width, 0.9f * info.Height);
    path.Close();

    // Create two SKPaint objects
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Magenta,
        StrokeWidth = 50
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    // Fill and stroke the path
    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

První obrysem se skládá z volání [ `MoveTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.MoveTo/p/System.Single/System.Single/) pomocí souřadnice X a Y místo `SKPoint` hodnotu následovanou tří volání [ `LineTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.LineTo/p/System.Single/System.Single/) k vykreslení tři stranách trojúhelníku. Druhý obrysem má jenom dvě volání `LineTo` ale dokončením obrysem pomocí volání [ `Close` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Close()/), který zavře Kontury. Rozdíl je důležité:

[![](paths-images/twotrianglecontours-small.png "Trojitá snímek obrazovky stránky dva profily okrajů trojúhelníček")](paths-images/twotrianglecontours-large.png#lightbox "Trojitá snímek obrazovky stránky dva profily trojúhelníček okrajů")

Jak můžete vidět, první obrysem je samozřejmě řadu tři řádky připojené, ale end není připojení k začátku. Dva řádky překrývat v horní části. Druhý obrysem je samozřejmě uzavřený a dosáhlo s jedním méně `LineTo` volá, protože `Close` metoda automaticky přidá poslední řádek zavřete Kontury.

`SKCanvas` Definuje jen jedna [ `DrawPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawPath/p/SkiaSharp.SKPath/SkiaSharp.SKPaint/) metodu, která v této ukázce je volána dvakrát k vyplnění a obtažení cesty. Všechny profily okrajů jsou vyplněny, i ty, které nebudou ukončeny. Pro účely naplnění uzavřené cesty se předpokládá přímku existovat mezi počáteční a koncové body obrysu. Když odeberete poslední `LineTo` z první obrysem nebo odebrat `Close` volání z druhé obrysem každý obrysem budou mít pouze dvě strany se naplní, jako by šlo trojúhelník.

`SKPath` definuje mnoho dalších metod a vlastností. Následující metody přidejte celý rozvrhy na cestu, která může být uzavřen nebo neuzavřené podle metody:

- `AddRect`
- [`AddRoundedRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddRoundedRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddCircle`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddCircle/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddOval`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddOval/p/SkiaSharp.SKRect/SkiaSharp.SKPathDirection/)
- [`AddArc`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) Chcete-li přidat křivka na obvodu elipsy
- `AddPath` Chcete-li přidat jiné cesty pro aktuální cestu
- [`AddPathReverse`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddPathReverse/p/SkiaSharp.SKPath/) Chcete-li přidat jiné cesty v zpětného

Mějte na paměti, že `SKPath` objekt definuje pouze geometrie &mdash; řadu body a připojení. Pouze tehdy, když `SKPath` je kombinováno se `SKPaint` objektu je cesta vykreslují konkrétní barvu, šířku tahu a tak dále. Navíc mějte na paměti, `SKPaint` byl předán objekt `DrawPath` metoda definuje vlastnosti celou cestu. Pokud chcete k vykreslení něco nutnosti několik barvy, musíte použít samostatné cestu pro jednotlivé barvy.

Stejně jako vzhled začátku a konci řádku je definované zakončení tahu, je definována vzhled připojení mezi dvěma čárami *tahu spojení*. Toto určíte nastavením [ `StrokeJoin` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeJoin/) vlastnost `SKPaint` k členem [ `SKStrokeJoin` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeJoin/) výčtu:

- [`Miter`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Miter/) pro pointy spojení
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Round/) pro zaokrouhlené spojení
- [`Bevel`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Bevel/) pro připojení k chopped vypnout

**Tahu spojení** stránka zobrazuje tyto tři obtažení spojení s kódem podobně jako **tahu CAP k vzdálené ploše** stránky. Toto je `PaintSurface` obslužné rutiny událostí v [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs) třídy:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Right
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width - 100;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = 2 * textPaint.FontSpacing;
    string[] strStrokeJoins = { "Miter", "Round", "Bevel" };

    foreach (string strStrokeJoin in strStrokeJoins)
    {
        // Display text
        canvas.DrawText(strStrokeJoin, xText, y, textPaint);

        // Get stroke-join value
        SKStrokeJoin strokeJoin;
        Enum.TryParse(strStrokeJoin, out strokeJoin);

        // Create path
        SKPath path = new SKPath();
        path.MoveTo(xLine1, y - 80);
        path.LineTo(xLine1, y + 80);
        path.LineTo(xLine2, y + 80);

        // Display thick line
        thickLinePaint.StrokeJoin = strokeJoin;
        canvas.DrawPath(path, thickLinePaint);

        // Display thin line
        canvas.DrawPath(path, thinLinePaint);
        y += 3 * textPaint.FontSpacing;
    }
}
```

Tady je program běžící na třech platformách:

[![](paths-images/strokejoins-small.png "Trojitá snímek obrazovky stránky připojí tahu")](paths-images/strokejoins-large.png#lightbox "Trojitá snímek obrazovky stránky tahu spojení")

Pokosové spojení se skládá z sharp bodu, kde řádky připojit. Když dva řádky v malých úhlu, se může stát pokosové spojení poměrně dlouho. Aby nadměrně dlouhá pokosové spojení, je omezena délka pokosové spojení hodnotu [ `StrokeMiter` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeMiter/) vlastnost `SKPaint`. Pokosové spojení delší, než tato délka je chopped vypnout stane zkosení spojení.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
