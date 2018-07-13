---
title: Pixely a jednotky nezávislé na zařízení
description: Tento článek popisuje rozdíly mezi souřadnice ve Skiasharpu a souřadnice Xamarin.Forms a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: b0655934b0389b06dca5ef6bab3badc0023400b4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997375"
---
# <a name="pixels-and-device-independent-units"></a>Pixely a jednotky nezávislé na zařízení

_Prozkoumáte rozdíly mezi souřadnice Xamarin.Forms a souřadnice ve Skiasharpu_

Tento článek popisuje rozdíly mezi souřadnicový systém používán ve Skiasharpu a Xamarin.Forms. Můžete získat informace k převodu mezi dvěma systémy souřadnic a také nakreslit grafiku, které vyplnit ke konkrétní oblasti:

![](pixels-images/screenfillexample.png "Objekt, který vyplní celé obrazovky")

Pokud nějakou dobu, jsme se programování v Xamarin.Forms, bude pravděpodobně chování pro souřadnice Xamarin.Forms a velikostí. Kruhy vykreslen v předchozích dvou článcích zdát trochu malé vám.

Těchto kruzích *jsou* malé ve srovnání s velikostí Xamarin.Forms. Ve výchozím nastavení kreslení ve Skiasharpu v jednotkách, které pixelů, zatímco Xamarin.Forms základů souřadnice a velikosti na jednotce nezávislých na zařízení stanovené základní platformy. (Další informace o Xamarin.Forms souřadnicový systém najdete v [kapitola 5. Řešení velikostí](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) knihy *vytváření mobilních aplikací pomocí Xamarin.Forms*.)

