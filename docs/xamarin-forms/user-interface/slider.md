---
title: Posuvník Xamarin.Forms
description: Jezdec Xamarin.Forms je vodorovný pruh, který lze ovládat uživatel vybrat hodnotu double z průběžné rozsahu. Tento článek vysvětluje, jak použít třídu posuvník vybrat hodnotu z rozsahu průběžné hodnoty.
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/10/2018
ms.openlocfilehash: 2ba4ffa1bcaee5f95fbd963cd48e694569ec7850
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986093"
---
# <a name="xamarinforms-slider"></a>Posuvník Xamarin.Forms

_Pomocí posuvníku pro výběr z rozsahu průběžné hodnoty._

Xamarin.Forms [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) je vodorovný pruh, který může uživatel vybrat manipulovat `double` hodnotu z průběžné rozsahu.

`Slider` Definuje tři vlastnosti typu `double`:

- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) je minimum rozsahu, s výchozí hodnotou 0.
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) je maximální rozsah, s výchozí hodnotou 1.
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) je hodnota posuvníku, která může být v rozsahu mezi `Minimum` a `Maximum` a má výchozí hodnotu 0.

Všechny tři vlastnosti využívají `BindableProperty` objekty. `Value` Vlastnost má režim výchozí vazby `BindingMode.TwoWay`, což znamená, že je vhodný jako zdroj vazby v aplikaci, která se používá [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architektury.

> [!WARNING]
> Interně `Slider` zajišťuje, že `Minimum` je menší než `Maximum`. Pokud `Minimum` nebo `Maximum` někdy jsou nastavené tak, aby `Minimum` je nejméně `Maximum`, je vyvolána výjimka. Zobrazit [ **opatření** ](#precautions) níže v části Další informace o nastavení `Minimum` a `Maximum` vlastnosti.

`Slider` Převede `Value` vlastnost tak, že je mezi `Minimum` a `Maximum`včetně. Pokud `Minimum` je nastavena na hodnotu větší než `Value` vlastnost, `Slider` nastaví `Value` vlastnost `Minimum`. Podobně pokud `Maximum` je nastavena na hodnotu menší než `Value`, pak `Slider` nastaví `Value` vlastnost `Maximum`.

`Slider` definuje [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) událost, která je aktivována, pokud `Value` změny, buď pomocí manipulace s uživateli `Slider` nebo když program nastaví `Value` vlastnost přímo. A `ValueChanged` událost se aktivuje, i když `Value` vlastnost převeden, jak je popsáno v předchozím odstavci.

[ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) Objekt, který doprovází `ValueChanged` událost má dvě vlastnosti, oba typu `double`: [ `OldValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.OldValue/) a [ `NewValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.NewValue/). V době událost se aktivuje, hodnota `NewValue` je stejné jako `Value` vlastnost `Slider` objektu.

> [!WARNING]
> Nepoužívejte možnosti neomezeným vodorovné rozložení `Center`, `Start`, nebo `End` s `Slider`. Na Android a UPW `Slider` Sbalí panel o nulové délce a v systémech iOS, na panelu je velmi krátké. Ponechte výchozí `HorizontalOptions` nastavení `Fill`a nepoužívejte šířku `Auto` při vložení `Slider` v `Grid` rozložení.

`Slider` Také definuje několik vlastností, které ovlivňují její vzhled:

- [`MinimumTrackColor`](xref:Xamarin.Forms.Slider.MinimumTrackColorProperty) je panel Barva vlevo od jezdce.
- [`MaximumTrackColor`](xref:Xamarin.Forms.Slider.MaximumTrackColorProperty) je panel barvy vpravo od jezdce.
- [`ThumbColor`](xref:Xamarin.Forms.Slider.ThumbColorProperty) je barva jezdce. Tato vlastnost není podporována na univerzální platformu Windows.
- [`ThumbImage`](xref:Xamarin.Forms.Slider.ThumbImageProperty) je obrázku používaného pro Flash, typu [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource). Tato vlastnost není podporována na univerzální platformu Windows.

> [!NOTE]
> `ThumbColor` a `ThumbImage` vlastnosti se vzájemně vylučují. Pokud jsou nastaveny obě vlastnosti, `ThumbImage` vlastnost bude mít přednost.

## <a name="basic-slider-code-and-markup"></a>Základní kód posuvník a značky

[ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) ukázka začíná tři stránky, které jsou funkčně stejný jako, ale jsou implementované v různých způsobů. První stránka používá pouze kód jazyka C#, druhý používá obslužnou rutinu v kódu XAML a třetí je schopni vyhnout obslužné rutiny události pomocí datové vazby v souboru XAML.

### <a name="creating-a-slider-in-code"></a>Vytváří se ovládací prvek posuvník v kódu

**Základní kód posuvník** stránku [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) příklad ukazuje zobrazení k vytvoření `Slider` a dva `Label` objekty v kódu:

```csharp
public class BasicSliderCodePage : ContentPage
{
    public BasicSliderCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Slider slider = new Slider
        {
            Maximum = 360
        };
        slider.ValueChanged += (sender, args) =>
        {
            rotationLabel.Rotation = slider.Value;
            displayLabel.Text = String.Format("The Slider value is {0}", args.NewValue);
        };

        Title = "Basic Slider Code";
        Padding = new Thickness(10, 0);
        Content = new StackLayout
        {
            Children =
            {
                rotationLabel,
                slider,
                displayLabel
            }
        };
    }
}
```

`Slider` Je inicializován mít `Maximum` vlastnost 360. `ValueChanged` Obslužná rutina `Slider` používá `Value` vlastnost `slider` nastavíte `Rotation` vlastnosti prvního `Label` a používá `String.Format` metodu s `NewValue` vlastnost argumenty události nastavit `Text` vlastnost druhého `Label`. Tyto dvě metody k získání aktuální hodnoty `Slider` jsou zaměnitelné.

Tady je program spuštěn zařízení s iOS, Android a univerzální platformu Windows (UPW):

[![Základní posuvník kód](slider-images/BasicSliderCode.png "základní posuvník kódu")](slider-images/BasicSliderCode-Large.png#lightbox)

Druhý `Label` zobrazí text "(neinicializovaná)" až `Slider` je zpracováván, který případech první `ValueChanged` událost se aktivuje. Všimněte si, že počet desetinných míst, které se zobrazují se liší pro tři platformy. Tyto rozdíly jsou související s implementací platformy `Slider` a jsou popsány dále v tomto článku v části [rozdíly pro tyto platformy implementace](#implementations).

### <a name="creating-a-slider-in-xaml"></a>Vytváří se ovládací prvek posuvník v XAML

**Základní XAML posuvník** stránka je funkčně stejný jako **základní kód posuvník** ale implementované převážně v XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderXamlPage"
             Title="Basic Slider XAML"
             Padding="10, 0">
    <StackLayout>
        <Label x:Name="rotatingLabel"
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider Maximum="360"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Soubor kódu obsahuje obslužnou rutinu pro `ValueChanged` události:

```csharp
public partial class BasicSliderXamlPage : ContentPage
{
    public BasicSliderXamlPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double value = args.NewValue;
        rotatingLabel.Rotation = value;
        displayLabel.Text = String.Format("The Slider value is {0}", value);
    }
}
```

Je také možné získat obslužné rutiny událostí `Slider` , která se spouští události prostřednictvím `sender` argument. `Value` Vlastnost obsahuje aktuální hodnotu:

```csharp
double value = ((Slider)sender).Value;
```

Pokud `Slider` objekt byl udělen název v souboru XAML `x:Name` atribut (například "posuvníku") a pak obslužná rutina události může odkazovat tento objekt přímo:

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>Datové vazby posuvníku

**Základní vazby posuvník** stránce ukazuje, jak napsat téměř ekvivalentní programu, který eliminuje `Value` obslužné rutiny události pomocí [datové vazby](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderBindingsPage"
             Title="Basic Slider Bindings"
             Padding="10, 0">
    <StackLayout>
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference slider},
                                  Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360" />

        <Label x:Name="displayLabel"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='The Slider value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Rotation` Vlastnosti prvního `Label` je vázán na `Value` vlastnost `Slider`, jak je `Text` vlastnost druhého `Label` s `StringFormat` specifikace. **Základní vazby posuvník** stránku funkce trochu jinak než ze dvou předchozí stránky: Když poprvé objeví na stránce, druhý `Label` zobrazí textový řetězec s hodnotou. Toto je na výhodu plynoucí z použití datových vazeb. K zobrazení textu bez vazby na data, je třeba konkrétně inicializovat `Text` vlastnost `Label` nebo simulovat jeho spuštění nástroje `ValueChanged` události ve volání obslužné rutiny události z konstruktoru třídy.

<a name="precautions" />

## <a name="precautions"></a>Opatření

Hodnota `Minimum` vlastnost musí být vždy menší než hodnota `Maximum` vlastnost. Následující kód způsobí, že fragment kódu `Slider` pro vyvolání výjimky:

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

Kompilátor jazyka C# vygeneruje kód, který nastaví tyto dvě vlastnosti v pořadí, a kdy `Minimum` je nastavena na 10, je větší než výchozí `Maximum` hodnotu 1. Můžete se vyhnout výjimky v tomto případě nastavení `Maximum` vlastnost první:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Nastavení `Maximum` 20 není problém vzhledem k tomu, že je větší než výchozí `Minimum` nastavení 0. Když `Minimum` je nastaven, hodnota je menší než `Maximum` hodnota 20.

Stejný problém existuje v XAML. Nastavit vlastnosti v pořadí, které zajistí, že `Maximum` je vždy větší než `Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

Můžete nastavit `Minimum` a `Maximum` hodnoty záporná čísla, ale pouze v pořadí, ve kterém `Minimum` je vždy menší než `Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

`Value` Vlastnost je vždy větší než nebo rovna hodnotě `Minimum` hodnota a menší než nebo rovno `Maximum`. Pokud `Value` je nastavena na hodnotu mimo tento interval, bude hodnota přiřadit k ležet v rozsahu, ale je vyvolána žádná výjimka. Například tento kód vám *není* vyvolat výjimku:

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

Místo toho `Value` vlastnost převeden na `Maximum` hodnotu 1.

Tady je výše uvedeném fragmentu kódu:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Když `Minimum` nastavená na 10, pak `Value` je také nastavena na hodnotu 10.

Pokud `ValueChanged` obslužná rutina události byla připojena v době, která `Value` vlastnost je převést na jinou hodnotu než výchozí hodnotu 0, o `ValueChanged` událost se aktivuje. Tady je fragment kódu XAML:

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

Když `Minimum` nastavená na 10 `Value` je také nastavena na 10 a `ValueChanged` událost se aktivuje. Tato situace může nastat, předtím, než byl vytvořen zbytku stránky a obslužná rutina se může pokusit odkazují jiné elementy na stránce, které dosud nebyly vytvořeny. Můžete chtít přidat kód, který `ValueChanged` obslužná rutina, která vyhledává `null` hodnot jiných prvků na stránce. Nebo můžete nastavit `ValueChanged` obslužná rutina události po `Slider` hodnoty byly inicializovány.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>Implementace rozdíly pro tyto platformy

Zobrazit hodnotu na snímcích obrazovky je uvedeno výše `Slider` s různým počtem desetinných míst. To se vztahuje jak `Slider` se implementuje na platformy Android a UPW.

### <a name="the-android-implementation"></a>Implementace s Androidem

Android provádění `Slider` je založen na Android [ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/) a vždy nastaví [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) vlastnost na hodnotu 1000. To znamená, že `Slider` v systému Android má pouze 1,001 jednotlivých hodnot. Pokud nastavíte `Slider` mít `Minimum` 0 a `Maximum` 5000 a potom jako `Slider` je zpracováván `Value` vlastnost má hodnotu 0, 5, 10, 15 a tak dále.

### <a name="the-uwp-implementation"></a>Implementace UPW

Implementace UPW `Slider` je založen na UPW [ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider) ovládacího prvku. `StepFrequency` Vlastnost UPW `Slider` je nastavena na rozdíl dané `Maximum` a `Minimum` vlastnosti dělený 10, ale ne větší než 1.

Například pro výchozí rozsah 0 až 1 `StepFrequency` je nastavena na 0,1. Jako `Slider` je zpracováván `Value` je vlastnost omezená na 0, 0.1, 0.2, 0.3, 0.4, 0,5, 0.6, 0,7, 0,8, 0.9 a 1.0. (Tím je zřejmé ve poslední stránky [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) ukázka.) Když rozdíl mezi `Maximum` a `Minimum` vlastnosti je 10 nebo vyšší verzi, pak `StepFrequency` je nastavená na 1 a `Value` vlastnost má celočíselné hodnoty.

Kromě toho [ `ThumbColor` ](xref:Xamarin.Forms.Slider.ThumbColorProperty) a [ `ThumbImage` ](xref:Xamarin.Forms.Slider.ThumbImageProperty) vlastnosti nejsou podporovány na UPW.

### <a name="the-stepslider-solution"></a>StepSlider řešení

Větší variabilitu `StepSlider` je podrobněji popsána [kapitoly 27. Vlastní renderery](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) knihy *vytváření mobilních aplikací pomocí Xamarin.Forms*. `StepSlider` Je podobný `Slider` , ale přidá `Steps` vlastnosti a určit tak počet hodnot mezi `Minimum` a `Maximum`.

## <a name="sliders-for-color-selection"></a>Posuvníky pro výběr barvy

Poslední dvě stránky v [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) ukázka, jak použít tři `Slider` instance pro výběr barvy. První stránka zpracovává všechny interakce v souboru kódu na pozadí, zatímco na druhé stránce ukazuje, jak pomocí datové vazby ViewModel.

### <a name="handling-sliders-in-the-code-behind-file"></a>Zpracování jezdce v souboru kódu na pozadí

**RGB posuvníky** vytvoří instanci stránky `BoxView` zobrazíte barvy, tři `Slider` instance červené, zelené a modré složky barvy a tři `Label` prvky pro zobrazení těchto barev hodnoty:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.RgbColorSlidersPage"
             Title="RGB Color Sliders">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="Maximum" Value="255" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView x:Name="boxView"
                 Color="Black"
                 VerticalOptions="FillAndExpand" />

        <Slider x:Name="redSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="redLabel" />

        <Slider x:Name="greenSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="greenLabel" />

        <Slider x:Name="blueSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="blueLabel" />
    </StackLayout>
</ContentPage>
```

A `Style` poskytuje všechny tři `Slider` prvky v rozsahu od 0 do 255. `Slider` Prvky sdílet stejný `ValueChanged` obslužné rutiny, které je implementované v souboru kódu na pozadí:

```csharp
public partial class RgbColorSlidersPage : ContentPage
{
    public RgbColorSlidersPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (sender == redSlider)
        {
            redLabel.Text = String.Format("Red = {0:X2}", (int)args.NewValue);
        }
        else if (sender == greenSlider)
        {
            greenLabel.Text = String.Format("Green = {0:X2}", (int)args.NewValue);
        }
        else if (sender == blueSlider)
        {
            blueLabel.Text = String.Format("Blue = {0:X2}", (int)args.NewValue);
        }

        boxView.Color = Color.FromRgb((int)redSlider.Value,
                                      (int)greenSlider.Value,
                                      (int)blueSlider.Value);
    }
}
```

První oddíl sady `Text` vlastnost některého `Label` instance krátký textový řetězec označující hodnotu `Slider` v šestnáctkové soustavě. Potom všechny tři `Slider` instance přistupuje k vytvoření `Color` hodnotu z komponenty RGB:

[![Posuvníky barva RGB](slider-images/RgbColorSliders.png "RGB posuvníky")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>Svázání posuvníku ViewModel

**HSL posuvníky** stránce ukazuje způsob použití ViewModel provádět výpočty, použít k vytvoření `Color` hodnotu z hue, sytosti a světlosti hodnot. Všechny modely ViewModels, jako jsou `HSLColorViewModel` implementuje třída `INotifyPropertyChanged` rozhraní a aktivuje se `PropertyChanged` událost při každé změně jedna z vlastností:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));
            }
        }
        get
        {
            return color;
        }
    }
}
```

Modely ViewModels a `INotifyPropertyChanged` rozhraní jsou popsány v následujícím článku [datové vazby](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**HslColorSlidersPage.xaml** vytvoří soubor `HslColorViewModel` a nastaví se na stránku `BindingContext` vlastnost. To umožňuje všechny elementy v souboru XAML vytvořit vazbu vlastnosti ViewModel:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SliderDemos"
             x:Class="SliderDemos.HslColorSlidersPage"
             Title="HSL Color Sliders">

    <ContentPage.BindingContext>
        <local:HslColorViewModel Color="Chocolate" />
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</ContentPage>
```

Jako `Slider` prvky manipulováno, `BoxView` a `Label` prvky jsou aktualizovány ze ViewModel:

[![HSL posuvníky](slider-images/HslColorSliders.png "HSL posuvníky")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat` Komponentu `Binding` – rozšíření značek nastavená u formátu "F2" zobrazit dvě desetinná místa. (Řetězce formátování v datové vazby je popsán v článku [formátování řetězce](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) Je však omezená na hodnotu 0, 0.1, 0.2, UPW verzi programu... 0.9 a 1.0. Toto je přímý výsledek provádění na UPW `Slider` jak je popsáno výše v části [rozdíly pro tyto platformy implementace](#implementations).

## <a name="related-links"></a>Související odkazy

- [Ukázka ukázky posuvníku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [Posuvník rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)
