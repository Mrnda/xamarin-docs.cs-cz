---
title: Segmentované zobrazení rastrových obrázků ve Skiasharpu
description: Zobrazí rastrový obrázek ve Skiasharpu tak, aby se některé oblasti roztažená a některých oblastech nejsou.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79AE2033-C41C-4447-95A6-76D22E913D19
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: e5bfa076a8746abd6275e9d7a8393c7c8ab53294
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615233"
---
# <a name="segmented-display-of-skiasharp-bitmaps"></a>Segmentované zobrazení rastrových obrázků ve Skiasharpu

SkiaSharp `SKCanvas` objekt definuje metodu s názvem `DrawBitmapNinePatch` a dvě metody s názvem `DrawBitmapLattice` , které jsou velmi podobné. Obě tyto metody vykreslení rastrového obrázku na velikost cílového obdélníku, ale místo rovnoměrně roztažení rastrový obrázek, zobrazení části rastrového obrázku v jeho v pixelech a roztáhnout jiné části rastrového obrázku tak, aby obdélníku:

![Segmentované ukázky](segmented-images/SegmentedSample.png "segmentované vzorku")

Tyto metody jsou obvykle používané k vykreslování rastrové obrázky, které tvoří součást objekty uživatelského rozhraní, jako jsou tlačítka. Při navrhování tlačítko, obvykle chcete, aby velikost tlačítka založený na obsah na tlačítko, ale budete zřejmě chtít okraj tlačítka. Chcete-li být stejnou šířku bez ohledu na obsah na tlačítko. To je ideální aplikace z `DrawBitmapNinePatch`.

`DrawBitmapNinePatch` zvláštním případem je `DrawBitmapLattice` , ale je snadnější ze dvou způsobů používání a pochopení.

## <a name="the-nine-patch-display"></a>Zobrazení devět oprava 

Koncepčně [ `DrawBitmapNinePatch` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmapNinePatch/p/SkiaSharp.SKBitmap/SkiaSharp.SKRectI/SkiaSharp.SKRect/SkiaSharp.SKPaint/) rastrový obrázek dělí na devět obdélníky:

![Oprava devět](segmented-images/NinePatch.png "devět opravy")

Obdélníky na čtyři rohy se zobrazují v jejich velikosti pixelů. Jak šipky označují, jiné oblasti na okrajích rastrového obrázku jsou roztažená vodorovně nebo svisle do oblasti cílového obdélníku. Obdélník v centru je roztažená vodorovně i svisle. 

Pokud cílového obdélníku zobrazíte i čtyři rohy v jejich v pixelech není dostatek místa, jejich zmenšování dostupné velikosti a nic ale jsou zobrazeny čtyři rohy.

Rastrový obrázek rozdělit do těchto devíti obdélníky, je pouze potřeba zadávat obdélníku v centru. Toto je syntaxe `DrawBitmapNinePatch` metody:

```csharp
canvas.DrawBitmapNinePatch(bitmap, centerRectangle, destRectangle, paint);
```

Obdélník center je relativně k rastrového obrázku. Je `SKRectI` hodnota (celé číslo verze `SKRect`) a všechny souřadnice a velikosti představují v jednotkách, které pixelů. Cílového obdélníku je relativní vzhledem k zobrazovacím povrchu. `paint` Argument je nepovinný.

**Devíti oprava zobrazení** stránku [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) ukázka nejprve používá statický konstruktor k vytvoření veřejná statická vlastnost typu `SKBitmap`:

```csharp
public partial class NinePatchDisplayPage : ContentPage
{
    static NinePatchDisplayPage()
    {
        using (SKCanvas canvas = new SKCanvas(FiveByFiveBitmap))
        using (SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 10
        })
        {
            for (int x = 50; x < 500; x += 100)
                for (int y = 50; y < 500; y += 100)
                {
                    canvas.DrawCircle(x, y, 40, paint);
                }
        }
    }

    public static SKBitmap FiveByFiveBitmap { get; } = new SKBitmap(500, 500);
    ···
}
```

