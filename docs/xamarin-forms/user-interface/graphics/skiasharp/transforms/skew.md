---
title: Transformace zkosení
description: Tento článek vysvětluje, jak transformace zkosení vytvořit nakloněné grafických objektů v SkiaSharp a to demonstruje se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: charlespetzold
ms.author: chape
ms.date: 03/20/2017
ms.openlocfilehash: 951fc02dfff1721c1391c5d0c8a21452a156cfdb
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615350"
---
# <a name="the-skew-transform"></a>Transformace zkosení

_Podívejte se, jak transformace zkosení vytvořit nakloněné grafických objektů v ve Skiasharpu_

Transformace zkosení v SkiaSharp, nastane grafických objektů, jako je například stínové na tomto obrázku:

![](skew-images/skewexample.png "Příklad zkosení z programu zkosení stínovaný Text")

Zkosení transformuje na parallelograms obdélníky, ale je výrazně nerovnoměrnou distribucí elipsa stále elipsu.

Přestože Xamarin.Forms definuje vlastnosti pro překlad, měřítko a otočení, neexistuje žádná odpovídající vlastnost v Xamarin.Forms pro zkosení.

[ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/System.Single/System.Single/) Metodu `SKCanvas` přijímá dva argumenty pro zkosení vodorovné a svislé zkosení:

```csharp
public void Skew (Single xSkew, Single ySkew)
```

Sekundy [ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/SkiaSharp.SKPoint/) metoda kombinuje tyto argumenty v jediném `SKPoint` hodnotu:

```csharp
public void Skew (SKPoint skew)
```

Je ale pravděpodobné, že budete používat jednu z těchto dvou metod v izolaci.

**Zkosení experimentovat** stránky umožňuje experimentovat s Nerovnoměrná distribuce hodnoty rozsahu – 10 až 10. Textový řetězec je umístěn v levém horním rohu stránky s Nerovnoměrná distribuce hodnoty získané ze dvou `Slider` elementy. Tady je `PaintSurface` obslužné rutiny v [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) třídy:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        canvas.Skew((float)xSkewSlider.Value, (float)ySkewSlider.Value);
        canvas.DrawText(text, 0, -textBounds.Top, textPaint);
    }
}
```

Hodnoty `xSkew` argument posunout na konec textu pro kladné hodnoty nebo doleva pro záporné hodnoty. Hodnoty `ySkew` posunout dolů vpravo od textu, pro kladné hodnoty nebo pro záporné hodnoty:

[![](skew-images/skewexperiment-small.png "Trojitá snímek obrazovky stránky zkosení Experiment")](skew-images/skewexperiment-large.png#lightbox "Trojitá snímek obrazovky stránky zkosení experimentu")

Pokud `xSkew` záporná `ySkew`, výsledkem je otočení, ale také přizpůsobený trochu jako označuje zobrazení UPW.

Transformace vzorce jsou následující:

x! = x + xSkew. y

y' = ySkew. x + y

Například pro pozitivní `xSkew` hodnoty transformovaná `x'` hodnota se zvyšuje s rostoucím `y` zvyšuje. Je to, co způsobí, že jeho sklon.

Pokud trojúhelník 200 pixelů na šířku a 100 pixelů na výšku je umístěn pomocí jeho levého horního rohu, v okamžiku (0, 0) a je vykreslen pomocí `xSkew` hodnotu 1.5, rovnoběžník následující výsledky:

![](skew-images/skeweffect.png "Efekt transformace zkosení v obdélníku")

Souřadnice dolního okraje mají `y` hodnoty od 100, tak, aby byl posunuta doprava 150 pixelů.

Pro nenulové hodnoty `xSkew` nebo `ySkew`pouze bod (0, 0) zůstává stejná. Tento bod lze považovat za středu zkosení. Pokud budete potřebovat středu zkosení být něco jiného (což je obvykle případ), neexistuje žádný `Skew` metodu, která stanoví, že. Je potřeba explicitně kombinovat `Translate` volání s `Skew` volání. Zarovnat na střed zkosení v `px` a `py`, následující volání:

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

Kompozitní transformace vzorce jsou:

x! = x + xSkew. (y-py)

y' = ySkew. (x – px) + y

Pokud `ySkew` je nula a zadáváte pouze nenulová hodnota `xSkew`, pak `px` není použita hodnota. Hodnota je bezvýznamná a podobně pro `ySkew` a `py`.

Vám může s klidem více zadání zkosení jako úhel sklonu, jako je například úhel α v tomto diagramu:

![](skew-images/skewangleeffect.png "Efekt transformace zkosení s obdélníkem s zkosení úhel uvedené")

Poměr shift 150 pixelů na svislou 100 pixelů je tangens úhlu, v tomto příkladu 56.3 stupňů.

