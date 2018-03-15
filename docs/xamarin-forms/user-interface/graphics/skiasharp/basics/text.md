---
title: "Integrace textu a obrázků"
description: "Zjistit, jak určit velikost vykreslené textový řetězec textu integrovat SkiaSharp grafiky"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 1d174e441cd46255d62283521e7db2802b49072f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="integrating-text-and-graphics"></a>Integrace textu a obrázků

_Zjistit, jak určit velikost vykreslené textový řetězec textu integrovat SkiaSharp grafiky_

Tento článek ukazuje, jak měřit text, pravděpodobně škálování text na konkrétní velikost a integrovat jiné grafiky text:

![](text-images/textandgraphicsexample.png "Text v obdélníků")

SkiaSharp `Canvas` třída také obsahuje metody pro kreslení obdélníku ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) a obdélník se zaoblenými hranami ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/)). Tyto metody vyžadují rámeček být definován jako `SKRect` hodnotu.

**Rámcová Text** stránky centra krátký textový řetězec na stránce a její rámeček skládá z dvojice zaokrouhlené obdélníků okolí. [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) Třída ukazuje, jak se provádí.

V SkiaSharp použijete `SKPaint` třída sady text a atributy písma, ale můžete ji použít i k získání vykreslené velikost textu. Na začátek následující `PaintSurface` dvě obslužné rutiny události volá jinou `MeasureText` metody. První [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) volání má jednoduchou `string` argument a vrátí šířku pixelů textu na základě aktuálního písma atributů. Program pak vypočítá nový `TextSize` vlastnost `SKPaint` objektu podle této vykreslené šířka aktuální `TextSize` vlastnost a šířku oblasti zobrazení. Slouží k nastavení `TextSize` tak, aby text řetězec k vykreslení na 90 % šířky obrazovky:

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
    SKRect textBounds;
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

Druhý [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/) volání má `SKRect` argument, tak získá šířku i výšku vykreslené textu. `Height` Vlastnost tohoto objektu `SKRect` hodnota závisí na přítomnost velkých písmen, horní a dolní dotahy v textovém řetězci. Různé `Height` hodnoty jsou hlášené pro textové řetězce "mom", "kočka" a "pes", například.

`Left` a `Top` vlastnosti `SKRect` struktura oznámení souřadnice levého horního rohu vykresleného textu, zda text se zobrazí při `DrawText` volání s X a Y pozice 0. Například pokud tento program běží na simulátor pro zařízení iPhone 7 `TextSize` přiřazena hodnota 90.6254 v důsledku výpočtu následující první volání `MeasureText`. `SKRect` Hodnoty získané z druhé volání `MeasureText` má následující hodnoty vlastností:

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

Mějte na paměti, že souřadnice X a Y můžete předat `DrawText` metoda zadejte levé části textu v směrného plánu. `Top` Hodnota znamená, že text rozšiřuje 68 pixelů nad tohoto směrného plánu a (odečtením 68 it z 88) 20 pixelů níže směrného plánu. `Left` Hodnota 6 značí, zda text začíná 6 pixelů pravé hodnoty X ve `DrawText` volání. To umožňuje normální mezery mezi znaky. Pokud chcete zobrazit text správně zapojena v levém horním rohu obrazovky, předat negativy těchto `Left` a `Top` hodnoty jako souřadnice X a Y `DrawText`, v tomto příkladu &ndash;6 a 68.

`SKRect` Struktura definuje několik užitečný vlastnosti a metody, z nichž některé jsou použité ve zbývající části `PaintSurface` obslužné rutiny. `MidX` a `MidY` hodnoty stanovují souřadnice středu rámeček. (V příkladu iPhone 7 tyto hodnoty jsou 338.4107 a &ndash;24.) Následující kód používá tyto hodnoty pro nejjednodušší výpočtu souřadnice na střed text na displeji:

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

`PaintSurface` Obslužná rutina se ukončí pomocí dvou volání `DrawRoundRect`, které vyžadují argumenty `SKRect`. To `SKRect` hodnota je určitě podobná `SKRect` hodnotu ze `MeasureText` metoda, ale nemohou být stejné. První, musí se jednat o něco větší, aby zaoblený obdélník není zakreslit nad okraji textu, a druhé, musí posunutí v prostoru tak, aby `Left` a `Top` hodnoty odpovídají levého horního rohu, kde má být rámeček umístěné. Tyto dvě úlohy se dosahuje prostřednictvím `Offset` a `Inflate` metody definované `SKRect`:

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

Následující, zbývající část metodu je jednoduché. Vytvoří další `SKPaint` objekt pro ohraničení a volání `DrawRoundRect` dvakrát. Druhé volání používá obdélníku zvětšený o jiné 10 pixelů. První volání Určuje poloměr rohu 20 pixelů; druhý má rohu radius 30 pixelů, takže se zdá být paralelní:

 [![](text-images/framedtext-small.png "Trojitá snímek obrazovky stránky s rámečkem Text")](text-images/framedtext-large.png#lightbox "Trojitá snímek obrazovky stránky textu s rámečkem")

Chcete-li telefonu nebo do strany simulátoru zobrazíte text a zvýšit velikost rámce.

Pokud potřebujete center část textu na obrazovce, můžete provést přibližně bez měření text nastavením `TextAlign` vlastnost `SKPaint` k `SKTextAlign.Center`. Souřadnice X, zadejte v `DrawText` metoda pak označuje, kde je umístěn vodorovném centru textu. Pokud předáte středový obrazovky `DrawText` metody text bude vodorovně zarovnaný na střed a *téměř* svisle na střed vzhledem k tomu, že směrného plánu se svisle zarovnaný na střed.

Vlastní text mohou být považovány za mnohem grafické možnost. Jeden jednoduchý možnost je k zobrazení osnovy textových znaků, místo normálních vyplněný zobrazení:

[![](text-images/outlinedtext-small.png "Snímek obrazovky stránky uvedených Text se třemi")](text-images/outlinedtext-large.png#lightbox "Trojitá snímek obrazovky stránky uvedených textu")

To se provádí jednoduše tak, že změna normální `Style` vlastnost `SKPaint` objekt z jeho výchozí nastavení `SKPaintStyle.Fill` k `SKPaintStyle.Stroke` a zadáním šířku tahu. `PaintSurface` Obslužnou rutinu **uvedených Text** stránka zobrazuje, jak se provádí:

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
    SKRect textBounds;
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

 V posledních několika článcích můžete, měli byste nyní viděli, jak pomocí SkiaSharp kreslení textu, kroužky, tři tečky a zaokrouhlené obdélníky. Dalším krokem je [SkiaSharp čar a cesty](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md) ve kterém se dozvíte, jak kreslení čar připojené grafiky cesty.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
