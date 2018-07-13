---
title: Základní animace v ve Skiasharpu
description: Tento článek vysvětluje, jak animace grafiky ve Skiasharpu v Xamarin.Forms aplikací a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 393716e17b042224f2b0bae8c526132489af26c6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994809"
---
# <a name="basic-animation-in-skiasharp"></a>Základní animace v ve Skiasharpu

_Objevte, jak animace grafiky ve Skiasharpu_

Lze animovat grafiky ve Skiasharpu v Xamarin.Forms způsobením `PaintSurface` metodu, která se velmi často volána pokaždé, když vykreslování grafiky trochu jinak. Tady je animace, uvidíte později v tomto článku se soustředných bodovým zdánlivě rozbalte v Centru pro:

![](animation-images/animationexample.png "Několik soustředných kruhy zdánlivě rozšíření z centra")

**Pulsating Elipsa** stránku [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program animuje dvě osy elipsa tak, aby se zdá být pulsating a dokonce můžete řídit rychlost tento opatřen:


[ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) soubor vytvoří Xamarin.Forms `Slider` a `Label` zobrazíte aktuální hodnotu posuvníku. Toto je běžný způsob, jak integrovat `SKCanvasView` s dalšími zobrazeními Xamarin.Forms:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.PulsatingEllipsePage"
             Title="Pulsating Ellipse">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Slider x:Name="slider"
                Grid.Row="0"
                Maximum="10"
                Minimum="0.1"
                Value="5"
                Margin="20, 0" />

        <Label Grid.Row="1"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Cycle time = {0:F1} seconds'}"
               HorizontalTextAlignment="Center" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Vytvoří soubor kódu na pozadí `Stopwatch` objektu, která bude sloužit jako hodin vysokou přesností. `OnAppearing` Přepsání sady `pageIsActive` pole `true` a volá metodu s názvem `AnimationLoop`. `OnDisappearing` Přepsání sad, které `pageIsActive` pole `false`:

```csharp
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float scale;            // ranges from 0 to 1 to 0

public PulsatingEllipsePage()
{
    InitializeComponent();
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    AnimationLoop();
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    pageIsActive = false;
}
```

`AnimationLoop` Spuštění metody `Stopwatch` a potom smyčky při `pageIsActive` je `true`. Toto je v podstatě "nekonečná smyčka" na stránce je aktivní, ale nezpůsobí program přestane reagovat, protože dojde k závěru smyčky voláním `Task.Delay` s `await` operátor, který umožňuje jiné části funkce programu. Argument `Task.Delay` způsobí, že se dokončí za sekundu 1/30. Definuje frekvenci snímků animace.

```csharp
async Task AnimationLoop()
{
    stopwatch.Start();

    while (pageIsActive)
    {
        double cycleTime = slider.Value;
        double t = stopwatch.Elapsed.TotalSeconds % cycleTime / cycleTime;
        scale = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;
        canvasView.InvalidateSurface();
        await Task.Delay(TimeSpan.FromSeconds(1.0 / 30));
    }

    stopwatch.Stop();
}

```

`while` Smyčky začíná získáním času cyklu od `Slider`. Toto je doba v sekundách, například 5. Druhý příkaz vypočítá hodnotu `t` pro *čas*. Pro `cycleTime` 5, `t` zvyšuje od 0 do 1 každých 5 sekund. Argument `Math.Sin` funkce v druhý příkaz rozsahu od 0 do 2π každých 5 sekund. `Math.Sin` Funkce vrací hodnotu od 0 do 1 zpět na 0 a potom na &ndash;1 a 0 každých 5 sekund, ale s hodnotami, které se mění pomaleji, pokud je hodnotou téměř 1 nebo -1. Hodnota 1 přičtena tak, aby se vždy kladné hodnoty a pak je rozdělen 2, takže hodnoty od ½ na 1 ½ 0 ½, ale pomalu, pokud hodnota je přibližně 1 až 0. Toto je uloženo v `scale` pole a `SKCanvasView` zneplatněna.

`PaintSurface` Tato metoda používá `scale` hodnotě k výpočtu dvě osy se třemi tečkami:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float maxRadius = 0.75f * Math.Min(info.Width, info.Height) / 2;
    float minRadius = 0.25f * maxRadius;

    float xRadius = minRadius * scale + maxRadius * (1 - scale);
    float yRadius = maxRadius * scale + minRadius * (1 - scale);

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.Color = SKColors.Blue;
        paint.StrokeWidth = 50;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);

        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.SkyBlue;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
    }
}
```

Metoda vypočítá maximální radius na základě velikosti oblasti zobrazení, minimální radius založeny na maximální radius. `scale` Je animovaný hodnotu mezi 0 a 1 a zpět na 0, tak metodu, která používá k výpočtu `xRadius` a `yRadius` , který je v rozsahu `minRadius` a `maxRadius`. Tyto hodnoty slouží k vykreslení a vyplnit elipsu:

[![](animation-images/pulsatingellipse-small.png "Trojitá snímek obrazovky stránky blikající Elipsa")](animation-images/pulsatingellipse-large.png#lightbox "Trojitá snímek obrazovky stránky blikající elipsa")

Všimněte si, že `SKPaint` objekt je vytvořen v `using` bloku. Například mnoho tříd ve Skiasharpu `SKPaint` je odvozena z `SKObject`, která je odvozena z `SKNativeObject`, která implementuje [ `IDisposable` ](xref:System.IDisposable) rozhraní. `SKPaint` přepsání `Dispose` metodu pro uvolnění nespravovaných prostředků.

 Vložení `SKPaint` v `using` bloku zajišťuje, že `Dispose` je volána na konci bloku k uvolnění těchto nespravovaných prostředků. K tomu přesto při paměti používané `SKPaint` je uvolněn objekt systému uvolňování paměti .NET, ale v kódu animace, je nejlepší v uvolňování paměti v více odpovídajícím poněkud aktivní.

 V tomto konkrétním případě lepší řešení je vytvořit dva `SKPaint` objekty jednou a uložit je jako pole.

To **rozbalení kruhy** nemá animace. [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) Třídy začíná tak, že definujete několik polí, včetně `SKPaint` objektu:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    const double cycleTime = 1000;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float t;
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke
    };

    public ExpandingCirclesPage()
    {
        Title = "Expanding Circles";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ...
}
```

