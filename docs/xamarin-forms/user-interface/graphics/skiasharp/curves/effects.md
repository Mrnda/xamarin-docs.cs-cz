---
title: Efekty cest v ve Skiasharpu
description: Tento článek vysvětluje různé účinky cesty ve Skiasharpu, které umožňují cesty pro vytažení a následně a to demonstruje se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: charlespetzold
ms.author: chape
ms.date: 07/29/2017
ms.openlocfilehash: 28f628fb4e8ab77e9c36e6e1972d7269ad0dad4d
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615675"
---
# <a name="path-effects-in-skiasharp"></a>Efekty cest v ve Skiasharpu

_Zjišťování různých efekty cest, umožňujících cesty pro vytažení a vyplnění_

A *vliv cestu* je instance [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) třídu, která se vytvoří s jednu z osmi statické `Create` metody. `SKPathEffect` Objektu je nastaven na [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) vlastnost `SKPaint` objekt pro celou řadu zajímavých efektů, například vytažení čáry s malé replikované cesta:

![](effects-images/patheffectsample.png "Ukázka propojené řetězce")

Efekty cest vám umožní:

- Protože byl zdvih čáry s tečky a spojovníky
- Řádek s žádným plného tahu
- Vyplnění oblasti šrafování řádky
- Vyplnění oblasti s cestou vedle sebe
- Ujistěte se, ostrých zaoblené
- Přidání náhodného "kolísání" čar a křivek

Kromě toho můžete kombinovat dva nebo více efekty cest.

Tento článek také ukazuje, jak používat `GetFillPath` metoda `SKPaint` převést jednu cestu na jinou cestu s použitím vlastnosti `SKPaint`, včetně `StrokeWidth` a `PathEffect`. Výsledkem je některé zajímavé techniky, jako je například získání cestu, která je přehled jinou cestu. `GetFillPath` je také užitečné souvislosti efekty cest.

## <a name="dots-and-dashes"></a>Tečky a spojovníky

