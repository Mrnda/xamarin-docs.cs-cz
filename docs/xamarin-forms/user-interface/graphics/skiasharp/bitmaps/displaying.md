---
title: Zobrazení rastrové obrázky ve Skiasharpu
description: Zjistěte, jak zobrazit ve Skiasharpu rastrové obrázky v pixelů, velikosti a rozšířit tak, aby vyplnil obdélníky při zachování poměru stran.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8E074F8D-4715-4146-8CC0-FD7A8290EDE9
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 31a5841c78b68b916e3df3d1d6598b86b0ec8bd4
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130994"
---
# <a name="displaying-skiasharp-bitmaps"></a>Zobrazení rastrové obrázky ve Skiasharpu

Předmět rastrové obrázky ve Skiasharpu byla zavedena v následujícím článku  **[základy rastrového obrázku v SkiaSharp](../basics/bitmaps.md)**. Tento článek vám ukázal, tři způsoby, jak zatížení rastrové obrázky a tři způsoby, jak zobrazit rastrových obrázků. Tento článek kontroly techniky k načtení rastrové obrázky a platí hlubší využívání `DrawBitmap` metody `SKCanvas`.

![Ukázkové zobrazení](displaying-images/DisplayingSample.png "zobrazení vzorku")

`DrawBitmapLattice` a `DrawBitmapNinePatch` metody jsou popsané v článku  **[segmentované zobrazení rastrových obrázků ve Skiasharpu](segmented.md)**.

Ukázky na této stránce jsou z **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** aplikace. Z domovské stránky této aplikace, zvolte **rastrové obrázky ve Skiasharpu**a pak přejděte **zobrazení rastrové obrázky** části.

## <a name="loading-a-bitmap"></a>Načítání rastrový obrázek

Rastrový obrázek používá aplikace ve Skiasharpu obvykle pocházejí z jednoho ze tří různých zdrojů:

- Z více než Internetu
- Ze zdroje vložený do spustitelného souboru
- Z knihovny fotografií uživatele

Je také možné pro aplikace ve Skiasharpu vytvořit nový rastrový obrázek, a kreslit v něm algorithmically nastavit rastrové bity. Tyto metody jsou popsané v článcích **[vytváření a kreslení ve Skiasharpu rastrových obrázků](drawing.md)** a **[přístup k pixelů rastrového obrázku ve Skiasharpu](pixel-bits.md)**.

V následujících příkladech kódu tři načítání rastrový obrázek, třída předpokládá, že je pole typu `SKBitmap`:

```csharp
SKBitmap bitmap;
```

Jako článek **[základy rastrového obrázku v SkiaSharp](../basics/bitmaps.md)** uvedeno, je nejlepší způsob, jak načíst bitmapu přes Internet s [ `HttpClient` ](xref:System.Net.Http.HttpClient) třídy. Jednu instanci této třídy lze definovat jako pole:

```csharp
HttpClient httpClient = new HttpClient();
```

Při použití `HttpClient` pomocí aplikace pro iOS a Android, budete chtít nastavit vlastnosti projektu, jak je popsáno v dokumentech na  **[zabezpečení TLS (Transport Layer) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)**.

Kód, který používá `HttpClient` obvykle zahrnuje `await` operátoru, takže musí být umístěn v `async` metody:

```csharp
try
{
    using (Stream stream = await httpClient.GetStreamAsync("https:// ··· "))
    using (MemoryStream memStream = new MemoryStream())
    {
        await stream.CopyToAsync(memStream);
        memStream.Seek(0, SeekOrigin.Begin);

        bitmap = SKBitmap.Decode(stream);
        ···
    };
}
catch
{
    ···
}
```

Všimněte si, že `Stream` získaného z `GetStreamAsync` je zkopírována `MemoryStream`. Android není povolena `Stream` z `HttpClient` zpracovat hlavního vlákna s výjimkou v asynchronních metodách. 

[ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/System.IO.Stream/) Provádí spoustu práce: `Stream` předaný objekt odkazuje na blok paměti obsahující celý rastrového obrázku v jednom běžných formátů souborů rastrový obrázek, obecně JPEG, PNG, nebo ve formátu GIF. `Decode` Metoda musí určit formát a pak dekódovat rastrového obrázku na formát SkiaSharp vaší vlastní interní rastrového obrázku.

