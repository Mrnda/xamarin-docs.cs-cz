---
title: Část 5. Z vazby dat na rozhraní MVVM
description: Architekturní vzor Model-View-ViewModel (modelem MVVM) byla vyvinuta s XAML v paměti. Vzor vynucuje oddělení mezi tři vrstvy softwaru – uživatelské rozhraní jazyka XAML volat zobrazení; Základní data, nazývá Model; a zprostředkovatel mezi zobrazení a modelu s názvem ViewModel. Zobrazení a ViewModel jsou často připojené prostřednictvím vazby dat, které jsou definované v souboru XAML. Vazby pro zobrazení je obvykle instanci ViewModel.
ms.prod: xamarin
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 95cd79a4bd6da47757cfeb12a2862ccb5a66fee2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>Část 5. Z vazby dat na rozhraní MVVM

_Architekturní vzor Model-View-ViewModel (modelem MVVM) byla vyvinuta s XAML v paměti. Vzor vynucuje oddělení mezi tři vrstvy softwaru – uživatelské rozhraní jazyka XAML volat zobrazení; Základní data, nazývá Model; a zprostředkovatel mezi zobrazení a modelu s názvem ViewModel. Zobrazení a ViewModel jsou často připojené prostřednictvím vazby dat, které jsou definované v souboru XAML. Vazby pro zobrazení je obvykle instanci ViewModel._

## <a name="a-simple-viewmodel"></a>Jednoduché ViewModel

Jako úvod do ViewModels nejprve Podíváme se na program bez jeden.
Dříve jste viděli, jak definovat novou deklaraci oboru názvů XML povolíte souboru XAML na referenční třídy v ostatních sestavení. Tady je program, který definuje deklarace oboru názvů XML pro `System` obor názvů:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Můžete použít program `x:Static` získat aktuální datum a čas z statických `DateTime.Now` vlastnost a nastavte ho `DateTime` hodnotu `BindingContext` na `StackLayout`:

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext` je velmi speciální vlastnost: Když nastavíte `BindingContext` na element, zdědí všechny podřízené objekty tohoto elementu. To znamená, že všechny podřízené objekty daného `StackLayout` mají tato stejné `BindingContext`, a mohou obsahovat jednoduché vazby na vlastnosti daného objektu.

V **One-Shot DateTime** programu, dva podřízených obsahovat vazby na vlastnosti této `DateTime` hodnoty, ale dva další podřízené objekty obsahovat vazby, které se zdá, že chybí cesta vazby. To znamená, že `DateTime` vlastní hodnota se používá pro `StringFormat`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.OneShotDateTimePage"
             Title="One-Shot DateTime Page">

    <StackLayout BindingContext="{x:Static sys:DateTime.Now}"
                 HorizontalOptions="Center"
                 VerticalOptions="Center">

        <Label Text="{Binding Year, StringFormat='The year is {0}'}" />
        <Label Text="{Binding StringFormat='The month is {0:MMMM}'}" />
        <Label Text="{Binding Day, StringFormat='The day is {0}'}" />
        <Label Text="{Binding StringFormat='The time is {0:T}'}" />

    </StackLayout>
</ContentPage>
```

Samozřejmě velký problém je, že datum a čas sady po při stránce první a nikdy změnit:

[![](data-bindings-to-mvvm-images/oneshotdatetime.png "Zobrazení zobrazení datum a čas")](data-bindings-to-mvvm-images/oneshotdatetime-large.png#lightbox "zobrazení zobrazení datum a čas")

Soubor XAML můžete zobrazit hodiny, které vždy zobrazí aktuální čas, ale potřebuje nějaký kód, abyste. Když přemýšlení z hlediska modelem MVVM, Model a ViewModel jsou třídy, které jsou vytvořené zcela v kódu. Zobrazení je často souboru XAML, který odkazuje na vlastnosti definované v ViewModel prostřednictvím datové vazby.

Model správné je které ignorují z ViewModel a správné ViewModel zobrazení, které ignorují. Však velmi často programátorem přizpůsobuje jim datové typy vystavené ViewModel na datové typy související s konkrétní uživatelská rozhraní. Například pokud Model přistupuje k databázi, která obsahuje řetězce 8bitové znaků ASCII, ViewModel potřebovat pro převod mezi tyto řetězce řetězců v kódu Unicode pro přizpůsobení výhradní použití Unicode v uživatelském rozhraní.

V jednoduché příklady, jak rozhraní MVVM (jako jsou ty zobrazené) často neexistuje Model vůbec a vzor zahrnuje právě zobrazení a ViewModel propojené s datové vazby.

Tady je ViewModel pro času s právě jednu vlastnost s názvem `DateTime`, ale které aktualizace, které `DateTime` vlastnost každou sekundu:

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    class ClockViewModel : INotifyPropertyChanged
    {
        DateTime dateTime;

        public event PropertyChangedEventHandler PropertyChanged;

        public ClockViewModel()
        {
            this.DateTime = DateTime.Now;

            Device.StartTimer(TimeSpan.FromSeconds(1), () =>
                {
                    this.DateTime = DateTime.Now;
                    return true;
                });
        }

        public DateTime DateTime
        {
            set
            {
                if (dateTime != value)
                {
                    dateTime = value;

                    if (PropertyChanged != null)
                    {
                        PropertyChanged(this, new PropertyChangedEventArgs("DateTime"));
                    }
                }
            }
            get
            {
                return dateTime;
            }
        }
    }
}
```

Obecně implementovat ViewModels `INotifyPropertyChanged` rozhraní, což znamená, že třída aktivuje `PropertyChanged` událost vždy, když jedna z vlastností změny. Mechanismus vazby dat v Xamarin.Forms připojí k tomuto obslužnou rutinu `PropertyChanged` události, může být upozorněni, když vlastnost změny a udržování cíl aktualizován na novou hodnotu.

Hodiny, které podle této ViewModel může být stejně jednoduché jako tento:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ClockPage"
             Title="Clock Page">

    <Label Text="{Binding DateTime, StringFormat='{0:T}'}"
           FontSize="Large"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Label.BindingContext>
            <local:ClockViewModel />
        </Label.BindingContext>
    </Label>
</ContentPage>
```

Všimněte si jak `ClockViewModel` je nastaven na `BindingContext` z `Label` pomocí značek element vlastnosti. Alternativně můžete vytvořit instanci `ClockViewModel` v `Resources` kolekce a nastavte ji na `BindingContext` prostřednictvím `StaticResource` – rozšíření značek. Nebo můžete vytvořit soubor kódu instanci ViewModel.

`Binding` – Rozšíření značek na `Text` vlastnost `Label` formáty `DateTime` vlastnost. Zde je zobrazení:

[![](data-bindings-to-mvvm-images/clock.png "Zobrazení zobrazení datum a čas prostřednictvím ViewModel")](data-bindings-to-mvvm-images/clock-large.png#lightbox "zobrazení datum a čas prostřednictvím ViewModel zobrazení")

Je také možné přístup jednotlivé vlastnosti `DateTime` vlastnost ViewModel oddělením vlastnosti s dobami:

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>Interaktivní rozhraní MVVM

Rozhraní MVVM se velmi často používá s obousměrný datové vazby pro interaktivní zobrazení založené na základní datový model.

Tady je třídy s názvem `HslViewModel` , která převádí `Color` hodnotu do `Hue`, `Saturation`, a `Luminosity` hodnoty a naopak:

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    public class HslViewModel : INotifyPropertyChanged
    {
        double hue, saturation, luminosity;
        Color color;

        public event PropertyChangedEventHandler PropertyChanged;

        public double Hue
        {
            set
            {
                if (hue != value)
                {
                    hue = value;
                    OnPropertyChanged("Hue");
                    SetNewColor();
                }
            }
            get
            {
                return hue;
            }
        }

        public double Saturation
        {
            set
            {
                if (saturation != value)
                {
                    saturation = value;
                    OnPropertyChanged("Saturation");
                    SetNewColor();
                }
            }
            get
            {
                return saturation;
            }
        }

        public double Luminosity
        {
            set
            {
                if (luminosity != value)
                {
                    luminosity = value;
                    OnPropertyChanged("Luminosity");
                    SetNewColor();
                }
            }
            get
            {
                return luminosity;
            }
        }

        public Color Color
        {
            set
            {
                if (color != value)
                {
                    color = value;
                    OnPropertyChanged("Color");

                    Hue = value.Hue;
                    Saturation = value.Saturation;
                    Luminosity = value.Luminosity;
                }
            }
            get
            {
                return color;
            }
        }

        void SetNewColor()
        {
            Color = Color.FromHsla(Hue, Saturation, Luminosity);
        }

        protected virtual void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Změny `Hue`, `Saturation`, a `Luminosity` vlastnosti Příčina `Color` vlastnost změnit a změny `Color` způsobí, že ostatní tři vlastnosti chcete změnit. Může se to zdát jako nekonečnou smyčku, s tím rozdílem, že třída nepodporuje vyvolat `PropertyChanged` událostí není-li vlastnost ve skutečnosti se změnila. To element end vloží do jinak neovladatelném zpětné vazby.

Obsahuje následující soubor XAML `BoxView` jejichž `Color` vlastnost je vázána na `Color` vlastnost ViewModel a tři `Slider` a tři `Label` zobrazení vázána na `Hue`, `Saturation`a `Luminosity` vlastnosti:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.HslColorScrollPage"
             Title="HSL Color Scroll Page">
    <ContentPage.BindingContext>
        <local:HslViewModel Color="Aqua" />
    </ContentPage.BindingContext>

    <StackLayout Padding="10, 0">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Hue, Mode=TwoWay}" />

        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Saturation, Mode=TwoWay}" />

        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Luminosity, Mode=TwoWay}" />
    </StackLayout>
