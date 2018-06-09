---
title: Rozhraní příkazového Xamarin.Forms
description: Tento článek vysvětluje, jak implementovat vlastnost příkazu s Xamarin.Forms datové vazby. Rozhraní řídicího poskytuje alternativní způsob implementace příkazy, je mnohem lepší vhodný k architektuře rozhraní MVVM.
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 37fe5bbcfa3dbc6aa5483c89b49c1698a00ecbb6
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241309"
---
# <a name="the-xamarinforms-command-interface"></a>Rozhraní příkazového Xamarin.Forms

V architektuře Model-View-ViewModel (modelem MVVM) jsou definované vazby dat mezi vlastnosti v ViewModel, který je obvykle třídu odvozenou z `INotifyPropertyChanged`a vlastnosti v zobrazení, která je obecně souboru XAML. Někdy potřeb, které jdou nad rámec těchto vazeb vlastnost tím, že uživatel zahájíte příkazy, které ovlivňují něco v ViewModel má aplikace. Tyto příkazy jsou obecně signalizovala pomocí kliknutí na tlačítko nebo prostem odposlouchávání a tradičně jsou zpracovány v souboru kódu na pozadí v obslužnou rutinu pro `Clicked` události `Button` nebo `Tapped` události `TapGestureRecognizer`.

Rozhraní řídicího poskytuje alternativní způsob implementace příkazy, je mnohem lepší vhodný k architektuře rozhraní MVVM. ViewModel samotné mohou obsahovat příkazy, které metody, které jsou spouštěny v reakci na konkrétní aktivity v zobrazení, jako jsou `Button` klikněte na tlačítko. Datové vazby jsou definované mezi tyto příkazy a `Button`.

Umožňuje vytvoření vazby dat mezi `Button` a ViewModel, `Button` definuje dvě vlastnosti:

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) typu <xref:System.Windows.Input.ICommand>
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) typu `Object`

Pokud chcete používat rozhraní příkazového, definujete vazbu dat, která je cílena `Command` vlastnost `Button` Pokud je zdrojem na vlastnost ve ViewModel typu `ICommand`. ViewModel obsahuje kód, který přidružené `ICommand` vlastnost, která se spustí, až po kliknutí na tlačítko. Můžete nastavit `CommandParameter` na libovolná data k rozlišení mezi více tlačítek, pokud jsou všechny vázán ke stejné `ICommand` vlastnost ViewModel.

`Command` a `CommandParameter` vlastnosti jsou definovány také následující třídy:

- [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) a proto [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/), která je odvozena z `MenuItem`
- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) a proto [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/), která je odvozena z `TextCell`
- [`TapGestureRecognizer`](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)

[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) definuje [ `SearchCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) vlastnost typu `ICommand` a [ `SearchCommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) vlastnost. [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) Vlastnost [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) je také typu `ICommand`.

Všechny tyto příkazy lze zpracovat v rámci ViewModel způsobem, který nezávisí na objekt konkrétní uživatelského rozhraní v zobrazení.

## <a name="the-icommand-interface"></a>Rozhraní ICommand

<xref:System.Windows.Input.ICommand> Rozhraní není součástí Xamarin.Forms. Je definována místo v [System.Windows.Input](xref:System.Windows.Input) obor názvů a se skládá ze dvou metod a jedna událost:

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

