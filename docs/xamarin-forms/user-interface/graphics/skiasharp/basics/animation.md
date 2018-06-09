---
title: Základní animace v SkiaSharp
description: Tento článek vysvětluje, jak animace grafiky SkiaSharp v aplikacích Xamarin.Forms a to ukazuje s ukázkový kód.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 08583a62719927b900c6aeede1b3b4398ed803de
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243340"
---
# <a name="basic-animation-in-skiasharp"></a>Základní animace v SkiaSharp

_Způsob animace SkiaSharp grafiky_

Můžete animace SkiaSharp grafiky ve Xamarin.Forms tím, že na `PaintSurface` metoda, která se má volat velmi často, pokaždé, když se může lišit kreslení grafiky. Tady je animace uvidíte později v tomto článku doplněnou soustředných kroužky, které zdánlivě rozbalte ze softwaru:

![](animation-images/animationexample.png "Několik soustředných kroužky zdánlivě rozšiřování z centra")

**Pulsating elipsy** stránku [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program animuje dvěma osami elipsy tak, aby se zdá, že se pulsating a můžete řídit i počet tento opatřen:


[ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) soubor vytvoří platformě Xamarin.Forms `Slider` a `Label` zobrazit aktuální hodnotu jezdce. Toto je běžný způsob, jak integrovat `SKCanvasView` s dalšími zobrazeními Xamarin.Forms:

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

Vytvoří soubor kódu `Stopwatch` objekt, který má sloužit jako hodiny vysokou přesnost. `OnAppearing` Přepsat nastaví `pageIsActive` do `true` a volá metodu s názvem `AnimationLoop`. `OnDisappearing` Přepsání sad, které `pageIsActive` do `false`:

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

`AnimationLoop` Metoda spustí `Stopwatch` a pak smyčky při `pageIsActive` je `true`. Toto je v podstatě "nekonečné smyčce" stránky je aktivní, ale nezpůsobuje program přesah vzhledem k tomu, že dojde k závěru, že opakování ve smyčce pomocí volání `Task.Delay` s `await` operátor, který umožňuje dalších částí funkce programu. Argument `Task.Delay` způsobí, že její dokončení za sekundu 1/30. Definuje obnovovací frekvence animace.

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

`while` Smyčky začíná získáním času cyklu od `Slider`. Toto je čas v sekundách, například 5. Druhý příkaz vypočítá hodnotu `t` pro *čas*. Pro `cycleTime` 5, `t` zvyšuje od 0 do 1 každých 5 sekund. Argument `Math.Sin` funkce v druhý příkaz rozsahu od 0 do 2π každých 5 sekund. `Math.Sin` Funkce vrátí hodnotu v rozsahu od 0 do 1 zpět na hodnotu 0 a pak na &ndash;1 a 0 každých 5 sekund, ale s hodnotami, které pomaleji změnit, pokud je hodnota téměř 1 nebo -1. Hodnota 1 je přidáván, takže jsou vždy aktivní kladné hodnoty a pak je rozdělený podle 2, takže hodnoty v rozsahu od ½ na 1 ½ 0 ½, ale pomalu, pokud hodnota je přibližně 1 a 0. Toto je uloženo v `scale` pole a `SKCanvasView` je zrušena.

`PaintSurface` Tato metoda používá `scale` hodnotě k výpočtu dvěma osami elipsy:

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

Metoda vypočítá maximální radius na základě velikosti oblasti zobrazení a minimální radius podle maximální radius. `scale` Hodnota je animované mezi 0 a 1 a zpět na hodnotu 0, aby metoda používá k výpočtu `xRadius` a `yRadius` , je v rozsahu `minRadius` a `maxRadius`. Kreslení a vyplňte elipsy se používají tyto hodnoty:

[![](animation-images/pulsatingellipse-small.png "Trojitá snímek obrazovky stránky blikající elipsy")](animation-images/pulsatingellipse-large.png#lightbox "Trojitá snímek obrazovky stránky blikající třemi tečkami")

Všimněte si, že `SKPaint` objekt se vytvoří v `using` bloku. Jako mnoho tříd SkiaSharp `SKPaint` je odvozena z `SKObject`, která je odvozena z `SKNativeObject`, který implementuje [ `IDisposable` ](https://developer.xamarin.com/api/type/System.IDisposable/) rozhraní. `SKPaint` přepsání `Dispose` metodu pro uvolnění nespravovaných prostředků.

 Vložení `SKPaint` v `using` bloku zajišťuje, že `Dispose` nazývá na konci bloku k uvolnění těchto nespravovaných prostředků. To se stane, přesto když paměti používané `SKPaint` uvolněno objekt modulem garbage collector v rozhraní .NET, ale v animace kódu, je nejvhodnější zajistit poněkud proaktivní v uvolnění paměti v více odpovídajícím.

 Lepší řešení v tomto případě by vytvořte dvě `SKPaint` objekty jednou a uložit je jako pole.

Co se **rozšiřování kroužky** nemá animace. [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) Třída začne definováním několik polí, včetně `SKPaint` objektu:

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

Tento program používá jiný přístup k animace podle platformě Xamarin.Forms `Device.StartTimer`. `t` Pole je animovaný od 0 do 1 každý `cycleTime` milisekundách:

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

`PaintSurface` Obslužná rutina nevykresluje 5 soustředných kroužky s animovaný poloměr. Pokud `baseRadius` proměnná počítá se jako 100, pak jako `t` je animovaný od 0 do 1, poloměr nárůst pět kroužky od 0 do 100, 100 až 200, 200 až 300, 300 až 400 a 400 na 500. Pro většinu kroužky `strokeWidth` je 50, ale pro první kroužek `strokeWidth` animuje od 0 do 50. Pro většinu kroužky je modrou barvu, ale pro poslední kruhu barvu animovaný z modré na průhledné:

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

Výsledkem je, že obrázek vypadá stejné při `t` rovná 0, jako když `t` rovná 1 a kroužky zdá se, že chcete-li pokračovat, navždy rozšíření:

[![](animation-images/expandingcircles-small.png "Trojitá snímek obrazovky stránky rozšiřování kroužky")](animation-images/expandingcircles-large.png#lightbox "Trojitá snímek obrazovky stránky rozšiřování kroužky")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
