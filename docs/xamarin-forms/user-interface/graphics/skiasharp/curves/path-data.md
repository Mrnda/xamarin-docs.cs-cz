---
title: Data SVG cesty
description: "Zadejte cesty pomocí textové řetězce ve formátu Škálovatelné vektorová grafika"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/24/2017
ms.openlocfilehash: feb4c5f4c7e7ad3fc5f762786001be9aa57ae718
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="svg-path-data"></a>Data SVG cesty

_Zadejte cesty pomocí textové řetězce ve formátu Škálovatelné vektorová grafika_

`SKPath` Třída podporuje definici celou cestu objektů z textové řetězce ve formátu navázat specifikací škálovatelné grafiky SVG (Vector). Uvidíte později v tomto článku jak může představovat celou cestu jako je tato v textovém řetězci:

![](path-data-images/pathdatasample.png "Ukázka cestu definovaný s SVG data cesty")

SVG je na základě XML grafiky programovací jazyk pro webové stránky. Protože SVG musí umožňovat cesty být definován v značky namísto řady volání funkce, obsahuje standardní SVG velmi stručným způsob zadání cesty celý grafiky jako textový řetězec.

V rámci SkiaSharp tento formát se označuje jako "SVG cesta data". Formát je podporováno také v založených na Windows XAML programovací prostředí, včetně Windows Presentation Foundation a univerzální platformy Windows, kde se označuje jako [syntaxe značek cesty](https://msdn.microsoft.com/library/ms752293%28v=vs.110%29.aspx) nebo [přesunout a kreslení příkazy Syntaxe](/windows/uwp/xaml-platform/move-draw-commands-syntax/). Můžete také fungovat jako formátu exchange pro Image vektorové grafiky, zejména v textové soubory, jako je například XML.

SkiaSharp definuje dvě metody s slova `SvgPathData` jejich názvy:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Statické [ `ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) metoda převede řetězec na `SKPath` objektu, zatímco [ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) převede `SKPath` objekt na řetězec.

Tady je řetězec SVG pro hvězdu pět odkazoval na střed v bodu (0, 0) o poloměru 100:

```csharp
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Písmena jsou příkazy, které sestavení `SKPath` objektu. `M` označuje `MoveTo` volat, `L` je `LineTo`, a `Z` je `Close` zavřete profilu okraje. Každou dvojici čísel poskytuje souřadnici X a Y bodu. Všimněte si, že `L` příkaz následuje několik bodů oddělených čárkami. V řadě souřadnice a body, čárky a prázdné znaky jsou považovány shodně. Některé programátory preferuje uvést čárkami mezi souřadnice X a Y a nikoli mezi body, ale čárky nebo prostory jsou potřeba jenom ke zabránilo nejednoznačnosti. Toto je perfektně právní:

```csharp
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

Syntaxe data cesty SVG oficiálně zdokumentována [části 8.3 specifikace SVG](http://www.w3.org/TR/SVG11/paths.html#PathData). Zde je souhrn:

## <a name="moveto"></a>**MoveTo**

```csharp
M x y
```

Toto nové obrysem v cestě začne nastavením aktuální pozici. Data cesty by měl začínat vždy `M` příkaz.

## <a name="lineto"></a>**LineTo**

```csharp
L x y ...
```

Tento příkaz přidá přímku (nebo řádky) do cesty a nastaví nové aktuální pozici na konec posledního řádku. Můžete provést `L` s několika párů *x* a *y* souřadnice.

## <a name="horizontal-lineto"></a>**Vodorovné LineTo**

```csharp
H x ...
```

Tento příkaz přidá na vodorovném řádku na cestu a nastaví nové aktuální pozici na konec řádku. Můžete provést `H` příkaz s více *x* souřadnice, ale nebude mít mnoho smysl.

## <a name="vertical-line"></a>**Svislá čára**

```csharp
V y ...
```

Tento příkaz přidá svislice na cestu a nastaví nové aktuální pozici na konec řádku.

## <a name="close"></a>**Zavřete**

```csharp
Z
```

`C` Příkaz zavře přidáním přímku od aktuální pozice na začátek Kontury obrysu.

## <a name="arcto"></a>**ArcTo**

Příkaz pro přidání eliptické oblouku do Kontury je zdaleka příkaz těch nejsložitějších ve specifikaci celý data cesty SVG. Je pouze příkaz, ve kterém může představovat čísla něco jiného než souřadnic hodnoty:

```csharp
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

*Rx* a *Tě* parametry jsou vodorovného a svislého poloměr se třemi tečkami. *Úhel otočení* je po směru hodinových ručiček ve stupních.

Nastavte *velké oblouk příznak* 1 pro velké oblouk nebo na hodnotu 0 pro malé oblouk.

Nastavte *oblouku příznak* 1 pro po směru hodinových ručiček a na hodnotu 0 pro proti směru hodinových ručiček.

Oblouk vykreslením do bodu (*x*, *y*), který bude nový aktuální pozici.

## <a name="cubicto"></a>**CubicTo**

```csharp
C x1 y1 x2 y2 x3 y3 ...
```

Tento příkaz přidá krychlový Bézierovy křivky z aktuální pozici k (*x3*, *y3*), který bude nový aktuální pozici. V bodech (*x1*, *y1*) a (*x2*, *y2*) jsou kontrolní body.

Při jedné lze zadat více Bézierových křivek `C` příkaz. Počet bodů musí být násobkem 3.

Je také "smooth" Bézierovy křivky příkaz:

```csharp
S x2 y2 x3 y3 ...
```

Tento příkaz by mělo vycházet regulární příkaz Bézierovy (i když, který má není nezbytně nutné). Příkaz smooth Bézierovy vypočítá první řídicí bod tak, aby se odraz druhý řídicí bod předchozí Bézierovy kolem jejich vzájemné bodu. Tyto tři body jsou proto colinear a připojení mezi dvě Bézierovy křivky je smooth.

## <a name="quadto"></a>**QuadTo**

```csharp
Q x1 y1 x2 y2 ...
```

Pro kvadratických Bézierových křivek počet bodů musí být násobkem 2. Řídicí bod je (*x1*, *y1*) a koncový bod (a nové aktuální pozici) (*x2*, *y2*)

Je také smooth kvadratické křivky příkaz:

```csharp
T x2 y2 ...
```

Řídicí bod je vypočítáváno na řídicí bod předchozí kvadratické křivky.

Všechny tyto příkazy jsou také k dispozici ve verzích "relativní", kde souřadnic body jsou relativní vzhledem k aktuální pozici. Tyto příkazy relativní začínat malá písmena, například `c` místo `C` pro relativní verzi krychlový Bézierovy příkazu.

Jedná se rozsah definici data cesty SVG. Neexistuje žádné zařízení pro opakující se skupiny příkazy nebo provádění libovolného typu výpočtu. Příkazy pro `ConicTo` nebo jiné typy oblouk specifikace nejsou k dispozici.

Statické [ `SKPath.ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) metoda očekává platný řetězec příkazů SVG. Pokud se detekuje chyby syntaxe, vrátí metoda `null`. To je indikace pouze chyby.

[ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) Metoda je užitečný pro získávání dat cesta SVG z existující `SKPath` objekt pro přenos do jiného programu, nebo k ukládání ve formátu textového souboru například XML. ( `ToSvgPathData` Metoda není ukázáno v ukázkovém kódu v tomto článku.) Proveďte *není* očekávat `ToSvgPathData` vrátit řetězec odpovídající přesně volání metod, které vytvořili cestu. Zejména budete zjistíte, že oblouky se převedou na několika `QuadTo` příkazy, a jak se zobrazují v cestě data vrácená z `ToSvgPathData`.

**Cesta Data Hello** stránky spells na slovo "HELLO" pomocí data cesty SVG. Jak `SKPath` a `SKPaint` objekty jsou definovány jako pole v [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) třídy:

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

Cesta k definování textový řetězec začíná v levém horním rohu v bodě (0, 0). Každý písmeno je 50 jednotky široké a 100 jednotky výšku a písmena jsou odděleny jiné jednotky 25, což znamená, že se celou cestu 350 jednotky široké.

"H" text "Hello" se skládá z tři profily okrajů jednořádkové dvě Bézierovy křivky krychlový připojené při "E". Všimněte si, že `C` příkaz následuje šest bodů a dva kontrolních bodů mají Souřadnice Y – 10 a 110, který zapisuje je mimo rozsah Souřadnice Y ostatní písmena. Dva řádky připojené, je "L" při ' o. je elipsy, který je vykreslen s `A` příkaz.

Všimněte si, že `M` příkaz, který začíná poslední obrysem nastaví pozici k bodu (350, 50), což je svislé středu doleva strany ' o. Jak znázorňuje následující první čísla `A` příkaz, má se třemi tečkami vodorovné radius 25 a svislé radius 50. Koncový bod je indikován posledních pár čísel ve `A` příkaz, který představuje bod (300, 49.9). Která se záměrně jen mírně liší od počáteční bod. Pokud koncový bod je nastavena na počáteční bod, nebude vykreslen oblouk. Kreslení dokončení elipsy, musíte nastavit koncový bod zavřete na (ale není rovno) počáteční bod, nebo musíte použít dva nebo více `A` příkazy, jednotlivých součástí se třemi tečkami dokončení.

Můžete chtít přidejte následující příkaz pro konstruktor stránky a pak nastavit zarážky prozkoumat výsledný řetězec:

```csharp
string str = helloPath.ToSvgPathData();
```

Dozvíte se, že oblouk se nahradil údajem dlouho řadu `Q` příkazy pro postupného aproximace oblouku pomocí kvadratických Bézierových křivek.

`PaintSurface` Obslužná rutina získává úzkou hranice cesta, která neobsahuje kontrolní body pro "E" a "o. křivek. Tři transformace přesunutí center cesty do bodu (0, 0), škálování cestu k velikost na plátno (ale také zohledněním šířku tahu) a potom center cesty na střed plátna:

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

Cesta vyplní celé plátno, která vypadá přijatelnější při zobrazení na šířku:

[![](path-data-images/pathdatahello-small.png "Trojitá snímek obrazovky stránky cesta Data Hello")](path-data-images/pathdatahello-large.png "Trojitá snímek obrazovky stránky cesta Data Hello")

**Cesta Data Cat** stránka je podobné. Oba objekty Malování a cesta jsou definovány jako pole v [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) třídy:

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

Vedoucí cat je kruh a zde je vykreslen se dvěma `A` příkazy, z nichž každý nevykresluje polokruhu. Obě `A` příkazy pro hlavičky definují vodorovného a svislého poloměr 100. První oblouk začíná na (240, 100) a končí (240, 300), který se stane počáteční bod pro druhý oblouk, který končí zpět na (240, 100).

Dva očí vykresleny také se dvěma `A` příkazy a stejně jako u vaší kočky head, druhý `A` příkaz končí na stejném místě jako začátek první `A` příkaz. Ale tyto dvojice `A` příkazy nedefinují elipsy. S každou oblouku je 40 jednotek a poloměr je také 40 jednotek, což znamená, že tyto oblouky nejsou úplné semicircles.

`PaintSurface` Obslužná rutina provede transformací podobné jako v předchozím příkladu, ale nastaví jedné `Scale` faktor, který se zachovat poměr stran a poskytnout málo okraj, abyste vaší kočky vousů nemáte touch strany obrazovky:

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

Tady je programy spuštěné na všech tří platformách:

[![](path-data-images/pathdatacat-small.png "Trojitá snímek obrazovky stránky cesta Data Cat")](path-data-images/pathdatacat-large.png "Trojitá snímek obrazovky stránky cesta Data Cat")

Za normálních okolností, kdy `SKPath` objektu je definována jako pole, obrysy cesty musí být definován v konstruktoru nebo jiným způsobem. Při použití data SVG cesty, ale už víte, že zcela v definici pole lze zadat cestu.

Dříve **Ugly hodiny analogovým** ukázku v [ **transformace otočit** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) článek zobrazí jako jednoduchý řádky do nesprávných rukou hodiny. **Poměrně analogovým hodiny** uvedený program nahradí tyto řádky s `SKPath` objekty definované jako pole v [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) třídy spolu s `SKPaint` objekty:

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

Rukou hodin a minut nyní mají uzavřen oblasti, tak aby se tyto rukou od sebe liší, jsou vykreslovány se černý obrys i šedé výplně pomocí `handStrokePaint` a `handFillPaint` objekty.

V dříve **Ugly hodiny analogovým** ukázce málo kroužky, která označena za hodin a minut se vykresluje v smyčku. V tomto **poměrně analogovým hodiny** ukázku, se používá úplně jinou přístup: hodin a minut značky jsou řádky tečkovaná vykreslené s `minuteMarkPaint` a `hourMarkPaint` objekty:

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

[ **Tečky a pomlčky** ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) průvodce popsané, jak můžete použít `SKPathEffect.CreateDash` metodu pro vytvoření na přerušovanou čáru. První argument `float` pole, které obvykle má dva elementy: první prvek je délka pomlček a druhý prvkem je mezera mezi pomlček. Když `StrokeCap` je nastavena na `SKStrokeCap.Round`, pak zaokrouhlené konců čárka efektivně prodloužit délka dash podle šířku tahu na obou stranách čárka. Nastavení první prvek pole na hodnotu 0, tedy vytvoří tečkovaná čára.

Tyto rozestupu se řídí druhý prvkem pole. Jak uvidíte krátce, tyto dvě `SKPaint` objekty se používají k vykreslení kroužky se serverem radius 90 jednotek. Obvodu tento kroužek je proto 180π, což znamená, že značky 60 minut musí být uvedeny všechny jednotky 3π, což je druhá hodnota v `float` pole v `minuteMarkPaint`. Značky ve 12 hodin musí být každý 15π jednotek, což je hodnota za sekundu `float` pole.

`PrettyAnalogClockPage` Třída nastaví časovače zneplatní povrchu každých 16 milisekund a `PaintSurface` obslužná rutina je volána v tomto kurzu. Starší definice `SKPath` a `SKPaint` objekty povolit pro velmi čisté kreslení kódu:

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

Něco speciální provádí pomocí druhé straně, ale. Protože hodiny se aktualizuje každých 16 milisekund `Millisecond` vlastnost `DateTime` hodnotu lze potenciálně na druhé straně animace oblouku místo ten, který přesune v diskrétní skoků z druhého druhou. Ale tento kód neumožňuje přesun jako plynulé. Místo toho použije platformě Xamarin.Forms [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) a [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) animace usnadnění funkce pro jiný druh pohyb. Tyto funkce usnadnění způsobit druhé straně přesouvat v jerkier způsobem & #x 2014; stahování zpět malým před přesune ho a potom mírně překročením teploty svůj cíl, vliv to bohužel nelze reprodukovat v těchto statické snímky obrazovky:

[![](path-data-images/prettyanalogclock-small.png "Trojitá snímek obrazovky stránky poměrně analogovým hodiny")](path-data-images/prettyanalogclock-large.png "Trojitá snímek obrazovky stránky poměrně analogovým hodiny")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
