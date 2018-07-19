---
title: Oříznutí rastrové obrázky ve Skiasharpu
description: Zjistěte, jak ve Skiasharpu použít k návrhu uživatelského rozhraní pro interaktivní který popisuje obdélník oříznutí.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0A79AB27-C69F-4376-8FFE-FF46E4783F30
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: b264fb14e0dc50ca980b8035a75f6f6d8b3ed85f
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130985"
---
# <a name="cropping-skiasharp-bitmaps"></a>Oříznutí rastrové obrázky ve Skiasharpu

[ **Vytváření a kreslení ve Skiasharpu bitmap** ](drawing.md) článek popisuje, jak `SKBitmap` může být předán objekt `SKCanvas` konstruktor. Kreslicí metodu volat u této grafiky plátna způsobí, že mají být vykresleny v rastrového obrázku. Kreslení metody patří mezi ně `DrawBitmap`, což znamená, že tato technika umožňuje přenos část nebo všechny jeden rastrový obrázek do jiné rastrového obrázku, možná s transformací použitá.

Tento postup můžete použít pro oříznutí rastrový obrázek voláním [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) metoda pomocí zdrojových a cílových obdélníky:

```csharp
canvas.DrawBitmap(bitmap, sourceRect, destRect);
```

Aplikace, které implementují oříznutí často ale poskytují rozhraní pro uživatele na interaktivní výběr obdélník oříznutí:

![Oříznutí ukázka](cropping-images/CroppingSample.png "oříznutí vzorku")

Tento článek se zaměřuje na rozhraní.

## <a name="encapsulating-the-cropping-rectangle"></a>Zapouzdření obdélník oříznutí

Je vhodné izolovat některé oříznutí logiky ve třídě s názvem `CroppingRectangle`. Parametry konstruktoru patří maximální obdélník, který je obvykle velikost rastrového obrázku se vyhnete, a volitelné poměr stran. Nejdřív definuje počáteční obdélník oříznutí, které je veřejné v konstruktoru `Rect` vlastnost typu `SKRect`. Tento počáteční obdélník oříznutí je 80 % šířku a výšku obdélníku rastrový obrázek, ale je pak upravit, pokud je zadán poměr stran:

```csharp
class CroppingRectangle
{
    ···
    SKRect maxRect;             // generally the size of the bitmap
    float? aspectRatio;

    public CroppingRectangle(SKRect maxRect, float? aspectRatio = null)
    {
        this.maxRect = maxRect;
        this.aspectRatio = aspectRatio;

        // Set initial cropping rectangle
        Rect = new SKRect(0.9f * maxRect.Left + 0.1f * maxRect.Right,
                          0.9f * maxRect.Top + 0.1f * maxRect.Bottom,
                          0.1f * maxRect.Left + 0.9f * maxRect.Right,
                          0.1f * maxRect.Top + 0.9f * maxRect.Bottom);

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            SKRect rect = Rect;
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;
                rect.Left = (maxRect.Width - width) / 2;
                rect.Right = rect.Left + width;
            }
            else
            {
                float height = rect.Width / aspect;
                rect.Top = (maxRect.Height - height) / 2;
                rect.Bottom = rect.Top + height;
            }

            Rect = rect;
        }
    }
    
    public SKRect Rect { set; get; }
    ···
}
```

Jednu část užitečné informace, které `CroppingRectangle` je k dispozici je také pole `SKPoint` hodnoty odpovídající čtyři rohů obdélníku oříznutí v pořadí levého horního, pravého horního, pravého dolního a levého dolního:

```csharp
class CroppingRectangle
{
    ···
    public SKPoint[] Corners
    {
        get
        {
            return new SKPoint[]
            {
                new SKPoint(Rect.Left, Rect.Top),
                new SKPoint(Rect.Right, Rect.Top),
                new SKPoint(Rect.Right, Rect.Bottom),
                new SKPoint(Rect.Left, Rect.Bottom)
            };
        }
    }
    ···
}
```

Toto pole se používá v následující metodu, která se nazývá `HitTest`. `SKPoint` Parametr je bod odpovídá prstu touch nebo klikněte na tlačítko myši. Metoda vrátí index (0, 1, 2 nebo 3) odpovídající rohu, který bude ukazatel prstem nebo myši dotčená vzdálenosti dána `radius` parametr: 

