---
title: 3D otočení
description: Otočit 2D objektů v 3D prostoru pomocí-afinní transformace.
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: a7d76bfad6a34bcd43b304a6ea4a8f9210e3550c
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/03/2018
---
# <a name="3d-rotations"></a>3D otočení

_Otočit 2D objektů v 3D prostoru pomocí-afinní transformace._

Běžné uplatňování-afinní transformace simuluje oběh 2D objektu v 3D prostoru:

![](3d-rotation-images/3drotationsexample.png "Textový řetězec otáčet v 3D prostoru")

Tato úloha zahrnuje práci s trojrozměrné otáčení a pak odvozování jiných afinní `SKMatrix` transformace, který provádí tyto 3D otočení.

Je obtížné vyvíjet to `SKMatrix` transformace funguje pouze v rámci dvěma rozměry. Úloha změní mnohem jednodušší, pokud tato matice 3 3 je odvozený od matice 4 ve 4 se používá v 3D grafický. Zahrnuje SkiaSharp [ `SKMatrix44` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.PreConcat/p/SkiaSharp.SKMatrix44/) třídu pro tento účel, ale některé pozadí 3D grafického procesoru je nezbytné pro pochopení 3D otáčení a matice 4 4 transformace.

Trojrozměrné souřadnicový systém přidá třetí osy Z. koncepčně volat, osy Z v pravém úhlu na obrazovku. Souřadnice body v 3D prostoru jsou vypsány s tři čísla: (x, y, z). V 3D systém souřadnic používané v tomto článku, zvýšení hodnoty X jsou napravo a roste hodnoty y přejděte dolů, jako v případě dvěma rozměry. Zvyšující kladné Z hodnoty pocházejí z obrazovky. Levém horním rohu, jako v případě grafiky 2D pochází. Na obrazovce si lze představit jako XY rovině s osy Z v pravém úhlu k této roviny.

Tomu se říká levé souřadnicový systém. Pokud bod ukazováčkem pro vaše vlevo ve směru kladné X souřadnice (napravo) a prstu střední ve směru zvýšení Y koordinuje (dolů), pak palec bodů ve směru zvýšení souřadnice Z – rozšíření se z na obrazovce.

V 3D grafiky jsou transformací založené na matice 4 4. Tady je matice 4 4 identity:

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

Při práci s matice 4 4, je vhodné k identifikaci buněk s jejich čísla řádků a sloupců:

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

Ale SkiaSharp `Matrix44` třídy se mírně liší. Jediný způsob, jak nastavit nebo získat hodnoty jednotlivých buněk `SKMatrix44` je pomocí [ `Item` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Item/p/System.Int32/System.Int32/) indexer. Indexy řádků a sloupců, které jsou od nuly spíše než založené na jeden a jsou vzájemně zaměněny řádků a sloupců. Buňky M14 v diagramu přistupuje pomocí indexeru `[3, 0]` v `SKMatrix44` objektu.

V rámci systému 3D grafický je 1 4 matice pro vynásobí matice 4 4 transformace převést bod 3D (x, y, z):

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Podobá se 2D transformuje které se provádějí v tři dimenze, 3D transformace se předpokládá, že proběhne ve čtyři dimenze. Čtvrtý dimenze se označuje jako W a 3D prostoru se předpokládá, že v rámci 4D místa, kde jsou W souřadnice rovna 1. Transformace vzorce jsou následující:

x' = M11·x + M21·y + M31·z + M41

y' = M12·x + M22·y + M32·z + M42

z' = M13·x + M23·y + M33·z + M43

w' = M14·x + M24·y + M34·z + M44

Je zřejmé od vzorců transformace, buněk `M11`, `M22`, `M33` jsou škálování faktory v X, Y a pokynů, a `M41`, `M42`, a `M43` jsou překlad faktory X, Y a Z pokynů.

Převést tyto souřadnice 3D místa, kde W rovná 1, x', y ", a z 'souřadnice jsou všechny rozdělené podle w":

x"= x' / w.

y"= y" / w.

z" = z' / w'

w" = w' / w' = 1

Že dělení w, poskytuje perspektivy v 3D prostoru. Pokud w se rovná 1, pak dojde k žádné perspektivy.

Otáčení v 3D prostoru může být poměrně složité, ale jsou nejjednodušší otáčení kolem osy X, Y a Z. Rotaci kolem osy X úhlu α je tato matice:

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

