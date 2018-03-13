---
title: "Typy výplně cesta"
description: "Zjištění různých důsledky možné s SkiaSharp cesta výplně typy"
ms.topic: article
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: cee93f082386e78a643b8c07dd48d0196e8ecd6c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="the-path-fill-types"></a>Typy výplně cesta

_Zjištění různých důsledky možné s SkiaSharp cesta výplně typy_

Dva profily okrajů v cestě může dojít k překrytí a řádky, které tvoří jeden obrysem může dojít k překrytí. Všechny uzavřené oblasti potenciálně některá, ale možná nebudete chtít vyplnit závorkách oblasti. Tady je příklad:

![](fill-types-images/filltypeexample.png "Odkazoval pěti hvězdičkami částečně filles")

Máte ještě kontrolu nad to. Se řídí algoritmus naplnění [ `SKFillType` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.FillType/) vlastnost `SKPath`, který je členem skupiny nastavení [ `SKPathFillType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathFillType/) – výčet:

- [`Winding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.Winding/), výchozí
- [`EvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.EvenOdd/)
- [`InverseWinding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseWinding/)
- [`InverseEvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseEvenOdd/)

Algoritmy vinutí a lichý určete, jestli všechny uzavřené oblasti je vyplněna nebo není doplní podle hypotetický linií z této oblasti do nekonečna. Daného řádku protne jeden nebo více řádky hranic, které tvoří cestu. Vinutí režim Pokud počet řádků hranic vykresluje v jednom směru vyrovnávání snížit počet řádků, které jsou vykreslovány v opačným směrem a potom v oblasti, které není vyplněna. V opačném případě se naplní oblasti. Algoritmus lichý doplní oblast, pokud je počet řádků hranic liché.

S mnoha běžných cest algoritmus vinutí často doplní všechny závorkách oblasti cesty. Algoritmus lichý obecně poskytuje zajímavějšího výsledky.

Classic příkladem je hvězdu pět ukazuje, jak je předvedeno v **Five-Pointed hvězdičkami** stránky. [FivePointedStarPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/FivePointedStarPage.xaml) soubor vytvoří dvě instance `Picker` zobrazení vyberte cestu, zadejte typ a zda je cesta vytažený nebo vyplněna nebo obojí a v jakém pořadí:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.FivePointedStarPage"
             Title="Five-Pointed Star">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="fillTypePicker"
                Title="Path Fill Type"
                Grid.Row="0"
                Grid.Column="0"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Winding</x:String>
                <x:String>EvenOdd</x:String>
                <x:String>InverseWinding</x:String>
                <x:String>InverseEvenOdd</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="drawingModePicker"
                Title="Drawing Mode"
                Grid.Row="0"
                Grid.Column="1"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Fill only</x:String>
                <x:String>Stroke only</x:String>
                <x:String>Stroke then Fill</x:String>
                <x:String>Fill then Stroke</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Soubor modelu code-behind využívá jak `Picker` hodnoty k vykreslení hvězdu na kterou pět:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPath path = new SKPath
    {
        FillType = (SKPathFillType)Enum.Parse(typeof(SKPathFillType),
                        fillTypePicker.Items[fillTypePicker.SelectedIndex])
    };
    path.MoveTo(info.Width / 2, info.Height / 2 - radius);

    for (int i = 1; i < 5; i++)
    {
        // angle from vertical
        double angle = i * 4 * Math.PI / 5;
        path.LineTo(center + new SKPoint(radius * (float)Math.Sin(angle),
                                        -radius * (float)Math.Cos(angle)));
    }
    path.Close();

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 50,
        StrokeJoin = SKStrokeJoin.Round
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    switch (drawingModePicker.SelectedIndex)
    {
        case 0:
            canvas.DrawPath(path, fillPaint);
            break;

        case 1:
            canvas.DrawPath(path, strokePaint);
            break;

        case 2:
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case 3:
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

Za normálních okolností typ výplně cesty by měl mít vliv pouze výplněmi a není tahy, ale dva `Inverse` režimy ovlivnit výplně a tahy. Pro výplně, dva `Inverse` typy vyplnit oblasti oppositely tak, aby se naplní oblasti mimo hvězdičkou. Pro tahy, dva `Inverse` typy barvu všechno kromě tahu. Pomocí těchto typů inverzní výplně může vytvořit některé liché důsledky, jako ukazuje na snímku obrazovky iOS:

[![](fill-types-images/fivepointedstar-small.png "Trojitá snímek obrazovky stránky hvězdičky Five-Pointed")](fill-types-images/fivepointedstar-large.png#lightbox "Trojitá snímek obrazovky stránky Five-Pointed hvězdičkou")

Mobilní snímky obrazovky Android a Windows zobrazit typické lichý a vinutí důsledky, ale pořadí tahu a výplně ovlivní také výsledky.

Algoritmus vinutí je závislá na směru, že jsou vykreslovány řádky. Obvykle při vytváření cestu, můžete určit, že směr jako určíte, že řádky jsou vykreslovány z jednoho bodu do jiného. Ale `SKPath` třída také definuje metody, třeba `AddRect` a `AddCircle` , kreslení celý rozvrhů. Chcete-li řídit, jak jsou vykreslovány tyto objekty, metody obsahovat parametr typu [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/), který má dva členy:

- [`Clockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.Clockwise/)
- [`CounterClockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.CounterClockwise/)

Metody v `SKPath` , zahrnout `SKPathDirection` parametr zadejte pro něj výchozí hodnota je `Clockwise`.

**Kruhy překrývajících se** stránky vytvoří cestu s čtyři kruhy překrývajících se s typ výplně lichý cesta:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(info.Width, info.Height) / 4;

    SKPath path = new SKPath
    {
        FillType = SKPathFillType.EvenOdd
    };

    path.AddCircle(center.X - radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X - radius / 2, center.Y + radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y + radius / 2, radius);

    SKPaint paint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    canvas.DrawPath(path, paint);

    paint.Style = SKPaintStyle.Stroke;
    paint.StrokeWidth = 10;
    paint.Color = SKColors.Magenta;

    canvas.DrawPath(path, paint);
}
```

Jedná se o zajímavých obrázek vytvořen s minimální kódu:

[![](fill-types-images/overlappingcircles-small.png "Trojitá snímek obrazovky stránky kruhy překrývajících se")](fill-types-images/overlappingcircles-large.png#lightbox "Trojitá snímek obrazovky stránky kruhy překrývajících se")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