Pokud chcete používat rozhraní příkazového, vaše ViewModel obsahuje vlastnosti typu `ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

ViewModel také musí odkazovat třídu, která implementuje `ICommand` rozhraní. Tato třída bude za chvíli popsané. V zobrazení `Command` vlastnost `Button` je vázána na tuto vlastnost:

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

Uživatel stiskne `Button`, `Button` volání `Execute` metoda v `ICommand` objekt vázána na jeho `Command` vlastnost. Který je nejjednodušší součástí řídicího rozhraní.

`CanExecute` Metoda je složitější. Když vazby nejprve musí být definován `Command` vlastnost `Button`, a kdy se změní datové vazby nějakým způsobem `Button` volání `CanExecute` metoda v `ICommand` objektu. Pokud `CanExecute` vrátí `false`, pak se `Button` vypne. To znamená, že konkrétní příkaz je nyní k dispozici nebo je neplatný.

`Button` Také připojí na obslužnou rutinu `CanExecuteChanged` události `ICommand`. Událost je aktivována z v rámci ViewModel. Když tuto událost je aktivována, `Button` volání `CanExecute` znovu. `Button` Umožňuje sám sebe, pokud `CanExecute` vrátí `true` a vypne, pokud `CanExecute` vrátí `false`.

> [!IMPORTANT]
> Nepoužívejte `IsEnabled` vlastnost `Button` Pokud používáte rozhraní příkazu.  

## <a name="the-command-class"></a>Příkaz – třída

Pokud vaše ViewModel definuje prvku typu `ICommand`, ViewModel musí také obsahovat nebo odkazovat na třídu, která implementuje `ICommand` rozhraní. Tato třída musí obsahovat nebo odkaz `Execute` a `CanExecute` metody a ještě efektivněji `CanExecuteChanged` událost vždy, když `CanExecute` metoda může vrátit na jinou hodnotu.

Můžete napsat takové třídu sami nebo můžete použít třídu, která má někdo jiný zapsána. Protože `ICommand` je součástí systému Windows, že se používá let s modelem MVVM Windows aplikace. Použití třídy Windows, který implementuje `ICommand` můžete sdílet vaše ViewModels mezi aplikací systému Windows a Xamarin.Forms aplikací.

Pokud sdílení ViewModels mezi Windows a Xamarin.Forms nehrají důležitou roli, pak můžete použít [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) nebo [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) třídy součástí Xamarin.Forms implementovat `ICommand`rozhraní. Tyto třídy umožňují určit orgánů `Execute` a `CanExecute` metody v konstruktorech třídy. Použít `Command<T>` při použití `CommandParameter` vlastnost k rozlišení mezi více zobrazení vázána na stejné `ICommand` vlastnost a jednodušší `Command` třídy, při které není požadavkem.

## <a name="basic-commanding"></a>Tvorba základní příkazů

**Osoba položka** stránku [ **ukázky vazby dat** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) program ukazuje některé jednoduché příkazy implementované v ViewModel.

`PersonViewModel` Definuje tři vlastnosti s názvem `Name`, `Age`, a `Skills` které definují osoby. Tato třída neodpovídá *není* obsahovat žádné `ICommand` vlastnosti:

```csharp
public class PersonViewModel : INotifyPropertyChanged
{
    string name;
    double age;
    string skills;

    public event PropertyChangedEventHandler PropertyChanged;

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public double Age
    {
        set { SetProperty(ref age, value); }
        get { return age; }
    }

    public string Skills
    {
        set { SetProperty(ref skills, value); }
        get { return skills; }
    }