Po váš kód zavolá `SKBitmap.Decode`, pravděpodobně zruší platnost `CanvasView` tak, aby `PaintSurface` obslužná rutina může zobrazit nově načtená rastrového obrázku.

Druhý způsob, jak načíst bitmapu se zahrnutím rastrového obrázku jako vložený prostředek v knihovně .NET Standard odkazuje projekty jednotlivé platformy. Resource ID je předán [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) metody. Toto ID prostředku se skládá z názvu sestavení, název složky a název souboru prostředků oddělených tečkami:

```csharp
string resourceID = "assemblyName.folderName.fileName";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    bitmap = SKBitmap.Decode(stream);
    ···
}
```

Rastrové soubory mohou být také uloženy jako prostředky v projektu pro jednotlivé platformy pro iOS, Android a univerzální platformu Windows (UPW). Načítání těchto rastrové obrázky, ale vyžaduje kód, který se nachází v projektu platformy.

Třetí přístup k získání rastrový obrázek je z knihovny obrázků uživatele. Následující kód používá závislou službu, která je součástí **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** aplikace. **SkiaSharpFormsDemo** knihovna .NET Standard zahrnuje `IPhotoLibrary` rozhraní, zatímco obsahuje všechny projekty platformy `PhotoLibrary` třídu, která implementuje rozhraní.

```csharp
IPhotoicturePicker picturePicker = DependencyService.Get<IPhotoLibrary>();

using (Stream stream = await picturePicker.GetImageStreamAsync())
{
    if (stream != null)
    {
        bitmap = SKBitmap.Decode(stream);
        ···
    }
}
```

Obecně platí, takový kód také zruší platnost `CanvasView` tak, aby `PaintSurface` obslužná rutina může zobrazit nový rastrový obrázek.

`SKBitmap` Třída definuje několika užitečným vlastnostem, včetně [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) a [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/), který odhalit dimenze pixel rastrový obrázek, stejně jako mnoho metod, včetně metody vytvoření rastrové obrázky, kopírovat a vystavit bitů pixel. 

## <a name="displaying-in-pixel-dimensions"></a>Zobrazení v pixelech

