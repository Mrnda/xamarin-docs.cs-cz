---
title: Ukládání bitmap ve Skiasharpu k souborům
description: Prozkoumejte různé formáty souborů podporovaných ve Skiasharpu pro uložení bitmap do knihovny fotografií uživatele.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2D696CB6-B31B-42BC-8D3B-11D63B1E7D9C
author: charlespetzold
ms.author: chape
ms.date: 07/10/2018
ms.openlocfilehash: 5ef18728bbf417750575bad88b3498f66fa585c4
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276012"
---
# <a name="saving-skiasharp-bitmaps-to-files"></a>Ukládání bitmap ve Skiasharpu k souborům

Po aplikaci ve Skiasharpu vytvořil nebo změnil rastrový obrázek, aplikace chtít uložit do knihovny fotografií uživatele rastrového obrázku:

![Ukládání bitmap](saving-images/SavingSample.png "ukládání bitmap")

Tato úloha zahrnuje dva kroky:

- Převod bitmap ve Skiasharpu na data ve formátu určitého souboru, jako je například JPEG nebo PNG.
- Ukládá výsledek do knihovny fotografií pomocí kódu pro konkrétní platformu.

## <a name="file-formats-and-codecs"></a>Kodeky a formáty souborů

Většina dnešních oblíbených bitmapový soubor formátuje použít kompresi pro úsporu místa. Dvě rozsáhlé kategorie komprese se nazývají _míru ztrát_ a _beze ztrát_. Tyto podmínky označuje, zda algoritmus komprese vede ke ztrátě dat.

Nejoblíbenější míru ztrát formát vyvinula společnost Joint Photographic Experts Group a je volána JPEG. Algoritmus komprese JPEG analyzuje image pomocí matematické nástroj zvaný _diskrétní kosinus transformace_a pokusí se odebrat data, která nejsou důležité pro zachování visual věrností na obrázku. Do jaké míry komprese se dá řídit pomocí nastavení obecně označovaná jako _kvality_. Nastavení vyšší kvality za následek větší soubory.

Naproti tomu algoritmu beze ztrát komprese analyzuje bitovou kopii pro opakování a vzory v pixelech, které mohou být kódovány způsobem, který snižuje data, ale nemá za následek ztrátu žádné informace. Zcela ze komprimovaný soubor je možné obnovit původní data rastrového obrázku. Formát primární beze ztrát komprimovaný soubor používán v současnosti jsou Portable Network Graphics (PNG).

Obecně platí JPEG se používá pro fotografie, zatímco PNG se používá pro bitové kopie, které ručně nebo algorithmically vygenerována. Všechny beze ztrát komprese algoritmus, který snižuje velikost některých souborů musí nutně zvětšit velikost ostatních. Naštěstí tento nárůst velikosti obvykle dochází jenom pro data, která obsahuje velké množství informací náhodné (nebo zdánlivě náhodné).

Komprese, který algoritmy jsou dostatečně složité a zaručujete, že dvě podmínky, které popisují kompresi a dekompresi procesy:

- _dekódování_ &mdash; čtení formátu souboru rastrového obrázku a jeho dekomprimovat
- _kódování_ &mdash; komprimování bitmapy a zápis do formátu souboru rastrového obrázku

[ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/) Třída obsahuje několik metod pojmenovaných `Decode` , vytvořte `SKBitmap` z komprimované zdrojové. Vše, který je potřeba je zadat název souboru, datový proud nebo pole bajtů. Můžete určit formát souboru a přebírají funkci správnou interní dekódování.