Použití [ `PathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) metodu popsanou v článku [ **tečky a pomlčky**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md). První argument metody je pole obsahující sudý počet dva nebo více hodnot, střídavě délky pomlčky a mezery mezi pomlček délky:

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

Tyto hodnoty jsou *není* vzhledem k šířce stroke. Například pokud je šířka tahu 10 a chcete řádku složené Čtvereček pomlček a mezer čtvereček, nastavte `intervals` pole do {10, 10}. `phase` Argument určuje, kde mezi vzorem pomlček začíná na řádku. V tomto příkladu, pokud má čára začínat Čtvereček mezery, nastavte `phase` až 10.

Konce pomlček se vztahuje `StrokeCap` vlastnost `SKPaint`. Pro celou stroke šířky, je velmi běžné pro tuto vlastnost nastavte na `SKStrokeCap.Round` zaokrouhlit konce pomlček. V tomto případě hodnoty `intervals` pole *není* zahrnují navíc délka vyplývající z zaokrouhlení, což znamená, že kruhové tečkou vyžaduje zadání šířku nula. Pro šířku tahu 10, k vytvoření čáry pomocí tečky a mezery mezi body stejný průměr, použijte `intervals` pole {0, 20}.

**Animovat tečkované Text** je podobná stránka **uvedených Text** stránky je popsáno v článku [ **integrace textu a grafiky** ](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) v tím, že nastavíte, zobrazuje uvedených textové znaky `Style` vlastnost `SKPaint` objektu `SKPaintStyle.Stroke`. Kromě toho **animovat tečkované Text** používá `SKPathEffect.CreateDash` poskytnout to osnovy vychází tečkovaná vzhled a také animuje program `phase` argument `SKPathEffect.CreateDash` metoda aby tečky zdá se, že místo kolem textu znaky. Tady je v režimu na šířku stránka:

[![](effects-images/animateddottedtext-small.png "Trojitá snímek obrazovky stránky animovat tečkované Text")](effects-images/animateddottedtext-large.png#lightbox "Trojitá snímek obrazovky stránky animovat tečkované Text")

[ `AnimatedDottedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Třídy zahájení definovat některé konstanty a také přepsání `OnAppearing` a `OnDisappearing` metody pro animaci:

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    const string text = "DOTTED";
    const float strokeWidth = 10;
    static readonly float[] dashArray = { 0, 2 * strokeWidth };

    SKCanvasView canvasView;
    bool pageIsActive;

    public AnimatedDottedTextPage()
    {
        Title = "Animated Dotted Text";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;

        Device.StartTimer(TimeSpan.FromSeconds(1f / 60), () =>
        {
            canvasView.InvalidateSurface();
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

`PaintSurface` Obslužná rutina začíná tím, že vytvoříte `SKPaint` objektu pro zobrazení potřebného textu. `TextSize` Vlastnost je upravena podle šířka obrazovky:

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create an SKPaint object to display the text
        using (SKPaint textPaint = new SKPaint
            {
                Style = SKPaintStyle.Stroke,
                StrokeWidth = strokeWidth,
                StrokeCap = SKStrokeCap.Round,
                Color = SKColors.Blue,
            })
        {
            // Adjust TextSize property so text is 95% of screen width
            float textWidth = textPaint.MeasureText(text);
            textPaint.TextSize *= 0.95f * info.Width / textWidth;

            // Find the text bounds
            SKRect textBounds = new SKRect();
            textPaint.MeasureText(text, ref textBounds);

            // Calculate offsets to center the text on the screen
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            // Animate the phase; t is 0 to 1 every second
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 1 / 1);
            float phase = -t * 2 * strokeWidth;

            // Create dotted line effect based on dash array and phase
            using (SKPathEffect dashEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                // Set it to the paint object
                textPaint.PathEffect = dashEffect;

                // And draw the text
                canvas.DrawText(text, xText, yText, textPaint);
            }
        }
    }
}
```

Na konci metody `SKPathEffect.CreateDash` metoda je volána pomocí `dashArray` , který je definován jako pole a animovaného `phase` hodnotu. `SKPathEffect` Instance je nastavena na `PathEffect` vlastnost `SKPaint` objektu pro zobrazení potřebného textu.

Alternativně můžete nastavit `SKPathEffect` objektu `SKPaint` objektu před měření text a zarovnání na stránce. V takovém případě však animovaný tečky a pomlčky způsobit, že některé změny velikosti vykresleného textu, a obvykle uvede do trochu vibrace text. (Vyzkoušejte ji!)

Uvidíte také jako kruh animovaný tečky kolem textové znaky, má určitého bodu v každé uzavřené křivky kde body zdá se, že do a z existence vyvolat přes pop. To je, kde cestu, která definuje znak osnovy začíná a končí. Pokud délka cesty není násobkem délce mezi vzorem pomlček (v tomto případě 20 pixelů) může obsahovat pouze část tohoto vzorce na konci cesty.

Je možné nastavit délku v čárkovém vzoru má přizpůsobit délka cesty, ale který vyžaduje určení délka cesty technika, která se v některém z budoucích článků.

**Tečka a pomlčka způsobů** program animuje mezi vzorem pomlček sama tak, aby pomlčky zdá se, že dělení do bodů, které se kombinují na krátké pomlčky formulář znovu:

[![](effects-images/dotdashmorph-small.png "Trojitá snímek obrazovky stránky způsobů čárka – tečka")](effects-images/dotdashmorph-large.png#lightbox "Trojitá snímek obrazovky stránky způsobů čárka – tečka")

[ `DotDashMorphPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Třídy přepsání `OnAppearing` a `OnDisappearing` metody, stejně jako předchozí aplikace nebyla, ale definuje třídu `SKPaint` objektu jako pole:

```csharp
public class DotDashMorphPage : ContentPage
{
    const float strokeWidth = 30;
    static readonly float[] dashArray = new float[4];

    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint ellipsePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = strokeWidth,
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create elliptical path
        using (SKPath ellipsePath = new SKPath())
        {
            ellipsePath.AddOval(new SKRect(50, 50, info.Width - 50, info.Height - 50));

            // Create animated path effect
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 3 / 3);
            float phase = 0;

            if (t < 0.25f)  // 1, 0, 1, 2 --> 0, 2, 0, 2
            {
                float tsub = 4 * t;
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2 * tsub;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2;
            }
            else if (t < 0.5f)  // 0, 2, 0, 2 --> 1, 2, 1, 0
            {
                float tsub = 4 * (t - 0.25f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2 * (1 - tsub);
                phase = strokeWidth * tsub;
            }
            else if (t < 0.75f) // 1, 2, 1, 0 --> 0, 2, 0, 2
            {
                float tsub = 4 * (t - 0.5f);
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2 * tsub;
                phase = strokeWidth * (1 - tsub);
            }
            else               // 0, 2, 0, 2 --> 1, 0, 1, 2
            {
                float tsub = 4 * (t - 0.75f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2 * (1 - tsub);
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2;
            }

            using (SKPathEffect pathEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                ellipsePaint.PathEffect = pathEffect;
                canvas.DrawPath(ellipsePath, ellipsePaint);
            }
        }
    }
}
```

`PaintSurface` Obslužná rutina vytvoří elipsy cestu na základě velikosti stránky a provede dlouhé část kódu, který nastaví `dashArray` a `phase` proměnné. Jako proměnnou animovaný `t` rozsahu 0 až 1, `if` bloky rozdělte této doby do čtyř čtvrtletí a v každé z těchto čtvrtletí `tsub` také od 0 do 1. Na konci velmi program vytvoří `SKPathEffect` a nastaví ji `SKPaint` objektů pro kreslení.

## <a name="from-path-to-path"></a>Z cesty na cestu

[ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) Metoda `SKPaint` převede jednu cestu na jiný v závislosti na nastavení v `SKPaint` objektu. Chcete-li zjistit, jak to funguje, nahraďte `canvas.DrawPath` volání v předchozí program následujícím kódem:

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

V tomto nového kódu `GetFillPath` volání převede `ellipsePath` (která je právě elipsu) do `newPath`, který se následně zobrazí s `newPaint`. `newPaint` Je vytvořen objekt se všemi možnými nastavení výchozí vlastnost s tím rozdílem, že `Style` je vlastnost nastavena na základě na logickou hodnotu návratová hodnota z `GetFillPath`.

Vizuály jsou stejné s výjimkou barvu, která je nastavena v `ellipsePaint` , ale ne `newPaint`. Místo jednoduchá elipsa definované v `ellipsePath`, `newPath` obsahuje mnoho rozvrhy cesty, které definují řady tečky a pomlčky. To je výsledkem použití různých vlastností `ellipsePaint` – `StrokeWidth`, `StrokeCap`, a `PathEffect` – k `ellipsePath` a uvedením Výsledná cesta `newPath`. `GetFillPath` Metoda vrátí logickou hodnotu označující, zda cílová cesta je pro vyplnění nebo Ne, v tomto příkladu je návratová hodnota `true` pro vyplnění cestu.

Zkuste změnit `Style` nastavení `newPaint` k `SKPaintStyle.Stroke` a zobrazí se jednotlivé cesty obrysy uvedených šířka v pixelech jeden řádek.

## <a name="stroking-with-a-path"></a>Vytažení s cestou

[ `SKPathEffect.Create1DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create1DPath/p/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPath1DPathEffectStyle/) Metoda se koncepčně podobá `SKPathEffect.CreateDash` s tím rozdílem, že zadáte cestu, spíše než vzor pomlček a mezer. Tato cesta se replikují více než jednou tah řádku nebo křivky.

Syntaxe je následující:

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

> [!IMPORTANT]
> Dávejte pozor: neexistuje přetížení `Create1DPath` , která je definována s argumentem typu výčtu `SkPath1DPathEffect` s malým "k". Tento název se o chybu, a proto, že výčet a metoda jsou zastaralé, ale je to velmi jednoduché nepoužívané metody, která se stanou součástí vašeho kódu a je obtížné zjistit, co přesně je chybná.

Obecně platí, cesta, kterou předat `Create1DPath` bude malé a na střed kolem bodu (0, 0). `advance` Parametr určuje vzdálenost od centra cestu jako cesta se replikuje na řádku. Tento argument se obvykle nastavena přibližné šířku cesty. `phase` Argument jde skvěle dohromady jako stejný atribut role tady to dělá v `CreateDash` metody.

[ `SKPath1DPathEffectStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath1DPathEffectStyle/) Má tři členy:

- [`Translate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Translate/)
- [`Rotate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Rotate/)
- [`Morph`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Morph/)

`Translate` Člen způsobí, že cesta tak si zachováte orientaci stejné jako se replikují podle řádku nebo křivky. Pro `Rotate`, cesta otočen podle tangens křivky. Cesta obsahuje její normální orientací vodorovné čáry. `Morph` je podobný `Rotate` s tím rozdílem, že samotné cestě je také zakřivené tak, aby odpovídaly zaoblení řádku se vytažený.

**Vliv cestu 1 D** stránce ukazuje těchto tří možností. [ **OneDimensionalPathEffectPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) soubor definuje ovládacího prvku pro výběr obsahující tři položky odpovídající tři členy výčtu:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.OneDimensionalPathEffectPage"
             Title="1D Path Effect">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="effectStylePicker"
                Title="Effect Style"
                Grid.Row="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Translate</x:String>
                <x:String>Rotate</x:String>
                <x:String>Morph</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1" />
    </Grid>
</ContentPage>
```

[ **OneDimensionalPathEffectPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) použití modelu code-behind soubor definuje tří `SKPathEffect` objekty jako pole. Tyto jsou vytvořeny pomocí `SKPathEffect.Create1DPath` s `SKPath` objekty vytvořené pomocí `SKPath.ParseSvgPathData`. První je jednoduché pole, druhá je tvaru kosočtverce a třetí obdélníku. Ty se používají k předvedení tři efekt styly:

```csharp
public partial class OneDimensionalPathEffectPage : ContentPage
{
    SKPathEffect translatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 -10 L 10 -10, 10 10, -10 10 Z"),
                                  24, 0, SKPath1DPathEffectStyle.Translate);

    SKPathEffect rotatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 0 L 0 -10, 10 0, 0 10 Z"),
                                  20, 0, SKPath1DPathEffectStyle.Rotate);

    SKPathEffect morphPathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -25 -10 L 25 -10, 25 10, -25 10 Z"),
                                  55, 0, SKPath1DPathEffectStyle.Morph);

    SKPaint pathPaint = new SKPaint
    {
        Color = SKColors.Blue
    };

    public OneDimensionalPathEffectPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath path = new SKPath())
        {
            path.MoveTo(new SKPoint(0, 0));
            path.CubicTo(new SKPoint(2 * info.Width, info.Height),
                         new SKPoint(-info.Width, info.Height),
                         new SKPoint(info.Width, 0));

            switch (effectStylePicker.Items[effectStylePicker.SelectedIndex])
            {
                case "Translate":
                    pathPaint.PathEffect = translatePathEffect;
                    break;

                case "Rotate":
                    pathPaint.PathEffect = rotatePathEffect;
                    break;

                case "Morph":
                    pathPaint.PathEffect = morphPathEffect;
                    break;
            }

            canvas.DrawPath(path, pathPaint);
        }
    }
}
```

`PaintSurface` Bézierovy křivky smyčky kolem samotné, který přistupuje k výběru k určení, které vytvoří obslužnou rutinu `PathEffect` by měl být použité k obtažení ho. Tyto tři možnosti – `Translate`, `Rotate`, a `Morph` – zobrazují zleva doprava:

[![](effects-images/1dpatheffect-small.png "Trojitá snímek obrazovky stránky vliv cestu 1D")](effects-images/1dpatheffect-large.png#lightbox "Trojitá snímek obrazovky stránky vliv cestu 1 D")

Cesta zadaná v `SKPathEffect.Create1DPath` metoda je vždy vyplněné. Cesta zadaná v `DrawPath` metoda je vždy vytažený, pokud `SKPaint` objekt má jeho `PathEffect` nastavenou na vliv cestu 1 D. Všimněte si, že `pathPaint` objekt nemá žádné `Style` nastavení, kde je obvykle použit výchozí `Fill`, ale cesta je vytažený bez ohledu na to.

Pole použité v `Translate` příklad je 20 pixelů čtverec a `advance` argument je nastaven na 24. Tento rozdíl způsobí, že mezery mezi poli při řádku je přibližně vodorovně nebo svisle, ale pole při dojít k překrytí trochu řádku totiž Úhlopříčný úhlopříčně pole je 28.3 pixelů.

Kosočtverec v `Rotate` příklad je také 20 pixelů na šířku. `advance` Je nastavená na 20, aby body nadále touch podle kosočtverce je otáčí spolu zaoblení řádku.

Tvar obdélníku v `Morph` příkladem je 50 pixelů na šířku se `advance` nastavení 55 tak malá mezera mezi obdélníky, jak jsou ohnuty kolem Bézierovy křivky.

Pokud `advance` argumentu je menší než velikost cesty a pak může dojít k překrytí replikované cesty. Výsledkem může být některé zajímavé efekty. **Propojené řetězu** stránce se zobrazuje řada překrývajícími se kruhy, které vypadá to, že se podobají propojený řetězec, který přestane reagovat ve tvaru výrazný, ale trolejového vedení:

[![](effects-images/linkedchain-small.png "Trojitá snímek obrazovky stránky propojené řetězu")](effects-images/linkedchain-large.png#lightbox "Trojitá snímek obrazovky stránky propojené řetězce")

Podívejte se velmi zavřít a uvidíte, že ty nejsou ve skutečnosti kruzích. Každé propojení v řetězci je dvě elipsy, velikost a umístěn, takže se zdá se, že pro připojení s sousední odkazy.

Řetězec nebo kabel jednotné distribuce přestane reagovat ve formuláři trolejového vedení. Vytvořené ve formě obrácenou trolejového vedení arch rovnoměrná distribuce tlaku váha arch využívá výhod. Trolejového vedení má zdánlivě jednoduché matematické Popis:

y =. COSH(x / a)

*Cosh* hyperbolický kosinus funkcí. Pro *x* rovná 0, *cosh* rovná nule a *y* rovná *a*. To je center trolejového. Podobně jako *kosinus* funkce, *cosh* je označen jako *i*, to znamená, že *cosh(–x)* rovná *cosh(x)*, a zvýšení hodnoty pro zvýšení kladné nebo záporné argumenty. Tyto hodnoty popsat, aby tvořily strany trolejového křivky.

Hledání správné hodnoty *a* podle trolejového vedení dimenzím, na stránce telefonu není přímé výpočtu. Pokud *w* a *h* jsou šířky a výšky obdélníku, optimální hodnotu *a* splňuje následující rovnice:

COSH (w/2 /) = 1 + h / a

V následující metodu [ `LinkedChainPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) třída zahrnuje tento rovnosti rekapitulací dvou výrazů vlevo a vpravo od znaménka rovnosti jako `left` a `right`. Pro malé hodnoty *a*, `left` je větší než `right`; pro velké hodnoty *a*, `left` je menší než `right`. `while` Smyčky zúží v na optimální hodnoty *a*:

```csharp
float FindOptimumA(float width, float height)
{
    Func<float, float> left = (float a) => (float)Math.Cosh(width / 2 / a);
    Func<float, float> right = (float a) => 1 + height / a;

    float gtA = 1;         // starting value for left > right
    float ltA = 10000;     // starting value for left < right

    while (Math.Abs(gtA - ltA) > 0.1f)
    {
        float avgA = (gtA + ltA) / 2;

        if (left(avgA) < right(avgA))
        {
            ltA = avgA;
        }
        else
        {
            gtA = avgA;
        }
    }

    return (gtA + ltA) / 2;
}
```

`SKPath` Objektu pro propojení se vytvoří v konstruktoru třídy a výsledné `SKPathEffect` objektu je nastaven na `PathEffect` vlastnost `SKPaint` objekt, který je uložen jako pole:

```csharp
public class LinkedChainPage : ContentPage
{
    const float linkRadius = 30;
    const float linkThickness = 5;

    Func<float, float, float> catenary = (float a, float x) => (float)(a * Math.Cosh(x / a));

    SKPaint linksPaint = new SKPaint
    {
        Color = SKColors.Silver
    };

    public LinkedChainPage()
    {
        Title = "Linked Chain";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the individual links
        SKRect outer = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
        SKRect inner = outer;
        inner.Inflate(-linkThickness, -linkThickness);

        using (SKPath linkPath = new SKPath())
        {
            linkPath.AddArc(outer, 55, 160);
            linkPath.ArcTo(inner, 215, -160, false);
            linkPath.Close();

            linkPath.AddArc(outer, 235, 160);
            linkPath.ArcTo(inner, 395, -160, false);
            linkPath.Close();

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(linkPath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);
        }
    }
    ...
}
```