Dvě stránky v tomto článku použijte tento stejný rastrový obrázek. Rastrový obrázek je 500 pixelů čtvereček a se skládá z pole 25 kruhy stejné velikosti, každý zabírá Čtvereček oblasti 100 pixelů:

![Kroužek z mřížky](segmented-images/CircleGrid.png "kroužek z mřížky")

Vytvoří konstruktor instance programu `SKCanvasView` s `PaintSurface` obslužná rutina, která používá `DrawBitmapNinePatch` zobrazíte roztažená do jeho celý zobrazovacím povrchu rastrového obrázku:

```csharp
public class NinePatchDisplayPage : ContentPage
{
    ···
    public NinePatchDisplayPage()
    {
        Title = "Nine-Patch Display";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRectI centerRect = new SKRectI(100, 100, 400, 400);
        canvas.DrawBitmapNinePatch(FiveByFiveBitmap, centerRect, info.Rect);
    }
}
```

`centerRect` Obdélník zahrnuje centrální pole 16 kruzích. Kruhy v rozích se zobrazují v jejich v pixelech a všechno ostatní je roztažená odpovídajícím způsobem:

[![Zobrazení devět Patch](segmented-images/NinePatchDisplay.png "zobrazení devět oprava")](segmented-images/NinePatchDisplay-Large.png#lightbox)

Stránka UPW je 500 pixelů na šířku a proto zobrazí horní a dolní řádky jako řadu objektů kruhy stejné velikosti. V opačném případě jsou všechny kruhy, které nejsou v rozích roztažená do formuláře symbol tří teček.

Neobvyklé zobrazení objektů, který se skládá z kombinace kruhů a symbol tří teček zkuste definování obdélník center tak, aby se překrývá řádků a sloupců kruhy:

```csharp
SKRectI centerRect = new SKRectI(150, 150, 350, 350);
```

## <a name="the-lattice-display"></a>Zobrazení mřížky

Dva `DrawBitmapLattice` metody jsou podobné `DrawBitmapNinePatch`, ale zobecněné pro libovolný počet rozdělení vodorovně nebo svisle. Tyto divize jsou určené pole celých čísel odpovídající pixelů. 

[ `DrawBitmapLattice` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmapLattice/p/SkiaSharp.SKBitmap/System.Int32[]/System.Int32[]/SkiaSharp.SKRect/SkiaSharp.SKPaint/) Metodu s parametry pro tato pole celých čísel zřejmě nefunguje. [ `DrawBitmapLattice` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmapLattice/p/SkiaSharp.SKBitmap/SkiaSharp.SKLattice/SkiaSharp.SKRect/SkiaSharp.SKPaint/) Metodu s parametrem typu `SKLattice` funguje, a, který se bude používat ve vzorcích vidíte níže.

[ `SKLattice` ](https://developer.xamarin.com/api/type/SkiaSharp.SKLattice/) Struktury definuje čtyři vlastnosti:

- [`XDivs`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.XDivs/), pole celých čísel
- [`YDivs`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.YDivs/), pole celých čísel
- [`Flags`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.Flags/), pole `SKLatticeFlags`, výčtový typ.
- [`Bounds`](https://developer.xamarin.com/api/property/SkiaSharp.SKLattice.Bounds/) typu `Nullable<SKRectI>` k určení volitelné zdrojového obdélníku v rámci rastrového obrázku

`XDivs` Pole rozdělí svislé pruhy šířku rastrového obrázku. První pruhu táhne od pixelu 0 na levé straně na `XDivs[0]`. Tento pás se vykreslí v jeho šířka v pixelech. Druhý pruhu sahá od `XDivs[0]` k `XDivs[1]`a je roztažená. Třetí pruhu sahá od `XDivs[1]` k `XDivs[2]` a je vykreslen v jeho šířka v pixelech. Poslední pruh sahá od poslední prvek pole pravým okrajem rastrového obrázku. Pokud pole má sudý počet prvků, pak se zobrazí v jeho šířka v pixelech. V opačném případě je roztažená. Total number of svislé pruhy je jeden vyšší než počet prvků v poli.

`YDivs` Pole je podobné. Výška pole se rozdělí na vodorovné pruhy.

Společně `XDivs` a `YDivs` pole rozdělit rastrového obrázku do rámečků. Počet obdélníků se rovná produktu počet vodorovné pruhy a počet svislé pruhy.

Podle dokumentace Skia `Flags` pole obsahuje jeden element pro každé obdélník, nejprve do horního řádku obdélníky, pak druhého řádku a tak dále. `Flags` Pole je typu [ `SKLatticeFlags` ](https://developer.xamarin.com/api/type/SkiaSharp.SKLatticeFlags/), výčet s následující členy:

- `Default` nahraďte hodnotou 0
- `Transparent` nahraďte hodnotou 1

Ale tyto příznaky nevypadají fungovat jako mají, a je nejlepší je ignorovat. Ale nemají nastavený `Flags` vlastnost `null`. Nastavte ho na celou řadu `SKLatticeFlags` hodnoty dostatečně velký, aby zahrnuje celkový počet obdélníků.

**Opravy devíti mřížkových** stránce používá `DrawBitmapLattice` tak, aby napodoboval `DrawBitmapNinePatch`. Používá stejný rastrový obrázek vytvořené v `NinePatchDisplayPage`:

```csharp
public class LatticeNinePatchPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeNinePatchPage ()
    {
        Title = "Lattice Nine-Patch";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    `
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 400 };
        lattice.YDivs = new int[] { 100, 400 };
        lattice.Flags = new SKLatticeFlags[9]; 

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

Jak `XDivs` a `YDivs` vlastnosti jsou nastaveny na pole pouze dvě celá čísla rozdělení rastrového obrázku nastaven na tři pruhy, vodorovně i svisle: z pixel 0 na pixel (renderováno velikost v pixelech), 100 ze 100 pixelů na 400 pixelů (roztáhnout), a z 400 pixelů na pixel 500 (velikost v pixelech). Společně `XDivs` a `YDivs` definovat celkem 9 obdélníky, což je velikost z `Flags` pole. Jednoduše vytváření pole je dostatečná pro vytvoření pole `SKLatticeFlags.Default` hodnoty.

Zobrazení je stejný jako předchozí program:

[![Mřížkových devět Patch](segmented-images/LatticeNinePatch.png "mřížkových devět oprava")](segmented-images/LatticeNinePatch-Large.png#lightbox)

**Zobrazení mřížky** stránky rozdělí 16 obdélníky rastrového obrázku:

```csharp
public class LatticeDisplayPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeDisplayPage()
    {
        Title = "Lattice Display";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 200, 400 };
        lattice.YDivs = new int[] { 100, 300, 400 };

        int count = (lattice.XDivs.Length + 1) * (lattice.YDivs.Length + 1);
        lattice.Flags = new SKLatticeFlags[count];

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

`XDivs` a `YDivs` pole se poněkud liší, což způsobí displej tak, aby nesmí být poměrně symetrické jako v předchozích příkladech:

[![Zobrazení mřížky](segmented-images/LatticeDisplay.png "mřížkových zobrazení")](segmented-images/LatticeDisplay-Large.png#lightbox)

V iOS a Android obrázků na levé straně se zobrazují jenom menší kruhy v jejich velikosti pixelů. Všechno ostatní je roztažená.

**Zobrazení mřížky** stránky umožňuje zobecnit vytváření `Flags` pole, abyste mohli experimentovat s `XDivs` a `YDivs` snadněji. Zejména, bude potřeba se podívat, co se stane, když jste nastavili na první prvek `XDivs` nebo `YDivs` pole na hodnotu 0. 

## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