    public override string ToString()
    {
        return Name + ", age " + Age;
    }

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

`PersonCollectionViewModel` Vidět níže vytváří nové objekty typu `PersonViewModel` a umožňuje uživateli vyplnit data. K tomuto účelu třída definuje vlastnosti `IsEditing` typu `bool` a `PersonEdit` typu `PersonViewModel`. Kromě toho třída definuje tři vlastnosti typu `ICommand` a vlastnost s názvem `Persons` typu `IList<PersonViewModel>`:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{
    PersonViewModel personEdit;
    bool isEditing;

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public bool IsEditing
    {
        private set { SetProperty(ref isEditing, value); }
        get { return isEditing; }
    }

    public PersonViewModel PersonEdit
    {
        set { SetProperty(ref personEdit, value); }
        get { return personEdit; }
    }

    public ICommand NewCommand { private set; get; }

    public ICommand SubmitCommand { private set; get; }

    public ICommand CancelCommand { private set; get; }

    public IList<PersonViewModel> Persons { get; } = new ObservableCollection<PersonViewModel>();

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Zkrácený výpis nezahrnuje konstruktoru třídy, která je tam, kde tři vlastnosti typu `ICommand` jsou definovány, které se krátce zobrazí. Všimněte si, že se změní na tři vlastnosti typu `ICommand` a `Persons` vlastnost nezpůsobovalo neustálé `PropertyChanged` události je aktivováno. Tyto vlastnosti jsou nastavené při prvním vytvoření třídy a neměňte po tomto datu.

Před zkoumání konstruktoru `PersonCollectionViewModel` třídy, podíváme se na v souboru XAML **osoba položka** program. Tato položka obsahuje `Grid` s jeho `BindingContext` vlastnost nastavena na hodnotu `PersonCollectionViewModel`. `Grid` Obsahuje `Button` s textem **nový** s jeho `Command` vlastnost vázána na `NewCommand` vlastnost v ViewModel, formuláře položky s vlastnostmi vázána na `IsEditing` vlastnosti, jako dobře jako vlastnosti `PersonViewModel`, a další dvě tlačítka vázaný k `SubmitCommand` a `CancelCommand` vlastnosti ViewModel. Konečné `ListView` zobrazí kolekci již byl zadán osob:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.PersonEntryPage"
             Title="Person Entry">
    <Grid Margin="10">
        <Grid.BindingContext>
            <local:PersonCollectionViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <!-- New Button -->
        <Button Text="New"
                Grid.Row="0"
                Command="{Binding NewCommand}"
                HorizontalOptions="Start" />

        <!-- Entry Form -->
        <Grid Grid.Row="1"
              IsEnabled="{Binding IsEditing}">

            <Grid BindingContext="{Binding PersonEdit}">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Label Text="Name: " Grid.Row="0" Grid.Column="0" />
                <Entry Text="{Binding Name}"
                       Grid.Row="0" Grid.Column="1" />

                <Label Text="Age: " Grid.Row="1" Grid.Column="0" />
                <StackLayout Orientation="Horizontal"
                             Grid.Row="1" Grid.Column="1">
                    <Stepper Value="{Binding Age}"
                             Maximum="100" />
                    <Label Text="{Binding Age, StringFormat='{0} years old'}"
                           VerticalOptions="Center" />
                </StackLayout>

                <Label Text="Skills: " Grid.Row="2" Grid.Column="0" />
                <Entry Text="{Binding Skills}"
                       Grid.Row="2" Grid.Column="1" />

            </Grid>
        </Grid>

        <!-- Submit and Cancel Buttons -->
        <Grid Grid.Row="2">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>

            <Button Text="Submit"
                    Grid.Column="0"
                    Command="{Binding SubmitCommand}"
                    VerticalOptions="CenterAndExpand" />

            <Button Text="Cancel"
                    Grid.Column="1"
                    Command="{Binding CancelCommand}"
                    VerticalOptions="CenterAndExpand" />
        </Grid>

        <!-- List of Persons -->
        <ListView Grid.Row="3"
                  ItemsSource="{Binding Persons}" />
    </Grid>
</ContentPage>
```

Zde je, jak to funguje: první stisknutí uživatele **nový** tlačítko. To umožňuje formuláře, ale zakáže **nový** tlačítko. Uživatel potom zadá název, stáří a znalosti. V průběhu úprav, může uživatel stisknout **zrušit** tlačítko a začít od začátku. Pokud byl zadán název a platné stáří je pouze **odeslání** tlačítko povoleno. Stisknutím to **odeslání** tlačítko převádí osoba, která do kolekce zobrazí `ListView`. Po buď **zrušit** nebo **odeslání** stisknutí tlačítka, formuláře se vymaže a **nový** tlačítko opět povolena.

Na obrazovce iOS na levé straně se zobrazí rozložení předtím, než je zadaná platná stáří. Android a UWP obrazovky zobrazit **odeslání** tlačítko povoleno po stáří:

[![Položka osoba](commanding-images/personentry-small.png "osoba položka")](commanding-images/personentry-large.png#lightbox "osoba položka")

Program nemá žádné zařízení pro úpravy existujících položek a neukládá položky, když přejdete mimo stránku.

Veškerou logiku pro **nový**, **odeslání**, a **zrušit** tlačítka zpracovává při `PersonCollectionViewModel` prostřednictvím definice `NewCommand`, `SubmitCommand`, a `CancelCommand` vlastnosti. Konstruktoru `PersonCollectionViewModel` nastaví tyto tři vlastnosti pro objekty typu `Command`.  

A [konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) z `Command` třída umožňuje předání argumentů typu `Action` a `Func<bool>` odpovídající `Execute` a `CanExecute` metody. Je to nejjednodušší provádět můžete určit tyto akce a funkce jako lambda funkce přímo `Command` konstruktor. Zde je definice `Command` objekt pro `NewCommand` vlastnost:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {
        NewCommand = new Command(
            execute: () =>
            {
                PersonEdit = new PersonViewModel();
                PersonEdit.PropertyChanged += OnPersonEditPropertyChanged;
                IsEditing = true;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return !IsEditing;
            });

        ···

    }

    void OnPersonEditPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        (SubmitCommand as Command).ChangeCanExecute();
    }

    void RefreshCanExecutes()
    {
        (NewCommand as Command).ChangeCanExecute();
        (SubmitCommand as Command).ChangeCanExecute();
        (CancelCommand as Command).ChangeCanExecute();
    }

    ···

}
```

Když uživatel klikne **nový** tlačítko `execute` předaný funkci `Command` konstruktor se spustí. Tím se vytvoří nový `PersonViewModel` objektu, nastaví obslužnou rutinu pro tento objekt `PropertyChanged` událostí, nastaví `IsEditing` k `true`a volá `RefreshCanExecutes` metoda definované po konstruktoru.

Kromě implementace `ICommand` rozhraní, `Command` třída také definuje metodu s názvem `ChangeCanExecute`. Vaše ViewModel by měly volat `ChangeCanExecute` pro `ICommand` vlastnost vždy, když se něco stane, které mohou změnit vrácenou hodnotu `CanExecute` metoda. Volání `ChangeCanExecute` způsobí, že `Command` třída má provést, `CanExecuteChanged` metoda. `Button` Se připojilo obslužnou rutinu pro tuto událost a reaguje voláním `CanExecute` znovu a pak povolení založena na návratovou hodnotu této metody.

Když `execute` metodu `NewCommand` volání `RefreshCanExecutes`, `NewCommand` vlastnost získá volání `ChangeCanExecute`a `Button` volání `canExecute` metoda, která teď vrátí `false` protože `IsEditing`je vlastnost `true`.

`PropertyChanged` Obslužné rutiny pro nové `PersonViewModel` objektu volání `ChangeCanExecute` metodu `SubmitCommand`. Tady je způsob implementace tohoto příkazu:


```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        SubmitCommand = new Command(
            execute: () =>
            {
                Persons.Add(PersonEdit);
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return PersonEdit != null &&
                       PersonEdit.Name != null &&
                       PersonEdit.Name.Length > 1 &&
                       PersonEdit.Age > 0;
            });

        ···
    }

    ···

}
```

`canExecute` Funkce pro `SubmitCommand` nazývá pokaždé, když je vlastnost změnit v `PersonViewModel` upravovaný objekt. Vrátí `true` pouze tehdy, když `Name` vlastnost je alespoň jeden znak, a `Age` je větší než 0. V ten moment se **odeslání** aktivuje tlačítko.

`execute` Funkce pro **odeslání** odebere obslužná rutina vlastnost změnit z `PersonViewModel`, přidá objekt, který má `Persons` kolekce a vrátí všechno počáteční podmínky.

`execute` Funkce pro **zrušit** tlačítko nemá vše, který **odeslání** execept nemá tlačítko Přidat objekt do kolekce:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        CancelCommand = new Command(
            execute: () =>
            {
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return IsEditing;
            });
    }