</ContentPage>
```

Vazba na každém `Label` je výchozí `OneWay`. Stačí, když se zobrazí hodnotu. Ale vazbu na každém `Slider` je `TwoWay`. To umožňuje `Slider` inicializované ze ViewModel. Všimněte si, že `Color` je nastavena na `Blue` při vytvoření instance ViewModel. Ke změně, ale `Slider` také je potřeba nastavit novou hodnotu pro vlastnost v ViewModel, který pak vypočítá novou barvu.

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "Rozhraní MVVM pomocí obousměrné vazby dat")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "rozhraní MVVM pomocí obousměrné vazby dat")

## <a name="commanding-with-viewmodels"></a>Tvorba příkazů s ViewModels

V mnoha případech je omezen na zpracování datové položky vzoru rozhraní MVVM: datové objekty v ViewModel paralelní objekty uživatelského rozhraní v zobrazení.

Ale někdy zobrazení musí obsahovat tlačítka, která spouštět různé akce v ViewModel. Ale nesmí obsahovat ViewModel `Clicked` obslužné rutiny pro tlačítka vzhledem k tomu, který by tie ViewModel k zlepší konkrétní uživatelského rozhraní.

Chcete-li povolit ViewModels více nezávislých objektů, určitého uživatelského rozhraní, ale stále povolit metody, která se má volat v rámci ViewModel, *příkaz* rozhraní existuje. Tento příkaz rozhraní podporuje následující prvky v Xamarin.Forms:

-  `Button`
-  `MenuItem`
-  `ToolbarItem`
-  `SearchBar`
-  `TextCell` (a tím i taky `ImageCell`)
-  `ListView`
-  `TapGestureRecognizer`

S výjimkou produktů `SearchBar` a `ListView` elementu, tyto prvky definovat dvě vlastnosti:

-  `Command` typu  `System.Windows.Input.ICommand`
-  `CommandParameter` typu  `Object`

`SearchBar` Definuje `SearchCommand` a `SearchCommandParameter` vlastnosti, když `ListView` definuje `RefreshCommand` vlastnost typu `ICommand`.

`ICommand` Rozhraní definuje dvě metody a jedna událost:

-  `void Execute(object arg)`
-  `bool CanExecute(object arg)`
-  `event EventHandler CanExecuteChanged`

ViewModel můžete definovat vlastnosti typu `ICommand`. Pak vytvořte vazbu těchto vlastností `Command` vlastnost jednotlivých `Button` nebo jiného elementu, nebo možná vlastní zobrazení, které toto rozhraní implementuje. Volitelně můžete nastavit `CommandParameter` vlastnost k identifikaci jednotlivých `Button` objekty (nebo jiných prvků), je vázána na tuto vlastnost ViewModel. Interně `Button` volání `Execute` metoda vždy, když uživatel klepnutím `Button`, předejte do `Execute` metoda jeho `CommandParameter`.

`CanExecute` Metoda a `CanExecuteChanged` události se používají pro případy, kde `Button` klepněte na může být aktuálně neplatná, v takovém případě `Button` měli zakázat sám sebe. `Button` Volání `CanExecute` při `Command` nejprve vlastnost nastavena a vždy, když `CanExecuteChanged` událost je aktivována. Pokud `CanExecute` vrátí `false`, `Button` vypne a nebude vytvářet `Execute` volání.

Nápovědu k přidání tvorba příkazů na vašeho ViewModels, Xamarin.Forms definuje dvě třídy, které implementují `ICommand`: `Command` a `Command<T>` kde `T` je typ argumenty, které mají `Execute` a `CanExecute`. Tyto dvě třídy definují několik konstruktorů plus `ChangeCanExecute` metoda, která ViewModel můžete volat k vynucení `Command` objekt, který chcete aktivovat `CanExecuteChanged` událostí.

Zde je ViewModel pro jednoduché klávesnici, který je určený pro zadání telefonního čísla. Všimněte si, že `Execute` a `CanExecute` metoda jsou definovány jako lambda funkce přímo v konstruktoru:

```csharp
using System;
using System.ComponentModel;
using System.Windows.Input;
using Xamarin.Forms;

