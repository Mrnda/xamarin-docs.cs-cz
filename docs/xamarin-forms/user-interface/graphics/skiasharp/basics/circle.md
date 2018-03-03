---
title: "Kreslení kruh jednoduché"
description: "Seznámíte se základy kreslení SkiaSharp, včetně plátna a Malování"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 63314efdd29c8da0273459de2d12f7b807968a04
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="drawing-a-simple-circle"></a>Kreslení kruh jednoduché

_Seznámíte se základy kreslení SkiaSharp, včetně plátna a Malování_

Tento článek představuje koncepty kreslení grafiky v Xamarin.Forms pomocí SkiaSharp, včetně vytváření `SKCanvasView` objektu k hostování zpracování grafiky `PaintSurface` událostí a používání `SKPaint` objekt, který chcete zadat barvy a další kreslení atributy.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) program obsahuje ukázkový kód pro tuto řadu SkiaSharp články. Nárok na první stránku **Jednoduchý kruh** a vyvolá třídy stránky [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs). Tento kód ukazuje, jak k vykreslení kruh v centru stránky se serverem radius 100 pixelů. Obrys kruhu je red a je blue vnitřek kruhu.

![](circle-images/circleexample.png "Modré kruh označeno červeně")

[ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) Stránky třída odvozená z `ContentPage` a obsahuje dva `using` direktivy pro obory názvů SkiaSharp:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Vytvoří následující konstruktor třídy [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/) objektu, připojí obslužnou rutinu pro [ `PaintSurface` ](https://developer.xamarin.com/api/event/SkiaSharp.Views.Forms.SKCanvasView.PaintSurface/) událostí a nastaví `SKCanvasView` objektu jako obsahu stránky:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvasView` Zabírá celou oblast obsahu stránky. Případně můžete kombinovat `SKCanvasView` s další Xamarin.Forms `View` odvozené konfigurace, jako je najdete v další příklady.

`PaintSurface` Obslužné rutiny události je, kde můžete udělat všechny kreslení. Tato metoda je obecně volat vícekrát, když váš program běží, tak ho měli spravovat všechny informace nezbytné k opětovnému vytvoření grafiky zobrazit:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[ `SKPaintSurfaceEventArgs` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs/) Objekt, který doprovází událost má dvě vlastnosti:

- [`Info`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info/) typu [`SKImageInfo`](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/)
- [`Surface`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface/) typu [`SKSurface`](https://developer.xamarin.com/api/type/SkiaSharp.SKSurface/)

`SKImageInfo` Struktura obsahuje informace o kreslicí plochy, co je nejdůležitější, bude šířka a výška v pixelech. `SKSurface` Objekt představuje kreslicí plochy sám sebe. V tomto programu kreslicí plochy je video zobrazení, ale v ostatních aplikacích `SKSurface` objekt může také představovat rastrový obrázek, který používáte k vykreslení SkiaSharp.

Vlastnost nejdůležitější `SKSurface` je [ `Canvas` ](https://developer.xamarin.com/api/property/SkiaSharp.SKSurface.Canvas/) typu [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/). Tato třída je grafiky, kreslení kontext, který slouží k provádění skutečné kreslení. `SKCanvas` Objekt zapouzdří stav grafiky, který obsahuje transformace grafiky a oříznutí.

Zde je typické začátek `PaintSurface` obslužné rutiny události:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();
    ...
}

```

[ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) Metoda vymaže na plátno transparentní barvou. Přetížení umožňuje zadat barvu pozadí na plátno.

Cílem je k vykreslení červené kolečko vyplněnou blue. Protože tato konkrétní grafické bitová kopie obsahuje dvě různé barvy, úlohy je nutné provést ve dvou krocích. Prvním krokem je k vykreslení obrys kruhu. K určení, barvu a jiné vlastnosti na řádku, vytvoření a inicializace [ `SKPaint` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaint/) objektu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 25
    };
    ...
}
```

[ `Style` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Style/) Vlastnost znamená, že chcete *tahu* řádku (v tomto případě obrys kruhu) místo *výplně* vnitřek. Tři členy [ `SKPaintStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaintStyle/) výčtu jsou následující:

- [`Fill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Fill/)
- [`Stroke`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Stroke/)
- [`StrokeAndFill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.StrokeAndFill/)

Výchozí hodnota je `Fill`. Třetí možnost slouží k obtažení řádku a vyplnění vnitřku stejné barvy.

Nastavte [ `Color` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Color/) vlastnost na hodnotu typu [ `SKColor` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColor/). Jeden ze způsobů, jak získat `SKColor` hodnota je převod platformě Xamarin.Forms `Color` hodnotu `SKColor` hodnotu pomocí metody rozšíření [ `ToSKColor` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.Extensions.ToSKColor/p/Xamarin.Forms.Color/). [ `Extensions` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.Extensions/) Třídy v `SkiaSharp.Views.Forms` obor názvů zahrnuje další metody, které převést mezi Xamarin.Forms a SkiaSharp hodnotami.

[ `StrokeWidth` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeWidth/) Vlastnost určuje tloušťku čáry. Zde je nastavena na 25 pixelů.

Můžete použít `SKPaint` objektu k vykreslení kruhu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Vzhledem k levého horního rohu zobrazení plochy nejsou zadány souřadnice. X koordinuje zvyšte napravo a zvýšení Souřadnice Y směrem dolů. V diskusi o grafice často matematické zápis (x, y) se používá k označení bod. Bod (0, 0) je levém horním rohu zobrazení plochy a se často říká *původu*.

První dva argumenty `DrawCircle` znamenat souřadnice X a Y středu kruhu. Tyto jsou přiřazeny poloviční šířky a výšky zobrazení plochy uvést střed kruhu ve středu zobrazení prostor. Třetí argument určuje protokolu radius na kruh a poslední argument je `SKPaint` objektu.

K vyplnění vnitřku kruhu, můžete změnit dvě vlastnosti `SKPaint` objekt a volání `DrawCircle` znovu. Tento kód také ukazuje alternativní způsob získání `SKColor` hodnotu z jednoho z mnoha pole [ `SKColors` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColors/) strukturu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
Tentokrát `DrawCircle` volání doplní kruhu pomocí nové vlastnosti `SKPaint` objektu.

Tady je program spuštěný v iOS, Android a univerzální platformu Windows:

[![](circle-images/simplecircle-small.png "Trojitá snímek obrazovky stránky Jednoduchý kruh")](circle-images/simplecircle-large.png "Trojitá snímek obrazovky stránky jednoduché kruhu.")

Při spuštění programu, můžete zapnout telefon nebo simulátoru ze strany najdete v části jak bude překreslen na obrázku. Pokaždé, když na obrázku musí být překreslen, `PaintSurface` obslužné rutiny události je volán znovu.

`SKPaint` Objekt je trochu déle, než kolekce grafiky kreslení vlastnosti. Tyto objekty jsou velmi jednoduché. Můžete opakovaně použít `SKPaint` objekty nemá tohoto programu, nebo můžete vytvořit více `SKPaint` objekty pro různé kombinace kreslení vlastnosti. Můžete vytvořit a inicializaci tyto objekty mimo `PaintSurface` obslužná rutina události a je uložit jako pole ve třídě stránky.

Šířka obrysu na kruh se sice zadává jako 25 pixelů & #x 2014; nebo jeden čtvrtletí úhlu kruh & #x 2014; zdá být užší a je dobré důvod k tomu: poloviční šířky řádku je zakrytý modrý kruh. Argumenty, které mají `DrawCircle` metoda definovat abstraktní geometrickou souřadnice kruh. Modré interior velikost tak, aby daná dimenze na nejbližší pixelů, ale obrys 25. pixelů celou přechází geometrickou kruh & #x 2014; polovinu na uvnitř a polovina na povrchu.

Další vzorek v [integrace s Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) vizuálně článek to ukazuje.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
