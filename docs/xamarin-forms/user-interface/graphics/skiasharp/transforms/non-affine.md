---
title: "Afinní transformace"
description: "Vytvoření perspektivy a Sbíhavost důsledky s třetí sloupec maticové transformace"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 2e2e83404bc93bd07885008b868c51eba2ff7140
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="non-affine-transforms"></a>Afinní transformace

_Vytvoření perspektivy a Sbíhavost důsledky s třetí sloupec maticové transformace_

Překlad, změna velikosti, otáčení a zkosení jsou všechny klasifikovány jako *afinní* transformace. Afinní transformace zachovat paralelní řádky. Pokud jsou dva řádky paralelní před transformovat, zůstanou paralelní po transformaci. Obdélníky jsou vždy transformuje na parallelograms.

SkiaSharp je však také schopná-afinní transformace, které mají možnost transformace obdélníku do jakékoli konvexní čtyřúhelník:

![](non-affine-images/nonaffinetransformexample.png "Převede na konvexní čtyřúhelník rastrový obrázek")

Konvexní čtyřúhelník je okolo obrázek s vnitřní úhly vždy menší než 180 stupňů a strany, které si navzájem zasahují.

Bez afinní transformace výsledek třetí řádek matice transformace nastavena na hodnoty než 0, 0 a 1. Kompletní `SKMatrix` násobení je:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

Výsledná transformace vzorce jsou:

x' = ScaleX·x + SkewX·y + TransX

y' = SkewY·x + ScaleY·y + TransY

z` = Persp0·x + Persp1·y + Persp2

Základní pravidlo použití matice 3 3 pro dvourozměrná transformace je, že všechno, co na rovině zůstává, kde Z rovná 1. Pokud `Persp0` a `Persp1` mají hodnotu 0, a `Persp2` hodnotu 1, transformovat přesunul souřadnice Z této plochy.

Pokud chcete obnovit to dvourozměrná transformace, je třeba souřadnice přesunout zpět touto rovinou. Další krok je povinný. X', y ", a z"hodnoty musí být rozdělené podle z':

x" = x' / z'

y"= y, nebo z.

z" = z' / z' = 1

Toto jsou známé jako *homogenní souřadnice* a byly vyvinuty společností matematikovi srpen Ferdinand Möbius, mnohem lepší známé pro jeho topologické oddity pruhu Möbius.

Pokud z, 0, výsledkem dělení nekonečné souřadnice. Ve skutečnosti byl jeden ze společnosti Möbius motivace pro vývoj homogenní souřadnice možnost představují nekonečné hodnoty s konečná čísla.

Při zobrazení grafiky, ale budete chtít vyhnout vykreslování něco s souřadnice, které transformace nekonečné hodnoty. Tyhle souřadnice nebude vykreslen. Vše v blízkosti Tyhle souřadnice bude velmi velké a pravděpodobně ne vizuálně souvislého.

V této rovnici nechcete hodnotu z "stal nula:

z` = Persp0·x + Persp1·y + Persp2

`Persp2` Buňky může být nula nebo není nula. Pokud `Persp2` nula, pak je z' je nulová bodu (0, 0), a které nejsou obvykle žádoucí vzhledem k tomu, že tento bod je velmi běžné u dvourozměrná grafiky. Pokud `Persp2` není rovný nule, a pokud je bez ztráty Obecné `Persp2` vyřešen v 1. Například, pokud zjistíte, že `Persp2` by měla být 5, pak můžete jednoduše rozdělíte všechny buňky v matici o 5, díky čemuž `Persp2` rovno 1 a výsledkem bude stejná.

Z těchto důvodů `Persp2` často stanoví na 1, který má stejnou hodnotu v matici identity.

Obecně platí `Persp0` a `Persp1` jsou malé čísla. Předpokládejme například, že začnete s matice identity, ale sada `Persp0` k 0,01:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

Transformace vzorce jsou:

x: = x / (0.01·x + 1)

y' = y / (0.01·x + 1)

Tato transformace teď použijte k vykreslení pole čtvercový 100 pixelů umístěný na počátku. Zde je, jak jsou transformovány čtyři rohy:

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

Pokud x je 100 a potom z' jmenovatel je 2, takže souřadnice x a y je oproti jiným situacím poloviční. Na pravé straně pole bude kratší než na levé straně:

![](non-affine-images/nonaffinetransform.png "Pole podrobí-afinní transformace")

`Persp` Součástí názvy těchto buněk odkazuje na "perspektivy", protože perspektivního zkreslení naznačuje, že pole se teď sklopí s pravé straně dále za prohlížeč.

**Perspektivy testu** stránce můžete experimentovat s hodnotami `Persp0` a `Pers1` k podívat, jak pracují. Přiměřené hodnoty těchto buňkách matice jsou tak malá, který `Slider` v univerzální platformu Windows nemůže zpracovat správně je. Pro přizpůsobení UWP problém, dva `Slider` elementů v [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) muset inicializována tak, aby rozsahu od -1 do 1:

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

