---
title: Převaděče hodnot vazeb Xamarin.Forms
description: Tento článek vysvětluje, jak přetypujte nebo převeďte hodnot v rámci datové vazby Xamarin.Forms implementací převaděč hodnot (což je také převaděč vazby nebo převaděč hodnoty vazby).
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 28892692133020de1fa5a6eb007bb3f9bcf2612b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997477"
---
# <a name="xamarinforms-binding-value-converters"></a>Převaděče hodnot vazeb Xamarin.Forms

Datové vazby pro vlastnost target a v některých případech vlastnost Cílová vlastnost source obvykle přenést data z vlastnost source. Tento přenos je jednoduché, když vlastnosti zdroje a cíle jsou stejného typu, nebo když jednoho typu lze převést na jiný typ prostřednictvím implicitního převodu. Když to není tento případ, převod typu musí proběhnout.

V [ **formátování řetězce** ](string-formatting.md) článku jste viděli, jak můžete použít `StringFormat` vlastnost datovou vazbu pro jakýkoli typ převést na řetězec. Pro ostatní typy převodů, budete muset napsat specializovaný kód do třídy, která implementuje [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) rozhraní. (Univerzální platforma Windows obsahuje podobné třídu s názvem [ `IValueConverter` ](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) v `Windows.UI.Xaml.Data` obor názvů, ale to `IValueConverter` probíhá `Xamarin.Forms` oboru názvů.) Třídy, které implementují `IValueConverter` se nazývají *převaděče hodnoty*, ale že se také často označují jako *vazby převaděče* nebo *převaděče hodnot vazeb*.

## <a name="the-ivalueconverter-interface"></a>Rozhraní IValueConverter

Předpokládejme, že chcete definovat datové vazby, kde je vlastnost source typu `int` , ale je vlastnost target `bool`. Chcete, aby tato vazba dat k vytvoření `false` hodnotu, pokud je zdroj celé číslo rovno 0, a `true` jinak.  

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

Nastavit instanci této třídy pro [ `Converter` ](xref:Xamarin.Forms.Binding.Converter) vlastnost `Binding` třídy nebo [ `Converter` ](xref:Xamarin.Forms.Xaml.BindingExtension.Converter) vlastnost `Binding` – rozšíření značek. Tato třída se stane součástí datové vazby.

`Convert` Metoda se volá, když k přesunu dat ze zdroje do cíle v `OneWay` nebo `TwoWay` vazby. `value` Parametru je objekt nebo hodnotu ze zdroje dat neumožňující vazbu. Metoda musí vracet hodnotu typu cíle datové vazby. Metoda je vidět tady přetypování `value` parametr `int` a porovná ho s 0 pro `bool` návratovou hodnotu.

`ConvertBack` Metoda se volá, když data se přesouvají z cíle na zdroj v `TwoWay` nebo `OneWayToSource` vazby. `ConvertBack` provádí opačnou převod: předpokládá `value` parametr je `bool` z cíle a převede jej na `int` návratovou hodnotu pro zdroj.

Pokud také obsahuje datové vazby `StringFormat` nastavení, převaděč hodnoty vyvolání dříve, než je formátováno výsledek jako řetězec.

**Tlačítka povolte** stránku [ **ukázky vazby dat** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) Ukázka předvádí, jak používat tento převaděč hodnoty v datové vazbě. `IntToBoolConverter` Je vytvořena instance ve slovníku prostředků na stránce. Je pak odkazuje se `StaticResource` – rozšíření značek k nastavení `Converter` vlastnost ve dvou datových vazbách. Je velmi běžné sdílet převaděče data mezi víc vazeb dat na stránce:

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

Pokud převaděč hodnoty se používá ve více stránek vaší aplikace, můžete vytvořit instanci ho do slovníku prostředků **App.xaml** souboru.

**Tlačítka povolte** stránce ukazuje společného potřebovat při `Button` provádí operaci na základě textu, který uživatel zadá do `Entry` zobrazení. Pokud není nic byla zadána do `Entry`, `Button` by mělo být zakázáno. Každý `Button` obsahuje datové vazby v jeho `IsEnabled` vlastnost. Zdroj datové vazby je `Length` vlastnost `Text` vlastnosti k odpovídající položce `Entry`. Pokud to `Length` jedná o vlastnost neumožňující převaděč hodnoty 0, vrátí `true` a `Button` zapnutá:

[![Povolit tlačítka](converters-images/enablebuttons-small.png "povolení tlačítka")](converters-images/enablebuttons-large.png#lightbox "povolit tlačítek")

Všimněte si, že `Text` vlastnost v každém `Entry` je inicializován na prázdný řetězec. `Text` Vlastnost `null` ve výchozím nastavení a data vazby nebude fungovat v takovém případě.

Některé převaděče hodnot jsou psány konkrétně pro konkrétní aplikace, zatímco ostatní zobecněný. Pokud víte, že převaděč hodnoty se použijí jen v `OneWay` vazby, pak bude `ConvertBack` metoda může jednoduše provést vrácení `null`.

`Convert` Výše uvedená metoda implicitně předpokládá, `value` argument je typu `int` a vrácená hodnota musí být typu `bool`. Podobně `ConvertBack` metoda předpokládá, že `value` argument je typu `bool` a vrácená hodnota je `int`. Pokud to není tento případ, dojde k výjimce modulu runtime.

Převodníky hodnot více zobecnit a přijímat několik různých typů dat, můžete napsat. `Convert` a `ConvertBack` metody můžete použít `as` nebo `is` operátory se `value` parametr, nebo může volat `GetType` na tento parametr k určení jeho typu a potom proveďte něco vhodné. Očekávaný typ jednotlivých metod návratová hodnota je dán `targetType` parametru. V některých případech se používají převaděče hodnot s jinou cílovou typů; datové vazby Převaděč hodnoty můžete použít `targetType` argument k provedení převodu správného typu.

Pokud právě probíhá převod se liší pro různé jazykové verze, použijte `culture` parametr pro tento účel. `parameter` Argument `Convert` a `ConvertBack` je popsána dále v tomto článku.

## <a name="binding-converter-properties"></a>Převaděč vlastnosti vazby

Třídy převaděče hodnot mohou mít vlastnosti a obecných parametrů. Převede tento převaděč určitou hodnotu `bool` ze zdroje do objektu typu `T` pro cíl:

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

**Přepnout indikátory** stránky ukazuje, jak může být použita k zobrazení hodnoty `Switch` zobrazení. I když je společné pro vytvoření instance převaděče hodnot jako prostředky ve slovníku prostředků, na této stránce ukazuje alternativu: každý převaděč hodnoty je vytvořena instance mezi `Binding.Converter` značky element vlastnosti. `x:TypeArguments` Určuje obecný argument a `TrueObject` a `FalseObject` jsou nastaveny na objekty tohoto typu:

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

Za posledních tří `Switch` a `Label` dvojice, obecný argument je nastaven na `Style`a celé `Style` objekty jsou k dispozici pro hodnoty `TrueObject` a `FalseObject`. Tyto přepsat implicitní styl `Label` nastavit slovník prostředků, takže vlastností ve stylu jsou explicitně přiřazeny `Label`. Při přepnutí `Switch` způsobí, že odpovídající `Label` tak, aby odrážely změny:

[![Přepnout indikátory](converters-images/switchindicators-small.png "Přepnout indikátory")](converters-images/switchindicators-large.png#lightbox "Přepnout indikátory")

Je také možné použít [ `Triggers` ](~/xamarin-forms/app-fundamentals/triggers.md) provádět podobné změny v uživatelském rozhraní založené na jiné zobrazení.

## <a name="binding-converter-parameters"></a>Převaděč parametry vazby

`Binding` Definuje třídu [ `ConverterParameter` ](xref:Xamarin.Forms.Binding.ConverterParameter) vlastnost a `Binding` také definuje – rozšíření značek [ `ConverterParameter` ](xref:Xamarin.Forms.Xaml.BindingExtension.ConverterParameter) vlastnost. Pokud je tato vlastnost nastavena, pak hodnota předaná `Convert` a `ConvertBack` metody jako `parameter` argument. I v případě, že instance převaděč hodnoty je sdílen mezi několika datové vazby `ConverterParameter` se může lišit pro převod poněkud liší.

Použití `ConverterParameter` je znázorněn s programem výběr barvy. V takovém případě `RgbColorViewModel` má tři vlastnosti typu `double` s názvem `Red`, `Green`, a `Blue` , který se používá k vytvoření `Color` hodnotu:

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

`Red`, `Green`, A `Blue` vlastnosti rozsahu od 0 do 1. Ale můžete dát přednost, že součásti bude zobrazen jako dvouciferných šestnáctkových hodnot.

Pokud chcete zobrazit jako šestnáctkové hodnoty v XAML, musí být vynásobené 255, převést na celé číslo a ve formátu specifikace "X2" `StringFormat` vlastnost. První dva úkoly (vynásobí 255 a převod na celé číslo) mohou být zpracovány převaděč hodnoty. Převaděč hodnoty za generalizovaný nejdříve proveďte násobitel je možné zadat při `ConverterParameter` vlastnost, což znamená, že přejde `Convert` a `ConvertBack` metody jako `parameter` argument:

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

`Convert` Převede z `double` k `int` při vynásobí `parameter` hodnota; `ConvertBack` rozdělí na celé číslo `value` argumentu ve `parameter` a vrátí `double` výsledek. (V programu je uvedeno níže, převaděč hodnoty se používá jenom ve spojení s formátování řetězce, takže `ConvertBack` se nepoužívá.)

Typ `parameter` argument je pravděpodobně lišit v závislosti na tom, zda je definován datové vazby v kódu nebo XAML. Pokud `ConverterParameter` vlastnost `Binding` je nastavena v kódu, je pravděpodobně být nastavena na číselnou hodnotu:

```csharp
binding.ConverterParameter = 255;
```

`ConverterParameter` Vlastnost je typu `Object`, takže kompilátor jazyka C# literálu 255 interpretuje jako celé číslo a nastaví vlastnost k dané hodnotě.

V XAML, ale `ConverterParameter` by mohla být nastavit takto:

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

255 vypadá jako číslo, ale protože `ConverterParameter` je typu `Object`, analyzátoru XAML považuje za řetězec 255.

Z tohoto důvodu převaděč hodnoty uvedené nahoře zahrnuje samostatné `GetParameter` metoda, která zpracovává případy `parameter` typ `double`, `int`, nebo `string`.  

**Výběr barvy RGB** vytvoří instanci stránky `DoubleToIntConverter` v následující definici dvě implicitní styly slovníku prostředků:

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

Hodnoty `Red` a `Green` vlastnosti jsou zobrazovány `Binding` – rozšíření značek. `Blue` Vlastností, ale vytvoří instanci `Binding` třídy k předvedení jak explicitní `double` hodnota může být nastavena na `ConverterParameter` vlastnost.

Tady je výsledek:

[![Výběr barvy RGB](converters-images/rgbcolorselector-small.png "výběr barvy RGB")](converters-images/rgbcolorselector-large.png#lightbox "výběr barvy RGB")


## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Data vazby kapitola z knihy Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