namespace XamlSamples
{
    class KeypadViewModel : INotifyPropertyChanged
    {
        string inputString = "";
        string displayText = "";
        char[] specialChars = { '*', '#' };

        public event PropertyChangedEventHandler PropertyChanged;

        // Constructor
        public KeypadViewModel()
        {
            AddCharCommand = new Command<string>((key) =>
                {
                    // Add the key to the input string.
                    InputString += key;
                });

            DeleteCharCommand = new Command(() =>
                {
                    // Strip a character from the input string.
                    InputString = InputString.Substring(0, InputString.Length - 1);
                },
                () =>
                {
                    // Return true if there's something to delete.
                    return InputString.Length > 0;
                });
        }

        // Public properties
        public string InputString
        {
            protected set
            {
                if (inputString != value)
                {
                    inputString = value;
                    OnPropertyChanged("InputString");
                    DisplayText = FormatText(inputString);

                    // Perhaps the delete button must be enabled/disabled.
                    ((Command)DeleteCharCommand).ChangeCanExecute();
                }
            }

            get { return inputString; }
        }

        public string DisplayText
        {
            protected set
            {
                if (displayText != value)
                {
                    displayText = value;
                    OnPropertyChanged("DisplayText");
                }
            }
            get { return displayText; }
        }

        // ICommand implementations
        public ICommand AddCharCommand { protected set; get; }

        public ICommand DeleteCharCommand { protected set; get; }

        string FormatText(string str)
        {
            bool hasNonNumbers = str.IndexOfAny(specialChars) != -1;
            string formatted = str;

            if (hasNonNumbers || str.Length < 4 || str.Length > 10)
            {
            }
            else if (str.Length < 8)
            {
                formatted = String.Format("{0}-{1}",
                                          str.Substring(0, 3),
                                          str.Substring(3));
            }
            else
            {
                formatted = String.Format("({0}) {1}-{2}",
                                          str.Substring(0, 3),
                                          str.Substring(3, 3),
                                          str.Substring(6));
            }
            return formatted;
        }

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Tato ViewModel předpokládá, že `AddCharCommand` vlastnost je vázána na `Command` vlastnost několik tlačítka (nebo cokoliv jiného, co má příkaz rozhraní), z nichž každý je identifikována `CommandParameter`. Tato tlačítka Přidat znaků, který má `InputString` vlastnosti, která řadí telefonní číslo pro `DisplayText` vlastnost.

Je také druhý vlastnost typu `ICommand` s názvem `DeleteCharCommand`. To je vázána na tlačítko Zpět mezery, ale tlačítko by mělo být zakázáno, pokud nejsou znaky odstranit.

Následující klávesnici není pokročilé jako vizuálně, jak by mohla být. Místo toho kód byla snížena na minimum k předvedení zřetelněji použití rozhraní příkazového:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.KeypadPage"
             Title="Keypad Page">

