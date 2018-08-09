---
title: Transformace měřítka
description: Thhis článek zkoumá transformace měřítka ve Skiasharpu škálování objektů různých velikostí a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: 94105cbb83e4c6eb3558ca3fc55e505ab41f28fe
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615600"
---
# <a name="the-scale-transform"></a>Transformace měřítka

_Zjistit transformace měřítka ve Skiasharpu škálování pro různě velké objekty_

Jak už víte, v [transformace The přeložit](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md) článku transformace translace můžete přesunout grafický objekt z jednoho umístění do jiného. Transformace měřítka naproti tomu změní velikost grafický objekt:

![](scale-images/scaleexample.png "Vysoký Wordu horizontálně velikost")

Transformace měřítka také často způsobuje grafiky souřadnice přesunout, protože byly provedeny větší.

Dříve jste viděli dvou vzorců transformace, které popisují efekty převodu faktory `dx` a `dy`:

x! = x + dx

y' = y + dy

Škálování úrovně `sx` a `sy` místo additive násobení jsou:

x! = sx. x

y' = sy. y

Výchozí hodnoty přeložit faktory mají hodnotu 0; výchozí hodnoty škálování faktory jsou od 1.

`SKCanvas` Třída definuje čtyři `Scale` metody. První [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/) pro případech, kdy chcete, aby stejné vodorovné a svislé škálování faktor je metoda:

```csharp
public void Scale (Single s)
```

To se označuje jako *isotropic* škálování &mdash; škálování, který je stejné v obou směrech. Škálování isotropic zachová poměr stran objektu.

Druhá [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) metoda vám umožňuje zadat různé hodnoty pro horizontální a vertikální škálování:

```csharp
public void Scale (Single sx, Single sy)
```

V důsledku *anisotropního* škálování.
Třetí [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/SkiaSharp.SKPoint/) metoda kombinuje dva faktory měřítka v jediném `SKPoint` hodnotu:

```csharp
public void Scale (SKPoint size)
```

Čtvrtý `Scale` metoda najdete za chvíli.

**Základní Škálovací** stránce ukazuje `Scale` metody. [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) soubor XAML obsahuje dva `Slider` prvky, které vám umožní vybrat horizontální a vertikální škálování faktory mezi 0 a 10. [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) soubor kódu na pozadí používá tyto hodnoty, aby volala `Scale` před zobrazením zakulacený obdélník pouze tah přerušovanou čárou a přizpůsobí nějaký text v levém horním rohu horním rohu plátna:

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

Může vás zajímat: škálování faktorů vliv hodnota vrácená z `MeasureText` metoda `SKPaint`? Odpověď: vůbec ne. `Scale` je metoda `SKCanvas`. To nemá vliv na cokoli, co dělat s `SKPaint` objektu, dokud tento objekt použijete k vykreslení něco na plátně.

Jak vidíte, vše, co vykreslit po `Scale` volání zvyšuje proporcionálně:

[![](scale-images/basicscale-small.png "Trojitá snímek obrazovky stránky základní Škálovací")](scale-images/basicscale-large.png#lightbox "Trojitá snímek obrazovky stránky základní Škálovací")

Text, šířka přerušovanou čáru, délku pomlčky v tomto řádku zaokrouhlení rohů a 10 pixel rozpětí mezi levém a horním okraji na plátno a zakulacený obdélník musí dodržovat všechny stejné faktory měřítka.

> [!IMPORTANT]
> Univerzální platforma Windows správně nevykresluje anisotropicly škálován text.

Anisotropního škálování způsobí, že šířka tahu se liší pro řádky souladu s vodorovné a svislé osy. (Toto se taky zřejmé z první image na této stránce.) Pokud nechcete, aby šířka tahu neplatila škálování faktory, nastavte na hodnotu 0 a bude vždy jeden pixel široké bez ohledu na to `Scale` nastavení.

Škálování je relativní vůči levém horním rohu plátna. Může to být přesně co chcete, ale nemusí být. Předpokládejme, že chcete umístit textové a obdélník někde jinde na plátně a vy chcete škálovat vzhledem k jeho střed. V takovém případě můžete použít čtvrtou verzi [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/System.Single/System.Single/) metodu, která obsahuje dvě další parametry k určení center škálování:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px` a `py` parametry definovat bod, který se někdy označuje jako *škálování center* , ale v SkiaSharp dokumentace se označuje jako *bodu otáčení*. Toto je bod vzhledem k levém horním rohu plátna, která nemá vliv škálování. Všechny škálování nastane vzhledem k tohoto centra.

[ **Zarovnaný na střed škálování** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) stránce ukazuje, jak to funguje. `PaintSurface` Obslužná rutina je podobná **základní Škálovací** programu s výjimkou, že `margin` hodnota se počítá na střed text ve vodorovném směru, což znamená, že program nejlépe funguje v režimu na výšku:

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

Je umístěn levého horního rohu zakulacený obdélník `margin` pixelů od levého okraje na plátno a `margin` pixelů od horního. Poslední dva argumenty, které mají `Scale` metody jsou nastaveny na tyto hodnoty a šířka a Výška textu, což je také šířka a výška zakulacený obdélník. To znamená, že všechny škálování je relativní vzhledem k centru obdélníku:

[![](scale-images/centeredscale-small.png "Trojitá snímek obrazovky stránky na střed škálování")](scale-images/centeredscale-large.png#lightbox "Trojitá snímek obrazovky stránky škálování na střed")

`Slider` Prvky v rámci tohoto programu mají celou řadu &ndash;10 až 10. Jak je vidět, záporné hodnoty vertikální škálování (třeba na Android obrazovky v centru) způsobit, že objekty kolem vodorovnou osu, která se předá prostřednictvím centra pro škálování. Záporné hodnoty vodorovné škálování (jako je například na pravé straně obrazovky UPW) způsobit, že objekty kolem svislá osa, který se předá prostřednictvím centra pro škálování.

Tato verze čtvrtý `Scale` metoda je ve skutečnosti místní. Můžete chtít podívat, jak to funguje tak, že nahradíte `Scale` metoda v tento kód následujícím kódem:

```csharp
canvas.Translate(-px, -py);
```

Jedná se o negativy souřadnice bodu otáčení.

Nyní spusťte program znovu. Zobrazí se vám posunuty obdélník a text tak, aby centru v levém horním rohu plátna. Stěží uvidíte ho. Posuvníky, protože nyní program neškáluje se vůbec samozřejmě nefungují.

Nyní přidejte základní `Scale` volání (bez měřítka center) *před* , který `Translate` volání:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Pokud jste obeznámeni s v tomto cvičení v jiné že grafické programování systémů, si možná myslíte, že je nesprávný, ale není. Skia zahrnuje volání po sobě jdoucích transformace trochu odlišně od co asi znáte.

S po sobě jdoucích `Scale` a `Translate` volání, center zakulacený obdélník je stále v levém horním rohu, ale teď můžete škálovat je relativní vzhledem k levém horním rohu plátna, což je také center zakulacený obdélník.

Nyní, před který `Scale` volání přidejte další `Translate` volání centrování hodnotami:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Škálovaná výsledek to přejde zpět do původní polohy. Tyto tři volání jsou ekvivalentní:

```csharp
canvas.Scale(sx, sy, px, py);
```

Jednotlivé transformace jsou compounded tak, aby celková transformace vzorec:

 x! = sx. (x – px) + px

 y' = sy. (y-py) + py

Mějte na paměti, že výchozí hodnoty `sx` a `sy` jsou od 1. Je snadné sami přesvědčit, že tyto vzorce není transformovat bodu otáčení (px, py). Zůstane ve stejném umístění vzhledem k na plátno.

Když zkombinujete `Translate` a `Scale` volání, záleží na pořadí. Pokud `Translate` , přichází po `Scale`, překlad faktory jsou efektivně škálovat faktory měřítka. Pokud `Translate` byla zaslána před `Scale`, faktory překladu nejsou škálovat. Tento proces se stane něco srozumitelnější (i když spolu více matematické) při předmětem transformační matice se zavedl až.

`SKPath` Třída definuje jen pro čtení [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) vlastnost, která se vrátí `SKRect` definování rozsahu souřadnice v cestě. Například, když `Bounds` vlastnost se získávají z cesty hendecagram vytvořili dříve, `Left` a `Top` přibližně – 100, jsou vlastnosti obdélníku `Right` a `Bottom` vlastnosti jsou přibližně 100 a `Width` a `Height` vlastnosti jsou přibližně 200. (Většina skutečnými hodnotami jsou malé méně protože body hvězdiček, které jsou definovány pomocí kruhu s poloměrem 100, ale pouze vrchního bodu souběžně s vodorovné nebo svislé osy.)

Dostupnost tyto informace znamená, že by měl být možné odvodit škálování a překládat faktory, které jsou vhodné pro škálování cestu k velikosti na plátně. [ **Anisotropního škálování** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) stránce ukazuje to ukazuje 11 hvězdičkou. *Anisotropního* škálování znamená, že nerovnost vodorovného a svislého směry, což znamená, že na hvězdičku původní poměr stran nezachovají. Zde je příslušný kód v `PaintSurface` obslužné rutiny:

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

`pathBounds` Obdélník získali v horní části tohoto kódu a pak použít později s šířku a výšku na plátně v `Scale` volání. Volání samostatně škálovaly souřadnice cesty je vykreslen pomocí `DrawPath` volání, ale na hvězdičku bude umístěný v pravém horním rohu plátna. Je potřeba přesunout dolů a doleva. To je práce `Translate` volání. Tyto dvě vlastnosti `pathBounds` jsou přibližně – 100, takže překlad faktory jsou přibližně 100. Protože `Translate` po volání `Scale` volání, tyto hodnoty jsou efektivně škálovat škálování faktorů, tak při přechodu středu hvězdy střed plátna:

[![](scale-images/anisotropicscaling-small.png "Trojitá snímek obrazovky stránky Anisotropního škálování")](scale-images/anisotropicscaling-large.png#lightbox "Trojitá snímek obrazovky stránky Anisotropního škálování")

Jiný způsob, jak se dá chápat `Scale` a `Translate` volání je určit efekt, v opačném pořadí: `Translate` volání posune cestu, aby se stal plně viditelný, ale orientovaný v levém horním rohu plátna. `Scale` Metoda pak zvětší této star vzhledem k levého horního rohu.

Ve skutečnosti zobrazí se, že tento registr je o něco větší než na plátno. Problém je šířka tahu. `Bounds` Vlastnost `SKPath` označuje dimenze souřadnice kódovaný v cestě, a to je program používá pro její škálování. Po vykreslení cestu s šířku tahu konkrétní vykreslené cesta je větší než na plátno.

Chcete-li vyřešit tento problém, které potřebujete ke kompenzaci za to. Jedním z přístupů snadno v rámci tohoto programu, je přidejte následující příkaz těsně před `Scale` volání:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Tím se zvyšuje `pathBounds` obdélník ve verzi 1.5 jednotek na všechny čtyři strany. Jedná se o rozumné řešení pouze v případě, že spojení stroke zaokrouhleno. Pokosové spojení mohou být delší a je těžké k výpočtu.

Podobné techniky s textem, můžete použít také jako **Anisotropního Text** demonstruje stránky. Tady je odpovídající část `PaintSurface` obslužnou rutinu z [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) třídy:

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

Je podobná logika a text rozšíří na velikost stránky založené na obdélníku rozsah textu, který vrací z `MeasureText` (což je o něco větší než vlastní text):

[![](scale-images/anisotropictext-small.png "Trojitá snímek obrazovky stránky Anisotropního testovací")](scale-images/anisotropictext-large.png#lightbox "Trojitá snímek obrazovky stránky Anisotropního testu")

Pokud potřebujete zachovat poměr stran grafických objektů, je vhodné použití isotropic škálování. **Isotropic škálování** stránce ukazuje to pro star ukazuje 11. Koncepčně tady jsou kroky pro zobrazení grafického objektu v Centru pro stránky se isotropic škálování:

- Převede uzel center grafický objekt do levého horního rohu.
- Změnit velikost objektu podle minimální rozměry vodorovného a svislého stránky dělený grafický objekt dimenze.
- Převede uzel center škálován objektu do centra pro stránky.

[ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) Před zobrazení hvězdičky v obráceném pořadí provede tyto kroky:

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

Kód také zobrazí hvězdičku deset vícekrát, pokaždé, když snížení škálování faktor 10 % a postupně změnou barvy z red blue:

[![](scale-images/isotropicscaling-small.png "Trojitá snímek obrazovky stránky Isotropic škálování")](scale-images/isotropicscaling-large.png#lightbox "Trojitá snímek obrazovky stránky Isotropic škálování")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
