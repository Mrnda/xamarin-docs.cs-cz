---
title: Cesta efekty
description: Zjištění různých cesta účinky, které umožňují cesty, které se použije pro vytažení a naplnění
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: charlespetzold
ms.author: chape
ms.date: 07/29/2017
ms.openlocfilehash: 4097aea4079555b26b586db5ec63fa261d5e7946
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="path-effects"></a>Cesta efekty

_Zjištění různých cesta účinky, které umožňují cesty, které se použije pro vytažení a naplnění_

A *efektu cesta* je instance [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) třídu, která je vytvořena s jedním osm statických `Create` metody. `SKPathEffect` Je pak nastavena [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) vlastnost `SKPaint` objekt pro škálu zajímavé důsledky, například vytažení čáry s malé replikované cesta:

![](effects-images/patheffectsample.png "Ukázka propojený řetězec")

Cesta důsledky umožňují:

- Obtažení čáru tečky a pomlčky
- Tahu čáry s jakoukoli vyplněný cestu
- Zadejte oblast šrafování řádků
- Zadejte oblast s cestou vedle sebe
- Ujistěte se, ostré rozích zaokrouhlené
- Přidejte náhodný "zmenší" čar a křivek

Kromě toho můžete kombinovat dvou nebo více cesta účinky.

Tento článek také ukazuje, jak používat `GetFillPath` metodu `SKPaint` převést jednu cestu do jiné cesty použitím vlastnosti `SKPaint`, včetně `StrokeWidth` a `PathEffect`. Výsledkem některé zajímavé techniky, jako je například získat cestu, která je přehledu z jiné cesty. `GetFillPath` je také užitečné souvislosti s cesta účinky.

## <a name="dots-and-dashes"></a>Tečky a pomlčky

