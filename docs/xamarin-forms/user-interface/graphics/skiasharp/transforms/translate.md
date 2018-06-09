---
title: Transformace přeložit
description: Článek examiens použití transformace přeložit k posunutí SkiaSharp grafiky v aplikacích Xamarin.Forms a to ukazuje s ukázkový kód.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: dbc7ffe5c3828876579ba72a387c86d8221c1641
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244819"
---
# <a name="the-translate-transform"></a>Transformace přeložit

_Další informace o použití transformace přeložit se posunou SkiaSharp grafiky_

Nejjednodušší typ transformace v SkiaSharp je *převede* nebo *překlad* transformace. Tato transformace posune grafické objekty v vodorovného a svislého směru. V tom smyslu překlad je nejvíce nepotřebné transformace, protože obvykle dosáhnete stejného efektu jednoduše změna souřadnice, které používáte ve funkci kreslení. Při vykreslování cestu, ale všechny souřadnice jsou zapouzdřený v cestě, proto je mnohem snazší přeložit transformace se posunou celou cestu.

Překlad je také užitečné pro animace a jednoduchý text důsledky:

![](translate-images/translateexample.png "Stín textu zobrazovat, Ryté písmo a reliéfu s překlad")

[ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) Metoda v `SKCanvas` má dva parametry, které způsobí následně vykresleného grafických objektů posunutí vodorovně a svisle:

```csharp
public void Translate (Single dx, Single dy)
```

Tyto argumenty může mít zápornou hodnotu. Druhý [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/SkiaSharp.SKPoint/) metoda kombinuje dvě překlad hodnot do jedné `SKPoint` hodnotu:

```csharp
public void Translate (SKPoint point)
```

**Nahromadění převede** stránky [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) ukázka programu ukazuje, že více volá z `Translate` metoda jsou kumulativní. [ `AccumulatedTranslate` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) Třída zobrazí 20 verze stejné obdélníku, každé z nich posun z předchozí rámeček právě dostatek tak jejich funkce stretch společně diagonálních. Tady je `PaintSurface` obslužné rutiny události:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Color = SKColors.Black;
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = 3;

        int rectangleCount = 20;
        SKRect rect = new SKRect(0, 0, 250, 250);
        float xTranslate = (info.Width - rect.Width) / (rectangleCount - 1);
        float yTranslate = (info.Height - rect.Height) / (rectangleCount - 1);

        for (int i = 0; i < rectangleCount; i++)
        {
            canvas.DrawRect(rect, strokePaint);
            canvas.Translate(xTranslate, yTranslate);
        }
    }
}
```

Následných obdélníků skapat dolní části stránky:

[![](translate-images/accumulatedtranslate-small.png "Trojitá snímek obrazovky stránky nahromadění převede")](translate-images/accumulatedtranslate-large.png#lightbox "Trojitá snímek obrazovky stránky nahromadění převede")

Pokud jsou faktory Akumulovaná překlad `dx` a `dy`, a zadáte v kreslení funkce bod je (`x`, `y`), pak vykreslením grafického objektu v bodě (`x'`, `y'`), kde:

x: = x + DirectX

y' = y + dy

Toto jsou známé jako *transformace vzorce* pro překlad. Výchozí hodnoty `dx` a `dy` pro novou `SKCanvas` mají hodnotu 0.

Je běžné pro použití transformace přeložit pro stínové efekty a podobné techniky, jako **převede Text důsledky** ukazuje stránky. Tady je příslušné části `PaintSurface` obslužné rutiny v [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) třídy:

```csharp
float textSize = 150;

using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = textSize;
    textPaint.FakeBoldText = true;

    float x = 10;
    float y = textSize;

    // Shadow
    canvas.Translate(10, 10);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("SHADOW", x, y, textPaint);
    canvas.Translate(-10, -10);
    textPaint.Color = SKColors.Pink;
    canvas.DrawText("SHADOW", x, y, textPaint);

    y += 2 * textSize;

    // Engrave
    canvas.Translate(-5, -5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("ENGRAVE", x, y, textPaint);
    canvas.ResetMatrix();
    textPaint.Color = SKColors.White;
    canvas.DrawText("ENGRAVE", x, y, textPaint);

    y += 2 * textSize;

    // Emboss
    canvas.Save();
    canvas.Translate(5, 5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("EMBOSS", x, y, textPaint);
    canvas.Restore();
    textPaint.Color = SKColors.White;
    canvas.DrawText("EMBOSS", x, y, textPaint);
}
```

V každé tři příklady `Translate` je volána pro zobrazení textu k posunutí z umístění, které poskytují `x` a `y` proměnné. Text se potom zobrazí znovu v jiné barvy s neplatí překlad:

[![](translate-images/translatetexteffects-small.png "Trojitá snímek obrazovky stránky převede Text důsledky")](translate-images/translatetexteffects-large.png#lightbox "Trojitá snímek obrazovky stránky převede Text efekty")

Každý tři příklad ukazuje jiný způsob negace `Translate` volání:

V prvním příkladu jednoduše volá `Translate` znovu, ale s záporné hodnoty. Protože `Translate` volání jsou kumulativní, celkový počet překlad toto pořadí volání jednoduše obnoví na výchozí hodnoty nula.

Druhé volání příklad [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix()/). To způsobí, že všechny transformace se vraťte do výchozího stavu.

Třetí příklad uloží stav v z `SKCanvas` objekt s volání [ `Save` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Save()/) a následnému obnovení stavu se volání [ `Restore` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Restore/). To je nejvíce univerzální způsob k manipulaci s transformací pro řadu kreslení operace. Tyto `Save` a `Restore` volá funkci jako zásobníky: můžete volat `Save` více času a pak volání `Restore` v obrátíte pořadí se vrátíte do předchozího stavu. `Save` Metoda vrátí celé číslo, a může předat tuto celého [ `RestoreToCount` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RestoreToCount/) efektivně volat `Restore` vícekrát. [ `SaveCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.SaveCount/) Vlastnost vrací počet stavů, které jsou aktuálně uloženy v zásobníku.

