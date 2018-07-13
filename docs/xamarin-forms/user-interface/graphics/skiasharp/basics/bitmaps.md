---
title: Bitmapa – základy ve ve Skiasharpu
description: Tento článek vysvětluje, jak načíst rastrové obrázky v SkiaSharp z různých zdrojů a jejich zobrazení v aplikacích Xamarin.Forms a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 04/03/2017
ms.openlocfilehash: dec6fa1534f14836ae98677ad33e280ff510fb97
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995182"
---
# <a name="bitmap-basics-in-skiasharp"></a>Bitmapa – základy ve ve Skiasharpu

_Načíst rastrové obrázky z různých zdrojů a jejich zobrazení._

Podpora rastrové obrázky v SkiaSharp je poměrně rozsáhlý. Tento článek se týká pouze Základy &mdash; zatížení rastrové obrázky a způsob jejich zobrazení:

![](bitmaps-images/bitmapssample.png "Zobrazení dvou rastrových obrázků")

Rastrový obrázek ve Skiasharpu je objekt typu [ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/). Existuje mnoho způsobů, jak vytvořit rastrový obrázek, ale v tomto článku omezuje na [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/SkiaSharp.SKStream/) metodu, která načte bitovou mapu ze [ `SKStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStream/) objekt, který odkazuje na soubor rastrového obrázku. Je vhodné použít [ `SKManagedStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKManagedStream/) třídu odvozenou od `SKStream` protože nemá konstruktor, který přijímá .NET [ `Stream` ](xref:System.IO.Stream) objektu.

**Základní rastrové obrázky** stránku **SkiaSharpFormsDemos** program ukazuje, jak načíst rastrové obrázky ze tří různých zdrojů:

- Z více než Internetu
- Ze zdroje vložený do spustitelného souboru
- Z knihovny fotografií uživatele

Tři `SKBitmap` objekty pro tyto tři zdroje se definují jako pole v [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) třídy:

```csharp
public class BasicBitmapsPage : ContentPage
{
    SKCanvasView canvasView;
    SKBitmap webBitmap;
    SKBitmap resourceBitmap;
    SKBitmap libraryBitmap;

    public BasicBitmapsPage()
    {
        Title = "Basic Bitmaps";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ...
    }
    ...
}
```

## <a name="loading-a-bitmap-from-the-web"></a>Načítání rastrový obrázek z webu

Načíst bitmapu podle adresy URL, můžete použít [ `WebRequest` ](xref:System.Net.WebRequest) třídy, jak je znázorněno v následujícím kódu proveden v `BasicBitmapsPage` konstruktoru. Adresa URL zde odkazuje na oblast na webu Xamarin se některé ukázkové rastrové obrázky. Balíček na webu umožňuje přidávání specifikace pro změnu velikosti rastrový obrázek pro konkrétní width:

```csharp
Uri uri = new Uri("http://developer.xamarin.com/demo/IMG_3256.JPG?width=480");
WebRequest request = WebRequest.Create(uri);
request.BeginGetResponse((IAsyncResult arg) =>
{
    try
    {
        using (Stream stream = request.EndGetResponse(arg).GetResponseStream())
        using (MemoryStream memStream = new MemoryStream())
        {
            stream.CopyTo(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            using (SKManagedStream skStream = new SKManagedStream(memStream))
            {
                webBitmap = SKBitmap.Decode(skStream);
            }
        }
    }
    catch
    {
    }

    Device.BeginInvokeOnMainThread(() => canvasView.InvalidateSurface());

}, null);
```

Po úspěšném stažení rastrový obrázek, metody zpětného volání předána `BeginGetResponse` metoda spuštění. `EndGetResponse` Musí být ve volání `try` blokovat v případě, že došlo k chybě. `Stream` Získaného z `GetResponseStream` není dostatečné na některých platformách, takže obsah rastrového obrázku se zkopíruje do `MemoryStream` objektu. V tomto okamžiku `SKManagedStream` objekt může být vytvořen. Tuto chybu odkazuje na soubor rastrového obrázku, který je pravděpodobně soubor JPEG nebo PNG. `SKBitmap.Decode` Metoda dekóduje rastrového obrázku a ukládá výsledky v interním formátu ve Skiasharpu.

