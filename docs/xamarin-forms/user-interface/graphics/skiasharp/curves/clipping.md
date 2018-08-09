---
title: Ořezy cestami a oblastmi
description: Tento článek vysvětluje, jak pomocí cesty ve Skiasharpu na grafiku klip určité oblasti a vytvořit oblasti a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: charlespetzold
ms.author: chape
ms.date: 06/16/2017
ms.openlocfilehash: 0c07d68535349004eeefeaa18daa9c59b889a6a7
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615285"
---
# <a name="clipping-with-paths-and-regions"></a>Ořezy cestami a oblastmi

_Použít cesty k klip grafiky na určité oblasti a k vytváření oblastí_

Někdy je potřeba omezit vykreslování grafiky na konkrétní oblasti. To se označuje jako *výstřižek*. Výstřižek můžete použít pro speciální efekty, jako je například tento obrázek opic zobrazené prostřednictvím klíčové dírky:

![](clipping-images/clippingsample.png "Opic prostřednictvím klíčové dírky")

*Výstřižek oblasti* je oblast na obrazovce, ve kterém jsou generovány grafiky. Cokoli, co se zobrazí mimo oblasti výstřižek není vykresleno. Oblasti výstřižek většinou definovaný pomocí [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) objekt, ale můžete také definovat oblasti výstřižek pomocí [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) objektu. Tyto dva typy objektů v první zdát související, protože oblast můžete vytvořit pomocí cesty. Ale možné vytvořit cestu z oblasti, a jsou velmi odlišné interně: cesta se skládá z řady čar a křivek, zatímco oblast je definován pomocí posloupnosti řádků vodorovné kontroly.

Byl vytvořen na obrázku výše **opic prostřednictvím klíčové dírky** stránky. [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) Třída definuje cestu pomocí SVG dat a používá konstruktor k načtení rastrový obrázek z programu prostředky:

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
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

I když `keyholePath` osnovy klíčové dírky popisuje objekt, souřadnice jsou zcela libovolného a odrážejí, co bylo vhodné při navrhování data cesty. Z tohoto důvodu `PaintSurface` obslužná rutina získá hranice touto cestou a volání `Translate` a `Scale` přesunout cestu do středu obrazovky a aby bylo téměř vysoké jako na obrazovce:


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

Ale není vykreslen cestu. Místo toho následující transformace, cesta slouží k nastavení oblasti výstřižek pomocí tohoto příkazu:

```csharp
canvas.ClipPath(keyholePath);
```

`PaintSurface` Obslužná rutina potom resetuje transformace voláním `ResetMatrix` a kreslí rastrového obrázku můžete rozšířit do plnou výškou na obrazovce. Tento kód předpokládá, že rastrový obrázek čtvereček, což je tento konkrétní rastrový obrázek. Rastrový obrázek je vykreslen pouze v rámci oblasti určené ořezové cesty:

[![](clipping-images/monkeythroughkeyhole-small.png "Trojitá snímek opic prostřednictvím stránky klíčové dírky")](clipping-images/monkeythroughkeyhole-large.png#lightbox "Trojitá snímek opic prostřednictvím klíčové dírky stránky")

Ořezové cesty se vztahuje transformace při platit `ClipPath` metoda je volána, a pro transformace v platnosti Pokud grafický objekt (například rastrový obrázek) nezobrazuje. Ořezové cesty je součástí plátna stav, který je uložen s `Save` metoda a obnovit pomocí `Restore` metoda.

## <a name="combining-clipping-paths"></a>Kombinování ořezové cesty

Přesněji řečeno, oblasti výstřižek není "set" `ClipPath` metody. Místo toho se zkombinuje s existující ořezovou cestu, která začíná jako obdélník rovná velikosti obrazovky. Můžete získat pomocí výstřižek oblasti obdélníkový hranice [ `ClipBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipBounds/) vlastnost nebo [ `ClipDeviceBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipDeviceBounds/) vlastnost. `ClipBounds` Vrátí vlastnost `SKRect` hodnotu, která odpovídá některé transformace, která může být v platnosti. `ClipDeviceBounds` Vrátí vlastnost `RectI` hodnotu. Toto je obdélníku s dimenzemi celé číslo a popisuje oblasti výstřižek aplikace skutečný v pixelech.

Žádné volání `ClipPath` snižuje oblasti výstřižek kombinací výstřižek oblasti s novou oblast. Úplná syntaxe [ `ClipPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipPath/p/SkiaSharp.SKPath/SkiaSharp.SKClipOperation/System.Boolean/) metoda je:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

K dispozici je také [ `ClipRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRect/p/SkiaSharp.SKRect/SkiaSharp.SKClipOperation/System.Boolean/) metodu, která kombinuje s obdélník oříznutí:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Ve výchozím nastavení, výsledná výstřižek oblasti jsou průsečíkem existující oblasti výstřižek a `SKPath` nebo `SKRect` , který je uveden v `ClipPath` nebo `ClipRect` metody. To je patrné **čtyři kruhy Intersect klip** stránky. `PaintSurface` Obslužné rutiny v [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) třídy opětovně používá stejný `SKPath` objektu, který chcete vytvořit čtyři překrývající se kruhy, z nichž každý snižuje oblasti výstřižek prostřednictvím následná volání `ClipPath`:

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

Co je ponecháno je průsečík tyto čtyři kruhy:

[![](clipping-images//fourcircleintersectclip-small.png "Trojitá snímek obrazovky stránky čtyři kruh Intersect klip")](clipping-images/fourcircleintersectclip-large.png#lightbox "Trojitá snímek obrazovky stránky klip Intersect čtyři kruh")

[ `SKClipOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKClipOperation/) Výčet má jenom dva členy:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Difference/) Odebere zadanou cestu nebo obdélník z existující oblasti oříznutí

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Intersect/) Zadaná cesta nebo obdélníku s existující oblasti výstřižek protíná

Pokud nahradíte čtyři `SKClipOperation.Intersect` argumentů `FourCircleIntersectClipPage` třídy s `SKClipOperation.Difference`, zobrazí se následující:

[![](clipping-images//fourcircledifferenceclip-small.png "Trojitá snímek obrazovky stránky klip Intersect čtyři kroužek s operace zjišťování rozdílů")](clipping-images/fourcircledifferenceclip-large.png#lightbox "Trojitá snímek obrazovky stránky klip Intersect čtyři kroužek s operace zjišťování rozdílů")

Čtyři překrývající se kruhy byly odebrány z oblasti oříznutí.

**Klip operace** stránce ukazuje rozdíl mezi tyto dvě operace pomocí jenom pár kruzích. První kruh vlevo je přidán do oblasti výstřižek s provozem klip výchozí `Intersect`, zatímco ve druhém kruhu na pravé straně se přidá do oblasti výstřižek s klip operace označený text popisku:

[![](clipping-images//clipoperations-small.png "Trojitá snímek obrazovky stránky klip operace")](clipping-images/clipoperations-large.png#lightbox "Trojitá snímek obrazovky stránky klip operace")

[ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs) Třída definuje dvě `SKPaint` objekty jako pole a potom vyfiltruje obrazovky na dvě oblasti obdélníkový. Tyto oblasti se liší v závislosti na tom, jestli je v režimu na výšku nebo šířku telefon. `DisplayClipOp` Třídy zobrazí text a volání `ClipPath` s cestami dvě kruh pro ilustraci každou operaci klipu:

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

Volání `DrawPaint` celé plátno tankujeme, které obvykle způsobí, že `SKPaint` objektu, ale v tomto případě metodu právě jsou vykreslovány v rámci oblasti oříznutí.

## <a name="exploring-regions"></a>Zkoumání oblastí

Pokud zkoumáte dokumentaci k rozhraní API pro `SKCanvas`, možná jste si všimli přetížení `ClipPath` a `ClipRect` metody, které jsou podobné metodám, které je popsáno výše, ale místo toho máte parametr s názvem [ `SKRegionOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegionOperation/) spíše než `SKClipOperation`. `SKRegionOperation` má šest členy, poskytují poněkud vyšší flexibilitu ve spojení cest k oblasti formuláře výstřižek:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Difference/)

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Intersect/)

- [`Union`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Union/)

- [`XOR`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.XOR/)

- [`ReverseDifference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.ReverseDifference/)

- [`Replace`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Replace/)

Ale přetížení `ClipPath` a `ClipRect` s `SKRegionOperation` parametry jsou zastaralé a nelze jej použít.

Můžete dál používat `SKRegionOperation` výčtu, ale vyžaduje definování oblasti omezení z hlediska [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) objektu.

Nově vytvořená `SKRegion` popisuje objekt, na prázdnou oblast. První volání objektu je obvykle [ `SetRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetRect/p/SkiaSharp.SKRectI/) tak, aby v oblasti popisu obdélníkovou oblast. Parametr `SetRect` je `SKRectI` hodnotu &mdash; hodnota obdélníku s vlastnostmi celé číslo. Potom můžete zavolat [ `SetPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetPath/p/SkiaSharp.SKPath/SkiaSharp.SKRegion/) s `SKPath` objektu. Tím se vytvoří oblast, která je stejná jako vnitřní cesty, ale ořízne počáteční obdélníkovou oblast.

`SKRegionOperation` Výčet pochází jenom do hry při volání [ `Op` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.Op/p/SkiaSharp.SKRegion/SkiaSharp.SKRegionOperation/) přetížení metody, jako je například tento:

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

Oblasti, kterou děláte `Op` volání na číslo zkombinuje s zadanou jako parametr na základě oblast `SKRegionOperation` člena. Pokud nakonec získáte oblast vhodný pro oříznutí, použijete, který nastavit jako výstřižek oblast plátna pomocí [ `ClipRegion` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRegion/p/SkiaSharp.SKRegion/SkiaSharp.SKClipOperation/) metoda `SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

Následující snímek obrazovky ukazuje omezení oblasti podle šesti oblastech operací. Kruh vlevo je oblast, která `Op` metoda je volána na a správné kruhu je oblast, předán `Op` metoda:

[![](clipping-images//regionoperations-small.png "Trojitá snímek obrazovky stránky oblasti operace")](clipping-images/regionoperations-large.png#lightbox "Trojitá snímek obrazovky stránky oblasti operace")

Jsou tyto všechny možnosti kombinace těchto dvou kruzích? Vezměte v úvahu výsledná image jako kombinace tři komponenty, které samy o sobě v `Difference`, `Intersect`, a `ReverseDifference` operace. Celkový počet kombinací je dva třetí mocninu nebo 8. Jsou dva, které chybí původní oblast (který výsledkem volání není `Op` vůbec) a zcela prázdnou oblast.

Použití oblastí pro oříznutí, protože je potřeba nejprve vytvořit cestu a potom v oblasti z této cesty a pak sloučí více oblastí je obtížnější. Celkovou strukturu **oblasti operace** stránka je velmi podobný **klip operace** ale [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) třídy vyfiltruje obrazovky do šesti oblastí a ukazuje další práci potřebnou k oblasti pro tuto úlohu použít:

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

Tady je velký rozdíl mezi `ClipPath` metoda a `ClipRegion` metody:

> [!IMPORTANT]
> Na rozdíl od `ClipPath` metody `ClipRegion` metoda nemá vliv transformací.

Informace o tom důvody pro tento rozdíl, je dobré znát jaké oblasti je. Pokud jste si mysleli, že o tom, jak klip operace nebo operace oblasti může být implementují interně, pravděpodobně vypadá velmi složité. Spolu se několik cest potenciálně velmi složitý a osnovy Výsledná cesta je pravděpodobně vylepšením připomínající.

Ale tuto úlohu je výrazně zjednodušené pokud každá cesta je omezená na řadu řádky vodorovné kontroly, jako například sítě na zastaralý a takový trubky televizorů. Každý řádek kontroly je jednoduše vodorovné čáry s počáteční bod a koncový bod. Například kruhu s poloměrem 10 lze rozložit na 20 řádků vodorovné kontroly, z nichž každý začíná v levé části kruhu a končí na pravé straně. Kombinace dvou kruhy s jakoukoli operaci oblasti stane velmi jednoduchý, protože ji stačí zkoušení produktu počáteční a koncové souřadnice každý pár odpovídající prohledávání řádků.

To je, co je oblast: řady čar vodorovné kontroly, který definuje oblast.

Ale pokud oblast je omezená na řadu kontroly řádky, tyto kontroly, které řádky jsou založeny na dimenzi pixelu. Přesněji řečeno oblast není objekt vektorové grafiky. Je ze své podstaty blíž ke komprimované monochromatický rastrový obrázek než na cestu. Oblasti v důsledku toho nelze škálovat nebo otočit bez ztráty přesnosti a proto se nepřevedou při použití pro ořezové oblasti.

Můžete však použití transformací na oblasti pro účely vykreslování. **Oblasti malířského** program ukazuje zřetelně vnitřní povaze oblastech. [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) Vytvoří třídu `SKRegion` objektu na základě `SKPath` 10 jednotek poloměr kruhu. Transformace poté rozbalí této kruh tak, aby vyplnil stránky:

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

`DrawRegion` Volání vyplní oblast zvýrazněných oranžovou barvou, zatímco `DrawPath` volání tahy původní cesta modrou barvu pro porovnání:

[![](clipping-images//regionpaint-small.png "Trojitá snímek obrazovky stránky oblasti Malování")](clipping-images/regionpaint-large.png#lightbox "Trojitá snímek obrazovky stránky oblasti Malování")

Oblast je jasně řady diskrétní souřadnic.

Pokud nepotřebujete k použití transformací souvislosti s vaší oblasti oříznutí, můžete použít oblastí pro oříznutí, jako **čtyři – listu Jetelové** demonstruje stránky. [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) Třídy vytvoří oblast složené ze čtyř oblastí cyklický, nastaví tuto oblast složené jako oblast výstřižek a pak vykreslí řadu 360 rovné čáry pocházející z centra na stránce:

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

To skutečně nevypadá Jetelové listu – čtyři, ale je obrázek, který jinak může být obtížné vykreslit bez omezení:

[![](clipping-images//fourleafclover-small.png "Trojitá snímek obrazovky stránky čtyři – listu Jetelové")](clipping-images/fourleafclover-large.png#lightbox "Trojitá snímek Jetelové listu – čtyři stránky")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
