---
title: Manipulace dotyků
description: Tento článek vysvětluje, jak používat maticové transformace pro implementaci přetažením dotykového ovládání, sevření a otočení a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: charlespetzold
ms.author: chape
ms.date: 04/03/2018
ms.openlocfilehash: 2de5b9a3a6bf0d36330212a52ba5c7278b970efc
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130904"
---
# <a name="touch-manipulations"></a>Manipulace dotyků

_Použití matice transformuje k implementaci přetažením dotykového ovládání, sevření a otočení_

V prostředích s více dotyků ohrožují na mobilních zařízeních uživatelů často používají jejich prstů k manipulaci s objekty na obrazovce. Běžné gesta jako je například jedním prstem přetahování a roztažením prstů dvě můžete přesunout a škálovat objekty nebo dokonce i jejich otočení. Tyto gesta jsou obvykle implementovány pomocí transformační matice a v tomto článku se dozvíte, jak to provést.

![](touch-images/touchmanipulationsexample.png "Rastrový obrázek překladu, měřítko a otočení")

## <a name="manipulating-one-bitmap"></a>Manipulace s jeden rastrový obrázek

**Touch manipulace** stránce ukazuje manipulace dotykové ovládání v jediné bitmapě.
Tato ukázka používá touch sledování účinku uvedené v článku [vyvolání události z účinků](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Poskytuje podporu pro několik dalších souborů **Touch manipulace** stránky. První je [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) výčet, který určuje různé druhy touch manipulace implementovat v kódu, bude zobrazení:

```csharp
enum TouchManipulationMode
{
    None,
    PanOnly,
    IsotropicScale,     // includes panning
    AnisotropicScale,   // includes panning
    ScaleRotate,        // implies isotropic scaling
    ScaleDualRotate     // adds one-finger rotation
}
```

`PanOnly` je s jedním prstem přetažení, která je implementována pomocí překladu. Všechny ostatní možnosti také zahrnovat posouvání, ale zahrnují dvěma prsty: `IsotropicScale` je operace prstů, jehož výsledkem objekt škálování stejně vodorovného a svislého směry. `AnisotropicScale` umožňuje škálování nerovnost.

`ScaleRotate` Možnost je pro dvě prstem změnu měřítka a otočení. Škálování je isotropic. Implementace dvou prstem otočení pomocí anizotropního škálování jen těžko pohybů prstem jsou v podstatě stejné.

`ScaleDualRotate` Možnost přidá otočení jedním prstem. Při jedné prstem objektu, přetahovaného objektu je nejprve otáčet kolem středu tak, aby středu objektu zarovnán s přetahování vektoru.

[ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) obsahuje soubor `Picker` s členy `TouchManipulationMode` výčtu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.TouchManipulationPage"
             Title="Touch Manipulation">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker Title="Touch Mode"
                Grid.Row="0"
                SelectedIndexChanged="OnTouchModePickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>None</x:String>
                <x:String>PanOnly</x:String>
                <x:String>IsotropicScale</x:String>
                <x:String>AnisotropicScale</x:String>
                <x:String>ScaleRotate</x:String>
                <x:String>ScaleDualRotate</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                4
            </Picker.SelectedIndex>
        </Picker>

        <Grid BackgroundColor="White"
              Grid.Row="1">

            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>
    </Grid>
</ContentPage>
```

Směrem k dolní části je `SKCanvasView` a `TouchEffect` připojené k jedné buňce `Grid` , který obklopuje.

[ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) má soubor kódu na pozadí `bitmap` pole, ale není typu `SKBitmap`. Typ je `TouchManipulationBitmap` (zobrazí se vám zanedlouho třídu):

```csharp
public partial class TouchManipulationPage : ContentPage
{
    TouchManipulationBitmap bitmap;
    ...

    public TouchManipulationPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.MountainClimbers.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            SKBitmap bitmap = SKBitmap.Decode(stream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

Vytvoří instanci konstruktoru `TouchManipulationBitmap` objekt předáním konstruktoru `SKBitmap` získané ze vloženého prostředku. Konstruktor končí tím, že nastavíte `Mode` vlastnost `TouchManager` vlastnost `TouchManipulationBitmap` objektu na člen `TouchManipulationMode` výčtu.

`SelectedIndexChanged` Obslužné rutiny pro `Picker` také nastaví tuto `Mode` vlastnost:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    void OnTouchModePickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (bitmap != null)
        {
            Picker picker = (Picker)sender;
            TouchManipulationMode mode;
            Enum.TryParse(picker.Items[picker.SelectedIndex], out mode);
            bitmap.TouchManager.Mode = mode;
        }
    }
    ...
}
```

`TouchAction` Obslužná rutina `TouchEffect` vytvořena ve voláních soubor XAML dvě metody v `TouchManipulationBitmap` s názvem `HitTest` a `ProcessTouchEvent`:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    List<long> touchIds = new List<long>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (bitmap.HitTest(point))
                {
                    touchIds.Add(args.Id);
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    break;
                }
                break;

            case TouchActionType.Moved:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    touchIds.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Pokud `HitTest` vrátí metoda `true` &mdash; to znamená, že má některé prstem na obrazovce v rámci zabraná rastrového obrázku &mdash; potom touch ID je přidán k `TouchIds` kolekce. Toto ID představuje posloupnost událostí dotykového ovládání pro tento prstem, dokud prstu výtahy z obrazovky. Pokud více prsty touch rastrového obrázku, pak bude `touchIds` kolekce obsahuje touch ID pro každý prstem.

`TouchAction` Také volá obslužná rutina `ProcessTouchEvent` třídy v `TouchManipulationBitmap`. Tady některých (ale ne všech) aplikace skutečný touch neproběhne zpracování.

[ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) Představuje obálkovou třídu pro třídy `SKBitmap` , která obsahuje kód pro vykreslení rastrového obrázku a zpracovávat události dotykové ovládání. Funguje ve spojení s více zobecnit kód v `TouchManipulationManager` (která se zobrazí za chvíli).

`TouchManipulationBitmap` Konstruktor uloží `SKBitmap` a vytvoří dvě vlastnosti `TouchManager` vlastnost typu `TouchManipulationManager` a `Matrix` vlastnost typu `SKMatrix`:

```csharp
class TouchManipulationBitmap
{
    SKBitmap bitmap;
    ...

    public TouchManipulationBitmap(SKBitmap bitmap)
    {
        this.bitmap = bitmap;
        Matrix = SKMatrix.MakeIdentity();

        TouchManager = new TouchManipulationManager
        {
            Mode = TouchManipulationMode.ScaleRotate
        };
    }

    public TouchManipulationManager TouchManager { set; get; }

    public SKMatrix Matrix { set; get; }
    ...
}
```

To `Matrix` vlastnost je součet transformace vyplývající z dotykového ovládání aktivit. Jak uvidíte, každá událost touch vyřeší do matice, který je pak zřetězená s `SKMatrix` hodnota uložená `Matrix` vlastnost.

`TouchManipulationBitmap` Objekt vykresluje jeho `Paint` metoda. Argument je `SKCanvas` objektu. To `SKCanvas` již můžete mít transformace použita, proto `Paint` metoda zřetězí `Matrix` vlastnost přidružený rastrový obrázek pro existující transformace a obnoví na plátně po dokončení:

```csharp
class TouchManipulationBitmap
{
    ...
    public void Paint(SKCanvas canvas)
    {
        canvas.Save();
        SKMatrix matrix = Matrix;
        canvas.Concat(ref matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();
    }
    ...
}
```

`HitTest` Vrátí metoda `true` Pokud uživatel se dotýká obrazovky v okamžiku v rámci hranic rastrového obrázku. Jako uživatel manipuluje rastrový obrázek, rastrového obrázku může otočit nebo dokonce (pomocí kombinace anisotropního měřítka a otočení) být ve tvaru se z něj rovnoběžník. Může být obávat, který `HitTest` metoda musí implementovat spíše složité analýze geometrie v takovém případě.

Zástupce je však k dispozici:

Zjištění, zda se bod nachází v rámci hranic transformovaný obdélníku je stejný jako zjištění, zda inverzní transformovaný bodu leží v hranicích Netransformovaný obdélník. To je mnohem jednodušší výpočet a můžete použít pohodlný `Contains` metody definované `SKRect`:

```csharp
class TouchManipulationBitmap
{
    ...
    public bool HitTest(SKPoint location)
    {
        // Invert the matrix
        SKMatrix inverseMatrix;

        if (Matrix.TryInvert(out inverseMatrix))
        {
            // Transform the point using the inverted matrix
            SKPoint transformedPoint = inverseMatrix.MapPoint(location);

            // Check if it's in the untransformed bitmap rectangle
            SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
            return rect.Contains(transformedPoint);
        }
        return false;
    }
    ...
}
```

Druhá veřejná metoda v `TouchManipulationBitmap` je `ProcessTouchEvent`. Když tato metoda je volána, již bylo zjištěno, že události touch patří do této konkrétní rastrového obrázku. Metoda udržuje slovník [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) objekty, které je jednoduše dřívějšímu bodu a nový bod každé prstem:

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

Tady je slovníku a `ProcessTouchEvent` metoda sama:

```csharp
class TouchManipulationBitmap
{
    ...
    Dictionary<long, TouchManipulationInfo> touchDictionary =
        new Dictionary<long, TouchManipulationInfo>();
    ...
    public void ProcessTouchEvent(long id, TouchActionType type, SKPoint location)
    {
        switch (type)
        {
            case TouchActionType.Pressed:
                touchDictionary.Add(id, new TouchManipulationInfo
                {
                    PreviousPoint = location,
                    NewPoint = location
                });
                break;

            case TouchActionType.Moved:
                TouchManipulationInfo info = touchDictionary[id];
                info.NewPoint = location;
                Manipulate();
                info.PreviousPoint = info.NewPoint;
                break;

            case TouchActionType.Released:
                touchDictionary[id].NewPoint = location;
                Manipulate();
                touchDictionary.Remove(id);
                break;

            case TouchActionType.Cancelled:
                touchDictionary.Remove(id);
                break;
        }
    }
    ...
}
```

V `Moved` a `Released` události, volání metod `Manipulate`. Za těchto okolností `touchDictionary` obsahuje jeden nebo více `TouchManipulationInfo` objekty. Pokud `touchDictionary` obsahuje jednu položku, je pravděpodobné, který `PreviousPoint` a `NewPoint` hodnoty rovny a představují přesun prstem. Pokud více prsty se dotýká rastrový obrázek, slovník obsahuje více než jednu položku, ale pouze jeden z těchto položek má jiné `PreviousPoint` a `NewPoint` hodnoty. Všechny ostatní mají stejné `PreviousPoint` a `NewPoint` hodnoty.

To je důležité: `Manipulate` metoda můžete předpokládat, že se zpracování pohyb pouze jedním prstem. V okamžiku tohoto volání žádná další prsty není přesun a pokud, skutečně přesouváte (což je pravděpodobně), tyto pohybů plb typu budou zpracovány v budoucích volání `Manipulate`.

`Manipulate` Slovníku metoda nejprve zkopíruje do pole pro usnadnění práce. Ignoruje nic jiného než první dvě položky. Pokud více než dvěma prsty se pokoušejí o manipulaci s rastrový obrázek, ostatní budou ignorovány. `Manipulate` je poslední člen `TouchManipulationBitmap`:

```csharp
class TouchManipulationBitmap
{
    ...
    void Manipulate()
    {
        TouchManipulationInfo[] infos = new TouchManipulationInfo[touchDictionary.Count];
        touchDictionary.Values.CopyTo(infos, 0);
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();

        if (infos.Length == 1)
        {
            SKPoint prevPoint = infos[0].PreviousPoint;
            SKPoint newPoint = infos[0].NewPoint;
            SKPoint pivotPoint = Matrix.MapPoint(bitmap.Width / 2, bitmap.Height / 2);

            touchMatrix = TouchManager.OneFingerManipulate(prevPoint, newPoint, pivotPoint);
        }
        else if (infos.Length >= 2)
        {
            int pivotIndex = infos[0].NewPoint == infos[0].PreviousPoint ? 0 : 1;
            SKPoint pivotPoint = infos[pivotIndex].NewPoint;
            SKPoint newPoint = infos[1 - pivotIndex].NewPoint;
            SKPoint prevPoint = infos[1 - pivotIndex].PreviousPoint;

            touchMatrix = TouchManager.TwoFingerManipulate(prevPoint, newPoint, pivotPoint);
        }

        SKMatrix matrix = Matrix;
        SKMatrix.PostConcat(ref matrix, touchMatrix);
        Matrix = matrix;
    }
}
```

Pokud je jedním prstem manipulace s rastrový obrázek, `Manipulate` volání `OneFingerManipulate` metodu `TouchManipulationManager` objektu. Pro dvěma prsty, které volá `TwoFingerManipulate`. Argumenty pro tyto metody jsou stejné: `prevPoint` a `newPoint` argumenty představují prstu, který je přesun. Ale `pivotPoint` argument se liší pro dvě volání:

Pro manipulaci s jedním prstem `pivotPoint` centrum rastrového obrázku. Toto je povolit pro rotaci jedním prstem. Pro manipulaci s dvěma prstem, události indikuje pohyb pouze jedním prstem, tak, aby `pivotPoint` je prstu, který není přesun.

V obou případech `TouchManipulationManager` vrátí `SKMatrix` hodnotu, kterou metodu zřetězí s aktuálním `Matrix` vlastnost, která `TouchManipulationPage` používá k vykreslení rastrového obrázku.

`TouchManipulationManager` je zobecněný a používá další soubory, s výjimkou `TouchManipulationMode`. Je možné použít tuto třídu beze změny ve svých vlastních aplikacích. Definuje jedinou vlastnost typ `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


Ale budete pravděpodobně chtít vyhnout `AnisotropicScale` možnost. Je velmi snadné pomocí této možnosti pro manipulaci s rastrového obrázku tak, aby jedním z faktorů škálování nula. Díky tomu rastrový obrázek zmizí z pohledu nikdy k vrácení. Pokud skutečně potřebujete anisotropního škálování, je vhodné k vylepšení logiku, aby se zabránilo nežádoucí výsledky.

`TouchManipulationManager` využívá vektorů, ale neexistuje totiž žádný `SKVector` struktury v SkiaSharp, `SKPoint` místo ní se použije. `SKPoint` podporuje operátor odčítání a výsledek lze považovat za vektoru. Logika pouze konkrétní vektoru, která potřeba přidat je `Magnitude` výpočtu:

```csharp
class TouchManipulationManager
{
    ...
    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
}
```

Pokaždé, když byl vybrán otočení, manipulaci s jedním prstem a prstem dvě metody zpracování otáčení nejprve. Pokud se zjistí otočení komponentu otočení vlastně odebraná. Co je ještě je interpretován jako posouvání a změna měřítka.

Tady je `OneFingerManipulate` metody. Pokud jedním prstem otočení není povolená, pak je jednoduchou logiku &mdash; jednoduše používá předchozí bod a nový bod pro vytvoření Vektoru s názvem `delta` , který odpovídá přesně překladu. S jedním prstem otočení povolena metoda používá úhly z bodu otáčení (System center rastrového obrázku) k dřívějšímu bodu a nový bod k sestavení kompletních otočení matice:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }

    public SKMatrix OneFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        if (Mode == TouchManipulationMode.None)
        {
            return SKMatrix.MakeIdentity();
        }

        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint delta = newPoint - prevPoint;

        if (Mode == TouchManipulationMode.ScaleDualRotate)  // One-finger rotation
        {
            SKPoint oldVector = prevPoint - pivotPoint;
            SKPoint newVector = newPoint - pivotPoint;

            // Avoid rotation if fingers are too close to center
            if (Magnitude(newVector) > 25 && Magnitude(oldVector) > 25)
            {
                float prevAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                // Calculate rotation matrix
                float angle = newAngle - prevAngle;
                touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                // Effectively rotate the old vector
                float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                oldVector.X = magnitudeRatio * newVector.X;
                oldVector.Y = magnitudeRatio * newVector.Y;

                // Recalculate delta
                delta = newVector - oldVector;
            }
        }

        // Multiply the rotation matrix by a translation matrix
        SKMatrix.PostConcat(ref touchMatrix, SKMatrix.MakeTranslation(delta.X, delta.Y));

        return touchMatrix;
    }
    ...
}
```

V `TwoFingerManipulate` metody bodu otáčení je pozice prstu, který není v tomto případě konkrétní touch přesunutí. Otočení je velmi podobný otočení jedním prstem, a potom s názvem vektoru `oldVector` (založená na předchozí bod) dojde k přenastavení pro otáčení. Přesun zbývajících je interpretován jako škálování:

```csharp
class TouchManipulationManager
{
    ...
    public SKMatrix TwoFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint oldVector = prevPoint - pivotPoint;
        SKPoint newVector = newPoint - pivotPoint;