    ···

}
```

`canExecute` Metoda vrátí `true` kdykoli `PersonViewModel` upravována.

Tyto postupy by mohla být přizpůsobena složitější scénáře: vlastnost v `PersonCollectionViewModel` může být vázána na `SelectedItem` vlastnost `ListView` pro úpravy existujících položek a **odstranit** tlačítko nebylo možné přidat do odstranit Tyto položky.

Není nutné definovat `execute` a `canExecute` metody jako funkce lambda. Můžete napsat je běžnými privátní metody v ViewModel a odkazujte na ně v `Command` konstruktory. Tento postup však zpravidla vést k velkému metod, které odkazuje ViewModel pouze jednou.

## <a name="using-command-parameters"></a>Pomocí parametrů příkazu

V některých případech je vhodné pro jeden nebo více tlačítek (nebo jiné objekty uživatelského rozhraní) sdílet stejný `ICommand` vlastnost ViewModel. V takovém případě použijete `CommandParameter` vlastnost k rozlišení mezi tlačítka.

Můžete dál používat `Command` třídu pro tyto sdílené `ICommand` vlastnosti. Definuje třídu [alternativní konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action%7BSystem.Object%7D/System.Func%7BSystem.Object,System.Boolean%7D/) který přijímá `execute` a `canExecute` metody s parametry typu `Object`. Jedná se jak `CommandParameter` předaný těchto metod.

Ale při použití `CommandParameter`, jednoduše používat obecná [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) třídu k určení typu objektu nastavena na `CommandParameter`. `execute` a `canExecute` metody, které zadáte mít parametry daného typu.

**Decimal klávesnice** stránky tento postup ukazuje ukazuje, jak implementovat klávesnice pro zadání desetinná čísla. `BindingContext` Pro `Grid` je `DecimalKeypadViewModel`. `Entry` Vlastnost pro tento ViewModel je vázána na `Text` vlastnost `Label`. Všechny `Button` objekty, které jsou vázány na různé příkazy v ViewModel: `ClearCommand`, `BackspaceCommand`, a `DigitCommand`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.DecimalKeypadPage"
             Title="Decimal Keyboard">

    <Grid WidthRequest="240"
          HeightRequest="480"
          ColumnSpacing="2"
          RowSpacing="2"
          HorizontalOptions="Center"
          VerticalOptions="Center">

        <Grid.BindingContext>
            <local:DecimalKeypadViewModel />
        </Grid.BindingContext>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Button">
                    <Setter Property="FontSize" Value="32" />
                    <Setter Property="BorderWidth" Value="1" />
                    <Setter Property="BorderColor" Value="Black" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Label Text="{Binding Entry}"
               Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3"
               FontSize="32"
               LineBreakMode="HeadTruncation"
               VerticalTextAlignment="Center"
               HorizontalTextAlignment="End" />

        <Button Text="CLEAR"
                Grid.Row="1" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding ClearCommand}" />

        <Button Text="&#x21E6;"
                Grid.Row="1" Grid.Column="2"
                Command="{Binding BackspaceCommand}" />

        <Button Text="7"
                Grid.Row="2" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="7" />

        <Button Text="8"
                Grid.Row="2" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="8" />

        <Button Text="9"
                Grid.Row="2" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="9" />

        <Button Text="4"
                Grid.Row="3" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="4" />

        <Button Text="5"
                Grid.Row="3" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="5" />

        <Button Text="6"
                Grid.Row="3" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="6" />

        <Button Text="1"
                Grid.Row="4" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="1" />

        <Button Text="2"
                Grid.Row="4" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="2" />

        <Button Text="3"
                Grid.Row="4" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="3" />

        <Button Text="0"
                Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding DigitCommand}"
                CommandParameter="0" />

        <Button Text="&#x00B7;"
                Grid.Row="5" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="." />
    </Grid>
</ContentPage>
```