Metoda zpětného volání předána `BeginGetResponse` spustí po dokončení provádění, konstruktor, což znamená, `SKCanvasView` je potřeba ukončit platnost umožňující `PaintSurface` obslužné rutiny aktualizace zobrazíte. Ale `BeginGetResponse` zpětného volání běží sekundární vlákno provádění, takže je nutné použít `Device.BeginInvokeOnMainThread` ke spuštění `InvalidateSurface` metodu ve vlákně uživatelského rozhraní.

## <a name="loading-a-bitmap-resource"></a>Načítání prostředku rastrového obrázku

Z hlediska kódu je nejjednodušší přístup k načítání rastrové obrázky včetně prostředku rastrového obrázku přímo ve vaší aplikaci. **SkiaSharpFormsDemos** program obsahuje složku s názvem **média** obsahující soubor rastrového obrázku s názvem **monkey.png**. V **vlastnosti** dialogové okno pro tento soubor je třeba zadat takový soubor **akce sestavení** z **integrovaný prostředek**!

Má každý vložený prostředek *ID prostředku* , který se skládá z názvu projektu, složku a název souboru, všechny připojené podle období: **SkiaSharpFormsDemos.Media.monkey.png**. Přístup k tomuto prostředku můžete získat tak, že zadáte tento prostředek ID jako argument [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) metodu [ `Assembly` ](xref:System.Reflection.Assembly) třídy:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
using (SKManagedStream skStream = new SKManagedStream(stream))
{
    resourceBitmap = SKBitmap.Decode(skStream);
}
```

To `Stream` objekt převést přímo do `SKManagedStream` objektu.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Načítání rastrový obrázek z knihovny fotografií

Je také možné pro uživatele k načtení fotografii z knihovny obrázků zařízení. Toto zařízení není k dispozici pomocí Xamarin.Forms, samotného. Úloha vyžaduje závislou službu, jako jsou popsané v článku [výběr z knihovny obrázků fotografie](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

**IPicturePicker.cs** souboru a tři **PicturePickerImplementation.cs** soubory z tohoto článku byly zkopírovány do různých projektů **SkiaSharpFormsDemos**řešení a nové názvy oborů názvů. Kromě toho, Android **MainActivity.cs** soubor byl změněn, jak je popsáno v článku a projekt pro iOS se předala oprávnění pro přístup k knihovny fotografií se dvěma řádky směrem k dolní části **info.plist**  souboru.

`BasicBitmapsPage` Přidá konstruktor `TapGestureRecognizer` k `SKCanvasView` upozornit odposlouchávání. Po obdržení klepnutím na `Tapped` obslužná rutina získá přístup ke službě závislost obrázek výběru a volání `GetImageStreamAsync`. Pokud `Stream` je vrácen objekt a potom se obsah zkopíruje do `MemoryStream`, jak je požadováno v některé z platforem. Zbývající část kódu je podobný dvě techniky:

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPicturePicker picturePicker = DependencyService.Get<IPicturePicker>();

    using (Stream stream = await picturePicker.GetImageStreamAsync())
    {
        if (stream != null)
        {
            using (MemoryStream memStream = new MemoryStream())
            {
                stream.CopyTo(memStream);
                memStream.Seek(0, SeekOrigin.Begin);

                using (SKManagedStream skStream = new SKManagedStream(memStream))
                {
                    libraryBitmap = SKBitmap.Decode(skStream);
                }
            }
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

Všimněte si, že `Tapped` volání obslužné rutiny `InvalidateSurface` metodu `SKCanvasView` objektu. Tím se vytvoří nové volání `PaintSurface` obslužné rutiny.

## <a name="displaying-the-bitmaps"></a>Zobrazení rastrové obrázky

`PaintSurface` Obslužná rutina potřebuje rychle zobrazit tři rastrových obrázků. Obslužná rutina předpokládá, že telefon je v režimu na výšku a svisle rozdělí tři stejné části plátna.

Zobrazí se první rastrového obrázku s nejjednodušší [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) metody. Všechno, co je třeba zadat jsou souřadnice X a Y, kde má být umístěn levém horním rohu rastrového obrázku:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

I když `SKPaint` je definován parametr, má výchozí hodnotu `null` a můžete ji ignorovat. Pixely rastrového obrázku jednoduše přenese na pixelech zobrazovacím povrchu se mapování 1: 1.

Program můžete získat pixel rozměry rastrový obrázek s [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) a [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/) vlastnosti. Tyto vlastnosti povolit program k výpočtu souřadnice, kde umístit rastrového obrázku ve středu horní třetí plátna:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    if (webBitmap != null)
    {
        float x = (info.Width - webBitmap.Width) / 2;
        float y = (info.Height / 3 - webBitmap.Height) / 2;
        canvas.DrawBitmap(webBitmap, x, y);
    }
    ...
}
```

