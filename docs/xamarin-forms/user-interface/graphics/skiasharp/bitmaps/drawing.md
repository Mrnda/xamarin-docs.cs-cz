---
title: Vytváření a kreslení ve Skiasharpu rastrových obrázků
description: Zjistěte, jak vytvořit ve Skiasharpu rastrové obrázky a nakreslete na tyto rastrové obrázky tak, že vytvoříte plátno, na jejich základě.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 79BD3266-D457-4E50-BDDF-33450035FA0F
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: fa32b2bdb95044c8171542ff4156ec3c15747372
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130990"
---
# <a name="creating-and-drawing-on-skiasharp-bitmaps"></a>Vytváření a kreslení ve Skiasharpu rastrových obrázků

Už víte, jak můžou aplikace načíst rastrové obrázky z webu, ze zdrojů aplikace a z knihovny fotografií uživatele. Je také možné vytvořit nové rastrové obrázky v rámci vaší aplikace. Nejjednodušším přístupem zahrnuje jednu konstruktory [ `SKBitmap` ](https://developer.xamarin.com/api/constructor/SkiaSharp.SKBitmap.SKBitmap/p/System.Int32/System.Int32/System.Boolean/):

```csharp
SKBitmap bitmap = new SKBitmap(width, height);
```

`width` a `height` parametry jsou celá čísla a určují rozměry pixel rastrového obrázku. Tento konstruktor vytvoří rastrový obrázek barevné čtyř bajtů na pixel: jeden bajt každý pro červená, zelená, modrá a komponenty alpha (neprůhlednost). 

Poté, co jste vytvořili nový rastrový obrázek, je nutné získat něco na povrchu rastrového obrázku. Obvykle to provedete jedním ze dvou způsobů:

- Nakreslit bitmapou pomocí standardní `Canvas` kreslení metody.
- Bitů pixel přímý přístup.

Tento článek popisuje první přístup:

![Vzorek](drawing-images/DrawingSample.png "vzorek")

Druhý postup je popsán v článku [ **přístup k pixelů rastrového obrázku ve Skiasharpu**](pixel-bits.md).

## <a name="drawing-on-the-bitmap"></a>Na základě rastrového obrázku

Kreslení na ploše rastrový obrázek je stejný jako kreslení v zobrazení videa. Kreslení v zobrazení videa, získáte `SKCanvas` objektu z `PaintSurface` argumenty události. Chcete-li nakreslit na rastrový obrázek, můžete vytvořit `SKCanvas` pomocí [ `SKCanvas` ](https://developer.xamarin.com/api/constructor/SkiaSharp.SKCanvas.SKCanvas/p/SkiaSharp.SKBitmap/) konstruktor:

```csharp
SKCanvas canvas = new SKCanvas(bitmap);
```

Jakmile budete hotovi, na základě rastrový obrázek, můžete vyřazení `SKCanvas` objektu. Z tohoto důvodu `SKCanvas` konstruktor je obvykle volána `using` – příkaz:

```csharp
using (SKCanvas canvas = new SKCanvas(bitmap))
{
    ··· // call drawing function
}
```

Rastrový obrázek je pak možné zobrazit. Později, můžete vytvořit nový program `SKCanvas` objektu na základě stejné rastrového obrázku a kreslit v něm dál.

**Hello bitmapy** stránku **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** aplikace zapíše text "Hello, rastrový obrázek!" na rastrový obrázek a potom zobrazí rastrový více než jednou.  

Konstruktor třídy `HelloBitmapPage` začíná tím, že vytvoříte `SKPaint` objektu pro zobrazení textu. Určuje rozměry v podobě textového řetězce a vytvoří se tyto rozměry rastrový obrázek. Potom vytvoří `SKCanvas` objektu podle tento rastrový obrázek, volání `Clear`a pak zavolá `DrawText`. Je vždy vhodné volat `Clear` s Nový rastrový obrázek vzhledem k tomu, že nově vytvořený rastrový obrázek může obsahovat náhodná data.

Konstruktor končí tím, že vytvoříte `SKCanvasView` zobrazíte rastrového obrázku:

```csharp
public partial class HelloBitmapPage : ContentPage
{
    const string TEXT = "Hello, Bitmap!";
    SKBitmap helloBitmap;

    public HelloBitmapPage()
    {
        Title = TEXT;

        // Create bitmap and draw on it
        using (SKPaint textPaint = new SKPaint { TextSize = 48 })
        {
            SKRect bounds = new SKRect();
            textPaint.MeasureText(TEXT, ref bounds);

            helloBitmap = new SKBitmap((int)bounds.Right,
                                       (int)bounds.Height);

            using (SKCanvas bitmapCanvas = new SKCanvas(helloBitmap))
            {
                bitmapCanvas.Clear();
                bitmapCanvas.DrawText(TEXT, 0, -bounds.Top, textPaint);
            }
        }

        // Create SKCanvasView to view result 
        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Aqua);

        for (float y = 0; y < info.Height; y += helloBitmap.Height)
            for (float x = 0; x < info.Width; x += helloBitmap.Width)
            {
                canvas.DrawBitmap(helloBitmap, x, y);
            }
    }
}
```

`PaintSurface` Rastrový obrázek více než jednou v řádků a sloupců zobrazení vykreslí obslužné rutiny. Všimněte si, že `Clear` metodu `PaintSurface` obslužná rutina nemá argument `SKColors.Aqua`, který barvy pozadí zobrazovacím povrchu:

[![Dobrý den, rastrový obrázek! ] (drawing-images/HelloBitmap.png "Hello, rastrový obrázek!")](drawing-images/HelloBitmap-Large.png#lightbox)

Vzhled akvamarínový pozadí odhalí, že je transparentní, s výjimkou text rastrového obrázku.

## <a name="clearing-and-transparency"></a>Vymazání a transparentnost

Zobrazení **Hello bitmapy** stránce ukazuje, že rastrový obrázek aplikaci vytvořili je transparentní, s výjimkou černý text. To je důvod, proč zobrazena akvamarínový barva zobrazovacím povrchu.

V dokumentaci `Clear` metody `SKCanvas` popisuje pomocí příkazu: "Nahradí všechny pixely aktuálního klipu na plátno." Použijte slova "nahrazuje" odhalí důležitou vlastnost z těchto metod: všechna vykreslení metody `SKCanvas` přidat nějakou existující zobrazovacím povrchu. `Clear` Metody _nahradit_ co je již nainstalováno.

`Clear` existuje ve dvou verzích: 

- [ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear/p/SkiaSharp.SKColor/) Metodou `SKColor` parametr nahradí pixelů zobrazovacím povrchu barvy v pixelech.

- [ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) Metoda bez parametrů nahradí obrazových bodů [ `SKColors.Empty` ](https://developer.xamarin.com/api/property/SkiaSharp.SKColors.Empty/) barvu, která je barva, ve kterém jsou všechny součásti (červená, zelená, modrá a alfa) jsou nastaveny na hodnotu nula. Tato barva se někdy označuje jako "průhledný černý."

Volání `Clear` bez argumentů na nový rastrový obrázek inicializuje celý rastrový obrázek zcela transparentní. Nic následně vykreslen na rastrový obrázek bude obvykle částečně neprůhledné nebo neprůhledný.

Tady je něco, co akci: V **Hello bitmapy** stránka, nahraďte `Clear` metoda u `bitmapCanvas` s touto položkou:

```csharp
bitmapCanvas.Clear(new SKColor(255, 0, 0, 128));
```

Pořadí `SKColor` parametry konstruktoru je červené, zelená, modrá a alfa, kde je každá hodnota může být v rozsahu od 0 do 255. Mějte na paměti, že je transparentní, při alfa hodnotu 255 alfa hodnotu 0 je skrytá.

Hodnota (255, 0, 0, 128) vymaže pixelů rastrového obrázku na červené pixelů s 50 % neprůhlednosti. To znamená, že je poloprůhledného rastrového obrázku na pozadí. Kombinuje poloprůhledného red pozadí rastrového obrázku s akvamarínový pozadí zobrazovacím povrchu vytvořit šedé pozadí.

Zkuste nastavit barvu textu průhledný černý tak, že vložíte následující přiřazení `SKPaint` inicializátoru:

```csharp
Color = new SKColor(0, 0, 0, 0)
```

Si možná myslíte, že tento transparentní text by vytvořit úplně průhledné oblasti rastrového obrázku, přes který zobrazoval akvamarínový pozadí zobrazovacím povrchu. To ale znamená ne tak. Text je vykreslen na co je již na rastrový obrázek. Transparentní text se nezobrazí vůbec.

Ne `Draw` metoda někdy provádí rastrový obrázek více transparentní. Pouze `Clear` můžete to provést.

## <a name="bitmap-color-types"></a>Typy barvy rastrového obrázku

Nejjednodušší `SKBitmap` konstruktor umožňuje zadat celé číslo šířka v pixelech a výšku rastrového obrázku. Další `SKBitmap` konstruktory jsou složitější. Tyto konstruktory vyžadují argumenty ze dvou typů výčtu: [ `SKColorType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColorType/) a [ `SKAlphaType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKAlphaType/). Použít další konstruktory [ `SKImageInfo` ](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/) strukturu, která se slučuje tyto informace.

`SKColorType` Výčet má 9 členy. Každý z těchto členů popisuje určitým způsobem ukládání obrazových bodů rastrového obrázku:

- `Unknown`
- `Alpha8` &mdash; Každý pixel je 8 bitů reprezentující alfa hodnotu z zcela průhledná úplně neprůhledná
- `Rgb565` &mdash; Každý pixel, je 16 bitů, 5 bity pro červené a modré a 6 pro zelená
- `Argb4444` &mdash; Každý pixel, je 16 bitů, 4 pro platformu alpha, červené, zelené a modré
- `Rgba8888` &mdash; Každý pixel je 32 bitů, 8 pro červená, zelená, modrá a alfa
- `Bgra8888` &mdash; Každý pixel je 32 bitů, 8 pro modrý, zelená, červenou a alfa
- `Index8` &mdash; Každý pixel je 8 bitů a představuje index do [`SKColorTable`](https://developer.xamarin.com/api/type/SkiaSharp.SKColorTable/)
- `Gray8` &mdash; Každý pixel je 8 bitů představuje šedé odstín od černé na bílou
- `RgbaF16` &mdash; Každý pixel je 64 bitů s červená, zelená, modrá a alfa ve formátu s plovoucí desetinnou čárkou 16 bitů

Dva formáty, kde je každý pixel 32 pixelů (4 bajtů) se často nazývají _barevné_ formátů. Mnoho dalších formátů data z doby, kdy video zobrazí sami z nebyly schopné plnou barvu. Rastrové obrázky omezené barvy byly dostatečné pro tyto zobrazí a povolený rastrové obrázky, aby obsadily méně místa v paměti. 

V dnešních dnech programátoři téměř vždy použijte rastrové obrázky plnými barvami a nemusíte zabývat jiných formátů. Výjimkou je `RgbaF16` formátu, který umožňuje vyšší rozlišení barev než i formáty plnými barvami. Ale tento formát se používá pro speciální účely, jako jsou lékařské vytvoření bitové kopie a moc nedává smysl při použití se zobrazí standardní plnými barvami.

Tato série článků samotného omezí `SKBitmap` barva formátů používaných ve výchozím nastavení když ne `SKColorType` člen je určený. Tento výchozí formát je založen na základní platformy. Platformách podporovaných aplikací Xamarin.Forms je výchozí typ barvy:

- `Rgba8888` pro zařízení s iOS a Android
- `Bgra8888` pro UPW

Jediný rozdíl je v tom pořadí 4 bajtů v paměti, a to pouze stává problémem, když se dostanete přímo bitů pixel. To se stává důležité, dokud se nedostanete na článek [ **přístup k pixelů rastrového obrázku ve Skiasharpu**](pixel-bits.md).

`SKAlphaType` Výčet má čtyři členy:

- `Unknown`
- `Opaque` &mdash; Bitmapa má bez průhlednosti
- `Premul` &mdash; barevných složek jsou předem vynásobené alfa
- `Unpremul` &mdash; barevné složky nejsou předem vynásobené alfa

Tady je pixel red rastrového obrázku na 4 bajty s 50 % průhledností bajtů podle pořadí červená, zelená, modrá, alfa:

0xFF 0x00 0x00 0x80

Při vykreslení rastrový obrázek obsahující poloprůhledného pixelů na zobrazovacím povrchu, komponenty barvu každého obrazového bodu rastrový obrázek musí vynásobené alfa obrazového a musí se vynásobí barevným odpovídající pixelu zobrazovacím povrchu podle 255 mínus hodnota alfa. Můžete kombinovat pak dvě pixelů. Rastrový obrázek můžou zobrazovat rychleji barevným v pixelech rastrového obrázku již byly pre násobeným hodnotu alfa. Stejné red obrazového bodu tímto způsobem budou uloženy v předem vynásobený formát:

0x80 0x00 0x00 0x80

Toto zvýšení výkonu je důvod, proč `SkiaSharp` rastrové obrázky ve výchozím nastavení jsou vytvořeny pomocí `Premul` formátu. Ale znovu, bude nutné pouze v případě, že přístup k a manipulaci s bitů na pixel to vědět.

## <a name="drawing-on-existing-bitmaps"></a>Na základě existující rastrových obrázků

Není nutné vytvořit nový rastrový obrázek, chcete-li nakreslit na něj. Můžete také nakreslit existujícího rastrového obrázku. 

**Opic Moustache** stránka používá k načtení konstruktoru **MonkeyFace.png** bitové kopie. Potom vytvoří `SKCanvas` objektu podle tento rastrový obrázek a používá `SKPaint` a `SKPath` objektů pro kreslení moustache na něj:

```csharp
public partial class MonkeyMoustachePage : ContentPage
{
    SKBitmap monkeyBitmap;

    public MonkeyMoustachePage()
    {
        Title = "Monkey Moustache";

        monkeyBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MonkeyFace.png");

        // Create canvas based on bitmap
        using (SKCanvas canvas = new SKCanvas(monkeyBitmap))
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                paint.StrokeWidth = 24;
                paint.StrokeCap = SKStrokeCap.Round;

                using (SKPath path = new SKPath())
                {
                    path.MoveTo(380, 390);
                    path.CubicTo(560, 390, 560, 280, 500, 280);

                    path.MoveTo(320, 390);
                    path.CubicTo(140, 390, 140, 280, 200, 280);

                    canvas.DrawPath(path, paint);
                }
            }
        }

        // Create SKCanvasView to view result 
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
        canvas.DrawBitmap(monkeyBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Konstruktor končí tím, že vytvoříte `SKCanvasView` jehož `PaintSurface` obslužná rutina jednoduše zobrazuje výsledek:

[![Opic Moustache](drawing-images/MonkeyMoustache.png "opic Moustache")](drawing-images/MonkeyMoustache-Large.png#lightbox)

## <a name="copying-and-modifying-bitmaps"></a>Kopírování a úpravy rastrových obrázků

Metody `SKCanvas` , můžete použít k vykreslení rastrový obrázek zahrnují `DrawBitmap`. To znamená, že lze nakreslit jednoho rastrového obrázku na jiný, obvykle úpravy nějakým způsobem.

Nejvíce univerzální způsob, jak upravit rastrový obrázek je přístup k bits skutečné pixel, předmět popsané v článku  **[přístup k ve Skiasharpu rastrový obrázek pixelů](pixel-bits.md)**. Existuje ale spoustu jiných postupů k úpravě rastrové obrázky, nevyžadují přístup k bitů pixel.

Součástí následující rastrového obrázku **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** aplikace je 360 pixelů na šířku a 480 pixelů na výšku:

![Horská oblast Climbers](drawing-images/MountainClimbers.jpg "Climbers Horská oblast")

Předpokládejme, že jste neobdrželi oprávnění z opic na levé straně publikovat tuto fotografii. Jedním z řešení je nesrozumitelné rozpoznávání tváře opic pomocí techniky označované jako _pixelization_. Pixelů rozpoznávání tváře jsme nahradili bloky barvy, nemůžete provádět funkce. Barevné bloky jsou obvykle odvozena z původní bitové kopie zprůměrováním barvy obrazových bodů odpovídající tyto bloky. Ale není nutné k provedení tohoto výpočtu průměru sami. Dojde automaticky při kopírování rastrový obrázek do menší rozměr pixelů. 

Rozpoznávání tváře levé opic zabírá zhruba na 72 pixelů Čtvereček oblast s levého horního rohu v okamžiku (112, 238). Podíváme nahradit tuto oblast Čtvereček 72 pixelů 9 x 9 škálu barevné kostky, z nichž každý je 8 8 pixelech čtvereček.

**Pixelize Image** stránka načte v tento rastrový obrázek a nejprve vytvoří malý 9 pixel Čtvereček rastrový obrázek volá `faceBitmap`. Toto je cíl kopírování jen opic tváře. Cílového obdélníku je jenom 9pixelů čtverec, ale zdrojového obdélníku je čtverec 72 pixelů. Každý blok 8 8 pixelech zdroje jsou konsolidovány do právě jeden pixel zprůměrováním barvy.

Dalším krokem je zkopírovat původní rastrového obrázku na nový rastrový obrázek stejné velikosti volá `pixelizedBitmap`. Velmi malé `faceBitmap` se pak zkopíruje takovou s 72 pixelů Čtvereček cílového obdélníku tak, aby každý pixel `faceBitmap` rozbalen 8krát velikost:

```csharp
public class PixelizedImagePage : ContentPage
{
    SKBitmap pixelizedBitmap;

    public PixelizedImagePage ()
    {
        Title = "Pixelize Image";

        SKBitmap originalBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        // Create tiny bitmap for pixelized face
        SKBitmap faceBitmap = new SKBitmap(9, 9);

        // Copy subset of original bitmap to that
        using (SKCanvas canvas = new SKCanvas(faceBitmap))
        {
            canvas.Clear();
            canvas.DrawBitmap(originalBitmap,
                              new SKRect(112, 238, 184, 310),   // source
                              new SKRect(0, 0, 9, 9));          // destination

        }

        // Create full-sized bitmap for copy
        pixelizedBitmap = new SKBitmap(originalBitmap.Width, originalBitmap.Height);

        using (SKCanvas canvas = new SKCanvas(pixelizedBitmap))
        {
            canvas.Clear();

            // Draw original in full size
            canvas.DrawBitmap(originalBitmap, new SKPoint());

            // Draw tiny bitmap to cover face
            canvas.DrawBitmap(faceBitmap, 
                              new SKRect(112, 238, 184, 310));  // destination
        }

        // Create SKCanvasView to view result
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
        canvas.DrawBitmap(pixelizedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Konstruktor končí tím, že vytvoříte `SKCanvasView` k zobrazení výsledku:

[![Pixelize Image](drawing-images/PixelizeImage.png "Pixelize bitové kopie")](drawing-images/PixelizeImage-Large.png#lightbox)

<a name="rotating-bitmaps" />

## <a name="rotating-bitmaps"></a>Otočení rastrových obrázků

Jiné běžné úlohy je otáčení rastrových obrázků. To je užitečné hlavně při načítání rastrové obrázky z knihovny fotografií zařízení iPhone nebo iPad. Pokud máte zařízení v konkrétní orientaci při pořízení fotky, bude pravděpodobně vzhůru nohama nebo směřující na obrázku.

Zapnutí rastrový obrázek vzhůru nohama vyžaduje vytvoření jiného rastrový obrázek stejnou velikost jako první a pak nastavení transformace otočit o 180 stupňů při kopírování první druhé. Ve všech příkladech v této části `bitmap` je `SKBitmap` objekt, který je potřeba otočit:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(180, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Při obměně o 90 stupňů, je potřeba vytvořit rastrový obrázek, který je jiný než původní velikost je pomocí výměny výšku a šířku. Například pokud původní rastrový obrázek hodnotu 1200 pixelů a 800 pixelů na výšku, otočený rastrový obrázek hodnotu 800 pixelů a 1200 pixelů. Nastavit posunutí a otočení, které otáčet kolem jeho levého horního rohu a pak posune do zobrazení rastrového obrázku. (Mějte na paměti, která `Translate` a `RotateDegrees` metody jsou volány v opačném pořadí tak, jak jsou použity.) Tady je kód pro výměnu 90 stupňů po směru hodinových ručiček:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(bitmap.Height, 0);
    canvas.RotateDegrees(90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```                        

A tady je podobné funkce pro výměnu 90 stupňů proti směru hodinových ručiček:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(0, bitmap.Width);
    canvas.RotateDegrees(-90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Tyto dvě metody se používají v **fotografii skládankou** stránky je popsáno v článku [ **oříznutí rastrové obrázky ve Skiasharpu**](cropping.md#tile-division).

Program, který umožňuje uživateli otočit rastrový obrázek do 90 stupňů musí implementovat pouze jedna funkce pro výměnu o 90 stupňů. Uživatel pak otáčením v jakékoli přírůstek 90 stupňů opakované vykonání tuto jednu funkci.

Program můžete také otočit rastrový obrázek jakékoli množství. Jednoduché jedním z přístupů je upravit funkce, který otáčí o 180 stupňů nahrazením 180 zobecněný `angle` proměnné:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(angle, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

V tomto obecném případě, ale bude tuto logiku oříznout vypnout rohů otočený rastrového obrázku. Lepším řešením je výpočet velikosti otočený bitmapou pomocí trigonometrických zahrnout tyto rohy. 

Tato trigonometrických je zobrazena ve **rastrový obrázek Rotator** stránky. Vytvoří soubor XAML `SKCanvasView` a `Slider` , který může být v rozsahu od 0 do 360 stupňů s `Label` aktuální hodnotu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapRotatorPage"
             Title="Bitmap Rotator">
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="slider"
                Maximum="360"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Rotate by {0:F0}&#x00B0;'}"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Soubor kódu na pozadí načte prostředku rastrového obrázku a uloží ho jako statické pole jen pro čtení s názvem `originalBitmap`. Rastrový obrázek zobrazen v `PaintSurface` obslužná rutina je `rotatedBitmap`, které je zpočátku nastaven `originalBitmap`:

```csharp
public partial class BitmapRotatorPage : ContentPage
{
    static readonly SKBitmap originalBitmap = 
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.Banana.jpg");

    SKBitmap rotatedBitmap = originalBitmap;

    public BitmapRotatorPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(rotatedBitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double angle = args.NewValue;
        double radians = Math.PI * angle / 180;
        float sine = (float)Math.Abs(Math.Sin(radians));
        float cosine = (float)Math.Abs(Math.Cos(radians));
        int originalWidth = originalBitmap.Width;
        int originalHeight = originalBitmap.Height;
        int rotatedWidth = (int)(cosine * originalWidth + sine * originalHeight);
        int rotatedHeight = (int)(cosine * originalHeight + sine * originalWidth);

        rotatedBitmap = new SKBitmap(rotatedWidth, rotatedHeight);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear(SKColors.LightPink);
            canvas.Translate(rotatedWidth / 2, rotatedHeight / 2);
            canvas.RotateDegrees((float)angle);
            canvas.Translate(-originalWidth / 2, -originalHeight / 2);
            canvas.DrawBitmap(originalBitmap, new SKPoint());
        }

        canvasView.InvalidateSurface();
    }
}
```

`ValueChanged` Obslužná rutina `Slider` provádí operace, které vytvářejí nové `rotatedBitmap` podle úhel otočení. Novou šířku a výšku jsou založeny na absolute values siny a cosines původní šířky a výšky. Transformace používané k vykreslení původního rastrového obrázku na otočený rastrový obrázek přesunout původní center rastrového obrázku na původní název, pak otočit o zadaný počet stupňů a poté je převést monitorovaný pomocí center do center otočený rastrového obrázku. ( `Translate` a `RotateDegrees` metody jsou volány v opačném pořadí, než jak se používají.)

Všimněte si použití `Clear` metoda pozadí `rotatedBitmap` světle růžová. Toto je pouze pro ilustraci velikost `rotatedBitmap` na displeji:

[![Bitmap – Rotator](drawing-images/BitmapRotator.png "Rotator rastrového obrázku")](drawing-images/BitmapRotator-Large.png#lightbox)

Otočený rastrového obrázku je dostatečně velký pro zahrnují celou původní rastrový obrázek, ale ne větší.

## <a name="flipping-bitmaps"></a>Překlopení rastrových obrázků

Jiná operace, které se běžně provádějí rastrových obrázků, se nazývá _překlopení_. Rastrový obrázek je koncepčně, otáčet v trojrozměrném kolem svislé osy nebo vodorovnou osu přes Centrum rastrového obrázku. Svislé překlopení vytvoří zrcadlový obraz.

**Rastrový obrázek překlopení** stránku **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** demonstates aplikace tyto procesy. Obsahuje soubor XAML `SKCanvasView` a dvě tlačítka pro překlopení vertikální i horizontální:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapFlipperPage"
             Title="Bitmap Flipper">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        
        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Button Text="Flip Vertical"
                Grid.Row="1" Grid.Column="0"
                Margin="0, 10"
                Clicked="OnFlipVerticalClicked" />

        <Button Text="Flip Horizontal"
                Grid.Row="1" Grid.Column="1"
                Margin="0, 10"
                Clicked="OnFlipHorizontalClicked" />
    </Grid>
</ContentPage>
```

Implementuje tyto dvě operace v souboru kódu na pozadí `Clicked` obslužné rutiny pro tlačítka: 

```csharp
public partial class BitmapFlipperPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public BitmapFlipperPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnFlipVerticalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(-1, 1, bitmap.Width / 2, 0);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnFlipHorizontalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(1, -1, 0, bitmap.Height / 2);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }
}
```

Překlopit svisle provádí škálování transformace s horizontální škálování faktorem &ndash;1. Škálování center je svislé center rastrového obrázku. Horizontální přetočení je škálování transformace s vertikální škálování faktorem &ndash;1. 

Jak je vidět v obráceném objektu lettering na opic shirt překlopení není stejný jako otočení. Ale jak ukazuje snímek obrazovky UPW na pravé straně, obě překlopení vodorovně a svisle je stejný jako otáčení 180stupňový rozsah s orientací:

[![Bitmap – překlopení](drawing-images/BitmapFlipper.png "rastrového obrázku překlopení")](drawing-images/BitmapFlipper-Large.png#lightbox)

Další obecné úloze, kterou může zpracovat pomocí podobné techniky je oříznutí rastrového obrázku na podmnožinu obdélníkový. Toto je popsáno v následujícím článku [ **oříznutí rastrové obrázky ve Skiasharpu**](cropping.md).

## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
