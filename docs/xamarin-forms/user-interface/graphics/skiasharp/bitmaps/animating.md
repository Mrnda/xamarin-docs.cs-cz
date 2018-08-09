---
title: Animace rastrové obrázky ve Skiasharpu
description: Zjistěte, jak k provádění animace rastrový obrázek postupně zobrazením řadu rastrové obrázky a vykreslování animované soubory formátu GIF.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD
author: charlespetzold
ms.author: chape
ms.date: 07/12/2018
ms.openlocfilehash: 45a009757d84aa98acc41f6cd2bf672c8472c5bb
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615574"
---
# <a name="animating-skiasharp-bitmaps"></a>Animace rastrové obrázky ve Skiasharpu

Aplikace, které obecně animace grafiky ve Skiasharpu volat `InvalidateSurface` na `SKCanvasView` s pevnou sazbou, často každých 16 milisekund. Zrušení platnosti na plochu aktivuje volání `PaintSurface` obslužné rutiny pro překreslení zobrazení. Jak vizuály se měl překreslit dobu 60 sekund, aby se zobrazily jako animovat plynule.

Nicméně pokud grafiky jsou příliš složité mohl být vykreslen v milisekundách 16, animace může stát nestabilní. Programátor rozhodnout, že ke snížení frekvence obnovování 30krát nebo časy 15 sekund, ale někdy dokonce, který není dostatečné. Někdy grafiky jsou tak komplikovaná, že se jednoduše nemůže být vykreslen v reálném čase.

Jedním z řešení je předem připravit pro animaci vykreslením jednotlivé snímky animace v řadě rastrových obrázků. Animace zobrazíte je jenom nutné zobrazit tyto bitmapy postupně dobu 60 sekund. 

Samozřejmě, která je potenciálně velké rastrové obrázky, ale to je jak velký objem rozpočtu 3D animovaných filmů probíhají. 3D grafiky jsou mnohem příliš složité mohl být vykreslen v reálném čase. Mnoho času zpracování je potřeba vykreslit každém snímku. Co se zobrazí při sledování videa je v podstatě řada rastrových obrázků.

Můžete udělat něco podobného v SkiaSharp. Tento článek ukazuje dva druhy animace rastrového obrázku. První příklad je animace Mandelbrot sady:

![Animace ukázka](animating-images/AnimatingSample.png "animace vzorku")

Druhý příklad ukazuje, jak ve Skiasharpu použitých k vykreslování animovaný GIF.

## <a name="bitmap-animation"></a>Animace rastrový obrázek

Sada Mandelbrot je vizuálně fascinující, ale computionally zdlouhavé. (Informace o sadě Mandelbrot a matematiku se tady použít, najdete v článku [kapitole 20 _vytváření mobilních aplikací pomocí Xamarin.Forms_ ](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf) začínající na stránce 666. Následující popis předpokládá, že základní znalosti.)

[ **Mandelbrot animace** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/MandelAnima/) Ukázka používá k simulaci průběžné přiblížení pevnému bodu v sadě Mandelbrot animace rastrového obrázku. Přiblížit následuje oddálení, a potom cyklus se opakuje navždy nebo jsou až do ukončení programu. 

Program připraví této animace tak, že vytvoříte až 50 rastrové obrázky, které jsou uloženy v místním úložišti aplikace. Každý rastrový obrázek zahrnuje poloviční šířku a výšku plochy komplexní jako předchozí rastrový obrázek. (V programu, tyto rastrové obrázky se říká, že představují integral _úrovně přiblížení_.) Rastrové obrázky se zobrazí v pořadí. Škálování jednotlivých rastrového obrázku je animovaný zajistit hladký průběh z jednoho rastrového obrázku na jiný.

