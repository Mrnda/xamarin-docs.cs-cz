---
title: Bitmap Basics
description: Načíst rastrové obrázky z různých zdrojů a jejich zobrazení.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 04/03/2017
ms.openlocfilehash: b86068c2ed5063c25f76e81fdf477550b1437984
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="bitmap-basics"></a>Bitmap Basics

_Načíst rastrové obrázky z různých zdrojů a jejich zobrazení._

Podpora rastrových obrázků v SkiaSharp je poměrně rozsáhlé. Tento článek se týká pouze Základy &mdash; jak načíst rastrové obrázky a jejich zobrazení:

![](bitmaps-images/bitmapssample.png "Zobrazení dvě bitové mapy")

Rastrový obrázek SkiaSharp je objekt typu [ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/). Existuje celá řada způsobů pro vytvoření rastrového obrázku, ale v tomto článku omezuje, aby se do [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/SkiaSharp.SKStream/) metodu, která načte bitovou mapu z [ `SKStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStream/) objekt, který odkazuje na soubor rastrového obrázku. Je možné použít [ `SKManagedStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKManagedStream/) třídu odvozenou od `SKStream` protože má konstruktor, který přijímá .NET [ `Stream` ](https://developer.xamarin.com/api/type/System.IO.Stream/) objektu.

**Základní bitmap** stránku **SkiaSharpFormsDemos** program ukazuje, jak načíst rastrové obrázky ze tří různých zdrojů:

- Příliš Internetu
- Z prostředku vložených ve spustitelném souboru
- Z knihovny fotografií uživatele

Tři `SKBitmap` objekty pro tyto tři zdroje jsou definovány jako pole v [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) třídy:

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

## <a name="loading-a-bitmap-from-the-web"></a>Načítání bitmapy z webu

Načíst rastrový obrázek založené na adresu URL, můžete použít [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) třídy, jak je znázorněno v následujícím kódu spustit v `BasicBitmapsPage` konstruktor. Adresu URL sem odkazuje na oblast na webu Xamarin s bitovými mapami některé ukázka. Balíček na webu umožňuje připojování specifikace pro změnu velikosti rastrového obrázku na konkrétní šířku:

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

Když bitmapy byly úspěšně staženy, metody zpětného volání předaný `BeginGetResponse` metoda spustí. `EndGetResponse` Volání musí být ve `try` blokovat v případě, že došlo k chybě. `Stream` Získaného z `GetResponseStream` není odpovídající na některých platformách, a proto rastrový obrázek obsahu se zkopírují do `MemoryStream` objektu. V tomto okamžiku `SKManagedStream` můžete vytvořit objekt. Tuto chybu odkazuje rastrového obrázku, který je pravděpodobně soubor JPEG nebo PNG. `SKBitmap.Decode` Metoda dekóduje rastrového obrázku a ukládá výsledky do interní SkiaSharp formátu.

Metoda zpětného volání předaný `BeginGetResponse` spustí po dokončení provádění, konstruktoru, což znamená, které `SKCanvasView` musí být zrušena umožňující `PaintSurface` obslužná rutina aktualizace zobrazení. Ale `BeginGetResponse` zpětného volání běží sekundární podproces provádění, takže je nutné použít `Device.BeginInvokeOnMainThread` ke spuštění `InvalidateSurface` metoda ve vláknu uživatelského rozhraní.

## <a name="loading-a-bitmap-resource"></a>Načítání prostředků rastrového obrázku

Z hlediska kódu je snadný přístup k načítání bitmap včetně prostředků rastrový obrázek přímo v aplikaci. **SkiaSharpFormsDemos** program obsahuje složku s názvem **média** obsahující soubor rastrového obrázku s názvem **monkey.png**. V **vlastnosti** dialogové okno pro tento soubor musí dát takový soubor **akce sestavení** z **vložený prostředek**!

Má každý vložený prostředek *ID prostředku* , se skládá z název projektu, složku a název souboru, všechny připojené podle období: **SkiaSharpFormsDemos.Media.monkey.png**. Přístup k tomuto prostředku můžete získat tak, že zadáte tento prostředek ID jako argument pro [ `GetManifestResourceStream` ](https://developer.xamarin.com/api/member/System.Reflection.Assembly.GetManifestResourceStream/p/System.String/) metodu [ `Assembly` ](https://developer.xamarin.com/api/type/System.Reflection.Assembly/) třídy:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
using (SKManagedStream skStream = new SKManagedStream(stream))
{
    resourceBitmap = SKBitmap.Decode(skStream);
}
```

To `Stream` přímo na možné převést objekt `SKManagedStream` objektu.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Načítání bitmapy z knihovny fotografie

Je také možné uživatele, pro zatížení fotografie z knihovny obrázků zařízení. Pokud tuto funkci není poskytovaný Xamarin.Forms sám sebe. Úloha vyžaduje službu závislosti, jako jsou popsané v článku [výběr fotografie z knihovny obrázků](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

**IPicturePicker.cs** souboru a tři **PicturePickerImplementation.cs** soubory z tohoto článku byly zkopírovány do různých projektů **SkiaSharpFormsDemos**řešení a zadané nové názvy oborů názvů. Kromě toho pro Android **MainActivity.cs** soubor byl upraven, jak je popsáno v článku a projekt pro iOS byla udělil oprávnění k přístupu ke knihovně fotografie se dvěma řádky směrem k dolní části **info.plist**  souboru.

`BasicBitmapsPage` Přidá konstruktor `TapGestureRecognizer` k `SKCanvasView` upozornit odposlouchávání. Po obdržení klepněte `Tapped` obslužná rutina získá přístup k výběru obrázek závislostí služby a volání `GetImageStreamAsync`. Pokud `Stream` se vrátí objekt a potom obsah je zkopírován do `MemoryStream`, podle požadavku některé platformy. Zbytek kód je podobná další dvě metody:

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

Všimněte si, že `Tapped` volání obslužné rutiny `InvalidateSurface` metodu `SKCanvasView` objektu. Tím se vygeneruje nové volání `PaintSurface` obslužné rutiny.

## <a name="displaying-the-bitmaps"></a>Zobrazení rastrových obrázků

`PaintSurface` Obslužné rutiny musí zobrazit tři bitmapy. Obslužná rutina předpokládá, že telefon je v režimu na výšku a rozdělí na plátno svisle na tři stejné části.

První rastrového obrázku se zobrazí se i ty nejjednodušší [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) metoda. Všechny, které je třeba zadat jsou souřadnice X a Y, kde má být umístěn levém horním rohu bitmapy:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

I když `SKPaint` parametr je definovaný, má výchozí hodnotu `null` a můžete ji ignorovat. Pixelů bitmapy přenesou jednoduše pixelů zobrazení plochy s mapování 1: 1.

Program můžete získat dimenze pixelů rastru pomocí [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) a [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/) vlastnosti. Tyto vlastnosti povolit program vypočítat souřadnice na pozici bitovou mapu ve středu třetí horní plátna:

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

S verzí se zobrazují dvě bitmapy [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) s `SKRect` parametr:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Třetí verzi [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) má dva `SKRect` argumenty pro zadání podmnožinu rastrový obrázek zobrazit, ale tuto verzi obdélníková není použit v tomto článku.

Tady je kód, který zobrazí rastrový obrázek načtené ze rastrový obrázek vložený prostředek:

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

Rastrový obrázek je roztažen tak, aby dimenze obdélníku, proto opic vodorovně roztažen tak tyto snímcích obrazovky:

[![](bitmaps-images/basicbitmaps-small.png "Trojitá snímek obrazovky stránky základní bitmap")](bitmaps-images/basicbitmaps-large.png#lightbox "Trojitá snímek obrazovky stránky základní rastrové obrázky")

Bitovou kopii třetí &mdash; které lze zobrazit pouze pokud spuštění programu a načíst fotografie z vlastní knihovny obrázků &mdash; se zobrazí také v obdélníku, ale obdélníku pozice a velikosti upraveny tak, aby zachovat poměr stran rastrového obrázku. Tohoto výpočtu je trochu složitější, protože vyžaduje výpočet měřítko podle velikosti bitovou mapu a rámeček cílové a zarovnání obdélníku v této oblasti:

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

Pokud žádné rastrový obrázek ještě byl načten z knihovny obrázků, pak se `else` bloku zobrazí text pro vyzvání uživatele klepnout na obrazovce.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Výběr fotografie z knihovny obrázků](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
