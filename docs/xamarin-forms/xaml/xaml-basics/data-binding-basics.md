---
title: Část 4. Základy vazba dat
description: Datové vazby povolit vlastnosti dvou objektů propojení tak, aby změnu v jednom způsobí změnu na druhý. To je velmi cenným nástrojem ke zjištění a během vazby dat lze definovat zcela v kódu, XAML poskytuje zkratky a usnadnění práce. V důsledku toho je vazba jedním z nejdůležitějších rozšíření značek v Xamarin.Forms.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 342288C3-BB4C-4924-B178-72E112D777BA
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 2aa6fd2f54c09921621a12af9401a6f84ae37ffa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="part-4-data-binding-basics"></a>Část 4. Základy vazba dat

_Datové vazby povolit vlastnosti dvou objektů propojení tak, aby změnu v jednom způsobí změnu na druhý. To je velmi cenným nástrojem ke zjištění a během vazby dat lze definovat zcela v kódu, XAML poskytuje zkratky a usnadnění práce. V důsledku toho je vazba jedním z nejdůležitějších rozšíření značek v Xamarin.Forms._

## <a name="data-bindings"></a>Datové vazby

Datové vazby připojit vlastnosti dva objekty, volá se *zdroj* a *cíl*. V kódu dva kroky jsou povinné: `BindingContext` vlastnost cílového objektu musí být nastavená na zdrojový objekt a `SetBinding` – metoda (často se používá ve spojení s `Binding` – třída) musí být volána v cílovém objektu k vytvoření vazby vlastnost, objekt, který chcete vlastnost zdrojový objekt.

Vlastnost target musí být vlastnost vazbu, což znamená, že cílový objekt musí být odvozeny od `BindableObject`. V online dokumentaci Xamarin.Forms určuje vlastnosti, které jsou vazbu vlastnosti. Vlastnost `Label` například `Text` je přidružena vazbu vlastnosti `TextProperty`.

V kódu, musí také provádět stejné dva kroky, které jsou potřeba v kódu, s tím rozdílem, že `Binding` – rozšíření značek přebírá místo `SetBinding` volání a `Binding` třídy.

Když definujete datové vazby v jazyce XAML, existují však několika způsoby, jak nastavit `BindingContext` cílového objektu. Někdy je nastaven ze souboru kódu na pozadí, někdy pomocí `StaticResource` nebo `x:Static` rozšíření značek a někdy jako obsah `BindingContext` značky element vlastnosti.

Vazby se nejčastěji používají k propojení vizuály programu s základní datový model, obvykle v zjištění aplikace architektury modelem MVVM (Model-View-ViewModel), jak je popsáno v [část 5. Z vazby dat na rozhraní MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), ale je možné, jiné scénáře.

## <a name="view-to-view-bindings"></a>Zobrazení zobrazení vazby

Můžete definovat vazby dat na propojení vlastnosti ze dvou zobrazení na stejné stránce. V takovém případě nastavte `BindingContext` cílový objekt pomocí `x:Reference` – rozšíření značek.

Tady je soubor XAML, který obsahuje `Slider` a dvě `Label` zobrazení, z nichž jeden je otáčet o `Slider` hodnotu a druhou, která zobrazuje `Slider` hodnotu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderBindingsPage"
             Title="Slider Bindings Page">

    <StackLayout>
        <Label Text="ROTATION"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Slider` Obsahuje `x:Name` atribut, který je odkazován objektem dva `Label` zobrazení pomocí `x:Reference` – rozšíření značek.

`x:Reference` Vazby rozšíření definuje vlastnost s názvem `Name` v takovém případě nastavte na název je odkazovaný element `slider`. Však `ReferenceExtension` třídu, která definuje `x:Reference` – rozšíření značek také definuje `ContentProperty` atribut pro `Name`, což znamená, že není výslovně požadováno. Pouze pro řadu první `x:Reference` zahrnuje "Name =" ale druhý nemá:

```csharp
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

`Binding` – Rozšíření značek samotné může mít několik vlastností, podobně jako `BindingBase` a `Binding` třídy. `ContentProperty` Pro `Binding` je `Path`, ale "cesta =" součástí rozšíření značek lze vynechat, pokud se cesta první položky v `Binding` – rozšíření značek. V prvním příkladu má "cesta =", ale v druhém příkladu vynechá ho:

```csharp
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Vlastnosti může být na jeden řádek, nebo rozdělené na více řádků:

```csharp
Text="{Binding Value, 
               StringFormat='The angle is {0:F0} degrees'}"