```csharp
class CroppingRectangle
{
    ···
    public int HitTest(SKPoint point, float radius)
    {
        SKPoint[] corners = Corners;

        for (int index = 0; index < corners.Length; index++)
        {
            SKPoint diff = point - corners[index];
                
            if ((float)Math.Sqrt(diff.X * diff.X + diff.Y * diff.Y) < radius)
            {
                return index;
            }
        }

        return -1;
    }
    ···
}
```

Pokud bod dotyku nebo myši nebyla v rámci `radius` jednotek jakékoli horního, metoda vrátí &ndash;1.

Finální metoda `CroppingRectangle` nazývá `MoveCorner`, která je volána v reakci na dotyk nebo pohybu myši. Tyto dva parametry označení index rohu přesouvá a nové umístění této rohu. V první polovině roku metodu nastavuje obdélník oříznutí založené na nové umístění horního, ale vždy v rámci hranice `maxRect`, což je velikost rastrového obrázku. Tato logika také bere v úvahu `MINIMUM` pole, aby se zabránilo sbalení obdélník oříznutí, do nothing:

```csharp
class CroppingRectangle
{
    const float MINIMUM = 10;   // pixels width or height
    ···
    public void MoveCorner(int index, SKPoint point)
    {
        SKRect rect = Rect;

        switch (index)
        {
            case 0: // upper-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 1: // upper-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 2: // lower-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;

            case 3: // lower-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;
        }

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;

                switch (index)
                {
                    case 0:
                    case 3: rect.Left = rect.Right - width; break;
                    case 1:
                    case 2: rect.Right = rect.Left + width; break;
                }
            }
            else
            {
                float height = rect.Width / aspect;

                switch (index)
                {
                    case 0:
                    case 1: rect.Top = rect.Bottom - height; break;
                    case 2:
                    case 3: rect.Bottom = rect.Top + height; break;
                }
            }
        }

        Rect = rect;
    }
}
```

V druhé polovině metody upraví pro volitelné poměr stran.

Mějte na paměti, že je vše v této třídě v jednotkách, které pixelů.

## <a name="a-canvas-view-just-for-cropping"></a>Plátno zobrazení jen pro oříznutí

`CroppingRectangle` Používá třídy, právě jste viděli `PhotoCropperCanvasView` třída, která je odvozena z `SKCanvasView`. Tato třída je zodpovědná za zobrazení rastrového obrázku a obdélník oříznutí, jakož i zpracování události dotyku nebo myši pro změnu obdélník oříznutí.

`PhotoCropperCanvasView` Konstruktor vyžaduje rastrový obrázek. Poměr stran je volitelný. Konstruktoru vytvoří instanci objektu typu `CroppingRectangle` na základě této rastrového obrázku a poměr stran a uloží ho jako pole:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        this.bitmap = bitmap;

        SKRect bitmapRect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
        croppingRect = new CroppingRectangle(bitmapRect, aspectRatio);
        ···
    }
    ···
}
```

Protože tato třída je odvozena z `SKCanvasView`, není nutné pro instalaci obslužné rutiny pro `PaintSurface` událostí. Místo toho můžete přepsat jeho `OnPaintSurface` metoda. Metoda zobrazí rastrový obrázek a používá několik `SKPaint` objekty, které jsou uloženy jako pole aktuální obdélník oříznutí:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    const int CORNER = 50;      // pixel length of cropper corner
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;
    ···
    // Drawing objects
    SKPaint cornerStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 10
    };

    SKPaint edgeStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 2
    };
    ···
    protected override void OnPaintSurface(SKPaintSurfaceEventArgs args)
    {
        base.OnPaintSurface(args);

        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Gray);

        // Calculate rectangle for displaying bitmap 
        float scale = Math.Min((float)info.Width / bitmap.Width, (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect bitmapRect = new SKRect(x, y, x + scale * bitmap.Width, y + scale * bitmap.Height);
        canvas.DrawBitmap(bitmap, bitmapRect);

        // Calculate a matrix transform for displaying the cropping rectangle
        SKMatrix bitmapScaleMatrix = SKMatrix.MakeIdentity();
        bitmapScaleMatrix.SetScaleTranslate(scale, scale, x, y);

        // Display rectangle
        SKRect scaledCropRect = bitmapScaleMatrix.MapRect(croppingRect.Rect);
        canvas.DrawRect(scaledCropRect, edgeStroke);

        // Display heavier corners
        using (SKPath path = new SKPath())
        {
            path.MoveTo(scaledCropRect.Left, scaledCropRect.Top + CORNER);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Left + CORNER, scaledCropRect.Top);

            path.MoveTo(scaledCropRect.Right - CORNER, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top + CORNER);

            path.MoveTo(scaledCropRect.Right, scaledCropRect.Bottom - CORNER);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Right - CORNER, scaledCropRect.Bottom);

            path.MoveTo(scaledCropRect.Left + CORNER, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom - CORNER);

            canvas.DrawPath(path, cornerStroke);
        }

        // Invert the transform for touch tracking
        bitmapScaleMatrix.TryInvert(out inverseBitmapMatrix);
    }
    ···
}
```

