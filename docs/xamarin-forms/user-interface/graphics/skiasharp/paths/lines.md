---
title: Čáry a zakončení tahů
description: Tento článek vysvětluje, jak můžete ve Skiasharpu kreslení čar pomocí různých zakončení tahů v aplikacích Xamarin.Forms a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 000bf24c1b06baab892f0b165c8b9eeebebce49d
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615259"
---
# <a name="lines-and-stroke-caps"></a>Čáry a zakončení tahů

_Další informace o použití ve Skiasharpu kreslení čar pomocí různých zakončení tahů_

V SkiaSharp vykreslování jedné čáry je značně odlišná od vykreslení řadu připojené přímé čáry. I při vykreslování jedné čáry, ale často je potřeba poskytnout řádcích šířka tahu konkrétní a čím širší řádku, nejdůležitější změní vzhled konce řádků, volá se, *zakončení tahu*:

![](lines-images/strokecapsexample.png "Tři možnosti zakončení tahu")

Pro vykreslování jedné čáry `SKCanvas` definuje jednoduchý [ `DrawLine` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawLine/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) metoda, jejíž argumenty označuje počáteční a koncovou souřadnice řádek s `SKPaint` objektu:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

Ve výchozím nastavení `StrokeWidth` vlastnictví nově vytvořenou instanci `SKPaint` objekt je 0, který má stejný účinek jako hodnota 1 při vykreslování řádku o jeden pixel v tloušťku. Tento popis se zobrazuje velmi dynamického zajišťování na zařízeních s vysokým rozlišením jako jsou telefony, takže budete pravděpodobně chtít nastavit `StrokeWidth` větší hodnotu. Po spuštění kreslení čar proměnlivou velikostí tloušťku ohraničení, která vyvolá jiný problém, ale: jak zahájení a ukončení tyto řádky tlustých vykreslení?

Je volána vzhled zahájení a ukončení řádků *zakončení řádku* nebo v Skia, *zakončení tahu*. Slovo "limit" v tomto kontextu označuje druh hat &mdash; něco, který je umístěný na konci řádku. Můžete nastavit [ `StrokeCap` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeCap/) vlastnost `SKPaint` objektu do jedné z následujících členů [ `SKStrokeCap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeCap/) výčtu:

- [`Butt`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Butt/) (výchozí)
- [`Square`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)

Ty se nejlépe popisují ukázkový program. Druhá část na domovskou stránku [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program začíná na stránku s názvem **zakončení tahů** na základě [ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs) třídy. Tuto stránku definuje `PaintSurface` obslužná rutina události, která prochází tří členů `SKStrokeCap` výčet, název člena výčtu zobrazení a kreslení čáry použitím stroke cap:

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

Pro každého člena `SKStrokeCap` výčet, obslužná rutina nakreslí dva řádky jeden tloušťka tahu 50 pixelů a další řádek umístěný v horní části tloušťka tahu 2 pixelů. Tento druhý řádek je určený k objasnění geometrické začátku a konci řádku nezávisle na tloušťku čáry a zakončení tahů:

[![](lines-images/strokecaps-small.png "Trojitá snímek obrazovky stránky zakončení tahů")](lines-images/strokecaps-large.png#lightbox "Trojitá snímek obrazovky stránky zakončení tahů")

Jak je vidět, `Square` a `Round` zakončení tahů efektivně rozšířit délka řádku poloviční šířku tahu na začátek řádku a znovu na konci. Toto rozšíření je důležitá, pokud je potřeba určit dimenze vykreslené grafického objektu.

`SKCanvas` Třída také obsahuje jinou metodu pro vytvoření více řádků, který je o něco společné:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points` Parametr je pole `SKPoint` hodnoty a `mode` je členem skupiny [ `SKPointMode` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPointMode/) výčet, který má tři členy:

- [`Points`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Points/) k vykreslení jednotlivých bodů
- [`Lines`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Lines/) pro každý pár body připojení
- [`Polygon`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Polygon/) pro všechny po sobě následujícími body připojení

**Více řádků** stránce ukazuje tuto metodu. [ `MultipleLinesPage` Soubor XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml) vytvoří dvě `Picker` zobrazení, která vám umožní vybrat člena `SKPointMode` výčet a členem skupiny `SKStrokeCap` výčtu:

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

`SelectedIndexChanged` Obslužné rutiny pro obě `Picker` zobrazení jednoduše zruší platnost `SKCanvasView` objektu:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Tato obslužná rutina musí zkontrolovat existenci `SKCanvasView` objekt protože obslužná rutina události je první voláno, když `SelectedIndex` vlastnost `Picker` nastavena na 0 v souboru XAML a, který předchází `SKCanvasView` po vytvoření instance.

`PaintSurface` Obslužná rutina přistupuje k obecné metody pro získání dvou vybrané položky z `Picker` zobrazení a převod na hodnoty výčtu:

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

Snímek obrazovky ukazuje různé `Picker` výběr na třech platformách:

[![](lines-images/multiplelines-small.png "Trojitá snímek obrazovky stránky více řádků")](lines-images/multiplelines-large.png#lightbox "Trojitá snímek obrazovky stránky více řádků")

IPhone na levém ukazuje jak `SKPointMode.Points` způsobí, že člen výčtu `DrawPoints` k vykreslení všech bodů v `SKPoint` pole jako čtverec, pokud je zakončení řádku `Butt` nebo `Square`. Kruhy jsou generovány, pokud je zakončení řádku `Round`.

Pokud místo toho použijete `SKPointMode.Lines`, jak je znázorněno na obrazovce s Androidem v centru `DrawPoints` metoda nakreslí čáru mezi dvěma `SKPoint` pomocí limit zadaný řádek v tomto případě `Round`.

UPW – snímek obrazovky ukazuje výsledek `SKPointMode.Polygon` hodnotu. Řádek je vykreslen mezi po sobě následujících bodů v poli, ale podíváte úzce uvidíte, že nejste připojeni tyto řádky. Každá z těchto samostatné řádky začíná a končí limit zadaný řádek. Pokud vyberete `Round` velká, může být čar k připojení, ale ve skutečnosti nejsou připojené.

Zda řádky jsou připojené nebo Nepřipojeno je zásadní aspekt práce s grafické cesty.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
