---
title: 3D otáčení v ve Skiasharpu
description: Tento článek vysvětluje, jak používat neafinní transformace otočení 2D objekty v 3D prostoru a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 84ebdd007d17eaf0bcfc1be119cb4130299503bc
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615662"
---
# <a name="3d-rotations-in-skiasharp"></a>3D otáčení v ve Skiasharpu

_Pomocí neafinní transformace otočení 2D objekty v 3D prostoru._

Běžné použití neafinní transformace je budete jen simulovat otočení 2D objektu v 3D prostoru:

![](3d-rotation-images/3drotationsexample.png "Otočit textového řetězce v 3D prostoru")

Tato práce spočívá ve práce s trojrozměrné rotace a odvozování neafinní `SKMatrix` transformaci, která provádí tyto 3D otáčení.

Je obtížné rozvíjet to `SKMatrix` transformace funguje pouze v rámci dvou dimenzí. Úlohy se změní mnohem jednodušší, pokud tato matice 3 3 je odvozen z matice 4 4 používaných pro 3D grafiky. Zahrnuje ve Skiasharpu [ `SKMatrix44` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.PreConcat/p/SkiaSharp.SKMatrix44/) třídu pro tento účel, ale některé na pozadí v 3D grafiky, je nutný pro pochopení 3D otáčení a 4 4 transformační matice.

Trojrozměrného souřadnicový systém přidá třetí osy Z. koncepčně volá, je na ose Z kolmo na obrazovku. Souřadnice bodů v 3D prostoru jsou označeny pomocí tří čísel: (x, y, z). V 3D jsou souřadnicový systém použité v tomto článku, zvýšení hodnoty X napravo a zvýšení hodnoty Y, přestanou fungovat, stejně jako u dvou dimenzí. Rostoucí kladné Z hodnoty pocházejí z obrazovky. Levém horním rohu, stejně jako u 2D grafika je zdrojem. Na obrazovce můžete představit jako rovině XY s osy v pravém úhlu tento rovinou.

Tomu se říká vlevo systém souřadnic. Pokud bod ukazováčkem pro vaše levém ve směru kladná X souřadnice (napravo) a střední prstu ve směru Y zvýšení koordinuje (dolů), pak vaše thumb body v směr zvyšované souřadnice Z – z rozšíření navýšení kapacity na obrazovce.

V 3D grafiky transformací podle matice 4 4. Tady je 4 4 jednotkovou matici:

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

Při práci s matice 4 4, je vhodné určit buňky s příslušnými čísly řádků a sloupců:

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

Nicméně SkiaSharp `Matrix44` třídy se mírně liší. Jediný způsob, jak nastavit nebo získat hodnoty jednotlivých buněk `SKMatrix44` pomocí [ `Item` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Item/p/System.Int32/System.Int32/) indexeru. Indexy řádků a sloupců jsou založený na nule, spíše než založen na jedničce, a jsou přehozeny řádků a sloupců. Buňka M14 v diagramu výše se přistupuje pomocí indexeru `[3, 0]` v `SKMatrix44` objektu.

V systému 3D grafiky 3D bod (x, y, z) je převedena na 1 4 matice pro vynásobí matice 4 4 transformace:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Obdobná 2D transformace které se provádějí v trojrozměrném, 3D transformace se předpokládá, že proběhne do čtyř dimenzí. Čtvrtý dimenze se označuje jako W a 3D prostoru se předpokládá, že existují v rámci 4D místa, kde jsou souřadnice W rovnající se 1. Transformace vzorce jsou následující:

x! = M11·x M21·y + M31·z + M41

y' = M12·x M22·y + M32·z + M42

z' = M13·x + M23·y + M33·z + M43

w "= M14·x M24·y + M34·z + M44

Je zřejmé z vzorce transformace, která buňky `M11`, `M22`, `M33` faktory jsou škálování ve směru X, Y a Z a `M41`, `M42`, a `M43` jsou překlad faktory X, Y a Z směry.

Převést tyto souřadnice 3D prostoru, W rovná hodnotě 1, x ", y", a z "souřadnice jsou všechny dělený w":

x"= x' / w"

