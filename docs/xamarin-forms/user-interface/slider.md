---
title: Pomocí posuvníku
description: Použijte při výběru z rozsahu průběžné hodnoty jezdce.
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/16/2018
ms.openlocfilehash: 99109f6377037ffb9f622b7ddb237b42d241e505
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="using-slider"></a>Pomocí posuvníku

_Použijte při výběru z rozsahu průběžné hodnoty jezdce._

Platformě Xamarin.Forms [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) je vodorovné panel, který smí uživatel manipulovat uživateli vybrat `double` hodnotu nepřetržitá rozsahu. 

`Slider` Definuje tři vlastnosti typu `double`:

- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) je minimální rozsah, výchozí hodnotou 0.
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) je maximální rozsah, výchozí hodnotou 1.
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) je hodnota jezdce, které mohou být v rozsahu mezi `Minimum` a `Maximum` a výchozí hodnota je 0.

Všechny tři vlastnosti jsou zajišťované `BindableProperty` objekty. `Value` Vlastnost má režim výchozí vazby `BindingMode.TwoWay`, což znamená, že se jedná o vhodné jako zdroj vazby v aplikaci, která používá [Model-View-ViewModel (modelem MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architektura. 

> [!WARNING]
> Interně `Slider` zajistí, že `Minimum` je menší než `Maximum`. Pokud `Minimum` nebo `Maximum` někdy nastaveny tak, aby `Minimum` je menší než `Maximum`, je vyvolána výjimka. Najdete v článku [ **opatření** ](#precautions) části níže Další informace o nastavení `Minimum` a `Maximum` vlastnosti.

`Slider` Převede `Value` vlastnost, aby byla mezi `Minimum` a `Maximum`(včetně). Pokud `Minimum` je nastavena na hodnotu větší než `Value` vlastnost, `Slider` nastaví `Value` vlastnost, která má `Minimum`. Podobně pokud `Maximum` je nastavená na hodnotu menší než `Value`, pak `Slider` nastaví `Value` vlastnost `Maximum`. 

`Slider` definuje [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) událost, která je aktivována, pokud `Value` změny, buď pomocí manipulace s uživatele `Slider` nebo když program nastaví `Value` vlastnost přímo. A `ValueChanged` událost je aktivována, i pokud `Value` vlastnost je má, jak je popsáno v předchozím odstavci. 

[ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) Objekt, který doprovází `ValueChanged` událostí má dvě vlastnosti, obě typu `double`: [ `OldValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.OldValue/) a [ `NewValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.NewValue/). V době událost je aktivována například, hodnota `NewValue` je stejný jako `Value` vlastnost `Slider` objektu.

> [!WARNING]
> Nepoužívejte možnosti neomezeným vodorovném rozložení `Center`, `Start`, nebo `End` s `Slider`. Pro Android a UPW `Slider` sbalí na panel nulové délky a v systému iOS, na panelu je velmi krátké. Ponechte výchozí `HorizontalOptions` nastavení `Fill`a nepoužíváte šířka `Auto` při uvedení `Slider` v `Grid` rozložení.

## <a name="basic-slider-code-and-markup"></a>Základní posuvníku kód a značky

[ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) ukázka začíná tři stránek, které fungují stejně, ale jsou implementované různými způsoby. První stránka používá jenom kód C#, druhá používá XAML obslužnou rutinu v kódu a třetí je k tomu obslužné rutiny události pomocí datové vazby v souboru XAML.

### <a name="creating-a-slider-in-code"></a>Vytváření jezdce v kódu

**Základní posuvníku kódu** stránky v [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) příklad ukazuje zobrazení k vytvoření `Slider` a dvě `Label` objekty v kódu:

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

`Slider` Je inicializována tak, aby měl `Maximum` vlastnost 360. `ValueChanged` Obslužnou rutinu `Slider` používá `Value` vlastnost `slider` objekt, který chcete nastavit `Rotation` vlastnost první `Label` a používá `String.Format` metoda s `NewValue` vlastnost argumenty událostí nastavit `Text` vlastnost druhý `Label`. Tyto dva přístupy k získání aktuální hodnota `Slider` zaměnitelné. 

Tady je programy spuštěné na iOS, Android a univerzální platformu Windows (UWP) zařízení:

[![Základní posuvníku kódu](slider-images/BasicSliderCode.png "kód základní posuvníku")](slider-images/BasicSliderCode-Large.png#lightbox)

Druhý `Label` zobrazí text "(Neinicializovaný)", dokud `Slider` se s nimi manipulovat, který případů první `ValueChanged` událost, která má být aktivována. Všimněte si, že počet desetinných míst, které se zobrazují se liší pro tři platformy. Tyto rozdíly jsou související s implementace platformy `Slider` a jsou popsány dále v tomto článku v části [platformy implementace rozdíly](#implementations).

### <a name="creating-a-slider-in-xaml"></a>Vytváření jezdce v jazyce XAML

**Základní XAML posuvníku** stránky je funkčně stejný jako **základní posuvníku kód** ale implementovaná většinou v jazyce XAML:

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

Soubor kódu obsahuje obslužnou rutinu pro `ValueChanged` událostí:

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

Je také možné získat obslužné rutiny událostí `Slider` , se aktivuje událost prostřednictvím `sender` argument. `Value` Vlastnost obsahuje aktuální hodnotu:

```csharp
double value = ((Slider)sender).Value;
```

Pokud `Slider` objekt byl přiřazen název v souboru XAML s `x:Name` atribut (například "posuvník"), pak obslužné rutiny události může odkazovat na tento objekt přímo:

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>Datové vazby posuvníku

**Základní vazby posuvníku** stránka zobrazuje jak napsat téměř ekvivalentní program, který eliminuje `Value` obslužné rutiny události pomocí [datové vazby](~/xamarin-forms/app-fundamentals/data-binding/index.md):

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

`Rotation` Vlastnost první `Label` je vázána `Value` vlastnost `Slider`, jako je `Text` vlastnost druhý `Label` s `StringFormat` specifikace. **Základní vazby posuvníku** stránku funkce trochu jinak z dvě předchozí stránky: Pokud nejprve zobrazí stránku, druhý `Label` zobrazí textový řetězec s hodnotou. Toto je výhodou použití datových vazeb. Chcete-li zobrazit text bez vazby dat, museli byste konkrétně inicializovat `Text` vlastnost `Label` nebo simulovat pálení z `ValueChanged` událostí pomocí volání obslužné rutiny události z konstruktoru třídy.

<a name="precautions" />

## <a name="precautions"></a>Opatření

Hodnota `Minimum` vlastnost musí být vždy nižší než hodnota `Maximum` vlastnost. Následující kód příčiny fragment kódu `Slider` pro vyvolání k výjimce:

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

Kompilátor jazyka C# generuje kód, který nastaví tyto dvě vlastnosti v pořadí, a kdy `Minimum` je nastavena na 10, je větší než výchozí `Maximum` hodnotu 1. Se můžete vyhnout výjimka v tomto případě nastavení `Maximum` vlastnost první:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Nastavení `Maximum` až 20 číslic se nejedná o problém protože je větší než výchozí `Minimum` nastavení 0. Když `Minimum` není nastaven, hodnota je menší než `Maximum` hodnotu 20.

Stejný problém existuje v jazyce XAML. Nastavit vlastnosti v pořadí, které zajistí, že `Maximum` je vždy větší než `Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

Můžete nastavit `Minimum` a `Maximum` hodnoty záporná čísla, ale jenom v pořadí, ve kterém `Minimum` je vždy menší než `Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

`Value` Vlastnost je vždy větší než nebo rovna hodnotě `Minimum` hodnota a menší než nebo rovno `Maximum`. Pokud `Value` je nastaven na hodnotu mimo tento rozsah, bude hodnota přiřadit k ležet v rozsahu, ale žádné došlo k výjimce. Například tento kód vám *není* vyvolat výjimku:

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

Místo toho `Value` vlastnost sloučen s `Maximum` hodnotu 1.

Tady je výše uvedeném fragmentu kódu: 

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Když `Minimum` nastavena na 10, pak `Value` také nastaven na hodnotu 10. 

Pokud `ValueChanged` obslužné rutiny události byla připojena v době, `Value` vlastnost sloučen s něco jiného než jeho výchozí hodnotu 0, pak `ValueChanged` událost je aktivována. Zde je fragment kódu jazyka XAML: 

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

Když `Minimum` je nastaven na hodnotu 10, `Value` je také nastavena na 10 a `ValueChanged` událost je aktivována. Tato situace může nastat, předtím, než má byla vytvořená zbývající části stránky a obslužná rutina se může pokusit odkazy na další prvky na stránce, které dosud nebyly vytvořeny. Můžete chtít přidat kód, který `ValueChanged` obslužná rutina, která vyhledává `null` hodnoty další elementy na stránce. Nebo můžete nastavit `ValueChanged` obslužné rutiny události po `Slider` hodnoty byly inicializovány.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>Rozdíly implementace platformy

Snímky obrazovky uvedena výše zobrazit hodnotu `Slider` s jiný počet desetinných míst. Vztahuje se k jak `Slider` se implementuje na platformy Android a UWP.

### <a name="the-android-implementation"></a>Android implementace 

Android implementace `Slider` je založena na Android [ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/) a vždy nastavuje [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) vlastnost do 1000. To znamená, že `Slider` v systému Android má pouze 1,001 diskrétními hodnotami. Pokud nastavíte `Slider` tak, aby měl `Minimum` 0 a `Maximum` 5000 a potom jako `Slider` se s nimi manipulovat, `Value` vlastnost má hodnoty 0, 5, 10, 15 a tak dále. 

### <a name="the-uwp-implementation"></a>Implementace UWP

Implementace UWP `Slider` je založena na UWP [ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider) ovládacího prvku. `StepFrequency` Vlastnost UWP `Slider` je nastaven na rozdíl `Maximum` a `Minimum` vlastnosti dělený 10, ale není větší než 1. 

Například pro výchozí rozsah 0 až 1 `StepFrequency` je nastavena na 0,1. Jako `Slider` se s nimi manipulovat, `Value` vlastnost je omezené na 0, 0.1, 0.2, 0.3, 0.4, 0,5, 0,6, 0,7, 0,8, 0,9 a 1.0. (Toto je zřejmé ve na poslední stránku [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) ukázkové.) Když rozdíl mezi `Maximum` a `Minimum` vlastnosti je 10 nebo vyšší, pak `StepFrequency` je nastavena na hodnotu 1 a `Value` vlastnost má celočíselné hodnoty. 

### <a name="the-stepslider-solution"></a>Řešení StepSlider

Rozmanitější `StepSlider` je podrobněji [kapitoly 27. Vlastní nástroji pro vykreslování](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) knihy *vytváření mobilních aplikací s Xamarin.Forms*. `StepSlider` Je podobná `Slider` , ale přidá `Steps` vlastnosti a určit počet hodnot mezi `Minimum` a `Maximum`.

## <a name="sliders-for-color-selection"></a>Posuvníky pro výběr barev

Konečné dvě stránky v [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) ukázku, jak použít tři `Slider` instance pro výběr barev. Na první stránku zpracovává všechny interakce v souboru kódu na pozadí, zatímco druhé stránce ukazuje, jak použít datovou vazbu s ViewModel. 

### <a name="handling-sliders-in-the-code-behind-file"></a>Zpracování posuvníků v souboru kódu na pozadí 

**Posuvníky barva RGB** vytvoří stránky `BoxView` zobrazíte barvu, tři `Slider` instance červené, zelené a modré součásti, barvu a tři `Label` prvky pro zobrazení těchto barev hodnoty:

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

A `Style` poskytuje všechny tři `Slider` elementy a rozsahu od 0 do 255. `Slider` Elementy sdílet stejný `ValueChanged` obslužná rutina, která je implementovaná v souboru kódu na pozadí:

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

První oddíl sady `Text` vlastnost jednoho `Label` instance na krátký textový řetězec označující hodnotu `Slider` v šestnáctkové soustavě. Potom všechny tři `Slider` instance přistupuje k vytvoření `Color` hodnotu z komponenty RGB:

[![Posuvníky barva RGB](slider-images/RgbColorSliders.png "posuvníky barva RGB")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>Vytvoření vazby ViewModel posuvníku

**HSL posuvníky** stránky ukazuje, jak pomocí ViewModel provádět výpočty použít k vytvoření `Color` hodnotu z hue, sytost a světlost hodnot. Všechny ViewModels, jako `HSLColorViewModel` třída implementuje `INotifyPropertyChanged` rozhraní a aktivuje se `PropertyChanged` událost vždy, když jedna z vlastností změny:

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

ViewModels a `INotifyPropertyChanged` rozhraní jsou popsané v článku [datové vazby](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**HslColorSlidersPage.xaml** soubor vytvoří `HslColorViewModel` a nastaví se na stránku `BindingContext` vlastnost. To umožňuje všechny elementy v souboru XAML pro vazbu vlastnosti ViewModel:

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

Jako `Slider` elementy jsou s nimi manipulovat, `BoxView` a `Label` elementy jsou aktualizovány ze ViewModel:

[![HSL posuvníky](slider-images/HslColorSliders.png "HSL posuvníky")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat` Komponentu `Binding` – rozšíření značek je nastaven pro formát "F2" pro zobrazení dvě desetinná místa. (Řetězec formátování v vazby dat je popsána v článku [formátování řetězce](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) Je však omezená na hodnoty 0, 0.1, 0.2, UWP verzi programu... 0,9 a 1.0. Toto je přímý výsledek provádění UWP `Slider` jak je popsáno výše v části [platformy implementace rozdíly](#implementations).

## <a name="related-links"></a>Související odkazy

- [Ukázka ukázky posuvníku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [Posuvník rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)
