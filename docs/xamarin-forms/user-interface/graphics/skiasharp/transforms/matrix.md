---
title: "Maticové transformace"
description: "Ponořit hlouběji do SkiaSharp transformací s univerzální transformační matice"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: 85402768990869a2121cdea5ab7d232d80d64ed2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="matrix-transforms"></a>Maticové transformace

_Ponořit hlouběji do SkiaSharp transformací s univerzální transformační matice_

Všechny transformací použitá pro `SKCanvas` objekt se konsolidují v jednu instanci [ `SKMatrix` ](https://developer.xamarin.com/api/type/SkiaSharp.SKMatrix/) struktury. Toto je podobná těm ve všech systémech moderní 2D grafiky standardní 3 3 transformace matice.

Jak jste se seznámili, můžete použít transformací v SkiaSharp, bez znalosti o transformaci matice, ale transformace matice je důležité z hlediska teoretické a je velmi důležité při použití transformace k úpravě cesty nebo pro zpracování vstupu komplexní touch, obě což je ukázán v tomto článku a další.

![](matrix-images/matrixtransformexample.png "Rastrový obrázek podrobí afinní transformace")

Aktuální transformační matice použít `SKCanvas` je k dispozici kdykoli přímým přístupem jen pro čtení [ `TotalMatrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.TotalMatrix/) vlastnost. Můžete nastavit nový matice transformace pomocí [ `SetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.SetMatrix/p/SkiaSharp.SKMatrix/) metoda a vy můžete obnovit tento transformační matice výchozí hodnoty pomocí volání [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix/).

Jediným jiných `SKCanvas` člen, který přímo funguje s transformační matice na plátno [ `Concat` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Concat/p/SkiaSharp.SKMatrix@/) který zřetězí dvě matic vynásobením je společně.

Maticové transformace výchozí matice identity a skládá se z 1 na v diagonálních buněk a 0 je everywhere else:

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

Můžete vytvořit matici identity pomocí statické [ `SKMatrix.MakeIdentity` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeIdentity()/) metoda:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

`SKMatrix` Nemá výchozí konstruktor *není* vrátit matici identity. Vrátí matice se všemi buněk nastaven na hodnotu nula. Nepoužívejte `SKMatrix` konstruktor Pokud budete chtít nastavit těchto buněk ručně.

Když SkiaSharp vykreslí grafický objekt, je 1 3 matice s 1 ve sloupci třetí efektivně převést každý bod (x, y):

<pre>
| x  y  1 |
</pre>

Tato matice 1 3 představuje bod trojrozměrné s souřadnice nastavena na hodnotu 1. Proč matice dvourozměrná transformace vyžaduje práce v tři dimenze je matematickém důvodů, proč (popsané později). Si můžete představit Tato matice 1 3 jako představující bod v 3D systém souřadnic, ale vždy na 2D rovinu, kde Z hodnotu 1.

Tato matice 1 3 násobí transformační matice a výsledkem je bodu na plátně se vykresluje:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

Pomocí násobení matic standardní, převedený body jsou následující:

x' = x

y' = y

z' = 1

To je výchozí transformace.

Při `Translate` metoda je volána na `SKCanvas` objekt, `tx` a `ty` argumenty, které mají `Translate` metoda stane první dva buněk ve třetím řádku transformační matice:

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

Vynásobením je teď následujícím způsobem:

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

Zde jsou transformace vzorce:

x: = x + tx

y' = y + ty

Škálování faktory mít výchozí hodnotu 1. Při volání `Scale` metoda na nový `SKCanvas` objektu, výsledná transformační matice neobsahuje `sx` a `sy` argumenty v buňkách diagonálních:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

Transformace vzorce jsou následující:

x' = sx · x

y' = sy · y

Maticové transformace po volání `Skew` obsahuje dva argumenty v buňkách matice přiléhající k škálování faktory:

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

Transformace vzorce jsou:

x' = x + xSkew · y

y' = ySkew · x + y

Pro volání `RotateDegrees` nebo `RotateRadians` pro úhel α, transformační matice vypadá takto:

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

Zde jsou transformace vzorce:

x: cos(α) · = x - sin(α) · y

y' = sin(α) · x - cos(α) · y

Když α je 0 stupňů, je identita matice. Když α 180 stupňů, transformační matice vypadá takto:

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

Otočení 180 stupňů je ekvivalentní objekt překlopení vodorovně a svisle, který provádí taky nastavení škálování faktory – 1.

Všechny tyto typy transformací jsou klasifikovány jako *afinní* transformace. Afinní transformace zahrnují nikdy třetí sloupec matice, která zůstává na výchozí hodnotu 0, 0 a 1. Článek [Non-Afinní transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md) popisuje-afinní transformace.

## <a name="matrix-multiplication"></a>Násobení matic

S použitím transformační matice jeden velký výhoda spočívá v tom, že je možné získat složené transformací násobení matic, což se často označuje v dokumentaci k SkiaSharp jako *zřetězení*. Mnoho transformovat související metody v `SKCanvas` k "předběžné zřetězení" nebo "pre-concat." Vztahuje se pracovního násobení, což je důležité, protože není komutativní násobení matic.

Například v dokumentaci [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) metoda říká to "Pre-concats aktuální matice s zadaný překlad" při v dokumentaci pro [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) Metoda upozorněním, že IT oddělení "Pre-concats aktuální matice s zadaný měřítkem."

To znamená, že transformace určeného volání metody, které je násobitel (levé operand) a aktuální transformační matice se násobenec (pravém operand).

Předpokládejme, že `Translate` nazývá následované `Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

`Scale` Transformace se násobí hodnotou `Translate` transformace pro matici složené transformace:

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` může být volána před provedením `Translate` podobné výjimky:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

V takovém případě je obrácený pořadí násobení a škálování faktory efektivně se použijí pro překlad faktory:

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

Tady je `Scale` metoda s bodem pivot:

```csharp
canvas.Scale(sx, sy, px, py);
```

Jde o ekvivalent následující volání přeložit a škálování:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Tři transformační matice se násobí v obráceném pořadí z jak metody zobrazují v kódu:

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

### <a name="the-skmatrix-structure"></a>Struktura SKMatrix

`SKMatrix` Struktura definuje devět vlastností čtení/zápisu typu `float` odpovídající devět buňkách matice transformace:

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` také definuje vlastnost s názvem [ `Values` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix.Values/) typu `float[]`. Tuto vlastnost lze nastavit nebo získat devět hodnot v jednom spuštění v pořadí `ScaleX`, `SkewX`, `TransX`, `SkewY`, `ScaleY`, `TransY`, `Persp0`, `Persp1`, a `Persp2`.

`Persp0`, `Persp1`, A `Persp2` buněk, které jsou popsané v článku, [Non-Afinní transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md). Pokud tyto buňky mají výchozí hodnoty 0, 0 a 1, pro transformaci se násobí hodnotou souřadnice bodu takto:

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

x' = ScaleX · x + SkewX · y + TransX

y' = SkewX · x + ScaleY · y + TransY

z' = 1

Toto je kompletní dvourozměrná afinní transformace. Afinní transformace zachovává paralelní řádky, což znamená, že obdélníku nikdy převede na jakoukoli jinou hodnotu než rovnoběžník.

`SKMatrix` Struktura definuje několik statické metody pro vytvoření `SKMatrix` hodnoty. Tyto všechny návratové `SKMatrix` hodnoty:

- [`MakeTranslation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeTranslation/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/System.Single/System.Single/) s bodem pivot
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/) pro úhel v radiánech
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/System.Single/System.Single/) pro úhel v radiánech s bodem pivot
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/)
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/System.Single/System.Single/) s bodem pivot
- [`MakeSkew`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeSkew/p/System.Single/System.Single/)