y"= y' / w"

z"= z" / w "

w"= w" / w "= 1

Tento dělení w "poskytuje perspektivou v 3D prostoru. Pokud w "se rovná 1, dojde k žádné perspektivy.

Rotace v 3D prostoru může být poměrně složité, ale jsou nejjednodušší otočení kolem osy X, Y a Z. Otočení kolem osy X úhlu α je tato matice:

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

Hodnoty X nemění, když podroben Tato transformace. Otočení kolem osy Y opustí hodnoty Y beze změny:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Otočení kolem osy Z je stejné jako 2D grafika:

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

Směru odvozené od uchopení pera souřadnicovém systému. Toto je levou systému, tak pokud bod thumb vaše levém při zvyšování hodnoty pro konkrétní osu – doprava pro rotaci kolem osy X, dolů pro rotaci kolem osy Y a vůči vám pro rotaci kolem osy Z – potom křivka Jo prstů malovat označuje směr pro kladné hodnoty úhlu otočení.

`SKMatrix44` má zobecněn statické [ `CreateRotation` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotation/p/System.Single/System.Single/System.Single/System.Single/) a [ `CreateRotationDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotationDegrees/p/System.Single/System.Single/System.Single/System.Single/) metody, které vám umožňují určit okolo kterého dojde k otočení osy:

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

Pro rotaci kolem osy X nastavte na 1, 0, 0 první tři argumenty. Pro rotaci kolem osy Y nastavte u nich hodnotu 0, 1, 0 a pro rotaci kolem osy Z, nastavte u nich hodnotu 0, 0, 1.

Čtvrtý sloupec 4 ve 4 je pro perspektivu. `SKMatrix44` Nemá žádné metody pro vytvoření perspektivy transformací, ale můžete vytvořit vlastní pomocí následujícího kódu:

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

Důvod pro název argumentu `depth` bude zřejmé, za chvíli. Tento kód vytvoří matici:

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

Transformace vzorce za následek následující výpočet w ":

w! = – z nebo hloubka + 1

Tato stránka slouží ke snížení souřadnice X a Y, když hodnoty Z jsou menší než nula (koncepčně za XY roviny) a ke zvýšení souřadnice X a Y pro kladné hodnoty Z. Když se rovná souřadnice `depth`, pak w "je nula a souřadnice stát nekonečné. Systémy 3D grafiky jsou postavené na metafora fotoaparát a `depth` hodnotu v tomto poli představuje vzdálenost fotoaparátu/kamery od počátku systém souřadnic. Pokud grafický objekt má Z koordinovat, který je `depth` jednotky ze zdroje se dotýká koncepčně přehledu fotoaparátu/kamery a nekonečně velká.

Mějte na paměti, že budete pravděpodobně používat toto `perspectiveMatrix` hodnotu v kombinaci s otočení matice. Pokud grafický objekt střídajících má větší než X nebo Y souřadnice `depth`, pak otočení tento objekt v 3D prostoru by mohla zahrnovat Z souřadnice větší než `depth`. To je třeba se vyhnout! Při vytváření `perspectiveMatrix` chcete nastavit `depth` na hodnotu dostatečně velký pro všechny souřadnice objektu grafiky bez ohledu na to, jak se otočí. Tím se zajistí, že to se nikdy jakékoli dělení nulou.

Kombinování 3D otáčení a perspektivy vyžaduje násobení matic 4 4 společně. Pro tento účel `SKMatrix44` definuje metody, zřetězení. Pokud `A` a `B` jsou `SKMatrix44` objekty, následující kód nastaví stejné na x B:

```csharp
A.PostConcat(B);
```

Když 4 4 transformační matice se používá v systému 2D grafika, použije se na 2D objekty. Tyto objekty jsou bez stromové struktury a se předpokládá, že má souřadnice Z nula. Transformace násobení je o něco jednodušší než transformací, která je uvedeno výše:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Z výsledků ve vzorcích transformace, které nezahrnují jakékoli buňky ve třetím řádku matice tuto hodnotu 0:

x! = M11·x M21·y + M41

y' = M12·x M22·y + M42

z' = M13·x M23·y + M43

w "= M14·x M24·y + M44

Navíc z "souřadnice je bezvýznamná i zde. Při 3D objekt je zobrazen ve 2D grafika systému, je sbaleny do dvourozměrné objekt ignorováním Z hodnot souřadnic. Transformace vzorce jsou pouze tyto dvě:

x"= x' / w"

y"= y' / w"

To znamená, že třetí řádek *a* třetí sloupec matice 4 4 můžete ignorovat.

Ale pokud je to, proč je dokonce nezbytné matice 4 4 na prvním místě?

I když jsou relevantní pro dvourozměrná transformace, třetí řádek a sloupec třetí řádek a třetí sloupec 4 ve 4 *proveďte* hrají roli před, že různé `SKMatrix44` hodnoty jsou navzájem vynásobeny. Předpokládejme například, že vynásobit otočení kolem osy Y s perspektivní transformace:

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

V rámci produktu buňku `M14` teď obsahuje hodnotu perspektivy. Pokud chcete použít tento matice 2D objektů, jsou odstraněny třetí řádek a sloupec převést na 3 3 matice:

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

Teď se dá bod 2D transformace:

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

Vzorce transformace jsou:

x! = cos ·x (α)

y' = y

z! = (sin (α) / hloubku) ·x + 1

Nyní vše, co dělit z ":

x"= cos ·x (α) / ((sin (α) / hloubku) ·x + 1)

y"= y / ((sin (α) / hloubku) ·x + 1)

Když se 2D objekty jsou otočeny kladné úhel okolo osy Y, pak kladné hodnoty X ubíhají na pozadí při záporné hodnoty X zobrazeny na popředí. Hodnoty X zdá se, že chcete přesuňte se blíže na ose Y (což se řídí hodnotu kosinus) jako souřadnice nejvzdálenější osy Y změní menší nebo větší, při přechodu od diváka nebo blíž k uživateli.

Při použití `SKMatrix44`, provádět všechny 3D otočení a perspektivy operace vynásobením různých `SKMatrix44` hodnoty. Pak můžete extrahovat dvojrozměrné matici po 3 3 ze 4 ve 4 pomocí matici [ `Matrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Matrix/) vlastnost `SKMatrix44` třídy. Tato vlastnost vrátí známým `SKMatrix` hodnotu.