Kromě toho [ `SKCodec` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCodec/) třída má dvě metody s názvem `Create` , který můžete vytvořit `SKCodec` objektu z komprimované zdrojové a umožňuje aplikaci získat více zahrnutých v procesu dekódování. ( `SKCodec` Je například v následujícím článku [ **animace rastrové obrázky ve Skiasharpu** ](animating.md#gif-animation) souvislosti animovaný GIF – dekódování.)

Při kódování rastrový obrázek, jsou požadovány další informace: kodér musíte znát konkrétní formátu aplikace chce používat (JPEG nebo PNG nebo něco jiného). Pokud je žádoucí míru ztrát formátu, kódovat musíte taky vědět požadovanou úroveň kvality. 

`SKBitmap` Třída definuje jeden [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Encode/p/SkiaSharp.SKWStream/SkiaSharp.SKEncodedImageFormat/System.Int32/) metoda s následující syntaxí:

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

Tato metoda je popsána podrobněji za chvíli. Kódovaný rastrového obrázku se zapisují do zapisovatelný datový proud. ("W" v `SKWStream` zastupuje "s možností zápisu".) Druhý a třetí argument určit formát souboru a (pro míru ztrát formáty) požadovanou kvalitu od 0 do 100.

Kromě toho [ `SKImage` ](https://developer.xamarin.com/api/type/SkiaSharp.SKImage/) a [ `SKPixmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPixmap/) třídy také definovat `Encode` metody, které jsou o něco větší variabilitu a která můžete dát přednost. Můžete snadno vytvořit `SKImage` objektu z `SKBitmap` pomocí statické [ `SKImage.FromBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKImage.FromBitmap/p/SkiaSharp.SKBitmap/) metody. Můžete získat `SKPixmap` objektu z `SKBitmp` pomocí [ `PeekPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.PeekPixels()/) metoda.

Jeden z [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKImage.Encode()/) metody definované `SKImage` nemá žádné parametry a uloží do formátu PNG. Konstruktor bez parametrů metody je velmi snadné použití.

## <a name="platform-specific-code-for-saving-bitmap-files"></a>Kód specifický pro platformu pro ukládání souborů rastrový obrázek

Při kódování `SKBitmap` formát objektu do konkrétního souboru, obecně byste ubývá objekt datového proudu s nějakým nebo pole data. Některé `Encode` metod (včetně se žádné parametry definované `SKImage`) vrátit [ `SKData` ](https://developer.xamarin.com/api/type/SkiaSharp.SKData/) objektu, který lze převést na pole bajtů pomocí [ `ToArray` ](https://developer.xamarin.com/api/member/SkiaSharp.SKData.ToArray()/) metody. Tato data by pak musí být uložen do souboru. 

Ukládání do souboru v místním úložišti aplikace je poměrně snadné, protože můžete použít standardní `System.IO` třídy a metody pro tuto úlohu. Tato technika je znázorněn v následujícím článku [ **animace rastrové obrázky ve Skiasharpu** ](animating.md#bitmap-animation) souvislosti animace řadu rastrové obrázky Mandelbrot sady.

Pokud chcete, aby ho sdílet jinými aplikacemi, musí být uložen do knihovny fotografií uživatele. Tato úloha vyžaduje kód specifický pro platformu a použijte Xamarin.Forms [ `DependencyService` ](xref:Xamarin.Forms.DependencyService).

**SkiaSharpFormsDemo** projekt [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) aplikace definuje `IPhotoLibrary` rozhraní použité s `DependencyService` třídy. Definuje syntaxe `SavePhotoAsync` metody:

```csharp
public interface IPhotoLibrary
{
    Task<Stream> PickPhotoAsync();

    Task<bool> SavePhotoAsync(byte[] data, string folder, string filename);
}
```

Toto rozhraní definuje také `PickPhotoAsync` metodu, která se používá k otevření souboru – výběr specifické pro platformu pro knihovnu fotografií zařízení.

Pro `SavePhotoAsync`, je první argument pole bajtů obsahující rastrového obrázku již kódovaný do konkrétního souboru ve formátu, jako je například JPEG nebo PNG. Je možné, že aplikace může být vhodné izolovat všechny rastrové obrázky, který vytvoří do konkrétní složky, které je zadaný v parametru další, následovaný názvem souboru. Metoda vrátí logickou hodnotu, která signalizuje úspěch, nebo ne.

Tady je způsob `SavePhotoAsync` se implementuje na třech platformách:

### <a name="the-ios-implementation"></a>Implementace iOS

Implementace iOS `SavePhotoAsync` používá [ `SaveToPhotosAlbum` ](https://developer.xamarin.com/api/member/UIKit.UIImage.SaveToPhotosAlbum/) metoda `UIImage`:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        NSData nsData = NSData.FromArray(data);
        UIImage image = new UIImage(nsData);
        TaskCompletionSource<bool> taskCompletionSource = new TaskCompletionSource<bool>();

        image.SaveToPhotosAlbum((UIImage img, NSError error) =>
        {
            taskCompletionSource.SetResult(error == null);
        });

        return taskCompletionSource.Task;
    }
}
```

Bohužel neexistuje žádný způsob, jak zadat název souboru nebo složky pro bitovou kopii. 

**Info.plist** souboru v projektu pro iOS vyžaduje klíč označující, že přidává do knihovny fotografií imagí:

```xml
<key>NSPhotoLibraryAddUsageDescription</key>
<string>SkiaSharp Forms Demos adds images to your photo library</string>
```

Pozor! Je velmi podobné, ale nikoli stejný klíč oprávnění pro jednoduše přístup k knihovny fotografií:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>SkiaSharp Forms Demos accesses your photo library</string>
```

### <a name="the-android-implementation"></a>Implementace s Androidem

Android provádění `SavePhotoAsync` nejprve ověří, zda `folder` argument je `null` nebo prázdný řetězec. Pokud ano, rastrového obrázku se uloží do kořenového adresáře knihovny fotografií. V opačném případě je získali složce, a pokud neexistuje, vytvoří se:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        try
        {
            File picturesDirectory = Environment.GetExternalStoragePublicDirectory(Environment.DirectoryPictures);
            File folderDirectory = picturesDirectory;

            if (!string.IsNullOrEmpty(folder))
            {
                folderDirectory = new File(picturesDirectory, folder);
                folderDirectory.Mkdirs();
            }

            using (File bitmapFile = new File(folderDirectory, filename))
            {
                bitmapFile.CreateNewFile();

                using (FileOutputStream outputStream = new FileOutputStream(bitmapFile))
                {
                    await outputStream.WriteAsync(data);
                }

                // Make sure it shows up in the Photos gallery promptly.
                MediaScannerConnection.ScanFile(MainActivity.Instance,
                                                new string[] { bitmapFile.Path },
                                                new string[] { "image/png", "image/jpeg" }, null);
            }
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

Volání `MediaScannerConnection.ScanFile` není bezpodmínečně nutné, ale pokud testujete program okamžitě kontrolou knihovny fotografií, je mnohem aktualizováním zobrazení galerie knihovny.

**AndroidManifest.xml** soubor vyžaduje následující značku oprávnění:

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### <a name="the-uwp-implementation"></a>Implementace UPW

Implementace UPW `SavePhotoAsync` je velmi podobné struktury Android implementace:

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        StorageFolder picturesDirectory = KnownFolders.PicturesLibrary;
        StorageFolder folderDirectory = picturesDirectory;

        // Get the folder or create it if necessary
        if (!string.IsNullOrEmpty(folder))
        {
            try
            {
                folderDirectory = await picturesDirectory.GetFolderAsync(folder);
            }
            catch
            { }

            if (folderDirectory == null)
            {
                try
                {
                    folderDirectory = await picturesDirectory.CreateFolderAsync(folder);
                }
                catch
                {
                    return false;
                }
            }
        }

        try
        {
            // Create the file.
            StorageFile storageFile = await folderDirectory.CreateFileAsync(filename,
                                                CreationCollisionOption.GenerateUniqueName);

            // Convert byte[] to Windows buffer and write it out.
            IBuffer buffer = WindowsRuntimeBuffer.Create(data, 0, data.Length, data.Length);
            await FileIO.WriteBufferAsync(storageFile, buffer);
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

**Možnosti** část **Package.appxmanifest** vyžaduje soubor **knihovny obrázků**.

## <a name="exploring-the-image-formats"></a>Zkoumání formátů obrázku

Tady je [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Encode/p/SkiaSharp.SKWStream/SkiaSharp.SKEncodedImageFormat/System.Int32/) metoda `SKImage` znovu:

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

[`SKEncodedImageFormat`](https://developer.xamarin.com/api/type/SkiaSharp.SKEncodedImageFormat/) je výčet s členy, které odkazují na jedenáct formáty souborů rastrový obrázek, z nichž některé jsou spíše skrytého:

- `Astc` &mdash; Škálovatelná adaptivní textury komprese
- `Bmp` &mdash; Rastrového obrázku Windows
- `Dng` &mdash; Digitální negativní Adobe
- `Gif` &mdash; Formát GIF
- `Ico` &mdash; Obrázky ikon pro Windows
- `Jpeg` &mdash; Joint Photographic Experts Group
- `Ktx` &mdash; Formát textury Khronos OpenGL
- `Pkm` &mdash; Pokémon uložit soubor
- `Png` &mdash; Formát PNG
- `Wbmp` &mdash; Formát pro protokol rastrový obrázek bezdrátové aplikace (1 bitů na pixel)
- `Webp` &mdash; Formát Google WebP

Jak krátce zobrazí, pouze tyto tři formáty souborů (`Jpeg`, `Png`, a `Webp`) skutečně podporuje ve Skiasharpu.

Uložit `SKBitmap` objekt s názvem `bitmap` do knihovny fotografií uživatele, musíte také členem skupiny `SKEncodedImageFormat` výčtu s názvem `imageFormat` a (pro míru ztrát formáty) celé `quality` proměnné. Následující kód můžete použít pro tento rastrový obrázek uložte do souboru s názvem `filename` v `folder` složky:

```csharp
using (MemoryStream memStream = new MemoryStream())
using (SKManagedWStream wstream = new SKManagedWStream(memStream))
{
    bitmap.Encode(wstream, imageFormat, quality);
    byte[] data = memStream.ToArray();

    // Check the data array for content!

    bool success = await DependencyService.Get<IPhotoLibrary>().SavePhotoAsync(data, folder, filename);

    // Check return value for success!
}
```

`SKManagedWStream` Třída odvozena z `SKWStream` (což je zkratka pro "stream s možností zápisu"). `Encode` Metoda zapíše soubor kódovaný rastrový obrázek do tohoto datového proudu. Komentáře v kódu odkazovat na některé kontroly chyb, že možná budete muset provést. 

**Uložit formáty souborů** stránku [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) aplikace používá podobný kód k tomu, abyste mohli experimentovat s ukládáním rastrový obrázek v různých formátech.

Obsahuje soubor XAML `SKCanvasView` , který zobrazí rastrový obrázek, zatímco zbytek stránky obsahuje všechno, co aplikace potřebuje k volání `Encode` metoda `SKBitmap`. Má `Picker` člena `SKEncodedImageFormat` výčtu, `Slider` kvality argument rastrového obrázku míru ztrát formátů dvě `Entry` zobrazení název souboru a název složky a `Button` pro uložení souboru.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.SaveFileFormatsPage"
             Title="Save Bitmap Formats">

    <StackLayout Margin="10">
        <skiaforms:SKCanvasView PaintSurface="OnCanvasViewPaintSurface"
                                VerticalOptions="FillAndExpand" />

        <Picker x:Name="formatPicker"
                Title="image format"
                SelectedIndexChanged="OnFormatPickerChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKEncodedImageFormat}">
                    <x:Static Member="skia:SKEncodedImageFormat.Astc" />
                    <x:Static Member="skia:SKEncodedImageFormat.Bmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Dng" />
                    <x:Static Member="skia:SKEncodedImageFormat.Gif" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ico" />
                    <x:Static Member="skia:SKEncodedImageFormat.Jpeg" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ktx" />
                    <x:Static Member="skia:SKEncodedImageFormat.Pkm" />
                    <x:Static Member="skia:SKEncodedImageFormat.Png" />
                    <x:Static Member="skia:SKEncodedImageFormat.Wbmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Webp" />
                </x:Array>
            </Picker.ItemsSource>
        </Picker>

        <Slider x:Name="qualitySlider"
                Maximum="100"
                Value="50" />

        <Label Text="{Binding Source={x:Reference qualitySlider},
                              Path=Value,
                              StringFormat='Quality = {0:F0}'}"
               HorizontalTextAlignment="Center" />

        <StackLayout Orientation="Horizontal">
            <Label Text="Folder Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="folderNameEntry"
                   Text="SaveFileFormats"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="File Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="fileNameEntry"
                   Text="Sample.xxx"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <Button Text="Save" 
                Clicked="OnButtonClicked">
            <Button.Triggers>
                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference formatPicker},
                                               Path=SelectedIndex}"
                             Value="-1">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>

                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference fileNameEntry},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Button.Triggers>
        </Button>

        <Label x:Name="statusLabel"
               Text="OK"
               Margin="10, 0" />
    </StackLayout>
</ContentPage>
```

Použití modelu code-behind soubor prostředku rastrového obrázku načte a použije `SKCanvasView` ji zobrazíte. Nikdy bitmap, – změny. `SelectedIndexChanged` Obslužné rutiny pro `Picker` upraví název souboru s příponou, která je stejná jako člen výčtu:

```csharp
public partial class SaveFileFormatsPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(typeof(SaveFileFormatsPage),
        "SkiaSharpFormsDemos.Media.MonkeyFace.png");

    public SaveFileFormatsPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        args.Surface.Canvas.DrawBitmap(bitmap, args.Info.Rect, BitmapStretch.Uniform);
    }

    void OnFormatPickerChanged(object sender, EventArgs args)
    {
        if (formatPicker.SelectedIndex != -1)
        {
            SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
            fileNameEntry.Text = Path.ChangeExtension(fileNameEntry.Text, imageFormat.ToString());
            statusLabel.Text = "OK";
        }
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
        int quality = (int)qualitySlider.Value;

        using (MemoryStream memStream = new MemoryStream())
        using (SKManagedWStream wstream = new SKManagedWStream(memStream))
        {
            bitmap.Encode(wstream, imageFormat, quality);
            byte[] data = memStream.ToArray();

            if (data == null)
            {
                statusLabel.Text = "Encode returned null";
            }
            else if (data.Length == 0)
            {
                statusLabel.Text = "Encode returned empty array";
            }
            else
            {
                bool success = await DependencyService.Get<IPhotoLibrary>().
                    SavePhotoAsync(data, folderNameEntry.Text, fileNameEntry.Text);

                if (!success)
                {
                    statusLabel.Text = "SavePhotoAsync return false";
                }
                else
                {
                    statusLabel.Text = "Success!";
                }
            }
        }
    }
}
```

`Clicked` Obslužné rutiny pro `Button` funguje reálném. Získá dva argumenty pro `Encode` z `Picker` a `Slider`a pak používá starší vytvořit kód `SKManagedWStream` pro `Encode` metoda. Dva `Entry` poskytnout názvy souborů a složek pro zobrazení `SavePhotoAsync` metoda.

Většinu této metody se věnovala zpracování problémy nebo chyby. Pokud `Encode` vytvoří prázdné pole, znamená to, že tento formát daného souboru není podporován. Pokud `SavePhotoAsync` vrátí `false`, pak soubor nebyla úspěšně uložena. 

Tady je program běžící na třech platformách:

[![Uložit formáty souborů](saving-images/SaveFileFormats.png "uložit formáty souborů")](saving-images/SaveFileFormats-Large.png#lightbox)

Tento snímek obrazovky ukazuje jenom tři formáty, které jsou podporovány na těchto platformách:

- JPEG
- PNG
- WebP

Pro všechny ostatní formáty `Encode` metoda nic zapíše do datového proudu a výsledné bajtové pole je prázdné.

Rastrový obrázek, který **uložit formáty souborů** ukládání stránek je čtverec 600 pixelů. 4 bajtů na pixel, který je celkem 1,440,000 bajtů v paměti. V následující tabulce jsou uvedeny velikost souboru pro různé kombinace formát souboru a kvality:

|Formát|Kvalita|Velikost|
|------|------:|---:|
| PNG | Není k dispozici | 492 TIS. |
| JPEG | 0 | 2,95 TIS. |
|      | 50 | 22.1 K |
|      | 100 | 206 TIS. |
| WebP | 0 | 2,71 TIS. |
|      | 50 | 11,9 K |
|      | 100 | 101 TIS. |

Můžete experimentovat s různými nastaveními kvality a podívejte se na výsledky.

## <a name="saving-finger-paint-art"></a>Ukládá se finger-paint obrázky

Běžné využívání rastrový obrázek je v kreslení programy, kde funkce jak něco volána _rastrový obrázek stínové_. Kreslení zůstane na rastrový obrázek, který se následně zobrazí program. Rastrový obrázek také se hodí pro ukládání výkresu.

[ **Malování prstem v SkiaSharp** ](../paths/finger-paint.md) článku jsme vám ukázali, jak používat dotykové ovládání sledování k implementaci základní kreslení prsty, která program. Program podporuje pouze jednu barvu a šířku pouze jedním skokem však uchované celý kreslení v kolekci `SKPath` objekty.

**Malování prstem s uložit** stránku [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) ukázka také zachová celý kreslení v kolekci `SKPath` objekty, ale je také vykreslí kreslení na rastrový obrázek, který můžete uložit do knihovny fotografií.

Velká část tento program je podobný původní **Malování prstem** programu. Jedním rozšířením je, že soubor XAML nyní vytvoří tlačítek označené **vymazat** a **Uložit**:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Bitmaps.FingerPaintSavePage"
             Title="Finger Paint Save">

    <StackLayout>
        <Grid BackgroundColor="White"
              VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
        </Grid>

        <Button Text="Clear"
                Grid.Row="0"
                Margin="50, 5"
                Clicked="OnClearButtonClicked" />

        <Button Text="Save"
                Grid.Row="1"
                Margin="50, 5"
                Clicked="OnSaveButtonClicked" />

    </StackLayout>
</ContentPage>
```

Soubor kódu na pozadí udržuje pole typu `SKBitmap` s názvem `saveBitmap`. Vytvoří nebo znovu vytvořit v této rastrového obrázku `PaintSurface` obslužné rutiny pokaždé, když se povrch změny velikosti zobrazení. Pokud je nutné znovu vytvořit rastrový obrázek, obsah existujícího rastrového obrázku se zkopírují na nový rastrový obrázek tak, aby všechno, co se uchovávají bez ohledu na to, jak zobrazovacím povrchu se mění velikost:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    SKBitmap saveBitmap;

    public FingerPaintSavePage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        // Create bitmap the size of the display surface
        if (saveBitmap == null)
        {
            saveBitmap = new SKBitmap(info.Width, info.Height);
        }
        // Or create new bitmap for a new size of display surface
        else if (saveBitmap.Width < info.Width || saveBitmap.Height < info.Height)
        {
            SKBitmap newBitmap = new SKBitmap(Math.Max(saveBitmap.Width, info.Width),
                                              Math.Max(saveBitmap.Height, info.Height));

            using (SKCanvas newCanvas = new SKCanvas(newBitmap))
            {
                newCanvas.Clear();
                newCanvas.DrawBitmap(saveBitmap, 0, 0);
            }

            saveBitmap = newBitmap;
        }

        // Render the bitmap
        canvas.Clear();
        canvas.DrawBitmap(saveBitmap, 0, 0);
    }
    ···
}
```

Kreslení provádí `PaintSurface` dojde k velmi po uplynutí obslužné rutiny a se skládá pouze z vykreslování rastrového obrázku.

Dotykové ovládání zpracování je podobný starší programu. Program udržuje dvě kolekce `inProgressPaths` a `completedPaths`, obsahující vše, co uživatel má vykreslit od posledního zobrazení byl vymazán. Pro každou událost touch `OnTouchEffectAction` volání obsluhy `UpdateBitmap`:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                            (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }

    void UpdateBitmap()
    {
        using (SKCanvas saveBitmapCanvas = new SKCanvas(saveBitmap))
        {
            saveBitmapCanvas.Clear();

            foreach (SKPath path in completedPaths)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }

            foreach (SKPath path in inProgressPaths.Values)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }
        }

        canvasView.InvalidateSurface();
    }
    ···
}
```

