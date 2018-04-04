---
title: Tečky a pomlčky
description: Hlavní rozbor všech nástroje kreslení čar desítkovém a přerušovanou v SkiaSharp
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 274c8e9a79fa3fadff14f1174d86aad04d902b05
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="dots-and-dashes"></a>Tečky a pomlčky

_Hlavní rozbor všech nástroje kreslení čar desítkovém a přerušovanou v SkiaSharp_

SkiaSharp umožňuje kreslení čar, které nejsou plnou, ale místo toho se skládají ze tečky a pomlčky:

![](dots-images/dottedlinesample.png "Tečkovaná čára")

Můžete to provést pomocí *efektu cesta*, což je instance [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) třídu, která můžete nastavit na hodnotu [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) vlastnost `SKPaint`. Můžete vytvořit cestu efekt (nebo zkombinujte cesta efekty) pomocí statické `Create` metody definované `SKPathEffect`.

Kreslení čar desítkovém nebo přerušované, můžete použít [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) statickou metodu. Existují dva argumenty: Toto pole je nejdřív `float` hodnoty, které označují délek tečky a pomlčky a délka mezery mezi nimi. Toto pole musí obsahovat sudý počet elementů a měla by existovat aspoň dva elementy. (Může být nulový počet elementů v poli, ale jehož výsledkem je plná čára.) Pokud existují dva elementy, první je délka tečkou nebo pomlčkou a druhý je délka mezera před další tečkou nebo pomlčkou. Pokud existuje více než dva elementy, pak jsou v tomto pořadí: čárka délka, délka mezeru, dash délka, délka mezery a tak dále.

Obecně platí budete chtít provést délky dash a mezery mezi násobkem šířku tahu. Pokud šířku tahu je 10 pixelů, například pak pole {10, 10} bude zakreslit tečkovaná čára kde tečky a mezer mají stejnou délku jako sílu tahu.

Ale `StrokeCap` nastavit `SKPaint` objekt ovlivní také tyto tečky a pomlčky. Jak se krátce zobrazí, který má vliv na prvky tohoto pole.

S tečkami a přerušované čáry je ukázán na **tečky a pomlčky** stránky. [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) soubor vytvoří dvě instance `Picker` zobrazení, jeden pro umožňují vyberte zakončení tahu a druhou pro vyberte dash pole:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.DotsAndDashesPage"
             Title="Dots and Dashes">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="0"
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

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>10, 10</x:String>
                <x:String>30, 10</x:String>
                <x:String>10, 10, 30, 10</x:String>
                <x:String>0, 20</x:String>
                <x:String>20, 20</x:String>
                <x:String>0, 20, 20, 20</x:String>
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

První tři položky v `dashArrayPicker` předpokládá, že šířku tahu je 10 pixelů. {10, 10} pole je pro tečkovaná čára {30, 10} je pro na přerušovanou čáru a {10, 10, 30, 10} je pro řádek tečky a pomlčky. (Další tři bude za chvíli popsané.)

[ `DotsAndDashesPage` Souboru kódu](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) obsahuje `PaintSurface` obslužné rutiny události a několika pomocné rutiny pro přístup k `Picker` zobrazení:

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
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = (SKStrokeCap)Enum.Parse(typeof(SKStrokeCap),
                        strokeCapPicker.Items[strokeCapPicker.SelectedIndex]),

        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }

    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string[] strs = picker.Items[picker.SelectedIndex].Split(new char[] { ' ', ',' },
                                                             StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

Na následujících snímcích obrazovky obrazovce iOS zcela vlevo znázorňuje tečkovaná čára:

[![](dots-images/dotsanddashes-small.png "Trojitá snímek obrazovky stránky tečky a pomlčky")](dots-images/dotsanddashes-large.png#lightbox "Trojitá snímek obrazovky stránky tečky a pomlčky")

Však obrazovce Android také má zobrazit tečkovaná čára pomocí pole {10, 10}, ale místo toho řádek je plná. Co se stalo? Problém je, že obrazovce Android také má nastavení tahu CAP k vzdálené ploše `Square`. Tato zásada rozšiřuje všechny pomlčky podle poloviční šířku tahu způsobuje zaplnit mezer.

Chcete-li získat tento problém vyřešit, při použití zakončení tahu z `Square` nebo `Round`, musíte snížit dash délky v poli tahu délkou (někdy výsledkem dash délku 0) a zvýšit délky mezera délkou tahu. Jedná se jak čárka poslední tři pole v `Picker` se počítá v souboru XAML:

- {10, 10} se změní na {0, 20} pro tečkovaná čára
- {30, 10} stane {20, 20} pro na přerušovanou čáru
- {10, 10, 30, 10} stane {0, 20, 20, 20} pro desítkovém a přerušované čáry

Zobrazí obrazovky systému Windows, které s tečkami a přerušovanou řádek tah cap z `Round`. `Round` Zakončení tahu často poskytuje nejlepší vzhled tečky a pomlčky v silné čáry.

Pokud byl proveden žádné zmínky o druhý parametr `SKPathEffect.CreateDash` metoda. Tento parametr je s názvem `phase` odkazuje posun, tečky a pomlčky vzor pro začátek řádku. Například, pokud je pole dash {10, 10} a `phase` je 10 a pak na začátku řádku mezera spíše než tečku.

Jeden zajímavé aplikaci `phase` parametr je v animace. **Animovaný Spirála** je podobná stránce **Archimedean Spirála** stránky, vyjma toho, že [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs) animuje – třída `phase` parametr. Stránky také ukazuje další způsob, jak animace. Z předchozího příkladu [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs) použít `Task.Delay` metoda řídit animace. Tento příklad používá místo toho platformě Xamarin.Forms `Device.Timer` metoda:


```csharp
const double cycleTime = 250;       // in milliseconds

SKCanvasView canvasView;
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float dashPhase;

public AnimatedSpiralPage()
{
    Title = "Animated Spiral";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    stopwatch.Start();

    Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
    {
        double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
        dashPhase = (float)(10 * t);
        canvasView.InvalidateSurface();

        if (!pageIsActive)
        {
            stopwatch.Stop();
        }

        return pageIsActive;
    });
}
```

Samozřejmě budete muset skutečně program animace:

[![](dots-images/animatedspiral-small.png "Trojitá snímek obrazovky stránky animovaný Spirála")](dots-images/animatedspiral-large.png#lightbox "Trojitá snímek obrazovky stránky animovaný Spirála")

Nyní jste viděli kreslení čar a křivek pomocí čištění vzorce definovat. Část později publikování se bude zabývat různými typy křivek který `SKPath` podporuje.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
