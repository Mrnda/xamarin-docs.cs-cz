---
title: "Výstřižek se cesty a oblasti"
description: "Použití cesty grafiky klip určitých oblastí a vytvářet oblastí"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: charlespetzold
ms.author: chape
ms.date: 06/16/2017
ms.openlocfilehash: bb99984f93f494cfb5ad3d37ccb25f0b91d0b489
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="clipping-with-paths-and-regions"></a>Výstřižek se cesty a oblasti

_Použití cesty grafiky klip určitých oblastí a vytvářet oblastí_

V některých případech je potřeba omezit vykreslování grafiky na určitou oblast. To se označuje jako *výstřižek*. Výstřižek můžete použít pro zvláštní efekty, jako je například tato kopie opic, prostřednictvím klíčové dírky:

![](clipping-images/clippingsample.png "Opic prostřednictvím klíčové dírky")

*Výstřižek oblasti* je oblast na obrazovce, ve kterém jsou grafické vykresleny. Všechno, co se zobrazí mimo oblasti výstřižek není vykreslen. Oblasti výstřižek je obvykle definováno [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) objektu, ale můžete taky definovat výstřižek oblasti pomocí [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) objektu. Tyto dva typy objektů v první zdát související, protože oblast můžete vytvořit z cesty. Však možné vytvořit cestu z oblasti a jsou velmi odlišné interně: cesta se skládá z řady čar a křivek, při oblast je definována řadu vodorovné prohledávání řádků.

Byl vytvořen na obrázku výše **opic prostřednictvím klíčové dírky** stránky. [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) Třída definuje cestu pomocí SVG dat a používá konstruktoru načíst rastrový obrázek z programu prostředků:

```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    SKBitmap bitmap;
    SKPath keyholePath = SKPath.ParseSvgPathData(
        "M 300 130 L 250 350 L 450 350 L 400 130 A 70 70 0 1 0 300 130 Z");

    public MonkeyThroughKeyholePage()
    {
        Title = "Monkey through Keyhole";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

I když `keyholePath` objekt popisuje obrys klíčové dírky, souřadnice jsou zcela libovolný a odrážet, co je vhodné při navrhování data cesty. Z tohoto důvodu `PaintSurface` obslužná rutina získává hranice touto cestou a volání `Translate` a `Scale` přesunout cestu k centru obrazovky a aby téměř výšku jako obrazovky:


```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transform to center and enlarge clip path to window height
        SKRect bounds;
        keyholePath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.98f * info.Height / bounds.Height);
        canvas.Translate(-bounds.MidX, -bounds.MidY);

        // Set the clip path
        canvas.ClipPath(keyholePath);

        // Reset transforms
        canvas.ResetMatrix();

        // Display monkey to fill height of window but maintain aspect ratio
        canvas.DrawBitmap(bitmap,
            new SKRect((info.Width - info.Height) / 2, 0,
                       (info.Width + info.Height) / 2, info.Height));
    }
}
```

Ale cesta není vykreslen. Místo toho následující transformace, cesta slouží k nastavení výstřižek oblasti s Tento příkaz:

```csharp
canvas.ClipPath(keyholePath);
```

`PaintSurface` Obslužná rutina pak obnoví transformace pomocí volání `ResetMatrix` a nevykresluje rastrový obrázek k rozšíření na úplné výšku obrazovky. Tento kód předpokládá, že bitmapy se čtvereček, který má tato konkrétní rastrový obrázek. Bitmapy je vykreslen pouze v oblasti určené cestou výstřižek:

[![](clipping-images/monkeythroughkeyhole-small.png "Trojitá snímek obrazovky opic prostřednictvím stránky klíčové dírky")](clipping-images/monkeythroughkeyhole-large.png#lightbox "Trojitá snímek obrazovky opic prostřednictvím klíčové dírky stránky")

Cesta výstřižek je podmíněno transformace při platit `ClipPath` metoda je volána, a k transformace platit při grafického objektu (například rastrový obrázek) se nezobrazí. Cesta výstřižek je součástí plátno stavu, který je uložený s `Save` metoda a obnovený s `Restore` metoda.

## <a name="combining-clipping-paths"></a>Kombinování ořezové cesty

Přesněji řečeno, oblasti výstřižek není "nastavení" `ClipPath` metoda. Místo toho se zkombinuje s existující cestou výstřižek, který je vytvořen jako obdélníku stejnou velikost jako na obrazovku. Můžete získat obdélníková hranice výstřižek oblasti pomocí [ `ClipBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipBounds/) vlastnost nebo [ `ClipDeviceBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipDeviceBounds/) vlastnost. `ClipBounds` Vlastnost vrátí `SKRect` hodnotu, která odráží všechny transformuje, který může být v platnosti. `ClipDeviceBounds` Vlastnost vrátí `RectI` hodnotu. Toto je obdélníku s dimenzemi v celé číslo a popisuje oblasti výstřižek v skutečné v pixelech.

Žádné volat na `ClipPath` snižuje oblasti výstřižek tím, že propojíte ořezové oblasti s novou oblast. Úplná syntaxe [ `ClipPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipPath/p/SkiaSharp.SKPath/SkiaSharp.SKClipOperation/System.Boolean/) metoda je:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

