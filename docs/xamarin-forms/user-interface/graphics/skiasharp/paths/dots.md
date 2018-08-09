---
title: Tečky a pomlčky ve Skiasharpu
description: Tento článek popisuje, jak hlavní složitými rozhraními z kreslení ve Skiasharpu tečkovaná a přerušované čáry a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 7c336e6b5224f61ff84eb39652788b23f52b806e
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615415"
---
# <a name="dots-and-dashes-in-skiasharp"></a>Tečky a pomlčky ve Skiasharpu

_Hlavní složitými rozhraními z kreslení ve Skiasharpu tečkovaná a přerušované čáry_

Ve Skiasharpu umožňuje kreslení čar, které nejsou pevné, ale místo toho se skládá z tečky a čárky:

![](dots-images/dottedlinesample.png "Tečkovaná čára")

Použijete k tomu *vliv cestu*, což je instance [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) třídu, která nastavíte, a [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) vlastnost `SKPaint`. Můžete vytvořit cestu efekt (nebo kombinace efekty cest) pomocí statické `Create` metody definované `SKPathEffect`.

Chcete-li nakreslit tečkovaná nebo přerušované čáry, použijte [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) statické metody. Existují dva argumenty: Toto pole je nejprve `float` hodnoty, které označují délek tečky a pomlčky a délku mezery mezi nimi. Toto pole musí obsahovat sudý počet prvků a měla by existovat aspoň dva elementy. (Může existovat nulovým počtem elementů jako pole, ale jehož výsledkem je plná čára.) Pokud existují dva prvky, první je délka tečky nebo pomlčky a druhý je délka mezeru před další tečky nebo pomlčky. Pokud existuje více než dva prvky, pak jsou v tomto pořadí: pomlčka délku, délku mezery, pomlčky délka, délku mezery a tak dále.

Obecně platí budete chtít provést pomlčky a mezery délky násobkem šířka tahu. Pokud je šířka tahu 10 pixelů, například pole {10, 10} bude nakreslete tečkovaná čára kde tečky a mezery mají stejnou délku jako tloušťka tahu.

Ale `StrokeCap` nastavení `SKPaint` objekt ovlivní také tyto tečky a spojovníky. Jak zobrazí krátce, který má vliv na elementy tohoto pole.

Tečkované a přerušované čáry je ukázán v **tečky a pomlčky** stránky. [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) soubor vytvoří dvě `Picker` zobrazení, jeden pro umožňující vybrat zakončení tahů a druhý k výběru dash pole:

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

První tři položky v `dashArrayPicker` předpokládá, že šířka tahu je 10 pixelů. {10, 10} pole je pro tečkovaná čára {30, 10} je pro na přerušovanou čáru a {10, 10, 30, 10} je pro řádek tečky a pomlčky. (Další tři probereme za chvíli.)

[ `DotsAndDashesPage` Soubor kódu na pozadí](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) obsahuje `PaintSurface` obslužná rutina události a pomocné rutiny pro přístup k několika `Picker` zobrazení:

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

Na následujících snímcích obrazovky ukazuje na obrazovce iOS úplně vlevo tečkovaná čára:

[![](dots-images/dotsanddashes-small.png "Trojitá snímek obrazovky stránky tečky a čárky")](dots-images/dotsanddashes-large.png#lightbox "Trojitá snímek obrazovky stránky tečky a spojovníky")

Však Android obrazovka by měla také zobrazit tečkovaná čára použití pole {10, 10}, ale místo toho je plná. Co se stalo? Problém se také, že Android obrazovky je nastavení zakončení tahu `Square`. Tato zásada rozšiřuje všechny pomlčky podle poloviční šířku tahu způsobuje vyplnit mezery.

Chcete tento problém vyřešit, při použití zakončení tahu z `Square` nebo `Round`, musíte zmenšit dash délky pole podle tahu (někdy výsledkem dash délku 0) a zvýšení délky gap podle úhozů. To je jak pomlčka poslední tři pole ve `Picker` se vypočítaly v souboru XAML:

- {10, 10} stane {0, 20} pro tečkovaná čára
- {30, 10} stane {20, 20} pro přerušované čáry
- {10, 10, 30, 10} {0, 20, 20, 20} se stane tečkovaná a přerušované čáry

Ukazuje obrazovku UPW, tečkami, jež Čárkovaná čára pro tah cap z `Round`. `Round` Zakončení tahu poskytuje silné čáry často vypadalo nejlépe, tečky a pomlčky.

Zatím žádná zmínka se neprovedl druhého parametru `SKPathEffect.CreateDash` metody. Tento parametr je pojmenován `phase` a odkazuje na posun mezi vzorem pomlček a tečku na začátku řádku. Například, pokud je pole dash {10, 10} a `phase` je 10 a pak řádek začíná mezerou spíše než tečku.

Jedním zajímavým `phase` parametr je animace. **Animovat něco jako Spirála** je podobná stránka **něco jako Spirála Archimedean** stránce, s výjimkou, že [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs) animuje třídy `phase` parametr. Na stránce také ukazuje další způsob, jak animace. Předchozí příklad [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs) použít `Task.Delay` metodu pro ovládací prvek animace. Tento příklad používá místo toho Xamarin.Forms `Device.Timer` metody:


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

Samozřejmě budete mít ve skutečnosti program zobrazíte animaci spustit:

[![](dots-images/animatedspiral-small.png "Trojitá snímek obrazovky stránky animovat něco jako Spirála")](dots-images/animatedspiral-large.png#lightbox "Trojitá snímek obrazovky stránky animovat něco jako Spirála")

Už teď víte, jak kreslení čar a definovat křivky pomocí parametrické rovnice. Část aby byly publikované a později se bude zabývat různými druhy křivky, který `SKPath` podporuje.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
