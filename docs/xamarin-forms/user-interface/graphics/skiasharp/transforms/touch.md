---
title: "Manipulace dotykového ovládání"
description: "Použití matice transformuje implementovat přetahování dotykového ovládání, roztáhnout a otočení"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: b418e0179c95a424c88d5f5063a09f984bb13ec0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="touch-manipulations"></a>Manipulace dotykového ovládání

_Použití matice transformuje implementovat přetahování dotykového ovládání, roztáhnout a otočení_

V prostředích s více touch například na mobilních zařízeních uživatelů často používají jejich prsty manipulovat s objekty, na obrazovce. Například jeden prstem přetažení a roztahováním prstů dvě běžné gesta můžete přesunout a škálování objekty nebo i jejich otočení. Tyto gesta jsou obecně implementovaná pomocí transformační matice a v tomto článku se dozvíte, jak to provést.

![](touch-images/touchmanipulationsexample.png "Rastrový obrázek podrobí překlad, změny velikosti a oběh")

## <a name="manipulating-one-bitmap"></a>Manipulace s jeden rastrového obrázku

**Touch manipulaci** stránky ukazuje touch manipulace na jednom rastrového obrázku.
Tato ukázka využívá touch sledování účinku uvedené v článku [vyvolání události z důsledky](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Poskytují podporu pro několik dalších souborů **Touch manipulaci** stránky. První je [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) výčtu, který označuje různé typy zpracování touch implementované kód budete zobrazuje:

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

`PanOnly` je přetáhněte prstem jeden, který je implementováno s překlad. Všechny ostatní možnosti také zahrnovat posouvání ale zahrnují dvěma prsty: `IsotropicScale` je roztahováním operace, která vyústí v objektu škálování stejně v vodorovného a svislého směru. `AnisotropicScale` Umožňuje nerovné škálování.

`ScaleRotate` Možnost je pro dva prstem škálování a otočení. Změna velikosti je isotropic. Implementace dva prstem otočení s volba škálování je problematické, protože pohybů prstem jsou v podstatě stejné.

`ScaleDualRotate` Možnost přidá jeden prstem otočení. Při jednom prstem nastavuje tažením objekt, objekt taženou nejprve otočen kolem jeho center tak, aby center objektu zarovnán s přetahování vektoru.

[ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) soubor obsahuje `Picker` s členů `TouchManipulationMode` výčtu:

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

Směrem dolní je `SKCanvasView` a `TouchEffect` připojené k jedné buňce `Grid` uzavře ho.

[ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) má souboru kódu na pozadí `bitmap` pole, ale není typu `SKBitmap`. Typ je `TouchManipulationBitmap` (třídu se krátce zobrazí):

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            SKBitmap bitmap = SKBitmap.Decode(skStream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

V konstruktoru vytvoří `TouchManipulationBitmap` objekt, předejte do konstruktoru `SKBitmap` získané z vložený prostředek. Konstruktor se ukončí nastavení `Mode` vlastnost `TouchManager` vlastnost `TouchManipulationBitmap` objekt, který chcete členem `TouchManipulationMode` – výčet.

`SelectedIndexChanged` Obslužné rutiny pro `Picker` také nastaví to `Mode` vlastnost:

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

`TouchAction` Obslužnou rutinu `TouchEffect` instanci ve voláních souboru XAML dvě metody v `TouchManipulationBitmap` s názvem `HitTest` a `ProcessTouchEvent`:

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

Pokud `HitTest` metoda vrátí `true` & #x 2014; význam, prstem má dotýkal obrazovky v oblasti obsazena rastrový obrázek & #x 2014; potom touch ID je přidán do `TouchIds` kolekce. Toto ID představuje posloupnost událostí dotykového ovládání pro tento prstu, dokud prstu zruší na obrazovce. Pokud více prsty touch rastrového obrázku, pak se `touchIds` kolekce obsahuje touch ID pro každý prstu.

`TouchAction` Obslužná rutina také voláním `ProcessTouchEvent` třídy v `TouchManipulationBitmap`. To je, kdy některé (ale ne všechny) z reálného touch zpracování probíhá.

[ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) Třída představuje obálkovou třídu pro `SKBitmap` obsahující kód pro vykreslení bitovou mapu a zpracování události dotykového ovládání. Funguje ve spojení s více zobecněn kód `TouchManipulationManager` (která se krátce zobrazí).

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

To `Matrix` vlastnost je Akumulovaná transformace vyplývající z veškerou aktivitu dotykového ovládání. Jak zjistíte, každá událost touch je přeložit na přehled, který je pak zřetězen s `SKMatrix` hodnotě uložené `Matrix` vlastnost.

`TouchManipulationBitmap` Objekt nevykresluje sám jeho `Paint` metoda. Argument je `SKCanvas` objektu. To `SKCanvas` již můžete mít transformace použít, proto `Paint` metoda zřetězí `Matrix` vlastnost přidružené rastrového obrázku na existující transformace a obnoví na plátno po dokončení:

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

`HitTest` Metoda vrátí `true` Pokud uživatel dotykem obrazovky na místo uvnitř bitmapy. Jako uživatel manipuluje bitovou mapu, bitmapy může otáčet nebo i (pomocí kombinace volba změny velikosti a oběh) být ve tvaru rovnoběžník. Může obávat, který `HitTest` metoda musí implementovat v takovém případě místo komplexní geometrie analýzu.

Zástupce je však k dispozici:

Určení, zda bod leží v hranicích transformovaných obdélníku je stejný jako určení toho, jestli bod inverzní transformovaných leží v hranicích Netransformovaný rámečku. Který je mnohem snazší výpočtu a můžete použít vhodného `Contains` metoda definované `SKRect`:

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

Druhý veřejná metoda v `TouchManipulationBitmap` je `ProcessTouchEvent`. Když tato metoda je volána, již bylo zjištěno, že událost touch patří do této konkrétní rastrového obrázku. Metoda udržuje slovník [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) objekty, které je jednoduše předchozího bodu a nový bod každý prstu:

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

V `Moved` a `Released` události, volání metod `Manipulate`. V těchto časech `touchDictionary` obsahuje jeden nebo více `TouchManipulationInfo` objekty. Pokud `touchDictionary` obsahuje jednu položku, je pravděpodobné, který `PreviousPoint` a `NewPoint` hodnoty nerovné a představují pohyb prstu. Pokud více prsty, které jsou dotykové ovládání bitovou mapu, slovník obsahuje více než jednu položku, ale pouze jeden z těchto položek má jiný `PreviousPoint` a `NewPoint` hodnoty. Všechny ostatní mají stejné `PreviousPoint` a `NewPoint` hodnoty.

To je důležité: `Manipulate` metoda můžete předpokládat, že ji zpracovává pohyb pouze jedním prstem. V době toto volání se žádné z ostatních prstů přesunutí a pokud se skutečně přesouváte (jako je pravděpodobně), tyto pohybů budou zpracovány v budoucí volání `Manipulate`.

`Manipulate` Slovníku metoda nejprve zkopíruje do pole pro usnadnění práce. Ignoruje jakoukoli jinou hodnotu než první dva záznamy. Pokud více než dva prsty, které se pokoušejí o manipulaci s bitovou mapu, další se ignorují. `Manipulate` je posledním členem `TouchManipulationBitmap`:

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

Pokud je jedním prstem manipulace rastrového obrázku, `Manipulate` volání `OneFingerManipulate` metodu `TouchManipulationManager` objektu. Pro dva prsty, zavolá `TwoFingerManipulate`. Argumenty, které mají tyto metody jsou stejné: `prevPoint` a `newPoint` argumenty představují prstu, který je přesunutí. Ale `pivotPoint` argument je jiný pro dvě volání:

Pro manipulaci s jedním prstem `pivotPoint` je středu bitové mapy. To je umožnit pro jeden prstem otočení. Pro manipulaci s dvěma prstu, událost označuje pohyb pouze jedním prstem tak, aby `pivotPoint` je prstu, který není přesunutí.

V obou případech `TouchManipulationManager` vrátí `SKMatrix` hodnotu, která metoda zřetězí s aktuálním `Matrix` vlastnost který `TouchManipulationPage` k vykreslení bitové mapy.

`TouchManipulationManager` je zobecněn a používá žádné soubory s výjimkou `TouchManipulationMode`. Je možné používat tuto třídu bez změn v aplikaci. Definuje vlastnosti jediné typu `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


Však budete pravděpodobně chtít vyhnout `AnisotropicScale` možnost. Je velmi snadné tato možnost k manipulaci s bitovou mapu, aby jednoho z faktorů škálování klesne na nulu. Který zpřístupňuje rastrový obrázek zmizí z nebyl zřejmý nikdy k vrácení. Pokud skutečně potřebujete volba škálování, budete chtít vylepšit logiku, aby se zabránilo nežádoucí výsledky.

`TouchManipulationManager` využívá vektorů, ale vzhledem k tomu, že neexistuje žádné `SKVector` struktury SkiaSharp, `SKPoint` bude místo něj použita. `SKPoint` operátor odčítání a výsledek lze považovat za vektor podporuje. Pouze konkrétní vektoru logiky, která potřeby přidat je `Magnitude` výpočet:

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

Vždy, když byla vybrána otočení, zpracovat manipulaci jeden prstem a dva prstem metody je oběh nejprve. Pokud se detekuje všechny otočení komponentu otočení efektivně odebrat. Co je ještě interpretována jako posouvání a změna měřítka.

Tady je `OneFingerManipulate` metoda. Pokud jeden prstem otočení není povolený, pak logika je jednoduchý & #x 2014; jednoduše používá předchozího bodu a nový bod vytvořit vektor s názvem `delta` odpovídající přesněji překlad. S jedním prstem otočení povolené metoda použije úhly z bodu pivot (center bitmapy) do předchozího bodu a nový bod k vytvoření matice otočení:

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

V `TwoFingerManipulate` metody bodem pivot je pozice prstu, který není v tomto případě konkrétní touch přesunutí. Je velmi podobný jako jeden prstem otočení je oběh a potom s názvem vektoru `oldVector` (podle předchozího bodu) se upraví pro otáčení. Přesun zbývajících interpretována jako škálování:

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

Můžete si všimnout, že neexistuje žádné explicitní překlad v této metodě. Ale, jak `MakeRotation` a `MakeScale` metody jsou založeny na bod pivot a který obsahuje implicitní překlad. Pokud používáte dvě prsty, které pro bitovou mapu a je přetáhnete ve stejném směru `TouchManipulation` získají řady událostí touch střídavých mezi dvěma prsty. Jako každý prstem přesune relativně k jiných, změnu velikosti nebo otáčení výsledky, ale je Negované podle jiných prstu pohyb a výsledkem je, překlad.

Jenom zbývající část **Touch manipulaci s** stránka je `PaintSurface` obslužné rutiny v `TouchManipulationPage` souboru kódu na pozadí. Volá `Paint` metodu `TouchManipulationBitmap`, které se vztahují matice představující Akumulovaná touch aktivity:

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

`PaintSurface` Obslužná rutina se ukončí zobrazení `MatrixDisplay` objekt zobrazující matice Akumulovaná touch:

[![](touch-images/touchmanipulation-small.png "Trojitá snímek obrazovky stránky Touch manipulaci s")](touch-images/touchmanipulation-large.png "Trojitá snímek obrazovky stránky Touch manipulace")

## <a name="manipulating-multiple-bitmaps"></a>Manipulace s více rastrové obrázky

Jednou z výhod, jako izolace touch zpracování kódu v třídách `TouchManipulationBitmap` a `TouchManipulationManager` je možnost opakovaně použít tyto třídy v aplikaci, která umožňuje uživatelům pracovat s více bitmapy.

**Rastrový obrázek bodový zobrazení** stránky ukazuje, jak to provést. Místo definice pole typu `TouchManipulationBitmap`, [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) třída definuje `List` objektů rastrového obrázku:

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
                using (SKManagedStream skStream = new SKManagedStream(stream))
                {
                    SKBitmap bitmap = SKBitmap.Decode(skStream);
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

Konstruktor načte ve všech rastrových obrázků, k dispozici jako vložené prostředky a přidá je do `bitmapCollection`. Všimněte si, že `Matrix` vlastnost inicializovala na každém `TouchManipulationBitmap` objektu, tak se levém horním rohu každé bitmapy posunut 100 pixelů.

`BitmapScatterView` Stránky je rovněž třeba zpracování událostí dotykového ovládání pro více bitmapy. Místo definice `List` z touch ID aktuálně manipulovat `TouchManipulationBitmap` objekty, tento program vyžaduje slovník:

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

Všimněte si jak `Pressed` projde logiky `bitmapCollection` pozpátku. Rastrových obrázků, které často překrývají. Rastrové obrázky později v kolekci vizuálně leží na rastrové obrázky dříve v kolekci. Pokud existují více rastrové obrázky v části prstu, který stiskem tlačítka na obrazovce, nejhornější ten musí být ten, který je zpracováván této prstu.

Všimněte si také, že `Pressed` logiku přesune tento rastrového obrázku na konec kolekce tak, aby vizuálně přesune do horní části balík jiné bitmapy.

V `Moved` a `Released` události, `TouchAction` volání obslužné rutiny `ProcessingTouchEvent` metoda v `TouchManipulationBitmap` stejně jako starší program.

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

Kód prochází kolekci a zobrazí balík bitmap od začátku kolekce na konec:

[![](touch-images/bitmapscatterview-small.png "Trojitá snímek obrazovky stránky zobrazení bodový rastrový obrázek")](touch-images/bitmapscatterview-large.png "Trojitá snímek obrazovky stránky zobrazení bodový rastrového obrázku")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [Vyvolání událostí z efekty](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
