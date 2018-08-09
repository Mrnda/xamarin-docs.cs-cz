---
title: Základní informace cesty ve Skiasharpu
description: Tento článek zkoumá objektu ve Skiasharpu SKPath kombinování připojené čar a křivek a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 3c07614c12fb503638d3d5e63b24eb5367ba691a
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615532"
---
# <a name="path-basics-in-skiasharp"></a>Základní informace cesty ve Skiasharpu

_Prozkoumejte objektu ve Skiasharpu SKPath kombinování připojené čar a křivek_

Jeden z vašich nejdůležitějších soukromí uživatelů cesty grafiky je schopnost definovat, kdy má být připojen více řádků a kdy by neměl být připojen. Rozdíl může být poměrně viditelné, jak ukazují lineárních tyto dvě trojúhelníky:

![](paths-images/connectedlinesexample.png "Dvě trojúhelníky zobrazuje rozdíl mezi řádky připojené a odpojené")

Cesty grafiky jsou zapouzdřena objektem [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) objektu. Cesta je kolekce jednoho nebo více *rozvrhy*. Každý rozvrh je kolekce *připojené* přímých čar a křivek. Obrysy nejsou připojené k sobě navzájem, ale vizuálně mohou překrývat. Někdy může jeden rozvrh překrývat sama sebe.

Obrysem obvykle začíná volání následující metodu `SKPath`:

- `MoveTo` Chcete-li začít novou obrysu

Argument k této metodě je jedno místo, které můžete vyjádřit jako `SKPoint` hodnotu nebo jako samostatné X a Y souřadnic. `MoveTo` Volání vytvoří obrysu a počáteční bod na začátek *aktuálního místa*. Můžete volat následující metody, které dál obrysu řádku nebo křivky z aktuálního místa k umístění zadané v metodě, která se pak stane aktuální nový bod:

- `LineTo` Chcete-li přidat do cesty rovné čáry
- `ArcTo` Chcete-li přidat oblouk, což je řádek na obvod kruhu nebo elipsa
- `CubicTo` Chcete-li přidat kubické Bézierovy křivky
- `QuadTo` Chcete-li přidat kvadratické Bézierovy křivky
- `ConicTo` Chcete-li přidat racionální kvadratické Bézierovy křivky, který může přesně vykreslit conic části (symbol tří teček, paraboly a hyperboly)

Žádná z těchto pět metod neobsahuje všechny informace potřebné k popisu křivky nebo řádku. Každá z těchto pět metod funguje ve spojení s aktuálním bodem stanovené bezprostředně před volání metody. Například `LineTo` metoda přidá čárou se obrysu na základě aktuálního místa, tak parametr `LineTo` je pouze jediný bod.

`SKPath` Třída rovněž definuje metody, které mají stejné názvy jako těchto šest metod, ale `R` na začátku:

- `RMoveTo`
- `RLineTo`
- `RArcTo`
- `RCubicTo`
- `RQuadTo`
- `RConicTo`

`R` Zastupuje *relativní*. Mají stejnou syntaxi jako odpovídající metody bez `R` , ale jsou relativní vzhledem k aktuálnímu bodu. Toto jsou užitečné pro kreslení podobné části cesty v metodě, kterou je možné volat více než jednou.

Rozvrh skončí s jiným voláním metody `MoveTo` nebo `RMoveTo`, který začíná nové rozvrh nebo volání `Close`, která ukončí obrysu. `Close` Metoda automaticky připojí rovnou linii od aktuálního místa na první bod obrysu a označuje cestu, např. Uzavřeno, což znamená, že bude vykreslen bez jakékoli zakončení tahů.

Rozdíl mezi otevřené a uzavřené rozvrhy je znázorněn v **dvě rozvrhy trojúhelník** stránky, který používá `SKPath` objekt s dvěma rozvrhy k vykreslení dvou trojúhelníky. Je otevřený obrysu první a druhý je uzavřen. Tady je [ `TwoTriangleContours` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs) třídy:

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

