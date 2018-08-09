---
title: Maticové transformace v ve Skiasharpu
description: Tento článek podrobně transformace SkiaSharp s univerzální transformační matice věnuje hlouběji a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: 8d5f1a08f7e1bff5ca2f9b696463bc03340476af
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615428"
---
# <a name="matrix-transforms-in-skiasharp"></a>Maticové transformace v ve Skiasharpu

_Ponořte se hlouběji do transformace SkiaSharp s univerzální transformační matice_

Všechny transformací použitá pro `SKCanvas` konsolidované objektu v jedné instance [ `SKMatrix` ](https://developer.xamarin.com/api/type/SkiaSharp.SKMatrix/) struktury. Toto je standardní matice 3 3 transformace podobné těm v všechny moderní 2D grafika systémy.

Jak už víte, bez znalosti o transformaci, matice, ale transformační matice je důležité z hlediska teoretické, a je zásadní význam při použití transformace na změnit cesty nebo pro zpracování složitých dotykové ovládání, obě, můžete použít transformace v ve Skiasharpu což je ukázán v tomto článku a dalších.

![](matrix-images/matrixtransformexample.png "Rastrový obrázek podroben afinní transformace")

Aktuální transformační matice u `SKCanvas` je k dispozici v každém okamžiku díky přístupu jen pro čtení [ `TotalMatrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.TotalMatrix/) vlastnost. Můžete nastavit nové transformační matice s použitím [ `SetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.SetMatrix/p/SkiaSharp.SKMatrix/) metody kde můžete obnovit tento transformační matice výchozí hodnoty voláním [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix/).

Jediná Další `SKCanvas` člena, který pracuje přímo s transformační matice plátna je [ `Concat` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Concat/p/SkiaSharp.SKMatrix@/) který zřetězí dvě matice vynásobením je společně.

Maticové transformace výchozí je jednotkovou matici a skládá se z 1 v Úhlopříčný buňky a uživatele 0 všude, kde else:

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

Můžete vytvořit matici identity pomocí statické [ `SKMatrix.MakeIdentity` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeIdentity()/) metody:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

`SKMatrix` Výchozí konstruktor neprovede *není* vrátit jednotkovou matici. Vrátí matici s všechny buňky nastaven na hodnotu nula. Nepoužívejte `SKMatrix` konstruktoru jen pokud plánujete nastavit tyto buňky ručně.

Když ve Skiasharpu vykreslí grafický objekt, každý bod (x, y) je efektivně převedena na 1 3 matice s 1 ve třetím sloupci:

<pre>
| x  y  1 |
</pre>

Tato matice 1 3 představuje bod trojrozměrného s souřadnice nastavena na hodnotu 1. Proč dvojrozměrné matici po transformaci vyžaduje práce v trojrozměrném jsou matematické důvodů (prodiskutována později). Tato matice 1 3 jako představující bod v 3D souřadnicový systém, ale vždy v rovině 2D, kde Z rovná 1 si můžete představit.

Tato matice 1 3 se pak vynásobí transformační matice a výsledek je vykreslen na plátně bod:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

Pomocí násobení matic standardní převedený body jsou následující:

x! = x

y' = y

z' = 1

To je výchozí transformace.

Když `Translate` metoda je volána v `SKCanvas` objektu, `tx` a `ty` argumenty, které mají `Translate` metoda stane první dvě buňky ve třetím řádku transformační matice:

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

Násobení je teď následujícím způsobem:

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

Tady jsou vzorce transformace:

x! = x + tx

y' = y + ty

Škálování faktory mají výchozí hodnotu 1. Při volání `Scale` metoda na novém `SKCanvas` objektu, výsledná transformační matice neobsahuje `sx` a `sy` argumenty v Úhlopříčný buněk:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

Transformace vzorce jsou následující:

x! = sx. x

y' = sy. y

Maticové transformace po volání `Skew` obsahuje dva argumenty v buňkách matice vedle škálování faktory:

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

Vzorce transformace jsou:

x! = x + xSkew. y

y' = ySkew. x + y

Pro volání `RotateDegrees` nebo `RotateRadians` pro úhel α transformační matice vypadá takto:

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

Tady jsou vzorce transformace:

x! = cos(α). x - sin(α). y

y' = sin(α). x - cos(α). y

Když α 0 stupňů, je jednotkovou matici. 180stupňový rozsah s orientací po α transformační matice vypadá takto:

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

Otočení kolem osy 180 stupňů je ekvivalentní překlopení objektu vodorovně a svisle, která se také provádí nastavením měřítko – 1.

Všechny tyto typy transformací, které jsou klasifikovány jako *nastavená na affine* transformace. Afinní transformace zahrnují nikdy třetí sloupec matrice, který zůstane na výchozí hodnotu 0, 0 a 1. Tento článek [Non-Afinní transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md) popisuje neafinní transformace.

## <a name="matrix-multiplication"></a>Násobení matic

Jednu velkou výhodou s použití transformační matice se, že je možné získat kompozitní transformace násobení matic, což se často označuje v dokumentaci ve Skiasharpu jako *zřetězení*. Mnoho metod související transformace v `SKCanvas` k "předběžné zřetězení" nebo "pre-concat." To se vztahuje pracovního násobení, což je důležité, protože násobení matic není komutativní.

Například v dokumentaci [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) metoda uvádí, že se "Pre-concats aktuální matice s zadaný překladu" při dokumentaci pro [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) Metoda říká, že se "Pre-concats aktuální matice pomocí zadaného rozsahu."

To znamená, že transformace zadané ve volání metody, které je násobitel (levý operand) a aktuální transformační matice se násobenec (operand pravé strany).

Předpokládejme, že `Translate` je volána po něm `Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

`Scale` Transformace se násobí hodnotou `Translate` transformace pro matici kompozitní transformace:

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` může být volána před `Translate` tímto způsobem:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

V takovém případě je obrácený pořadí násobení a faktory měřítka efektivně se použijí pro překlad faktory:

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

Tady je `Scale` metodu s bodem otáčení:

```csharp
canvas.Scale(sx, sy, px, py);
```

Toto je shodné s následující volání přeložit a škálování:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Tři transformační matice se vynásobené v obráceném pořadí ze zobrazení metody v kódu:

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

### <a name="the-skmatrix-structure"></a>Struktura SKMatrix

`SKMatrix` Struktury definuje devět vlastností čtení/zápisu typu `float` odpovídající devět buňky ovládacího prvku transformační matice:

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` také definuje vlastnost s názvem [ `Values` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix.Values/) typu `float[]`. Tuto vlastnost lze použít k nastavení nebo získání devět hodnot v jednom kroku v pořadí `ScaleX`, `SkewX`, `TransX`, `SkewY`, `ScaleY`, `TransY`, `Persp0`, `Persp1`, a `Persp2`.

`Persp0`, `Persp1`, A `Persp2` buňky jsou popsány v následujícím článku [Non-Afinní transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md). Pokud tyto výchozí hodnoty 0, 0 a 1, transformací, která se násobí hodnotou souřadnice bodu takto:

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

x! = ScaleX. x + SkewX. y + TransX

y' = SkewX. x + ScaleY. y + TransY

z' = 1

Toto je úplný dvojrozměrné afinní transformace. Afinní transformace zachová paralelní řádky, což znamená, že obdélník se nikdy transformuje na nic jiného než se z něj rovnoběžník.

`SKMatrix` Struktury definuje několik statické metody pro vytvoření `SKMatrix` hodnoty. Tyto všechny návratové `SKMatrix` hodnoty:

- [`MakeTranslation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeTranslation/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/System.Single/System.Single/) s bodem otáčení
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/) pro úhel v radiánech
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/System.Single/System.Single/) pro úhel v radiánech s bodem otáčení
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/)
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/System.Single/System.Single/) s bodem otáčení
- [`MakeSkew`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeSkew/p/System.Single/System.Single/)

