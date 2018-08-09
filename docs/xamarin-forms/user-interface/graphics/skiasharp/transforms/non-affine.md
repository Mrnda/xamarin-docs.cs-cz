---
title: Neafinní transformace
description: Tento článek vysvětluje, jak vytvořit perspektivy a Sbíhavost efekty třetí sloupec transformační matice a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 13f2a1160d012a6b7720bd84340a1cdd0f991535
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615649"
---
# <a name="non-affine-transforms"></a>Neafinní transformace

_Vytvoření perspektivy a Sbíhavost efekty s třetí sloupec transformační matice_

Překlad, škálování, otočení a zkosení jsou všechny klasifikované jako *nastavená na affine* transformace. Afinní transformace zachovat paralelní řádky. Pokud jsou dva řádky paralelní před transformace, zůstanou paralelní po transformaci. Obdélníky jsou vždy transformuje na parallelograms.

Ve Skiasharpu je však také schopné neafinní transformace, které mají funkce pro transformaci obdélníku do jakékoli konvexní čtyřúhelník:

![](non-affine-images/nonaffinetransformexample.png "Transformuje na konvexní čtyřúhelník rastrový obrázek")

Konvexní čtyřúhelník je obrázek čtyřsměrná s vnitřní úhly vždy menší než 180stupňový rozsah s orientací a stran, které není překračují mezi sebou.

Non-nastavená na affine transformuje výsledek, při třetím řádku transformační matice je nastavena na hodnotu jinou než 0, 0 a 1. Kompletní `SKMatrix` násobení je:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

Vzorce výsledná transformace jsou:

x' = ScaleX·x + SkewX·y + TransX

y' = SkewY·x ScaleY·y + TransY

z' = Persp0·x Persp1·y + Persp2

Základní pravidlo použití 3 3 matice pro dvourozměrná transformace je, že všechno, co v rovině zůstanou, kde Z rovná 1. Není-li `Persp0` a `Persp1` mají hodnotu 0, a `Persp2` rovná 1, transformací, která se má přesunout souřadnice Z této plochy.

Pokud chcete obnovit to dvourozměrná transformace, souřadnice musí přesunout zpátky na této roviny. Další krok je povinný. X ", y", a z "hodnoty musí být rozdělené podle z":

x"= x' / z.

y"= y' / z.

z"= z" / z' = 1

Toto jsou známé jako *homogenní souřadnice* a byly vytvořeny podle matematikovi srpna Ferdinand Möbius, mnohem lepší známé pro jeho topologické oddity Möbius pruh.

Pokud z "je 0, výsledkem dělení nekonečné souřadnice. Ve skutečnosti byl jeden ze společnosti Möbius motivace pro vývoj homogenní souřadnice schopnost představují nekonečné hodnoty se konečná čísla.

Při zobrazení grafiky, ale chcete se vyhnout vykreslování něco s souřadnice, které transformují nekonečné hodnoty. Tyhle souřadnice se nevykreslí. Všechno, co pixelům Tyhle souřadnice bude velmi velké a pravděpodobně není vizuálně souvislé.

V tomto rovnice nechcete, aby hodnotu z "změní na hodnotu nula:

z' = Persp0·x Persp1·y + Persp2

`Persp2` Buňka může být buď nula nebo není nula. Pokud `Persp2` je nula a pak z "je nula (0, 0) bodu a, která obvykle není žádoucí, protože tento bod je velmi běžné v dvojrozměrné grafiky. Pokud `Persp2` není rovno nule, pak pokud nedochází ke ztrátě obecnosti `Persp2` je pevně nastavena na 1. Například, pokud zjistíte, že `Persp2` by měl být 5, pak v 5, můžete rozdělit všechny buňky v matici jednoduše díky `Persp2` rovno 1 a výsledky budou stejné.

Z těchto důvodů `Persp2` je často pevně nastavena na 1, což je v jednotkovou matici stejnou hodnotu.

Obecně platí `Persp0` a `Persp1` jsou malé čísla. Předpokládejme například, že se pustíte do jednotkovou matici, ale sada `Persp0` na 0,01:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