        if (Mode == TouchManipulationMode.ScaleRotate ||
            Mode == TouchManipulationMode.ScaleDualRotate)
        {
            // Find angles from pivot point to touch points
            float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
            float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

            // Calculate rotation matrix
            float angle = newAngle - oldAngle;
            touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

            // Effectively rotate the old vector
            float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
            oldVector.X = magnitudeRatio * newVector.X;
            oldVector.Y = magnitudeRatio * newVector.Y;
        }

        float scaleX = 1;
        float scaleY = 1;

        if (Mode == TouchManipulationMode.AnisotropicScale)
        {
            scaleX = newVector.X / oldVector.X;
            scaleY = newVector.Y / oldVector.Y;

        }
        else if (Mode == TouchManipulationMode.IsotropicScale ||
                 Mode == TouchManipulationMode.ScaleRotate ||
                 Mode == TouchManipulationMode.ScaleDualRotate)
        {
            scaleX = scaleY = Magnitude(newVector) / Magnitude(oldVector);
        }

        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
        {
            SKMatrix.PostConcat(ref touchMatrix,
                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y));
        }

        return touchMatrix;
    }
    ...
}
```

Můžete si všimnout, že neexistuje žádný explicitní překlad v této metodě. Nicméně, obojí `MakeRotation` a `MakeScale` metody jsou založeny na bodu otáčení a implicitní převod, který obsahuje. Pokud používáte dvěma prsty na rastrový obrázek a jejich přetažením ve stejném směru `TouchManipulation` získáte řadu dotyků přepínání mezi dvěma prsty. Pokaždé prstem pohybuje relativně k jiné, změna velikosti nebo otočení výsledky, ale je negovat pohybem prstu jiné a výsledkem je překladu.

Jediný zbývající část **Touch manipulace** stránka je `PaintSurface` obslužné rutiny v `TouchManipulationPage` soubor kódu na pozadí. Volá `Paint` metodu `TouchManipulationBitmap`, které se vztahují matice představující aktivity nahromaděné dotykové ovládání:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    MatrixDisplay matrixDisplay = new MatrixDisplay();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        bitmap.Paint(canvas);

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(bitmap.Matrix);

        matrixDisplay.Paint(canvas, bitmap.Matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));
    }
}
```