    <Grid HorizontalOptions="Center"
          VerticalOptions="Center">
        <Grid.BindingContext>
            <local:KeypadViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
        </Grid.ColumnDefinitions>

        <!-- Internal Grid for top row of items -->
        <Grid Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="Auto" />
            </Grid.ColumnDefinitions>

            <Frame Grid.Column="0"
                   OutlineColor="Accent">
                <Label Text="{Binding DisplayText}" />
            </Frame>

            <Button Text="&#x21E6;"
                    Command="{Binding DeleteCharCommand}"
                    Grid.Column="1"
                    BorderWidth="0" />
        </Grid>

        <Button Text="1"
                Command="{Binding AddCharCommand}"
                CommandParameter="1"
                Grid.Row="1" Grid.Column="0" />

        <Button Text="2"
                Command="{Binding AddCharCommand}"
                CommandParameter="2"
                Grid.Row="1" Grid.Column="1" />

        <Button Text="3"
                Command="{Binding AddCharCommand}"
                CommandParameter="3"
                Grid.Row="1" Grid.Column="2" />

        <Button Text="4"
                Command="{Binding AddCharCommand}"
                CommandParameter="4"
                Grid.Row="2" Grid.Column="0" />

        <Button Text="5"
                Command="{Binding AddCharCommand}"
                CommandParameter="5"
                Grid.Row="2" Grid.Column="1" />

        <Button Text="6"
                Command="{Binding AddCharCommand}"
                CommandParameter="6"
                Grid.Row="2" Grid.Column="2" />

        <Button Text="7"
                Command="{Binding AddCharCommand}"
                CommandParameter="7"
                Grid.Row="3" Grid.Column="0" />

        <Button Text="8"
                Command="{Binding AddCharCommand}"
                CommandParameter="8"
                Grid.Row="3" Grid.Column="1" />

        <Button Text="9"
                Command="{Binding AddCharCommand}"
                CommandParameter="9"
                Grid.Row="3" Grid.Column="2" />

        <Button Text="*"
                Command="{Binding AddCharCommand}"
                CommandParameter="*"
                Grid.Row="4" Grid.Column="0" />

        <Button Text="0"
                Command="{Binding AddCharCommand}"
                CommandParameter="0"
                Grid.Row="4" Grid.Column="1" />

        <Button Text="#"
                Command="{Binding AddCharCommand}"
                CommandParameter="#"
                Grid.Row="4" Grid.Column="2" />
    </Grid>
</ContentPage>
```

`Command` Vlastnost první `Button` , zobrazí se v tomto značek je vázán k `DeleteCharCommand`; zbytek je vázána na `AddCharCommand` s `CommandParameter` který je stejný jako znak, který se zobrazí na `Button` vzhled. Tady je program v akci:

[![](data-bindings-to-mvvm-images/keypad.png "Pomocí příkazů a rozhraní MVVM kalkulačky")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "kalkulačky pomocí příkazů a rozhraní MVVM")

### <a name="invoking-asynchronous-methods"></a>Volání asynchronních metod

Příkazy můžete také vyvolat asynchronních metod. Toho dosáhnete pomocí `async` a `await` klíčová slova při zadávání `Execute` metoda:

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

Znamená to, že `DownloadAsync` je metoda `Task` a by měl být očekáváno:

```csharp
async Task DownloadAsync ()
{
    await Task.Run (() => Download ());
}

void Download ()
{
    ...
}
```

## <a name="implementing-a-navigation-menu"></a>Implementace navigační nabídky

[XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/) program, který obsahuje zdrojový kód z této série článků používá ViewModel pro její domovské stránky. Tento ViewModel je definice krátké třídy s tři vlastnosti s názvem `Type`, `Title`, a `Description` obsahující typ jednotlivých ukázkové stránky, název a krátký popis. Kromě toho ViewModel definuje statickou vlastnost s názvem `All` tedy kolekce všechny stránky v aplikaci:

```csharp
public class PageDataViewModel
{
    public PageDataViewModel(Type type, string title, string description)
    {
        Type = type;
        Title = title;
        Description = description;
    }

    public Type Type { private set; get; }

    public string Title { private set; get; }

    public string Description { private set; get; }

