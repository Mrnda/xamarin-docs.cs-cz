---
title: Přístup k bits pixel ve Skiasharpu
description: Zjišťování různých technik vytváření pro přístup a úpravy bity pixel ve Skiasharpu rastrových obrázků.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: DBB58522-F816-4A8C-96A5-E0236F16A5C6
author: charlespetzold
ms.author: chape
ms.date: 07/11/2018
ms.openlocfilehash: 5d79dd89b5313d5d7ead665c54e9a27026cea38c
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615623"
---
# <a name="accessing-skiasharp-pixel-bits"></a>Přístup k bits pixel ve Skiasharpu

Jak už jste viděli v článku [ **rastrové obrázky ve Skiasharpu ukládání souborů**](saving.md), rastrových obrázků jsou obvykle uloženy v souborech v komprimovaném formátu, jako je například JPEG nebo PNG. V constrast není komprimována rastrový obrázek ve Skiasharpu uložené v paměti. Uloží se jako sekvenční řady pixelů. Tento nekomprimovaný formát usnadňuje přenos rastrových obrázků pro zobrazovací ploše.

Blok paměti obsazena rastrový obrázek ve Skiasharpu je uspořádaný v podobě velmi jednoduché: začíná první řádek v pixelech, zleva doprava a pak pokračuje druhém řádku. Pro barevné rastrové obrázky každý pixel se skládá ze čtyř bajtů, to znamená, že místo celková paměť vyžadovanou rastrového obrázku je čtyřikrát produktu svou šířku a výšku.

Tento článek popisuje, jak aplikace můžete získat přístup k těmto pixelů, buď přímo, díky přístupu do bloku paměti pixel rastrový obrázek, nebo nepřímo. V některých případech může být vhodné programu k analýze pixely obrázku a vytvoření histogramu z nějaké. Aplikace častěji, můžete vytvořit jedinečný imagí algorithmically vytvořením pixelů, které tvoří rastrového obrázku:

![Pixel bity ukázky](pixel-bits-images/PixelBitsSample.png "Pixel bity vzorku")

## <a name="the-techniques"></a>Techniky

Ve Skiasharpu poskytuje několik metod pro přístup k rastrové bity pixelů. Která z nich se rozhodnete je obvykle to kompromis mezi kódování výkon a pohodlí, (která týkající se údržby a snadné ladění). Ve většině případů budete používat jeden z následujících metod a vlastností `SKBitmap` pro přístup k rastrového obrázku v pixelech:

- `GetPixel` a `SetPixel` metody umožňují získat nebo nastavit barvu jeden pixel.
- `Pixels` Vlastnost získá pole barev pixelů pro celý rastrový obrázek nebo nastaví pole barev.
- `GetPixels` Vrátí adresu pixel paměť používanou rastrového obrázku.
- `SetPixels` Nahradí adresu pixel paměť používanou rastrového obrázku.

Můžete si představit první dvě techniky jako "vysokou úroveň" a další dva jako "nízké úrovni." Existují některé jiné metody a vlastnosti, které můžete použít, ale toto jsou nejvíc hodí v situaci.

Aby bylo možné rozdíly ve výkonu mezi tyto postupy, najdete v článku [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) aplikace obsahuje stránku s názvem **přechodu rastrový obrázek** , který Vytvoří rastrový obrázek s pixelů, které kombinují odstínech červené a modré k vytvoření přechodu. Program vytvoří osm jiné kopie bitmapy, všechny pomocí různých technik pro nastavení pixelů rastrového obrázku. Každá z těchto osm rastrové obrázky se vytvoří v samostatné metodě, která také nastavuje stručný textový popis techniky a vypočítá dobu potřebnou k nastavení pixelech. Každá metoda prochází logiku nastavení pixelů 100krát získat lepší odhad výkonu. 

### <a name="the-setpixel-method"></a>SetPixel – metoda

Pokud potřebujete jenom chcete nastavit nebo získat několika jednotlivých pixelech [ `SetPixel` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.SetPixel/p/System.Int32/System.Int32/SkiaSharp.SKColor/) a [ `GetPixel` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.GetPixel/p/System.Int32/System.Int32/) metody jsou ideální. Pro každou z těchto dvou metod zadejte celé číslo sloupců a řádků. Bez ohledu na formát pixelu, tyto dvě metody umožňují získat nebo nastavit pixel jako `SKColor` hodnotu:

```csharp
bitmap.SetPixel(col, row, color);

SKColor color = bitmap.GetPixel(col, row);
```

`col` Argument musí být v rozsahu od 0 do jednoho menší než `Width` vlastností rastrového obrázku, a `row` rozsahu 0 až jeden menší než `Height` vlastnost.

Tady je metoda **přechodu rastrový obrázek** , který nastavuje obsah rastrového obrázku pomocí `SetPixel` metody. Rastrový obrázek je 256 x 256 pixelů a `for` smyčky jsou pevně zakódovaná s rozsah hodnot:

```csharp
public class GradientBitmapPage : ContentPage
{
    const int REPS = 100;
        
    Stopwatch stopwatch = new Stopwatch();
    ···
    SKBitmap FillBitmapSetPixel(out string description, out int milliseconds)
    {
        description = "SetPixel";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        for (int rep = 0; rep < REPS; rep++)
            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    bitmap.SetPixel(col, row, new SKColor((byte)col, 0, (byte)row));
                }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
}
```

Barva nastavena pro každý pixel má červené rovno rastrový obrázek sloupce a modré rovná na řádek. Výsledná rastrového obrázku je v levém horním rohu, v pravém horním rohu, v levém dolním a purpurová v pravém dolním, s přechody jinde modré red černá.

`SetPixel` Metody volané 65 536 časy a bez ohledu na to jak efektivní může být tato metoda je není obecně vhodné, aby daný počet volání rozhraní API, když je k dispozici alternativní. Naštěstí se několik alternativ.

### <a name="the-pixels-property"></a>Vlastnost pixelů

`SKBitmap` definuje [ `Pixels` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Pixels/) vlastnost, která vrací pole `SKColor` hodnoty pro celý rastrový obrázek. Můžete také použít `Pixels` nastavit pole hodnot barvy rastrového obrázku:

```csharp
SKColor[] pixels = bitmap.Pixels;

bitmap.Pixels = pixels;
```

Pixely jsou uspořádány do pole, počínaje první řádek zleva doprava, pak druhém řádku a tak dále. Celkový počet barev v poli je roven součin rastrový obrázek šířku a výšku.

I když tato vlastnost se zobrazí účinný, mějte na paměti, obrazových bodů jsou kopírovány z rastrový obrázek do pole a pole zpět do rastrový obrázek, který pixely převedou od a do `SKColor` hodnoty.

Tady je metoda `GradientBitmapPage` třídu, která nastaví pomocí rastrového obrázku `Pixels` vlastnost. Metoda přidělí `SKColor` pole požadovaná velikost, ale mohli použít `Pixels` vlastnost k vytvoření takové pole:

```csharp
SKBitmap FillBitmapPixelsProp(out string description, out int milliseconds)
{
    description = "Pixels property";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    SKColor[] pixels = new SKColor[256 * 256]; 

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                pixels[256 * row + col] = new SKColor((byte)col, 0, (byte)row);
            }

    bitmap.Pixels = pixels;

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Všimněte si, že index `pixels` pole je potřeba vypočítat z `row` a `col` proměnné. Řádek se násobí hodnotou počet pixelů na každém řádku (256 v tomto případě) a pak se má přidat sloupec.

`SKBitmap` Definuje také podobná `Bytes` vlastnost, která vrací pole bajtů pro celý rastrový obrázek, ale je těžkopádnější pro barevné bitmapy.

### <a name="the-getpixels-pointer"></a>Ukazatel GetPixels

Potenciálně procesorově nejvýkonnější techniku pro přístup k pixelů rastrového obrázku je [ `GetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.GetPixels()/), neměl by se zaměňovat s `GetPixel` metoda nebo `Pixels` vlastnost. Okamžitě uvidíte rozdíl oproti `GetPixels` , vrátí se něco není velmi běžné v programování v jazyce C#:

```csharp
IntPtr pixelsAddr = bitmap.GetPixels();
```

.NET [ `IntPtr` ](xref:System.IntPtr) reprezentuje typ ukazatele. Je volána `IntPtr` vzhledem k tomu, že je délka celého čísla na nativní procesoru počítače, ve které program běží, obvykle 32 bity a 64 bitů v délce. `IntPtr` , Který `GetPixels` vrátí je adresa skutečné bloku paměti, která objekt bitmap používá k ukládání svých pixelů. 

