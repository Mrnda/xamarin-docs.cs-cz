---
title: "Otočit transformace"
description: "Prozkoumat efekty a animací možné pomocí SkiaSharp rotační transformace"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: c87f9a561ac2f7a8c3da1c1e4ab839431073fcb9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="the-rotate-transform"></a>Otočit transformace

_Prozkoumat efekty a animací možné pomocí SkiaSharp rotační transformace_

U otáčení transformace SkiaSharp grafických objektů rozdělit volné omezení zarovnání s vodorovného a svislého osy:

![](rotate-images/rotateexample.png "Text otočen kolem center")

Pro výměnu grafického objektu kolem bodu (0, 0), SkiaSharp podporuje obě [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/) metoda a [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/) metoda:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

Kruh 360 stupňů je stejný jako 2π radiánech, takže můžete snadno pro převod mezi dvě jednotky. Použijte, podle toho, co je vhodné. Trigonometrické funkce v statických [ `Math` ](https://developer.xamarin.com/api/type/System.Math/) třída použít jednotky radiánech.

Otočení je po směru hodinových ručiček pro zvýšení úhly. (I když otočení na kartézský souřadnicový systém proti směru hodinových ručiček podle konvence, po směru hodinových ručiček kolem je konzistentní s Souřadnice Y zvýšení probíhající dolů.) Záporné úhly a úhly větší, než je povoleno 360 stupňů.

Transformace vzorce pro otočení jsou složitější než ty, které pro přeložit a škálovatelnost. Pro úhel α jsou transformace vzorce:

x: = x•cos(α) – y•sin(α)   

y' = x•sin(α) + y•cos(α)

**Základní otočit** stránky ukazuje `RotateDegrees` metoda. [ `BasicRotate.xaml.cs` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) Soubor zobrazuje text na střed stránky směrného a na základě otočí `Slider` s rozsahem – 360 do 360. Tady je příslušné části `PaintSurface` obslužné rutiny:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Protože otočení je zaměřená na levém horním rohu plátna pro většinu úhly nastaven v tomto programu Tento text otočen z obrazovky:

[![](rotate-images/basicrotate-small.png "Trojitá snímek obrazovky stránky základní otočit")](rotate-images/basicrotate-large.png "Trojitá snímek obrazovky otočit základní stránky")

Velmi často budete chtít otočit něco zaměřená na bod zadaný pivot používat tyto verze [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/System.Single/System.Single/) a [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/System.Single/System.Single/) metody:

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**Zarovnaný na střed otočit** stránka je podobně jako **základní otočit** s tím rozdílem, že rozšířené verze `RotateDegrees` se používá k nastavení střed otáčení do stejného bodu k umístění text:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Nyní text otočí kolem bodu k umístění text, který je vodorovném centru směrného plánu text:

[![](rotate-images/centeredrotate-small.png "Trojitá snímek obrazovky stránky zarovnaný na střed otočit")](rotate-images/centeredrotate-large.png "Trojitá snímek obrazovky stránky zarovnaný na střed otočit")

Stejně jako u zarovnaný verzi `Scale` metoda, na střed verzi `RotateDegrees` volání je zástupce:

```csharp
RotateDegrees (degrees, px, py);
```

Jde o ekvivalent takto:

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

Dozvíte se, že v některých případech můžete kombinovat `Translate` volá s `Rotate` volání. Tady jsou například `RotateDegrees` a `DrawText` zavolá **zarovnaný na střed otočit** stránky;

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`RotateDegrees` Volání je ekvivalentní ke dvěma `Translate` volání a jiných-zarovnaný na střed `RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText` Je ekvivalentní volání k zobrazení textu v konkrétních místech `Translate` volání pro danou lokalitu, za nímž následuje `DrawText` v bodě (0, 0):

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

Dvě po sobě jdoucích `Translate` volání navzájem zruší:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

Koncepčně dva soubory použije v pořadí, než jak se zobrazují v kódu. `DrawText` Volání zobrazí text v levém horním rohu na plátno. `RotateDegrees` Volání otočí tento text relativně k levého horního rohu. Pak se `Translate` volání přesune text na střed plátna.

Obvykle tam existuje několik způsobů, jak kombinovat otočení a překlad. **Text otočen** stránky vytvoří následující zobrazení:

[![](rotate-images/rotatedtext-small.png "Trojitá snímek obrazovky stránky Text otočen")](rotate-images/rotatedtext-large.png "Trojitá snímek obrazovky stránky Text otočen")

Tady je `PaintSurface` obslužnou rutinu [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) třídy:

```csharp
static readonly string text = "    ROTATE";
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 72
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float yText = yCenter - textBounds.Height / 2 - textBounds.Top;

        for (int degrees = 0; degrees < 360; degrees += 30)
        {
            canvas.Save();
            canvas.RotateDegrees(degrees, xCenter, yCenter);
            canvas.DrawText(text, xCenter, yText, textPaint);
            canvas.Restore();
        }
    }
}

```

`xCenter` a `yCenter` hodnoty stanovují na střed plátna. `yText` Hodnota je trochu posun od. To znamená souřadnici Y potřebné k umístění text tak, aby skutečně svisle na střed na stránce. `for` Smyčky nastaví rotaci kolem zarovnaný na střed na střed plátna. Otočení je v přírůstcích po 30 stupňů. Text je vykreslen pomocí `yText` hodnotu. Počet prázdných hodnot před slovo "OTOČIT" v `text` hodnota byla určena empirically pro připojení mezi tyto 12 textové řetězce, zobrazí jako dodecagon.

Jedním ze způsobů pro zjednodušení tento kód je vyšší, úhel otočení o 30 stupňů prostřednictvím smyčky po `DrawText` volání. Tím se eliminuje potřeba pro volání `Save` a `Restore`. Všimněte si, že `degrees` proměnná se už používá v rámci textu `for` bloku:

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

Je také možné použít jednoduchý způsob `RotateDegrees` podle zahájením literálu smyčky pomocí volání `Translate` přesunout vše na střed plátna:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

Upravenou `yText` výpočtu již nezahrnují `yCenter`. Nyní `DrawText` volání centra textu ve svislém směru v horní části plátna.

Protože transformace jsou použity koncepčně opačné jak se zobrazují v kódu, je možné mít na začátku globálnější transformací, následuje další místní transformace. To je často nejjednodušší způsob, jak kombinovat otočení a překlad.

Předpokládejme například, že chcete kreslení grafického objektu, který otočí kolem jeho center podobně jako planetu, otáčení na ose. Můžete ale také chcete tento objekt základem center obrazovky podobně jako planetu obkroužení kolem sun.

To provedete tak umístění objektu v levém horním rohu na plátno a potom pomocí animace pro rotaci kolem tohoto rohu. V dalším kroku převede objekt vodorovně jako kruhová protokolu radius. Nyní budou vztahovat rotaci kolem počátku také druhý animovaný. Díky tomu objekt základem rohu. Nyní se převede na střed plátna.

Tady je `PaintSurface` obslužná rutina, která obsahuje tyto transformace volání v obráceném pořadí:

```csharp
float revolveDegrees, rotateDegrees;
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Red
    })
    {
        // Translate to center of canvas
        canvas.Translate(info.Width / 2, info.Height / 2);

        // Rotate around center of canvas
        canvas.RotateDegrees(revolveDegrees);

        // Translate horizontally
        float radius = Math.Min(info.Width, info.Height) / 3;
        canvas.Translate(radius, 0);

        // Rotate around center of object
        canvas.RotateDegrees(rotateDegrees);

        // Draw a square
        canvas.DrawRect(new SKRect(-50, -50, 50, 50), fillPaint);
    }
}
```

`revolveDegrees` a `rotateDegrees` pole nedochází. Tento program používá jiný animace postup podle platformě Xamarin.Forms `Animation` třídy. (Tato třída je popsaná v [kapitoly 22 *vytváření mobilních aplikací s Xamarin.Forms*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)) `OnAppearing` přepsání vytvoří dvě `Animation` objekty s metody zpětného volání a pak zavolá `Commit` na nich doby trvání animace:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    new Animation((value) => revolveDegrees = 360 * (float)value).
        Commit(this, "revolveAnimation", length: 10000, repeat: () => true);

    new Animation((value) =>
    {
        rotateDegrees = 360 * (float)value;
        canvasView.InvalidateSurface();
    }).Commit(this, "rotateAnimation", length: 1000, repeat: () => true);
}
```

První `Animation` objekt animuje `revolveDegrees` od 0 do 360 stupňů více než 10 sekund. Druhý animuje `rotateDegrees` od 0 do 360 stupňů každou 1 sekundu a také by způsobila neplatnost prostor pro generování jiném volání `PaintSurface` obslužné rutiny. `OnDisappearing` Přepsání zruší tyto dvě animací:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

**Ugly analogovým hodiny** programu (nazývané, protože více atraktivní analogovým hodiny budou popsané v článku na novější) používá otočení zakreslit značky minutu a hodinu hodin a Otočit do rukou. Program nevykresluje hodiny pomocí libovolné souřadnicový systém založené na kruh, který je umístěn na střed v bodě (0, 0) o poloměru 100. Překlad a škálování používá pro rozšíření a center tohoto kroužku na stránce.

`Translate` a `Scale` volání platí globálně pro hodiny, takže těch, které jsou první ty, která se má volat po inicializaci `SKPaint` objekty:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    using (SKPaint fillPaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.Color = SKColors.Black;
        strokePaint.StrokeCap = SKStrokeCap.Round;

        fillPaint.Style = SKPaintStyle.Fill;
        fillPaint.Color = SKColors.Gray;

        // Transform for 100-radius circle centered at origin
        canvas.Translate(info.Width / 2f, info.Height / 2f);
        canvas.Scale(Math.Min(info.Width / 200f, info.Height / 200f));
        ...
    }
}

```csharp
There are 60 marks of two different sizes that must be drawn in a circle around the clock. The `DrawCircle` call draws that circle at the point (0, –90), which relative to the center of the clock corresponds to 12:00. The `RotateDegrees` call increments the rotation angle by 6 degrees after every tick mark. The `angle` variable is used solely to determine if a large circle or a small circle is drawn:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        // Hour and minute marks
        for (int angle = 0; angle < 360; angle += 6)
        {
            canvas.DrawCircle(0, -90, angle % 30 == 0 ? 4 : 2, fillPaint);
            canvas.RotateDegrees(6);
        }
    ...
    }
}
```

Nakonec `PaintSurface` obslužná rutina získá aktuální čas a vypočítá otočení stupňů za hodinu, minutu a druhý rukou. Každý dlaně se vykresluje v pozici 12:00 tak, aby úhel otočení je relativní vzhledem ke který:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        DateTime dateTime = DateTime.Now;

        // Hour hand
        strokePaint.StrokeWidth = 20;
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawLine(0, 0, 0, -50, strokePaint);
        canvas.Restore();

        // Minute hand
        strokePaint.StrokeWidth = 10;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawLine(0, 0, 0, -70, strokePaint);
        canvas.Restore();

        // Second hand
        strokePaint.StrokeWidth = 2;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Second);
        canvas.DrawLine(0, 10, 0, -80, strokePaint);
        canvas.Restore();
    }
}
```

Hodiny je určitě funkční, i když jsou do rukou místo hrubých:

[![](rotate-images/uglyanalogclock-small.png "Trojitá snímek obrazovky stránky Ugly analogovým hodiny textu")](rotate-images/uglyanalogclock-large.png "Triple screenshot of the Ugly Analog page")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