SkiaSharp [ `Canvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) třída definuje čtyři `DrawBitmap` metody. Tyto metody umožňují rastrové obrázky, který se má zobrazit fundamentálně odlišný způsob dvěma způsoby: 

- Určení `SKPoint` hodnotu (nebo samostatné `x` a `y` hodnoty) zobrazí rastrový obrázek v jeho v pixelech. Pixely rastrového obrázku se mapují přímo na zobrazení videa v pixelech.
- Určení obdélník způsobí, že rastrový obrázek pro roztažen tak, aby velikost a tvar obdélníku. 

Zobrazí rastrový obrázek v jeho v pixelech pomocí [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKPoint/SkiaSharp.SKPaint/) s `SKPoint` parametr nebo [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) samostatné `x` a `y` parametry:

```csharp
DrawBitmap(SKBitmap bitmap, SKPoint pt, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, float x, float y, SKPaint paint = null)
```

Tyto dvě metody jsou funkčně stejný jako. Zadaný bod určuje umístění levého horního rohu rastrového obrázku vzhledem k na plátno. Protože rozlišení pixelů mobilních zařízení, je tak velká, menší rastrové obrázky se obvykle zobrazují na těchto zařízeních poměrně malý.

Volitelný `SKPaint` parametr umožňuje zobrazit bitmapou pomocí programu blend režimy nebo efektů filtru. Toto je popsána v další články.

**v pixelech** stránku **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** ukázkový program zobrazí prostředku rastrového obrázku, který je 320 pixelů na šířku a 240 pixelů:

```csharp
public class PixelDimensionsPage : ContentPage
{
    SKBitmap bitmap;

    public PixelDimensionsPage()
    {
        Title = "Pixel Dimensions";

        // Load the bitmap from a resource
        string resourceID = "SkiaSharpFormsDemos.Media.Banana.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }

        // Create the SKCanvasView and set the PaintSurface handler
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

        float x = (info.Width - bitmap.Width) / 2;
        float y = (info.Height - bitmap.Height) / 2;

        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

`PaintSurface` Obslužná rutina centra rastrový obrázek výpočtem `x` a `y` hodnoty na základě pixel rozměry zobrazovacím povrchu a dimenze pixel rastrového obrázku:

[![V pixelech](displaying-images/PixelDimensions.png "v pixelech")](displaying-images/PixelDimensions-Large.png#lightbox)

Pokud aplikace chce zobrazit rastrového obrázku v jeho levého horního rohu, by jednoduše předat souřadnice (0, 0). 

## <a name="a-method-for-loading-resource-bitmaps"></a>Metody pro načítání prostředků rastrových obrázků

Řadu chystá se ukázky muset načíst prostředky rastrového obrázku. Statické `BitmapExtensions` třídy v **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** řešení obsahuje metody pro pomoc:

```csharp
static class BitmapExtensions
{
    public static SKBitmap LoadBitmapResource(Type type, string resourceID)
    {
        Assembly assembly = type.GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            return SKBitmap.Decode(stream);
        }
    }
    ···
}
```

Všimněte si, že `Type` parametru. To může být `Type` objekt přidružený k libovolného typu v sestavení, která ukládá prostředku rastrového obrázku.

To `LoadBitmapResource` metoda budou použita všechny následné ukázky, které vyžadují prostředky rastrového obrázku.

## <a name="stretching-to-fill-a-rectangle"></a>Roztáhnout tak, aby vyplnil obdélník

`SKCanvas` Také definuje třídu [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) metody, která vykreslí rastrový obrázek pro obdélníku a druhý [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) metody, která vykreslí obdélníkové podmnožiny rastrový obrázek pro obdélník:

```
DrawBitmap(SKBitmap bitmap, SKRect dest, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, SKRect source, SKRect dest, SKPaint paint = null)
```

V obou případech je rastrového obrázku roztažený tak, aby vyplnil obdélníku s názvem `dest`. Ve druhé metodě `source` obdélník umožňuje vybrat podmnožinu rastrového obrázku. `dest` Obdélníku je relativní vzhledem k výstupní zařízení; `source` obdélníku je relativní vzhledem k rastrového obrázku.

**Vyplnit obdélník** stránce ukazuje první z těchto dvou metod zobrazením stejné rastrového obrázku používaného v předchozím příkladu v obdélníku stejnou velikost plátna: 

```csharp
public class FillRectanglePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public FillRectanglePage ()
    {
        Title = "Fill Rectangle";

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

Všimněte si nového `BitmapExtensions.LoadBitmapResource` metody nastavte `SKBitmap` pole. Cílového obdélníku se získávají z [ `Rect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Rect/) vlastnost `SKImageInfo`, který popisuje velikost zobrazovacím povrchu:

[![Vyplnit obdélník](displaying-images/FillRectangle.png "vyplnit obdélník")](displaying-images/FillRectangle-Large.png#lightbox)

Toto je obvykle _není_ co chcete. Na obrázku je narušeny roztažení odlišně v směrech vodorovný a svislý. Při zobrazení rastrový obrázek něco jiného než její velikost v pixelech, obvykle budete chtít zachovat původní poměr stran obrázku rastrový obrázek.

## <a name="stretching-while-preserving-the-aspect-ratio"></a>Roztažení při zachování poměru stran

Roztažení rastrový obrázek při zachování poměru stran procesu se taky říká _jednotné škálování_. Tento termín navrhuje vylepšením přístup. Jedním z možných řešení je zobrazena ve **jednotné škálování** stránky:

```csharp
public class UniformScalingPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(UniformScalingPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public UniformScalingPage()
    {
        Title = "Uniform Scaling";

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

        float scale = Math.Min((float)info.Width / bitmap.Width, 
                               (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect destRect = new SKRect(x, y, x + scale * bitmap.Width, 
                                           y + scale * bitmap.Height);

        canvas.DrawBitmap(bitmap, destRect);
    }
}
```

`PaintSurface` Vypočítá obslužné rutiny `scale` faktorem, který je minimální poměr zobrazení šířku a výšku na rastrový obrázek šířku a výšku. `x` a `y` hodnoty pak lze vypočítat pro zarovnání škálován rastrového obrázku v rámci zobrazení šířku a výšku. Má levého horního rohu cílového obdélníku `x` a `y` a pravém dolním rohu tyto hodnoty a měřítkem šířku a výšku rastrového obrázku:

[![Jednotné škálování](displaying-images/UniformScaling.png "jednotné škálování")](displaying-images/UniformScaling-Large.png#lightbox)

Zapněte otočením telefonu zobrazit rastrový obrázek roztažená do této oblasti:

[![Jednotné prostředí škálování](displaying-images/UniformScaling-Landscape.png "jednotné škálování na šířku")](displaying-images/UniformScaling-Landscape-Large.png#lightbox)

Výhodou použití této funkce `scale` faktor změní zřejmé, pokud chcete implementovat trochu jiný algoritmus. Předpokládejme, že chcete zachovat poměr stran rastrový obrázek, ale také vyplnit cílového obdélníku. Jediným způsobem, je to možné je oříznutí součástí bitové kopie, ale můžete implementovat tento algoritmus jednoduše tak, že změna `Math.Min` k `Math.Max` ve výše uvedeném kódu. Tady je výsledek: 

[![Jednotné alternativní škálování](displaying-images/UniformScaling-Alternative.png "alternativní jednotné škálování")](displaying-images/UniformScaling-Alternative-Large.png#lightbox)

Zachování poměru stran rastrového obrázku ale oblasti vlevo a vpravo od rastrového obrázku jsou oříznuté.

## <a name="a-versatile-bitmap-display-function"></a>Zobrazení funkce univerzální rastrový obrázek

Programovací prostředí založeného na XAML (například UPW a Xamarin.Forms) mít zařízení pro zvětšení nebo zmenšení velikosti rastrové obrázky při zachování jejich poměry stran. I když ve Skiasharpu nezahrnuje tuto funkci, můžete ho implementovat sami. `BitmapExtensions` Třída součástí [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) aplikace ukazuje jak. Třída definuje dva nové `DrawBitmap` metody, které provedení výpočtu poměru stran. Tyto nové metody jsou metody rozšíření `SKCanvas`.

Nové `DrawBitmap` metody zahrnovat parametr typu `BitmapStretch`, výčet definovaný v **BitmapExtensions.cs** souboru:

```csharp
public enum BitmapStretch
{
    None,
    Fill,
    Uniform,
    UniformToFill,
    AspectFit = Uniform,
    AspectFill = UniformToFill
}
```

`None`, `Fill`, `Uniform`, A `UniformToFill` členy jsou stejné jako na UPW [ `Stretch` ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) výčtu. Podobně jako Xamarin.Forms [ `Aspect` ](xref:Xamarin.Forms.Aspect) výčet definuje členy `Fill`, `AspectFit`, a `AspectFill`.

**Jednotné škálování** stránky zobrazené výše centra rastrového obrázku v obdélníku, ale může být vhodné další možnosti, jako je například umístění rastrový obrázek na levé nebo pravé části obdélníku, nebo top a bottom. To je účel `BitmapAlignment` výčtu:

```csharp
public enum BitmapAlignment
{
    Start,
    Center,
    End
}
```

Nastavení zarovnání nemají žádný vliv při použití s `BitmapStretch.Fill`.

První `DrawBitmap` funkce rozšíření obsahuje cílového obdélníku, ale bez zdrojového obdélníku. Výchozí hodnoty jsou definovány, takže pokud chcete rastrový obrázek na střed, stačí jen zadat `BitmapStretch` člena:

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect dest, 
                                  BitmapStretch stretch, 
                                  BitmapAlignment horizontal = BitmapAlignment.Center, 
                                  BitmapAlignment vertical = BitmapAlignment.Center, 
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * bitmap.Width, scale * bitmap.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, display, paint);
        }
    }
    ···
}
```

Hlavním účelem tato metoda je k výpočtu měřítko s názvem `scale` , který se následně použije na rastrový obrázek šířku a výšku při volání `CalculateDisplayRect` metody. Toto je metoda, která vypočítá obdélníku pro zobrazení podle vodorovného a svislého zarovnání rastrového obrázku:

```csharp
static class BitmapExtensions
{
    ···
    static SKRect CalculateDisplayRect(SKRect dest, float bmpWidth, float bmpHeight, 
                                       BitmapAlignment horizontal, BitmapAlignment vertical)
    {
        float x = 0;
        float y = 0;

        switch (horizontal)
        {
            case BitmapAlignment.Center:
                x = (dest.Width - bmpWidth) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                x = dest.Width - bmpWidth;
                break;
        }

        switch (vertical)
        {
            case BitmapAlignment.Center:
                y = (dest.Height - bmpHeight) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                y = dest.Height - bmpHeight;
                break;
        }

        x += dest.Left;
        y += dest.Top;

        return new SKRect(x, y, x + bmpWidth, y + bmpHeight);
    }
}
```

`BitmapExtensions` Třída obsahuje další `DrawBitmap` metodu s zdrojového obdélníku pro zadání podmnožinu rastrového obrázku. Tato metoda je podobná té předchozí, s tím rozdílem, že měřítko se počítá na základě `source` obdélník a použijí se `source` obdélník při volání funkce `CalculateDisplayRect`:

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect source, SKRect dest,
                                  BitmapStretch stretch,
                                  BitmapAlignment horizontal = BitmapAlignment.Center,
                                  BitmapAlignment vertical = BitmapAlignment.Center,
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, source, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / source.Width, dest.Height / source.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / source.Width, dest.Height / source.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * source.Width, scale * source.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, source, display, paint);
        }
    }
    ···
}
```

První z těchto dvou nových `DrawBitmap` metody jsou popsané v článku **škálování režimy** stránky. Soubor XAML obsahuje tři `Picker` prvky, které vám umožní vybrat členy `BitmapStretch` a `BitmapAlignment` výčty:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.ScalingModesPage"
             Title="Scaling Modes">

    <Grid Padding="10">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <skia:SKCanvasView x:Name="canvasView" 
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="Stretch:"
               Grid.Row="1" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="stretchPicker"
                Grid.Row="1" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapStretch}">
                    <x:Static Member="local:BitmapStretch.None" />
                    <x:Static Member="local:BitmapStretch.Fill" />
                    <x:Static Member="local:BitmapStretch.Uniform" />
                    <x:Static Member="local:BitmapStretch.UniformToFill" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Horizontal Alignment:"
               Grid.Row="2" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="horizontalPicker" 
                Grid.Row="2" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Vertical Alignment:"
               Grid.Row="3" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="verticalPicker" 
                Grid.Row="3" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</ContentPage>
```

Soubor kódu na pozadí jednoduše zruší platnost `CanvasView` při `Picker` položka se změnila. `PaintSurface` Obslužná rutina přistupuje k tři `Picker` zobrazení pro volání `DrawBitmap` – metoda rozšíření:

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public ScalingModesPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, dest, stretch, horizontal, vertical);
    }
}
```

Tady jsou některé kombinace možností:

[![Škálování režimy](displaying-images/ScalingModes.png "škálování režimy")](displaying-images/ScalingModes-Large.png#lightbox)

**Obdélník dílčí** stránka má téměř stejný soubor XAML jako **škálování režimy**, ale použití modelu code-behind soubor definuje obdélníkové podmnožiny Dal rastrového obrázku `SOURCE` pole: 

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");

    static readonly SKRect SOURCE = new SKRect(94, 12, 212, 118);

    public RectangleSubsetPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, SOURCE, dest, stretch, horizontal, vertical);
    }
}
```

Tento zdroj obdélník izoluje opic head, jak je znázorněno v těchto snímků obrazovky:

[![Dílčí obdélník](displaying-images/RectangleSubset.png "dílčí obdélník")](displaying-images/RectangleSubset-Large.png#lightbox)

## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

