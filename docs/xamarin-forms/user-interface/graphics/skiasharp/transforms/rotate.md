---
title: Transformace rotace
description: Tento článek popisuje dopady a možnosti s transformace rotace ve Skiasharpu animace a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CBB3CD72-4377-4EA3-A768-0C4228229FC2
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: 1f34c64ca7c1bc9d0d0202f35602976364ab6075
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615246"
---
# <a name="the-rotate-transform"></a>Transformace rotace

_Prozkoumejte efekty a animace s transformace rotace ve Skiasharpu_

Pomocí transformace rotace ve Skiasharpu grafických objektů už se osvoboďte omezení zarovnání s vodorovné a svislé osy:

![](rotate-images/rotateexample.png "Text otáčet kolem System center")

Pro rotaci kolem bodu (0, 0), ve Skiasharpu podporuje obě grafický objekt [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/) metoda a [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/) metody:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

Kruh 360 stupňů je stejný jako 2π radiány tak, aby byl snadno převést mezi dvěma jednotkami. Použijte, podle toho, co je vhodné. Trigonometrické funkce v statické [ `Math` ](xref:System.Math) třídy používat jednotky radiánů.

Musí se obměnit po směru hodinových ručiček pro zvýšení úhlů. (Sice proti směru hodinových ručiček podle konvence otočení na systém souřadnic kartézský rotaci kolem je konzistentní s nimi souřadnice zvýšení přejít dolů.) Záporné úhly a úhly větší, než je povoleno 360 stupňů.

Transformace vzorce pro rotaci jsou složitější než u přeložit a škálování. Pro úhel α vzorce transformace jsou:

x! = x•cos(α) – y•sin(α)   

y' = x•sin(α) + y•cos(α)

**Základní otočit** stránce ukazuje `RotateDegrees` metody. [ `BasicRotate.xaml.cs` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) Souboru zobrazí text s jeho základní zarovnání na střed na stránce a na základě otočí `Slider` s celou řadou – 360 do 360. Tady je odpovídající část `PaintSurface` obslužné rutiny:

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

Protože otočení tkví v levém horním rohu plátna pro většinu úhly nastavit v rámci tohoto programu, je tento text otočen mimo obrazovku:

[![](rotate-images/basicrotate-small.png "Trojitá snímek obrazovky stránky základní otočit")](rotate-images/basicrotate-large.png#lightbox "Trojitá snímek obrazovky stránky základní otočení")

Velmi často budete chtít otočit něco zaměřená na bod zadaný pivot pomocí těchto verzích [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/System.Single/System.Single/) a [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/System.Single/System.Single/) metody:

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**Zarovnaný na střed otáčení** stránka je stejně jako **základní otočit** s tím rozdílem, že rozšířenou verzi `RotateDegrees` slouží k nastavení střed otáčení do stejného bodu používá umístění textu:

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

Nyní text otočí kolem bodu k umístění text, který je vodorovné center směrného plánu textu:

[![](rotate-images/centeredrotate-small.png "Trojitá snímek obrazovky stránky na střed otáčení")](rotate-images/centeredrotate-large.png#lightbox "Trojitá snímek obrazovky stránky na střed otáčení")

Stejně jako u zaměřena na verzi `Scale` metody, zaměřena na verzi `RotateDegrees` volání je zástupce:

```csharp
RotateDegrees (degrees, px, py);
```

To je ekvivalentní následujícímu zápisu:

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

Dozvíte se, že v některých případech můžete kombinovat `Translate` volání s `Rotate` volání. Tady jsou například `RotateDegrees` a `DrawText` volání **zarovnaný na střed otáčení** stránce;

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`RotateDegrees` Volání je ekvivalentní ke dvěma `Translate` volání a jiných střed `RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText` Je ekvivalentní volání k zobrazení textu v konkrétních místech `Translate` volání pro danou lokaci, za nímž následuje `DrawText` okamžiku (0, 0):

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

Dvě po sobě jdoucích `Translate` volání mezi sebou zrušit navýšení kapacity:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

Dva transformační soubory koncepčně, se použijí v pořadí, než jak se zobrazují v kódu. `DrawText` Volání zobrazí text v levém horním rohu plátna. `RotateDegrees` Volání otočí textu relativní k levého horního rohu. Pak bude `Translate` volání přesune text na střed plátna.

Jsou obvykle několik způsobů, jak kombinovat otočení a překladu. **Otočený Text** stránka vytvoří následující výstup:

[![](rotate-images/rotatedtext-small.png "Trojitá snímek obrazovky stránky otočený Text")](rotate-images/rotatedtext-large.png#lightbox "Trojitá snímek obrazovky stránky otočený Text")

Tady je `PaintSurface` obslužná rutina [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) třídy:

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

`xCenter` a `yCenter` hodnoty označují střed plátna. `yText` Hodnota je trochu posun od. To znamená souřadnici Y potřebné k umístění text tak, aby skutečně svisle na střed na stránce. `for` Smyčky pak nastaví otočení zarovnaný na střed na střed plátna. Otočení je dokupuje se násobek 30 stupňů. Text je vykreslen `yText` hodnotu. Počet prázdných hodnot před slovo "OTOČIT" v `text` hodnota byla určena empirických navázat připojení mezi těmito 12 textové řetězce se zdají být dodecagon.

Jedním ze způsobů pro zjednodušení tento kód je se zvýší pokaždé, když pomocí smyčky po úhel otočení ve stupních 30 `DrawText` volání. Tím se eliminuje nutnost volání `Save` a `Restore`. Všimněte si, že `degrees` proměnná se už používá v těle `for` blok:

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

Je také možné používat jednoduchá forma `RotateDegrees` podle zahájením literálu smyčky voláním `Translate` všechno přesunout do střed plátna:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

Upravené `yText` výpočtu už zahrnuje `yCenter`. Nyní `DrawText` volání vystředí text svisle v horní části plátna.

Protože transformace jsou použity koncepčně než jak se zobrazují v kódu, je možné začneme víc globálních transformací, za nímž následuje další místní transformace. Často je to nejjednodušší způsob, jak kombinovat otočení a překladu.

Předpokládejme například, že chcete kreslit grafický objekt, který otáčí kolem středu podobně jako globálním otáčení na ose. Ale také chcete tento objekt točí kolem středu obrazovky, podobně jako globálním obkroužení kolem slunce.

Můžete to provést umístěním objektů v levém horním rohu plátna a následným použitím animace otočení kolem tohoto rohu. V dalším kroku převede objekt vodorovně jako kruhová protokolu radius. Teď použijte druhý animovaný otočení, také kolem počátku. Tím je objekt točí kolem rohu. Nyní se převede na střed plátna.

Tady je `PaintSurface` obslužná rutina, která obsahuje následující transformace volání v obráceném pořadí:

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

`revolveDegrees` a `rotateDegrees` pole jsou animovat. Tento program využívá techniku jiné animace založené na Xamarin.Forms `Animation` třídy. (Tato třída je popsána v [kapitoly 22 *vytváření mobilních aplikací pomocí Xamarin.Forms*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)) `OnAppearing` přepsání vytvoří dva `Animation` objekty pomocí metod zpětného volání a pak zavolá `Commit` na nich na dobu animace:

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

První `Animation` objekt animuje `revolveDegrees` od 0 do 360 stupňů více než 10 sekund. Pro druhou kolekci animuje `rotateDegrees` od 0 do 360 stupňů každou 1 sekundu a také zruší platnost plochu pro vygenerování další volání `PaintSurface` obslužné rutiny. `OnDisappearing` Přepsání zruší těchto dvou animace:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

**Bez zbytečných prvků analogové hodiny** programu (tedy volá se, protože atraktivnější analogové hodiny najdete v článku novější) používá otočení zakreslit značky minutu a hodina hodin a otočení do rukou. V programu kreslení pomocí libovolného souřadnicový systém založen na kruh, které jsou zaměřeny na bod (0, 0) s poloměrem 100 hodin. Otočení a změny používá, tím rozbalíte a center této kruhu na stránce.

`Translate` a `Scale` volání platí globálně pro clock, takže to jsou první těch, která se má volat po inicializaci `SKPaint` objekty:

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

Nakonec `PaintSurface` obslužná rutina získá aktuální čas a vypočítá otočení stupňů za hodinu, minutu a druhá hands. Každý ručně vykreslením na pozici, 12:00, tak, aby úhel otočení je relativní vzhledem ke, který:

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

Hodin je určitě funkční, i když jsou do rukou místo hrubého:

[![](rotate-images/uglyanalogclock-small.png "Trojitá snímek obrazovky stránky bez zbytečných prvků textu hodiny obdobu jmenovek")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