Tento program používá jiný přístup k animace založené na Xamarin.Forms `Device.StartTimer`. `t` Pole je animovaný od 0 do 1 každý `cycleTime` milisekundách:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            t = (float)(stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }
            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`PaintSurface` 5 soustředných kruhy s animovaný poloměry nakreslí obslužné rutiny. Pokud `baseRadius` proměnné se vypočte takto: 100, pak jako `t` je animovaný od 0 do 1, poloměr pět kruhy nárůst od 0 do 100, 100 až 200, 200 až 300, 300 až 400 a 400 až 500. Pro většinu z kruhů `strokeWidth` je 50, ale pro první kruh, `strokeWidth` animuje od 0 do 50. Pro většinu z kruhů je modrá barva, ale pro poslední kruhu je animovaný barvu z modré na transparentní:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
        float baseRadius = Math.Min(info.Width, info.Height) / 12;

        for (int circle = 0; circle < 5; circle++)
        {
            float radius = baseRadius * (circle + t);

            paint.StrokeWidth = baseRadius / 2 * (circle == 0 ? t : 1);
            paint.Color = new SKColor(0, 0, 255,
                (byte)(255 * (circle == 4 ? (1 - t) : 1)));

            canvas.DrawCircle(center.X, center.Y, radius, paint);
        }
    }
}
```

Výsledkem je, že obrázek vypadá shodovat `t` rovná 0, jako když `t` je rovno 1 a kruhy zdá se, že pokračujte v rozbalování navždy:

[![](animation-images/expandingcircles-small.png "Trojitá snímek obrazovky stránky rozbalení kruhy")](animation-images/expandingcircles-large.png#lightbox "Trojitá snímek obrazovky stránky rozbalení kruhy")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