K dispozici je také [ `ClipRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRect/p/SkiaSharp.SKRect/SkiaSharp.SKClipOperation/System.Boolean/) metoda, která spojuje oblasti výstřižek se obdélníku:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Ve výchozím nastavení, je oblast výsledné výstřižek průnik oblasti existující výstřižek a `SKPath` nebo `SKRect` který je uveden v `ClipPath` nebo `ClipRect` metoda. Tento postup je znázorněn v **čtyři kroužky Intersect klip** stránky. `PaintSurface` Obslužné rutiny v [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) třída opětovně používá stejný `SKPath` objekt, který chcete vytvořit čtyři překrývající se kruhy, z nichž každý snižuje oblasti výstřižek prostřednictvím následná volání `ClipPath`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float size = Math.Min(info.Width, info.Height);
    float radius = 0.4f * size;
    float offset = size / 2 - radius;

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    using (SKPath path = new SKPath())
    {
        path.AddCircle(-offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(-offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColors.Blue;
            canvas.DrawPaint(paint);
        }
    }
}
```

Co je ponechán je průnik tyto čtyři kruhy:

[![](clipping-images//fourcircleintersectclip-small.png "Trojitá snímek obrazovky stránky čtyři kruh Intersect klip")](clipping-images/fourcircleintersectclip-large.png#lightbox "Trojitá snímek obrazovky stránky klip Intersect čtyři kruhu.")

[ `SKClipOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKClipOperation/) Výčet má pouze dva členy:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Difference/) Odebere zadaná cesta nebo obdélníku z existující oblasti výstřižek

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Intersect/) Zadaná cesta nebo obdélníku existující oblasti výstřižek protíná

Pokud nahradíte čtyři `SKClipOperation.Intersect` argumentů `FourCircleIntersectClipPage` třídy s `SKClipOperation.Difference`, zobrazí se následující:

[![](clipping-images//fourcircledifferenceclip-small.png "Trojitá snímek obrazovky stránky čtyři kruh Intersect klip rozdíl operace")](clipping-images/fourcircledifferenceclip-large.png#lightbox "Trojitá snímek obrazovky stránky čtyři kruh Intersect klip rozdíl operace")

Čtyři kruhy překrývajících se byly odebrány z oblasti výstřižek.

**Klip Operations** stránky ukazuje rozdíl mezi těmito dvěma operacemi s jenom pár kroužky. V prvním kruhu na levé straně je přidáno do oblasti výstřižek s provozem klip výchozí `Intersect`, zatímco druhý kruhu na pravé straně je přidáno do oblasti výstřižek klip operace indikován textový popisek:

[![](clipping-images//clipoperations-small.png "Trojitá snímek obrazovky stránky klip operace")](clipping-images/clipoperations-large.png#lightbox "Trojitá snímek obrazovky stránky klip operace")

[ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs) Třída definuje dvě `SKPaint` objektů jako pole a pak rozdělí na obrazovce na dvě obdélníková oblasti. Tyto oblasti se liší v závislosti na tom, jestli je telefonu v režimu na výšku nebo na šířku. `DisplayClipOp` Třída pak zobrazí text a volání `ClipPath` s dvě kruh cesty k objasnění každou operaci klip:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;

    foreach (SKClipOperation clipOp in Enum.GetValues(typeof(SKClipOperation)))
    {
        // Portrait mode
        if (info.Height > info.Width)
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width, y + info.Height / 2), clipOp);
            y += info.Height / 2;
        }
        // Landscape mode
        else
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width / 2, y + info.Height), clipOp);
            x += info.Width / 2;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKClipOperation clipOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(clipOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    canvas.Save();

    using (SKPath path1 = new SKPath())
    {
        path1.AddCircle(xCenter - radius / 2, yCenter, radius);
        canvas.ClipPath(path1);

        using (SKPath path2 = new SKPath())
        {
            path2.AddCircle(xCenter + radius / 2, yCenter, radius);
            canvas.ClipPath(path2, clipOp);

            canvas.DrawPaint(fillPaint);
        }
    }

    canvas.Restore();
}
```

Volání metody `DrawPaint` celé plátno pro vyplnění pomocí které obvykle způsobí, že `SKPaint` objektu, ale v takovém případě metodu právě vybarví v oblasti výstřižek.

## <a name="exploring-regions"></a>Zkoumání oblastí

Pokud jste prozkoumali dokumentaci rozhraní API pro `SKCanvas`, budete možná jste si všimli přetížení `ClipPath` a `ClipRect` metody, které jsou podobné metod popsaných výše, ale místo toho mají parametr s názvem [ `SKRegionOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegionOperation/) místo `SKClipOperation`. `SKRegionOperation` má šest členů, poskytuje trochu větší flexibilitu při kombinování cesty do formuláře výstřižek oblastí:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Difference/)

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Intersect/)

- [`Union`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Union/)

- [`XOR`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.XOR/)

- [`ReverseDifference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.ReverseDifference/)

- [`Replace`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Replace/)

Ale přetížení `ClipPath` a `ClipRect` s `SKRegionOperation` parametry jsou zastaralé a nelze jej použít.

Můžete dál používat `SKRegionOperation` výčtu, ale vyžaduje definování oblasti výstřižek z hlediska [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) objektu.

Nově vytvořená `SKRegion` objekt popisuje na prázdnou oblast. Obvykle je první volání objektu [ `SetRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetRect/p/SkiaSharp.SKRectI/) tak, aby oblast popisují obdélníkovou oblast. Parametr pro `SetRect` je `SKRectI` hodnotu & #x 2014; hodnota obdélníku s vlastnostmi celé číslo. Potom můžete volat [ `SetPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetPath/p/SkiaSharp.SKPath/SkiaSharp.SKRegion/) s `SKPath` objektu. Tím se vytvoří oblasti, která je stejná jako vnitřní cesty, ale oříznuto počáteční obdélníkovou oblast.

`SKRegionOperation` Výčtu pochází jenom do play při volání jednoho z [ `Op` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.Op/p/SkiaSharp.SKRegion/SkiaSharp.SKRegionOperation/) přetížení metody, jako je tato:

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

Oblast, kterou vytvoříte `Op` volání je zkombinován s oblasti zadána jako parametr na základě `SKRegionOperation` člen. Když nakonec získáte oblast vhodný pro výstřižek, můžete, nastavit jako oblasti výstřižek na plátno pomocí [ `ClipRegion` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRegion/p/SkiaSharp.SKRegion/SkiaSharp.SKClipOperation/) metodu `SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

Následující snímek obrazovky ukazuje výstřižek oblasti podle činnosti šesti oblast. Levé kroužek je oblast, `Op` metoda je volána na a pravém kroužek je oblast předaný `Op` metoda:

[![](clipping-images//regionoperations-small.png "Trojitá snímek obrazovky stránky oblast operace")](clipping-images/regionoperations-large.png#lightbox "Trojitá snímek obrazovky stránky operace oblast")

Jsou tyto všechny možnosti kombinace těchto dvou kroužky? Vezměte v úvahu bitovou kopii výsledné jako kombinace tří součástí, která jsou zobrazená v samotné `Difference`, `Intersect`, a `ReverseDifference` operace. Celkový počet kombinací je dva třetí mocninu nebo osm. Jsou dva, které chybí oblasti původní (který výsledkem není volání `Op` vůbec) a oblast zcela prázdný.

Je těžší používat oblasti pro výstřižek, protože je třeba nejprve vytvořit cestu a poté v oblasti z této cestě a kombinovat více oblastí. Celková struktura **oblast Operations** stránka je velmi podobné **klip Operations** ale [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) třída rozděluje obrazovky do šesti oblastí a Zobrazuje navíc práce potřebné pro tuto úlohu použít oblastí:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;
    float width = info.Height > info.Width ? info.Width / 2 : info.Width / 3;
    float height = info.Height > info.Width ? info.Height / 3 : info.Height / 2;

    foreach (SKRegionOperation regionOp in Enum.GetValues(typeof(SKRegionOperation)))
    {
        DisplayClipOp(canvas, new SKRect(x, y, x + width, y + height), regionOp);

        if ((x += width) >= info.Width)
        {
            x = 0;
            y += height;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKRegionOperation regionOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(regionOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    SKRectI recti = new SKRectI((int)rect.Left, (int)rect.Top,
                                (int)rect.Right, (int)rect.Bottom);

    using (SKRegion wholeRectRegion = new SKRegion())
    {
        wholeRectRegion.SetRect(recti);

        using (SKRegion region1 = new SKRegion(wholeRectRegion))
        using (SKRegion region2 = new SKRegion(wholeRectRegion))
        {
            using (SKPath path1 = new SKPath())
            {
                path1.AddCircle(xCenter - radius / 2, yCenter, radius);
                region1.SetPath(path1);
            }

            using (SKPath path2 = new SKPath())
            {
                path2.AddCircle(xCenter + radius / 2, yCenter, radius);
                region2.SetPath(path2);
            }

            region1.Op(region2, regionOp);

            canvas.Save();
            canvas.ClipRegion(region1);
            canvas.DrawPaint(fillPaint);
            canvas.Restore();
        }
    }
}
```

Tady je velký rozdíl mezi `ClipPath` metoda a `ClipRegion` metoda:

> [!IMPORTANT]
> Na rozdíl od `ClipPath` metody `ClipRegion` – metoda neovlivňuje transformací.

Pro pochopení důvody tohoto rozdílu, je vhodné se seznámit s jaké oblasti je. Pokud jste si myslíte o tom, jak klip operace nebo oblasti provozu může implementovat interně, se pravděpodobně zdá být velmi složité. Několik potenciálně velmi složité cesty se spojují se a obrys výsledné cesty je pravděpodobně algoritmické při důvodů.

Ale tato úloha je výrazně jednodušší, pokud každá cesta je snížen na řadu řádky vodorovné kontroly, jako jsou ty stejné vakuu zkumavky televizní přijímače. Každý řádek kontroly je jednoduše na vodorovném řádku s bod počáteční a koncový bod. Například můžete kruh se serverem radius 10 rozložit na 20 vodorovné kontroly řádky, z nichž každá začíná v levé části na kruh a končí na pravé straně. Kombinování dva kruhy s všechny operace oblast stane velmi jednoduchý, protože se jedná pouze záležitost zkoumání počáteční a koncové souřadnice každý pár odpovídající prohledávání řádků.

To znamená, co je oblast: série vodorovné prohledávání řádků, která definuje oblast.

Ale když je oblast snížen na řadu prohledávání řádků, tyto kontroly, které řádky jsou založeny na dimenzi pixelu. Oblast přesněji řečeno, není objekt vektorové grafiky. Je blíže ve své podstatě komprimované černobílý bitmapy než na cestu. Oblasti v důsledku toho nelze škálovat nebo otáčet bez ztráty věrnosti a z toho důvodu, že nejsou transformovat při použití pro výstřižek oblasti.

Můžete však použít transformací oblasti pro účely Malování. **Oblast Malování** program vividly ukazuje vnitřní povaha oblasti. [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) Třída vytvoří `SKRegion` na základě objektu `SKPath` kruhu 10 jednotky radius. Transformace potom rozšíří tohoto kroužku k zaplnění stránky:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int radius = 10;

    // Create circular path
    using (SKPath circlePath = new SKPath())
    {
        circlePath.AddCircle(0, 0, radius);

        // Create circular region
        using (SKRegion circleRegion = new SKRegion())
        {
            circleRegion.SetRect(new SKRectI(-radius, -radius, radius, radius));
            circleRegion.SetPath(circlePath);

            // Set transform to move it to center and scale up
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(Math.Min(info.Width / 2, info.Height / 2) / radius);

            // Fill region
            using (SKPaint fillPaint = new SKPaint())
            {
                fillPaint.Style = SKPaintStyle.Fill;
                fillPaint.Color = SKColors.Orange;

                canvas.DrawRegion(circleRegion, fillPaint);
            }

            // Stroke path for comparison
            using (SKPaint strokePaint = new SKPaint())
            {
                strokePaint.Style = SKPaintStyle.Stroke;
                strokePaint.Color = SKColors.Blue;
                strokePaint.StrokeWidth = 0.1f;

                canvas.DrawPath(circlePath, strokePaint);
            }
        }
    }
}
```

`DrawRegion` Volání vyplní celé oblasti oranžovou barvu, když `DrawPath` volání tahy původní cesta modře pro porovnání:

[![](clipping-images//regionpaint-small.png "Trojitá snímek obrazovky stránky oblast Malování")](clipping-images/regionpaint-large.png#lightbox "Trojitá snímek obrazovky stránky oblast Malování")

Oblast je jasně řadu diskrétní souřadnice.

Pokud nemusíte používat transformací souvislosti s vaší oblasti výstřižek, můžete použít oblasti pro výstřižek, jako **čtyři – listu jetel** ukazuje stránky. [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) – Třída vytvoří složené oblast z čtyři cyklické oblastí, nastaví tento složený oblasti jako oblasti výstřižek a pak nevykresluje řadu 360 přímky ze středu stránky:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float xCenter = info.Width / 2;
    float yCenter = info.Height / 2;
    float radius = 0.24f * Math.Min(info.Width, info.Height);

    using (SKRegion wholeScreenRegion = new SKRegion())
    {
        wholeScreenRegion.SetRect(new SKRectI(0, 0, info.Width, info.Height));

        using (SKRegion leftRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion rightRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion topRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion bottomRegion = new SKRegion(wholeScreenRegion))
        {
            using (SKPath circlePath = new SKPath())
            {
                // Make basic circle path
                circlePath.AddCircle(xCenter, yCenter, radius);

                // Left leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, 0));
                leftRegion.SetPath(circlePath);

                // Right leaf
                circlePath.Transform(SKMatrix.MakeTranslation(2 * radius, 0));
                rightRegion.SetPath(circlePath);

                // Make union of right with left
                leftRegion.Op(rightRegion, SKRegionOperation.Union);

                // Top leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, -radius));
                topRegion.SetPath(circlePath);

                // Combine with bottom leaf
                circlePath.Transform(SKMatrix.MakeTranslation(0, 2 * radius));
                bottomRegion.SetPath(circlePath);

                // Make union of top with bottom
                bottomRegion.Op(topRegion, SKRegionOperation.Union);

                // Exclusive-OR left and right with top and bottom
                leftRegion.Op(bottomRegion, SKRegionOperation.XOR);

                // Set that as clip region
                canvas.ClipRegion(leftRegion);

                // Set transform for drawing lines from center
                canvas.Translate(xCenter, yCenter);

                // Draw 360 lines
                for (double angle = 0; angle < 360; angle++)
                {
                    float x = 2 * radius * (float)Math.Cos(Math.PI * angle / 180);
                    float y = 2 * radius * (float)Math.Sin(Math.PI * angle / 180);

                    using (SKPaint strokePaint = new SKPaint())
                    {
                        strokePaint.Color = SKColors.Green;
                        strokePaint.StrokeWidth = 2;

                        canvas.DrawLine(0, 0, x, y, strokePaint);
                    }
                }
            }
        }
    }
}
```

Je skutečně nevypadá čtyři – listu jetel, ale je obrázek, který jinak může být obtížné vykreslení bez výstřižek:

[![](clipping-images//fourleafclover-small.png "Trojitá snímek obrazovky stránky čtyři – listu jetel")](clipping-images/fourleafclover-large.png#lightbox "Trojitá snímek obrazovky stránky čtyři – listu jetel")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