Na stránce v [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program s názvem **Surface velikost** používá ve Skiasharpu textový výstup zobrazuje velikost zobrazovacím povrchu ze tří různých zdrojů:

- Normální Xamarin.Forms [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) a [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) vlastnosti `SKCanvasView` objektu.
- [ `CanvasSize` ](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKCanvasView.CanvasSize/) Vlastnost `SKCanvasView` objektu.
- [ `Size` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Size/) Vlastnost `SKImageInfo` hodnotu, která je konzistentní s `Width` a `Height` vlastnosti používané ve dvou předchozí stránky.

[ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) Třídy ukazuje, jak zobrazit tyto hodnoty. Konstruktor uloží `SKCanvasView` objektu jako pole, aby byla přístupná v `PaintSurface` obslužné rutiny události:

```csharp
SKCanvasView canvasView;

public SurfaceSizePage()
{
    Title = "Surface Size";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvas` obsahuje šest různých `DrawText` metody, ale to [ `DrawText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawText/p/System.String/System.Single/System.Single/SkiaSharp.SKPaint/) je nejjednodušší metoda:

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Zadejte textový řetězec, ve kterém se má text začít, souřadnic X a Y a `SKPaint` objektu. Souřadnice X Určuje, kde je umístěn levé straně textu, ale watch navýšení kapacity: Určuje souřadnici Y umístění *směrného plánu* textu. Pokud někdy ručně jste napsali na linkované papíru, je směrný plán na řádku, na které sit znaků a pod sestup které dolní dotahy (například na těch, na písmena g, p, q a y).

`SKPaint` Objektu umožňuje zadat barvu textu, rodina písem a velikost textu. Ve výchozím nastavení [ `TextSize` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.TextSize/) vlastnost má hodnotu 12, což vede k velmi malé na zařízeních s vysokým rozlišením, jako jsou telefony. V jinde nejjednodušší aplikace, budete také potřebovat některé informace o velikosti textu, který chcete zobrazit. `SKPaint` Třída definuje [ `FontMetrics` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontMetrics/) vlastnost a několik [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) metody, ale méně nápadité potřebám [ `FontSpacing` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontSpacing/) Vlastnosti zajišťující Doporučená hodnota mezery mezi po sobě jdoucích řádků textu.

Následující `PaintSurface` vytvoří obslužnou rutinu `SKPaint` objekt pro `TextSize` 40 pixelů, což je požadované výšky textu od horního okraje horní a dolní dotahy. `FontSpacing` Hodnota, která `SKPaint` vrátí objekt je o něco větší než, o 47 pixelů.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 40
    };

    float fontSpacing = paint.FontSpacing;
    float x = 20;               // left margin
    float y = fontSpacing;      // first baseline
    float indent = 100;

    canvas.DrawText("SKCanvasView Height and Width:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(String.Format("{0:F2} x {1:F2}",
                                  canvasView.Width, canvasView.Height),
                    x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKCanvasView CanvasSize:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(canvasView.CanvasSize.ToString(), x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKImageInfo Size:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(info.Size.ToString(), x + indent, y, paint);
}
```

Metoda začíná první řádek textu s souřadnici X 20 (málo okraj na levé straně) a souřadnice Y `fontSpacing`, což je o něco víc, než co je nutné zobrazit úplnou výška prvního řádku textu v horní části zobrazovacím povrchu. Po každé volání `DrawText`, Souřadnice Y se zvýší o jednu nebo dvě násobcích `fontSpacing`.

Tady je program spuštěn na všech třech platformách:

[![](pixels-images/surfacesize-small.png "Trojitá snímek obrazovky stránky Surface velikost")](pixels-images/surfacesize-large.png#lightbox "Trojitá snímek Surface velikost stránky")

Jak je vidět `CanvasSize` vlastnost `SKCanvasView` a `Size` vlastnost `SKImageInfo` hodnota jsou konzistentní vzhledem k aplikacím v generování sestav pixelech. `Height` a `Width` vlastnosti `SKCanvasView` jsou vlastnosti Xamarin.Forms a zobrazit zprávu velikost zobrazení v jednotkách nezávislých na zařízení definované platformou.

Simulátor Iosu 7 na levé straně má 2 pixelů na jednotku nezávislých na zařízení a s Androidem 5 Nexus v centru má 3 pixelů na jednotku. To je důvod, proč jednoduchého kruhu je uvedeno výše má různé velikosti na různých platformách.

Pokud si přejete pracovat jenom v jednotkách nezávislých na zařízení, můžete provést tak, že nastavíte `IgnorePixelScaling` vlastnost `SKCanvasView` k `true`. Však nemusí být výsledky. Ve Skiasharpu vykreslí grafiky na menší plochy zařízení, se rovná velikosti zobrazení v jednotkách nezávislých na zařízení velikost v pixelech. (Například ve Skiasharpu byste použili zobrazení surface 360 x 512 pixelů na Nexus 5.) Potom škálování této bitové kopie v velikost, čímž jaggies znatelný rastrového obrázku.

Pokud chcete zachovat stejné rozlišení obrázku, lepším řešením je napsat vlastní jednoduché funkce pro převod mezi těmito dvěma systémy souřadnic.

Kromě `DrawCircle` metody `SKCanvas` také definuje dva `DrawOval` metody, které nakreslíte elipsu. Elipsa je definována dvě poloměry místo jednoho protokolu radius. Toto jsou známé jako *hlavní radius* a *menší radius*. `DrawOval` Metoda nakreslí elipsu s dvěma poloměry paralelního na osách X a Y. Omezení je možné překonat s transformací nebo použijte cestu grafiky (k věnujeme později), ale [to `DrawOval` metoda](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) názvy dvou poloměry argument `rx` a `ry` označuje, že je paralelně OS X a Y:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Je možné pro kreslení elipsy, která naplní zobrazovacím povrchu? **Vyplnit elipsu** stránce ukazuje, jak. `PaintSurface` Obslužné rutině událostí ve [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) odečte poloviční šířku tahu ze třídy `xRadius` a `yRadius` hodnoty přizpůsobit celý tři tečky a jeho Osnova v rámci zobrazovacím povrchu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float strokeWidth = 50;
    float xRadius = (info.Width - strokeWidth) / 2;
    float yRadius = (info.Height - strokeWidth) / 2;

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = strokeWidth
    };
    canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
}
```

Tady je spuštěn na třech platformách:

[![](pixels-images/ellipsefill-small.png "Trojitá snímek obrazovky stránky Surface velikost")](pixels-images/ellipsefill-large.png#lightbox "Trojitá snímek Surface velikost stránky")

[Jiných `DrawOval` metoda](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/) má [ `SGRect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRect/) argument, který je definován jako souřadnice X a Y levého horního rohu a pravém dolním rohu obdélníku. Elipsy vyplní obdélníku, který naznačuje, že je možné pro použití v **Vyplnit elipsu** stránky následujícím způsobem:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Nicméně, který zkrátí všechny okraji osnovy na tři tečky na čtyřech stranách. Je potřeba upravit všechny `SKRect` na základě argumenty konstruktoru `strokeWidth` provést tuto práci vpravo:

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
