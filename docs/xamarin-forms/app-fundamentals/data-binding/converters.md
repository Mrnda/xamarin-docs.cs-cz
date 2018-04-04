---
title: Převodníky hodnot vazby
description: Přetypování nebo převést hodnoty v rámci datové vazby
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 260db2372977202df3d73e32645a358066146b40
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="binding-value-converters"></a>Převodníky hodnot vazby

Datové vazby obvykle přenášet data z zdrojová vlastnost pro vlastnost target a v některých případech z vlastnosti cílového zdrojová vlastnost. Tento přenos je jednoduché, když jsou vlastnosti zdrojové a cílové stejného typu, nebo když jeden typ lze převést na typ prostřednictvím implicitní převod. Pokud není tento případ, převod typu musí být provedeno.

V [ **formátování řetězce** ](string-formatting.md) článku jste viděli, jak můžete použít `StringFormat` vlastnost vazby dat jakéhokoli typu převést na řetězec. U jiných typů převody, budete muset napsat specializované kód ve třídě, která implementuje [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) rozhraní. (Universal Windows Platform obsahuje podobné třídy s názvem [ `IValueConverter` ](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) v `Windows.UI.Xaml.Data` oboru názvů, ale to `IValueConverter` v `Xamarin.Forms` oboru názvů.) Třídy implementující `IValueConverter` se nazývají *hodnotu převaděče*, ale jejich také často označovány jako *vazby převaděče* nebo *vazby převodníky hodnot*.

## <a name="the-ivalueconverter-interface"></a>Rozhraní IValueConverter

Předpokládejme, že chcete definovat vazbu dat, kde je zdrojová vlastnost typu `int` , ale vlastnost target je `bool`. Chcete, aby tato vazba dat k vytvoření `false` hodnotu, pokud je zdroj celé číslo rovno 0, a `true` jinak.  

To lze provést pomocí třídy, která implementuje `IValueConverter` rozhraní:

```csharp
public class IntToBoolConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value != 0;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? 1 : 0;
    }
}
```

Nastavení instance této třídy lze [ `Converter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Converter/) vlastnost `Binding` třídy nebo [ `Converter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Converter/) vlastnost `Binding` – rozšíření značek. Tato třída stane součástí datové vazby.

`Convert` Metoda je volána, když data se přesouvají ze zdroje do cíle v `OneWay` nebo `TwoWay` vazby. `value` Parametr je objekt nebo hodnota ve zdroji dat vazby. Metoda musí vracet hodnoty typu cíle datové vazby. Metoda tady uvedené přetypování `value` parametru `int` a porovná je s 0 `bool` vrátit hodnotu.

`ConvertBack` Metoda je volána, když data se přesouvají z cíle zdroj v `TwoWay` nebo `OneWayToSource` vazby. `ConvertBack` provede Obrácený převod: předpokládá `value` parametr je `bool` cíl a převede jej na `int` vrátit hodnotu pro tento zdroj.

Pokud také obsahuje datové vazby `StringFormat` nastavení, převaděč hodnoty je volána, než je výsledek naformátovaná jako řetězec.

**Povolit tlačítka** stránku [ **ukázky vazby dat** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) příklad ukazuje, jak používat tento převaděč hodnoty v datová vazba. `IntToBoolConverter` Je vytvořena instance ve slovníku prostředků stránky. Se pak odkazuje s `StaticResource` – rozšíření značek k nastavení `Converter` vlastnost dvě datové vazby. Je velmi běžné sdílet převaděče data mezi víc vazeb dat na stránce:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.EnableButtonsPage"
             Title="Enable Buttons">
    <ContentPage.Resources>
        <ResourceDictionary>
            <local:IntToBoolConverter x:Key="intToBool" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <Entry x:Name="entry1"
               Text=""
               Placeholder="enter search term"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Search"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry1},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />

        <Entry x:Name="entry2"
               Text=""
               Placeholder="enter destination"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Submit"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry2},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />
    </StackLayout>
