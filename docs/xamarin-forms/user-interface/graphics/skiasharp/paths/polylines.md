---
title: Lomené čáry a parametrické rovnice
description: Tento článek vysvětluje, jak k použití ve Skiasharpu k vykreslení všech řádků můžete definovat pomocí parametrické rovnice a to demonstruje se vzorovým kódem.
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 9118ca8e23e4c4a9023a1add89e26c4484979c8f
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615792"
---
# <a name="polylines-and-parametric-equations"></a>Lomené čáry a parametrické rovnice

_Použít k vykreslení libovolný řádek, který definujete pomocí parametrické rovnice ve Skiasharpu_

V pozdější části tohoto průvodce, zobrazí se vám různé metody, která `SKPath` definuje k vykreslení určité typy křivky. Je však někdy nezbytné k vykreslení typu křivku, která není přímo podporován `SKPath`. V takovém případě můžete použít lomené čáry (kolekce spojené čáry) Chcete-li nakreslit jakékoli křivku, která matematicky můžete definovat. Pokud řádky dostatečně zmenšíte a mnoha dostatek, výsledek bude vypadat křivky. Tato něco jako Spirála je ve skutečnosti 3600 málo řádků:

![](polylines-images/spiralexample.png "Něco jako Spirála")

Obecně je vhodné definovat křivku z hlediska pár parametrické rovnice. Toto jsou rovnice, pro který souřadnice X a Y závisí na třetí proměnné, říká se jim `t` dobu. Například následující parametrické rovnice definovat kruhu s poloměrem 1 zarovnaný na střed v okamžiku (0, 0) pro *t* od 0 do 1:

 x = cos(2πt) y = sin(2πt)

 Pokud chcete, poloměr větší než 1, můžete jednoduše vynásobit hodnoty sinus a kosinus tohoto protokolu radius a pokud potřebujete přesunout střed do jiného umístění, přidejte tyto hodnoty:

 x = xCenter + radius·cos(2πt) y = yCenter + radius·sin(2πt)

Pro elipsu s paralelními osy vodorovné a svislé se podílejí dvě poloměry:

x = xCenter + xRadius·cos(2πt) y = yCenter + yRadius·sin(2πt)

Můžete pak umístit ekvivalentní kód ve Skiasharpu v smyčku, která vypočítá různých fázích a přidá ji do cesty. Následující kód ve Skiasharpu vytvoří `SKPath` objekt pro elipsy, která naplní zobrazovacím povrchu. Smyčka projde 360 stupňů přímo. Centru poloviční šířku a výšku zobrazovacím povrchu a proto jsou dvě poloměry:

```csharp
SKPath path = new SKPath();

for (float angle = 0; angle < 360; angle += 1)
{
    double radians = Math.PI * angle / 180;
    float x = info.Width / 2 + (info.Width / 2) * (float)Math.Cos(radians);
    float y = info.Height / 2 + (info.Height / 2) * (float)Math.Sin(radians);

    if (angle == 0)
    {
        path.MoveTo(x, y);
    }
    else
    {
        path.LineTo(x, y);
    }
}
path.Close();
```

Výsledkem elipsa určené 360 málo řádků. Když je vykresleno, zobrazí se technologie smooth.

Samozřejmě, nemusíte vytvářet elipsa pomocí lomené čáry, protože `SKPath` zahrnuje `AddOval` metodu, která to udělá za vás. Ale můžete chtít nakreslete objekt visual, který není zahrnutý ve `SKPath`.

**Něco jako Spirála Archimedean** stránka obsahuje kód, který podobně jako na tři tečky kód, ale s zásadní rozdíl. Dojde k opakování kolem 360 stupňů kruhu 10krát, nepřetržitě nastavení protokolu radius:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(center.X, center.Y);

    using (SKPath path = new SKPath())
    {
        for (float angle = 0; angle < 3600; angle += 1)
        {
            float scaledRadius = radius * angle / 3600;
            double radians = Math.PI * angle / 180;
            float x = center.X + scaledRadius * (float)Math.Cos(radians);
            float y = center.Y + scaledRadius * (float)Math.Sin(radians);
            SKPoint point = new SKPoint(x, y);

            if (angle == 0)
            {
                path.MoveTo(point);
            }
            else
            {
                path.LineTo(point);
            }
        }

        SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 5
        };

        canvas.DrawPath(path, paint);
    }
}
```

Výsledek se také nazývá *aritmetických něco jako Spirála* vzhledem k tomu, že je konstantní posun mezi každou smyčku:

[![](polylines-images/archimedeanspiral-small.png "Trojitá snímek obrazovky stránky něco jako Spirála Archimedean")](polylines-images/archimedeanspiral-large.png#lightbox "Trojitá snímek obrazovky stránky něco jako Spirála Archimedean")

Všimněte si, že `SKPath` se vytvoří v `using` bloku. To `SKPath` využívá více paměti než `SKPath` objektů v předchozí programy, které navrhuje, které `using` blok je vhodnější uvolnit jakékoli nespravované prostředky.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