Hlavní práce `PaintSurface` obslužná rutina je vytvořit cestu pro samotné trolejového vedení. Po určení optimálním *a* a uložením do `optA` proměnné, je také nutné vypočítat posun z horní části okna. Poté jej lze nashromáždit kolekce `SKPoint` hodnoty trolejovém vedení, který do cestu a nakreslení cesty s dříve vytvořenou `SKPaint` objektu:

```csharp
public class LinkedChainPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        // Width and height of catenary
        int width = info.Width;
        float height = info.Height - linkRadius;

        // Find the optimum 'a' for this width and height
        float optA = FindOptimumA(width, height);

        // Calculate the vertical offset for that value of 'a'
        float yOffset = catenary(optA, -width / 2);

        // Create a path for the catenary
        SKPoint[] points = new SKPoint[width];

        for (int x = 0; x < width; x++)
        {
            points[x] = new SKPoint(x, yOffset - catenary(optA, x - width / 2));
        }

        using (SKPath path = new SKPath())
        {
            path.AddPoly(points, false);

            // And render that path with the linksPaint object
            canvas.DrawPath(path, linksPaint);
        }
    }
    ...
}
```

Tento program definuje cestu používanou v `Create1DPath` mít jeho (0, 0) přejděte v centru. Vypadá to, že přiměřené vzhledem k tomu, (0, 0) bodu cesty je v souladu s křivku, která je adorning nebo řádku. Ale můžete použít jiných střed (0, 0) bod pro některé speciální efekty.

**Dopravní pás** stránka vytvoří cesta podobné běžícím podlouhlá pásu s zakřivené horní a dolní to přizpůsoben pro velikosti okna. Tato cesta je vytažené jednoduchý `SKPaint` 20 pixelů na šířku a barevné šedá objektů a potom vytažený znovu s jinou `SKPaint` objektu `SKPathEffect` objekt odkazující na cestu podobný malý kontejneru:

[![](effects-images/conveyorbelt-small.png "Trojitá snímek obrazovky stránky dopravní pás")](effects-images/conveyorbelt-large.png#lightbox "Trojitá snímek obrazovky stránky dopravní pás")

(0, 0) bodu cesty kontejneru je popisovač, takže když `phase` argument je animovaný, sektorů zdá se, že točí kolem dopravní pás, možná vybírání rozsahu adres do vody v dolní části a vypsání uvedený v horní části.

[ `ConveyorBeltPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs) Třída implementuje animace s přepsáními `OnAppearing` a `OnDisappearing` metody. Cesta k oblasti je definován v konstruktoru na stránce:

```csharp
public class ConveyorBeltPage : ContentPage
{
    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint conveyerPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 20,
        Color = SKColors.DarkGray
    };

    SKPath bucketPath = new SKPath();

    SKPaint bucketsPaint = new SKPaint
    {
        Color = SKColors.BurlyWood,
    };

    public ConveyorBeltPage()
    {
        Title = "Conveyor Belt";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the bucket starting with the handle
        bucketPath.AddRect(new SKRect(-5, -3, 25, 3));

        // Sides
        bucketPath.AddRoundedRect(new SKRect(25, -19, 27, 18), 10, 10,
                                  SKPathDirection.CounterClockwise);
        bucketPath.AddRoundedRect(new SKRect(63, -19, 65, 18), 10, 10,
                                  SKPathDirection.CounterClockwise);

        // Five slats
        for (int i = 0; i < 5; i++)
        {
            bucketPath.MoveTo(25, -19 + 8 * i);
            bucketPath.LineTo(25, -13 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                             SKPathDirection.CounterClockwise, 65, -13 + 8 * i);
            bucketPath.LineTo(65, -19 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                             SKPathDirection.Clockwise, 25, -19 + 8 * i);
            bucketPath.Close();
        }

        // Arc to suggest the hidden side
        bucketPath.MoveTo(25, -17);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                         SKPathDirection.Clockwise, 65, -17);
        bucketPath.LineTo(65, -19);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                         SKPathDirection.CounterClockwise, 25, -19);
        bucketPath.Close();

        // Make it a little bigger and correct the orientation
        bucketPath.Transform(SKMatrix.MakeScale(-2, 2));
        bucketPath.Transform(SKMatrix.MakeRotationDegrees(90));
    }
    ...
```

Kód pro vytvoření kontejneru se dokončí s dvěma transformace, které se ujistěte se o něco větší kontejneru a jeho otočíte. Používání těchto transformací byla jednodušší než nastavení všechny souřadnice v předchozím kódu.

`PaintSurface` Obslužná rutina začne definováním cestu pro dopravní pás samotný. Toto je jednoduše pár řádků a dvojice středníkem kruhy, které jsou zpracovány s 20 pixelů celý řádek tmavě šedé:

```csharp
public class ConveyorBeltPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float width = info.Width / 3;
        float verticalMargin = width / 2 + 150;

        using (SKPath conveyerPath = new SKPath())
        {
            // Straight verticals capped by semicircles on top and bottom
            conveyerPath.MoveTo(width, verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large,
                               SKPathDirection.Clockwise, 2 * width, verticalMargin);
            conveyerPath.LineTo(2 * width, info.Height - verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large,
                               SKPathDirection.Clockwise, width, info.Height - verticalMargin);
            conveyerPath.Close();

            // Draw the conveyor belt itself
            canvas.DrawPath(conveyerPath, conveyerPaint);

            // Calculate spacing based on length of conveyer path
            float length = 2 * (info.Height - 2 * verticalMargin) +
                           2 * ((float)Math.PI * width / 2);

            // Value will be somewhere around 200
            float spacing = length / (float)Math.Round(length / 200);

            // Now animate the phase; t is 0 to 1 every 2 seconds
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 2 / 2);
            float phase = -t * spacing;

            // Create the buckets PathEffect
            using (SKPathEffect bucketsPathEffect =
                        SKPathEffect.Create1DPath(bucketPath, spacing, phase,
                                                  SKPath1DPathEffectStyle.Rotate))
            {
                // Set it to the Paint object and draw the path again
                bucketsPaint.PathEffect = bucketsPathEffect;
                canvas.DrawPath(conveyerPath, bucketsPaint);
            }
        }
    }
}
```

Logika pro kreslení dopravní pás nebude fungovat v režimu na šířku.

By měl být rozloženy sektorů o 200 pixelů od sebe na běžícím pásu. Dopravní pás je však pravděpodobně není násobkem dlouhý, 200 pixelů, což znamená, jako `phase` argument `SKPathEffect.Create1DPath` je animovaný, intervalů se vyvolat přes pop do proměnné a z existence.

Z tohoto důvodu program nejprve vypočítá hodnotu s názvem `length` , který je délka běžícím pásu. Protože dopravní pás se skládá z přímé čáry a středníkem kruhy, jedná se o jednoduchý výpočet. V dalším kroku se počet kbelíků vypočítá jako podíl `length` podle 200. To se zaokrouhlí na nejbližší celé číslo, a pak je číslo rozdělen do `length`. Výsledkem je mezery pro celočíselný počet kbelíků. `phase` Jednoduše část, která je argumentem.

## <a name="from-path-to-path-again"></a>Z cesty na cestu znovu

V dolní části `DrawSurface` obslužné rutiny v **dopravní pás**, okomentujte `canvas.DrawPath` volání a nahraďte ho následujícím kódem:

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

Stejně jako v předchozím příkladu `GetFillPath`, uvidíte, že výsledky jsou stejné s výjimkou barvu. Po provedení `GetFillPath`, `newPath` objekt obsahuje více kopií cesta kontejneru, každý umístěn ve stejném přímé, že animaci je umístěn v okamžiku volání.

## <a name="hatching-an-area"></a>Šrafování oblast

[ `SKPathEffect.Create2DLines` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DLine/p/System.Single/SkiaSharp.SKMatrix/) Metoda vyplní oblast paralelní řádků a často se mu říká *šrafovaných řádky*. Tato metoda má následující syntaxi:

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

`width` Argument určuje šířku tahu řádky šrafování. `matrix` Parametr je kombinace škálování a volitelné otáčení. Koeficient změny měřítka Určuje přírůstek pixel využívající Skia na mezery mezi řádky šrafování. Oddělení mezi řádky je měřítko minus `width` argument. Pokud škálovací faktor je menší než nebo rovna hodnotě `width` hodnotu, nevytvoří se žádný prostor mezi řádky šrafování, a bude oblasti se zobrazí pro vyplnění. Zadejte stejnou hodnotu pro horizontální a vertikální škálování.

Ve výchozím nastavení jsou vodorovné šrafování řádky. Pokud `matrix` parametr obsahuje otočení, šrafování řádky jsou otočit po směru hodinových ručiček.

**Šrafování výplně** stránce ukazuje tento vliv cestu. [ `HatchFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs) Třída definuje tři efekty cest jako pole, první pro vodorovné šrafování řádky s šířka v pixelech 3 s škálovací faktor určující, ke které jsou rozmístěné 6 pixelů od sebe. Oddělení mezi řádky je proto 3 pixelů. Pro svislý šrafování řádky s šířka v pixelech 6 rozmístěné 24 pixelů od sebe (takže oddělení je 18 pixelů), je druhý vliv cestu a třetí je pro Šikmé šrafování řádky 12 pixelů široký rozložených 36 pixelů od sebe.

