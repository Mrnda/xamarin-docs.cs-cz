---
title: Data cesty SVG v ve Skiasharpu
description: Tento článek vysvětluje, jak definovat cesty ve Skiasharpu používání textové řetězce ve formátu Scalable Vector Graphics a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: charlespetzold
ms.author: chape
ms.date: 05/24/2017
ms.openlocfilehash: f3c06198ae9e677c667c9216b3ace8784a6056b2
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615324"
---
# <a name="svg-path-data-in-skiasharp"></a>Data cesty SVG v ve Skiasharpu

_Definovat cesty k používání textové řetězce ve formátu Scalable Vector Graphics_

`SKPath` Třída podporuje definici celou cestu objektů z řetězce textu ve formátu stanovené specifikaci grafiky SVG (Scalable Vector). Zobrazí se vám dále v tomto článku jak mohou představovat celou cestu jako je například tento v textovém řetězci:

![](path-data-images/pathdatasample.png "Ukázka cestu definovanou s data cesty SVG")

SVG je založené na XML grafiky programovací jazyk pro webové stránky. Protože SVG musí umožňovat cesty byly definovány v značek namísto řadu volání funkcí, obsahuje standardní SVG velmi stručným způsobem určit cestu k celé grafiky jako textový řetězec.

V rámci SkiaSharp, tento formát je označován jako "Data cesty SVG-." Formát je podporováno také v XAML Windows programovací prostředí, včetně Windows Presentation Foundation a univerzální platformu Windows, ve kterém se označuje jako [syntaxe značek cesty](https://msdn.microsoft.com/library/ms752293%28v=vs.110%29.aspx) nebo [přesunout a nakreslete příkazy Syntaxe](/windows/uwp/xaml-platform/move-draw-commands-syntax/). Může sloužit také jako formát výměny pro vektorové grafiky, zejména v textové soubory, jako je například XML.

Ve Skiasharpu definuje dvě metody s slova `SvgPathData` v jejich názvy:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Statické [ `ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) metoda převede řetězec na `SKPath` objektu, zatímco [ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) převede `SKPath` objekt na řetězec.

Tady je řetězec SVG pro star pět odkazovala na střed v bodě (0, 0) s poloměrem 100:

```csharp
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Písmena jsou příkazy, které sestavení `SKPath` objektu. `M` označuje `MoveTo` volání, `L` je `LineTo`, a `Z` je `Close` zavřete obrysem. Každou dvojici čísel poskytuje souřadnici X a Y bodu. Všimněte si, že `L` příkaz následuje několik bodů oddělených čárkami. V řadě souřadnice a body, čárky a mezery jsou zpracovávána stejně jako. Někteří programátoři raději umístěte čárek mezi souřadnice X a Y, nikoli mezi body, ale čárkami nebo mezerami pouze povinnost, aby se zabránilo nejednoznačnosti. To je naprosto platný:

```csharp
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

Syntaxe data cesty SVG je formálně zdokumentované v [části 8.3 specifikace SVG](http://www.w3.org/TR/SVG11/paths.html#PathData). Toto je souhrn:

## <a name="moveto"></a>**MoveTo**

```csharp
M x y
```

Tím se zahájí nové obrysu v cestě tak, že nastavíte na aktuální pozici. Data cesty by měl vždy začínají znakem `M` příkazu.

## <a name="lineto"></a>**LineTo**

```csharp
L x y ...
```

Tento příkaz přidá rovné čáry (nebo řádky) do cesty a nastaví nové aktuální pozice na konec posledního řádku. Můžete postupovat podle `L` příkazu s více dvojice *x* a *y* souřadnice.

## <a name="horizontal-lineto"></a>**Vodorovné LineTo**

```csharp
H x ...
```

Tento příkaz přidá do cesty pro vodorovnou horizontální čáru a nastaví nové aktuální pozice na konci řádku. Můžete postupovat podle `H` příkaz s několika *x* souřadnice, ale to moc nedává smysl.

## <a name="vertical-line"></a>**Svislá čára**

```csharp
V y ...
```

Tento příkaz přidá do cesty pro svislou čáru a nastaví nové aktuální pozice na konci řádku.

## <a name="close"></a>**Zavřít**

```csharp
Z
```

`C` Příkaz zavře přidáním rovné čáry od aktuální pozice na začátku Kontury obrysu.

## <a name="arcto"></a>**ArcTo**

Příkaz pro přidání oblouku elipsy rozvrhu je jednoznačně nejpopulárnější nejsložitější příkaz ve specifikaci celý data cesty SVG. Je pouze příkaz, ve kterém může představovat čísla něco jiného než hodnoty souřadnic:

```csharp
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

*Rx* a *ry* parametry jsou vodorovný a svislý poloměr elipsy. *Úhel otočení* je po směru hodinových ručiček ve stupních.

Nastavte *velké oblouk příznak* pro velké oblouk 1 nebo 0 pro malé oblouk.

Nastavit *vyčištění příznaku* hodnotu 1, aby po směru hodinových ručiček a na hodnotu 0 pro proti směru hodinových ručiček.

Oblouku je vykresleno do bodu (*x*, *y*), který se stane novou aktuální pozici.

## <a name="cubicto"></a>**CubicTo**

```csharp
C x1 y1 x2 y2 x3 y3 ...
```

Tento příkaz přidá kubické Bézierovy křivky z aktuální pozice (*x3*, *y3*), který se stane novou aktuální pozici. Body (*x1*, *y1*) a (*x2*, *y2*) jsou kontrolní body.

Je možné zadat více Bézierových křivek tak jediné `C` příkazu. Počet bodů musí být násobkem 3.

Je také "smooth" Bézierovy křivky příkaz:

```csharp
S x2 y2 x3 y3 ...
```

Tento příkaz by měly dodržovat regulární příkaz Bézierovy (i když, který má není nezbytně nutné). Smooth Bézierovy příkaz vypočítá první řídicí bod tak, aby se reflexe druhého ovládacího bodu předchozí Bézierovy kolem jejich vzájemné bodu. Tyto tři body jsou proto colinear a připojení mezi dvě Bézierovy křivky je hladký průběh.

## <a name="quadto"></a>**QuadTo**

```csharp
Q x1 y1 x2 y2 ...
```

Kvadratické Bézierovy křivky počet bodů musí být násobkem 2. Řídicí bod (*x1*, *y1*) a koncový bod (a nové aktuální pozici) (*x2*, *y2*)

Je také smooth Kvadratická křivka příkaz:

```csharp
T x2 y2 ...
```

Řídicí bod se vypočítá podle řídicí bod předchozí Kvadratická křivka.

Tyto příkazy jsou dostupné v "relativní" verze, ve kterém jsou souřadnice bodů vzhledem k aktuální pozici. Tyto relativní příkazy začínají malými písmeny, například `c` spíše než `C` relativní verze kubické Bézierovy příkazu.

Toto je v rozsahu definice data cesty SVG. Neexistuje žádné zařízení pro opakující se skupiny příkazů nebo provedení libovolného typu výpočtu. Příkazy pro `ConicTo` nebo jiné typy oblouk specifikací nejsou k dispozici.

Statické [ `SKPath.ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) metoda očekává platný řetězec SVG příkazů. Pokud se zjistí všechny chyby syntaxe, metoda vrátí `null`. To je označení pouze chyby.

[ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) Metoda je užitečná pro získání data cesty SVG z existující `SKPath` objektu přenést do jiného programu, nebo k ukládání ve formátu textového souboru, například XML. ( `ToSvgPathData` Metoda není ukázáno v ukázkovém kódu v tomto článku.) Proveďte *není* očekávat `ToSvgPathData` vrátit řetězec přesně odpovídající volání metody, které vytvořili cestu. Konkrétně se dozvíte, že oblouky jsou převedeny na více `QuadTo` příkazy, a to je, jak se zobrazují v cestě data vrácená z `ToSvgPathData`.

**Cesty dat Hello** stránky spells si slovo "HELLO" data cesty SVG. Jak `SKPath` a `SKPaint` objekty jsou definovány jako pole v [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) třídy:

```csharp
public class PathDataHelloPage : ContentPage
{
    SKPath helloPath = SKPath.ParseSvgPathData(
        "M 0 0 L 0 100 M 0 50 L 50 50 M 50 0 L 50 100" +                // H
        "M 125 0 C 60 -10, 60 60, 125 50, 60 40, 60 110, 125 100" +     // E
        "M 150 0 L 150 100, 200 100" +                                  // L
        "M 225 0 L 225 100, 275 100" +                                  // L
        "M 300 50 A 25 50 0 1 0 300 49.9 Z");                           // O

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ...
}
```

Cesta, která definuje textový řetězec začíná levém horním rohu na místě (0, 0). Každé písmeno je 50 jednotek široký a výšku 100 jednotek a písmena jsou odděleny jiné jednotky 25, což znamená, že celou cestu je 350 jednotky široké.

"H" "Hello" se skládá ze tří jednořádkové rozvrh, "E" je dvě připojených kubické Bézierovy křivky. Všimněte si, že `C` příkaz následuje šest bodů a dva kontrolních bodů mají Souřadnice Y – 10 a 110, které se dostanou mimo rozsah Souřadnice Y ostatní písmena. "L" je dva spojené čáry, zatímco "jednoznakový je elipsy, který je vykreslen pomocí `A` příkazu.

Všimněte si, že `M` příkaz, který začíná poslední rozvrh nastaví pozici k bodu (350, 50), což je svislé center vlevo strany "jednoznakový. Podle první následující čísla `A` příkazu se třemi tečkami má vodorovný radius 25 a svislý poloměr 50. Koncový bod je indikován posledních pár čísel ve `A` příkaz, který představuje bod (300, 49.9). To je záměrně jen mírně lišit od počátečního bodu. Pokud koncový bod je nastavena na počáteční bod, se nevykreslí oblouk. K dokončení elipsa nakreslit, je nutné nastavit koncový bod Zavřít (ale není rovno) počáteční bod, nebo musíte použít nejmíň dva `A` příkazy, každá část úplný elipsy.

Můžete chtít přidejte následující příkaz pro konstruktor stránky a pak nastavte zarážku pro zjištění výsledného řetězce:

```csharp
string str = helloPath.ToSvgPathData();
```

Dozvíte se, že oblouku bylo nahrazeno tématem dlouhé řadě `Q` příkazy pro menších aproximace oblouku pomocí kvadratické Bézierovy křivky.

`PaintSurface` Obslužná rutina získává absolutní hranice cesty, který neobsahuje kontrolní body pro "E" a "jednoznakový křivky. Tři transformací přesunutí center cestu do bodu (0, 0), škálovat ji na velikost plátna (ale také zohledněním šířka tahu) a potom center cestu na střed plátna:

```csharp
public class PathDataHelloPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect bounds;
        helloPath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(info.Width / (bounds.Width + paint.StrokeWidth),
                     info.Height / (bounds.Height + paint.StrokeWidth));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(helloPath, paint);
    }
}
```

Cesta vyplní celé plátno, která vypadá přijatelnějšímu při zobrazení na šířku v režimu:

[![](path-data-images/pathdatahello-small.png "Trojitá snímek obrazovky stránky cesty dat Hello")](path-data-images/pathdatahello-large.png#lightbox "Trojitá snímek obrazovky stránky cesty dat Hello")

**Cesty dat Cat** je podobná stránka. Objekty cesty a Malování se definují jako pole v [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) třídy:

```csharp
public class PathDataCatPage : ContentPage
{
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 5
    };
    ...
}
```

Hlavní cat je kruhu a zde je vykreslen pomocí dvou `A` příkazy, z nichž každý nakreslí polokruhu. Obě `A` příkazy pro vedoucí definovat vodorovný a svislý poloměr 100. První oblouk začíná na (240, 100) a končí (240, 300,), který se stane počáteční bod pro druhý oblouk, které končí na (240, 100).

Dva oči vykresleny také dva `A` příkazy a stejně jako u kočky head, druhý `A` příkaz končí na stejném místě jako začátek první `A` příkazu. Ale tyto dvojice `A` příkazy nedefinují elipsu. Na každý oblouku je 40 jednotek a poloměr je také 40 jednotek, což znamená, že tyto oblouky nejsou úplné semicircles.

`PaintSurface` Obslužná rutina provede podobné transformace jako předchozí ukázce, ale nastaví jediného `Scale` faktorem, který je zachovat poměr stran a poskytuje malou okraj, aby kočky whiskers don't touch stranách obrazovky:

```csharp
public class PathDataCatPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        SKRect bounds;
        catPath.GetBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(0.9f * Math.Min(info.Width / bounds.Width,
                                     info.Height / bounds.Height));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(catPath, paint);
    }
}
```

Tady je program spuštěn na všech třech platformách:

[![](path-data-images/pathdatacat-small.png "Trojitá snímek obrazovky stránky cesty dat Cat")](path-data-images/pathdatacat-large.png#lightbox "Trojitá snímek obrazovky stránky cesty dat Cat")

Obvykle, když `SKPath` objekt je definovaný jako pole, rozvrh cesty musí být definován v konstruktoru nebo jiným způsobem. Při použití data cesty SVG, ale už víte, že cesta se dá nastavit jenom v definici pole.

Dříve **bez zbytečných prvků hodiny obdobu jmenovek** ukázku v [ **transformace otočení** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) článku do rukou hodin zobrazí jako jednoduchý řádky. **Poměrně obdobu jmenovek hodiny** uvedený program nahradí tyto řádky s `SKPath` objekty definované jako pole v [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) třídy společně s `SKPaint` objekty:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    // Clock hands pointing straight up
    SKPath hourHandPath = SKPath.ParseSvgPathData(
        "M 0 -60 C   0 -30 20 -30  5 -20 L  5   0" +
                "C   5 7.5 -5 7.5 -5   0 L -5 -20" +
                "C -20 -30  0 -30  0 -60 Z");

    SKPath minuteHandPath = SKPath.ParseSvgPathData(
        "M 0 -80 C   0 -75  0 -70  2.5 -60 L  2.5   0" +
                "C   2.5 5 -2.5 5 -2.5   0 L -2.5 -60" +
                "C 0 -70  0 -75  0 -80 Z");

    SKPath secondHandPath = SKPath.ParseSvgPathData(
        "M 0 10 L 0 -80");

    // SKPaint objects
    SKPaint handStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2,
        StrokeCap = SKStrokeCap.Round
    };

    SKPaint handFillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Gray
    };
    ...
}
```