Konečný program je popsáno v kapitole 20, jako jsou _vytváření mobilních aplikací pomocí Xamarin.Forms_, výpočtu sada Mandelbrot v **Mandelbrot animace** je asynchronní metodu s 8 Parametry. Parametry patří komplexní středový bod a šířku a výšku plochy složité kolem tohoto středový bod. Následující tři parametry jsou pixel šířku a výšku rastrového obrázku, který se má vytvořit a maximální počet iterací pro výpočet rekurzivní. `progress` Parametr se používá k zobrazení průběhu tento výpočet. `cancelToken` Parametr se nepoužívá v rámci tohoto programu:

```csharp
static class Mandelbrot
{
    public static Task<BitmapInfo> CalculateAsync(Complex center,
                                                  double width, double height,
                                                  int pixelWidth, int pixelHeight,
                                                  int iterations,
                                                  IProgress<double> progress,
                                                  CancellationToken cancelToken)
    {
        return Task.Run(() =>
        {
            int[] iterationCounts = new int[pixelWidth * pixelHeight];
            int index = 0;

            for (int row = 0; row < pixelHeight; row++)
            {
                progress.Report((double)row / pixelHeight);
                cancelToken.ThrowIfCancellationRequested();

                double y = center.Imaginary + height / 2 - row * height / pixelHeight;

                for (int col = 0; col < pixelWidth; col++)
                {
                    double x = center.Real - width / 2 + col * width / pixelWidth;
                    Complex c = new Complex(x, y);

                    if ((c - new Complex(-1, 0)).Magnitude < 1.0 / 4)
                    {
                        iterationCounts[index++] = -1;
                    }
                    // http://www.reenigne.org/blog/algorithm-for-mandelbrot-cardioid/
                    else if (c.Magnitude * c.Magnitude * (8 * c.Magnitude * c.Magnitude - 3) < 3.0 / 32 - c.Real)
                    {
                        iterationCounts[index++] = -1;
                    }
                    else
                    {
                        Complex z = 0;
                        int iteration = 0;

                        do
                        {
                            z = z * z + c;
                            iteration++;
                        }
                        while (iteration < iterations && z.Magnitude < 2);

                        if (iteration == iterations)
                        {
                            iterationCounts[index++] = -1;
                        }
                        else
                        {
                            iterationCounts[index++] = iteration;
                        }
                    }
                }
            }
            return new BitmapInfo(pixelWidth, pixelHeight, iterationCounts);
        }, cancelToken);
    }
}
```

Metoda vrátí objekt typu `BitmapInfo` , který obsahuje informace o vytváření rastrový obrázek:

```csharp
class BitmapInfo
{
    public BitmapInfo(int pixelWidth, int pixelHeight, int[] iterationCounts)
    {
        PixelWidth = pixelWidth;
        PixelHeight = pixelHeight;
        IterationCounts = iterationCounts;
    }

    public int PixelWidth { private set; get; }

    public int PixelHeight { private set; get; }

    public int[] IterationCounts { private set; get; }
}
```

**Mandelbrot animace** soubor XAML obsahuje dvě `Label` zobrazení, `ProgressBar`a `Button` také `SKCanvasView`:

```csharp
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="MandelAnima.MainPage"
             Title="Mandelbrot Animation">

    <StackLayout>
        <Label x:Name="statusLabel"
               HorizontalTextAlignment="Center" />
        <ProgressBar x:Name="progressBar" />

        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     Padding="5">
            <Label x:Name="storageLabel"
                   VerticalOptions="Center" />

            <Button x:Name="deleteButton"
                    Text="Delete All"
                    HorizontalOptions="EndAndExpand" 
                    Clicked="OnDeleteButtonClicked" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Soubor kódu na pozadí začíná tak, že definujete tři zásadní konstanty a pole rastrových obrázků:

```csharp
public partial class MainPage : ContentPage
{
    const int COUNT = 10;           // The number of bitmaps in the animation.
                                    // This can go up to 50!

    const int BITMAP_SIZE = 1000;   // Program uses square bitmaps exclusively

    // Uncomment just one of these, or define your own
    static readonly Complex center = new Complex(-1.17651152924355, 0.298520986549558);
    //   static readonly Complex center = new Complex(-0.774693089457127, 0.124226621261617);
    //   static readonly Complex center = new Complex(-0.556624880053304, 0.634696788141351);