Kód v `CroppingRectangle` třídy základů obdélník oříznutí na pixel velikost rastrového obrázku. Ale zobrazení rastrový obrázek podle `PhotoCropperCanvasView` třídy je škálování na základě velikosti oblasti zobrazení. `bitmapScaleMatrix` Počítané v `OnPaintSurface` přepsat mapy v pixelech rastrového obrázku na velikost a umístění rastrového obrázku, který se zobrazí. Tato matice se pak použije k transformaci obdélník oříznutí, aby mohly být zobrazeny vzhledem k rastrového obrázku.

Poslední řádek `OnPaintSurface` přepsání přebírá inverzní `bitmapScaleMatrix` a uloží ho jako `inverseBitmapMatrix` pole. Používá se pro zpracování dotykové ovládání.

A `TouchEffect` je vytvořena instance objektu jako pole, a připojí obslužnou rutinu do konstruktoru `TouchAction` události, ale `TouchEffect` musí být přidán do `Effects` kolekce _nadřazené_ z `SKCanvasView`odvozená díla, tak, aby dokončení `OnParentSet` přepsat:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    const int RADIUS = 100;     // pixel radius of touch hit-test
    ···
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;

    // Touch tracking 
    TouchEffect touchEffect = new TouchEffect();
    struct TouchPoint
    {
        public int CornerIndex { set; get; }
        public SKPoint Offset { set; get; }
    }

    Dictionary<long, TouchPoint> touchPoints = new Dictionary<long, TouchPoint>();
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        ···
        touchEffect.TouchAction += OnTouchEffectTouchAction;
    }
    ···
    protected override void OnParentSet()
    {
        base.OnParentSet();

        // Attach TouchEffect to parent view
        Parent.Effects.Add(touchEffect);
    }
    ···
    void OnTouchEffectTouchAction(object sender, TouchActionEventArgs args)
    {
        SKPoint pixelLocation = ConvertToPixel(args.Location);
        SKPoint bitmapLocation = inverseBitmapMatrix.MapPoint(pixelLocation);

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Convert radius to bitmap/cropping scale
                float radius = inverseBitmapMatrix.ScaleX * RADIUS;

                // Find corner that the finger is touching
                int cornerIndex = croppingRect.HitTest(bitmapLocation, radius);

                if (cornerIndex != -1 && !touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = new TouchPoint
                    {
                        CornerIndex = cornerIndex,
                        Offset = bitmapLocation - croppingRect.Corners[cornerIndex]
                    };

                    touchPoints.Add(args.Id, touchPoint);
                }
                break;

            case TouchActionType.Moved:
                if (touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = touchPoints[args.Id];
                    croppingRect.MoveCorner(touchPoint.CornerIndex, 
                                            bitmapLocation - touchPoint.Offset);
                    InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchPoints.ContainsKey(args.Id))
                {
                    touchPoints.Remove(args.Id);
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Xamarin.Forms.Point pt)
    {
        return new SKPoint((float)(CanvasSize.Width * pt.X / Width),
                           (float)(CanvasSize.Height * pt.Y / Height));
    }
}
```

Dotykové ovládání události byly zpracovány pomocí `TouchAction` obslužné rutiny jsou v jednotkách nezávislých na zařízení. Tyto nejdřív potřeba převést na pixelech pomocí `ConvertToPixel` metoda v dolní části třídy a poté převeden na `CroppingRectangle` jednotky pomocí `inverseBitmapMatrix`.

Pro `Pressed` události, `TouchAction` volání obslužné rutiny `HitTest` metoda `CroppingRectangle`. Pokud jiné než vrátí index &ndash;1, pak jeden z rohů obdélníku oříznutí je právě zpracováván. Index a posun bodu skutečné dotykové ovládání v horním je uložená v `TouchPoint` objektu a přidá do `touchPoints` slovníku.

Pro `Moved` událostí, `MoveCorner` metoda `CroppingRectangle` je volána k přesunutí rohu s možné úpravy poměru stran.

V každém okamžiku programu pomocí `PhotoCropperCanvasView` se dostanete `CroppedBitmap` vlastnost. Tato vlastnost se používá `Rect` vlastnost `CroppingRectangle` vytvořit nový rastrový obrázek oříznuté velikosti. Verze `DrawBitmap` s cílové a zdrojové obdélníky poté rozbalí podmnožinu původní rastrového obrázku:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public SKBitmap CroppedBitmap
    {
        get
        {
            SKRect cropRect = croppingRect.Rect;
            SKBitmap croppedBitmap = new SKBitmap((int)cropRect.Width, 
                                                  (int)cropRect.Height);
            SKRect dest = new SKRect(0, 0, cropRect.Width, cropRect.Height);
            SKRect source = new SKRect(cropRect.Left, cropRect.Top, 
                                       cropRect.Right, cropRect.Bottom);

            using (SKCanvas canvas = new SKCanvas(croppedBitmap))
            {
                canvas.DrawBitmap(bitmap, source, dest);
            }

            return croppedBitmap;
        }
    }
    ···
}
```

