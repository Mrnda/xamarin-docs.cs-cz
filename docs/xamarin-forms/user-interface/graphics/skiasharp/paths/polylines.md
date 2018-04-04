---
title: Čáru lomených a čištění vzorce
description: Použití SkiaSharp k vykreslení kterýkoli řádek, který můžete definovat s čištění vzorce
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: efd2dbac0f4a1190fac646d8e9e3120ee4d245a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="polylines-and-parametric-equations"></a>Čáru lomených a čištění vzorce

_Použití SkiaSharp k vykreslení kterýkoli řádek, který můžete definovat s čištění vzorce_

V pozdější části této příručky, zobrazí se různé metody, `SKPath` definuje k vykreslení určité typy křivek. Je však někdy potřebné k vykreslení typu křivky, který nepodporuje přímo `SKPath`. V takovém případě můžete k vykreslení žádné křivky, který matematicky můžete definovat čáru (kolekce připojené řádky). Pokud provedete řádky dostatečně malé, a množství dostatek výsledek bude vypadat křivka. Tato Spirála je ve skutečnosti 3 600 málo řádky:

![](polylines-images/spiralexample.png "Spirály")

Obvykle je nejvhodnější definovat křivky z hlediska pár čištění vzorce. Toto jsou vzorce, pro který souřadnice X a Y závisí na třetí proměnné, někdy označuje jako `t` dobu. Můžete například definovat následující čištění vzorce kruh se serverem radius 1 zarovnaný na střed v bodě (0, 0) pro *t* od 0 do 1:

 x = cos(2πt) y = sin(2πt)

 Pokud chcete radius větší než 1, můžete jednoduše vynásobit hodnoty sinus a kosinus tohoto protokolu radius a pokud potřebujete přesunout centru do jiného umístění, přidejte tyto hodnoty:

 x = xCenter + radius·cos(2πt) y = yCenter + radius·sin(2πt)

Pro elipsy s paralelní osy ve vodorovném a svislého se jedná o dvě poloměr:

x = xCenter + xRadius·cos(2πt) y = yCenter + yRadius·sin(2πt)

Pak můžete umístit kód ekvivalentní SkiaSharp ve smyčce, která vypočítá různé body a přidá do cestu. Následující kód SkiaSharp vytvoří `SKPath` objekt pro elipsy, který vyplní povrch zobrazení. Smyčky cyklů přímo do 360 stupňů. Centru je poloviční šířky a výšky zobrazení plochy, a proto jsou dvě poloměr:

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

Výsledkem elipsy definované 360 málo řádky. Je vykreslen, zobrazí se smooth.

Samozřejmě, nemusíte vytvářet elipsy pomocí lomenou čáru, protože `SKPath` zahrnuje `AddOval` metoda, která provede za vás. Ale můžete chtít kreslení visual objekt, který není poskytované `SKPath`.

**Archimedean Spirála** stránka obsahuje kód, který podobné kód elipsy, ale s velmi důležitý rozdíl. Dojde k opakování kolem 360 stupňů kruhu 10krát, nepřetržitě úpravě poloměr:

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

Výsledek se také nazývá *aritmetické Spirála* protože posun mezi každou smyčku je konstantní:

[![](polylines-images/archimedeanspiral-small.png "Trojitá snímek obrazovky stránky Spirála Archimedean")](polylines-images/archimedeanspiral-large.png#lightbox "Trojitá snímek obrazovky stránky Archimedean Spirála")

Všimněte si, že `SKPath` je vytvořen v `using` bloku. To `SKPath` spotřebovává více paměti, než `SKPath` objekty v předchozí programy, které naznačuje, které `using` bloku je vhodnější, aby uvolnila veškeré nespravované prostředky.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