`PaintSurface` Obslužná rutina dojde k závěru zobrazením `MatrixDisplay` objekt zobrazující matice nahromaděné dotykové ovládání:

[![](touch-images/touchmanipulation-small.png "Trojitá snímek obrazovky stránky Touch manipulace")](touch-images/touchmanipulation-large.png#lightbox "Trojitá snímek obrazovky stránky Touch manipulace")

## <a name="manipulating-multiple-bitmaps"></a>Manipulace s více rastrových obrázků

Jednou z výhod, jako izolace touch zpracování kódu ve třídách `TouchManipulationBitmap` a `TouchManipulationManager` je možnosti opakovaně používat tyto třídy v programu, který mu umožní pracovat s více rastrových obrázků.

**Zobrazení bodového grafu rastrový obrázek** stránce ukazuje, jak to lze provést. Místo definování pole typu `TouchManipulationBitmap`, [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) definuje třídu `List` objektů rastrového obrázku:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    List<TouchManipulationBitmap> bitmapCollection =
        new List<TouchManipulationBitmap>();
    ...
    public BitmapScatterViewPage()
    {
        InitializeComponent();

        // Load in all the available bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;
        string[] resourceIDs = assembly.GetManifestResourceNames();
        SKPoint position = new SKPoint();

        foreach (string resourceID in resourceIDs)
        {
            if (resourceID.EndsWith(".png") ||
                resourceID.EndsWith(".jpg"))
            {
                using (Stream stream = assembly.GetManifestResourceStream(resourceID))
                {
                    SKBitmap bitmap = SKBitmap.Decode(stream);
                    bitmapCollection.Add(new TouchManipulationBitmap(bitmap)
                    {
                        Matrix = SKMatrix.MakeTranslation(position.X, position.Y),
                    });
                    position.X += 100;
                    position.Y += 100;
                }
            }
        }
    }
    ...
}
```

Konstruktor načte ve všech rastrových obrázků, která je k dispozici jako vložené prostředky a přidá je do `bitmapCollection`. Všimněte si, že `Matrix` vlastnost je inicializována na každém `TouchManipulationBitmap` objektu, takže se levém horním rohu rastru posun × 100 pixelů.

`BitmapScatterView` Stránka musí také zpracovávat události dotykového ovládání pro více bitmapy. Místo definování samostatné výstrahy `List` z touch ID aktuálně manipulovat `TouchManipulationBitmap` objekty, tento program vyžaduje slovník:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    Dictionary<long, TouchManipulationBitmap> bitmapDictionary =
       new Dictionary<long, TouchManipulationBitmap>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                for (int i = bitmapCollection.Count - 1; i >= 0; i--)
                {
                    TouchManipulationBitmap bitmap = bitmapCollection[i];

                    if (bitmap.HitTest(point))
                    {
                        // Move bitmap to end of collection
                        bitmapCollection.Remove(bitmap);
                        bitmapCollection.Add(bitmap);

                        // Do the touch processing
                        bitmapDictionary.Add(args.Id, bitmap);
                        bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                        canvasView.InvalidateSurface();
                        break;
                    }
                }
                break;

            case TouchActionType.Moved:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    bitmapDictionary.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Všimněte si, že jak `Pressed` logiky prochází `bitmapCollection` v opačném pořadí. Rastrové obrázky často vzájemně překrývat. Rastrové obrázky později v kolekci vizuálně leží na rastrové obrázky dříve v kolekci. Pokud existuje více rastrové obrázky v části prstem, který stiskem tlačítka na obrazovce, je nejvyšší musí být ten, který je zpracováván této prstem.

Všimněte si také, že `Pressed` logiky tak, aby vizuálně přesune do horní části celé řady dalších rastrové obrázky přesune tento rastrový obrázek na konec kolekce.

V `Moved` a `Released` události, `TouchAction` volání obslužné rutiny `ProcessingTouchEvent` metoda ve `TouchManipulationBitmap` stejně jako dřívější programu.

Nakonec `PaintSurface` volání obslužné rutiny `Paint` metoda jednotlivých `TouchManipulationBitmap` objektu:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (TouchManipulationBitmap bitmap in bitmapCollection)
        {
            bitmap.Paint(canvas);
        }
    }
}
```