## <a name="hosting-the-photo-cropper-canvas-view"></a>Hostování zobrazení fotografií obrázek nůžek plátna

Pomocí těchto dvou tříd oříznutí logiku zpracování **fotografii oříznutí** stránku **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** aplikace obsahuje příliš mnoho zásahů provést. Vytvoří soubor XAML `Grid` na hostitele `PhotoCropperCanvasView` a **provádí** tlačítko:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoCroppingPage"
             Title="Photo Cropping">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid x:Name="canvasViewHost"
              Grid.Row="0"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="1"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

`PhotoCropperCanvasView` Nejde vytvořit v souboru XAML, protože vyžaduje parametr typu `SKBitmap`.

Místo toho `PhotoCropperCanvasView` je vytvořena instance v konstruktoru použití modelu code-behind soubor pomocí jedné rastrových obrázků prostředků:

```csharp
public partial class PhotoCroppingPage : ContentPage
{
    PhotoCropperCanvasView photoCropper;
    SKBitmap croppedBitmap;

    public PhotoCroppingPage ()
    {
        InitializeComponent ();

        SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        photoCropper = new PhotoCropperCanvasView(bitmap);
        canvasViewHost.Children.Add(photoCropper);
    }

    void OnDoneButtonClicked(object sender, EventArgs args)
    {
        croppedBitmap = photoCropper.CroppedBitmap;

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
        canvas.DrawBitmap(croppedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Uživatel může pak pracuje s obdélník oříznutí:

[![Obrázek 1 nůžek fotografii](cropping-images/PhotoCropping1.png "fotografii obrázek nůžek 1")](cropping-images/PhotoCropping1-Large.png#lightbox)

Pokud byla definována dobré obdélník oříznutí, klikněte na tlačítko **provádí** tlačítko. `Clicked` Získá oříznutý rastrový obrázek z obslužné rutiny `CroppedBitmap` vlastnost `PhotoCropperCanvasView`a nahradí nový obsah stránky `SKCanvasView` objekt, který se zobrazí tento oříznutý rastrový obrázek:

[![Obrázek 2 nůžek fotografii](cropping-images/PhotoCropping2.png "fotografii obrázek nůžek 2")](cropping-images/PhotoCropping2-Large.png#lightbox)

Zkuste nastavit druhý argument `PhotoCropperCanvasView` k 1.78f (třeba):

```csharp
photoCropper = new PhotoCropperCanvasView(bitmap, 1.78f);
```

Zobrazí se vám oříznutí obdélník omezen na 16 9 poměr stran charakteristik televize s vysokým rozlišením.

<a name="tile-division" />

## <a name="dividing-a-bitmap-into-tiles"></a>Dělení do bloků rastrový obrázek

Verze Xamarin.Forms známý díl stavebnice 14 až 15 zobrazovaly v 22 kapitoly knihy [ _vytváření mobilních aplikací pomocí Xamarin.Forms_ ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) a je možné stáhnout jako [  **XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle). Ale ještě nám zbývá stane zábavné (a často náročnější) Pokud je založen na bitové kopie z knihovny fotografií.

Tato verze 14 až 15 díl stavebnice je součástí **[SkiaSharpFormsDemos](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)** aplikace a se skládá z řady stránky s názvem **fotografii skládankou**.

**PhotoPuzzlePage1.xaml** skládá ze souboru `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage1"
             Title="Photo Puzzle">
    
    <Button Text="Pick a photo from your library"
            VerticalOptions="CenterAndExpand" 
            HorizontalOptions="CenterAndExpand"
            Clicked="OnPickButtonClicked"/>
    
