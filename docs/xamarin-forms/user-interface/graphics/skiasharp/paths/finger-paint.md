---
title: Malování prstem
description: Pomocí prstů k vyplnění na plátně.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: charlespetzold
ms.author: chape
ms.date: 04/05/2017
ms.openlocfilehash: 95c023d702d165b7a8a0ba392b2f87af58bfae07
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/05/2018
---
# <a name="finger-painting"></a>Malování prstem

_Pomocí prstů k vyplnění na plátně._

`SKPath` Objekt můžete průběžně aktualizovat a zobrazit. Tato funkce umožňuje cestu k používá pro interaktivní kreslení, například v finger-painting programu.

![](finger-paint-images/fingerpaintsample.png "Cvičení v Malování prstem")

Podpora touch v Xamarin.Forms neumožňuje sledování jednotlivé prsty, které na obrazovce, tak mít nežádoucí vliv touch sledování Xamarin.Forms byla vyvinuta poskytovat podporu dalších touch. Tento efekt je popsána v článku [ **vyvolání události z důsledky**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md). Ukázka programu [ **Touch sledování účinku ukázky** ](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/) obsahuje dvě stránky, které používají SkiaSharp, včetně finger-painting program.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) řešení zahrnuje tato událost sledování dotykového ovládání. Přenosná knihovna tříd projekt zahrnuje `TouchEffect` třídy, `TouchActionType` výčtu, `TouchActionEventHandler` delegovat a `TouchActionEventArgs` – třída. Všechny projekty platformy zahrnout `TouchEffect` třídy pro platformu; také obsahuje projekt pro iOS `TouchRecognizer` třídy.

**Malování prstem** stránky v **SkiaSharpFormsDemos** je zjednodušená implementace Malování prstem. Nebudou povolit výběr barvy a obtažení šířka, nemá žádný způsob, jak vymazat na plátno a samozřejmě nelze uložit kresby.

[ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml) souboru PUT `SKCanvasView` v jedné buňce `Grid` a připojí `TouchEffect` na který `Grid`:

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

Připojení `TouchEffect` přímo na `SKCanvasView` nefunguje v rámci všech platformách.

[ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs) definuje dvě kolekce pro ukládání souboru kódu `SKPath` objekty, a také `SKPaint` objektu pro vykreslení tyto cesty:

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

Jako název navrhovanou `inProgressPaths` ukládá cesty, které aktuálně přitahuje prsty, které minimálně jeden slovník. Slovník klíč je touch ID, který doprovází události dotykového ovládání. `completedPaths` Pole je kolekce cest, které byly při dokončení prstu, kreslení cesta zrušeno na obrazovce.

`TouchAction` Obslužná rutina spravuje tyto dvě kolekce. Když prstem nejprve dotykem na obrazovce novou `SKPath` se přidá do `inProgressPaths`. Při tomto prstem přesunu, další body přidány k cestě. Po vydání prstu, cesta se přenese do `completedPaths` kolekce. Můžete se více prsty malovat současně. Po každé změně na jednu z cesty nebo kolekce `SKCanvasView` zneplatněna:

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

Body doplňujícími touch sledování události jsou Xamarin.Forms souřadnice; Tyto nutné převést na SkiaSharp souřadnice, které jsou v pixelech. Je účelem `ConvertToPixel` metoda.

`PaintSurface` Obslužná rutina pak jednoduše vykreslí obě kolekce cest. Dříve dokončené cesty se zobrazí pod cesty v průběhu:

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

Vaše prstem malby jsou omezena pouze vašemu talentu:

[![](finger-paint-images/fingerpaint-small.png "Trojitá snímek obrazovky stránky Malování prstem")](finger-paint-images/fingerpaint-large.png#lightbox "Trojitá snímek obrazovky stránky prstem Malování")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Ukázky vliv touch sledování (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [Vyvolání událostí z efekty](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