    SKBitmap[] bitmaps = new SKBitmap[COUNT];   // array of bitmaps
    ···
}
```

V určitém okamžiku budete pravděpodobně chtít změnit `COUNT` hodnotu 50, chcete-li zobrazit celou škálu animace. Hodnoty větší než 50 nejsou užitečné. Kolem úroveň přiblížení 48 nebo tak stane dostatečná pro výpočet Mandelbrot sada řešení dvojité přesnosti s plovoucí desetinnou čárkou čísla. Tento problém je popsáno na stránce 684 _vytváření mobilních aplikací pomocí Xamarin.Forms_.

`center` Hodnota je velmi důležité. Toto je výběr zvětšení animace. Jsou tři hodnoty v souboru se používají tři konečné snímcích obrazovky v kapitole 20 _vytváření mobilních aplikací pomocí Xamarin.Forms_ na stránce 684, ale můžete experimentovat s programem v této kapitole přijít s jedním z vlastními hodnotami. 

**Mandelbrot animace** ukázka ukládá tyto `COUNT` rastrových obrázků v úložišti místní aplikace. Padesát rastrové obrázky vyžadují více než 20 MB místa na disku na vašem zařízení, takže můžete chtít vědět, jak velké úložiště, jsou tyto bitmapy zabírá a někdy můžete chtít odstranit všechny. To je účel tyto dvě metody v dolní části `MainPage` třídy:

```csharp
public partial class MainPage : ContentPage
{
    ···
    void TallyBitmapSizes()
    {
        long fileSize = 0;

        foreach (string filename in Directory.EnumerateFiles(FolderPath()))
        {
            fileSize += new FileInfo(filename).Length;
        }

        storageLabel.Text = $"Total storage: {fileSize:N0} bytes";
    }