```csharp
public class HatchFillPage : ContentPage
{
    SKPaint fillPaint = new SKPaint();

    SKPathEffect horzLinesPath = SKPathEffect.Create2DLine(3, SKMatrix.MakeScale(6, 6));

    SKPathEffect vertLinesPath = SKPathEffect.Create2DLine(6,
        Multiply(SKMatrix.MakeRotationDegrees(90), SKMatrix.MakeScale(24, 24)));

    SKPathEffect diagLinesPath = SKPathEffect.Create2DLine(12,
        Multiply(SKMatrix.MakeScale(36, 36), SKMatrix.MakeRotationDegrees(45)));

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

Všimněte si, že matice `Multiply` metody. Horizontální a vertikální škálování faktory jsou stejné, není důležité pořadí, ve které se vynásobené matice měřítka a otočení.

`PaintSurface` Obslužná rutina používá efekty těchto tří cest mají tři různé barvy v kombinaci s `fillPaint` tak, aby vyplnil zakulacený obdélník velikost, aby se vešel na stránku. `Style` Nastavenou na `fillPaint` se ignoruje; při `SKPaint` objekt zahrnuje vliv cestu, vytvořené z `SKPathEffect.Create2DLine`, bez ohledu na to naplní oblasti:

```csharp
public class HatchFillPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath roundRectPath = new SKPath())
        {
            // Create a path
            roundRectPath.AddRoundedRect(
                new SKRect(50, 50, info.Width - 50, info.Height - 50), 100, 100);

            // Horizontal hatch marks
            fillPaint.PathEffect = horzLinesPath;
            fillPaint.Color = SKColors.Red;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Vertical hatch marks
            fillPaint.PathEffect = vertLinesPath;
            fillPaint.Color = SKColors.Blue;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Diagonal hatch marks -- use clipping
            fillPaint.PathEffect = diagLinesPath;
            fillPaint.Color = SKColors.Green;

            canvas.Save();
            canvas.ClipPath(roundRectPath);
            canvas.DrawRect(new SKRect(0, 0, info.Width, info.Height), fillPaint);
            canvas.Restore();

            // Outline the path
            canvas.DrawPath(roundRectPath, strokePaint);
        }
    }
    ...
}
```

Pokud jste pečlivě si prohlédněte výsledky, uvidíte, že červené a modré šrafování řádků nejsou omezeny přesně na zakulacený obdélník. (Toto je zjevně charakteristiku základního kódu Skia.) Pokud je to nevyhovující, alternativním přístupem je zobrazena pro řádky Šikmé šrafování zeleně: zakulacený obdélník se používá jako ořezové cesty a šrafování řádky jsou vykreslovány na celou stránku.

`PaintSurface` Obslužná rutina končí volání jednoduše vytáhnout zakulacený obdélník, abyste si mohli zobrazit nesoulad s šrafování červené a modré čáry:

[![](effects-images/hatchfill-small.png "Trojitá snímek obrazovky stránky šrafování výplně")](effects-images/hatchfill-large.png#lightbox "Trojitá snímek obrazovky stránky šrafování výplně")

Na obrazovce s Androidem nevypadá skutečně tímto způsobem: škálování na snímku obrazovky způsobila dynamického zajišťování červené čáry a dynamicky konsolidovat do zdánlivě širší červené čáry a širší mezer.

## <a name="filling-with-a-path"></a>Vyplnění pomocí cesty

[ `SKPathEffect.Create2DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DPath/p/SkiaSharp.SKMatrix/SkiaSharp.SKPath/) Umožňuje vyplnění oblasti za cestu, která se replikuje vodorovně a svisle, výsledkem bude bloků v oblasti:

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

`SKMatrix` Měřítka označení vodorovné a svislé mezery replikované cestu. Nelze otočit cesta použití této funkce, ale `matrix` argument; Chcete-li cesta střídán, otočit samotné cestě pomocí `Transform` metody definované `SKPath`.

Replikované cesta je obvykle v souladu s levém a horním okraji obrazovky, místo oblasti se vyplněné. Toto chování můžete přepsat zadáním faktory překlad mezi 0 a škálování faktorů zadejte vodorovný a svislý posun od horní a levé strany.

