---
title: Integrace textu a obrázků
description: Tento článek vysvětluje, jak určit velikost vykreslené textový řetězec integrovat text ve Skiasharpu grafiky do aplikací Xamarin.Forms a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: be1524029ada79896f83517c3b439f2ad0e2c6d9
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615158"
---
# <a name="integrating-text-and-graphics"></a>Integrace textu a obrázků

_Informace o tom, k určení velikosti vykreslené textový řetězec textu integrovat grafiky ve Skiasharpu_

Tento článek ukazuje, jak měřit text, pravděpodobně škálování text, který má určité velikosti a integrovat jiných grafických text:

![](text-images/textandgraphicsexample.png "Text obklopený obdélníků")

SkiaSharp `Canvas` třída také obsahuje metody pro vykreslení obdélníku ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) a obdélník zaoblené rohy ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/)). Tyto metody vyžadují obdélníku je definovat jako `SKRect` hodnotu.

**Uvedeny Text** stránka centra krátký textový řetězec na stránce a obklopí ho objektu Frame skládá z dvojice zaoblené obdélníky. [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) Třídy ukazuje, jak se to dělá.

V SkiaSharp použijete `SKPaint` třída set text a atributy písma, ale můžete také použít k získání vykreslené velikost textu. Začátek následující `PaintSurface` obslužná rutina události zavolá dvě různé `MeasureText` metody. První [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) volání má jednoduchou `string` argument a vrátí šířku pixel text podle aktuální atributy písma. Program vypočítá nové `TextSize` vlastnost `SKPaint` objektu podle tohoto vykreslené šířku aktuálního `TextSize` vlastnost a šířku zobrazované oblasti. To slouží k nastavení `TextSize` tak, aby textový řetězec k vykreslení na 90 % šířka obrazovky:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string str = "Hello SkiaSharp!";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Chocolate
    };

    // Adjust TextSize property so text is 90% of screen width
    float textWidth = textPaint.MeasureText(str);
    textPaint.TextSize = 0.9f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

Druhá [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/) volání má `SKRect` argument, takže získává šířku a výšku vykresleného textu. `Height` Vlastnosti tohoto `SKRect` hodnota závisí na přítomnosti velká písmena, horní a dolní dotahy v textovém řetězci. Různé `Height` hodnoty jsou hlášeny pro text řetězce "mom", "cat" a "pes", např.

`Left` a `Top` vlastnosti `SKRect` struktura označení souřadnice levého horního rohu vykresleného textu, pokud se zobrazí text `DrawText` volání s X a Y pozice 0. Například, pokud tento program běží na simulátor pro zařízení iPhone 7, `TextSize` je přiřazena hodnota 90.6254 jako výsledek výpočtu po prvním volání `MeasureText`. `SKRect` Hodnota získaná z druhé volání `MeasureText` má následující hodnoty vlastností:

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

Mějte na paměti, že souřadnic X a Y můžete předat `DrawText` metody zadejte levé části textu na směrný plán. `Top` Hodnota značí, že text rozšiřuje 68 pixelů nad tohoto směrného plánu a (odečtením 68 it z 88) 20 pixelů pod směrného plánu. `Left` Hodnota 6 značí, zda text začíná na 6 pixelů napravo od hodnoty X ve `DrawText` volání. To umožňuje normální mezery mezi znaky. Pokud chcete správně zapojena zobrazení textu v levém horním rohu zobrazení, předejte negativy těchto `Left` a `Top` hodnoty souřadnic X a Y `DrawText`, v tomto příkladu &ndash;6 a 68.