Tlačítka 11 pro 10 číslic a desetinnou sdílet vazbu ke `DigitCommand`. `CommandParameter` Rozlišuje mezi tato tlačítka. Nastavte na hodnotu `CommandParameter` jsou obvykle stejné jako text ve tlačítko s výjimkou desetinné čárky, která pro účely přehlednosti se zobrazí s tečkou uprostřed znak zobrazí.

Tady je program v akci:

[![Decimal klávesnice](commanding-images/decimalkeyboard-small.png "Decimal klávesnice")](commanding-images/decimalkeyboard-large.png#lightbox "Decimal klávesnice")

Všimněte si, že na tlačítko desetinné čárky všechny tři snímcích obrazovky zakázaná, protože zadané číslo již obsahuje desetinné čárky.

`DecimalKeypadViewModel` Definuje `Entry` vlastnost typu `string` (což je jediná vlastnost, kterou se aktivuje `PropertyChanged` událostí) a tři vlastnosti typu `ICommand`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{
    string entry = "0";

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public string Entry
    {
        private set
        {
            if (entry != value)
            {
                entry = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Entry"));
            }
        }
        get
        {
            return entry;
        }
    }

    public ICommand ClearCommand { private set; get; }

    public ICommand BackspaceCommand { private set; get; }

    public ICommand DigitCommand { private set; get; }
}
```

Tlačítko odpovídající k `ClearCommand` je vždy povolena a jednoduše nastaví položku zpět na hodnotu "0":

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {
        ClearCommand = new Command(
            execute: () =>
            {
                Entry = "0";
                RefreshCanExecutes();
            });

        ···

    }

    void RefreshCanExecutes()
    {
        ((Command)BackspaceCommand).ChangeCanExecute();
        ((Command)DigitCommand).ChangeCanExecute();
    }

    ···

}
```

Protože tlačítko je vždy povolena, není nutné zadávat `canExecute` argument `Command` konstruktor.

Logika pro zadání čísla a mazání pomocí klávesy BACKSPACE je trochu složité, protože pokud byly zadány žádné číslic, pak se `Entry` vlastnost je řetězec "0". Pokud uživatel zadá další nuly, potom `Entry` stále obsahuje pouze jeden nula. Pokud uživatel zadá další číslice, že číslice nahrazuje na nule. Ale pokud uživatel zadá desetinné čárky před další číslice, spojovníky pak `Entry` je řetězec "0.".

**Backspace** tlačítko je dostupné, jenom když délka položka je větší než 1 nebo `Entry` není roven řetězec "0":

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        BackspaceCommand = new Command(
            execute: () =>
            {
                Entry = Entry.Substring(0, Entry.Length - 1);
                if (Entry == "")
                {
                    Entry = "0";
                }
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return Entry.Length > 1 || Entry != "0";
            });

        ···

    }

    ···

}
```

Logiku pro `execute` funkce pro **Backspace** tlačítko zajišťuje, že `Entry` je alespoň řetězec "0".

`DigitCommand` Vlastnost je vázána na 11 tlačítka, z nichž každý identifikuje s `CommandParameter` vlastnost. `DigitCommand` Může být nastaven na instanci běžné `Command` třídy, ale je jednodušší použít `Command<T>` obecná třída. Při používání řídicího rozhraní s XAML, `CommandParameter` vlastnosti jsou obvykle řetězce a který je typu Obecné argumentu. `execute` a `canExecute` pak obsahují argumenty typu `string`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        DigitCommand = new Command<string>(
            execute: (string arg) =>
            {
                Entry += arg;
                if (Entry.StartsWith("0") && !Entry.StartsWith("0."))
                {
                    Entry = Entry.Substring(1);
                }
                RefreshCanExecutes();
            },
            canExecute: (string arg) =>
            {
                return !(arg == "." && Entry.Contains("."));
            });
    }

    ···

}
```