`SKMatrix` také definuje několik statické metody, které řetězení dvou matic, což znamená, že je násobení. Tyto metody jsou pojmenované `Concat`, `PostConcat`, a `PreConcat`, a existují dvě verze jednotlivých. Tyto metody mít žádné návratové hodnoty; Místo toho budou odkazovat na existující `SKMatrix` hodnoty prostřednictvím `ref` argumenty. V následujícím příkladu `A`, `B`, a `R` (pro "výsledek") jsou všechny `SKMatrix` hodnoty.

Dva `Concat` metody jsou volány takto:

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

To proveďte následující násobení:

R = B × A

Jiné metody mít pouze dva parametry. První parametr je upravený a pro návrat z volání metody, obsahuje součin dvou matice. Dva `PostConcat` metody jsou volány takto:

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

Tyto volání provádět následující operace:

A = A × B

Dva `PreConcat` jsou podobné metody:

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

Tyto volání provádět následující operace:

A = B × A

Verze těchto volání metod se všemi `ref` argumenty jsou mírně efektivnější při volání základní implementace, ale může být matoucí někomu čtení kódu a za předpokladu, že to cokoli pomocí `ref` argument je změnit metodu. Kromě toho je často vhodnější předat argument, který je výsledkem mezi `Make` metody, třeba:

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