Však nemusíte si dělat starosti o transformací přenesou z jednoho volání `PaintSurface` obslužné rutiny na další. Každé nové volání na `PaintSurface` přináší čerstvou `SKCanvas` objekt s výchozí transformace.

Další běžné použití `Translate` transformace je pro vykreslování vizuální objekt, který byl původně vytvořen pomocí souřadnic, které jsou vhodné pro kreslení. Můžete například chtít zadat souřadnice analogovým hodiny center v bodě (0, 0). Potom můžete transformace můžete ho zobrazit požadované místo. Tento postup je znázorněn v [**Hendecagram pole**] stránky. [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs) Třída začíná vytvořením `SKPath` objekt pro odkazoval 11 hvězdičky. `HendecagramPath` Objektu je definována jako veřejné statické a jen pro čtení tak, aby byla přístupná z jiných aplikací ukázka. Je vytvořen ve statického konstruktoru:

```csharp
public class HendecagramArrayPage : ContentPage
{
    ...
    public static readonly SKPath HendecagramPath;

    static HendecagramArrayPage()
    {
        // Create 11-pointed star
        HendecagramPath = new SKPath();
        for (int i = 0; i < 11; i++)
        {
            double angle = 5 * i * 2 * Math.PI / 11;
            SKPoint pt = new SKPoint(100 * (float)Math.Sin(angle),
                                    -100 * (float)Math.Cos(angle));
            if (i == 0)
            {
                HendecagramPath.MoveTo(pt);
            }
            else
            {
                HendecagramPath.LineTo(pt);
            }
        }
        HendecagramPath.Close();
    }
}
```

Pokud středu hvězdy je bod (0, 0), jsou všechny body hvězdy na kruh kolem tohoto bodu. Každý bod je kombinace hodnot sinus a kosinus úhlu, který zvyšuje úroveň 5/11ths 360 stupňů. (Je také možné vytvořit hvězdičkou odkazoval 11 zvýšením úhel ve 2/11, 3 nebo 11ths nebo 4/11 kruhu.) Hodnota úhlu tohoto kroužku nastavená na 100.

Pokud tato cesta je vykreslen bez jakékoli transformací, centru bude umístěna v levém horním rohu `SKCanvas`a pouze čtvrtletí ho budou viditelné. `PaintSurface` Obslužná rutina `HendecagramPage` místo toho používá `Translate` na dlaždici na plátno s více kopií hvězdy každé z nich náhodně barevné:

```csharp
public class HendecagramArrayPage : ContentPage
{
    Random random = new Random();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            for (int x = 100; x < info.Width + 100; x += 200)
                for (int y = 100; y < info.Height + 100; y += 200)
                {
                    // Set random color
                    byte[] bytes = new byte[3];
                    random.NextBytes(bytes);
                    paint.Color = new SKColor(bytes[0], bytes[1], bytes[2]);

                    // Display the hendecagram
                    canvas.Save();
                    canvas.Translate(x, y);
                    canvas.DrawPath(HendecagramPath, paint);
                    canvas.Restore();
                }
        }
    }
}

```

Tady je výsledek:

[![](translate-images/hendecagramarray-small.png "Trojitá snímek obrazovky stránky pole Hendecagram")](translate-images/hendecagramarray-large.png#lightbox "Trojitá snímek obrazovky stránky Hendecagram pole")

Animace často zahrnuje transformací. **Hendecagram animace** stránky přesune hvězdičky odkazoval 11 v kruh. [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) Třídy začíná některá pole a přepsání `OnAppearing` a `OnDisappearing` metody pro spuštění a zastavení Xamarin.Forms časovače:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    const double cycleTime = 5000;      // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float angle;

    public HendecagramAnimationPage()
    {
        Title = "Hedecagram Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            angle = (float)(360 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`angle` Pole je animovaný od 0 do 360 stupňů každých 5 sekund. `PaintSurface` Obslužná rutina používá `angle` vlastnost dvěma způsoby: k určení odstín barvy v `SKColor.FromHsl` metoda a jako argumenty `Math.Sin` a `Math.Cos` metody, které řídí hvězdy umístění:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.Translate(info.Width / 2, info.Height / 2);
        float radius = (float)Math.Min(info.Width, info.Height) / 2 - 100;

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColor.FromHsl(angle, 100, 50);

            float x = radius * (float)Math.Sin(Math.PI * angle / 180);
            float y = -radius * (float)Math.Cos(Math.PI * angle / 180);
            canvas.Translate(x, y);
            canvas.DrawPath(HendecagramPage.HendecagramPath, paint);
        }
    }
}
```

`PaintSurface` Volání obslužné rutiny `Translate` metodu dvakrát, nejprve převede na střed plátna a potom nepřeloží na obvodu kruhu soustředí na (0, 0). Radius kruhu je nastaven jako co největší přitom hvězdičkou v hranicích stránky:

[![](translate-images/hendecagramanimation-small.png "Trojitá snímek obrazovky stránky animace Hendecagram")](translate-images/hendecagramanimation-large.png#lightbox "Trojitá snímek obrazovky stránky Hendecagram animace")

Všimněte si, že hvězdičkou udržuje stejné orientaci, jako je zásadní kolem center stránky. Není otočit vůbec. To je úlohu pro rotační transformace.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