    static PageDataViewModel()
    {
        All = new List<PageDataViewModel>
        {
            // Part 1. Getting Started with XAML
            new PageDataViewModel(typeof(HelloXamlPage), "Hello, XAML",
                                  "Display a Label with many properties set"),

            new PageDataViewModel(typeof(XamlPlusCodePage), "XAML + Code",
                                  "Interact with a Slider and Button"),

            // Part 2. Essential XAML Syntax
            new PageDataViewModel(typeof(GridDemoPage), "Grid Demo",
                                  "Explore XAML syntax with the Grid"),

            new PageDataViewModel(typeof(AbsoluteDemoPage), "Absolute Demo",
                                  "Explore XAML syntax with AbsoluteLayout"),

            // Part 3. XAML Markup Extensions
            new PageDataViewModel(typeof(SharedResourcesPage), "Shared Resources",
                                  "Using resource dictionaries to share resources"),

            new PageDataViewModel(typeof(StaticConstantsPage), "Static Constants",
                                  "Using the x:Static markup extensions"),

            new PageDataViewModel(typeof(RelativeLayoutPage), "Relative Layout",
                                  "Explore XAML markup extensions"),

            // Part 4. Data Binding Basics
            new PageDataViewModel(typeof(SliderBindingsPage), "Slider Bindings",
                                  "Bind properties of two views on the page"),

            new PageDataViewModel(typeof(SliderTransformsPage), "Slider Transforms",
                                  "Use Sliders with reverse bindings"),

            new PageDataViewModel(typeof(ListViewDemoPage), "ListView Demo",
                                  "Use a ListView with data bindings"),

            // Part 5. From Data Bindings to MVVM
            new PageDataViewModel(typeof(OneShotDateTimePage), "One-Shot DateTime",
                                  "Obtain the current DateTime and display it"),

            new PageDataViewModel(typeof(ClockPage), "Clock",
                                  "Dynamically display the current time"),

            new PageDataViewModel(typeof(HslColorScrollPage), "HSL Color Scroll",
                                  "Use a view model to select HSL colors"),

            new PageDataViewModel(typeof(KeypadPage), "Keypad",
                                  "Use a view model for numeric keypad logic")
        };
    }

    public static IList<PageDataViewModel> All { private set; get; } 
}
```

V souboru XAML `MainPage` definuje `ListBox` jejichž `ItemsSource` je nastavena na který `All` vlastnost a který obsahuje `TextCell` pro zobrazení `Title` a `Description` vlastnosti každé stránky:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage"
             Padding="5, 0"
             Title="XAML Samples">

    <ListView ItemsSource="{x:Static local:PageDataViewModel.All}"
              ItemSelected="OnListViewItemSelected">
        <ListView.ItemTemplate>
            <DataTemplate>
                <TextCell Text="{Binding Title}"
                          Detail="{Binding Description}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

Na stránkách jsou zobrazeny v posouvatelného seznamu:

[![](data-bindings-to-mvvm-images/mainpage.png "Posuvný seznam stránek")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "posuvný seznam stránek")

Obslužná rutina v souboru kódu se aktivuje, když uživatel vybere položku. Nastaví obslužné rutiny `SelectedItem` vlastnost `ListBox` zpět na `null` a vytvoří zvolené stránky a přejde do ní:

```csharp
private async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
{
    (sender as ListView).SelectedItem = null;

    if (args.SelectedItem != null)
    {
        PageDataViewModel pageData = args.SelectedItem as PageDataViewModel;
        Page page = (Page)Activator.CreateInstance(pageData.Type);
        await Navigation.PushAsync(page);
    }
}
```

## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/DYRLcqG2BAY]

**Xamarin momentální 2016: Rozhraní MVVM zjednodušená s Xamarin.Forms a modulu Prism**

## <a name="summary"></a>Souhrn

XAML je výkonný nástroj pro definování uživatelského rozhraní v Xamarin.Forms aplikace, zejména při datové vazby a rozhraní MVVM používají. Výsledkem je znázornění čistá, elegantní a potenciálně jazyk uživatelského rozhraní a podporu pozadí v kódu.


## <a name="related-links"></a>Související odkazy

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Část 1. Začínáme s jazykem XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Část 2. Základní syntaxe jazyka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Část 3. Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Část 4. Základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