Tím se vytvoří následující tabulku:

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

Toto je transformace škálování násobí hodnotou transformace přeložit. V tomto konkrétním případě `SKMatrix` struktura poskytuje metodu s názvem zástupce [ `SetScaleTranslate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.SetScaleTranslate/p/System.Single/System.Single/System.Single/System.Single/):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

Toto je jedna z několika prvních případech, kdy je bezpečně používat `SKMatrix` konstruktor. `SetScaleTranslate` Metoda nastaví všechny devět buňkách matice. Je také bezpečně používat `SKMatrix` konstruktor s statických `Rotate` a `RotateDegrees` metody:

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

Tyto metody se *není* řetězení rotační transformace na existující transformace. Metody nastavit všechny buňkách matice. Jejich fungují stejně jako `MakeRotation` a `MakeRotationDegrees` metody s tím rozdílem, že nemáte v vytvořit instanci `SKMatrix` hodnotu.

Předpokládejme, že máte `SKPath` objekt, který chcete zobrazit, ale si přejete, že mají poněkud jinou orientaci nebo jiné centrálního bodu. Všechny souřadnice této cestě můžete upravit pomocí volání [ `Transform` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Transform/p/SkiaSharp.SKMatrix/) metodu `SKPath` s `SKMatrix` argument. **Cesta transformace** stránky ukazuje, jak to udělat. [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) Třídy odkazy `HendecagramPath` objekt v poli, ale používá jeho konstruktoru použití transformace na této cestě:

```csharp
public class PathTransformPage : ContentPage
{
    SKPath transformedPath = HendecagramArrayPage.HendecagramPath;

    public PathTransformPage()
    {
        Title = "Path Transform";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        SKMatrix matrix = SKMatrix.MakeScale(3, 3);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(360f / 22));
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(300, 300));

        transformedPath.Transform(matrix);
    }
    ...
}
```

`HendecagramPath` Objekt má ve středisku (0, 0), a 11 body hvězdy rozšířit i z tohoto centra 100 jednotkami ve všech směrech. To znamená, že cesta obsahuje kladné a záporné souřadnice. **Cesta transformace** stránky upřednostní pro práci s hvězdičkou třikrát jako velký a se všechny kladné souřadnice. Kromě toho nechce jeden bod hvězdy tak, aby odkazoval nahoru. Chce místo pro jeden bod hvězdy přejděte rovnou dolů. (Protože hvězdičkou má 11 body, nemůže mít oba.) To vyžaduje otáčení hvězdičkou o 360 stupňů dělený 22.

Konstruktoru vytvoří `SKMatrix` objektu ze tří samostatných transformace pomocí `PostConcat` metoda s vzoru následující, kde A, B a C jsou instance třídy `SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

Toto je řadu následných součinů, takže výsledek je následující:

A × B × C

Podpora po sobě jdoucích součinů porozumět, co každý transformace nemá. Transformace škálování zvětšuje velikost souřadnice cesta faktorem, 3, takže souřadnice v rozsahu od – 300 do 300. Otočit transformace otočí hvězdičky kolem jeho počátek. Transformace přeložit pak posune, je 300 pixelů vpravo a dolů, takže všechny souřadnice stát kladné.

Existují další pořadí, které produkují stejné matice. Zde je jiný:

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

To nejdřív otočí cesty okolo jeho center a převede jej 100 pixelů vpravo a směrem dolů, takže všechny souřadnice jsou uváděny kladná. Hvězdy je pak zvýšit velikost relativně k jeho nové levého horního rohu, který je bod (0, 0).

`PaintSurface` Obslužná rutina může vykreslit jednoduše tuto cestu:

```csharp
public class PathTransformPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Magenta;
            paint.StrokeWidth = 5;

            canvas.DrawPath(transformedPath, paint);
        }
    }
}

```

Zobrazí se v levém horním rohu na plátno:

[![](matrix-images/pathtransform-small.png "Trojitá snímek obrazovky stránky cesta transformace")](matrix-images/pathtransform-large.png "Trojitá snímek obrazovky stránky cesta transformace")

Konstruktor tohoto programu se vztahuje matice cestu s následující volání:

