---
title: "Řádky a tahu CAP k vzdálené ploše"
description: "Další informace o použití SkiaSharp kreslení čar pomocí různých tahu CAP k vzdálené ploše"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 341d850709ff27f4dc397cee3bb2fc5f73c0ec3c
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="lines-and-stroke-caps"></a>Řádky a tahu CAP k vzdálené ploše

_Další informace o použití SkiaSharp kreslení čar pomocí různých tahu CAP k vzdálené ploše_

V SkiaSharp je příliš neliší od vykreslování řadu připojené přímky vykreslování jeden řádek. I při vykreslování jedné čáry, ale často je nutné dát řádky šířku tahu konkrétní a tím širší řádku, důležitější stane vzhled konce řádků, volá se *tahu cap*:

![](lines-images/strokecapsexample.png "Možnosti tři tahu CAP k vzdálené ploše")

Pro kreslení jedné čar `SKCanvas` definuje jednoduchou [ `DrawLine` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawLine/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) metoda, jejíž argumenty označuje počáteční a koncovou souřadnice řádek s `SKPaint` objektu:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

Ve výchozím nastavení `StrokeWidth` vlastnost nově vytvořenou instanci `SKPaint` objekt je 0, který má stejný účinek jako hodnota 1 v vykreslování řádek jednoho pixelu ve tloušťka. To se zobrazí velmi tenké na zařízení s vysokým rozlišením, jako jsou telefony, takže budete pravděpodobně chtít nastavit `StrokeWidth` na větší hodnotu. Jakmile začnete kreslení čar značné množství tloušťka, který vyvolá jinému problému, ale: jak se mají spuštění a ukončení čar tyto silných Generovat?

