---
title: Nakreslení jednoduchého kruhu v ve Skiasharpu
description: Tento článek vysvětluje základy kreslení ve Skiasharpu, včetně plátna a Malování, v aplikacích Xamarin.Forms a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: e06a1310fad01da11c8d8b115df504cc19426344
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615220"
---
# <a name="drawing-a-simple-circle-in-skiasharp"></a>Nakreslení jednoduchého kruhu v ve Skiasharpu

_Seznamte se se základy kreslení ve Skiasharpu, včetně plátna a Malování_

Tento článek představuje koncepty vykreslování grafiky v Xamarin.Forms používání Skiasharpu, včetně vytváření `SKCanvasView` objektu k hostování zpracování grafiky `PaintSurface` událostí a používání `SKPaint` objekt zadat barvu a další kreslení atributy.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program obsahuje ukázkový kód pro tuto sérii článků ve Skiasharpu. První stránka je oprávněn **jednoduchého kruhu** a vyvolá třídy stránky [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs). Tento kód ukazuje, jak nakreslit kruh uprostřed stránky s protokolem radius 100 pixelů. Přehled kruhu je červená a vnitřní kruhu je modrá.

![](circle-images/circleexample.png "Modrý kruh červeně")

[ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) Je odvozena z třídy stránky `ContentPage` a obsahuje dva `using` direktivy pro obory názvů ve Skiasharpu:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Vytvoří následující konstruktor třídy [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/) objektu, připojí obslužnou rutinu pro [ `PaintSurface` ](https://developer.xamarin.com/api/event/SkiaSharp.Views.Forms.SKCanvasView.PaintSurface/) událostí a nastaví `SKCanvasView` objektu jako obsah stránky:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvasView` Zabírá celou oblast obsahu stránky. Můžete také kombinovat `SKCanvasView` pomocí jiných Xamarin.Forms `View` odvozené konfigurace, jako je zobrazí v jiné příklady.

`PaintSurface` Obslužná rutina události je tam, kde dělají všechny výkresu. Tato metoda je obecně volána více než jednou, zatímco program běží, takže je vhodné ponechat všechny informace nezbytné pro opětovné vytvoření grafiky zobrazení:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[ `SKPaintSurfaceEventArgs` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs/) Objekt, který doprovází událost má dvě vlastnosti:

- [`Info`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info/) typu [`SKImageInfo`](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/)
- [`Surface`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface/) typu [`SKSurface`](https://developer.xamarin.com/api/type/SkiaSharp.SKSurface/)

`SKImageInfo` Struktura obsahuje informace o na návrhovém povrchu, co je nejdůležitější, ale šířka a výška v pixelech. `SKSurface` Objekt představuje na návrhovém povrchu, samotného. V rámci tohoto programu, je na návrhovém povrchu obrazovky, ale v jiných aplikacích `SKSurface` objekt může také představovat rastrový obrázek, který používáte ve Skiasharpu na nakreslit.

Nejdůležitější vlastnost `SKSurface` je [ `Canvas` ](https://developer.xamarin.com/api/property/SkiaSharp.SKSurface.Canvas/) typu [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/). Tato třída je grafika kreslení kontext, který slouží k provádění skutečné kreslení. `SKCanvas` Objekt zapouzdří stav grafiky, včetně transformací grafiky a oříznutí.

Zde je typický začátek `PaintSurface` obslužné rutiny události:

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

[ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) Metoda vymaže canvasu pomocí průhlednou barvu. Přetížení umožňuje zadat barvu pozadí na plátno.

Cílem je nakreslit červené kolečko vyplněna modrou. Protože tento konkrétní obrázek obsahuje dvě různé barvy, úlohy je nutné provést ve dvou krocích. Prvním krokem je nakreslíte obrys kruhu. Chcete-li zadat barvu a další charakteristiky čáry, vytváření a inicializace [ `SKPaint` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaint/) objektu:

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

[ `Style` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Style/) Vlastnost indikuje, že chcete *stroke* řádku (v tomto případě osnovy kruhu) spíše než *výplně* vnitřní. Tří členů [ `SKPaintStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaintStyle/) výčtu jsou následující:

- [`Fill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Fill/)
- [`Stroke`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Stroke/)
- [`StrokeAndFill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.StrokeAndFill/)

Výchozí hodnota je `Fill`. Třetí možnost slouží k obtažení řádku a vyplnění vnitřku stejnou barvu.

Nastavte [ `Color` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Color/) vlastnost na hodnotu typu [ `SKColor` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColor/). Jeden ze způsobů, jak získat `SKColor` hodnotu převedením Xamarin.Forms `Color` hodnota, která se `SKColor` hodnotu pomocí metody rozšíření [ `ToSKColor` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.Extensions.ToSKColor/p/Xamarin.Forms.Color/). [ `Extensions` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.Extensions/) Třídy v `SkiaSharp.Views.Forms` obor názvů obsahuje jiné metody, které provádějí převod mezi Xamarin.Forms a hodnotami ve Skiasharpu.

[ `StrokeWidth` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeWidth/) Vlastnost určuje tloušťku čáry. Tady je nastaveno na 25 pixelů.

Můžete použít `SKPaint` objekt nakreslete kruh:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Souřadnice jsou určen ve vztahu k levého horního rohu zobrazovacím povrchu. Souřadnice X zvýšení napravo a zvýšení Souřadnice Y směrem dolů. V diskuzi o grafice často matematických notation (x, y) se používá k označení bodu. Bod (0, 0) levého horního rohu zobrazovacím povrchu a často volá *původu*.

První dva argumenty `DrawCircle` označení souřadnice X a Y středu kruhu. Ty jsou přiřazeny poloviční šířku a výšku zobrazovacím povrchu umístění střed kruhu ve středu zobrazovacím povrchu. Třetí argument Určuje poloměr kruhu a poslední argument je `SKPaint` objektu.

K vyplnění vnitřku kruhu, je možné změnit dvě vlastnosti `SKPaint` objektu a volání `DrawCircle` znovu. Tento kód také ukazuje alternativní způsob, jak získat `SKColor` hodnotu z jednoho z mnoha pole [ `SKColors` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColors/) struktury:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
Tentokrát `DrawCircle` volání vyplní kruh pomocí nové vlastnosti `SKPaint` objektu.

Tady je program spuštěný v iOS, Android a univerzální platformu Windows:

[![](circle-images/simplecircle-small.png "Trojitá snímek obrazovky stránky jednoduchého kruhu")](circle-images/simplecircle-large.png#lightbox "Trojitá snímek obrazovky stránky jednoduchého kruhu")

Při spuštění programu, můžete zapnout telefon nebo simulátoru otočením naleznete v tématu Jak překreslení grafiky. Pokaždé, když grafika potřebuje překreslit, `PaintSurface` obslužná rutina události je volána znovu.

`SKPaint` Je poněkud složitější než kolekce grafiky vlastnosti objektu. Tyto objekty jsou velmi jednoduché. Můžete znovu použít `SKPaint` objektů se tohoto programu nebo můžete vytvořit více `SKPaint` objektů pro kreslení vlastnosti různé kombinace. Můžete vytvořit a inicializovat tyto objekty mimo `PaintSurface` obslužná rutina události kde si je uložit jako pole ve své třídě stránky.

I když šířka obrysu kruhu je zadán jako 25 pixelů &mdash; nebo jedna čtvrtina poloměr kruhu &mdash; zdá být tenčí a je k tomu dobrý důvod: poloviční šířku čáry je zakrytý modrý kruh. Argumenty, které mají `DrawCircle` metoda definuje abstraktní geometrické souřadnice kruhu. Modré vnitřní přizpůsoben pro daná dimenze na nejbližší pixelů, ale osnovy 25 pixel celou přechází geometrická kruhu &mdash; polovinu na vnitřní a druhá polovina na povrchu.

Další ukázky v [integrace se Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) článku ukazuje to vizuálně.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