`UpdateBitmap` Překreslí ho metoda `saveBitmap` vytvořením nového `SKCanvas`, zrušením zaškrtnutí a pak vykreslení všech cest v rastrového obrázku. Dojde k závěru, že podle zrušení platnosti `canvasView` tak, aby na displeji lze rozlišovat rastrového obrázku.

Tady jsou obslužné rutiny pro dvě tlačítka. **Vymazat** tlačítko vyčistí obě kolekce cestu, aktualizace `saveBitmap` (výsledkem vymazání rastrový obrázek) a zruší platnost `SKCanvasView`:

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    void OnClearButtonClicked(object sender, EventArgs args)
    {
        completedPaths.Clear();
        inProgressPaths.Clear();
        UpdateBitmap();
        canvasView.InvalidateSurface();
    }

    async void OnSaveButtonClicked(object sender, EventArgs args)
    {
        using (SKImage image = SKImage.FromBitmap(saveBitmap))
        {
            SKData data = image.Encode();
            DateTime dt = DateTime.Now;
            string filename = String.Format("FingerPaint-{0:D4}{1:D2}{2:D2}-{3:D2}{4:D2}{5:D2}{6:D3}.png",
                                            dt.Year, dt.Month, dt.Day, dt.Hour, dt.Minute, dt.Second, dt.Millisecond);

            IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
            bool result = await photoLibrary.SavePhotoAsync(data.ToArray(), "FingerPaint", filename);

            if (!result)
            {
                await DisplayAlert("FingerPaint", "Artwork could not be saved. Sorry!", "OK");
            }
        }
    }
}
```

**Uložit** tlačítko obslužná rutina používá zjednodušeného [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKImage.Encode()/) metodu z `SKImage`. Tato metoda kóduje ve formátu PNG. `SKImage` Objekt je vytvořen na základě `saveBitmap`a `SKData` objekt obsahuje kódovaný soubor PNG. 

`ToArray` Metoda `SKData` získá pole bajtů. Je to, co je předané metodě `SavePhotoAsync` metoda spolu s pevnou složka a jedinečný název souboru vytvořený z aktuální datum a čas.

Tady je program v akci:

[![Prsty malířského Uložit](saving-images/FingerPaintSave.png "prsty malířského uložit")](saving-images/FingerPaintSave-Large.png#lightbox)

Velmi podobné techniky je používán [ **SpinPaint** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SpinPaint/) vzorku. Toto je také kreslení prsty, která program, s tím rozdílem, že uživatel jsou vykreslovány pokryjte disku, který pak reprodukuje návrhy na čtyři quadrant. Barva změny Malování prstem jako disk otáčí:

[![Aktivovat Malování](saving-images/SpinPaint.png "aktivovat Malování")](saving-images/SpinPaint-Large.png#lightbox)

**Uložit** tlačítko `SpinPaint` třídy je podobná **Malování prstem** , uloží bitovou kopii do dlouhodobého složka (**SpainPaint**) a název souboru vytvořený z Datum a čas.

## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [SpinPaint (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SpinPaint/)