Je volána vzhled spuštění a ukončení čar *zakončení čáry* nebo v Skia, *tahu cap*. Slovo "cap" v tomto kontextu odkazuje na druh hat &mdash; něco, která se nachází na konci řádku. Nastavíte [ `StrokeCap` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeCap/) vlastnost `SKPaint` objektu na jednu z následujících členů [ `SKStrokeCap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeCap/) výčtu:

- [`Butt`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Butt/) (výchozí)
- [`Square`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)

Tyto jsou zobrazené nejlépe s ukázka programu. Druhá část domovské stránce [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) program začíná na stránce s názvem **tahu CAP k vzdálené ploše** na základě [ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs) třídy. Tuto stránku definuje `PaintSurface` obslužné rutiny události, který prochází tři členy `SKStrokeCap` výčtu, zobrazovat název člena výčtu a kreslení pomocí cap tahu řádek:

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
        TextAlign = SKTextAlign.Center
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

    float xText = info.Width / 2;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = textPaint.FontSpacing;

    foreach (SKStrokeCap strokeCap in Enum.GetValues(typeof(SKStrokeCap)))
    {
        // Display text
        canvas.DrawText(strokeCap.ToString(), xText, y, textPaint);
        y += textPaint.FontSpacing;

        // Display thick line
        thickLinePaint.StrokeCap = strokeCap;
        canvas.DrawLine(xLine1, y, xLine2, y, thickLinePaint);

        // Display thin line
        canvas.DrawLine(xLine1, y, xLine2, y, thinLinePaint);
        y += 2 * textPaint.FontSpacing;
    }
}
```

Pro každého člena `SKStrokeCap` výčtu obslužná rutina nevykresluje dva řádky, jeden s sílu tahu 50 pixelů a další čáru umístěný v horní části s sílu tahu 2 pixelů. Tento druhý řádek je určen k objasnění geometrickou začátku a konci řádku nezávisle na tloušťku čáry a zakončení tahu:

[![](lines-images/strokecaps-small.png "Trojitá snímek obrazovky stránky tahu CAP k vzdálené ploše")](lines-images/strokecaps-large.png#lightbox "Trojitá snímek obrazovky stránky tahu CAP k vzdálené ploše")

Jak je vidět `Square` a `Round` tahu CAP k vzdálené ploše efektivně rozšířit délka řádku poloviční šířku tahu na začátek řádku a znovu na konci. Toto rozšíření změní důležité, pokud je třeba určit dimenze vykreslené grafického objektu.

`SKCanvas` Třída také obsahuje další metody pro kreslení více řádků, které se poněkud specifických:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points` Parametr je pole `SKPoint` hodnoty a `mode` je členem skupiny [ `SKPointMode` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPointMode/) výčtu, který má tři členy:

- [`Points`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Points/) k vykreslení jednotlivé body
- [`Lines`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Lines/) pro každý pár body připojení
- [`Polygon`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Polygon/) pro připojení všech po sobě jdoucích bodů

**Více řádků** stránky ukazuje tuto metodu. [ `MultipleLinesPage` Souboru XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml) vytvoří dvě instance `Picker` zobrazení, které vám umožní vybrat členem `SKPointMode` výčet a členem `SKStrokeCap` výčtu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.MultipleLinesPage"
             Title="Multiple Lines">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="pointModePicker"
                Title="Point Mode"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Points</x:String>
                <x:String>Lines</x:String>
                <x:String>Polygon</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Butt</x:String>
                <x:String>Round</x:String>
                <x:String>Square</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

`SelectedIndexChanged` Obslužné rutiny pro obě `Picker` jednoduše by způsobila neplatnost zobrazení `SKCanvasView` objektu:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Tuto obslužnou rutinu musí zkontrolovat existenci `SKCanvasView` objekt vzhledem k tomu, že je první obslužná rutina události voláno, když `SelectedIndex` vlastnost `Picker` nastavena na 0 v souboru XAML a k tomu dojde před `SKCanvasView` po vytvoření instance.

`PaintSurface` Obslužná rutina přistupuje k obecné metody pro získání dva vybrané položky z `Picker` zobrazení a jejich převedení na výčet hodnot:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create an array of points scattered through the page
    SKPoint[] points = new SKPoint[10];

    for (int i = 0; i < 2; i++)
    {
        float x = (0.1f + 0.8f * i) * info.Width;

        for (int j = 0; j < 5; j++)
        {
            float y = (0.1f + 0.2f * j) * info.Height;
            points[2 * j + i] = new SKPoint(x, y);
        }
    }

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.DarkOrchid,
        StrokeWidth = 50,
        StrokeCap = GetPickerItem<SKStrokeCap>(strokeCapPicker)
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = GetPickerItem<SKPointMode>(pointModePicker);
    canvas.DrawPoints(pointMode, points, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }
    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}
```

Na snímku obrazovky vidíte celou řadu `Picker` výběr na tři platformy:

[![](lines-images/multiplelines-small.png "Trojitá snímek obrazovky stránky více řádků")](lines-images/multiplelines-large.png#lightbox "Trojitá snímek obrazovky stránky více řádků")

Pro iPhone na levém ukazuje jak `SKPointMode.Points` – člen výčtu způsobí, že `DrawPoints` k vykreslení každý z bodů `SKPoint` pole jako čtverce, pokud je zakončení čáry `Butt` nebo `Square`. Kroužky jsou vykreslovány, pokud je zakončení čáry `Round`.

Pokud místo toho použít `SKPointMode.Lines`, jak je znázorněno na obrazovce Android v centru `DrawPoints` metoda nakreslí mezi každý pár `SKPoint` hodnoty, v takovém případě pomocí krytky zadaný řádek `Round`.

Zařízení Windows mobile zobrazuje výsledek `SKPointMode.Polygon` hodnotu. Řádek vykreslením mezi následných body v poli, ale když se podíváte velmi úzce, uvidíte, že tyto řádky nejsou připojené. Každý z těchto samostatné řádky spustí a končí krytky zadaný řádek. Pokud jste vybrali `Round` CAP k vzdálené ploše, řádky se může zdát připojit, ale ve skutečnosti nejsou připojené.

Zda jsou řádky připojen nebo nebude připojený je velmi důležitý aspekt pracovat s grafické cesty.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