</ContentPage>
```

Pokud hodnota převaděč slouží na různých stránkách aplikace, můžete vytvořit instanci ji ve slovníku prostředků v **App.xaml** souboru.

**Povolit tlačítka** stránky ukazuje běžné potřebují, kdy `Button` provádí operace založené na text, který uživatel zadá do `Entry` zobrazení. Pokud nic zadali do `Entry`, `Button` by mělo být zakázáno. Každý `Button` obsahuje datová vazba na jeho `IsEnabled` vlastnost. Zdroj datové vazby `Length` vlastnost `Text` vlastnosti k odpovídající položce `Entry`. Pokud tento `Length` vlastnost není převaděč hodnoty 0, vrátí `true` a `Button` zapnutá:

[![Povolit tlačítka](converters-images/enablebuttons-small.png "povolit tlačítka")](converters-images/enablebuttons-large.png#lightbox "povolit tlačítka")

Všimněte si, že `Text` vlastnost v každé `Entry` je inicializován na prázdný řetězec. `Text` Vlastnost je `null` ve výchozím nastavení a data vazba nebude fungovat v takovém případě.

Některé převodníky hodnot jsou napsané konkrétně pro konkrétní aplikace, zatímco ostatní zobecněn. Pokud víte, že převaděč hodnoty, bude použit pouze v `OneWay` vazby, pak se `ConvertBack` metoda můžete jednoduše vrátit `null`.

`Convert` Výše uvedená metoda implicitně předpokládá, že `value` argument je typu `int` a návratová hodnota musí být typu `bool`. Podobně `ConvertBack` metoda předpokládá, že `value` argument je typu `bool` a že návratová hodnota `int`. Pokud není tento případ, dojde k výjimku modulu runtime.

Převodníky hodnot pro více zobecněny a tak, aby přijímal několika různých typů dat může zapisovat. `Convert` a `ConvertBack` můžete použít metody `as` nebo `is` operátory s `value` parametr, nebo můžete volat `GetType` na tento parametr k určení jeho typu a poté proveďte něco vhodné. Očekávaný typ návratovou hodnotu pro každou metodu je dána `targetType` parametr. V některých případech se používají převodníky hodnot s vazby dat jiný cíl typů; Převaděč hodnoty můžete použít `targetType` argument chcete provést konverzi pro správného typu.

Pokud převod provádí se liší pro různé kultury, použijte `culture` parametr pro tento účel. `parameter` Argument `Convert` a `ConvertBack` je popsána dále v tomto článku.

## <a name="binding-converter-properties"></a>Vlastnosti převaděč vazby

Hodnota převaděč třídy mohou mít vlastnosti a obecné parametry. Převede tento převaděč konkrétní hodnotu `bool` ze zdroje na objekt typu `T` pro cíl:

```csharp
public class BoolToObjectConverter<T> : IValueConverter
{
    public T TrueObject { set; get; }