Kód smyčky přes kolekce a zobrazí celé řady rastrové obrázky ze začátku kolekce na konec:

[![](touch-images/bitmapscatterview-small.png "Trojitá snímek obrazovky stránky zobrazení bodového grafu rastrový obrázek")](touch-images/bitmapscatterview-large.png#lightbox "Trojitá snímek obrazovky stránky zobrazení bodového grafu rastrový obrázek")

## <a name="single-finger-scaling"></a>Škálování jedním prstem

Operaci škálování obvykle vyžaduje gesto roztažením pomocí dvěma prsty. Nicméně je možné implementovat škálování jednoho prstem tím, že prstem přesunout rohů rastrový obrázek.

To je patrné **jedné Škálovací rohu prstem** stránky. Protože se o něco jiného typu než škálování, implementované v této ukázce se používá `TouchManipulationManager` třídy, nepoužívá dané třídy nebo `TouchManipulationBitmap` třídy. Místo toho veškerou logiku dotykového ovládání je v souboru kódu na pozadí. To je poněkud jednodušší logiky než obvykle, protože sleduje pouze jedním prstem najednou a jednoduše ignoruje všechny sekundární prsty, které může být klepnou na obrazovce.

[ **SingleFingerCornerScale.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml) vytvoří instanci stránky `SKCanvasView` třídy a vytvoří `TouchEffect` objektů pro sledování události dotykové ovládání:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.SingleFingerCornerScalePage"
             Title="Single Finger Corner Scale">

    <Grid BackgroundColor="White"
          Grid.Row="1">

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction"   />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

[ **SingleFingerCornerScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs) načtení prostředku rastrového obrázku ze souboru **média** adresáře a zobrazí ji pomocí `SKMatrix` definované jako objekty pole:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();
    ···

    public SingleFingerCornerScalePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.SetMatrix(currentMatrix);
        canvas.DrawBitmap(bitmap, 0, 0);
    }
    ···
}
```

To `SKMatrix` objekt je upravit podle logiky touch najdete níž.

Je zbytek souboru kódu na pozadí `TouchEffect` obslužné rutiny události. Začíná převedením aktuální umístění prstem na `SKPoint` hodnotu. Pro `Pressed` typ akce, obslužná rutina ověří, že se dotýká žádné prstem na obrazovce, a že prstu jsou uvnitř mezí rastrového obrázku.

Je klíčovou součástí kód `if` příkaz zahrnující dvě volání `Math.Pow` metoda. Tato matematické zkontroluje, jestli prstem umístění mimo elipsy, která naplní rastrového obrázku. Pokud ano, to je operaci škálování. Prstu se blíží jeden z rohů rastrového obrázku a bodu otáčení je určit, že je protilehlého rohu. Pokud je v této elipsa prstu, je regulární posouvání operace:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();

    // Information for translating and scaling
    long? touchId = null;
    SKPoint pressedLocation;
    SKMatrix pressedMatrix;

    // Information for scaling
    bool isScaling;
    SKPoint pivotPoint;
    ···

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Track only one finger
                if (touchId.HasValue)
                    return;

                // Check if the finger is within the boundaries of the bitmap
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = currentMatrix.MapRect(rect);
                if (!rect.Contains(point))
                    return;

                // First assume there will be no scaling
                isScaling = false;

                // If touch is outside interior ellipse, make this a scaling operation
                if (Math.Pow((point.X - rect.MidX) / (rect.Width / 2), 2) +
                    Math.Pow((point.Y - rect.MidY) / (rect.Height / 2), 2) > 1)
                {
                    isScaling = true;
                    float xPivot = point.X < rect.MidX ? rect.Right : rect.Left;
                    float yPivot = point.Y < rect.MidY ? rect.Bottom : rect.Top;
                    pivotPoint = new SKPoint(xPivot, yPivot);
                }

                // Common for either pan or scale
                touchId = args.Id;
                pressedLocation = point;
                pressedMatrix = currentMatrix;
                break;

            case TouchActionType.Moved:
                if (!touchId.HasValue || args.Id != touchId.Value)
                    return;

                SKMatrix matrix = SKMatrix.MakeIdentity();

                // Translating
                if (!isScaling)
                {
                    SKPoint delta = point - pressedLocation;
                    matrix = SKMatrix.MakeTranslation(delta.X, delta.Y);
                }
                // Scaling
                else
                {
                    float scaleX = (point.X - pivotPoint.X) / (pressedLocation.X - pivotPoint.X);
                    float scaleY = (point.Y - pivotPoint.Y) / (pressedLocation.Y - pivotPoint.Y);
                    matrix = SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);
                }

                // Concatenate the matrices
                SKMatrix.PreConcat(ref matrix, pressedMatrix);
                currentMatrix = matrix;
                canvasView.InvalidateSurface();
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = null;
                break;
        }
    }
}
```