Rukou hodinu a minutu teď mít uzavřeny oblastí, tak aby tyto praktické liší od sebe navzájem, jsou vykreslovány pomocí černého osnovy i šedé výplně pomocí `handStrokePaint` a `handFillPaint` objekty.

V předchozím **bez zbytečných prvků hodiny obdobu jmenovek** ukázce daném malém kruhy, která označena hodiny a minuty byly nakresleny ve smyčce. V tomto **poměrně obdobu jmenovek hodiny** ukázku, úplně jiný přístup se používá: hodiny a minuty. značky jsou tečkované čáry dekorace s `minuteMarkPaint` a `hourMarkPaint` objekty:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    SKPaint minuteMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 3 * 3.14159f }, 0)
    };

    SKPaint hourMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 6,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 15 * 3.14159f }, 0)
    };
    ...
}
```

[ **Tečky a pomlčky** ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) Průvodce popsáno, jak můžete použít `SKPathEffect.CreateDash` metodu pro vytvoření na přerušovanou čáru. První argument `float` pole, které má obvykle dva prvky: první prvek je délka pomlček a druhý prvek je mezera mezi pomlček. Když `StrokeCap` je nastavena na `SKStrokeCap.Round`, pak zakulacený konce čárka efektivně prodloužit délku pomlčky podle šířka tahu na obou stranách čárka. Proto nastavení na první prvek pole na hodnotu 0, vytvoří se tečkovaná čára.

Vzdálenost mezi tyto tečky se řídí druhém prvku pole. Jak se zobrazí po chvíli, tyto dvě `SKPaint` objekty se používají k kreslí kruhy s protokolem radius 90 jednotek. Obvod tento kruhu je proto 180π, to znamená, že značky 60 minut musí být uvedena každé jednotky 3π, což je druhá hodnota v `float` obsahuje pole `minuteMarkPaint`. Dvanáct hodin značky musí být uvedena každý 15π jednotek, což je hodnota v druhém `float` pole.

`PrettyAnalogClockPage` Třída nastaví časovače zrušit platnost povrchu každých 16 milisekund a `PaintSurface` obslužná rutina je volána v tomto kurzu. Předchozí definice `SKPath` a `SKPaint` objekty umožňují velmi čistým kreslení kódu:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Transform for 100-radius circle in center
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / 200, info.Height / 200));

        // Draw circles for hour and minute marks
        SKRect rect = new SKRect(-90, -90, 90, 90);
        canvas.DrawOval(rect, minuteMarkPaint);
        canvas.DrawOval(rect, hourMarkPaint);

        // Get time
        DateTime dateTime = DateTime.Now;

        // Draw hour hand
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawPath(hourHandPath, handStrokePaint);
        canvas.DrawPath(hourHandPath, handFillPaint);
        canvas.Restore();

        // Draw minute hand
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawPath(minuteHandPath, handStrokePaint);
        canvas.DrawPath(minuteHandPath, handFillPaint);
        canvas.Restore();

        // Draw second hand
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        canvas.Save();
        canvas.RotateDegrees(6 * (dateTime.Second + (float)t));
        canvas.DrawPath(secondHandPath, handStrokePaint);
        canvas.Restore();
    }
}
```

Něco speciální se dělá druhé straně, ale. Protože hodiny se aktualizuje každých 16 milisekund `Millisecond` vlastnost `DateTime` hodnota může potenciálně použít pro animaci vyčištění druhé straně místo jednoho, který přesouvá v samostatných přejde z druhého do druhé. Ale tento kód neumožňuje pohyb být hladký průběh. Místo toho využívá Xamarin.Forms [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) a [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) animace funkce pro jiný druh přesun usnadnění. Tyto funkce usnadnění způsobit druhé straně přesunout jerkier způsobem &mdash; přijímání změn zpět trochu před přesunu, a potom mírně over-pass-the potíží svůj cíl, efektu to bohužel není možné reprodukovat v těchto statických snímky obrazovky:

[![](path-data-images/prettyanalogclock-small.png "Trojitá snímek obrazovky stránky poměrně obdobu jmenovek hodiny")](path-data-images/prettyanalogclock-large.png#lightbox "Trojitá snímek obrazovky stránky poměrně obdobu jmenovek hodiny")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