**3D otočení** stránce umožňuje experimentovat s 3D otočení. [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) soubor vytvoří čtyři jezdců nastavte otočení kolem osy X, Y a a nastavit hodnotu hloubky:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.Rotation3DPage"
             Title="Rotation 3D">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
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
                    <Setter Property="Maximum" Value="360" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="xRotateSlider"
                Grid.Row="0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference xRotateSlider},
                              Path=Value,
                              StringFormat='X-Axis Rotation = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="yRotateSlider"
                Grid.Row="2"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference yRotateSlider},
                              Path=Value,
                              StringFormat='Y-Axis Rotation = {0:F0}'}"
               Grid.Row="3" />

        <Slider x:Name="zRotateSlider"
                Grid.Row="4"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zRotateSlider},
                              Path=Value,
                              StringFormat='Z-Axis Rotation = {0:F0}'}"
               Grid.Row="5" />

        <Slider x:Name="depthSlider"
                Grid.Row="6"
                Maximum="2500"
                Minimum="250"
                ValueChanged="OnSliderValueChanged" />

        <Label Grid.Row="7"
               Text="{Binding Source={x:Reference depthSlider},
                              Path=Value,
                              StringFormat='Depth = {0:F0}'}" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="8"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Všimněte si, že `depthSlider` je inicializován `Minimum` hodnota 250. Z toho vyplývá, že 2D objekt střídajících zde má souřadnice X a Y omezen na kruh definovaný 250 pixelů radius kolem počátku. Výsledkem otočení tohoto objektu v 3D prostoru bude vždy hodnoty souřadnic méně než 250.

[ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) načtení souboru kódu na pozadí rastrového obrázku, který je 300 pixelů Čtvereček:

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Pokud 3D transformace se jedná o tento rastrový obrázek, pak souřadnic X a Y rozsahu –150 až 150, zatímco rohy jsou 212 pixelů z centra, takže všechno, co je v rámci radius 250 pixelů.