První rozvrh se skládá z volání [ `MoveTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.MoveTo/p/System.Single/System.Single/) pomocí souřadnice X a Y spíše než výjimku `SKPoint` hodnotu, za nímž následuje tří volání [ `LineTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.LineTo/p/System.Single/System.Single/) nakreslete tři strany trojúhelník. Obrysu druhý má jenom dvě volání `LineTo` ale dokončí obrysu voláním [ `Close` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Close()/), která ukončí obrysu. Je důležité rozdíly:

[![](paths-images/twotrianglecontours-small.png "Trojitá snímek obrazovky stránky dvou rozvrhy trojúhelník")](paths-images/twotrianglecontours-large.png#lightbox "Trojitá snímek obrazovky stránky dvou rozvrhy trojúhelník")

Jak je vidět, obrysu první je samozřejmě posloupnost tří spojené čáry, ale není konec připojit s počátkem. Následující dva řádky se překrývají v horní části. Druhý rozvrh je samozřejmě uzavřen a bylo dosaženo s jedním méně `LineTo` volání, protože `Close` metoda automaticky přidá poslední řádek zavřete obrysu.

`SKCanvas` Definuje jen jedna [ `DrawPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawPath/p/SkiaSharp.SKPath/SkiaSharp.SKPaint/) metodu, která v této ukázce je volána dvakrát a vyplnit obtažení cesty. Všechny rozvrhy jsou vyplněny, včetně těch, které nebyly uzavřeny. Pro účely vyplnění neuzavřený cesty rovné čáry se předpokládá, že existují mezi počáteční a koncový bod obrysu. Pokud odeberete poslední `LineTo` z první rozvrh nebo odebrat `Close` volání z druhé rozvrh, každý rozvrh budou mít jenom dvě strany se vyplněné, jako by šlo trojúhelník.

`SKPath` definuje mnoho dalších metod a vlastností. Následující metody přidat celou rozvrhy do cesty, která může být potřeba zavřít nebo podle metody není ukončen:

- `AddRect`
- [`AddRoundedRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddRoundedRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddCircle`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddCircle/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddOval`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddOval/p/SkiaSharp.SKRect/SkiaSharp.SKPathDirection/)
- [`AddArc`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) Chcete-li přidat křivku na obvod elipsa
- `AddPath` přidat jinou cestu pro aktuální cestu
- [`AddPathReverse`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddPathReverse/p/SkiaSharp.SKPath/) Chcete-li přidat jinou cestu v opačném pořadí

Mějte na paměti, která `SKPath` objekt definuje pouze geometrie &mdash; sérii bodů a připojení. Pouze tehdy, když `SKPath` číslo zkombinuje s `SKPaint` objekt je vykreslen pomocí konkrétní barvu, šířku tahu a tak dále cesta. Také, mějte na paměti, která `SKPaint` předán objekt `DrawPath` metoda definuje vlastnosti celou cestu. Pokud chcete nakreslete něco vyžadující několik barev, musíte použít samostatné cesty pro každou barvu.

Stejně jako vzhled začátku a konci řádku je definován zakončení tahu, vzhled připojení mezi dvěma řádky je definován *stroke spojení*. Toto určíte tak, že nastavíte [ `StrokeJoin` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeJoin/) vlastnost `SKPaint` členovi [ `SKStrokeJoin` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeJoin/) výčtu:

- [`Miter`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Miter/) pointy spojení
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Round/) Zaoblený spojení
- [`Bevel`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Bevel/) Rozdělit vypnout spojení

**Stroke spojení** stránce zobrazí tyto tři obtažení spojení s kódem, podobně jako **zakončení tahů** stránky. Toto je `PaintSurface` obslužné rutině událostí ve [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs) třídy:

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

[![](paths-images/strokejoins-small.png "Trojitá snímek obrazovky stránky Stroke připojí")](paths-images/strokejoins-large.png#lightbox "Trojitá snímek obrazovky stránky připojí tahu")

Pokosové spojení se skládá z sharp bodu, kde připojit řádky. Když se připojí k dva řádky v malých úhlu, může být poměrně dlouhé pokosové spojení. Aby nadměrně dlouhá pokosové spojení, je omezena délka pokosové spojení hodnotu [ `StrokeMiter` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeMiter/) vlastnost `SKPaint`. Pokosové spojení, které překročí tento délka je rozdělit vypnout stane spojení zkosení.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