Do souboru XAML **zkosení Experiment úhel** stránka je podobný **zkosení úhel** stránky s výjimkou, že `Slider` prvky v rozsahu od –90 do 90 stupňů. [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) Soubor kódu na pozadí Zarovná text na stránce a používá `Translate` nastavit střed zkosení do centra pro stránky. Krátké `SkewDegrees` metoda v dolní části kód převede úhly zkosení hodnoty:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;

        canvas.Translate(xCenter, yCenter);
        SkewDegrees(canvas, xSkewSlider.Value, ySkewSlider.Value);
        canvas.Translate(-xCenter, -yCenter);
        canvas.DrawText(text, xText, yText, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

Jako úhel blíží kladné nebo záporné 90 stupňů, tangens blíží nekonečno, ale jedná o úhly až o 80 stupňů nebo tak lze použít:

[![](skew-images/skewangleexperiment-small.png "Trojitá snímek obrazovky stránky zkosení Experiment úhel")](skew-images/skewangleexperiment-large.png#lightbox "Trojitá snímek obrazovky stránky zkosení úhel experimentu")

Malé negativní vodorovné zkosení mohou napodobovat kurzívy nebo nakloněného text, jako **nakloněné Text** demonstruje stránky. [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) Třídy ukazuje, jak se to dělá:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Maroon,
        TextAlign = SKTextAlign.Center,
        TextSize = info.Width / 8       // empirically determined
    })
    {
        canvas.Translate(info.Width / 2, info.Height / 2);
        SkewDegrees(canvas, -20, 0);
        canvas.DrawText(Title, 0, 0, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

`TextAlign` Vlastnost `SKPaint` je nastavena na `Center`. Bez jakékoli transformace `DrawText` volání s souřadnice (0, 0) by způsobilo pozici text vodorovně na střed standardních hodnot v levém horním rohu. `SkewDegrees` Zkosí text ve vodorovném směru 20 stupňů vzhledem k směrného plánu. `Translate` Volání přesune vodorovné center standardních hodnot text na střed plátna:

[![](skew-images/obliquetext-small.png "Trojitá snímek obrazovky stránky nakloněné Text")](skew-images/obliquetext-large.png#lightbox "Trojitá snímek obrazovky stránky nakloněné Text")

**Zkosení stínovaný Text** stránce ukazuje, jak použít kombinaci 45 stupňů nerovnoměrné rozdělení a vertikální škálování, abyste Stín textu, který nastane pryč z textu. Tady je relevantní části `PaintSurface` obslužné rutiny:

```csharp
using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = info.Width / 6;   // empirically determined

    // Common to shadow and text
    string text = "Shadow";
    float xText = 20;
    float yText = info.Height / 2;

    // Shadow
    textPaint.Color = SKColors.LightGray;
    canvas.Save();
    canvas.Translate(xText, yText);
    canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
    canvas.Scale(1, 3);
    canvas.Translate(-xText, -yText);
    canvas.DrawText(text, xText, yText, textPaint);
    canvas.Restore();

    // Text
    textPaint.Color = SKColors.Blue;
    canvas.DrawText(text, xText, yText, textPaint);
}
```

Stínové kopie je zobrazená první a potom textu:

[![](skew-images/skewshadowtext1-small.png "Trojitá snímek obrazovky stránky zkosení stínovaný Text")](skew-images/skewshadowtext1-large.png#lightbox "Trojitá snímek obrazovky stránky zkosení stínovaný Text")

Svislé souřadnice předán `DrawText` metody určuje pozici textu vzhledem k směrného plánu. Je to stejné svislé souřadnice používaná pro středu zkosení. Tento postup nebude fungovat, pokud textový řetězec obsahuje dolní dotahy. Například nahraďte slovo "quirky" pro "Stín" a tady je výsledek:

[![](skew-images/skewshadowtext2-small.png "Trojitá snímek obrazovky stránky zkosení stínovaný Text alternativní slovem s dolní dotahy")](skew-images/skewshadowtext2-large.png#lightbox "Trojitá snímek obrazovky stránky zkosení stínovaný Text alternativní slovem s dolní dotahy")

Stín a text je stále zarovnán na standardních hodnot, ale účinek vypadá stejně nesprávné. Chcete-li vyřešit ho budete muset získat rozsah textu:

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

`Translate` Volání musí být upravena podle výšky dolní dotahy:

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

Nyní má stín sahá od dolní dotahy tyto:

[![](skew-images/skewshadowtext3-small.png "Trojitá snímek obrazovky stránky zkosení stínovaný Text s úpravami dotahy")](skew-images/skewshadowtext3-large.png#lightbox "Trojitá snímek obrazovky stránky zkosení stínovaný Text s úpravami dotahy")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