`PaintSurface` Obslužná rutina vytvoří `SKMatrix44` objekty podle posuvníky a vynásobí hodnotu je propojit pomocí `PostConcat`. `SKMatrix` Extrahují z poslední hodnotu `SKMatrix44` kolem objektu je podle přeložit transformace na střed otáčení uprostřed obrazovky:

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
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

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Use 3D matrix for 3D rotations and perspective
        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, (float)xRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, (float)yRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, (float)zRotateSlider.Value));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / (float)depthSlider.Value;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the bitmap
        canvas.SetMatrix(matrix);
        float xBitmap = xCenter - bitmap.Width / 2;
        float yBitmap = yCenter - bitmap.Height / 2;
        canvas.DrawBitmap(bitmap, xBitmap, yBitmap);
    }
}
```

Když můžete experimentovat s čtvrtý posuvník, můžete si všimnout, že nastavení různých hloubky nepřesouvejte objekt další mimo prohlížeč, ale místo toho změnit rozsah efekt perspektivy:

[![](3d-rotation-images/rotation3d-small.png "Trojitá snímek obrazovky stránky 3D otočení")](3d-rotation-images/rotation3d-large.png#lightbox "Trojitá snímek obrazovky stránky 3D otáčení")

**Animovat natočení 3D** také používá `SKMatrix44` pro animaci textového řetězce v 3D prostoru. `textPaint` Objekt nastaven jako pole se používá v konstruktoru určit rozsah textu:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    SKCanvasView canvasView;
    float xRotationDegrees, yRotationDegrees, zRotationDegrees;
    string text = "SkiaSharp";
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        TextSize = 100,
        StrokeWidth = 3,
    };
    SKRect textBounds;

    public AnimatedRotation3DPage()
    {
        Title = "Animated Rotation 3D";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Measure the text
        textPaint.MeasureText(text, ref textBounds);
    }
    ...
}
```

`OnAppearing` Přepsání definuje tři Xamarin.Forms `Animation` objekty animace `xRotationDegrees`, `yRotationDegrees`, a `zRotationDegrees` pole různým tempem. Všimněte si, že jsou nastavené období tyto animace budete Vymazat čísla – 5 sekund, 7 sekund a 11 sekund – tak celkové kombinaci pouze opakuje každé 385 sekund nebo delší než 10 minut:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();

        new Animation((value) => xRotationDegrees = 360 * (float)value).
            Commit(this, "xRotationAnimation", length: 5000, repeat: () => true);

        new Animation((value) => yRotationDegrees = 360 * (float)value).
            Commit(this, "yRotationAnimation", length: 7000, repeat: () => true);

        new Animation((value) =>
        {
            zRotationDegrees = 360 * (float)value;
            canvasView.InvalidateSurface();
        }).Commit(this, "zRotationAnimation", length: 11000, repeat: () => true);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        this.AbortAnimation("xRotationAnimation");
        this.AbortAnimation("yRotationAnimation");
        this.AbortAnimation("zRotationAnimation");
    }
    ...
}
```

Stejně jako v předchozím program `PaintCanvas` obslužná rutina vytvoří `SKMatrix44` hodnoty pro otočení a perspektivy a jejich vynásobí dohromady:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Scale so text fits
        float scale = Math.Min(info.Width / textBounds.Width,
                               info.Height / textBounds.Height);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(scale, scale));

        // Calculate composite 3D transforms
        float depth = 0.75f * scale * textBounds.Width;

        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, xRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, yRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, zRotationDegrees));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / depth;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the text
        canvas.SetMatrix(matrix);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;
        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

3D otočení je obklopená několik 2D transformace přesunout střed otáčení do středu obrazovky a umožňuje změnit velikost textového řetězce, tak, aby se stejnou šířku jako na obrazovce:

[![](3d-rotation-images/animatedrotation3d-small.png "Trojitá snímek obrazovky stránky 3D otočení animovat")](3d-rotation-images/animatedrotation3d-large.png#lightbox "Trojitá snímek obrazovky stránky animovat natočení 3D")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