Další dvě rastrových obrázků, které se zobrazují s verzí [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) s `SKRect` parametr:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Třetí verzi [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) má dva `SKRect` argumenty pro zadání obdélníkové podmnožiny rastrový obrázek pro zobrazení, ale tato verze není použit v tomto článku.

Tady je kód, který zobrazí rastrový obrázek načten ze integrovaný prostředek rastrového obrázku:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (resourceBitmap != null)
    {
        canvas.DrawBitmap(resourceBitmap,
            new SKRect(0, info.Height / 3, info.Width, 2 * info.Height / 3));
    }
    ...
}
```

Rastrový obrázek je roztažen tak, aby dimenze obdélníku, což je důvod, proč opic je vodorovně roztažená tyto snímcích obrazovky:

[![](bitmaps-images/basicbitmaps-small.png "Trojitá snímek obrazovky stránky základní rastrové obrázky")](bitmaps-images/basicbitmaps-large.png#lightbox "Trojitá snímek obrazovky stránky základní rastrových obrázků")

Třetího bitové kopie &mdash; to můžete vidět pouze pokud spuštění programu a načíst z knihovny obrázků fotografie &mdash; se také zobrazí v obdélníku, ale obdélníku pozici a velikost tak, aby zachovat poměr stran rastrového obrázku. Tento výpočet je o něco složitější, protože vyžaduje výpočet měřítko na základě velikosti rastrového obrázku a cílového obdélníku a zarovnání na střed obdélníku v této oblasti:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (libraryBitmap != null)
    {
        float scale = Math.Min((float)info.Width / libraryBitmap.Width,
                               info.Height / 3f / libraryBitmap.Height);

        float left = (info.Width - scale * libraryBitmap.Width) / 2;
        float top = (info.Height / 3 - scale * libraryBitmap.Height) / 2;
        float right = left + scale * libraryBitmap.Width;
        float bottom = top + scale * libraryBitmap.Height;
        SKRect rect = new SKRect(left, top, right, bottom);
        rect.Offset(0, 2 * info.Height / 3);

        canvas.DrawBitmap(libraryBitmap, rect);
    }
    else
    {
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Blue;
            paint.TextAlign = SKTextAlign.Center;
            paint.TextSize = 48;

            canvas.DrawText("Tap to load bitmap",
                info.Width / 2, 5 * info.Height / 6, paint);
        }
    }
}
```

Pokud má nebyly načteny žádné rastrový obrázek z knihovny obrázků, pak bude `else` bloku zobrazí text, který chcete vyzvat uživatele k klepněte na obrazovku.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Výběr z knihovny obrázků fotografie](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
