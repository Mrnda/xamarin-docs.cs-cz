---
title: Integrace s Xamarin.Forms
description: Vytvoření SkiaSharp grafiky, které reagují na touch a elementy Xamarin.Forms
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 3edc71977820ca618447e02caa032cf908e1aae4
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/03/2018
---
# <a name="integrating-with-xamarinforms"></a>Integrace s Xamarin.Forms

_Vytvoření SkiaSharp grafiky, které reagují na touch a elementy Xamarin.Forms_

Grafika SkiaSharp můžete integrovat s ostatními Xamarin.Forms několika způsoby. Zkombinováním SkiaSharp plátno a Xamarin.Forms prvků na stejné stránce a elementy Xamarin.Forms i pozice nad plátno SkiaSharp:

![](integration-images/integrationexample.png "Výběr barvy s posuvníky")

Další postup pro vytváření interaktivních SkiaSharp grafiky v Xamarin.Forms je prostřednictvím dotykového ovládání.
Na druhou stránku [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) nárok program **klepněte na přepnutí vyplnění**. Nakreslí Jednoduchý kruh dva způsoby, jak &mdash; bez výplně a s výplní &mdash; přepínat stav klepněte na. [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) Třída ukazuje, jak změnit SkiaSharp grafiky v reakci na vstup uživatele.

Pro tuto stránku `SKCanvasView` vytvoření instance třídy v [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) souboru, který také nastaví platformě Xamarin.Forms [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) v zobrazení:

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

Upozornění `skia` deklaraci oboru názvů XML.

`Tapped` Obslužné rutiny pro `TapGestureRecognizer` objekt jednoduše přepíná hodnota logická hodnota pole a volání [ `InvalidateSurface` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface()/) metodu `SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

Volání `InvalidateSurface` efektivně generuje volání `PaintSurface` obslužná rutina, která používá `showFill` pole zadejte nebo nevyplní kruhu:

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

`StrokeWidth` Vlastnost byla nastavena na 50 věnované rozdíl. Najdete v části šířku celý řádek kreslení vnitřku nejprve a obrys. Ve výchozím nastavení, obrázky vykresleného později v grafiky `PaintSurface` obslužné rutiny události skrývat těch, které vykresluje dříve v obslužná rutina.

**Barva prozkoumat** stránky ukazuje, jak můžete také integrovat SkiaSharp grafiky s jinými prvky Xamarin.Forms a také ukazuje rozdíl mezi dvě alternativní metody pro definování barvy v SkiaSharp. Statické [ `SKColor.FromHsl` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsl/p/System.Single/System.Single/System.Single/System.Byte/) metoda vytvoří `SKColor` hodnota založená na modelu Hue-sytost světlost:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

Statické [ `SKColor.FromHsv` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsv/p/System.Single/System.Single/System.Single/System.Byte/) metoda vytvoří `SKColor` hodnota založená na modelu podobné Hue-sytost-hodnota:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

V obou případech `h` argument v rozmezí od 0 do 360. `s`, `l`, A `v` argumenty v rozsahu od 0 do 100. `a` (Alpha nebo krytí) argument rozmezí od 0 do 255.

[ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) soubor vytvoří dvě `SKCanvasView` objekty v `StackLayout` -souběžného s `Slider` a `Label` zobrazení, které umožňují uživateli vybrat HSL a HSV – hodnoty barev:

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

Dva `SKCanvasView` prvky jsou v jednotlivých buněk `Grid` s `Label` uložený v horní části pro zobrazení výsledné hodnoty RGB barev.

[ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) souboru kódu je poměrně jednoduché. Sdílený `ValueChanged` obslužné rutiny pro tři `Slider` elementy jednoduše by způsobila neplatnost obě `SKCanvasView` elementy. `PaintSurface` Obslužné rutiny vymazat na plátno barvou indikován `Slider` prvky a také nastavit `Label` uložený na `SKCanvasView` prvky:

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

HSL i HSV barva modely hodnota Hue rozsah od 0 do 360 a označuje dominantní odstín barvy. Jedná se o tradiční barvy rainbow: červená, oranžová, žlutý, zelený, modrá, indigově modré, fialová a zpět v kruh a red.

V modelu HSL hodnotu 0 pro světlost je vždy černá a 100 hodnota je vždy bílé. Pokud je hodnota sytost 0, jsou hodnoty světlosti mezi 0 a 100 odstínech šedi. Zvýšení sytost přidá další barev. Čistý barvy (které jsou hodnoty RGB u některé komponenty rovna 255, jiné rovna 0 a třetí rozsahu od 0 do 255) dojít, když Sytost je 100 a světlost je 50.

V modelu HSV čistý barvy dojít při sytost a hodnota je 100. Pokud je hodnota 0, bez ohledu na ostatní nastavení, je černá barvu. Šedé odstínech nastane, když je sytost 0 a hodnotu v rozmezí od 0 do 100.

Ale nejlepší způsob, jak podívat, dva modely a experimentovat s nimi sami:

[![](integration-images/colorexplore-large.png "Trojitá snímek obrazovky stránky prozkoumat barva")](integration-images/colorexplore-small.png#lightbox "Trojitá snímek obrazovky stránky prozkoumat barev")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