**Cesta dlaždice vyplnit** stránce ukazuje tento vliv cestu. Cesta používaná pro dělení na bloky oblasti je definován jako pole [ `PathFileFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) třídy. Vodorovné a svislé souřadnice rozmezí –40 až 40, to znamená, že tato cesta je 80 pixelů Čtvereček:

```csharp
public class PathTileFillPage : ContentPage
{
    SKPath tilePath = SKPath.ParseSvgPathData(
        "M -20 -20 L 2 -20, 2 -40, 18 -40, 18 -20, 40 -20, " +
        "40 -12, 20 -12, 20 12, 40 12, 40 40, 22 40, 22 20, " +
        "-2 20, -2 40, -20 40, -20 8, -40 8, -40 -8, -20 -8 Z");
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Red;

            using (SKPathEffect pathEffect =
                   SKPathEffect.Create2DPath(SKMatrix.MakeScale(64, 64), tilePath))
            {
                paint.PathEffect = pathEffect;

                canvas.DrawRoundRect(
                    new SKRect(50, 50, info.Width - 50, info.Height - 50),
                    100, 100, paint);
            }
        }
    }
}
```

V `PaintSurface` obslužné rutiny, `SKPathEffect.Create2DPath` volání nastavuje vodorovné a svislé mezery na 64 způsobí 80 pixel Čtvereček dlaždice můžete překrývat. Naštěstí cestu vypadá podobně jako část ještě nám zbývá, meshing krásně s přiléhající dlaždice:

[![](effects-images/pathtilefill-small.png "Trojitá snímek obrazovky stránky zadejte cestu dlaždice")](effects-images/pathtilefill-large.png#lightbox "Trojitá snímek obrazovky stránky zadejte cestu dlaždice")

Probíhá škálování z původní snímek obrazovky způsobí, že některé narušení, zejména na obrazovce pro Android.

Všimněte si, že tyto dlaždice vždy zobrazovat celou a nikdy se zkrátí. Na prvních dvou snímky obrazovky to není i zřejmé, že je oblast je vyplněný zakulacený obdélník. Pokud budete chtít zkrátit tyto dlaždice na konkrétní oblasti, použijte cesty oříznutí.

Zkuste `Style` vlastnost `SKPaint` objektu `Stroke`, a zobrazí se vám jednotlivým dlaždicím uvedených spíše než vyplněné.

## <a name="rounding-sharp-corners"></a>Zaoblení rohů Sharp

**Zaokrouhlí pro sedmiúhelník** program zobrazí v [ **tři způsoby, jak nakreslit oblouk** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) článku používá tečný oblouk na křivku body elementu figure sedm přenosů. **Pro jiné zaokrouhlí sedmiúhelník** stránce ukazuje mnohem jednodušší postup, který používá vliv cestu, vytvořené z [ `SKPathEffect.CreateCorner` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCorner/p/System.Single/) metoda:

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

I když jediný argument jmenuje `radius` je nutné nastavit na polovinu požadované poloměr. (Toto je typické pro základní kód Skia.)

Tady je `PaintSurface` obslužné rutiny v [ `AnotherRoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) třídy:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);
    SKPoint[] vertices = new SKPoint[numVertices];
    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    float cornerRadius = 100;

    // Create the path
    using (SKPath path = new SKPath())
    {
        path.AddPoly(vertices, true);

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            // Set argument to half the desired corner radius!
            paint.PathEffect = SKPathEffect.CreateCorner(cornerRadius / 2);

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);

            // Uncomment DrawCircle call to verify corner radius
            float offset = cornerRadius / (float)Math.Sin(Math.PI * (numVertices - 2) / numVertices / 2);
            paint.Color = SKColors.Green;
            // canvas.DrawCircle(vertices[0].X, vertices[0].Y + offset, cornerRadius, paint);
        }
    }
}
```

Můžete použít tento efekt vytažení nebo na základě naplnění `Style` vlastnost `SKPaint` objektu. Tady je na všech třech platformách:

[![](effects-images/anotherroundedheptagon-small.png "Trojitá snímek obrazovky stránky pro jiné zaokrouhlí sedmiúhelník")](effects-images/anotherroundedheptagon-large.png#lightbox "Trojitá snímek obrazovky stránky pro jiné zaokrouhlí sedmiúhelník")

Uvidíte, že pro tento zakulacený sedmiúhelník je stejný jako předchozí program. Pokud potřebujete další přesvědčit poloměr je skutečně 100 spíše než 50 podle `SKPathEffect.CreateCorner` volání, které můžete Odkomentujte poslední příkaz v programu a viz 100 poloměr kruhu bude zobrazen v horním.

## <a name="random-jitter"></a>Náhodné zpoždění

Někdy bezchybného rovné čáry počítačové grafice nejsou poměrně co chtějí, a trochu náhodnost je žádoucí. V takovém případě budete chtít zkuste [ `SKPathEffect.CreateDiscrete` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDiscrete/p/System.Single/System.Single/System.UInt32/) metody:

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

Můžete použít tento vliv cestu pro vytažení nebo naplnění. Řádky jsou rozdělené do připojených segmenty – přibližné délka je určená `segLength` – a rozšířit v jiné pokyny. Rozsah odchylky od původní řádek je určená `deviation`.

Konečný argument je základní hodnota používá ke generování pseudonáhodné posloupnosti používané pro efekt. Vliv kolísání bude vypadat trochu jinak pro různé rychlosti. Argument má výchozí hodnotu 0, což znamená, že stejný efekt je vždy, když spustíte program. Pokud chcete jiné kolísání pokaždé, když je překreslit obrazovky, můžete nastavit počáteční hodnotu na `Millisecond` vlastnost `DataTime.Now` hodnoty (například).


**Kolísání experimentovat** stránce můžete experimentovat s různými hodnotami v vytažení obdélník:

[![](effects-images/jitterexperiment-small.png "Trojitá snímek obrazovky stránky kolísání experimentu")](effects-images/jitterexperiment-large.png#lightbox "Triple screenshot of the JitterExperiment page")

Program je straightfoward. [ **JitterExperimentPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) soubor vytvoří dvě `Slider` elementy a `SKCanvasView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.JitterExperimentPage"
             Title="Jitter Experiment">
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
                    <Setter Property="Margin" Value="20, 0" />
                    <Setter Property="Minimum" Value="0" />
                    <Setter Property="Maximum" Value="100" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="segLengthSlider"
                Grid.Row="0"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference segLengthSlider},
                              Path=Value,
                              StringFormat='Segment Length = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="deviationSlider"
                Grid.Row="2"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference deviationSlider},
                              Path=Value,
                              StringFormat='Deviation = {0:F0}'}"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