Můžete převést `IntPtr` do jazyka C# ukazatele typu s použitím [ `ToPointer` ](xref:System.IntPtr.ToPointer) metody. Syntaxe jazyka C# ukazatel je stejný jako C a C++:

```csharp
byte* ptr = (byte*)pixelsAddr.ToPointer();
```

`ptr` Je proměnná typu _bajtů ukazatel_. To `ptr` proměnná umožňuje přístup k jednotlivým počet bajtů paměti, které se používají k ukládání rastrového obrázku v pixelech. Pomocí kódu tímto způsobem můžete číst bajt tuto paměť nebo zápis bajtů do paměti:

```sharp
byte pixelComponent = *ptr;

*ptr = pixelComponent;
```

V tomto kontextu je hvězdička jazyka C# _operátor dereference_ a slouží jako odkaz obsah paměti, na které odkazuje `ptr`. Na začátku `ptr` body do prvního bajtu první pixelu prvního řádku rastrový obrázek, ale můžete provádět aritmetické operace na `ptr` proměnnou přesunout do jiných umístění v rámci rastrového obrázku.

Jednou z nevýhod je, že může být využit `ptr` označené proměnnou pouze v bloku kódu `unsafe` – klíčové slovo. Kromě toho sestavení musí mít příznak povolení nebezpečné bloky. To se provádí ve vlastnostech projektu.

Použití ukazatele v jazyce C# je velmi výkonné, ale také velmi nebezpečné. Musíte mít na paměti, že není přístup k paměti nad rámec ukazatel má odkazovat. To je důvod, proč použití ukazatele je spojen s aplikací word "nebezpečného".

Tady je metoda `GradientBitmapPage` třídu, která používá `GetPixels` metody. Všimněte si, že `unsafe` blok, který zahrnuje všechny kódu pomocí ukazatele bajtů:

```csharp
SKBitmap FillBitmapBytePtr(out string description, out int milliseconds)
{
    description = "GetPixels byte ptr";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            byte* ptr = (byte*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (byte)(col);   // red
                    *ptr++ = 0;             // green
                    *ptr++ = (byte)(row);   // blue
                    *ptr++ = 0xFF;          // alpha
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Při `ptr` proměnná je nejprve získaných `ToPointer` metodě odkazuje na první bajt je úplně vlevo pixel prvního řádku rastrového obrázku. `for` Smyčky pro `row` a `col` jsou nastavené tak, aby `ptr` může být zvýšena pomocí `++` operátor po nastavení všech bajtů každý pixel. Pro další 99 cyklicky projde pixelů `ptr` musí být nastavena zpět na začátek rastrového obrázku.

Každý pixel je čtyř bajtů paměti, takže každý bajt musí být nastavena samostatně. Kód předpokládá, že počet bajtů v pořadí červená, zelená, modrá a alfa, který je konzistentní s `SKColorType.Rgba8888` barva typu. Možná si Vzpomínáte, že se jedná o výchozí typ barvy pro iOS a Android, ale ne pro univerzální platformu Windows. Ve výchozím nastavení vytvoří UPW rastrové obrázky s `SKColorType.Bgra8888` barva typu. Z tohoto důvodu očekáváte mírně odlišné výsledky na této platformě!

Je možné převést hodnotu vrácenou z `ToPointer` k `uint` ukazatel spíše než `byte` ukazatele. To umožňuje celý pixel přistupovat v jednom příkazu. Použití `++` operátor má tento ukazatel se zvýší o čtyři bajty, aby ukazoval na další bod:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    SKBitmap FillBitmapUintPtr(out string description, out int milliseconds)
    {
        description = "GetPixels uint ptr";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        IntPtr pixelsAddr = bitmap.GetPixels();

        unsafe
        {
            for (int rep = 0; rep < REPS; rep++)
            {
                uint* ptr = (uint*)pixelsAddr.ToPointer();

                for (int row = 0; row < 256; row++)
                    for (int col = 0; col < 256; col++)
                    {
                        *ptr++ = MakePixel((byte)col, 0, (byte)row, 0xFF);
                    }
            }
        }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    uint MakePixel(byte red, byte green, byte blue, byte alpha) =>
            (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

Je pixel se nastavuje pomocí `MakePixel` metodu, která vytvoří pixel celé číslo z komponenty červené, zelená, modrá a alfa. Mějte na paměti, která `SKColorType.Rgba8888` formát má pixel bajtů řazení takto:

RR GG BB AA

Ale je celé číslo odpovídající těchto bajtů:

AABBGGRR

Nejméně významný bajt na celé číslo je uložen v souladu s architekturou little endian. To `MakePixel` metoda nebude fungovat správně při použití rastrové obrázky s `Bgra8888` barva typu.

`MakePixel` Metoda je označena příznakem s [ `MethodImplOptions.AggressiveInlining` ](xref:System.Runtime.CompilerServices.MethodImplOptions) – možnost kompilátoru předejděte tomu, aby to samostatné metodě, ale místo toho pro kompilaci kódu ve kterém je volána metoda podporovat. To by měl zvýšit výkon.

Zajímavé `SKColor` struktury definuje explicitní převod z `SKColor` na celé číslo bez znaménka, což znamená, že `SKColor` hodnoty mohou být vytvořeny a převod na `uint` lze použít místo `MakePixel`:

```csharp
SKBitmap FillBitmapUintPtrColor(out string description, out int milliseconds)
{
    description = "GetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            uint* ptr = (uint*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (uint)new SKColor((byte)col, 0, (byte)row);
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

To je jediný dotaz: je formát celé číslo `SKColor` hodnoty v pořadí podle `SKColorType.Rgba8888` barva typ, nebo `SKColorType.Bgra8888` barva typ nebo je to něco úplně? Odpověď na tuto otázku se ukázalo za chvíli.

### <a name="the-setpixels-method"></a>SetPixels – metoda

`SKBitmap` také definuje metodu s názvem [ `SetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.SetPixels/p/System.IntPtr/), kterou zavoláte takto:

```csharp
bitmap.SetPixels(intPtr);
```

Vzpomeňte si, že `GetPixels` získá `IntPtr` odkazování na blok paměti používané rastrový obrázek pro uložení jeho pixelů. `SetPixels` Volání _nahradí_ tento blok paměti s bloku paměti odkazuje `IntPtr` zadané jako `SetPixels` argument. Rastrový obrázek následně uvolní blok paměti, která používal dříve. Při příštím `GetPixels` je volána, získá bloku paměti sadu s `SetPixels`.

Zpočátku to vypadá jako `SetPixels` žádné další napájení a výkon než `GetPixels` při zachování méně vhodné. S `GetPixels` získat blok paměti rastrového obrázku a k němu přístup. S `SetPixels` přidělit a přístup k paměti a nastavte ji jako blok paměti rastrového obrázku.

Ale pomocí `SetPixels` nabízí různé výhody syntaktické: umožňuje přístup k rastrové bity pixel použití pole. Tady je metoda `GradientBitmapPage` , kde uvidíte tento postup. Metoda nejdřív definuje multidimenzionální bajtové pole odpovídající počet bajtů rastrového obrázku v pixelech. První dimenze je řádek je druhého rozměru sloupci a corresonds třetího rozměru do čtyř komponent každý pixel:

```csharp
SKBitmap FillBitmapByteBuffer(out string description, out int milliseconds)
{
    description = "SetPixels byte buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    byte[,,] buffer = new byte[256, 256, 4];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col, 0] = (byte)col;   // red
                buffer[row, col, 1] = 0;           // green
                buffer[row, col, 2] = (byte)row;   // blue
                buffer[row, col, 3] = 0xFF;        // alpha
            }
    
    unsafe
    {
        fixed (byte* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Potom po bylo vyplněno pole s pixelů, `unsafe` bloku a `fixed` prohlášení se používá k získání bajtů ukazatele odkazující na toto pole. Tento ukazatel bajtů potom můžete přetypovat na `IntPtr` předat `SetPixels`.

Pole, které vytvoříte nemusí být bajtové pole. Může se jednat celé číslo dvourozměrné pole pouze pro řádek a sloupec:

```csharp
SKBitmap FillBitmapUintBuffer(out string description, out int milliseconds)
{
    description = "SetPixels uint buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = MakePixel((byte)col, 0, (byte)row, 0xFF);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

`MakePixel` Metoda znovu umožňuje kombinovat barevných složek do 32-bit pixelů.

Jenom pro úplnost je zde stejný kód, ale `SKColor` hodnota přetypována na celé číslo bez znaménka:

```csharp
SKBitmap FillBitmapUintBufferColor(out string description, out int milliseconds)
{
    description = "SetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = (uint)new SKColor((byte)col, 0, (byte)row);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

### <a name="comparing-the-techniques"></a>Porovnání metod

Konstruktor třídy **barevný přechod** stránky volá všechny osm metody uvedené výše a uloží výsledky:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    string[] descriptions = new string[8];
    SKBitmap[] bitmaps = new SKBitmap[8];
    int[] elapsedTimes = new int[8];

    SKCanvasView canvasView;

    public GradientBitmapPage ()
    {
        Title = "Gradient Bitmap";

        bitmaps[0] = FillBitmapSetPixel(out descriptions[0], out elapsedTimes[0]);
        bitmaps[1] = FillBitmapPixelsProp(out descriptions[1], out elapsedTimes[1]);
        bitmaps[2] = FillBitmapBytePtr(out descriptions[2], out elapsedTimes[2]);
        bitmaps[4] = FillBitmapUintPtr(out descriptions[4], out elapsedTimes[4]);
        bitmaps[6] = FillBitmapUintPtrColor(out descriptions[6], out elapsedTimes[6]);
        bitmaps[3] = FillBitmapByteBuffer(out descriptions[3], out elapsedTimes[3]);
        bitmaps[5] = FillBitmapUintBuffer(out descriptions[5], out elapsedTimes[5]);
        bitmaps[7] = FillBitmapUintBufferColor(out descriptions[7], out elapsedTimes[7]);

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

Konstruktor končí tím, že vytvoříte `SKCanvasView` zobrazíte výsledná rastrových obrázků. `PaintSurface` Obslužná rutina jeho plochu rozdělí osm obdélníků a volání `Display` zobrazíte každé z nich:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        int width = info.Width;
        int height = info.Height;

        canvas.Clear();

        Display(canvas, 0, new SKRect(0, 0, width / 2, height / 4));
        Display(canvas, 1, new SKRect(width / 2, 0, width, height / 4));
        Display(canvas, 2, new SKRect(0, height / 4, width / 2, 2 * height / 4));
        Display(canvas, 3, new SKRect(width / 2, height / 4, width, 2 * height / 4));
        Display(canvas, 4, new SKRect(0, 2 * height / 4, width / 2, 3 * height / 4));
        Display(canvas, 5, new SKRect(width / 2, 2 * height / 4, width, 3 * height / 4));
        Display(canvas, 6, new SKRect(0, 3 * height / 4, width / 2, height));
        Display(canvas, 7, new SKRect(width / 2, 3 * height / 4, width, height));
    }

    void Display(SKCanvas canvas, int index, SKRect rect)
    {
        string text = String.Format("{0}: {1:F1} msec", descriptions[index], 
                                    (double)elapsedTimes[index] / REPS);

        SKRect bounds = new SKRect();

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.TextSize = (float)(12 * canvasView.CanvasSize.Width / canvasView.Width);
            textPaint.TextAlign = SKTextAlign.Center;
            textPaint.MeasureText("Tly", ref bounds);

            canvas.DrawText(text, new SKPoint(rect.MidX, rect.Bottom - bounds.Bottom), textPaint);
            rect.Bottom -= bounds.Height;
            canvas.DrawBitmap(bitmaps[index], rect, BitmapStretch.Uniform);
        }
    }
}
```

Pokud chcete povolit, aby kompilátor optimalizoval kód, tato stránka spouštěl v **vydání** režimu. Tady je této stránky, které běží na simulátor pro zařízení iPhone 8 na MacBook Pro, telefonu s Androidem 5 Nexus a Surface Pro 3 s Windows 10. Vzhledem k rozdílům hardwaru vyhnout porovnání doby výkonu mezi zařízeními, ale místo toho podívejte se na relativní časy na každé zařízení:

[![Rastrový obrázek přechodu](pixel-bits-images/GradientBitmap.png "přechodu rastrový obrázek")](pixel-bits-images/GradientBitmap-Large.png#lightbox)

Tady je tabulka, která se slučuje časy spuštění v milisekundách:

| rozhraní API       | Datový typ | iOS  | Android | UWP  |
| --------- | --------- | ----:| -------:| ----:|
| SetPixel  |           | 3.17 |   10.77 | 3.49 |
| Pixelů    |           | 0.32 |    1,23 | 0,07 |
| GetPixels | byte      | 0,09 |    0.24 | 0.10 |
|           | uint      | 0,06 |    0.26 | 0.05 |
|           | SKColor   | 0.29 |    0,99. | 0,07 |
| SetPixels | byte      | 1.33 |    6.78 | 0,11 |
|           | uint      | 0.14 |    0.69 | 0,06 |
|           | SKColor   | 0.35 |    1.93 | 0.10 |

Podle očekávání, volání `SetPixel` 65 536 doby je nejnižší effeicient způsob, jak nastavit rastrového obrázku v pixelech. Vyplnění `SKColor` pole a nastavení `Pixels` vlastnost je mnohem lepší a dokonce i příznivě porovná s některými `GetPixels` a `SetPixels` techniky. Práce s `uint` pixel hodnoty je obecně rychlejší než samostatné nastavení `byte` komponenty a převod `SKColor` hodnotu na celé číslo bez znaménka přidává režijní náklady na proces.

Je také zajímavé pro porovnání různých přechody: horní řádky všech třech platformách jsou stejné a zobrazit přechodu podle očekávání. To znamená, že `SetPixel` metoda a `Pixels` vlastnost správně vytvořit pixelů od barvy bez ohledu na základní formát pixelu.

Následující dva řádky zařízení s Iosem a Androidem snímky obrazovky jsou také stejné, který potvrdí, že trochu `MakePixel` správně definována metoda pro výchozí `Rgba8888` formát pixelu pro tyto platformy.

Dolní řádek zařízení s Iosem a Androidem snímky obrazovky je zpět, což znamená, že celé číslo bez znaménka získali přetypováním `SKColor` hodnotu ve formátu:

AARRGGBB

Bajty jsou v uvedeném pořadí:

AA BB GG ZÁZNAM O PROSTŘEDKU

Toto je `Bgra8888` řazení místo `Rgba8888` řazení. `Brga8888` Formát je výchozí nastavení pro platformu Universal Windows, což je důvod, proč přechody na poslední řádek tohoto snímku obrazovky je stejný jako první řádek. Střední dva řádky jsou nesprávná, protože předpokládá, že kód vytváření těchto rastrové obrázky, ale `Rgba8888` řazení.

Pokud chcete použít stejný kód pro přístup k bitů na pixel na všech třech platformách, můžete explicitně vytvořit `SKBitmap` buď pomocí `Rgba8888` nebo `Bgra8888` formátu. Pokud chcete přetypovat `SKColor` hodnoty na rastrový obrázek pixelů, použijte `Bgra8888`.

## <a name="random-access-of-pixels"></a>Náhodný přístup v pixelech

`FillBitmapBytePtr` a `FillBitmapUintPtr` metody v **přechodu rastrový obrázek** stránky využívali `for` smyček, které jsou navržené tak, aby postupně, zadejte rastrový obrázek z horní řádek do dolní řádek a v jednotlivých řádcích zleva doprava. Je pixel se dají nastavit příkazem zvýšen ukazatel. 

Někdy je potřeba přístup k pixely náhodně postupně. Pokud používáte `GetPixels` přístup, budete potřebovat k výpočtu ukazatelů podle řádků a sloupců. To je patrné **Rainbow sinus** stránky, která vytvoří rastrový obrázek znázorňující rainbow ve formě jednoho cyklu sinus křivky. 

Barvy rainbow je nejjednodušší vytvořit pomocí modelu barva HSL (odstín, sytost, světelnost). `SKColor.FromHsl` Metoda vytvoří `SKColor` hodnotu pomocí hue hodnoty, které v rozsahu od 0 do 360 (např. jedná o úhly kruh, ale přechod z červené, zelené a modré a zpět na hodnotu red) a přechod sytosti a světlosti hodnoty od 0 do 100. Pro barvy rainbow je třeba nastavit sytost na maximálně 100 a světelnost na střední 50.

**Rainbow sinus** tak, že opakování ve smyčce prostřednictvím řádky rastrového obrázku a potom ve smyčce 360 hodnoty odstín, který vytvoří tuto bitovou kopii. Od každé hodnoty odstín, který vypočítá sloupec rastrový obrázek, který je taky založený na hodnotu sinus:

```csharp
public class RainbowSinePage : ContentPage
{
    SKBitmap bitmap;

    public RainbowSinePage()
    {
        Title = "Rainbow Sine";

        bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);

        unsafe
        {
            // Pointer to first pixel of bitmap
            uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();

            // Loop through the rows
            for (int row = 0; row < bitmap.Height; row++)
            {
                // Calculate the sine curve angle and the sine value
                double angle = 2 * Math.PI * row / bitmap.Height;
                double sine = Math.Sin(angle);

                // Loop through the hues
                for (int hue = 0; hue < 360; hue++)
                {
                    // Calculate the column
                    int col = (int)(360 + 360 * sine + hue);

                    // Calculate the address
                    uint* ptr = basePtr + bitmap.Width * row + col;

                    // Store the color value
                    *ptr = (uint)SKColor.FromHsl(hue, 100, 50);
                }
            }
        }

        // Create the SKCanvasView
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
        canvas.DrawBitmap(bitmap, info.Rect);
    }
}
```

Všimněte si, že konstruktor vytvoří na základě rastrového obrázku `SKColorType.Bgra8888` formátu:

```csharp
bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);
```

To umožňuje programu používat převodu `SKColor` hodnoty do `uint` pixelů, bez obav. I když to není hrají roli v tomto konkrétním programu, vždy, když použijete `SKColor` převodu nastavení pixelů, byste zadat také `SKAlphaType.Unpremul` protože `SKColor` nepodporuje provedení operace premultiply součásti barvu podle hodnoty alfa.

Poté použije konstruktoru `GetPixels` metoda ukazatel na první pixel rastrového obrázku:

```csharp
uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();
```

Pro jakékoli konkrétní řádků a sloupců musí být přidané do hodnoty posunu `basePtr`. Tento posun je řádků krát šířka rastrového obrázku plus sloupci:

```csharp
uint* ptr = basePtr + bitmap.Width * row + col;
```

`SKColor` Hodnota je uložená v paměti pomocí tohoto ukazatele:

```csharp
*ptr = (uint)SKColor.FromHsl(hue, 100, 50);
```

V `PaintSurface` obslužná rutina `SKCanvasView`, rastrový obrázek je roztažen tak, aby vyplnění oblasti zobrazení:

[![Rainbow sinus](pixel-bits-images/RainbowSine.png "Rainbow sinus")](pixel-bits-images/RainbowSine-Large.png#lightbox)

## <a name="from-one-bitmap-to-another"></a>Z jednoho rastrového obrázku na jiný

Velmi mnoho úloh zpracování obrázků zahrnují úpravy pixelů podle přenášených z jednoho rastrového obrázku na jiný. Tato technika je patrné **úpravy barev** stránky. Na stránce načte jeden z prostředků rastrový obrázek a potom vám umožní změnit image pomocí tří `Slider` zobrazení:

[![Barva úpravy](pixel-bits-images/ColorAdjustment.png "barva úpravy")](pixel-bits-images/ColorAdjustment-Large.png#lightbox)

Pro každou barvu pixelu první `Slider` přidá hodnotu od 0 do 360 odstín, ale použije operátor zachovat výsledek rozmezí od 0 do 360 modulo efektivně posunutí barev podél spektra (jak ukazuje snímek obrazovky UPW). Druhá `Slider` lze vybrat násobení faktor mezi 0.5 a 2 má použít pro sytost a třetí `Slider` dělá to samé pro světelnost, jak je ukázáno v Androidu snímku obrazovky.

Program udržuje dvě rastrové obrázky, původní zdrojovou bitmapu s názvem `srcBitmap` a upravené cílovou bitmapu s názvem `dstBitmap`. Pokaždé, když `Slider` se přesune, program vypočítá všechny nové pixelů `dstBitmap`. Samozřejmě, budou uživatelé experimentovat tak, `Slider` zobrazení velmi rychle, takže mají nejlepší výkon, můžete spravovat. To zahrnuje `GetPixels` metodu pro zdrojové i cílové bitmapy. 

**Úpravy barev** stránky nemá pod kontrolou formát barev zdrojové a cílové bitmapy. Místo toho obsahuje mírně odlišné logiku pro `SKColorType.Rgba8888` a `SKColorType.Bgra8888` formátů. Zdroj a cíl mohou být různých formátech a program bude i nadále fungovat.

Tady je program s výjimkou zásadní `TransferPixels` metody, které se přenese pixelech tvoří zdroje do cíle. Nastaví konstruktor `dstBitmap` rovna `srcBitmap`. `PaintSurface` Obslužná rutina zobrazí `dstBitmap`:

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    SKBitmap srcBitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    SKBitmap dstBitmap;

    public ColorAdjustmentPage()
    {
        InitializeComponent();

        dstBitmap = new SKBitmap(srcBitmap.Width, srcBitmap.Height);
        OnSliderValueChanged(null, null);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        float hueAdjust = (float)hueSlider.Value;
        hueLabel.Text = $"Hue Adjustment: {hueAdjust:F0}";

        float saturationAdjust = (float)Math.Pow(2, saturationSlider.Value);
        saturationLabel.Text = $"Saturation Adjustment: {saturationAdjust:F2}";

        float luminosityAdjust = (float)Math.Pow(2, luminositySlider.Value);
        luminosityLabel.Text = $"Luminosity Adjustment: {luminosityAdjust:F2}";

        TransferPixels(hueAdjust, saturationAdjust, luminosityAdjust);
        canvasView.InvalidateSurface();
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(dstBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

`ValueChanged` Obslužné rutiny pro `Slider` zobrazení vypočítá hodnoty úpravy a volání `TransferPixels`.

Celý `TransferPixels` metoda je označena jako `unsafe`. Začíná získáním bajtů odkazy na bity pixel obou rastrových obrázků a potom prochází všechny řádky a sloupce. Metoda z zdrojovou bitmapu získá čtyř bajtů pro každý pixel. Může se jednat buď `Rgba8888` nebo `Bgra8888` pořadí. Kontroluje se pro typ barvy umožňuje `SKColor` hodnota, která má být vytvořen. HSL komponenty jsou pak extrahovat, upravit a použít k rekonstrukci `SKColor` hodnotu. V závislosti na tom, jestli je cílovou bitmapu `Rgba8888` nebo `Bgra8888`, bajty jsou uloženy v cílovém bitmp:

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    ···
    unsafe void TransferPixels(float hueAdjust, float saturationAdjust, float luminosityAdjust)
    {
        byte* srcPtr = (byte*)srcBitmap.GetPixels().ToPointer();
        byte* dstPtr = (byte*)dstBitmap.GetPixels().ToPointer();

        int width = srcBitmap.Width;       // same for both bitmaps
        int height = srcBitmap.Height;

        SKColorType typeOrg = srcBitmap.ColorType;
        SKColorType typeAdj = dstBitmap.ColorType;

        for (int row = 0; row < height; row++)
        {
            for (int col = 0; col < width; col++)
            {
                // Get color from original bitmap
                byte byte1 = *srcPtr++;         // red or blue
                byte byte2 = *srcPtr++;         // green
                byte byte3 = *srcPtr++;         // blue or red
                byte byte4 = *srcPtr++;         // alpha

                SKColor color = new SKColor();

                if (typeOrg == SKColorType.Rgba8888)
                {
                    color = new SKColor(byte1, byte2, byte3, byte4);
                }
                else if (typeOrg == SKColorType.Bgra8888)
                {
                    color = new SKColor(byte3, byte2, byte1, byte4);
                }

                // Get HSL components
                color.ToHsl(out float hue, out float saturation, out float luminosity);

                // Adjust HSL components based on adjustments
                hue = (hue + hueAdjust) % 360;
                saturation = Math.Max(0, Math.Min(100, saturationAdjust * saturation));
                luminosity = Math.Max(0, Math.Min(100, luminosityAdjust * luminosity));

                // Recreate color from HSL components
                color = SKColor.FromHsl(hue, saturation, luminosity);

                // Store the bytes in the adjusted bitmap
                if (typeAdj == SKColorType.Rgba8888)
                {
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Alpha;
                }
                else if (typeAdj == SKColorType.Bgra8888)
                {
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Alpha;
                }
            }
        }
    }
    ···
}
```

Je pravděpodobné, že výkon této metody lze ještě více zlepšit vytvořením samostatné metody pro různé kombinace typů barva zdrojové a cílové bitmapy a vyhněte se kontrola typu pro každý pixel. Další možností je, aby více `for` smyčky pro `col` proměnná založená na typ barvy.

## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