    void OnDeleteButtonClicked(object sender, EventArgs args)
    {
        foreach (string filepath in Directory.EnumerateFiles(FolderPath()))
        {
            File.Delete(filepath);
        }

        TallyBitmapSizes();
    }
}
```

Rastrové obrázky v místním úložišti můžete odstranit, zatímco program je animace tyto stejné rastrové obrázky, protože zachová aplikace v paměti. Ale při příštím spuštění programu, ji budou muset znovu vytvořit rastrové obrázky.

Začlenit rastrové obrázky uložené v úložišti místní aplikace `center` hodnotu v jejich názvy souborů, takže pokud změníte `center` nastavení, existující rastrové obrázky nenahradí ve službě storage a bude i nadále zabírají prostor.

Tady jsou metody, která `MainPage` používá pro vytváření názvů souborů, a také `MakePixel` metoda k definování pixelu hodnotu založené na součástech barvy:

```csharp
public partial class MainPage : ContentPage
{
    ···
    // File path for storing each bitmap in local storage
    string FolderPath() => 
        Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);

    string FilePath(int zoomLevel) => 
        Path.Combine(FolderPath(),
                     String.Format("R{0}I{1}Z{2:D2}.png", center.Real, center.Imaginary, zoomLevel));

    // Form bitmap pixel for Rgba8888 format
    uint MakePixel(byte alpha, byte red, byte green, byte blue) =>
        (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

`zoomLevel` Parametr `FilePath` rozsahu od 0 do `COUNT` konstantní mínus 1.

`MainPage` Volání konstruktoru `LoadAndStartAnimation` metody:

```csharp
public partial class MainPage : ContentPage
{
    ···
    public MainPage()
    {
        InitializeComponent();

        LoadAndStartAnimation();
    }
    ···
}
```

`LoadAndStartAnimation` Metoda je zodpovědná za přístup k místní úložiště aplikací se načíst všechny rastrové obrázky, může být vytvořena, když program byl spuštěn dříve. Cyklicky projde `zoomLevel` hodnoty od 0 do `COUNT`. Pokud soubor existuje, načte do `bitmaps` pole. V opačném případě je potřeba vytvořit rastrový obrázek pro konkrétní `center` a `zoomLevel` hodnoty voláním `Mandelbrot.CalculateAsync`. Tato metoda získá počty iterace pro každý pixel, který tato metoda převádí barvy:

```csharp
public partial class MainPage : ContentPage
{
    ···
    async void LoadAndStartAnimation()
    {
        // Show total bitmap storage
        TallyBitmapSizes();

        // Create progressReporter for async operation
        Progress<double> progressReporter =
            new Progress<double>((double progress) => progressBar.Progress = progress);

        // Create (unused) CancellationTokenSource for async operation
        CancellationTokenSource cancelTokenSource = new CancellationTokenSource();

        // Loop through all the zoom levels
        for (int zoomLevel = 0; zoomLevel < COUNT; zoomLevel++)
        {
            // If the file exists, load it
            if (File.Exists(FilePath(zoomLevel)))
            {
                statusLabel.Text = $"Loading bitmap for zoom level {zoomLevel}";

                using (Stream stream = File.OpenRead(FilePath(zoomLevel)))
                {
                    bitmaps[zoomLevel] = SKBitmap.Decode(stream);
                }
            }
            // Otherwise, create a new bitmap
            else
            {
                statusLabel.Text = $"Creating bitmap for zoom level {zoomLevel}";

                CancellationToken cancelToken = cancelTokenSource.Token;

                // Do the (generally lengthy) Mandelbrot calculation 
                BitmapInfo bitmapInfo =
                    await Mandelbrot.CalculateAsync(center,
                                                    4 / Math.Pow(2, zoomLevel),
                                                    4 / Math.Pow(2, zoomLevel),
                                                    BITMAP_SIZE, BITMAP_SIZE,
                                                    (int)Math.Pow(2, 10), progressReporter, cancelToken);

                // Create bitmap & get pointer to the pixel bits
                SKBitmap bitmap = new SKBitmap(BITMAP_SIZE, BITMAP_SIZE, SKColorType.Rgba8888, SKAlphaType.Opaque);
                IntPtr basePtr = bitmap.GetPixels();

                // Set pixel bits to color based on iteration count
                for (int row = 0; row < bitmap.Width; row++)
                    for (int col = 0; col < bitmap.Height; col++)
                    {
                        int iterationCount = bitmapInfo.IterationCounts[row * bitmap.Width + col];
                        uint pixel = 0xFF000000;            // black

                        if (iterationCount != -1)
                        {
                            double proportion = (iterationCount / 32.0) % 1;
                            byte red = 0, green = 0, blue = 0;

                            if (proportion < 0.5)
                            {
                                red = (byte)(255 * (1 - 2 * proportion));
                                blue = (byte)(255 * 2 * proportion);
                            }
                            else
                            {
                                proportion = 2 * (proportion - 0.5);
                                green = (byte)(255 * proportion);
                                blue = (byte)(255 * (1 - proportion));
                            }

                            pixel = MakePixel(0xFF, red, green, blue);
                        }

                        // Calculate pointer to pixel
                        IntPtr pixelPtr = basePtr + 4 * (row * bitmap.Width + col);

                        unsafe     // requires compiling with unsafe flag
                        {
                            *(uint*)pixelPtr.ToPointer() = pixel;
                        }
                    }

                // Save as PNG file
                SKData data = SKImage.FromBitmap(bitmap).Encode();

                try
                {
                    File.WriteAllBytes(FilePath(zoomLevel), data.ToArray());
                }
                catch
                {
                    // Probably out of space, but just ignore
                }

                // Store in array
                bitmaps[zoomLevel] = bitmap;

                // Show new bitmap sizes
                TallyBitmapSizes();
            }

            // Display the bitmap
            bitmapIndex = zoomLevel;
            canvasView.InvalidateSurface();
        }

        // Now start the animation
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }
    ···
}
```

Všimněte si, že program ukládá tyto rastrových obrázků v úložišti místní aplikace, nikoli v knihovnu fotografií zařízení. Knihovna .NET Standard 2.0 umožňuje používat známé `File.OpenRead` a `File.WriteAllBytes` metody pro tuto úlohu.

Po všech rastrových obrázků, které byly vytvořeny nebo načtena do paměti, spustí metodu `Stopwatch` objektu a volání `Device.StartTimer`. `OnTimerTick` Metoda je volána každých 16 milisekund.

`OnTimerTick` vypočítá `time` hodnotu v milisekundách, od 0 do 6000 časy `COUNT`, který apportions šesti sekund pro zobrazení jednotlivých rastrového obrázku. `progress` Hodnota používá `Math.Sin` hodnota se má vytvořit sinusovým animaci, která sníží se na začátku cyklu a pomalejší, protože po uplynutí obrátí směr. 

`progress` Hodnotu od 0 do `COUNT`. To znamená, že celočíselnou část `progress` je index do `bitmaps` pole při zlomkovou část `progress` indikuje úroveň přiblížení pro tento konkrétní rastrový obrázek. Tyto hodnoty jsou uloženy v `bitmapIndex` a `bitmapProgress` pole a jsou zobrazeny `Label` a `Slider` v souboru XAML. `SKCanvasView` Zneplatněna aktualizovat zobrazení rastrového obrázku:

```csharp
public partial class MainPage : ContentPage
{
    ···
    Stopwatch stopwatch = new Stopwatch();      // for the animation
    int bitmapIndex;
    double bitmapProgress = 0;
    ···
    bool OnTimerTick()
    {
        int cycle = 6000 * COUNT;       // total cycle length in milliseconds

        // Time in milliseconds from 0 to cycle
        int time = (int)(stopwatch.ElapsedMilliseconds % cycle);

        // Make it sinusoidal, including bitmap index and gradation between bitmaps
        double progress = COUNT * 0.5 * (1 + Math.Sin(2 * Math.PI * time / cycle - Math.PI / 2));

        // These are the field values that the PaintSurface handler uses
        bitmapIndex = (int)progress;
        bitmapProgress = progress - bitmapIndex;

        // It doesn't often happen that we get up to COUNT, but an exception would be raised
        if (bitmapIndex < COUNT)
        {
            // Show progress in UI
            statusLabel.Text = $"Displaying bitmap for zoom level {bitmapIndex}";
            progressBar.Progress = bitmapProgress;

            // Update the canvas
            canvasView.InvalidateSurface();
        }

        return true;
    }
    ···
}
```

Nakonec `PaintSurface` obslužná rutina `SKCanvasView` vypočítá cílového obdélníku zobrazíte co největší rastrového obrázku při zachování poměru stran. Je na základě zdrojového obdélníku `bitmapProgress` hodnotu. `fraction` Hodnotu vypočítat zde rozsahu od 0 v případě `bitmapProgress` je 0, chcete-li zobrazit celý rastrový obrázek, v případě 0,25 `bitmapProgress` 1 zobrazíte poloviční šířku a výšku rastrového obrázku, efektivní přiblížení:

```csharp
public partial class MainPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        if (bitmaps[bitmapIndex] != null)
        {
            // Determine destination rect as square in canvas
            int dimension = Math.Min(info.Width, info.Height);
            float x = (info.Width - dimension) / 2;
            float y = (info.Height - dimension) / 2;
            SKRect destRect = new SKRect(x, y, x + dimension, y + dimension);

            // Calculate source rectangle based on fraction:
            //  bitmapProgress == 0: full bitmap
            //  bitmapProgress == 1: half of length and width of bitmap
            float fraction = 0.5f * (1 - (float)Math.Pow(2, -bitmapProgress));
            SKBitmap bitmap = bitmaps[bitmapIndex];
            int width = bitmap.Width;
            int height = bitmap.Height;
            SKRect sourceRect = new SKRect(fraction * width, fraction * height, 
                                           (1 - fraction) * width, (1 - fraction) * height);

            // Display the bitmap
            canvas.DrawBitmap(bitmap, sourceRect, destRect);
        }
    }
    ···
}
```

Tady je program spuštěn na všech třech platformách:

[![Animace Mandelbrot](animating-images/MandelbrotAnimation.png "Mandelbrot animace")](animating-images/MandelbrotAnimation-Large.png#lightbox)

## <a name="gif-animation"></a>Animace ve formátu GIF

Specifikace formátu GIF (Graphics Interchange) obsahuje funkce, která umožňuje jeden soubor GIF tak, aby obsahovala více po sobě jdoucích snímků scény, které mohou být zobrazeny v daný okamžik, často ve smyčce. Tyto soubory jsou označovány jako _animované Gify_. Webové prohlížeče můžete přehrát animovaných Gifů a ve Skiasharpu umožňuje aplikaci extrahujte snímky z animovaný GIF a k jejich zobrazení postupně.

[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) ukázka zahrnuje prostředek animovaný obrázek GIF s názvem **Newtons_cradle_animation_book_2.gif** vytvořil DemonDeLuxe a stáhnout z [stojan Newton. ](https://en.wikipedia.org/wiki/Newton%27s_cradle) stránky na wikipedii. **Animovaný GIF** stránka obsahuje soubor XAML, který poskytuje tyto informace a vytváří instanci `SKCanvasView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.AnimatedGifPage"
             Title="Animated GIF">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skia:SKCanvasView x:Name="canvasView" 
                           Grid.Row="0"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="GIF file by DemonDeLuxe from Wikipedia Newton's Cradle page"
               Grid.Row="1"
               Margin="0, 5"
               HorizontalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

Soubor kódu na pozadí se nezobecňuje jakékoli animovaný GIF přehrávání. Ignoruje některé informace, které je k dispozici, zejména může mít počet opakování a jednoduše přehraje animovaný obrázek GIF ve smyčce. 

Použití SkisSharp extrahujte snímky z animovaný GIF zdá se, že chcete zdokumentovat, popis kód, který je podrobnější než obvykle:

Dekódování animovaný GIF dochází v konstruktoru na stránce a vyžaduje, aby `Stream` objekt odkazující rastrového obrázku použije k vytvoření `SKManagedStream` objekt a potom [ `SKCodec` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCodec/) objektu. [ `FrameCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodec.FrameCount/) Vlastnost označuje počet snímků, které tvoří animace. 

Tyto snímky se nakonec ukládají jako jednotlivé rastrové obrázky, takže konstruktor používá `FrameCount` přidělit pole typu `SKBitmap` spolu se dvěma `int` celkové pole po dobu trvání jednotlivých snímků a (pro usnadnění logiky animace) doby trvání.

[ `FrameInfo` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodec.FrameInfo/) Vlastnost `SKCodec` třída je pole [ `SKCodecFrameInfo` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCodecFrameInfo/) hodnoty, jeden pro každý snímek, ale jediné, co tento program přebírá z této struktury je [ `Duration` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodecFrameInfo.Duration/) rámce v milisekundách.

`SKCodec` definuje vlastnost s názvem [ `Info` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCodec.Info/) typu [ `SKImageInfo` ](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/), ale tato `SKImageInfo` hodnota označuje (alespoň pro tuto bitovou kopii), jestli je typ barva `SKColorType.Index8`, což znamená, že Každý pixel je index do typ barvy. Aby se zabránilo varovat s tabulkami barvy, program používá [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Width/) a [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Height/) informace z této struktury jeho vytvoření je vlastní barevné `ImageInfo` hodnotu. Každý `SKBitmap` od vytvoření.

`GetPixels` Metoda `SKBitmap` vrátí `IntPtr` odkazující na bitů pixel tento rastrový obrázek. Tyto bitů na pixel nebyla ještě nastavena. Že `IntPtr` se předávají do jednoho ze [ `GetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCodec.GetPixels/p/SkiaSharp.SKImageInfo/System.IntPtr/SkiaSharp.SKCodecOptions/) metody `SKCodec`. Tuto metodu zkopíruje snímek ze souboru ve formátu GIF do paměťový prostor odkazuje `IntPtr`. [ `SKCodecOptions` ](https://developer.xamarin.com/api/constructor/SkiaSharp.SKCodecOptions.SKCodecOptions/p/System.Int32/System.Boolean/) Konstruktor Určuje číslo snímku:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;
    ···

    public AnimatedGifPage ()
    {
        InitializeComponent ();

        string resourceID = "SkiaSharpFormsDemos.Media.Newtons_cradle_animation_book_2.gif";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        using (SKCodec codec = SKCodec.Create(skStream))
        {
            // Get frame count and allocate bitmaps
            int frameCount = codec.FrameCount;
            bitmaps = new SKBitmap[frameCount];
            durations = new int[frameCount];
            accumulatedDurations = new int[frameCount];

            // Note: There's also a RepetitionCount property of SKCodec not used here

            // Loop through the frames
            for (int frame = 0; frame < frameCount; frame++)
            {
                // From the FrameInfo collection, get the duration of each frame
                durations[frame] = codec.FrameInfo[frame].Duration;

                // Create a full-color bitmap for each frame
                SKImageInfo imageInfo = code.new SKImageInfo(codec.Info.Width, codec.Info.Height);
                bitmaps[frame] = new SKBitmap(imageInfo);

                // Get the address of the pixels in that bitmap
                IntPtr pointer = bitmaps[frame].GetPixels();

                // Create an SKCodecOptions value to specify the frame
                SKCodecOptions codecOptions = new SKCodecOptions(frame, false);

                // Copy pixels from the frame into the bitmap
                codec.GetPixels(imageInfo, pointer, codecOptions);
            }

            // Sum up the total duration
            for (int frame = 0; frame < durations.Length; frame++)
            {
                totalDuration += durations[frame];
            }

            // Calculate the accumulated durations 
            for (int frame = 0; frame < durations.Length; frame++)
            {
                accumulatedDurations[frame] = durations[frame] + 
                    (frame == 0 ? 0 : accumulatedDurations[frame - 1]);
            }
        }
    }
    ···
}
```

Bez ohledu `IntPtr` hodnoty, ne `unsafe` kódu se vyžaduje, protože `IntPtr` nikdy je převedena na hodnotu ukazatele C#.

Po každém snímku bylo extrahováno, konstruktor celkový počet zpracovaných položek do doby trvání všechny rámce a pak inicializuje pole jiného celkové doby trvání.

Zbývající část kódu na pozadí souboru je vyhrazen pro animace. `Device.StartTimer` Metoda se používá ke spuštění časovače, když a `OnTimerTick` používá zpětné volání `Stopwatch` objektem pro určení uplynulý čas v milisekundách. Opakování ve smyčce prostřednictvím pole součet dob trvání je dostačující k nalezení aktuálního rámce:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;

    Stopwatch stopwatch = new Stopwatch();
    bool isAnimating;

    int currentFrame;
    ···
    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        int msec = (int)(stopwatch.ElapsedMilliseconds % totalDuration);
        int frame = 0;

        // Find the frame based on the elapsed time
        for (frame = 0; frame < accumulatedDurations.Length; frame++)
        {
            if (msec < accumulatedDurations[frame])
            {
                break;
            }
        }

        // Save in a field and invalidate the SKCanvasView.
        if (currentFrame != frame)
        {
            currentFrame = frame;
            canvasView.InvalidateSurface();
        }

        return isAnimating;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);
            
        // Get the bitmap and center it
        SKBitmap bitmap = bitmaps[currentFrame];
        canvas.DrawBitmap(bitmap,info.Rect, BitmapStretch.Uniform);
    }
}
```

Pokaždé, když `currentframe` proměnné změny `SKCanvasView` zneplatněna a zobrazí se nový rámec:

[![Animovaný GIF](animating-images/AnimatedGif.png "animovaný obrázek GIF")](animating-images/AnimatedGif-Large.png#lightbox)

Samozřejmě, bude potřeba spustit program sami animace.

## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Animace Mandelbrot (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/MandelAnima/)