`PaintSurface` Obslužné rutiny v [ **JitterExperimentPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) soubor kódu na pozadí se nazývá pokaždé, když se `Slider` hodnota se mění. Volá `SKPathEffect.CreateDiscrete` použitím obou `Slider` hodnoty a použije ho k obtažení obdélník:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float segLength = (float)segLengthSlider.Value;
    float deviation = (float)deviationSlider.Value;

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.StrokeWidth = 5;
        paint.Color = SKColors.Blue;

        using (SKPathEffect pathEffect = SKPathEffect.CreateDiscrete(segLength, deviation))
        {
            paint.PathEffect = pathEffect;

            SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Tomu můžete použít pro vyplnění, v takovém případě obrys oblasti plného podléhá tyto náhodné odchylky. **Kolísání Text** stránce ukazuje, jak pomocí tohoto vliv cestu k zobrazení textu. Většina kódu v `PaintSurface` obslužná rutina [ `JitterTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) třídy připadá na velikost a zarovnání textu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "FUZZY";

    using (SKPaint textPaint = new SKPaint())
    {
        textPaint.Color = SKColors.Purple;
        textPaint.PathEffect = SKPathEffect.CreateDiscrete(3f, 10f);

        // Adjust TextSize property so text is 95% of screen width
        float textWidth = textPaint.MeasureText(text);
        textPaint.TextSize *= 0.95f * info.Width / textWidth;

        // Find the text bounds
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        // Calculate offsets to center the text on the screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

Tady je spuštěna v režimu na šířku na všech třech platformách:

[![](effects-images/jittertext-small.png "Trojitá snímek obrazovky stránky kolísání Text")](effects-images/jittertext-large.png#lightbox "Triple screenshot of the JitterText page")

## <a name="path-outlining"></a>Cesta osnovy

Seznámili jste se už dva trochu příklady [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) metoda `SKPaint`, která existuje také v [přetížení](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/):

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale)
```

Pouze první dva argumenty jsou povinné. Metoda má přístup k cestě odkazuje `src` argument, změní data cestu na základě vlastností stroke v `SKPaint` objektu (včetně `PathEffect` vlastnost) a pak zapíše výsledky do `dst` cestu. `resScale` Parametr umožňuje upravit přesnost vytvořte menší cílovou cestu a `cullRect` argument může eliminovat rozvrhy mimo obdélníku.

Jeden základní použití této metody nezahrnuje efekty cest vůbec. Pokud `SKPaint` objekt má jeho `Style` vlastnost nastavena na `SKPaintStyle.Stroke`a provede *není* mít jeho `PathEffect` nastavena, pak `GetFillPath` vytvoří, který představuje cestu *osnovy*z cesty zdrojových jako by se měl byla vytažený podle vlastností Malování.

Například pokud `src` cesta je jednoduchý kruh poloměru 500 a `SKPaint` objekt Určuje šířku tahu 100, pak bude `dst` dvou soustředných kruhy, jednu s protokolem radius 450 a druhý s protokolem radius 550 stane se cesta. Je volána metoda `GetFillPath` protože naplňování to `dst` cesta je stejný jako vytažení `src` cestu. Ale můžete také tah `dst` cesta zobrazíte obrysy cesty.

**Klepnutím osnovy cestu** ukazuje to. `SKCanvasView` a `TapGestureRecognizer` instance se vytvářejí v [ **TapToOutlineThePathPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) souboru. [ **TapToOutlineThePathPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) použití modelu code-behind soubor definuje tří `SKPaint` obtažení šířku 100 a 20 a třetí pro naplnění objekty jako pole, dvě vytažení pomocí:

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    bool outlineThePath = false;

    SKPaint redThickStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 100
    };

    SKPaint redThinStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 20
    };

    SKPaint blueFill = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    public TapToOutlineThePathPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewTapped(object sender, EventArgs args)
    {
        outlineThePath ^= true;
        (sender as SKCanvasView).InvalidateSurface();
    }
    ...
}
```

Pokud nebyl byla klepnutí na obrazovce, `PaintSurface` obslužná rutina používá `blueFill` a `redThickStroke` vykreslení objektů k vykreslení kruhové cestě:

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circlePath = new SKPath())
        {
            circlePath.AddCircle(info.Width / 2, info.Height / 2,
                                 Math.Min(info.Width / 2, info.Height / 2) -
                                 redThickStroke.StrokeWidth);

            if (!outlineThePath)
            {
                canvas.DrawPath(circlePath, blueFill);
                canvas.DrawPath(circlePath, redThickStroke);
            }
            else
            {
                using (SKPath outlinePath = new SKPath())
                {
                    redThickStroke.GetFillPath(circlePath, outlinePath);

                    canvas.DrawPath(outlinePath, blueFill);
                    canvas.DrawPath(outlinePath, redThinStroke);
                }
            }
        }
    }
}
```

Kruh vyplněný a vytažený dle očekávání:

[![](effects-images/taptooutlinethepathnormal-small.png "Trojitá snímek obrazovky s normální stránce klepněte na osnovy Path")](effects-images/taptooutlinethepathnormal-large.png#lightbox "Trojitá snímek obrazovky s normální stránce klepněte na osnovy Path")

Když klepnete na obrazovce `outlineThePath` je nastavena na `true`a `PaintSurface` obslužná rutina vytvoří čerstvé `SKPath` objektu a, který používá jako cílovou cestu ve volání `GetFillPath` na `redThickStroke` Malování objektu. Této cílové cesty se pak naplní a vytažené `redThinStroke`výsledkem následující:

[![](effects-images/taptooutlinethepathoutlined-small.png "Trojitá snímek obrazovky s obrysy stránce klepněte na osnovy Path")](effects-images/taptooutlinethepathoutlined-large.png#lightbox "Trojitá snímek obrazovky stránky klepněte na osnovy Path osnovy")

Dva červené kroužky jasně označují, že původní cyklická cesta byla převedena na dvou cyklické rozvrhů.

Tato metoda může být velmi užitečná při vývoji cesty pro `SKPathEffect.Create1DPath` metody. Cesty, které zadáte v těchto metodách jsou vyplněny vždy při replikaci cesty. Pokud nechcete, aby celou cestu pro vyplnění, je nutné pečlivě definovat obrysy.

Například v **propojené řetězu** ukázky, odkazy byly definovány pomocí řady čtyři oblouky každý pár jsou založené na dvou poloměry pro vytváření obrysových oblasti cesty pro vyplnění. Je možné, nahraďte kód v `LinkedChainPage` třídě, aby provedl trochu jinak.

Nejprve bude potřeba znovu definovat `linkRadius` konstantní:

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

`linkPath` Je teď jenom dva oblouky podle jednoho okruhu, s požadovanou start úhly a úhlu oblouku:

```csharp
using (SKPath linkPath = new SKPath())
{
    SKRect rect = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
    linkPath.AddArc(rect, 55, 160);
    linkPath.AddArc(rect, 235, 160);

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = linkThickness;

        using (SKPath outlinePath = new SKPath())
        {
            strokePaint.GetFillPath(linkPath, outlinePath);

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(outlinePath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);

        }

    }
}
```

`outlinePath` Objekt je pak příjemce obrys `linkPath` při je vytažené vlastnostmi zadanými v `strokePaint`.

Dalším příkladem použití této techniky se chystá další pro cestu používané `SKPathEffect.Create2DPath` metody.

## <a name="combining-path-effects"></a>Kombinování efekty cest

Dvě poslední vytvoření statické metody `SKPathEffect` jsou [ `SKPathEffect.CreateSum` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateSum/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/) a [ `SKPathEffect.CreateCompose` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCompose/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/):

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

Obě tyto metody kombinovat dva efekty cest k vytvoření efektu složenou cestu. `CreateSum` Metoda vytvoří, který je podobné účinkům dvě cesty použít samostatně, mohou mít vliv cestu při `CreateCompose` použije jednu vliv cestu ( `inner`) a poté použije `outer` jménu.

Seznámili jste se už jak `GetFillPath` metoda `SKPaint` můžete převést jednu cestu na jinou cestu na základě `SKPaint` vlastnosti (včetně `PathEffect`) proto to nesmí být *příliš* záhadnými a často způsob, jakým `SKPaint`objekt dvakrát se účinky dvě cesty zadané v provádět tuto operaci `CreateSum` nebo `CreateCompose` metody.

Jedno ze zřejmých použití `CreateSum` je definování `SKPaint` objekt, který vyplní jednu cestu vliv cestu a tahy cestu s jinou vliv cestu. To je patrné **kočky v rámci** vzorku, který zobrazuje vlnkovatý hrany pole kočky v rámci:

[![](effects-images/catsinframe-small.png "Trojitá snímek obrazovky stránky kočky v rámečku")](effects-images/catsinframe-large.png#lightbox "Trojitá snímek obrazovky stránky kočky v rámečku")

[ `CatsInFramePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs) Třídy začíná tak, že definujete několik polí. Možná poznáte na první pole [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) třídy z [ **Data cesty SVG** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) článku. Druhý cesta je založená na řádku a oblouk vzoru svatojakubská rámce:

```csharp
public class CatsInFramePage : ContentPage
{
    // From PathDataCatPage.cs
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint catStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5
    };

    SKPath scallopPath =
        SKPath.ParseSvgPathData("M 0 0 L 50 0 A 60 60 0 0 1 -50 0 Z");

    SKPaint framePaint = new SKPaint
    {
        Color = SKColors.Black
    };
    ...
}
```

`catPath` Se daly použít v `SKPathEffect.Create2DPath` metoda Pokud `SKPaint` objekt `Style` je nastavena na `Stroke`. Nicméně pokud `catPath` slouží přímo v rámci tohoto programu, pak celé hlavní kočky bude vyplněno, a dokonce i whiskers nebudou viditelné. (Vyzkoušejte ji!) Je potřeba získat přehled této cestě a použít tuto obrysu v `SKPathEffect.Create2DPath` metody.

Konstruktor dělá tuto práci. Použije dvě transformace na `catPath` přesunout (0, 0) přejděte k centru a snížit velikost. `GetFillPath` Získá všechny osnovy profily v `outlinedCatPath`, a tento objekt se používá v `SKPathEffect.Create2DPath` volání. Škálování bere v úvahu nejen `SKMatrix` hodnota jsou o něco větší než vodorovné a svislé velikost cat poskytnout malou vyrovnávací paměť mezi dlaždice, kdy byly faktory překlad odvozené trochu empirických tak, aby byla viditelná v úplné cat levý horní roh rámce:

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    public CatsInFramePage()
    {
        Title = "Cats in Frame";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Move (0, 0) point to center of cat path
        catPath.Transform(SKMatrix.MakeTranslation(-240, -175));

        // Now catPath is 400 by 250
        // Scale it down to 160 by 100
        catPath.Transform(SKMatrix.MakeScale(0.40f, 0.40f));

        // Get the outlines of the contours of the cat path
        SKPath outlinedCatPath = new SKPath();
        catStroke.GetFillPath(catPath, outlinedCatPath);

        // Create a 2D path effect from those outlines
        SKPathEffect fillEffect = SKPathEffect.Create2DPath(
            new SKMatrix { ScaleX = 170, ScaleY = 110,
                           TransX = 75, TransY = 80,
                           Persp2 = 1 },
            outlinedCatPath);

        // Create a 1D path effect from the scallop path
        SKPathEffect strokeEffect =
            SKPathEffect.Create1DPath(scallopPath, 75, 0, SKPath1DPathEffectStyle.Rotate);

        // Set the sum the effects to frame paint
        framePaint.PathEffect = SKPathEffect.CreateSum(fillEffect, strokeEffect);
    }
    ...
}
```

Potom volá konstruktor `SKPathEffect.Create1DPath` vlnkovatý rámce. Všimněte si, že šířka cesty je 100 pixelů, ale zálohy je 75 pixelů tak, aby se replikovaná cesta je překrývajících se kolem rámečku. Poslední příkaz volá konstruktor `SKPathEffect.CreateSum` kombinovat účinky dvě cesty a nastavit na výsledek `SKPaint` objektu.

Umožňuje tuto práci `PaintSurface` obslužná rutina se má být úplně jednoduché. Je pouze potřeba definovat obdélník a vykreslete jej pomocí `framePaint`:

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect rect = new SKRect(50, 50, info.Width - 50, info.Height - 50);
        canvas.ClipRect(rect);
        canvas.DrawRect(rect, framePaint);
    }
}
```

Algoritmy za efekty cest vždy způsobit celé cesty používá pro vytažení nebo naplnění, který se má zobrazit, což může způsobit, že některé vizuály zobrazit mimo obdélníku. `ClipRect` Volat před verzí `DrawRect` volání umožňuje tyto vizuály se značně čisticího modulu. (Vyzkoušet bez oříznutí!)

Je běžné použití `SKPathEffect.CreateCompose` přidání některých zpoždění do jiného vliv cestu. Určitě můžete experimentovat sami, ale tady je příklad poněkud liší:

**Přerušované čáry šrafování** vyplní šrafování řádky, které jsou přerušovaná elipsu. Většina práce v [ `DashedHatchLinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) třídy se provádí přímo v definici pole. Tato pole definují efekt dash a mohou mít vliv šrafování. Jsou definovány jako `static` vzhledem k tomu, že se pak odkazuje v `SKPathEffect.CreateCompose` volání `SKPaint` definice:

```csharp
public class DashedHatchLinesPage : ContentPage
{
    static SKPathEffect dashEffect =
        SKPathEffect.CreateDash(new float[] { 30, 30 }, 0);

    static SKPathEffect hatchEffect = SKPathEffect.Create2DLine(20,
        Multiply(SKMatrix.MakeScale(60, 60),
                 SKMatrix.MakeRotationDegrees(45)));

    SKPaint paint = new SKPaint()
    {
        PathEffect = SKPathEffect.CreateCompose(dashEffect, hatchEffect),
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

`PaintSurface` Obslužná rutina třeba tak, aby obsahovala pouze standardní režijní náklady a volání `DrawOval`:

```csharp
public class DashedHatchLinesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawOval(info.Width / 2, info.Height / 2,
                        0.45f * info.Width, 0.45f * info.Height,
                        paint);
    }
    ...
}
```

Jak jsme už zjištěný, řádky šrafování nejsou přesně omezené na vnitřní oblasti a v tomto příkladu, jsou vždy začínají na levé straně celý pomlčkou:

[![](effects-images/dashedhatchlines-small.png "Trojitá snímek obrazovky stránky přerušované čáry šrafování")](effects-images/dashedhatchlines-large.png#lightbox "Trojitá snímek obrazovky stránky přerušované čáry šrafování")

Teď, když už víte, efekty cest, které v rozsahu od jednoduché tečky a čárky na neobvyklé kombinace použít vaši představivost a podívejte se, co můžete vytvořit.



## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
