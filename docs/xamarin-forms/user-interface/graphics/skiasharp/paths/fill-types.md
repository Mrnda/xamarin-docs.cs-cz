---
title: Typy výplně cesty
description: Tento článek zkoumá různé účinky možné s typy výplně cesty ve Skiasharpu a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 17043054c920a69570f38b227d05980494e29139
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615467"
---
# <a name="the-path-fill-types"></a>Typy výplně cesty

_Objevte možnosti s typy výplně cesty ve Skiasharpu odlišnými efekty_

Dva rozvrhy v cestě může dojít k překrytí a řádky, které společně tvoří jednu rozvrh může dojít k překrytí. Žádné uzavřené oblast může potenciálně vyplní, nemusí ale být vhodné tak, aby vyplnil všechny uzavřené oblasti. Tady je příklad:

![](fill-types-images/filltypeexample.png "Ukazuje pěti hvězdičkami částečně filles")

Máte málo kontrolu nad tím. Řídí algoritmus naplnění [ `SKFillType` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.FillType/) vlastnost `SKPath`, které jste nastavili na člen [ `SKPathFillType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathFillType/) výčtu:

- [`Winding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.Winding/), výchozí hodnota
- [`EvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.EvenOdd/)
- [`InverseWinding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseWinding/)
- [`InverseEvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseEvenOdd/)

Algoritmy obtáčení a lichý určují, jestli všechny uzavřené oblasti je vyplněný nebo není doplní podle hypotetické linií z této oblasti do nekonečna. Tento řádek překročí jeden nebo více hranic řádků, které tvoří cestu. Vinutí režim Pokud počet hranic čáry dekorace v jednom směru zůstatek na počet řádků v opačným směrem a pak v oblasti není vyplněné. V opačném případě se vyplní oblast. Algoritmus lichý vyplní oblast, pokud počet hranic řádků je liché.

S mnoha běžných cest často vinutí algoritmus vyplní všechny uzavřené oblasti cesty. Lichý algoritmus vytvoří obecně zajímavější výsledky.

Klasickým příkladem je hvězdička pět ukazuje, jak je ukázáno v **Five-Pointed hvězdičky** stránky. [FivePointedStarPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FivePointedStarPage.xaml) soubor vytvoří dvě `Picker` zobrazení vyberte cestu, zadejte typ a určuje, zda je cesta vytažený nebo vyplněné nebo obojí a v jakém pořadí:

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

Soubor kódu na pozadí používá obě `Picker` hodnoty nakreslete hvězdička pět ukazuje:

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

Za normálních okolností by měl typ výplně cesty ovlivňují jenom výplně a ne tahy, ale dvě `Inverse` režimy ovlivnit výplně a tahů. Pro výplně, dvě `Inverse` typy výplně oblasti oppositely tak, aby oblasti mimo hvězdičky zaplněný. Pro tahy dva `Inverse` typy barva vše kromě stroke. Pomocí těchto typů inverzní výplně může vytvořit některé efekty liché, jak ukazuje snímek obrazovky s Iosem:

[![](fill-types-images/fivepointedstar-small.png "Trojitá snímek obrazovky stránky Five-Pointed Star")](fill-types-images/fivepointedstar-large.png#lightbox "Trojitá snímek obrazovky stránky Five-Pointed hvězda")

Android a UPW snímky obrazovky ukazují typické lichý a vinutí efekty, ale pořadí stroke a výplň také ovlivní výsledky.

Vinutí algoritmus je závislá na směru, jsou vykreslovány vedle řádky. Obvykle při vytváření cestu, můžete řídit tomto směru zadat, že řádky jsou vykreslovány z jednoho místa do jiného. Ale `SKPath` třída rovněž definuje metody, jako je `AddRect` a `AddCircle` , který vykreslení celého rozvrhů. Pokud chcete řídit, jak tyto objekty jsou vykreslovány, metody zahrnovat parametr typu [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/), který má dva členy:

- [`Clockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.Clockwise/)
- [`CounterClockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.CounterClockwise/)

Metody v `SKPath` , které zahrnují `SKPathDirection` parametr zadejte výchozí hodnotu `Clockwise`.

**Překrývajícími se kruhy** stránka vytvoří cestu s čtyři překrývající se kruhy s typem výplně lichý cesta:

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

Je zajímavé bitovou kopii vytvořenou s minimální kód:

[![](fill-types-images/overlappingcircles-small.png "Trojitá snímek obrazovky stránky s překrývajícími se kruhy")](fill-types-images/overlappingcircles-large.png#lightbox "Trojitá snímek obrazovky stránky s překrývajícími se kruhy")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