`SKMatrix` také definuje několik statické metody, které zřetězení dvou matice, což znamená, že je násobení. Tyto metody jsou pojmenovány `Concat`, `PostConcat`, a `PreConcat`, a existují dvě verze jednotlivých. Tyto metody mají bez návratové hodnoty; Místo toho, které odkazují existující `SKMatrix` hodnoty prostřednictvím `ref` argumenty. V následujícím příkladu `A`, `B`, a `R` (pro "výsledek") jsou všechny `SKMatrix` hodnoty.

Dva `Concat` metody jsou volány takto:

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

Tyto provedení násobení následující:

R = B × A

Jiné metody mít pouze dva parametry. První parametr je upravený a při návratu z volání metody, obsahuje součin dvou matice. Dva `PostConcat` metody jsou volány takto:

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

Tato volání provádět následující operace:

A = A × B

Dva `PreConcat` jsou podobné metody:

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

Tato volání provádět následující operace:

A = B × A

Verze těchto volání metod se všemi `ref` argumenty jsou efektivnější ve volání implementace základní, ale může být matoucí někomu čtení kódu a za předpokladu, že to cokoli pomocí `ref` argument je změnit metodu. Kromě toho je často vhodné předat argument, který je výsledkem jednoho z `Make` metody, například:

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