</ContentPage>
```

Implementuje soubor kódu na pozadí `Clicked` obslužná rutina, která se používá `IPhotoLibrary` závislou službu, aby mohl uživatel vybrat fotografii z knihovny fotografií:

```csharp
public partial class PhotoPuzzlePage1 : ContentPage
{
    public PhotoPuzzlePage1 ()
    {
        InitializeComponent ();
    }

    async void OnPickButtonClicked(object sender, EventArgs args)
    {
        IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
        using (Stream stream = await photoLibrary.PickPhotoAsync())
        {
            if (stream != null)
            {
                SKBitmap bitmap = SKBitmap.Decode(stream);

                await Navigation.PushAsync(new PhotoPuzzlePage2(bitmap));
            }
        }
    }
}
```

Metoda pak přejde do `PhotoPuzzlePage2`, předejte konstruktor vybrané rastrového obrázku.

Je možné, že fotografii vybrané v knihovně není orientovaný zobrazovaly v knihovny fotografií, ale otočený nebo vzhůru nohama. (To je zvlášť problém se zařízeními iOS.) Z tohoto důvodu `PhotoPuzzlePage2` umožňuje otočení obrázku do požadovaného orientace. Soubor XAML obsahuje tři tlačítka s popiskem **90&#x00B0; vpravo** (to znamená ve směru hodinových ručiček), **90&#x00B0; vlevo** (proti směru hodinových ručiček), a **provádí**.

Soubor kódu na pozadí implementuje logiku otočení rastrového obrázku je znázorněno v následujícím článku  **[vytváření a kreslení ve Skiasharpu rastrových obrázků](drawing.md#rotating-bitmaps)**. Může uživatel otočit o 90 stupňů po směru nebo proti směru hodinových ručiček image libovolný počet pokusů: 

```csharp
public partial class PhotoPuzzlePage2 : ContentPage
{
    SKBitmap bitmap;