```

To vše, co je vhodné.

Upozornění `StringFormat` vlastnost za sekundu `Binding` – rozšíření značek. V Xamarin.Forms, vazby neprovádět všechny převody implicitní typů, a pokud potřebujete zobrazit jiné než řetězec objekt jako řetězec musí poskytovat převaděče typů nebo použijte `StringFormat`. Na pozadí, statické `String.Format` metoda se používá k implementaci `StringFormat`. Který potenciálně problém je, protože formátování specifikace .NET zahrnují složené závorky, které slouží k oddělení rozšíření značek. Vytvoří se tím riziko složitá analyzátor jazyka XAML. Abyste tomu zabránili, put celý řetězec formátování do jednoduchých uvozovek:

```csharp
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Tady je spuštěným programem:

[![](data-binding-basics-images/sliderbinding.png "Zobrazení zobrazení vazby")](data-binding-basics-images/sliderbinding-large.png#lightbox "vazby zobrazení – zobrazení ")

## <a name="the-binding-mode"></a>Režim vazby 

Jediné zobrazení mohou mít vazby dat na některé jeho vlastnosti. Každé zobrazení však může mít jen jeden `BindingContext`, takže více vazby dat v tomto zobrazení, musí všechny vlastnosti objektu stejné odkazovat.

Zahrnuje řešení, aby tato a další problémy `Mode` vlastnost, která je nastavena na hodnotu členem `BindingMode` výčtu:

- `Default` 
- `OneWay` – hodnoty jsou převést ze zdrojové do cílové
- `OneWayToSource` – hodnoty jsou přeneseny z cíle na zdroj
- `TwoWay` – hodnoty přenášených obou směrech mezi zdrojem a cílem

Následující program znázorňuje jedno běžné použití `OneWayToSource` a `TwoWay` vazby režimy. Čtyři `Slider` zobrazení jsou určeny k řízení `Scale`, `Rotate`, `RotateX`, a `RotateY` vlastnosti `Label`. Na první pohled se zdá být jakoby tyto čtyři vlastnosti `Label` by měla být cíle vazby dat, protože každý se nastavuje `Slider`. Ale `BindingContext` z `Label` může být pouze jeden objekt, a neexistují čtyř různých posuvníky.

Z tohoto důvodu všechny vazby jsou nastaveny v zdánlivě zpětné způsoby: `BindingContext` každé ze čtyř posuvníků je nastaven na `Label`, a zda vazby jsou nastaveny na `Value` vlastnosti posuvníků. Pomocí `OneWayToSource` a `TwoWay` režimy, tyto `Value` vlastnosti můžete nastavit vlastnosti zdroje, které jsou `Scale`, `Rotate`, `RotateX`, a `RotateY` vlastnosti `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderTransformsPage"
             Padding="5"
             Title="Slider Transforms Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <!-- Scaled and rotated Label -->
        <Label x:Name="label"
               Text="TEXT"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <!-- Slider and identifying Label for Scale -->
        <Slider x:Name="scaleSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="1" Grid.Column="0"
                Maximum="10"
                Value="{Binding Scale, Mode=TwoWay}" />

        <Label BindingContext="{x:Reference scaleSlider}"
               Text="{Binding Value, StringFormat='Scale = {0:F1}'}"
               Grid.Row="1" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for Rotation -->
        <Slider x:Name="rotationSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="2" Grid.Column="0"
                Maximum="360"
                Value="{Binding Rotation, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationSlider}"
               Text="{Binding Value, StringFormat='Rotation = {0:F0}'}"
               Grid.Row="2" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationX -->
        <Slider x:Name="rotationXSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="3" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationX, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationXSlider}"
               Text="{Binding Value, StringFormat='RotationX = {0:F0}'}"
               Grid.Row="3" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationY -->
        <Slider x:Name="rotationYSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="4" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationY, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationYSlider}"
               Text="{Binding Value, StringFormat='RotationY = {0:F0}'}"
               Grid.Row="4" Grid.Column="1"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

Vazby na tři `Slider` zobrazení jsou `OneWayToSource`znamená, `Slider` hodnotu způsobí, že ke změně ve vlastnosti jeho `BindingContext`, který je `Label` s názvem `label`. Tyto tři `Slider` zobrazení způsobit změny `Rotate`, `RotateX`, a `RotateY` vlastnosti `Label`.

Ale vazby pro `Scale` vlastnost je `TwoWay`. Důvodem je, že `Scale` vlastnost má výchozí hodnotu 1 a pomocí `TwoWay` vazby příčiny `Slider` počáteční hodnota, která má být nastavena na 1 a nikoli na 0. Pokud byly vazbou `OneWayToSource`, `Scale` vlastnost by původně nastavena na hodnotu 0 z `Slider` výchozí hodnota. `Label` Nebudou viditelné a který může být úplně jasné uživateli.

 [![](data-binding-basics-images/slidertransforms.png "Zpětné vazby")](data-binding-basics-images/slidertransforms-large.png#lightbox "zpětné vazby")

## <a name="bindings-and-collections"></a>Vazby a kolekce

Nic znázorňuje výkon XAML a datové vazby lepší, než použitím šablon `ListView`.

`ListView` definuje `ItemsSource` vlastnost typu `IEnumerable`, a zobrazí položky v dané kolekci. Tyto položky mohou být objekty libovolného typu. Ve výchozím nastavení `ListView` používá `ToString` metoda každé položky k zobrazení této položky. V některých případech toto je právě co chcete, ale v mnoha případech `ToString` vrátí třída plně kvalifikovaný název objektu.

Ale položek v `ListView` kolekci lze zobrazit libovolným způsobem prostřednictvím *šablony*, což zahrnuje třídy, která je odvozena z `Cell`. Šablona je klonovat pro každou položku v `ListView`, a vazby dat, které byly nastaveny na šabloně jsou přeneseny na jednotlivé klonovat.

Velmi často budete chtít vytvořit vlastní buňky pro tyto položky pomocí `ViewCell` třídy. Tento proces je poněkud komplikované v kódu, ale v jazyce XAML bude velmi jednoduché.

Součástí XamlSamples je projekt třídy s názvem `NamedColor`. Každý `NamedColor` objekt má `Name` a `FriendlyName` vlastnosti typu `string`a `Color` vlastnost typu `Color`. Kromě toho `NamedColor` má 141 statická jen pro čtení pole typu `Color` odpovídající barev definovaných v platformě Xamarin.Forms `Color` třídy. Statický konstruktor vytvoří `IEnumerable<NamedColor>` kolekce, která obsahuje `NamedColor` objekty odpovídající tyto statická pole a přiřadí ji k jeho veřejné statické `All` vlastnost.

Nastavení statické `NamedColor.All` vlastnost, která má `ItemsSource` z `ListView` je snadné použití `x:Static` – rozšíření značek:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

Výsledné zobrazení vytváří skutečně jsou položky typu `XamlSamples.NamedColor`:

[![](data-binding-basics-images/listview1.png "Vazby ke kolekci")](data-binding-basics-images/listview1-large.png#lightbox "vazby ke kolekci")

Není velkého množství informací, ale `ListView` posouvatelného a které lze vybírat.

Definování šablon pro položky, budete chtít přerušení `ItemTemplate` vlastnost jako vlastnost prvek a nastavte ji na `DataTemplate`, které pak odkazy `ViewCell`. K `View` vlastnost `ViewCell` můžete definovat rozložení jeden nebo více pohledů pro zobrazení jednotlivých položek. Zde je jednoduchý příklad:

```xaml
<ListView ItemsSource="{x:Static local:NamedColor.All}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.View>
                    <Label Text="{Binding FriendlyName}" />
                </ViewCell.View>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

`Label` Element je nastaven na hodnotu `View` vlastnost `ViewCell`. ( `ViewCell.View` Značky nejsou potřebné, protože `View` vlastnost je vlastnost obsahu `ViewCell`.) Tento kód zobrazí `FriendlyName` vlastnost jednotlivých `NamedColor` objektu:

[![](data-binding-basics-images/listview2.png "Vytvoření vazby na kolekci s šablonu DataTemplate")](data-binding-basics-images/listview2-large.png#lightbox "vazby ke kolekci s šablonu DataTemplate")

Mnohem lepší. Nyní všechno, co je potřeba je smrk až šablony položky s další informace a skutečný barvu. Pro podporu této šablony, byly některé hodnoty a objekty definovány v slovník prostředků stránky:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <OnPlatform x:Key="boxSize"
                        x:TypeArguments="x:Double">
                <On Platform="iOS, Android, UWP" Value="50" />
            </OnPlatform>

            <OnPlatform x:Key="rowHeight"
                        x:TypeArguments="x:Int32">
                <On Platform="iOS, Android, UWP" Value="60" />
            </OnPlatform>

            <local:DoubleToIntConverter x:Key="intConverter" />

        </ResourceDictionary>
    </ContentPage.Resources>

    <ListView ItemsSource="{x:Static local:NamedColor.All}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <StackLayout Padding="5, 5, 0, 5"
                                 Orientation="Horizontal"
                                 Spacing="15">

                        <BoxView WidthRequest="{StaticResource boxSize}"
                                 HeightRequest="{StaticResource boxSize}"
                                 Color="{Binding Color}" />

                        <StackLayout Padding="5, 0, 0, 0"
                                     VerticalOptions="Center">

                            <Label Text="{Binding FriendlyName}"
                                   FontAttributes="Bold"
                                   FontSize="Medium" />

                            <StackLayout Orientation="Horizontal"
                                         Spacing="0">
                                <Label Text="{Binding Color.R,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat='R={0:X2}'}" />

                                <Label Text="{Binding Color.G,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', G={0:X2}'}" />

                                <Label Text="{Binding Color.B,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', B={0:X2}'}" />
                            </StackLayout>
                        </StackLayout>
                    </StackLayout>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

Všimněte si použití `OnPlatform` zadat velikost `BoxView` a výšku `ListView` řádků. I když hodnoty pro všechny tři platformy jsou stejné, může být kód snadno přizpůsobit pro ostatní hodnoty a systém doladit zobrazení. 

## <a name="binding-value-converters"></a>Převodníky hodnot vazby

Předchozí **ListView ukázku** souboru XAML zobrazuje jednotlivý `R`, `G`, a `B` vlastnosti platformě Xamarin.Forms `Color` struktury. Tyto vlastnosti jsou typu `double` a rozsahu od 0 do 1. Pokud chcete zobrazit hexadecimální hodnoty, nebudete moct používat jednoduše `StringFormat` s "X2" formátování specifikace. Funguje, jenom na celá čísla a kromě toho, `double` hodnoty se musí být násobí hodnotou 255.

Malé potíže se vyřeší s *převaděč hodnoty*, také zavolat *vazby převaděč*. Toto je třída, která implementuje `IValueConverter` rozhraní, což znamená, má dvě metody s názvem `Convert` a `ConvertBack`. `Convert` Metoda je volána při přenosu hodnotu ze zdrojového na cílový; `ConvertBack` metoda je volána pro přenosy z cíle zdroji v `OneWayToSource` nebo `TwoWay` vazby:

```csharp
using System;
using System.Globalization;
using Xamarin.Forms;

namespace XamlSamples
{
    class DoubleToIntConverter : IValueConverter
    {
        public object Convert(object value, Type targetType,
                              object parameter, CultureInfo culture)
        {
            double multiplier;

            if (!Double.TryParse(parameter as string, out multiplier))
                multiplier = 1;

            return (int)Math.Round(multiplier * (double)value);
        }

        public object ConvertBack(object value, Type targetType,
                                  object parameter, CultureInfo culture)
        {
            double divider;

            if (!Double.TryParse(parameter as string, out divider))
                divider = 1;

            return ((double)(int)value) / divider;
        }
    }
}
```

`ConvertBack` Metoda nehraje roli v tomto programu, protože vazby jsou pouze jedním ze způsobů ze zdroje k cíli. 

Vazba odkazuje převaděč vazba s `Converter` vlastnost. Převaděč vazba může také přijmout parametr zadaný `ConverterParameter` vlastnost. Pro některé univerzálnost Toto je, jak je zadán násobitel. Převaděč vazby kontroluje parametr převaděč pro platná `double` hodnotu.

Převaděč je vytvořena instance ve slovníku prostředků, může být sdílen více vazby:

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

Tři datové vazby odkazovat na tuto jediné instance. Všimněte si, že `Binding` – rozšíření značek obsahuje embedded `StaticResource` – rozšíření značek:

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

Tady je výsledek:

[![](data-binding-basics-images/listview3.png "Vytvoření vazby na kolekci s šablonu DataTemplate a převaděče")](data-binding-basics-images/listview3-large.png#lightbox "vazby ke kolekci s šablonu DataTemplate a převaděče")

`ListView` Je poměrně složité při zpracování změn, které můžou nastat dynamicky v základní data, ale jenom v případě provést určité kroky. Pokud kolekce položek přiřazen k `ItemsSource` vlastnost `ListView` změny během doby běhu –, pokud položky můžete přidat do nebo z kolekce odebrán – použijte `ObservableCollection` třídu pro tyto položky. `ObservableCollection` implementuje `INotifyCollectionChanged` rozhraní, a `ListView` nainstaluje obslužnou rutinu pro `CollectionChanged` událostí.

Pokud vlastnosti položek sami změnit za běhu, pak by měla implementovat položky v kolekci `INotifyPropertyChanged` rozhraní a signál změny hodnot vlastností, pomocí `PropertyChanged` událostí. Tento postup je znázorněn v další části této série [část 5. Z datové vazby k rozhraní MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Souhrn

Datové vazby poskytují výkonný mechanismus pro propojení vlastnosti mezi dvěma objekty v rámci stránky, nebo mezi vizuální objekty a základní data. Ale po zahájení práce s zdroje dat aplikace architekturní vzor oblíbených aplikací začne vznikat jako užitečné zlepší. To je popsaná v [část 5. Z vazby dat na rozhraní MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).



## <a name="related-links"></a>Související odkazy

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Část 1. Začínáme s XAML (ukázka)](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Část 2. Základní syntaxe jazyka XAML (ukázka)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Část 3. Rozšíření značek XAML (ukázka)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Část 5. Z datové vazby k rozhraní MVVM (ukázka)](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
