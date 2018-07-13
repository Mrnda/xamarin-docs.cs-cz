---
title: Integrace se Xamarin.Forms
description: Tento článek vysvětluje, jak vytvořit ve Skiasharpu grafiky, které reagují na dotykového ovládání a prvky Xamarin.Forms a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 35aede1a541d0ff62f6a4a5b57256c389e5a8640
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997515"
---
# <a name="integrating-with-xamarinforms"></a>Integrace se Xamarin.Forms

_Vytvořte ve Skiasharpu grafiky, které reagují na dotykové ovládání a prvky Xamarin.Forms_

Ve Skiasharpu grafiky můžete integrovat se zbytkem Xamarin.Forms několika způsoby. Můžete zkombinovat ve Skiasharpu plátno a Xamarin.Forms elementů na stejné stránce a dokonce i umístění Xamarin.Forms elementů nad plátnem ve Skiasharpu:

![](integration-images/integrationexample.png "Výběr barvy s posuvníky")

Další způsob, jak vytvářet interaktivní grafické obrazce ve Skiasharpu v Xamarin.Forms je prostřednictvím dotykové ovládání.
Na druhé stránce v [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programu je oprávněn **klepnutím na přepínač vyplnit**. Nakreslí Jednoduchý kruh dva způsoby, jak &mdash; bez výplně a s výplní &mdash; přepínat pomocí klepnutím. [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) Třídy ukazuje, jak je možné změnit grafiky ve Skiasharpu v reakci na vstup uživatele.

Pro tuto stránku `SKCanvasView` je vytvořena instance třídy v [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) soubor, který nastaví také Xamarin.Forms [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) pro zobrazení:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.TapToggleFillPage"
             Title="Tap Toggle Fill">

    <skia:SKCanvasView PaintSurface="OnCanvasViewPaintSurface">
        <skia:SKCanvasView.GestureRecognizers>
            <TapGestureRecognizer Tapped="OnCanvasViewTapped" />
        </skia:SKCanvasView.GestureRecognizers>
    </skia:SKCanvasView>
</ContentPage>
```

Všimněte si, že `skia` deklarace oboru názvů XML.

`Tapped` Obslužné rutiny pro `TapGestureRecognizer` objekt jednoduše přepne hodnotu pole Boolean a volání [ `InvalidateSurface` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface()/) metoda `SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

Volání `InvalidateSurface` efektivně generuje volání `PaintSurface` obslužná rutina, která používá `showFill` pole, které chcete vyplnit nebo není vyplnit kruhu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 50
    };
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);

    if (showFill)
    {
        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.Blue;
        canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    }
}
```

`StrokeWidth` Vlastnost byla nastavena na 50 věnované rozdíl. Šířku celého řádku vidíte, nejprve kreslení vnitřní a obrysu. Ve výchozím nastavení, přijde vykresleného v grafiky `PaintSurface` obslužnou rutinu události jsou vykreslovány dříve v obslužné rutině překrývat.

**Barva prozkoumat** stránce ukazuje, jak můžete také integrovat grafiky ve Skiasharpu další prvky Xamarin.Forms a také ukazuje rozdíl mezi dva alternativní způsoby definování barvy v SkiaSharp. Statické [ `SKColor.FromHsl` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsl/p/System.Single/System.Single/System.Single/System.Byte/) metoda vytvoří `SKColor` hodnota založená na modelu Hue-sytost světlosti:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

Statické [ `SKColor.FromHsv` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsv/p/System.Single/System.Single/System.Single/System.Byte/) metoda vytvoří `SKColor` hodnoty na základě podobné hodnotu Hue sytosti modelu:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

V obou případech platí `h` argument v rozsahu od 0 do 360. `s`, `l`, A `v` argumentů v rozsahu od 0 do 100. `a` (Alfa a krytí) argumentu v rozsahu 0 až 255.

[ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) soubor vytvoří dva `SKCanvasView` objekty v `StackLayout` – souběžně s `Slider` a `Label` zobrazení, které uživateli umožňují vybrat HSL a HSV hodnot barev:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Basics.ColorExplorePage"
             Title="Color Explore">
    <StackLayout>
        <!-- Hue slider -->
        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference hueSlider},
                              Path=Value,
                              StringFormat='Hue = {0:F0}'}" />

        <!-- Saturation slider -->
        <Slider x:Name="saturationSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference saturationSlider},
                              Path=Value,
                              StringFormat='Saturation = {0:F0}'}" />

        <!-- Lightness slider -->
        <Slider x:Name="lightnessSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference lightnessSlider},
                              Path=Value,
                              StringFormat='Lightness = {0:F0}'}" />

        <!-- HSL canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hslCanvasView"
                               PaintSurface="OnHslCanvasViewPaintSurface" />

            <Label x:Name="hslLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>

        <!-- Value slider -->
        <Slider x:Name="valueSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference valueSlider},
                              Path=Value,
                              StringFormat='Value = {0:F0}'}" />

        <!-- HSV canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hsvCanvasView"
                               PaintSurface="OnHsvCanvasViewPaintSurface" />

            <Label x:Name="hsvLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Dva `SKCanvasView` prvky jsou v jedné buňce `Grid` s `Label` na horní části pro zobrazení výsledná hodnota barvy RGB.

[ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) soubor kódu na pozadí je poměrně jednoduché. Sdílený `ValueChanged` obslužné rutiny pro tři `Slider` prvky jednoduše zruší platnost obě `SKCanvasView` elementy. `PaintSurface` Obslužné rutiny zrušte na plátno s barvou indikován `Slider` elementy a také nastavit `Label` sedí nahoře `SKCanvasView` prvky:

```csharp
public partial class ColorExplorePage : ContentPage
{
    public ColorExplorePage()
    {
        InitializeComponent();

        hueSlider.Value = 0;
        saturationSlider.Value = 100;
        lightnessSlider.Value = 50;
        valueSlider.Value = 100;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        hslCanvasView.InvalidateSurface();
        hsvCanvasView.InvalidateSurface();
    }

    void OnHslCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsl((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)lightnessSlider.Value);
        args.Surface.Canvas.Clear(color);

        hslLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }

    void OnHsvCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsv((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)valueSlider.Value);
        args.Surface.Canvas.Clear(color);

        hsvLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }
}
```

V HSL i HSV modely barva Hue hodnotu od 0 do 360 a označuje dominantní odstín barvy. Toto jsou tradiční barvy rainbow: red, oranžová, žlutá, zelená, modrá, indigo, violet a znovu v kruhu, red.

V modelu HSL – hodnoty 0 pro světlosti je vždy černá a 100 hodnota je vždy bílé. Pokud je hodnota sytost 0, jsou světlosti hodnoty v rozmezí 0 až 100 odstíny šedé. Zvýšení sytost přidá další barvu. Čistě barvy (které jsou hodnoty RGB jedna komponenta rovna 255, jiný roven 0 a třetí od 0 do 255) dojít, pokud Sytost je 100 a světlosti je 50.

V modelu HSV čistě barvy dojít při sytost a hodnota je 100. Pokud je hodnota 0, bez ohledu na případná další nastavení, je černá barva. Odstíny šedé dojít, když Sytost je 0 a hodnotu od 0 do 100.

Ale můžete experimentovat s nimi sami je nejlepší způsob, jak získat představu dvou modelů:

[![](integration-images/colorexplore-large.png "Trojitá snímek obrazovky stránky barva prozkoumat")](integration-images/colorexplore-small.png#lightbox "Trojitá snímek obrazovky stránky prozkoumat barva")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