Vzorce transformace jsou:

x! = x / (0.01·x + 1)

y' = y / (0.01·x + 1)

Tato transformace je teď možné použijte k vykreslení pole čtvercový 100 pixelů umístěny v počátku. Zde je, jak jsou transformovány čtyři rohy:

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

Pokud x je 100 a pak z "jmenovatel je 2, takže souřadnice x a y jsou oproti jiným situacím poloviční. Pravé straně pole bude kratší než na levé straně:

![](non-affine-images/nonaffinetransform.png "Pole podroben neafinní transformace")

`Persp` Součástí tyto názvy buňky označuje "perspektivy", protože perspektivního zkreslení naznačuje, že pole je nyní Naklápěcí s pravou stranou nic není dál od v prohlížeči.

**Perspektivy testování** stránce můžete experimentovat s hodnotami `Persp0` a `Pers1` získat představu pro jejich fungování. Přiměřené hodnoty těchto buňkách matice jsou to malé, který `Slider` na univerzální platformě Windows nelze správně jejich zpracování. Tak, aby vyhovovaly problém UPW, dva `Slider` prvky [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) inicializovat na rozsah od – 1 na 1:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.TestPerspectivePage"
             Title="Test Perpsective">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Label">
                    <Setter Property="HorizontalTextAlignment" Value="Center" />
                </Style>

                <Style TargetType="Slider">
                    <Setter Property="Minimum" Value="-1" />
                    <Setter Property="Maximum" Value="1" />
                    <Setter Property="Margin" Value="20, 0" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="persp0Slider"
                Grid.Row="0"
                ValueChanged="OnPersp0SliderValueChanged" />

        <Label x:Name="persp0Label"
               Text="Persp0 = 0.0000"
               Grid.Row="1" />

        <Slider x:Name="persp1Slider"
                Grid.Row="2"
                ValueChanged="OnPersp1SliderValueChanged" />

        <Label x:Name="persp1Label"
               Text="Persp1 = 0.0000"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Obslužné rutiny událostí pro jezdce v [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) soubor kódu na pozadí dělení hodnoty 100 tak, že jsou v rozsahu mezi –0.01 a 0,01. Kromě toho načte konstruktoru v rastrový obrázek:

```csharp
public partial class TestPerspectivePage : ContentPage
{
    SKBitmap bitmap;

    public TestPerspectivePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnPersp0SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp0Label.Text = String.Format("Persp0 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }

    void OnPersp1SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp1Label.Text = String.Format("Persp1 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }
    ...
}
```

`PaintSurface` Vypočítá obslužná rutina `SKMatrix` hodnotu s názvem `perspectiveMatrix` na základě hodnot těchto dvou posuvníky, vydělí se číslem 100. Toto číslo zkombinuje s dvěma přeložit transformace, které ukládají center Tato transformace v centru rastrového obrázku:

```csharp
public partial class TestPerspectivePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Calculate perspective matrix
        SKMatrix perspectiveMatrix = SKMatrix.MakeIdentity();
        perspectiveMatrix.Persp0 = (float)persp0Slider.Value / 100;
        perspectiveMatrix.Persp1 = (float)persp1Slider.Value / 100;

        // Center of screen
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);
        SKMatrix.PostConcat(ref matrix, perspectiveMatrix);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(xCenter, yCenter));

        // Coordinates to center bitmap on canvas
        float x = xCenter - bitmap.Width / 2;
        float y = yCenter - bitmap.Height / 2;

        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

Tady jsou některé ukázkové obrázky:

[![](non-affine-images/testperspective-small.png "Trojitá snímek obrazovky stránky perspektivy testování")](non-affine-images/testperspective-large.png#lightbox "Trojitá snímek obrazovky stránky perspektivy testování")

Jak můžete experimentovat s posuvníky, zjistíte, že hodnoty nad rámec 0.0066 nebo pod –0.0066 způsobit, že obrázek, který se náhle stát fractured a osamocené. Rastrový obrázek, který se má transformovat je čtverec 300 pixelů. Se transformuje vzhledem k jeho střed tak souřadnice rastrového obrázku v rozsahu od –150 150. Vzpomeňte si, že hodnotu z "je:

z' = Persp0·x Persp1·y + 1

Pokud `Persp0` nebo `Persp1` je větší než 0.0066 nebo pod –0.0066, pak je vždy některé souřadnice rastrový obrázek, který vede z' Hodnota nula. Který způsobí, že dělení nulou a vykreslování stane hustá doprava. Při použití neafinní transformace, chcete se vyhnout vykreslování k ničemu souřadnice, které způsobují dělení nulou.

Obecně platí, které nebudou nastavení `Persp0` a `Persp1` izolovaně. Je také často potřeba nastavit další buňky v matici k dosažení určitých typů neafinní transformace.

Je jedna neafinní transformace *Sbíhavost transformace*. Tento typ neafinní transformace zachovává celkový dimenze obdélník ale zužuje na jedné straně:

![](non-affine-images/tapertransform.png "Pole podroben Sbíhavost transformace")

[ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) Třída provádí výpočet zobecněný neafinní transformace na základě těchto parametrů:

- Obdélníková velikost obrázku, který se má transformovat,
- výčet, který označuje straně obdélníku, který zužuje,
- jiném výčtu, která určuje, jak zužuje a
- rozsah špičkou.

Zde je kód:

```csharp
enum TaperSide { Left, Top, Right, Bottom }

enum TaperCorner { LeftOrTop, RightOrBottom, Both }

static class TaperTransform
{
    public static SKMatrix Make(SKSize size, TaperSide taperSide, TaperCorner taperCorner, float taperFraction)
    {
        SKMatrix matrix = SKMatrix.MakeIdentity();

        switch (taperSide)
        {
            case TaperSide.Left:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp0 = (taperFraction - 1) / size.Width;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Top:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp1 = (taperFraction - 1) / size.Height;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Right:
                matrix.ScaleX = 1 / taperFraction;
                matrix.Persp0 = (1 - taperFraction) / (size.Width * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        break;
                }
                break;

            case TaperSide.Bottom:
                matrix.ScaleY = 1 / taperFraction;
                matrix.Persp1 = (1 - taperFraction) / (size.Height * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        break;
                }
                break;
        }
        return matrix;
    }
}
```

Tato třída se používá v **Sbíhavost transformace** stránky. Soubor XAML vytvoří dvě `Picker` prvky k výběru hodnot výčtu a `Slider` pro výběr Sbíhavost zlomek. [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) Obslužná rutina kombinuje Sbíhavost transformací, která se dvěma přeložit transformací, aby transformace vzhledem k levého horního rohu rastrového obrázku:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    TaperSide taperSide = (TaperSide)taperSidePicker.SelectedIndex;
    TaperCorner taperCorner = (TaperCorner)taperCornerPicker.SelectedIndex;
    float taperFraction = (float)taperFractionSlider.Value;

    SKMatrix taperMatrix =
        TaperTransform.Make(new SKSize(bitmap.Width, bitmap.Height),
                            taperSide, taperCorner, taperFraction);

    // Display the matrix in the lower-right corner
    SKSize matrixSize = matrixDisplay.Measure(taperMatrix);

    matrixDisplay.Paint(canvas, taperMatrix,
        new SKPoint(info.Width - matrixSize.Width,
                    info.Height - matrixSize.Height));

    // Center bitmap on canvas
    float x = (info.Width - bitmap.Width) / 2;
    float y = (info.Height - bitmap.Height) / 2;

    SKMatrix matrix = SKMatrix.MakeTranslation(-x, -y);
    SKMatrix.PostConcat(ref matrix, taperMatrix);
    SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(x, y));

    canvas.SetMatrix(matrix);
    canvas.DrawBitmap(bitmap, x, y);
}
```

Následuje několik příkladů:

[![](non-affine-images/tapertransform-small.png "Trojitá snímek obrazovky stránky Sbíhavost transformace")](non-affine-images/tapertransform-large.png#lightbox "Trojitá snímek obrazovky stránky Sbíhavost transformace")

3D otáčení, který je znázorněn v následujícím článku, je jiného typu zobecněný neafinní transformace [3D otáčení](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md).

Neafinní transformace můžete transformovat obdélníku do jakékoli konvexní čtyřúhelník. To je patrné podle **zobrazit Non-nastavená na Affine matici** stránky. Je velmi podobný **zobrazit nastavená na Affine matici** stránku ze [transformuje matice](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md) článku s tím rozdílem, že má čtvrtý `TouchPoint` objekt pro manipulaci s čtvrtý rohu rastrového obrázku:

[![](non-affine-images/shownonaffinematrix-small.png "Trojitá snímek obrazovky stránky zobrazit Non-nastavená na Affine matici")](non-affine-images/shownonaffinematrix-large.png#lightbox "Trojitá snímek obrazovky stránky zobrazit Non-nastavená na Affine matici")

Tak dlouho, dokud není při pokusu úhlu vnitřní jeden z rohů rastrového obrázku větší než 180 stupňů, nebo obě strany cross mezi sebou, program vypočítá úspěšně transformace pomocí této metody z [ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) třídy:

```csharp
static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL, SKPoint ptLR)
{
    // Scale transform
    SKMatrix S = SKMatrix.MakeScale(1 / size.Width, 1 / size.Height);

    // Affine transform
    SKMatrix A = new SKMatrix
    {
        ScaleX = ptUR.X - ptUL.X,
        SkewY = ptUR.Y - ptUL.Y,
        SkewX = ptLL.X - ptUL.X,
        ScaleY = ptLL.Y - ptUL.Y,
        TransX = ptUL.X,
        TransY = ptUL.Y,
        Persp2 = 1
    };

    // Non-Affine transform
    SKMatrix inverseA;
    A.TryInvert(out inverseA);
    SKPoint abPoint = inverseA.MapPoint(ptLR);
    float a = abPoint.X;
    float b = abPoint.Y;

    float scaleX = a / (a + b - 1);
    float scaleY = b / (a + b - 1);

    SKMatrix N = new SKMatrix
    {
        ScaleX = scaleX,
        ScaleY = scaleY,
        Persp0 = scaleX - 1,
        Persp1 = scaleY - 1,
        Persp2 = 1
    };

    // Multiply S * N * A
    SKMatrix result = SKMatrix.MakeIdentity();
    SKMatrix.PostConcat(ref result, S);
    SKMatrix.PostConcat(ref result, N);
    SKMatrix.PostConcat(ref result, A);

    return result;
}
```

Za účelem usnadnění výpočtu tato metoda získá celkový počet transformací, která se jako součin tři samostatné transformace, které jsou zde symbolized se šipkami, zobrazují, jak tyto soubory upravit čtyři rohy rastrového obrázku:

(0, 0) → → (0, 0) → (0, 0) x (0, y0) (vlevo nahoře)

(0, H) → (0, 1) → (0, 1) → (x1 y1) (levém)

(W, 0) → → (1, 0) → (1, 0) x (2, y2) (pravém)

(W, H) → prostředí → (a, b) → (1, 1) (x 3, y3) (pravém)

Poslední souřadnice na pravé straně jsou čtyři body přidružené k čtyři dotykovými body. Jedná se o konečné souřadnice rohů rastrového obrázku.

Š a představují šířku a výšku rastrového obrázku. První transformace (`S`) jednoduše škáluje rastrového obrázku na čtverec 1 pixelu. Druhý transformace je neafinní transformace `N`, a třetí je nastavená na affine transformace `A`. Tento afinní transformace jsou založené na tři body, je stejně jako dříve afinní [ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) metoda a nezahrnuje čtvrtý řádek s (bodu a, b).

`a` a `b` hodnoty se počítají tak, aby byla nastavená na affine třetí transformace. Kód získá inverzní afinní transformace a použije ho k mapování pravého dolního rohu. To je bod (a, b).

Další používání neafinní transformace je tak, aby napodoboval 3D grafiky. V následujícím článku [3D otáčení](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md) uvidíte obměna dvojrozměrného obrázku v 3D prostoru.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