    public PhotoPuzzlePage2 (SKBitmap bitmap)
    {
        this.bitmap = bitmap;

        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnRotateRightButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(bitmap.Height, 0);
            canvas.RotateDegrees(90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnRotateLeftButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(0, bitmap.Width);
            canvas.RotateDegrees(-90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        await Navigation.PushAsync(new PhotoPuzzlePage3(bitmap));
    }
}
```

Pokud uživatel klikne **provádí** tlačítko, `Clicked` obslužná rutina přejde na `PhotoPuzzlePage3`, předávání konečné otočený rastrového obrázku v konstruktoru na stránce.

`PhotoPuzzlePage3` Umožňuje fotografii oříznuté. Program vyžaduje Čtvereček rastrový obrázek pro rozdělení do mřížky 4 4 dlaždic.

**PhotoPuzzlePage3.xaml** obsahuje soubor `Label`, `Grid` na hostitele `PhotoCropperCanvasView`a další **provádí** tlačítko:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage3"
             Title="Photo Puzzle">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Label Text="Crop the photo to a square"
               Grid.Row="0"
               FontSize="Large"
               HorizontalTextAlignment="Center"
               Margin="5" />

        <Grid x:Name="canvasViewHost"
              Grid.Row="1"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="2"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

Vytvoří soubor kódu na pozadí `PhotoCropperCanvasView` s rastrový obrázek předaný konstruktoru. Všimněte si, že 1 je předána jako druhý argument `PhotoCropperCanvasView`. Tento poměr 1 vynutí obdélník oříznutí bude čtverec:

```csharp
public partial class PhotoPuzzlePage3 : ContentPage
{
    PhotoCropperCanvasView photoCropper;

    public PhotoPuzzlePage3(SKBitmap bitmap)
    {
        InitializeComponent ();

        photoCropper = new PhotoCropperCanvasView(bitmap, 1f);
        canvasViewHost.Children.Add(photoCropper);
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        SKBitmap croppedBitmap = photoCropper.CroppedBitmap;
        int width = croppedBitmap.Width / 4;
        int height = croppedBitmap.Height / 4;

        ImageSource[] imgSources = new ImageSource[15];

        for (int row = 0; row < 4; row++)
        {
            for (int col = 0; col < 4; col++)
            {
                // Skip the last one!
                if (row == 3 && col == 3)
                    break;

                // Create a bitmap 1/4 the width and height of the original
                SKBitmap bitmap = new SKBitmap(width, height);
                SKRect dest = new SKRect(0, 0, width, height);
                SKRect source = new SKRect(col * width, row * height, (col + 1) * width, (row + 1) * height);

                // Copy 1/16 of the original into that bitmap
                using (SKCanvas canvas = new SKCanvas(bitmap))
                {
                    canvas.DrawBitmap(croppedBitmap, source, dest);
                }

                imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
            }
        }

        await Navigation.PushAsync(new PhotoPuzzlePage4(imgSources));
    }
}
```

**Provádí** obslužná rutina události tlačítka získá šířku a výšku oříznutý rastrového obrázku (tyto dvě hodnoty musí být stejná) a potom rozděluje do 15 samostatné rastrové obrázky, z nichž každý je 1/4 šířku a výšku původní. (Poslední možné 16 bitmapy není vytvořena.) `DrawBitmap` Umožňuje rastrového obrázku na základě podmnožina větší rastrového obrázku na vytvořit metoda s zdrojového a cílového obdélníku.

## <a name="converting-to-xamarinforms-bitmaps"></a>Převádění bitmap Xamarin.Forms

V `OnDoneButtonClicked` metoda, pole vytvořené pro 15 rastrových obrázků, které je typu [ `ImageSource` ](xref:Xamarin.Forms.ImageSource):

```csharp
ImageSource[] imgSources = new ImageSource[15];
```

`ImageSource` je základním typem Xamarin.Forms, která zapouzdřuje rastrový obrázek. Naštěstí ve Skiasharpu umožňuje převod z rastrové obrázky ve Skiasharpu do bitmap Xamarin.Forms. **SkiaSharp.Views.Forms** definuje sestavení [ `SKBitmapImageSource` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKBitmapImageSource/) třídu odvozenou od `ImageSource` ale může na základě vytvořit ve Skiasharpu `SKBitmap` objektu. `SKBitmapImageSource` dokonce i definuje převody mezi `SKBitmapImageSource` a `SKBitmap`a to jak `SKBitmap` objekty jsou uloženy v poli jako Xamarin.Forms rastrové obrázky:

```csharp
imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
```

Toto pole rastrových obrázků je předán jako konstruktor do `PhotoPuzzlePage4`. Této stránce je zcela Xamarin.Forms a nepoužívá žádné ve Skiasharpu. Je velmi podobný [ **XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle), takže nebudete zde popsané, ale zobrazí vybrané fotografie rozdělit do 15 Čtvereček dlaždice:

[![Fotografii ještě nám zbývá 1](cropping-images/PhotoPuzzle1.png "fotografii ještě nám zbývá 1")](cropping-images/PhotoPuzzle1-Large.png#lightbox)

Stisknutím klávesy **náhodně** používá tlačítko nahoru všechny dlaždice:

[![Fotografii ještě nám zbývá 2](cropping-images/PhotoPuzzle2.png "fotografii ještě nám zbývá 2")](cropping-images/PhotoPuzzle2-Large.png#lightbox)

Nyní můžete vložit je zpět ve správném pořadí. Všechny dlaždice ve stejném řádku nebo sloupce jako prázdné hranaté můžete klepnutí jejich přesun do prázdné buňky. 

## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