```csharp
transformedPath.Transform(matrix);
```

Cesta nemá *není* zachovat tato matice jako vlastnost. Místo toho používá pro transformaci se všechny souřadnice cesty. Pokud `Transform` nazývá znovu, Transformovat se použije znovu, a je jediným způsobem, můžete se vrátit je použitím jiného přehled, který vrátí zpět pro transformaci. Naštěstí `SKMatrix` definuje strukturu [ `TryInverse` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.TryInvert/p/SkiaSharp.SKMatrix@/) metoda, která získává matice, který obrátí dané matice:

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

Volání metody `TryInverse` protože ne všechny matice jsou invertible, ale bez invertible matice není mohou být použity k grafiky transformace.

Můžete taky použít transformační matice do `SKPoint` hodnota, pole bodů `SKRect`, nebo i právě jedno číslo programu. `SKMatrix` Struktura podporuje tyto operace s kolekci metod, které začínají slovem `Map`, například tyto:

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

Pokud použijete tento poslední metodu, mějte na paměti, že `SKRect` struktura není schopné představující otočený obdélníku. Metoda má smysl jenom `SKMatrix` hodnotu představující překlad a škálování.

### <a name="interactive-experimentation"></a>Interaktivní experimentování

Jedním ze způsobů podívat afinní transformace je interaktivně přesunutím tři rozích bitmapy kolem obrazovky a zobrazuje, jaké transformace výsledků. Toto je cílem **zobrazit Afinní matici** stránky. Tato stránka vyžaduje dvě třídy, které používá také v jiných ukázky:

[ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchPoint.cs) Zobrazí třída průhledná kruh, který lze přetáhnout po obrazovce. `TouchPoint` vyžaduje, aby `SKCanvasView` nebo element, který je nadřazeným objektem `SKCanvasView` mít [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchEffect.cs) připojen. Nastavte `Capture` vlastnost `true`. V `TouchAction` obslužné rutiny události, musí volat program `ProcessTouchEvent` metoda v `TouchPoint` pro každou `TouchPoint` instance. Vrátí metoda `true` Pokud událost touch vytvořily bod touch přesunutí. Navíc `PaintSurface` obslužné rutiny musí volat `Paint` metoda v každé `TouchPoint` instance, předávání do ní `SKCanvas` objekt.

`TouchPoint` ukazuje běžné způsobem, že SkiaSharp visual můžete zapouzdřené v samostatné třídy. Třída můžete definovat vlastnosti pro zadání vlastnosti vizuál a metodu s názvem `Paint` s `SKCanvas` argument může vykreslit ho.

`Center` Vlastnost `TouchPoint` Určuje umístění objektu. Tuto vlastnost lze nastavit inicializovat umístění; změny vlastností, když uživatel nastavuje tažením kruh kolem na plátno.

**Zobrazit Afinní stránce matice** taky vyžaduje [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/MatrixDisplay.cs) třídy. Tato třída zobrazí buňkách `SKMatrix` objektu. Má dvě veřejné metody: `Measure` získat dimenze vykreslené matice a `Paint` můžete ho zobrazit. Obsahuje třídy `MatrixPaint` vlastnost typu `SKPaint` , lze nahradit jinou velikost písma nebo barev.

[ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) soubor vytvoří `SKCanvasView` a připojí `TouchEffect`. [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) souboru kódu vytvoří tři `TouchPoint` objektů a potom je nastaví na pozic odpovídající tři rozích rastrového obrázku, který načte ze embedded prostředek:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    SKMatrix matrix;
    SKBitmap bitmap;
    SKSize bitmapSize;

    TouchPoint[] touchPoints = new TouchPoint[3];

    MatrixDisplay matrixDisplay = new MatrixDisplay();

    public ShowAffineMatrixPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }

        touchPoints[0] = new TouchPoint(100, 100);                  // upper-left corner
        touchPoints[1] = new TouchPoint(bitmap.Width + 100, 100);   // upper-right corner
        touchPoints[2] = new TouchPoint(100, bitmap.Height + 100);  // lower-left corner

        bitmapSize = new SKSize(bitmap.Width, bitmap.Height);
        matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                           touchPoints[1].Center,
                                           touchPoints[2].Center);
    }
    ...
}
```

Afinní matice je jedinečně definované tři body. Tří `TouchPoint` objektů odpovídá levém horním pravém horním a dolním rozích bitové mapy. Protože afinní matice je pouze schopná transformace obdélníku do rovnoběžník, čtvrtou je zahrnuto v další tři. Konstruktor končí volání `ComputeMatrix`, která vypočítá buňkách `SKMatrix` objekt z těchto tří bodů.

`TouchAction` Volání obslužné rutiny `ProcessTouchEvent` metoda jednotlivých `TouchPoint`. `scale` Hodnotu převede z Xamarin.Forms souřadnice pixelů:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = canvasView.CanvasSize.Width / (float)canvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                               touchPoints[1].Center,
                                               touchPoints[2].Center);
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Pokud existuje `TouchPoint` přesunul, pak zavolá metodu `ComputeMatrix` znovu a zruší platnost povrchu.

`ComputeMatrix` Metoda určuje matice implicitní tyto tři body. Matice názvem `A` transformací jeden pixelů odmocnina obdélníku do rovnoběžník založené na tři body při transformace škálování názvem `S` škáluje rastrového obrázku na čtvereček obdélníku jeden pixelů. Složené matice je `S` × `A`:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL)
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

        SKMatrix result;
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

Nakonec `PaintSurface` metoda vykreslí rastrový obrázek podle této matice, matice zobrazí v dolní části obrazovky a vykreslí body touch v tři rozích bitmapy:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap using the matrix
        canvas.Save();
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(matrix);

        matrixDisplay.Paint(canvas, matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));

        // Display the touchpoints
        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }
  }
```

Na obrazovce iOS níže znázorňuje bitovou mapu, když prvním načtení stránky, když ho dvou obrazovkách zobrazit po některé manipulaci:

[![](matrix-images/showaffinematrix-small.png "Trojitá snímek obrazovky stránky zobrazit Afinní matici")](matrix-images/showaffinematrix-large.png "Trojitá snímek obrazovky stránky zobrazit Afinní matici")

I když se zdá být jako body touch přetáhněte rozích rastrový obrázek, který je pouze dojem. Matice počítá z bodů touch transformuje bitovou mapu, aby rozích se shodovat s body dotykového ovládání.

Je více fyzických uživatelům přesunout, přizpůsobit a otočit bitmap není přetažením rozích, ale pomocí jednoho nebo dvou prsty přímo na objekt, který má přetáhněte, prohnutí a otočit. To je popsaná v další článku [Touch manipulaci s](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md).

### <a name="the-reason-for-the-3-by-3-matrix"></a>Z důvodu pro matici 3 3

Může se očekávat, že dvourozměrná grafika systému by vyžadovaly pouze matice transformace 2 2:

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

Tento postup funguje pro škálování, otáčení a i zkosení, ale není schopen nejzákladnější transformací, což je překlad.

Problém je, že představuje matici 2 2 *lineární* transformaci v dvěma rozměry. Lineární transformace zachovává některé základní aritmetické operace, ale jeden z důsledky je, že lineární transformace nikdy mění bodu (0, 0). Lineární transformace znemožňuje překlad.

V tři dimenze lineární transformace matice vypadat třeba takto:

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

Buňky s názvem bez přípony `SkewXY` znamená, že hodnota zkosí souřadnici X na základě hodnot y; buňky `SkewXZ` znamená, že hodnota zkosí souřadnici X na základě hodnot Z; a hodnoty zkreslit podobně pro druhý `Skew` buněk.

Je možné omezit Tato matice 3D transformace dvourozměrné roviny tak, že nastavení `SkewZX` a `SkewZY` na hodnotu 0, a `ScaleZ` na 1:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

Pokud dvourozměrná grafiky jsou vykreslovány zcela v rovině v 3D místa, kde Z hodnotu 1, násobení transformace vypadá takto:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Všechno, co zůstává na dvourozměrné roviny, kde Z rovná 1, ale `SkewXZ` a `SkewYZ` buněk efektivně stát dvourozměrná překlad faktorů.

Toto je, jak trojrozměrné lineární transformace slouží jako dvourozměrná – lineární transformace. (Obdobně, transformací v 3D grafiky jsou na základě matice 4 4.)

`SKMatrix` Struktury SkiaSharp definuje vlastnosti pro tento třetí řádek:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

Hodnoty nenulová `Persp0` a `Persp1` mít za následek transformace, které přesun objektů vypnout dvourozměrné roviny, kde Z hodnotu 1. Co se stane, když tyto objekty jsou přesunuta zpět do této roviny je popsaná v článku na [Non-Afinní transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md).


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