`SKRect` Struktury definuje několik praktický vlastnostem a metodám, některé z nich se používá ve zbývající části `PaintSurface` obslužné rutiny. `MidX` a `MidY` hodnoty označují souřadnice na střed obdélníku. (V příkladu iPhone 7, jsou tyto hodnoty 338.4107 a &ndash;24.) Následující kód používá tyto hodnoty pro výpočet nejjednodušší souřadnic vystředí text na displeji:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(str, xText, yText, textPaint);
    ...
}
```

`PaintSurface` Obslužná rutina končí dvě volání na `DrawRoundRect`, které vyžadují argumenty `SKRect`. To `SKRect` hodnota je určitě podobně jako `SKRect` hodnota získaná `MeasureText` metody, ale nemohou být stejné. Nejprve, musí se jednat o něco větší tak, aby zakulacený obdélník nebude nakreslit přes okraji textu a za druhé, musí být posunuty v prostoru tak, aby `Left` a `Top` hodnoty odpovídají levého horního rohu, kde má být obdélník umístěn. Tyto dvě úlohy můžete provést tak `Offset` a `Inflate` metody definované `SKRect`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Create a new SKRect object for the frame around the text
    SKRect frameRect = textBounds;
    frameRect.Offset(xText, yText);
    frameRect.Inflate(10, 10);

    // Create an SKPaint object to display the frame
    SKPaint framePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5,
        Color = SKColors.Blue
    };

    // Draw one frame
    canvas.DrawRoundRect(frameRect, 20, 20, framePaint);

    // Inflate the frameRect and draw another
    frameRect.Inflate(10, 10);
    framePaint.Color = SKColors.DarkBlue;
    canvas.DrawRoundRect(frameRect, 30, 30, framePaint);
}
```

Pod je zbytek metodu přímočaré. Vytvoří další `SKPaint` objekt ohraničení a volání `DrawRoundRect` dvakrát. Druhé volání použije obdélník zvětšený jiného 10 pixelů. První volání Určuje poloměr zaoblení 20 pixelů druhý má poloměr zaoblení 30 pixelů, takže se zobrazí jako paralelní:

 [![](text-images/framedtext-small.png "Trojitá snímek obrazovky stránky uvedeny Text")](text-images/framedtext-large.png#lightbox "Trojitá snímek obrazovky stránky uvedeny Text")

Můžete ji na telefonu nebo simulátoru otočením zobrazíte text a zvýšit velikost rámce.

Pokud potřebujete pouze center nějaký text na obrazovce, které můžete provést přibližně bez měření text tak, že nastavíte `TextAlign` vlastnost `SKPaint` k `SKTextAlign.Center`. Souřadnice X, zadáte `DrawText` – metoda pak určuje, kde je umístěn centru vodorovného textu. Pokud předáte středního obrazovky `DrawText` metody text bude možné vodorovně na střed a *téměř* svisle na střed vzhledem k tomu, že vertikálně zarovnané směrného plánu.

Samotný text můžete mnohem zacházet jako s grafickém. Jednou z jednoduchých možností je zobrazení osnovy textové znaky spíše než normální plného zobrazení:

[![](text-images/outlinedtext-small.png "Trojitá snímek obrazovky stránky uvedeno Text")](text-images/outlinedtext-large.png#lightbox "ztrojnásobit snímek obrazovky stránky uvedeno Text")

Je to jednoduše tak, že změna normální `Style` vlastnost `SKPaint` objekt z jeho výchozí nastavení `SKPaintStyle.Fill` k `SKPaintStyle.Stroke` a zadáním šířku tahu. `PaintSurface` Obslužná rutina **uvedených Text** stránka zobrazuje, jak se to dělá:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "OUTLINE";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 1,
        FakeBoldText = true,
        Color = SKColors.Blue
    };

    // Adjust TextSize property so text is 95% of screen width
    float textWidth = textPaint.MeasureText(text);
    textPaint.TextSize = 0.95f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

 V posledních několika článcích, máte teď viděli, jak ve Skiasharpu použít k vykreslení textu, kruhy, symbol tří teček a zaoblené obdélníky. Dalším krokem je [ve Skiasharpu čáry a cesty](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md) ve kterém se naučíte k vykreslení spojené čáry cesty grafiky.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