`execute` Metoda připojí argument řetězec `Entry` vlastnost. Ale pokud výsledek začíná nulu (ale ne nulu a desetinné čárky) pak této počáteční nula musíte odstranit pomocí `Substring` funkce.

`canExecute` Metoda vrátí `false` pouze v případě, že je argumentem desetinné čárky (značí, že stisknutí desetinné čárky) a `Entry` již obsahuje desetinné čárky.

Všechny `execute` volání metody `RefreshCanExecutes`, který potom volá `ChangeCanExecute` pro obě `DigitCommand` a `ClearCommand`. To zajišťuje, že desetinné čárky a backspace tlačítka jsou povolené nebo zakázané podle aktuální pořadí zadaných číslic.

## <a name="adding-commands-to-existing-views"></a>Přidání příkazů na existující zobrazení

Pokud chcete použít rozhraní řídicího se zobrazeními, které ji nepodporují, je možné použít chování Xamarin.Forms, která převede událost příkaz. To je popsána v článku [ **opakovaně použitelného EventToCommandBehavior**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md).

## <a name="asynchronous-commanding-for-navigation-menus"></a>Asynchronní tvorba příkazů pro navigační nabídky

Tvorba příkazů je vhodné pro implementace navigační nabídky, jako je například, že [ **ukázky vazby dat** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) programu sám sebe. Zde je součástí **MainPage.xaml**:


```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MainPage"
             Title="Data Binding Demos"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection Title="Basic Bindings">

                <TextCell Text="Basic Code Binding"
                          Detail="Define a data-binding in code"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicCodeBindingPage}" />

                <TextCell Text="Basic XAML Binding"
                          Detail="Define a data-binding in XAML"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicXamlBindingPage}" />

                <TextCell Text="Alternative Code Binding"
                          Detail="Define a data-binding in code without a BindingContext"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:AlternativeCodeBindingPage}" />

                ···

            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

Při použití tvorba příkazů s XAML, `CommandParameter` vlastnosti jsou obvykle nastavené na řetězce. V takovém případě však použít příponu značek XAML tak, aby `CommandParameter` je typu `System.Type`.

Každý `Command` vlastnost je vázána na vlastnost s názvem `NavigateCommand`. Aby vlastnost je definována v souboru kódu na pozadí, **MainPage.xaml.cs**:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(
            async (Type pageType) =>
            {
                Page page = (Page)Activator.CreateInstance(pageType);
                await Navigation.PushAsync(page);
            });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Nastaví konstruktor `NavigateCommand` vlastnost, která má `execute` metoda, která vytvoří instanci `System.Type` parametr a poté přejde k němu. Protože `PushAsync` vyžaduje volání `await` operátor, `execute` metoda musí být označené jako o asynchronním. To je provedeno pomocí `async` – klíčové slovo před seznam parametrů.

Konstruktor také nastaví `BindingContext` stránky na sebe sama tak, aby vazby odkazovat `NavigateCommand` v této třídě.

Pořadí kód v tomto konstruktoru. Díky rozdíl: `InitializeComponent` volání způsobí, že XAML ho proto analyzovat, ale v tuto chvíli vytvoření vazby na vlastnost s názvem `NavigateCommand` nelze vyřešit, protože `BindingContext` je nastaven na `null`. Pokud `BindingContext` je nastavena v konstruktoru *před* `NavigateCommand` nastavena, pak vazby lze vyřešit při `BindingContext` nastavená, ale současně se `NavigateCommand` stále `null`. Nastavení `NavigateCommand` po `BindingContext` nebude mít žádný vliv na vazby, protože ke změně `NavigateCommand` není fire `PropertyChanged` událostí a vazbu nebude vědět, že `NavigateCommand` je nyní platný.

Obě nastavení `NavigateCommand` a `BindingContext` (v libovolném pořadí) před volání `InitializeComponent` bude fungovat, protože i komponent vazby jsou nastaveny, pokud analyzátor XAML nalezne definici vazby.

Datové vazby v některých případech může být složité, ale jako jste viděli v této série článků, jsou výkonný a flexibilní a pomůže výrazně organizace kódu oddělením základní logiku z uživatelského rozhraní.



## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Kapitola vazby dat z adresáře Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