Tím se vytvoří následující matice:

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

Toto je vynásobené transformace translace transformace měřítka. V tomto konkrétním případě `SKMatrix` struktury poskytuje metodu s názvem zástupce [ `SetScaleTranslate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.SetScaleTranslate/p/System.Single/System.Single/System.Single/System.Single/):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

Toto je jeden z několika situace, kdy je bezpečné používat `SKMatrix` konstruktoru. `SetScaleTranslate` Metoda nastaví všechny buňky devět matice. Je také bezpečně používat `SKMatrix` konstruktor s statické `Rotate` a `RotateDegrees` metody:

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

Tyto metody provádět *není* zřetězit transformace rotace do existující transformace. Metody nastavte všechny buňky v matici. Jsou funkčně stejný jako `MakeRotation` a `MakeRotationDegrees` metody s tím rozdílem, že není instance `SKMatrix` hodnotu.

Předpokládejme, že máte `SKPath` objekt, který chcete zobrazit, ale chcete raději, aby měla poněkud liší orientace nebo jiné středový bod. Souřadnice této cestě můžete upravit pomocí volání [ `Transform` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Transform/p/SkiaSharp.SKMatrix/) metoda `SKPath` s `SKMatrix` argument. **Cesta transformace** stránce ukazuje, jak to provést. [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) Třídy odkazy `HendecagramPath` objekt v poli, ale používá jeho konstruktor k použití transformace na tuto cestu:

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

`HendecagramPath` Objekt má ve středisku (0, 0), a jedenáct body na hvězdičku rozšířit ven z tohoto centra 100 jednotkami ve všech směrech. To znamená, že cesta obsahuje kladné a záporné souřadnice. **Cesta transformace** stránky upřednostňuje pro práci s star třikrát jako velké a všechna kladné souřadnice. Kromě toho nechce jeden bod hvězdičku, aby se bod nahoru. Chce místo pro jeden bod hvězdy tak, aby odkazovala přímo dolů. (Protože hvězdičky má jedenáct body, nemůže mít oba.) Tento postup vyžaduje otáčení hvězdičky o 360 stupňů dělený 22.

Konstruktor sestavení `SKMatrix` objekt ze tří samostatných transformace pomocí `PostConcat` metodu s následujícím vzorem, kde A, B a C jsou instancemi `SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

Toto je řadě po sobě jdoucích součinů, výsledek je následující:

A × B × C

Po sobě jdoucích součinů vám pomůže porozumět tomu, co dělá každý transformace. Transformace měřítka zvýší velikost souřadnice cesta 3, takže souřadnice rozsahu – 300 až 300. Transformace rotace otočí star kolem původu. Transformace translace pak posune, je 300 pixelů pravým tlačítkem a dolů tak všechny souřadnice stát kladné.

Existují jiné sekvence, které vyvolávají stejné matice. Tady je jiný:

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

To otočí nejdříve cestě kolem středu a převede jej na pravé straně 100 pixelů a tak veškeré souřadnice jsou uváděny kladné. Barva hvězdičky se pak zvýšit velikost vzhledem k jeho nové levého horního rohu, což je bod (0, 0).

`PaintSurface` Obslužná rutina může jednoduše vykreslit tuto cestu:

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

Zobrazí se v levém horním rohu plátna:

[![](matrix-images/pathtransform-small.png "Trojitá snímek obrazovky stránky cesta transformace")](matrix-images/pathtransform-large.png#lightbox "Trojitá snímek obrazovky stránky cesta transformace")

Konstruktor tento program se týká matice do cesty následující volání:

```csharp
transformedPath.Transform(matrix);
```

Cesta nemá *není* zachovat tato matice jako vlastnost. Místo toho ji transformací, která se vztahuje na všechny souřadnice cesty. Pokud `Transform` je volána znovu, znovu je použita transformace a je jediný způsob, jak se můžete vrátit použitím jiného matice, který vrátí zpět transformaci. Naštěstí `SKMatrix` definuje strukturu [ `TryInverse` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.TryInvert/p/SkiaSharp.SKMatrix/) metodu, která získá matici, která obrací daného matice:

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

Je volána metoda `TryInverse` protože ne všechny matice jsou inverzi, ale bez inverzi matice není pravděpodobně budou používat pro transformaci grafiky.

Můžete také použít transformační matice do `SKPoint` hodnoty, pole bodů `SKRect`, nebo jenom jedno číslo v rámci programu. `SKMatrix` Struktura podporuje tyto operace s kolekcí z metod, které začínají slovem `Map`, jako je například tyto:

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

Pokud použijete tento poslední metodu, mějte na paměti, která `SKRect` struktura není schopný reprezentovat otočený obdélník. Metoda má smysl jenom `SKMatrix` hodnotu představující překladu a škálování.

### <a name="interactive-experimentation"></a>Interaktivní experimentování

Jedním ze způsobů, chcete-li získat představu afinní transformace je interaktivní přesun tři rohů rastrového obrázku na obrazovce a podívat se, jaké transformace výsledků. Toto je myšlenku za **zobrazit nastavená na Affine matici** stránky. Tato stránka vyžaduje dvě třídy, které se používají také v jiných ukázky:

[ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchPoint.cs) Třída zobrazí více průchody průsvitných kruh, který můžete přetahovat po obrazovce. `TouchPoint` vyžaduje, aby `SKCanvasView` nebo element, který je nadřazená `SKCanvasView` mít [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchEffect.cs) připojené. Nastavte `Capture` vlastnost `true`. V `TouchAction` obslužná rutina události, program musí volat `ProcessTouchEvent` metoda ve `TouchPoint` pro každou `TouchPoint` instance. Metoda vrátí `true` Pokud událost touch výsledkem bodu touch přesunutí. Navíc `PaintSurface` obslužná rutina musí volat `Paint` metoda v každém `TouchPoint` instance předáním `SKCanvas` objektu.

`TouchPoint` ukazuje společného tak, že vizuál SkiaSharp, lze zapouzdřit v samostatné třídě. Třídu můžete definovat vlastnosti pro zadání vlastnosti vizuálu, a metodu s názvem `Paint` s `SKCanvas` argument může mít za následek ho.

`Center` Vlastnost `TouchPoint` označuje umístění objektu. Tuto vlastnost lze nastavit k inicializaci umístění. změny vlastností, když uživatel přetáhne kruh kolem plátna.

**Zobrazit stránku nastavená na Affine matice** také vyžaduje [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/MatrixDisplay.cs) třídy. Tato třída zobrazí buňky ovládacího prvku `SKMatrix` objektu. Má dvě veřejné metody: `Measure` získat dimenze vykreslené matice a `Paint` ji zobrazíte. Obsahuje třídy `MatrixPaint` vlastnost typu `SKPaint` , který se dá nahradit jinou velikost písma a barvy.

[ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) vytvoří soubor `SKCanvasView` a připojí `TouchEffect`. [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) soubor kódu na pozadí vytvoří tři `TouchPoint` objektů a potom je nastaví na polohami pro tři rohů rastrový obrázek, který se načítá z vložený prostředek:

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
        {
            bitmap = SKBitmap.Decode(stream);
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

Nastavená na affine matice je jednoznačně definována tři body. Tři `TouchPoint` objekty odpovídají levého horního, pravého horního a levého dolního rohu rastrového obrázku. Vzhledem k tomu je nastavená na affine matice pouze dokáže transformace obdélník se z něj rovnoběžník, čtvrtou odvozené od ostatních tří. Konstruktor končí volání `ComputeMatrix`, která vypočítá buňky ovládacího prvku `SKMatrix` objekt z těchto tří bodů.

`TouchAction` Volání obslužné rutiny `ProcessTouchEvent` metoda jednotlivých `TouchPoint`. `scale` Hodnotu převede ze souřadnice Xamarin.Forms na pixelech:

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

Pokud existuje `TouchPoint` přesunula, potom volá metodu `ComputeMatrix` znovu a zruší platnost povrchu.

`ComputeMatrix` Metoda určí matice odvozené od těchto tří bodů. Matice volá `A` transformace jeden pixel Čtvereček obdélníku do rovnoběžník podle tři body při škálování transformací, která se nazývá `S` škáluje rastrového obrázku na čtvereček obdélník jeden pixel. Složený Matrix `S` × `A`:

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

        SKMatrix result = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

Nakonec `PaintSurface` metoda vykreslí rastrový obrázek podle této matici, matice zobrazí v dolní části obrazovky a vykreslí dotykovými body v tři rozích rastrového obrázku:

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

Na následující obrazovce iOS zobrazí rastrový obrázek při prvním načtení stránky, při další dvě obrazovky zobrazit po některé manipulace s:

[![](matrix-images/showaffinematrix-small.png "Trojitá snímek obrazovky stránky zobrazit nastavená na Affine matici")](matrix-images/showaffinematrix-large.png#lightbox "Trojitá snímek obrazovky stránky zobrazit nastavená na Affine matici")

I když to vypadá, jako kdyby dotykovými body přetahováním rohů rastrového obrázku, který je pouze dojem. Matice počítají na základě dotykovými body transformuje rastrového obrázku tak, aby rohy shoduje s dotykovými body.

Je přirozenější pro uživatele k přesunutí, změna velikosti a otočit rastrové obrázky nejsou přetažením rohy, ale s použitím jedním nebo dvěma prsty přímo na objekt, který chcete přetáhnout, ovládání stažením prstů a otočení. Je to popsané v dalším článku [Touch manipulace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md).

### <a name="the-reason-for-the-3-by-3-matrix"></a>Důvod matice 3 3

To lze očekávat, že systém dvojrozměrné grafiky by vyžadovaly pouze 2 2 transformační matice:

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

Tento postup funguje pro škálování, otočení a dokonce i zkosení, ale není schopen nejzákladnější transformací, která je překladu.

Problém je, že představuje matici 2 2 *lineární* transformace ve dvou dimenzích. Lineární transformace zachová některé základní aritmetické operace, ale jeden z důsledky je lineární transformace nikdy mění bod (0, 0). Lineární transformovat nemožný překladu.

V trojrozměrném lineární transformační matice vypadá například takto:

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

Buňky označené `SkewXY` znamená, že hodnota zkosí souřadnici X na základě hodnot Y; buňku `SkewXZ` znamená, že hodnota zkosí souřadnici X na základě hodnot Z; a hodnoty zkosení podobně jako pro ostatní `Skew` buňky.

Je možné omezit Tato matice 3D transformace na dvourozměrné roviny nastavením `SkewZX` a `SkewZY` na hodnotu 0, a `ScaleZ` 1:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

Pokud dvojrozměrné grafiky jsou vykreslovány vedle zcela v rovině v 3D prostoru, kde Z rovná 1, násobení transformace vypadá takto:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Všechno, co zůstane na dvourozměrné roviny, kde Z rovná 1, ale `SkewXZ` a `SkewYZ` buňky efektivně promyslete dvojrozměrné překladu.

To je, jak trojrozměrného lineární transformace slouží jako dvojrozměrné – a lineární transformace. (Obdobně, transformace v 3D grafiky jsou na základě matice 4 4.)

`SKMatrix` Struktury ve Skiasharpu definuje vlastnosti pro tento třetí řádek:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

Hodnoty non-zero `Persp0` a `Persp1` způsobit transformace, které se přesunout objekty vypnout dvourozměrné roviny, kde Z rovná 1. Co se stane, když tyto objekty jsou přesunut na této roviny je popsané v článku na [Non-Afinní transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md).


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
