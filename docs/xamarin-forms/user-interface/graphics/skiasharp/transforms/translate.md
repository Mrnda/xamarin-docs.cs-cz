---
title: Transformace Translace
description: Tento článek examiens shift grafiky ve Skiasharpu v aplikacích Xamarin.Forms pomocí transformace translace a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 02361b5b2d00015ce168c075dc19522b6c04e446
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615441"
---
# <a name="the-translate-transform"></a>Transformace Translace

_Další informace o použití transformace translace posun grafiky ve Skiasharpu_

Se o nejjednodušší typ transformace SkiaSharp *přeložit* nebo *překlad* transformace. Tato transformace posune grafické objekty ve směru vodorovný a svislý. V tom smyslu překlad je nejvíce zbytečné transformace, protože lze obvykle dosažení stejného účinku jednoduchou změnou souřadnice, které používáte ve funkci výkresu. Při vykreslování cesty, ale všechny souřadnice jsou zapouzdřeny v cestě, proto je mnohem snadnější použití transformace translace posunout celou cestu.

Překlad je také užitečné pro animace a efekty jednoduchý text:

![](translate-images/translateexample.png "Stín textu, Ryté písmo a reliéf nezaregistrované")

[ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) Metoda `SKCanvas` má dva parametry, které způsobují následně vykresleného grafických objektů posunutí vodorovně a svisle:

```csharp
public void Translate (Single dx, Single dy)
```

Tyto argumenty mohou být záporná. Sekundy [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/SkiaSharp.SKPoint/) metoda kombinuje dvě překladu hodnoty v jednom `SKPoint` hodnotu:

```csharp
public void Translate (SKPoint point)
```

**Sbírají přeložit** stránku [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) ukázkový program ukazuje, že více volání z `Translate` metody jsou kumulativní. [ `AccumulatedTranslate` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) Třídy zobrazí 20 verze stejné obdélníku, každý z nich posun od předchozí obdélník tak akorát tak jejich roztáhnout po diagonální. Tady je `PaintSurface` obslužné rutiny události:

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

Po sobě jdoucích obdélníky skapat části stránky:

[![](translate-images/accumulatedtranslate-small.png "Trojitá snímek obrazovky stránky sbírají přeložit")](translate-images/accumulatedtranslate-large.png#lightbox "Trojitá snímek obrazovky stránky sbírají překlad")

Pokud součet posunutí faktory jsou `dx` a `dy`, a je bod zadáte výkresu – funkce (`x`, `y`), pak grafický objekt je vykreslen v místě (`x'`, `y'`), kde:

x! = x + dx

y' = y + dy

Toto jsou známé jako *transformace vzorce* pro překlad. Výchozí hodnoty `dx` a `dy` nový `SKCanvas` mají hodnotu 0.

Je běžné použití transformace translace pro efekty stínování a podobné techniky, jako **přeložit textových efektů** demonstruje stránky. Tady je odpovídající část `PaintSurface` obslužné rutiny v [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) třídy:

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

V každé tři příklady `Translate` se volá pro zobrazování textu na posun od daného podle umístění `x` a `y` proměnné. Text se pak zobrazí znovu v jiné barvy se žádný překlad vliv:

[![](translate-images/translatetexteffects-small.png "Trojitá snímek obrazovky stránky přeložit textových efektů")](translate-images/translatetexteffects-large.png#lightbox "Trojitá snímek obrazovky stránky přeložit textových efektů")

Všechny tři příklady ukazuje jiný způsob negace `Translate` volání:

První příklad jednoduše volá `Translate` znovu, ale s záporné hodnoty. Vzhledem k tomu, `Translate` volání jsou kumulativní, celkový počet překlad toto pořadí volání jednoduše obnoví na výchozí hodnoty nula.

Druhý příklad volá [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix()/). To způsobí, že všechny transformace se vraťte do výchozího stavu.

Třetí příklad uloží stav aplikace `SKCanvas` objektu voláním [ `Save` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Save()/) a potom obnoví stav voláním [ `Restore` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Restore/). Toto je nejčastěji univerzální způsob, jak pracovat s transformací pro řadu kreslicí operace. Tyto `Save` a `Restore` volá funkci jako zásobník: můžete volat `Save` více času a poté zavolejte `Restore` ve změnit pořadí se vraťte do předchozích stavů. `Save` Metoda vrátí celé číslo, a můžete předat tento celé číslo k [ `RestoreToCount` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RestoreToCount/) efektivně volat `Restore` více než jednou. [ `SaveCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.SaveCount/) Vlastnost vrací počet stavů, které jsou aktuálně uloženy do zásobníku.

Však není nutné se starat o transformacích přenesou z jednoho volání `PaintSurface` obslužné rutiny na další. Každé nové volání na `PaintSurface` přináší čerstvého `SKCanvas` objekt s výchozí transformace.

Další běžné použití `Translate` transformace je pro vykreslování vizuální objekt, který byl původně vytvořen pomocí souřadnic, které jsou vhodné pro kreslení. Můžete například chtít určuje souřadnice pro analogové hodiny se System center v okamžiku (0, 0). Potom můžete transformací zobrazíte ho do požadovaného. To je patrné [**Hendecagram pole**] stránky. [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs) Třídy začíná tím, že vytvoříte `SKPath` objekt pro odkazovala 11 hvězdičku. `HendecagramPath` Objekt je definovaný jako veřejné, statické a jen pro čtení tak, aby byla přístupná z jiných aplikací ukázku. Vytvoří se ve statickém konstruktoru:

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

Pokud středu hvězdy je bod (0, 0), jsou všechny body na hvězdičku na kolečko okolo tohoto bodu. Každý bod je kombinace hodnot sinus a kosinus úhlu, který zvýší o 5/11ths 360 stupňů. (Je také možné vytvořit hvězdičku ukazuje 11 zvýšením úhel 2/11, 3/11ths nebo 4/11 kruhu.) Poloměr této kruhu je nastaven jako 100.

Pokud tato cesta je vykreslen bez jakékoli transformací, centru budou umístěné v levém horním rohu `SKCanvas`a pouze čtvrtletí jeho se nebude zobrazovat. `PaintSurface` Obslužná rutina `HendecagramPage` místo toho používá `Translate` na dlaždici na plátno s využitím více kopií hvězdičky, každý z nich náhodně vybarvenými:

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

[![](translate-images/hendecagramarray-small.png "Trojitá snímek obrazovky stránky Hendecagram pole")](translate-images/hendecagramarray-large.png#lightbox "Trojitá snímek obrazovky stránky Hendecagram pole")

Animace často zahrnuje transformací. **Hendecagram animace** stránky stojí star ukazuje 11 v kruhu. [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) Třídy začíná některá pole a přepsání `OnAppearing` a `OnDisappearing` metod ke spuštění a zastavení časovače Xamarin.Forms:

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

`angle` Pole je animovaný od 0 do 360 stupňů každých 5 sekund. `PaintSurface` Obslužná rutina používá `angle` vlastnost dvěma způsoby: k určení odstín barvy v `SKColor.FromHsl` metoda a jako argument `Math.Sin` a `Math.Cos` metody k řízení umístění na hvězdičku:

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

`PaintSurface` Volání obslužné rutiny `Translate` metoda dvakrát, nejprve pro převod na střed plátna a pak pro převod obvod kruhu soustředí na (0, 0). Poloměr kruhu je nastaven tak, aby byl co nejblíže přitom zachovat hvězdičky v rámci stránky:

[![](translate-images/hendecagramanimation-small.png "Trojitá snímek obrazovky stránky Hendecagram animace")](translate-images/hendecagramanimation-large.png#lightbox "Trojitá snímek obrazovky stránky Hendecagram animace")

Všimněte si, že na hvězdičku udržuje stejné orientace jako především zajišťuje kolem středu stránky. Objekt se nedá vůbec otočit. To je úloha pro transformace otočení.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