Hodnoty X zůstávají stejné při podrobí Tato transformace. Otočení okolo osy Y opustí hodnoty y beze změny:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Otočení kolem osy Z je stejné jako grafiky 2D:

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

Směru je zahrnuto v uchopení pera z souřadnicový systém. Toto je levou systém, takže pokud se bod úchytu levé ruky při zvyšování hodnoty pro konkrétní osu – vpravo pro otočení okolo osy X dolů pro otočení okolo osy Y a směrem můžete pro otočení kolem osy Z – pak křivku z yo Další prsty udává směr pro kladné hodnoty úhlu otočení.

`SKMatrix44` má zobecněné statické [ `CreateRotation` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotation/p/System.Single/System.Single/System.Single/System.Single/) a [ `CreateRotationDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotationDegrees/p/System.Single/System.Single/System.Single/System.Single/) metody, které umožňují určit osy, kolem kterého dojde k otočení:

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

Pro otočení okolo osy X nastavte na 1, 0, 0 první tři argumenty. Pro otáčení okolo osy Y je nastavena na 0, 1, 0 a pro otáčení kolem osy Z je nastavena na 0, 0, 1.

Čtvrtém sloupci 4 ve 4 je pro perspektivu. `SKMatrix44` Nemá žádné metody pro vytvoření perspektivy transformací, ale můžete vytvořit jeden sami pomocí následujícího kódu:

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

Z důvodu pro argument název `depth` bude zřejmé za chvíli. Tento kód vytvoří matice:

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

Transformace vzorce mít za následek následující výpočtu w ":

w "= – z / hloubka + 1

To slouží ke snížení souřadnice X a Y při hodnoty Z jsou menší než nula (koncepčně za rovinou XY) a ke zvýšení souřadnice X a Y pro kladné hodnoty Z. Když souřadnice rovná `depth`, pak w se rovná nule a souřadnice přestat nekonečné. Prostorovou grafiku systémy jsou vytvořeny kolem jedná fotoaparát a `depth` v tomto poli hodnota představuje vzdálenost fotoaparát z tohoto počátku souřadnicový systém. Pokud má grafické objekt a Z koordinaci, který je `depth` jednotky z tohoto počátku, je koncepčně dotykové ovládání přehledu kamera a nekonečnou velká.

Mějte na paměti, že budete pravděpodobně používat toto `perspectiveMatrix` hodnota v kombinaci s otočení matice. Pokud má objekt grafiky otáčení souřadnic X nebo Y větší než `depth`, pak je oběh tohoto objektu v 3D prostoru bude pravděpodobně vytvářet souřadnice Z větší než `depth`. To je třeba se vyhnout! Při vytváření `perspectiveMatrix` chcete nastavit `depth` na hodnotu dostatečně velký pro všechny souřadnice v objektu grafiky, bez ohledu na to, jak je otočen. Tím se zajistí, že se nikdy žádné dělení nulou.

Kombinování 3D otáčení a perspektivy vyžaduje vynásobením matice 4 4 společně. Pro tento účel `SKMatrix44` definuje zřetězení metody. Pokud `A` a `B` jsou `SKMatrix44` objekty, pak následující kód nastaví stejnou × B:

```csharp
A.PostConcat(B);
```

Pokud 4 4 transformační matice se používá v systému 2D grafiky, se použije u 2D objekty. Tyto objekty jsou ploché a se předpokládá, že mají souřadnice Z nula. Transformace násobení je trochu jednodušší než transformace uvedena výše:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Z výsledků ve vzorcích transformace, které nezahrnují žádné buňky ve třetím řádku matice tuto hodnotu 0:

x: = M11·x + M21·y + M41

y' = M12·x + M22·y + M42

z' = M13·x + M23·y + M43

w "= M14·x + M24·y + M44

Kromě toho z' souřadnice je důležité i zde. Když 3D objektu se zobrazí v systému 2D grafiky, je ignorováním souřadnic hodnoty Z sbalený dvourozměrná objektu. Transformace vzorce jsou pouze tyto dva:

x"= x' / w.

y"= y" / w.

To znamená, že třetí řádek *a* třetí sloupec matice 4 4 můžete ignorovat.

Ale pokud je to, proč je dokonce nezbytné matice 4 4 na prvním místě?

I když jsou důležité pro dvourozměrná transformace, třetí řádků a sloupců třetí řádek a třetí sloupec 4 ve 4 *provést* hrají roli před který po různé `SKMatrix44` hodnoty se násobí společně. Předpokládejme například, že, vynásobte Otočení okolo osy Y s perspektivní transformace:

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

V rámci produktu buňky `M14` nyní obsahuje hodnotu perspektivy. Pokud chcete použít tento matice 2D objektů, jsou odstraněny třetí řádků a sloupců převést na matici 3 3:

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

Teď může sloužit k transformaci bod 2D:

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

Transformace vzorce jsou:

x: = cos ·x (α)

y' = y

z' = (sin (α) nebo hloubku) ·x + 1

Všechno teď dělení z':

x"= cos ·x (α) / ((sin (α) nebo hloubku) ·x + 1)

y"= y / ((sin (α) nebo hloubku) ·x + 1)

Když 2D objekty otáčejí s kladný úhel okolo osy Y, pak kladné hodnoty X ubíhají na pozadí při záporné hodnoty X zobrazeny na popředí. Hodnoty X zdá se, že blíže přesunout na ose Y (která se řídí hodnota kosinus) jako souřadnice nejvíce vzdálený od osy Y bude menší nebo větší, jak se přesouvají dále za prohlížeč nebo blíže do prohlížeče.

Při použití `SKMatrix44`, provádění všech 3D otočení a operací perspektivy vynásobením různé `SKMatrix44` hodnoty. Pak můžete rozbalit dvourozměrná 3 3 matice 4 ve 4 matice pomocí [ `Matrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Matrix/) vlastnost `SKMatrix44` třída. Tato vlastnost vrací známým `SKMatrix` hodnotu.