`Moved` Typ akce počítá matice odpovídající aktivitě touch od času prstu stisknutí obrazovky až po tuto dobu. Tento matice s matice zřetězuje v platnosti v době prstu prvním stisknutí rastrového obrázku. Operace škálování je vždy relativní k horním než ten, který některé prstu.

Pro malé nebo podlouhlá rastrové obrázky může být vnitřní elipsa zabírat většinu rastrového obrázku a nechte velmi malý oblastí v rozích škálování rastrového obrázku. Můžete dát přednost trochu jiný přístup, v takovém případě je možné nahradit tuto celý `if` blok, který nastaví `isScaling` k `true` s tímto kódem:

```csharp
float halfHeight = rect.Height / 2;
float halfWidth = rect.Width / 2;

// Top half of bitmap
if (point.Y < rect.MidY)
{
    float yRelative = (point.Y - rect.Top) / halfHeight;

    // Upper-left corner
    if (point.X < rect.MidX - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Bottom);
    }
    // Upper-right corner
    else if (point.X > rect.MidX + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Bottom);
    }
}
// Bottom half of bitmap
else
{
    float yRelative = (point.Y - rect.MidY) / halfHeight;

    // Lower-left corner
    if (point.X < rect.Left + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Top);
    }
    // Lower-right corner
    else if (point.X > rect.Right - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Top);
    }
}
```

Tento kód efektivně rozděluje oblasti rastrového obrázku do vnitřní kosočtverec a čtyři trojúhelníky v rozích. To umožňuje mnohem větší oblastí v rozích k převzetí a změnit velikost rastrového obrázku.

## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Vyvolání události z účinků](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