Použití [ `PathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) metoda popsanou v článku [ **tečky a pomlčky**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md). První argument metody je pole obsahující sudý počet dvě nebo více hodnot, střídavě délek pomlčky a délek mezery mezi pomlček:

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

Tyto hodnoty jsou *není* vzhledem k šířce tahu. Například pokud šířku tahu je 10 a chcete řádek složený z odmocnina čárek a mezer odmocnina, nastavte `intervals` pole na {10, 10}. `phase` Argument určuje, kde v čárkovém vzoru začíná na řádku. V tomto příkladu, pokud chcete řádek pro začínat odmocnina mezery, nastavte `phase` do 10.

Konců pomlček se vztahuje `StrokeCap` vlastnost `SKPaint`. Pro celou tahu šířky, je velmi běžné tuto vlastnost nastavit na `SKStrokeCap.Round` má být zaokrouhleno konců pomlček. V tomto případě hodnoty `intervals` pole proveďte *není* zahrnout další délka výsledkem zaokrouhlení, což znamená, že cyklické tečku vyžaduje zadání šířku nula. Pro šířku tahu 10, k vytvoření řádek s cyklické tečky a mezery mezi body stejný průměr, použijte `intervals` pole {0, 20}.

**Animovaný s tečkami Text** je podobná stránce **uvedených Text** stránky, které jsou popsané v článku [ **integrace textu a obrázků** ](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) v že zobrazuje uvedených textových znaků nastavením `Style` vlastnost `SKPaint` do objektu `SKPaintStyle.Stroke`. Kromě toho **animovaný s tečkami Text** používá `SKPathEffect.CreateDash` umožnit to popisují desítkovém vzhled a také animuje program `phase` argument `SKPathEffect.CreateDash` metoda aby tečky se zdá, že cestují kolem textu znaky. Zde je stránka v režimu na šířku:

[![](effects-images/animateddottedtext-small.png "Trojitá snímek obrazovky stránky animovaný s tečkami Text")](effects-images/animateddottedtext-large.png#lightbox "Trojitá snímek obrazovky stránky animovaný s tečkami textu")

[ `AnimatedDottedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Třídy začne definováním některé konstanty a také přepsání `OnAppearing` a `OnDisappearing` metody pro animace:

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

`PaintSurface` Obslužná rutina začíná vytvořením `SKPaint` objekt, který chcete zobrazit text. `TextSize` Vlastnosti objektů je upravena podle šířku obrazovky:

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
            SKRect textBounds;
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

Na konci metody `SKPathEffect.CreateDash` metoda je volána, pomocí `dashArray` který je definován jako pole a animovaného `phase` hodnotu. `SKPathEffect` Instance je nastaven na `PathEffect` vlastnost `SKPaint` objekt, který chcete zobrazit text.

Alternativně můžete nastavit `SKPathEffect` do objektu `SKPaint` objekt před měření text a zarovnání na stránce. V takovém případě však animovaný tečky a pomlčky způsobit některé variace velikost vykresleného textu a text se obvykle zavibrovat trochu. (Zkuste to!)

Můžete si všimnout, jako kruh animovaný tečky kolem textových znaků, se určité míry v každé uzavřené křivky kde pravděpodobně bodů pop a deaktivovat existence. Toto je, kde Cesta, která definuje obrys znak zahájení a ukončení. Pokud délka cesty není násobkem délka v čárkovém vzoru (v tomto případě 20 pixelů) může obsahovat pouze část tohoto vzorce na konci cesty.

Je možné upravit délku v čárkovém vzoru podle délka cesty, ale který vyžaduje určení délka cesty, technik, které bude popsaná v budoucnu článku.

**Dot a pomlčka způsobů** program animuje v čárkovém vzoru sám sebe, tak, aby pomlčky zdá se, že k rozdělení na tečky, které se kombinují a pomlčky formulář znovu:

[![](effects-images/dotdashmorph-small.png "Trojitá snímek obrazovky stránky tečkou Dash způsobů")](effects-images/dotdashmorph-large.png#lightbox "Trojitá snímek obrazovky stránky způsobů Dash tečku")

[ `DotDashMorphPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Třídy přepsání `OnAppearing` a `OnDisappearing` metody stejně jako předchozí aplikace nebyla, ale definuje třídu `SKPaint` objektu jako pole:

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

`PaintSurface` Obslužná rutina vytvoří eliptické cestu na základě velikosti stránky a provede dlouhé části kódu, který nastaví `dashArray` a `phase` proměnné. Jako proměnnou animovaný `t` rozsahu od 0 do 1, `if` bloky rozdělit této doby do čtyř čtvrtletí a v každé z těchto čtvrtletí `tsub` také rozsah od 0 do 1. Na konci velmi, program vytvoří `SKPathEffect` a nastaví na `SKPaint` objekt pro kreslení.

## <a name="from-path-to-path"></a>Z cesty k cestě

[ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) Metodu `SKPaint` změní jednu cestu do jiné na základě nastavení v `SKPaint` objektu. Pokud chcete zobrazit, jak to funguje, nahraďte `canvas.DrawPath` volání v předchozí program následujícím kódem:

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

V tento nový kód `GetFillPath` volání převede `ellipsePath` (což je právě oval) do `newPath`, který se následně zobrazí s `newPaint`. `newPaint` Vytvoření objektu se všemi možnými nastavení výchozí vlastnost s tím rozdílem, že `Style` vlastnost nastavena na základě na logická hodnota vrácená hodnota z `GetFillPath`.

Vizuálech jsou identické s výjimkou barvu, jež je nastavena v `ellipsePaint` ale ne `newPaint`. Místo jednoduché elipsy definované v `ellipsePath`, `newPath` obsahuje mnoho rozvrhy cesty definující řadu tečky a pomlčky. Toto je výsledek použití různé vlastnosti `ellipsePaint` – `StrokeWidth`, `StrokeCap`, a `PathEffect` – k `ellipsePath` a uvedení výsledné cestu v `newPath`. `GetFillPath` Metoda vrátí logickou hodnotu, která určuje zda je v cílové cestě se v; v tomto příkladu je návratovou hodnotu `true` pro naplnění cestu.

Zkuste změnit `Style` nastavení v `newPaint` k `SKPaintStyle.Stroke` a uvidíte jednotlivé cesty obrysy uvedených šířka v pixelech jeden řádek.

## <a name="stroking-with-a-path"></a>Vytažení s cestou

[ `SKPathEffect.Create1DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create1DPath/p/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPath1DPathEffectStyle/) Metoda se koncepčně podobá `SKPathEffect.CreateDash` s tím rozdílem, že zadáte cestu, nikoli vzor čárek a mezer. Tato cesta se replikují vícekrát tah řádek nebo křivky.

Syntaxe je následující:

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

> [!IMPORTANT]
> Sledujte: je přetížení `Create1DPath` který je definován s parametrem – výčet typu `SkPath1DPathEffect` s jedno malé písmeno, k." Tento název se o chybu a v důsledku toho je nesprávný, výčet a metoda jsou zastaralé, ale je velmi snadné nepoužívané metodu stane součástí kódu a je obtížné zjistit, jaká přesně.

Obecně platí, cesta, která je předat do `Create1DPath` bude malé a zarovnaný kolem bodu (0, 0). `advance` Určuje vzdálenost od centra cesty jako cesty se replikují na řádku. Tento argument se obvykle nastavili na přibližnou šířku cesty. `phase` Argument plní na stejný atribut role v tomto poli jak nemá v `CreateDash` metoda.

[ `SKPath1DPathEffectStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath1DPathEffectStyle/) Má tři členy:

- [`Translate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Translate/)
- [`Rotate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Rotate/)
- [`Morph`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Morph/)

`Translate` Člen způsobí, že cesta k zůstat v orientaci stejné, jako je replikované podle řádku nebo křivky. Pro `Rotate`, cesta otočen podle tangens na křivku. Cesta obsahuje jeho normální orientaci pro vodorovné čáry. `Morph` je podobná `Rotate` s tím rozdílem, že samotná cesta je také zakřivené tak, aby odpovídaly zakřivení řádku probíhá vytažený.

**Efektu cesta 1 D** stránky ukazuje tyto tři možnosti. [ **OneDimensionalPathEffectPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) soubor definuje ovládací prvek obsahující tři položky odpovídající tři členy výčtu výběr:

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

[ **OneDimensionalPathEffectPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) souboru kódu na pozadí definuje tři `SKPathEffect` objekty jako pole. Tyto soubory jsou všechny vytvořeny pomocí `SKPathEffect.Create1DPath` s `SKPath` objekty vytvořené pomocí `SKPath.ParseSvgPathData`. První je jednoduché pole, obrazce Kosočtverec je druhý a třetí je obdélníku. Ty se používají k předvedení styly tři vliv:

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

`PaintSurface` Obslužná rutina vytvoří Bézierovy křivky, který kolem sám v cyklu a přistupuje k výběru zjistíte, které `PathEffect` by měl být použité k obtažení ho. Tři možnosti – `Translate`, `Rotate`, a `Morph` – jsou uvedeny zleva doprava:

[![](effects-images/1dpatheffect-small.png "Trojitá snímek obrazovky stránky efektu cesta 1D")](effects-images/1dpatheffect-large.png#lightbox "Trojitá snímek obrazovky stránky efektu cesta 1 D")

Cesta zadaná v `SKPathEffect.Create1DPath` metoda je vždy vyplněna. Cesta zadaná v `DrawPath` metoda je vždy vytažený, pokud `SKPaint` objekt má jeho `PathEffect` vlastnost nastavena na hodnotu efekt cesta 1 D. Všimněte si, že `pathPaint` objekt nemá žádné `Style` normálně výchozí nastavení pro `Fill`, ale cesta je vytažený bez ohledu na to.

Pole použité v `Translate` příklad je 20 pixelů odmocnina a `advance` argument je nastaven na hodnotu 24. Tento rozdíl způsobí, že mezera mezi polí při řádek je přibližně vodorovné nebo svislé, ale do polí překrývat trochu při řádek je diagonálních, protože diagonálních pole je 28.3 pixelů. 

Kosočtverec tvaru v `Rotate` příklad je také 20 pixelů. `advance` Nastavena na 20, aby body nadále touch jako kosočtverec otáčí společně s zakřivení čáry.

Tvar rámečku v `Morph` příklad je 50 pixelů s `advance` nastavení 55, aby malá mezera mezi obdélníky, jako jsou ohnuty kolem Bézierovy křivky.

Pokud `advance` argument je menší než velikost cesty, pak může dojít k překrytí replikované cesty. Výsledkem může být zajímavých efektů. **Propojené řetězu** stránka se zobrazuje řada překrývajících se oblastí kroužky, které pravděpodobně tak, aby připomínaly propojené řetězec, který je přestane reagovat v rozlišovací tvar trolejového vedení:

[![](effects-images/linkedchain-small.png "Trojitá snímek obrazovky stránky propojené řetězu")](effects-images/linkedchain-large.png#lightbox "Trojitá snímek obrazovky stránky propojený řetězec")

Podívejte se velmi zavřít a uvidíte, že těch, které nejsou ve skutečnosti kroužky. Každé propojení v řetězci je dva oblouky, velikosti a umístěný, takže se pro připojení s sousedících odkazy.

Řetězec nebo kabel distribuce váhy uniform přestane reagovat ve formě trolejového vedení. Architektura vytvořené ve formě obráceným trolejového vedení výhody z stejným rozložením přetížení z váhu architektura. Trolejového vedení má zdánlivě jednoduchý matematickém Popis:

y = · COSH(x / a)

*Cosh* hyperbolický kosinus funkcí. Pro *x* rovná 0, *cosh* rovná nule a *y* rovná *a*. To je center trolejového. Podobně jako *kosinus* funkce, *cosh* se říká, že *i*, to znamená, že *cosh(–x)* rovná *cosh(x)*, a hodnoty zvyšují pro zvýšení kladné a záporné argumenty. Tyto hodnoty popisují křivek, které vytvářejí postranní trolejového.

Hledání správnou hodnotu *a* podle trolejového vedení dimenzím, na stránce telefonu není přímé výpočtu. Pokud *w* a *h* jsou šířky a výšky obdélníku, optimální hodnotu *a* splňuje následující rovnice:

COSH (w/2/a) = 1 + h / a

Následující metodu v [ `LinkedChainPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) třída zahrnuje tento rovnosti tím, že odkazuje na dvou výrazů vlevo a vpravo od rovná jako `left` a `right`. Pro malé hodnoty *a*, `left` je větší než `right`; pro velké hodnoty *a*, `left` je menší než `right`. `while` Smyčky zúží v na optimální hodnoty *a*:

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

`SKPath` Objektu pro odkazy. je vytvořený v konstruktoru třídy a výsledné `SKPathEffect` je pak nastavena `PathEffect` vlastnost `SKPaint` objekt, který je uložený jako pole:

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

Hlavní úloha `PaintSurface` obslužná rutina je vytvořit cestu pro trolejového vedení sám sebe. Po určení optimálním *a* a uložením do `optA` proměnné, je také nutné vypočítat posun z horní části okna. Potom ji hromadit kolekce `SKPoint` hodnoty pro trolejovém vedení, který zapnout na cestu a kreslení cestu s dříve vytvořenou `SKPaint` objektu:

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

Tento program definuje cestu použitou v `Create1DPath` tak, aby měl jeho (0, 0) přejděte v centru. Vypadá to přiměřené protože (0, 0) bodu cesty je zarovnáno s řádek nebo křivky, který je adorning. Ale můžete použít jiných-zarovnaný na střed (0, 0) bodu pro zvláštní efekty.

**Běžícím pásu** stránky vytvoří cestu podlouhlá běžícím pásu s zakřivené horní a dolní to je velikost okna rozměrům tvaru. Tato cesta je vytažené jednoduchou `SKPaint` objektu 20 pixelů a barevnou šedé a pak vytažený znovu s jinou `SKPaint` objektu s `SKPathEffect` objekt odkazující na cestu tvaru malé sady:

[![](effects-images/conveyorbelt-small.png "Trojitá snímek obrazovky stránky běžícím pásu")](effects-images/conveyorbelt-large.png#lightbox "Trojitá snímek obrazovky stránky běžícím pásu")

(0, 0) bod sady cesty je popisovač, takže pokud `phase` je animovaný argument, kbelíků se zdá, že základem běžícím pásu, případně vybírání rozsahu adres až horních dole a vypsání ho v horní části.

[ `ConveyorBeltPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs) Třída implementuje animace s přepsáními `OnAppearing` a `OnDisappearing` metody. Cesta v bloku je definováno v konstruktoru stránky:

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

Kód pro vytvoření sady dokončení s dvěma transformace, které se ujistěte se o něco větší sady a zapnout ho ze strany. Používání těchto transformací byla jednodušší než úpravě všechny souřadnice v předchozí kód. 

`PaintSurface` Obslužná rutina začne definováním cestu pro běžícím pásu sám sebe. Toto je jednoduše pár řádků a pár zadáte kroužky, které jsou vykreslovány s 20 pixelů širokou světlý šedá řádek:

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

Logika pro kreslení běžícím pásu nepracuje v režimu na šířku.

Kbelíků by měl rozmístěny o 200 pixelů na běžícím pásu od sebe. Dopravní pás je však pravděpodobně není násobkem dlouhý a 200 pixelů, což znamená, jako `phase` argument `SKPathEffect.Create1DPath` je animovaný, kbelíků bude pop, do a z existence. 

Z tohoto důvodu program nejprve vypočítá hodnotu s názvem `length` tedy délka běžícím pásu. Protože běžícím pásu se skládá z přímky a zadáte kroužky, jedná se o jednoduchý výpočet. Dále je počet intervalů, vypočítá jako podíl `length` podle 200. To se zaokrouhlí na nejbližší celé číslo, a pak je číslo rozdělené do `length`. Výsledkem je mezery pro integrální počet intervalů. `phase` Argument je jednoduše zlomek této.

## <a name="from-path-to-path-again"></a>Z cesty k cestě znovu

V dolní části `DrawSurface` obslužné rutiny v **běžícím pásu**, komentář `canvas.DrawPath` volání a nahraďte ji následujícím kódem:

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

Stejně jako u předchozí příklad `GetFillPath`, uvidíte, že výsledky jsou stejné s výjimkou barvu. Po provedení `GetFillPath`, `newPath` objekt obsahuje více kopií cesta sady, každý umístěn ve stejné přímé, že animaci je umístěný v čase volání.

## <a name="hatching-an-area"></a>Šrafování oblast

[ `SKPathEffect.Create2DLines` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DLine/p/System.Single/SkiaSharp.SKMatrix/) Metoda vyplní celé oblasti s paralelní řádky, které se často nazývá *šrafovaných řádky*. Metoda má následující syntaxi:

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

`width` Argument určuje šířku tahu čar šrafování. `matrix` Parametr je kombinací otočení škálování a volitelné. Měřítko určuje přírůstek pixelů, který Skia používá k mezery mezi řádky šrafování. Oddělení mezi řádky je měřítko minus `width` argument. Pokud na škálování faktor je menší než nebo rovno `width` hodnotu, bude bez mezery mezi řádky šrafování a aby byla vyplněna se zobrazí v oblasti. Zadejte stejnou hodnotu pro vodorovného a svislého škálování. 

Šrafování řádky jsou ve výchozím nastavení, vodorovné. Pokud `matrix` parametr obsahuje otočení, řádky šrafování otáčejí po směru hodinových ručiček.

**Šrafování výplně** stránky ukazuje platnost této cesty. [ `HatchFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs) Třída definuje tři důsledky cestu jako pole, první pro vodorovné šrafování řádky s šířka 3 pixelů se škálování označujícím Multi-Factor, které jsou rozmístěny 6 pixelů od sebe. Oddělení mezi řádky je proto 3 pixelů. Druhý efektu cesta je pro vertikální šrafování řádky s šířku 6 pixelů rozmístěny 24 pixelů od sebe (takže oddělení je 18 pixelů), a třetí je diagonálních šrafování řádků 12 pixelů celý rozmístěné 36 pixelů od sebe. 

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
        SKMatrix target;
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

Všimněte si matice `Multiply` metoda. Protože vodorovného a svislého škálování faktory jsou stejné, není důležité pořadí, ve kterém se násobí změny velikosti a oběh matice.

`PaintSurface` Obslužná rutina používá tyto tři cesta efekty pomocí tří různých barev v kombinaci s `fillPaint` k vyplnění zaoblený obdélník velikost, aby se vešel na stránku. `Style` Vlastnost nastavte u `fillPaint` je ignorován; Pokud `SKPaint` objekt zahrnuje efekt cesta vytvořené z `SKPathEffect.Create2DLine`, oblast se vyplní bez ohledu na to:

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

Pokud jste pečlivě si prohlédněte výsledky, uvidíte, že řádky červená a modrá šrafování nejsou omezen přesněji na Zaoblený obdélník. (To je zjevně vlastnosti Skia kódu.) Pokud nevyhovující, zobrazí se alternativní způsob řádků diagonálních šrafování zeleně: zaoblený obdélník slouží jako cestu výstřižek a šrafování řádky jsou vykreslovány na celou stránku.

`PaintSurface` Obslužná rutina se ukončí pomocí volání jednoduše obtažení zaokrouhlené obdélníku, abyste viděli nesoulad mezi databází červená a modrá šrafování řádků:

[![](effects-images/hatchfill-small.png "Trojitá snímek obrazovky stránky šrafování výplně")](effects-images/hatchfill-large.png#lightbox "Trojitá snímek obrazovky stránky šrafování výplně")

Android obrazovky nevypadá skutečně jako je například: škálování na snímku obrazovky způsobila dynamické red čar a dynamické konsolidovat do zdánlivě širší červené čáry a širší mezer.

## <a name="filling-with-a-path"></a>Vyplnění pomocí cesty

[ `SKPathEffect.Create2DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DPath/p/SkiaSharp.SKMatrix/SkiaSharp.SKPath/) Umožňuje vyplníte oblast na cestu, která se replikují vodorovně a svisle, výsledkem bude dlaždice oblasti:

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

`SKMatrix` Škálování faktory znamenat vodorovného a svislého mezery replikované cesty. Ale nemůže otočení cesty pomocí této `matrix` argument; Pokud chcete, aby cesta otáčet, otočit samotná cesta pomocí `Transform` metoda definované `SKPath`. 

Replikovaná složka je obvykle zarovnán levého a horního okraje obrazovky, nikoli oblasti má číslo. Toto chování můžete přepsat zadáním faktory překlad mezi 0 a škálování faktorů k určení vodorovného a svislého posunutí z stran levého a horního.

**Vyplnění dlaždice cesta** stránky ukazuje platnost této cesty. Cesty používanou pro dlaždice oblasti je definován jako pole v [ `PathFileFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) třídy. Souřadnice vodorovného a svislého rozsahu od –40 na 40, což znamená, že tato cesta je 80 pixelů odmocnina: 

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

V `PaintSurface` obslužnou rutinu, `SKPathEffect.Create2DPath` volání nastaví mezery vodorovného a svislého 64 způsobí odmocnina dlaždice 80 pixelů překrytí. Naštěstí cesta se podobá stavebnice část, výborně meshing s přiléhající dlaždice:

[![](effects-images/pathtilefill-small.png "Trojitá snímek obrazovky stránky zadejte cestu dlaždice")](effects-images/pathtilefill-large.png#lightbox "Trojitá snímek obrazovky stránky zadejte cestu dlaždice")

Škálování z původní snímek způsobí, že některé narušení, zvláště na obrazovce Android.

Všimněte si, že tato dlaždice vždy zobrazovat celou a nikdy se zkrátí. S výjimkou na obrazovce Windows 10 Mobile není i zřejmé, že oblast má číslo je zaoblený obdélník. Pokud chcete, aby došlo ke zkrácení tyto dlaždice do konkrétní oblasti, použijte cestu výstřižek.

Zkuste nastavení `Style` vlastnost `SKPaint` do objektu `Stroke`, a uvidíte jednotlivé dlaždice uvedených než vyplněna.

## <a name="rounding-sharp-corners"></a>Zaokrouhlení Sharp rozích

**Zaokrouhlené pro sedmiúhelník** program uvedené v [ **tři způsoby nakreslit oblouk** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) článku použít tečný oblouk na křivku body Obrázek dialogového okna sedm. **Pro jiné zaokrouhlené sedmiúhelník** na stránce se zobrazí mnohem snazší přístup, který používá efekt cesta vytvořené z [ `SKPathEffect.CreateCorner` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCorner/p/System.Single/) metoda:

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

I když je s názvem jeden argument `radius` ho musíte nastavit poloviční požadované rohu protokolu RADIUS. (Toto je typické pro Skia kódu).

Tady je `PaintSurface` obslužné rutiny v [ `AnotherRoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) třídy:

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

Můžete použít tento efekt vytažení nebo naplnění na základě `Style` vlastnost `SKPaint` objektu. Tady je na všech tří platformách:

[![](effects-images/anotherroundedheptagon-small.png "Trojitá snímek obrazovky stránky pro jiné zaokrouhlené sedmiúhelník")](effects-images/anotherroundedheptagon-large.png#lightbox "Trojitá snímek obrazovky stránky pro jiné zaokrouhlené sedmiúhelník")

Uvidíte, že tento zaokrouhlené pro sedmiúhelník je stejný jako starší programu. Pokud potřebujete další přesvědčit poloměr je skutečně 100 spíše než 50 zadaný v `SKPathEffect.CreateCorner` volání, která vám může zrušte komentář u poslední příkaz v programu a najdete kruh 100 radius přes rohu.

## <a name="random-jitter"></a>Náhodné zpoždění

Někdy bezchybné přímky grafiky počítače nejsou poměrně co chcete použít, a trochu náhodnost se požaduje. V takovém případě budete chtít zkuste [ `SKPathEffect.CreateDiscrete` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDiscrete/p/System.Single/System.Single/System.UInt32/) metoda:

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

Platnost této cesty můžete použít pro vytažení nebo naplnění. Řádky jsou rozdělené do připojené segmenty – přibližnou délka je zadána `segLength` – a rozšířit v různých směrech. Je zadána v rozsahu odchylky z původního řádku `deviation`. 

Konečný argument je základní hodnota používá ke generování pseudonáhodného pořadí používá pro účinek. Účinek kolísání bude vypadat pro různé semena mírně liší. Argument má výchozí hodnotu nula, což znamená, že stejný účinek se při každém spuštění programu. Pokud chcete jiné kolísání vždy, když je překreslen na obrazovce, můžete nastavit počáteční hodnotu na `Millisecond` vlastnost `DataTime.Now` hodnoty (například).


**Zmenší se Experiment** stránce můžete experimentovat s různými hodnotami v vytažení obdélníku:

[![](effects-images/jitterexperiment-small.png "Trojitá snímek obrazovky stránky zmenší experimentu")](effects-images/jitterexperiment-large.png#lightbox "Triple screenshot of the JitterExperiment page")

Tento program je straightfoward. [ **JitterExperimentPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) soubor vytvoří dvě instance `Slider` elementy a `SKCanvasView`:

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

`PaintSurface` Obslužné rutiny v [ **JitterExperimentPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) souboru kódu na pozadí se nazývá vždy, když `Slider` hodnotu změny. Zavolá `SKPathEffect.CreateDiscrete` použití dvou `Slider` hodnoty a použije ho k obtažení obdélníku:

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

Tomu můžete použít pro naplnění také, v takovém případě obrys oblasti vyplněný podléhá tyto odchylky náhodné. **Zmenší Text** stránky ukazuje, jak pomocí efektu tato cesta k zobrazení textu. Většina kód `PaintSurface` obslužnou rutinu [ `JitterTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) třída připadá na velikost a zarovnání textu:

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
        SKRect textBounds;
        textPaint.MeasureText(text, ref textBounds);

        // Calculate offsets to center the text on the screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

Zde je spuštěna v režimu na šířku na všech tří platformách:

[![](effects-images/jittertext-small.png "Trojitá snímek obrazovky stránky zmenší Text")](effects-images/jittertext-large.png#lightbox "Triple screenshot of the JitterText page")

## <a name="path-outlining"></a>Osnova cesta

Už jste viděli dva málo příklady [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) metodu `SKPaint`, který existuje také v [přetížení](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/):

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale)
```

Pouze první dva argumenty jsou povinné. Metoda přistupuje k cesta odkazuje `src` argument, upraví data cesty na základě vlastností tahu v `SKPaint` objektu (včetně `PathEffect` vlastnost) a pak zapíše výsledky do `dst` cesta. `resScale` Parametr umožňuje upravit přesnost vytvořit menší cílovou cestu a `cullRect` argument může eliminovat rozvrhy mimo obdélníku.

Jeden základní použití této metody nezahrnuje cesta důsledky vůbec. Pokud `SKPaint` objekt má jeho `Style` vlastnost nastavena na hodnotu `SKPaintStyle.Stroke`a nemá *není* mít jeho `PathEffect` nastavit, pak `GetFillPath` vytváří cestu, která představuje *outline*zdrojové cesty jako kdyby měl byla vytažený vlastnostmi Malování.

Například pokud `src` cesta je jednoduchý kruh poloměru 500 a `SKPaint` objektu určuje šířku tahu 100, pak se `dst` dvou soustředných kroužky, jeden s radius 450 a dalších se serverem radius 550 stane se cesta. Volání metody `GetFillPath` protože naplňování to `dst` cesta je stejný jako vytažení `src` cesta. Ale můžete také obtažení `dst` cesta zobrazíte obrysy cesty.

**Klepněte sem a Outline cesta** ukazuje to. `SKCanvasView` a `TapGestureRecognizer` instance v [ **TapToOutlineThePathPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) souboru. [ **TapToOutlineThePathPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) souboru kódu na pozadí definuje tři `SKPaint` objekty jako polí a dva jsou pro vytažení s obtažení šířky 100 a 20 a třetí pro naplnění:

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

Pokud nebyl byla stisknuté na obrazovce, `PaintSurface` obslužná rutina používá `blueFill` a `redThickStroke` malovat pro vykreslení cyklická cesta:

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

Kruhu je vyplněno a vytažený podle předpokladů:

[![](effects-images/taptooutlinethepathnormal-small.png "Trojitá snímek obrazovky stránky normální klepněte na obrys Path")](effects-images/taptooutlinethepathnormal-large.png#lightbox "Trojitá snímek obrazovky normální klepněte na obrys Path stránky")

Když klepnete na obrazovce `outlineThePath` je nastaven na `true`a `PaintSurface` obslužná rutina vytvoří čerstvou `SKPath` objektu a použije tento jako cílová cesta ve volání `GetFillPath` na `redThickStroke` Malování objektu. Že cílová cesta se pak vyplněno a vytažené `redThinStroke`, což je následující:

[![](effects-images/taptooutlinethepathoutlined-small.png "Trojitá snímek obrazovky stránky popsané klepněte na obrys Path")](effects-images/taptooutlinethepathoutlined-large.png#lightbox "Trojitá snímek obrazovky popsané klepněte na obrys Path stránky")

Na dva červeném kroužku jasně označuje, že původní cyklická cesta byl převeden do dvou cyklické rozvrhů.

Tato metoda může být velmi užitečná při vývoji cesty pro `SKPathEffect.Create1DPath` metoda. Cesty, které zadáte v těchto metod jsou vyplněny vždy při replikaci cesty. Pokud nechcete, aby byla vyplněna celou cestu, je nutné zadat pečlivě obrysy.

Například v **propojené řetězu** ukázku, odkazy, které byly definovány s řadou čtyři oblouky, každý pár jsou založené na dva poloměr se vymezí oblasti cesty, aby byla vyplněna. Je možné nahraďte kód v `LinkedChainPage` třída uděláte se může lišit.

Nejprve budete chtít znovu definovat `linkRadius` konstantní:

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

`linkPath` Je teď právě dva oblouky založené na jednom okruhu, s požadovanou spuštění úhly a oblouku úhly:

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

`outlinePath` Objekt je pak příjemce obrys `linkPath` při, jsou-li vytažené zadány ve vlastnosti `strokePaint`. 

Další příklad touto technikou na blížící se další pro cestu použitou v `SKPathEffect.Create2DPath` metody. 

## <a name="combining-path-effects"></a>Kombinování cesta efekty

Tyto dvě metody konečné statické vytvoření z `SKPathEffect` jsou [ `SKPathEffect.CreateSum` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateSum/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/) a [ `SKPathEffect.CreateCompose` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCompose/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/):

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

Obě tyto metody kombinovat dvě cesty důsledky vytvořit efekt složené cesty. `CreateSum` Metoda vytvoří efekt cesta, který je podobný vliv dvě cesty použít samostatně, při `CreateCompose` použije jednu efekt cesta ( `inner`) a poté použije `outer` na který.

Už jste viděli jak `GetFillPath` metodu `SKPaint` můžete převést jednu cestu do jiné cesty na základě `SKPaint` vlastnosti (včetně `PathEffect`), neměl by být *příliš* Záhadné, jak `SKPaint`objekt tuto operaci mohou provádět dvakrát se dvě cesty účinky podle `CreateSum` nebo `CreateCompose` metody.

Jedno zřejmé použití `CreateSum` je definovat `SKPaint` objekt, který vyplní cestu s jednu cestu účinek a tahy cestu s jinou cestu vliv. Tento postup je znázorněn v **kočky rámce** vzorku, který zobrazí pole kočky v rámci s vlnkovatý okraje:

[![](effects-images/catsinframe-small.png "Trojitá snímek obrazovky stránky kočky v rámečku")](effects-images/catsinframe-large.png#lightbox "Trojitá snímek obrazovky stránky kočky v rámečku")

[ `CatsInFramePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs) Třída začne definováním několik polí. Může rozpoznat první pole z [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) třídy z [ **Data cesty SVG** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) článku. Druhý cesta je založená na řádku a oblouk pro vzor svatojakubská rámečku:

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

`catPath` Může v `SKPathEffect.Create2DPath` metoda Pokud `SKPaint` objekt `Style` je nastavena na `Stroke`. Ale pokud `catPath` slouží přímo v tomto programu pak celý vedoucí kočky bude, a i vousů nebudou viditelné. (Zkuste to!) Je nutné získat obrys této cestě a používat tento obrysu v `SKPathEffect.Create2DPath` metoda.

Konstruktor nepodporuje tuto úlohu. Nejprve se vztahují dvě transformace na `catPath` přesunout (0, 0) přejděte do centra a snižovat velikost. `GetFillPath` Získá všechny osnovy obrysu ve `outlinedCatPath`, a tento objekt se používá v `SKPathEffect.Create2DPath` volání. Měřítko faktory pro `SKMatrix` hodnotu jsou o něco větší než ve vodorovném a svislém velikost cat zajistit málo vyrovnávací paměti mezi dlaždice, době, kdy byly faktory překlad odvozené poněkud empirically tak, aby se zobrazí na úplné cat levého horního rohu rámečku:

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

Pak zavolá konstruktoru `SKPathEffect.Create1DPath` pro vlnkovatý rámečku. Všimněte si, že šířku cesty je 100 pixelů, ale zálohy je 75 pixelů, aby replikované cesta je překryté kolem rámečku. Poslední příkaz volání konstruktoru `SKPathEffect.CreateSum` kombinovat důsledky dvě cesty a nastavit výsledek na `SKPaint` objektu.

Tento pracovní umožňuje `PaintSurface` obslužná rutina se úplně jednoduché. Pouze musí se definovat obdélníku a kreslení pomocí `framePaint`:

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

Algoritmy za důsledky cestu vždy způsobit celé cesty používanou pro vytažení nebo naplnění, který se má zobrazit, což může způsobit, že některé vizuální prvky se objeví mimo rámeček. `ClipRect` Volání před verzí `DrawRect` volání umožňuje vizuály být podstatně čisticí. (Vyzkoušet bez výstřižek!)

Je běžné použití `SKPathEffect.CreateCompose` přidat některé kolísání do jiného efektu cestu. Určitě můžete vyzkoušet sami, ale tady je poněkud jiný příklad:

**Přerušované čáry šrafování** doplní elipsy šrafování řádků, které jsou přerušovaná čára. Nejvíce práce v [ `DashedHatchLinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) třída provádí přímo do definice pole. Tato pole definovat efekt dash a šrafování vliv. Jsou definovány jako `static` vzhledem k tomu, že se pak odkazuje v `SKPathEffect.CreateCompose` volání v `SKPaint` definice:

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
        SKMatrix target;
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

`PaintSurface` Potřeba obslužná rutina obsahovat pouze standardní režijní náklady a jednoho volání `DrawOval`:

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

Jak jsme už zjištěný, řádky šrafování nejsou přesněji omezen na uvnitř oblasti a v tomto příkladu, bylo vždycky počítač v levém celou pomlčkou:

[![](effects-images/dashedhatchlines-small.png "Trojitá snímek obrazovky stránky přerušovanou řádky šrafování")](effects-images/dashedhatchlines-large.png#lightbox "Trojitá snímek obrazovky stránky přerušovanou šrafování řádky")

Teď, když jste viděli účinky cesty, které v rozsahu od jednoduchého tečky a pomlčky na neobvyklé kombinace, použijte vaši představivost a najdete, co můžete vytvořit.



## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