**Natočení 3D** stránky umožňuje experimentovat s 3D otočení. [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) soubor vytvoří čtyřech posuvníky nastavit Otočení okolo osy X, Y a Z a nastavte hodnotu hloubky:

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

Všimněte si, že `depthSlider` inicializován s `Minimum` hodnotu 250. To znamená, že se tady otáčet 2D objekt obsahuje souřadnice X a Y omezen na kruh definovaný radius 250 pixelů kolem počátku. Výsledkem otočení tohoto objektu v 3D prostoru bude vždy souřadnic hodnoty menší než 250.

[ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) souboru kódu se bude načítat rastrového obrázku, který je 300 pixelů odmocnina:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

Pokud 3D transformace se jedná o tento rastrového obrázku, pak souřadnice X a Y rozsahu –150 až 150, zatímco rozích 212 pixelů z centra, takže všechno, co je v rámci protokolu radius 250 pixelů.

`PaintSurface` Obslužná rutina vytvoří `SKMatrix44` objekty podle posuvníků a vynásobí společně pomocí `PostConcat`. `SKMatrix` Hodnota, na které se extrahují z konečné `SKMatrix44` se kolem objektu podle převede transformace na střed otočení v centru obrazovky:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

Pokud můžete experimentovat s čtvrtý jezdce, můžete si všimnout nastavení různých hloubka nelze přesunout objekt další mimo prohlížeč, že místo toho změnit rozsah účinek perspektivy:

[![](3d-rotation-images/rotation3d-small.png "Trojitá snímek obrazovky stránky natočení 3D")](3d-rotation-images/rotation3d-large.png#lightbox "Trojitá snímek obrazovky stránky natočení 3D")

**Animovaný natočení 3D** také používá `SKMatrix44` pro animaci textového řetězce v 3D prostoru. `textPaint` Objekt nastavit, protože pole v konstruktoru slouží k určení mezí text:

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

`OnAppearing` Přepsání definuje tři Xamarin.Forms `Animation` objekty, které se použije animaci `xRotationDegrees`, `yRotationDegrees`, a `zRotationDegrees` pole různým tempem. Všimněte si, že období tyto animací jsou nastaveny pro primární čísla – 5 sekund, 7 sekund a 11 sekund – tak celkovou kombinace pouze opakuje každý 385 sekund nebo delší než 10 minut:

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

Jako v předchozích programu `PaintCanvas` obslužná rutina vytvoří `SKMatrix44` hodnoty pro otáčení a perspektivy a je vynásobí společně:

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

Tato 3D otočení obklopená několik 2D transformací chcete přesunout střed otáčení center obrazovky a škálování velikost textový řetězec tak, aby se stejnou délku jako obrazovky:

[![](3d-rotation-images/animatedrotation3d-small.png "Trojitá snímek obrazovky stránky animovaný natočení 3D")](3d-rotation-images/animatedrotation3d-large.png#lightbox "Trojitá snímek obrazovky stránky animovaný natočení 3D")


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