    public T FalseObject { set; get; }

    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? TrueObject : FalseObject;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return ((T)value).Equals(TrueObject);
    }
}
```

**Přepínač indikátory** stránky ukazuje, jak může být použita k zobrazení hodnotu `Switch` zobrazení. Přestože je společné pro vytvoření instance převodníky hodnot jako prostředky v slovník prostředků, tato stránka představuje alternativu: vytvoření instance každé převaděč hodnoty mezi `Binding.Converter` značky element vlastnosti. `x:TypeArguments` Označuje obecné argument a `TrueObject` a `FalseObject` jsou nastaveny na objekty daného typu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SwitchIndicatorsPage"
             Title="Switch Indicators">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>

            <Style TargetType="Switch">
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Subscribe?" />
            <Switch x:Name="switch1" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch1}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Of course!"
                                                         FalseObject="No way!" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Allow popups?" />
            <Switch x:Name="switch2" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Yes"
                                                         FalseObject="No" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
                <Label.TextColor>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Color"
                                                         TrueObject="Green"
                                                         FalseObject="Red" />
                        </Binding.Converter>
                    </Binding>
                </Label.TextColor>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Learn more?" />
            <Switch x:Name="switch3" />
            <Label FontSize="18"
                   VerticalOptions="Center">
                <Label.Style>
                    <Binding Source="{x:Reference switch3}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Style">
                                <local:BoolToObjectConverter.TrueObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Indubitably!" />
                                        <Setter Property="FontAttributes" Value="Italic, Bold" />
                                        <Setter Property="TextColor" Value="Green" />
                                    </Style>                                    
                                </local:BoolToObjectConverter.TrueObject>

                                <local:BoolToObjectConverter.FalseObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Maybe later" />
                                        <Setter Property="FontAttributes" Value="None" />
                                        <Setter Property="TextColor" Value="Red" />
                                    </Style>
                                </local:BoolToObjectConverter.FalseObject>
                            </local:BoolToObjectConverter>
                        </Binding.Converter>
                    </Binding>
                </Label.Style>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Během posledních tří `Switch` a `Label` páry obecné argument je nastaven na `Style`a celý `Style` objekty jsou k dispozici pro hodnoty `TrueObject` a `FalseObject`. Toto přepsání implicitní styl pro `Label` nastavit slovník prostředků, takže vlastnosti v daném stylu explicitně přiřazené k `Label`. Přepnutím `Switch` způsobí, že odpovídající `Label` , aby odrážely změny:

[![Přepnout indikátory](converters-images/switchindicators-small.png "Přepnout indikátory")](converters-images/switchindicators-large.png#lightbox "přepínač ukazatele")

Je také možné použít [ `Triggers` ](~/xamarin-forms/app-fundamentals/triggers.md) při implementaci podobné změn v uživatelském rozhraní založené na jiné zobrazení.

## <a name="binding-converter-parameters"></a>Parametry převaděče vazby

`Binding` Třída definuje [ `ConverterParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.ConverterParameter/) vlastnost a `Binding` – rozšíření značek také definuje [ `ConverterParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.ConverterParameter/) vlastnost. Pokud je tato vlastnost nastavená, pak hodnota je předán `Convert` a `ConvertBack` metody jako `parameter` argument. I když instanci převaděč hodnoty je sdílena mezi několika datové vazby `ConverterParameter` mohou být různé provést poněkud jiné převody.

Použití `ConverterParameter` se demonstruje program výběr barev. V takovém případě `RgbColorViewModel` má tři vlastnosti typu `double` s názvem `Red`, `Green`, a `Blue` používající vytvořit `Color` hodnotu:

```csharp
public class RgbColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Red
    {
        set
        {
            if (color.R != value)
            {
                Color = new Color(value, color.G, color.B);
            }
        }
        get
        {
            return color.R;
        }
    }

    public double Green
    {
        set
        {
            if (color.G != value)
            {
                Color = new Color(color.R, value, color.B);
            }
        }
        get
        {
            return color.G;
        }
    }

    public double Blue
    {
        set
        {
            if (color.B != value)
            {
                Color = new Color(color.R, color.G, value);
            }
        }
        get
        {
            return color.B;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Red"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Green"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Blue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

`Red`, `Green`, A `Blue` vlastnosti rozsahu od 0 do 1. Může ale dáváte přednost tomu, že komponenty zobrazena jako letopočty šestnáctkové hodnoty.

Pokud chcete zobrazit jako hexadecimální hodnoty v jazyce XAML, se musí být násobí hodnotou 255, převést na celé číslo a ve formátu s specifikaci "X2" `StringFormat` vlastnost. Převaděč hodnoty může zpracovávat první dvě úlohy (vynásobí 255 a převádění na celá čísla). Chcete-li převaděč hodnoty jako zobecněn nejdříve, lze určit násobitel `ConverterParameter` vlastnost, což znamená, že zadá `Convert` a `ConvertBack` metody jako `parameter` argument:

```csharp
public class DoubleToIntConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)Math.Round((double)value * GetParameter(parameter));
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value / GetParameter(parameter);
    }

    double GetParameter(object parameter)
    {
        if (parameter is double)
            return (double)parameter;

        else if (parameter is int)
            return (int)parameter;

        else if (parameter is string)
            return double.Parse((string)parameter);

        return 1;
    }
}
```

`Convert` Převádí z `double` k `int` při vynásobí `parameter` value; `ConvertBack` rozdělí na celé číslo `value` argument podle `parameter` a vrátí `double` výsledek. (V programu je uvedeno níže, převaděč hodnoty se používá jen v rámci formátování řetězce, takže `ConvertBack` se nepoužívá.)

Typ `parameter` argument je pravděpodobně lišit v závislosti na tom, jestli je definována datové vazby v kódu nebo XAML. Pokud `ConverterParameter` vlastnost `Binding` nastavena v kódu, je pravděpodobně být nastavena na číselnou hodnotu:

```csharp
binding.ConverterParameter = 255;
```

`ConverterParameter` Vlastnost je typu `Object`, takže kompilátor jazyka C# literálu 255 interpretuje jako celé číslo a nastaví vlastnost na tuto hodnotu.

V jazyce XAML, ale `ConverterParameter` může nastavit takto:

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

255 vypadá jako číslo, ale protože `ConverterParameter` je typu `Object`, analyzátor XAML zpracovává 255 jako řetězec.

Z tohoto důvodu převaděč hodnoty výše uvedeném obsahuje samostatné `GetParameter` metoda, která zpracovává případů pro `parameter` se typu `double`, `int`, nebo `string`.  

**Selektor barva RGB** stránky vytvoří `DoubleToIntConverter` v jeho slovník prostředků definici dvě implicitní styly následující:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.RgbColorSelectorPage"
             Title="RGB Color Selector">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <local:DoubleToIntConverter x:Key="doubleToInt" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:RgbColorViewModel Color="Gray" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Red}" />
            <Label Text="{Binding Red,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Red = {0:X2}'}" />

            <Slider Value="{Binding Green}" />
            <Label Text="{Binding Green,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Green = {0:X2}'}" />

            <Slider Value="{Binding Blue}" />
            <Label>
                <Label.Text>
                    <Binding Path="Blue"
                             StringFormat="Blue = {0:X2}"
                             Converter="{StaticResource doubleToInt}">
                        <Binding.ConverterParameter>
                            <x:Double>255</x:Double>
                        </Binding.ConverterParameter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

Hodnoty `Red` a `Green` s se zobrazí vlastnosti `Binding` – rozšíření značek. `Blue` Vlastnost, ale vytvoří `Binding` třídy ukazují, jak pomocí explicitní `double` hodnota může být nastaven na `ConverterParameter` vlastnost.

Tady je výsledek:

[![Výběr barvy RGB](converters-images/rgbcolorselector-small.png "selektor barva RGB")](converters-images/rgbcolorselector-large.png#lightbox "selektor barva RGB")


## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Kapitola vazby dat z adresáře Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
