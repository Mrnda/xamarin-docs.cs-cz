---
title: Transformace škálování
description: Zjistit transformace škálování SkiaSharp pro škálování objekty, které se různé velikosti
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: b4a36e15bd5db72ef113748282175c6d31a95966
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="the-scale-transform"></a>Transformace škálování

_Zjistit transformace škálování SkiaSharp pro škálování objekty, které se různé velikosti_

Protože jste viděli v [převede transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md) článku transformace přeložit můžete přesunout objekt grafické z jednoho umístění do druhého. Naproti tomu transformace škálování změní velikost grafického objektu:

![](scale-images/scaleexample.png "Vysoký slovo škálovat velikost")

Transformace škálování také často způsobí, že grafiky souřadnice přesunout, protože byly provedeny větší.

Dříve jste viděli dva vzorce transformace, které popisují důsledky překlad faktory `dx` a `dy`:

x: = x + DirectX

y' = y + dy

Škálování faktory `sx` a `sy` jsou multiplikativní místo doplňkové:

x: sx · = x

y' = sy · y

Výchozí hodnoty přeložit faktory jsou 0; výchozí hodnoty měřítka faktory jsou 1.

`SKCanvas` Třída definuje čtyři `Scale` metody. První [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/) metoda je pro případy, pokud chcete stejné vodorovného a svislého škálování zohlednit:

```csharp
public void Scale (Single s)
```

To se označuje jako *isotropic* škálování &mdash; škálování tedy stejné v obou směrech. Isotropic škálování zachová poměr stran objektu.

Druhý [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) metoda umožňuje určit různé hodnoty vodorovného a svislého škálování:

```csharp
public void Scale (Single sx, Single sy)
```

Výsledkem je *volba* škálování.
Třetí [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/SkiaSharp.SKPoint/) metoda kombinuje dvě škálování faktory v jediném `SKPoint` hodnotu:

```csharp
public void Scale (SKPoint size)
```

Čtvrtý `Scale` metoda najdete za chvíli.

**Základní škálování** stránky ukazuje `Scale` metoda. [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) XAML soubor obsahuje dva `Slider` prvky, které vám umožní vybrat vodorovného a svislého škálování faktory mezi 0 a 10. [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) souboru kódu na pozadí používá tyto hodnoty pro volání `Scale` před zobrazení zaoblený obdélník pouze tah přerušovanou čárou a přizpůsobí nějaký text v levé horní horním rohu na plátno:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] {  7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        canvas.Scale((float)xScaleSlider.Value,
                     (float)yScaleSlider.Value);

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);

        float margin = 10;
        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Může vás zajímat: jak škálování faktory ovlivňují hodnota vrácená z `MeasureText` metodu `SKPaint`? Odpověď na otázku: vůbec. `Scale` je metoda `SKCanvas`. Nic dělat s neovlivňuje `SKPaint` objektu, dokud tento objekt pro vykreslení něco na plátně.

Jak vidíte, vše, co vykreslovat po `Scale` volání zvyšuje úměrně:

[![](scale-images/basicscale-small.png "Trojitá snímek obrazovky stránky základní škálování")](scale-images/basicscale-large.png#lightbox "Trojitá snímek obrazovky stránky základní měřítka")

Text, šířka přerušovanou čáru, délka pomlčky v daného řádku zaokrouhlení rozích a 10 pixelů okraje mezi horní a levé hrany na plátno a Zaoblený obdélník platí všechny stejné škálování faktorů.

> [!IMPORTANT]
> Univerzální platformu Windows nevykresluje správně anisotropicly škálovat text.

Volba škálování příčiny šířku tahu se liší řádky zarovnán vodorovné nebo svislé osy. (To je také zřejmé z první bitové kopie na této stránce.) Pokud nechcete, aby šířku tahu, která má mít vliv faktory škálování, nastavte na hodnotu 0 a bude vždy jeden pixel široká bez ohledu na to `Scale` nastavení.

Škálování je relativní vzhledem ke levého horního rohu plátna. Může to být přesně co chcete použít, ale nemusí být. Předpokládejme, že chcete umístění textu a obdélníku někde jinde na plátně a vy chcete škálovat relativně k jeho center. V takovém případě můžete použít čtvrtou verzi [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/System.Single/System.Single/) metoda, která zahrnuje dva další parametry k určení středu škálování:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px` a `py` parametry definovat bod, který se někdy označuje jako *škálování center* , ale v SkiaSharp dokumentace se označuje jako *bodu otáčení*. Toto je bod relativně k levém horním rohu na plátno, který nemá vliv škálování. Všechny škálování nastane relativně k této center.

[ **Zarovnaný na střed škálování** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) stránka zobrazuje, jak to funguje. `PaintSurface` Obslužná rutina je podobná **základní škálování** programu vyjma toho, že `margin` hodnota je vypočítána na střed text ve vodorovném směru, což naznačuje, že program funguje nejlépe v režimu na výšku:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);
        float margin = (info.Width - textBounds.Width) / 2;

        float sx = (float)xScaleSlider.Value;
        float sy = (float)yScaleSlider.Value;
        float px = margin + textBounds.Width / 2;
        float py = margin + textBounds.Height / 2;

        canvas.Scale(sx, sy, px, py);

        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Je umístěný levém horním rohu obdélníku zaokrouhlené `margin` pixelů z nalevo od plátna a `margin` pixelů shora. Poslední dva argumenty, které mají `Scale` metoda jsou nastavena na tyto hodnoty a šířka a Výška textu, což je také šířka a výška zaokrouhlené rámečku. To znamená, že všechny škálování je relativní vzhledem ke středu obdélníku:

[![](scale-images/centeredscale-small.png "Trojitá snímek obrazovky stránky zarovnaný na střed škálování")](scale-images/centeredscale-large.png#lightbox "Trojitá snímek obrazovky stránky škálování zarovnaný na střed")

`Slider` Elementy v tento program mít řadu &ndash;10 až 10. Jak vidíte, záporné hodnoty Vertical škálování (například na Android obrazovky v centru) způsobit, že objekty kolem vodorovné osy, které procházejí středu škálování. Záporné hodnoty vodorovných škálování (například obrazovce UWP na pravé straně) způsobit, že objekty kolem svislé osy, které procházejí středu škálování.

Tato verze čtvrtý `Scale` metoda je ve skutečnosti zástupce. Můžete chtít zjistit, jak to funguje tak, že nahradíte `Scale` metoda v tento kód následujícím kódem:

```csharp
canvas.Translate(-px, -py);
```

Jedná se o negativy souřadnic bodu pivot.

Nyní spusťte program znovu. Uvidíte zapuštěno obdélníku a text tak, aby centru v levém horním rohu na plátno. Sotva najdete ho. Posuvníků nefungují samozřejmě, protože teď nemá vůbec škálování program.

Nyní přidat základní `Scale` volání (bez škálování center) *před* , `Translate` volání:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Pokud jste obeznámeni s toto cvičení v jiné že systémy programováním grafiky, domníváte se, že je nesprávný, ale není. Skia zpracovává volání následných transformace trochu jinak z co je znají.

S následných `Scale` a `Translate` volání, středu zaokrouhlené rámeček je stále v levém horním rohu, ale je možné nyní škálovat relativně k levého horního rohu na plátno, což je také středu zaoblený obdélník.

Teď, než který `Scale` volání přidat další `Translate` volání centrování hodnotami:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Tento krok přesune škálovat výsledek zpět na původní pozici. Tyto tři volání odpovídají:

```csharp
canvas.Scale(sx, sy, px, py);
```

Jednotlivé transformace jsou kombinovaných tak, aby celkový transformace vzorec:

 x: sx · = (x – px) + px

 y' = sy · (y – py) + py

Mějte na paměti, výchozí hodnoty `sx` a `sy` 1. Je snadné přimět sami, že tyto vzorce není transformovat bodem pivot (px, py). Zůstane ve stejném umístění relativně k na plátno.

Když zkombinujete `Translate` a `Scale` volání, záleží na pořadí. Pokud `Translate` dodává po `Scale`, překlad faktory jsou efektivně škálovat škálování faktory. Pokud `Translate` zaslána před `Scale`, nejsou škálovat faktory překlad. Tento proces bude poněkud jasnější (i když více matematickém) Pokud je zavedená předmět transformační matice.

`SKPath` Třída definuje jen pro čtení [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) vlastnost, která vrací `SKRect` definování rozsah souřadnice v cestě. Například, když `Bounds` vlastnost se získávají z cesty hendecagram vytvořili dříve, `Left` a `Top` vlastnosti obdélníku, jsou přibližně – 100, `Right` a `Bottom` vlastnosti přibližně 100 a `Width` a `Height` vlastnosti jsou přibližně 200. (Většina skutečnými hodnotami jsou malé méně, protože body hvězdiček jsou definovány kruh se serverem radius 100, ale pouze nejvyšší bod je paralelní s vodorovné nebo svislé osy.)

Dostupnost tyto informace předpokládají, že by měl být možné odvozena škálování a převede faktory, které jsou vhodné pro škálování cestu k velikost na plátno. [ **Volba škálování** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) stránky to ukazuje hvězdičkou odkazoval 11. *Volba* škálování znamená, že nerovné v vodorovného a svislého pokynů, což znamená, že hvězdičkou nezachovají jeho původní poměr stran. Zde je odpovídající kód v `PaintSurface` obslužné rutiny:

```csharp
SKPath path = HendecagramPage.HendecagramPath;
SKRect pathBounds = path.Bounds;

using (SKPaint fillPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Pink
})
using (SKPaint strokePaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 3,
    StrokeJoin = SKStrokeJoin.Round
})
{
    canvas.Scale(info.Width / pathBounds.Width,
                 info.Height / pathBounds.Height);
    canvas.Translate(-pathBounds.Left, -pathBounds.Top);

    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

`pathBounds` Obdélníku získat v horní části tento kód a později se používá s šířka a výška na plátno v `Scale` volání. Volání samostatně škálovaly souřadnice cesty je vykreslen pomocí `DrawPath` volání ale hvězdičkou bude zarovnaný na střed v pravém horním rohu na plátno. Je třeba přesunout dolů a doleva. Toto je úkolem `Translate` volání. Tyto dvě vlastnosti `pathBounds` jsou přibližně – 100, takže překlad faktory jsou přibližně 100. Protože `Translate` po volání `Scale` volat, tyto hodnoty jsou efektivně škálovat škálování faktory, takže se přesouvají středu hvězdy na střed plátna:

[![](scale-images/anisotropicscaling-small.png "Trojitá snímek obrazovky stránky volba škálování")](scale-images/anisotropicscaling-large.png#lightbox "Trojitá snímek obrazovky stránky volba škálování")

Jiný způsob, jak se dá chápat `Scale` a `Translate` volání je pro ověření účinnosti zásad v opačném pořadí: `Translate` volání posune cestu, takže se zcela zobrazí ale orientované v levém horním rohu na plátno. `Scale` Metoda pak díky této hvězdičky větší relativně k levého horního rohu.

Ve skutečnosti že se jeví hvězdičkou o něco větší než na plátno. Problém je šířku tahu. `Bounds` Vlastnost `SKPath` označuje dimenze souřadnice kódovaný v cestě, a který je program používá ji škálovat. Po vykreslení cestu s šířku tahu konkrétní vykreslené cesta je větší než na plátno.

O řešení tohoto problému budete muset kompenzovat který. Jeden snadný přístup v tento program je přidat následující příkaz těsně před `Scale` volání:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Tím se zvyšuje `pathBounds` obdélníku 1,5 jednotkami na všechny čtyři strany. Jedná se o rozumné řešení jenom v případě, že se zaokrouhlí tahu spojení. Pokosové spojení, může být déle a je obtížné vypočítat.

Podobným způsobem s textem, můžete použít také jako **volba Text** ukazuje stránky. Tady je příslušné části `PaintSurface` obslužnou rutinu na základě [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) třídy:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 0.1f,
    StrokeJoin = SKStrokeJoin.Round
})
{
    SKRect textBounds = new SKRect();
    textPaint.MeasureText("HELLO", ref textBounds);

    // Inflate bounds by the stroke width
    textBounds.Inflate(textPaint.StrokeWidth / 2,
                       textPaint.StrokeWidth / 2);

    canvas.Scale(info.Width / textBounds.Width,
                 info.Height / textBounds.Height);
    canvas.Translate(-textBounds.Left, -textBounds.Top);

    canvas.DrawText("HELLO", 0, 0, textPaint);
}
```

Je podobné logiku a text zasahuje do velikosti stránky založené na vrácená z hranice obdélník text `MeasureText` (což je o něco větší než skutečná text):

[![](scale-images/anisotropictext-small.png "Trojitá snímek obrazovky stránky volba Test")](scale-images/anisotropictext-large.png#lightbox "Trojitá snímek obrazovky stránky volba testu")

Pokud potřebujete zachová poměr stran grafické objekty, budete chtít použít isotropic škálování. **Isotropic škálování** stránky to ukazuje pro hvězdičky odkazoval 11. Kroky pro zobrazení grafického objektu v centru stránku s isotropic škálování koncepčně, jsou:

- Převede center grafického objektu levém horním rohu.
- Škálování objekt v závislosti na minimum vodorovného a svislého stránky rozměry dělený grafického objektu dimenze.
- Převede center škálovat objektu k centru stránky.

[ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) Před zobrazení hvězdičkou v obráceném pořadí provede tyto kroky:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPath path = HendecagramArrayPage.HendecagramPath;
    SKRect pathBounds = path.Bounds;

    using (SKPaint fillPaint = new SKPaint())
    {
        fillPaint.Style = SKPaintStyle.Fill;

        float scale = Math.Min(info.Width / pathBounds.Width,
                               info.Height / pathBounds.Height);

        for (int i = 0; i <= 10; i++)
        {
            fillPaint.Color = new SKColor((byte)(255 * (10 - i) / 10),
                                          0,
                                          (byte)(255 * i / 10));
            canvas.Save();
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(scale);
            canvas.Translate(-pathBounds.MidX, -pathBounds.MidY);
            canvas.DrawPath(path, fillPaint);
            canvas.Restore();

            scale *= 0.9f;
        }
    }
}
```

Kód zobrazí také hvězdičkou deset vícekrát, pokaždé, když snížení škálování zohlednit 10 % a progresivně Změna barvy z červené na modrou:

[![](scale-images/isotropicscaling-small.png "Trojitá snímek obrazovky stránky Isotropic škálování")](scale-images/isotropicscaling-large.png#lightbox "Trojitá snímek obrazovky stránky Isotropic škálování")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