Obslužné rutiny události pro posuvníků v [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) souboru kódu tak, aby se v rozsahu mezi –0.01 a 0,01 dělení hodnot po 100. Kromě toho konstruktoru načítat rastrový obrázek:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

`PaintSurface` Vypočítá obslužná rutina `SKMatrix` hodnotu s názvem `perspectiveMatrix` na základě hodnot tyto dva jezdce dělený 100. To je v kombinaci s dvěma převede transformace, které se umístí středu Tato transformace v centru bitmapy:

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

Zde jsou některé obrázky, ukázka:

[![](non-affine-images/testperspective-small.png "Trojitá snímek obrazovky stránky perspektivy testu")](non-affine-images/testperspective-large.png#lightbox "Trojitá snímek obrazovky stránky perspektivy testu")

Proto byste posuvníků, zjistíte, že hodnoty nad rámec 0.0066 nebo pod –0.0066 způsobit bitovou kopii k najednou fractured a osamocené. Rastrový obrázek transformaci je hranaté 300 pixelů. Je transformovat relativně k jeho center, takže souřadnice bitmapy v rozsahu od –150 do 150. Odvolat, hodnotu z "je:

z` = Persp0·x + Persp1·y + 1

Pokud `Persp0` nebo `Persp1` je větší než 0.0066 nebo pod –0.0066, pak je vždy některé souřadnice rastrového obrázku, jejímž výsledkem z "hodnota nula. Která způsobí dělení nulou a vykreslování bude času. Pokud používáte jiný afinní transformace, budete chtít vyhnout vykreslování nic s souřadnice, které způsobí dělení nulou.

Obecně platí, které nebudou nastavení `Persp0` a `Persp1` izolovaně. Je také často nutné nastavit další buněk v matici k dosažení určitých typů-afinní transformace.

Jeden-afinní transformace je *Sbíhavost transformace*. Tento typ-afinní transformace uchovává celkové rozměry obdélníku ale zužuje jedné straně:

![](non-affine-images/tapertransform.png "Pole podrobí Sbíhavost transformace")

[ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) Třída provádí zobecněný výpočtu-afinní transformace na základě těchto parametrů:

- Obdélníková velikost bitové kopie transformace,
- výčet, který označuje na straně, který zužuje, rámeček
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

Tato třída se používá v **Sbíhavost transformace** stránky. Vytvoří dvě souboru XAML instance `Picker` elementy a vyberte hodnoty výčtu a `Slider` pro výběr Sbíhavost podílem. [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) Kombinuje obslužná rutina převede Sbíhavost transformace se dvěma transformací aby transformace relativně k levém horním rohu bitmapy:

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

Dalším typem zobecněný-afinní transformace je 3D otočení, které ukazují další článek, [3D otočení](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md).

Afinní transformace můžete převést obdélník do jakékoli konvexní čtyřúhelník. Tento postup je znázorněn podle **zobrazit bez Afinní matici** stránky. Je velmi podobné **zobrazit Afinní matici** stránku z [matice transformuje](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md) článek s tím rozdílem, že má čtvrtý `TouchPoint` objektu k manipulaci s čtvrtý rohu bitmapy:

[![](non-affine-images/shownonaffinematrix-small.png "Trojitá snímek obrazovky stránky zobrazit bez Afinní matici")](non-affine-images/shownonaffinematrix-large.png#lightbox "Trojitá snímek obrazovky stránky zobrazit bez Afinní matici")

Tak dlouho, dokud není pokusí úhlu interior jednoho rozích bitmapy větší než 180 stupňů nebo dvě strany cross navzájem, program úspěšně vypočítá transformace pomocí této metody z [ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) třídy:

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

Pro usnadnění výpočtu tato metoda získá celkový transformace jako produkt tři samostatné transformací, které jsou zde symbolized se šipkami znázorňující, jak tyto transformace upravit čtyři rohy bitmapy:

(0, 0) → (0, 0) → (0, 0) → (x 0, y0) (levém)

(0, H) → (0, 1) → (0, 1) → (x1, y1) (levém)

(W, 0) → (1, 0) → (1, 0) → (x 2, y2) (pravém)

(W, H) → (1, 1) → (a, b) → (x 3, y3) (pravém)

Poslední souřadnice vpravo jsou čtyři body přidružené k touch čtyři body. Jedná se o konečnou souřadnice rozích bitové mapy.

W a H představují šířka a výška bitové mapy. První transformace (`S`) jednoduše škáluje rastrového obrázku na čtverce 1 pixel. Druhý transformace je – afinní transformace `N`, a třetí je afinní transformace `A`. Tento afinní transformace je založena na tři body, takže je stejně jako dříve afinní [ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) metoda a nebude zahrnovat čtvrtý řádek s (bodu a, b).

`a` a `b` tak, aby třetí transformace afinní výpočtu hodnot. Kód získá inverzní afinní transformace a použije tento mapovat pravém dolním rohu. Je bod (a, b).

Jiné použití-afinní transformace je tak, aby napodoboval prostorovou grafiku. V následující článek [3D otočení](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md) najdete v článku o otočení dvourozměrná obrázek v 3D prostoru.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
