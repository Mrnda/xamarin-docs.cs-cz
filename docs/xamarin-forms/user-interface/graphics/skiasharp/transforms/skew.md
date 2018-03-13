---
title: "Zkosení transformace"
description: "V tématu jak můžete vytvořit nakloněné grafické objekty v SkiaSharp zkosení transformace"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: charlespetzold
ms.author: chape
ms.date: 03/20/2017
ms.openlocfilehash: a18b60d486a911e4a76298fd20a70f16ac392881
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="the-skew-transform"></a>Zkosení transformace

_V tématu jak můžete vytvořit nakloněné grafické objekty v SkiaSharp zkosení transformace_

V SkiaSharp nastane zkosení transformace grafické objekty, například stín na tomto obrázku:

![](skew-images/skewexample.png "Příklad zkosení z programu zkreslit stínu textu")

Zkosení transformuje obdélníků na parallelograms, ale zkreslilo elipsy je stále elipsy.

I když Xamarin.Forms definuje vlastnosti pro překlad, škálování a otáčení, není žádná odpovídající vlastnost Xamarin.Forms pro zkosení.

[ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/System.Single/System.Single/) Metodu `SKCanvas` přijímá dva argumenty pro vodorovného a svislého zkreslit:

```csharp
public void Skew (Single xSkew, Single ySkew)
```

Druhý [ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/SkiaSharp.SKPoint/) metoda kombinuje těchto argumentů v jediném `SKPoint` hodnotu:

```csharp
public void Skew (SKPoint skew)
```

Je však nepravděpodobné, že budete používat jednu z těchto dvou metod v izolaci.

**Zkreslit experimentovat** stránce lze experimentovat s zkosení hodnoty rozsahu – 10 až 10. Textový řetězec je umístěn v levém horním rohu stránky s zkosení hodnoty získané ze dvou `Slider` elementy. Tady je `PaintSurface` obslužné rutiny v [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) třídy:

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

Hodnoty `xSkew` argument posunutí dolní části textu pro kladné hodnoty nebo doleva pro záporné hodnoty. Hodnoty `ySkew` posunout dolů vpravo od textu, kladné hodnoty nebo službu záporné hodnoty:

[![](skew-images/skewexperiment-small.png "Trojitá snímek obrazovky stránky zkreslit experimentu")](skew-images/skewexperiment-large.png#lightbox "Trojitá snímek obrazovky stránky zkreslit experimentu")

Pokud `xSkew` je záporná z `ySkew`, výsledek je otočení, ale také poněkud škálovat jako Windows označuje zobrazení.

Transformace vzorce jsou následující:

x' = x + xSkew · y

y' = ySkew · x + y

Například pro pozitivní `xSkew` hodnota, transformovaných `x'` hodnota se zvyšuje s rostoucím `y` zvyšuje. To je, co způsobí, že náklon.

Pokud trojúhelníček 200 pixelů a 100 pixelů je nastavený s jeho levého horního rohu v bodě (0, 0) a je vykreslen pomocí `xSkew` hodnotu 1.5, následující rovnoběžník výsledky:

![](skew-images/skeweffect.png "Účinek zkosení transformace v obdélníku")

Souřadnice dolního okraje mít `y` hodnoty od 100, takže je zapuštěno 150 pixelů vpravo.

Pro nenulové hodnoty `xSkew` nebo `ySkew`, pouze bodu (0, 0) zůstává stejná. Tento bod lze považovat za středu zkosení. Pokud budete potřebovat středu zkosení být něčím jiným (které je obvykle případ), neexistuje žádný `Skew` metoda, která stanoví, že. Je potřeba explicitně kombinovat `Translate` volá s `Skew` volání. Na střed zkosení v `px` a `py`, zkontrolujte následující volání:

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

Složené transformace vzorce jsou:

x: = x + xSkew · (y – py)

y' = ySkew · (x – px) + y

Pokud `ySkew` nula a pouze určujete nenulovou hodnotu `xSkew`, pak `px` není použita hodnota. Hodnota je důležité a stejně tak pro `ySkew` a `py`.

Může si myslíte pohodlnější zadání zkosení jako úhel Náklon, jako je například úhel α do tohoto diagramu:

![](skew-images/skewangleeffect.png "Uvedené vliv zkosení transformace na obdélníku s zkosení úhlu")

Poměr shift 150 pixelů na 100 pixelů svislou je tangens úhlu, v tomto příkladu 56.3 stupňů.

Soubor XAML **zkreslit experimentu úhel** stránka je podobná **zkreslit úhel** stránky vyjma toho, že `Slider` elementy v rozsahu od –90 do 90 stupňů. [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) Souboru kódu na pozadí Zarovná text na stránce na střed a používá `Translate` nastavit středu zkosení k centru stránky. Malou `SkewDegrees` metoda v dolní části kód převede úhly zkosení hodnoty:

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

Jako úhlu blíží kladné a záporné 90 stupňů, tangens blíží infinity, ale úhly až o 80 stupních nebo tak lze použít:

[![](skew-images/skewangleexperiment-small.png "Trojitá snímek obrazovky stránky zkreslit experimentu úhel")](skew-images/skewangleexperiment-large.png#lightbox "Trojitá snímek obrazovky stránky zkreslit úhel experimentu")

Malé záporné vodorovné zkosení mohou napodobovat výběr kurzívy nebo nakloněného text, jako **nakloněné Text** ukazuje stránky. [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) Třída ukazuje, jak se provádí:

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

`TextAlign` Vlastnost `SKPaint` je nastaven na `Center`. Bez jakékoli transformace `DrawText` volání s souřadnice (0, 0) by způsobilo pozici text vodorovně na střed standardních hodnot v levém horním rohu. `SkewDegrees` Zkosí text ve vodorovném směru 20 stupňů relativně k směrného plánu. `Translate` Volání přesune vodorovném centru směrného plánu text na střed plátna:

[![](skew-images/obliquetext-small.png "Trojitá snímek obrazovky stránky nakloněné Text")](skew-images/obliquetext-large.png#lightbox "Trojitá snímek obrazovky stránky nakloněné textu")

**Zkreslit Text stínové** stránky ukazuje, jak lze pomocí kombinace 45 stupňů zkosení a svislého škálování na Stín textu, který nastane mimo text. Tady je příslušné části `PaintSurface` obslužné rutiny:

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

Stínové kopie je zobrazené první a potom text:

[![](skew-images/skewshadowtext1-small.png "Trojitá snímek obrazovky stránky zkreslit Text stínové")](skew-images/skewshadowtext1-large.png#lightbox "Trojitá snímek obrazovky stránky zkreslit stínu textu")

Předaný svislé souřadnice `DrawText` metoda označuje pozici textu relativně k směrného plánu. To je stejné souřadnice svislé používá pro středu zkosení. Tento postup nebude fungovat, pokud textový řetězec obsahuje dolní dotahy. Například nahraďte slovo "quirky" pro "Stínové" a zde je výsledek:

[![](skew-images/skewshadowtext2-small.png "Trojitá snímek obrazovky stránky zkreslit stínové Text alternativní slovem s dolní dotahy")](skew-images/skewshadowtext2-large.png#lightbox "Trojitá snímek obrazovky stránky zkreslit stínové Text alternativní slovem s dolní dotahy")

Stínové a text je zarovnán stále v směrného plánu, ale účinek právě vypadá nesprávný. Chcete-li odstranit ji musíte získat hranice text:

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

Nyní se bude stín rozšiřuje v dolní části těchto dolní dotahy:

[![](skew-images/skewshadowtext3-small.png "Trojitá snímek obrazovky stránky zkreslit Text stínové úprav pro dolní dotahy")](skew-images/skewshadowtext3-large.png#lightbox "Trojitá snímek obrazovky stránky zkreslit Text stínové úprav pro dolní dotahy")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
