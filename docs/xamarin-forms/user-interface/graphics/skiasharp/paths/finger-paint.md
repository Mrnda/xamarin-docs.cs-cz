---
title: Malování prstem v ve Skiasharpu
description: Tento článek vysvětluje, jak pomocí prstů k vykreslení na plátně ve Skiasharpu v Xamarin.Forms aplikací a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: charlespetzold
ms.author: chape
ms.date: 04/05/2017
ms.openlocfilehash: b0f28cd3e8a928a6da3169dee96ec089178a64e2
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615818"
---
# <a name="finger-painting-in-skiasharp"></a>Malování prstem v ve Skiasharpu

_Pomocí prstů malovat na plátně._

`SKPath` Objektu můžete průběžně aktualizovat a zobrazit. Tato funkce umožňuje cestu k používá pro interaktivní vykreslování, jako například kreslení prsty, která aplikaci.

![](finger-paint-images/fingerpaintsample.png "Cvičení v Malování prstem")

Podpora dotykového ovládání v Xamarin.Forms neumožňuje sledování jednotlivých prsty na obrazovce, takže efekt touch sledování Xamarin.Forms byla vyvinuta zajistit podporu dalších dotykového ovládání. Tento efekt je popsaný v článku [ **vyvolání události z účinků**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md). Ukázkový program [ **ukázky dotykové ovládání sledování efekt** ](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/) obsahuje dvě stránky, které používají SkiaSharp, včetně kreslení prsty, která program.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) řešení zahrnuje této události sledování dotykové ovládání. Projekt knihovny .NET Standard zahrnuje `TouchEffect` třídy, `TouchActionType` výčet, `TouchActionEventHandler` delegovat a `TouchActionEventArgs` třídy. Všechny projekty platformy zahrnout `TouchEffect` třídy pro danou platformu; obsahuje také projekt pro iOS `TouchRecognizer` třídy.

**Malování prstem** stránku **SkiaSharpFormsDemos** je zjednodušenou implementaci Malování prstem. Nepodporuje umožníte výběr barvy ani obtažení šířku, nemá žádný způsob, jak vymazat na plátno a samozřejmě nelze uložit kresby.

[ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml) souboru vloží `SKCanvasView` v jedné buňce `Grid` a připojí `TouchEffect` , který `Grid`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Paths.FingerPaintPage"
             Title="Finger Paint">

    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

Připojení `TouchEffect` přímo `SKCanvasView` nefunguje v rámci všech platformách.

[ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs) použití modelu code-behind soubor definuje dvě kolekce pro ukládání `SKPath` objekty, ale i `SKPaint` objektu pro vykreslení tyto cesty:

```csharp
public partial class FingerPaintPage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };

    public FingerPaintPage()
    {
        InitializeComponent();
    }
    ...
}
```

Jako název navrhnout `inProgressPaths` slovníku ukládá cesty, které jsou právě vykreslené prsty na jeden nebo více. Klíč slovníku je touch ID, který doprovází události dotykové ovládání. `completedPaths` Pole je kolekce cest, které byly při dokončení prstem kreslení cesty zrušeno z obrazovky.

`TouchAction` Obslužná rutina spravuje tyto dvě kolekce. Když prstem nejprve dotýká obrazovky, nový `SKPath` se přidá do `inProgressPaths`. Při přesunu této prstem další body jsou přidány do cesty. Když se uvolní prstu, cesta bude převeden na `completedPaths` kolekce. Můžete s více prstů malovat současně. Po každé změně na jeden z cesty nebo kolekce `SKCanvasView` zneplatněna:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                           (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }
}
```

Bodů doprovodném dotykové ovládání – sledování událostí, které jsou souřadnice Xamarin.Forms; Toto musí být převeden na souřadnicích SkiaSharp, které jsou v pixelech. To je účel `ConvertToPixel` metody.

`PaintSurface` Obslužná rutina pak jednoduše vykreslí obě kolekce cest. Dříve dokončených cesty se zobrazí pod částí cesty v průběhu:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ,,,
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (SKPath path in completedPaths)
        {
            canvas.DrawPath(path, paint);
        }

        foreach (SKPath path in inProgressPaths.Values)
        {
            canvas.DrawPath(path, paint);
        }
    }
    ...
}
```

Vaše prstem malby jsou omezené jenom vašeho talentu:

[![](finger-paint-images/fingerpaint-small.png "Trojitá snímek obrazovky stránky Malování prstem")](finger-paint-images/fingerpaint-large.png#lightbox "Trojitá snímek obrazovky stránky Malování prstem")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Ukázky dotykové ovládání sledování efekt (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [Vyvolání události z účinků](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
