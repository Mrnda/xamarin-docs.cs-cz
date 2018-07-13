---
title: Rozhraní příkazového řádku Xamarin.Forms
description: Tento článek vysvětluje, jak implementovat vlastnost příkazu s Xamarin.Forms datové vazby. Řídicího rozhraní poskytuje alternativní způsob implementace příkazů, který je mnohem lépe hodí pro architektury MVVM.
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: b18d042e34146a72b488da9017648a430c9cd353
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996370"
---
# <a name="the-xamarinforms-command-interface"></a>Rozhraní příkazového řádku Xamarin.Forms

V architektuře Model-View-ViewModel (MVVM) jsou definovány datové vazby mezi vlastnostmi v ViewModel, který je obvykle třída, která je odvozena z `INotifyPropertyChanged`a vlastností v zobrazení, která je obvykle soubor XAML. Někdy aplikace má nároky, které přesahují tyto vazby vlastnosti tak, že uživatel zahájil příkazy, které ovlivňují něco v ViewModel vyžaduje. Tyto příkazy jsou obecně signalizován pomocí kliknutí na tlačítko nebo prsty odposlouchávání a obvykle jsou zpracovány v souboru kódu na pozadí v obslužné rutiny pro `Clicked` událost `Button` nebo `Tapped` události `TapGestureRecognizer`.

Řídicího rozhraní poskytuje alternativní způsob implementace příkazů, který je mnohem lépe hodí pro architektury MVVM. ViewModel samotný mohou obsahovat příkazy, které jsou metody, které jsou provedeny v reakci na konkrétní aktivitu v zobrazení jako `Button` klikněte na tlačítko. Mezi tyto příkazy jsou definovány datové vazby a `Button`.

Chcete-li povolit vazby dat mezi `Button` a ViewModel, `Button` definuje dvě vlastnosti:

- [`Command`](xref:Xamarin.Forms.Button.Command) typu <xref:System.Windows.Input.ICommand>
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) typu `Object`

Pokud chcete používat rozhraní příkazového řádku, definujete datové vazby, který se zaměřuje `Command` vlastnost `Button` Pokud je zdrojem vlastnost ViewModel typ `ICommand`. Obsahuje kód spojený s, která ViewModel `ICommand` vlastnost, která se spouští při kliknutí na tlačítko. Můžete nastavit `CommandParameter` pro libovolná data rozlišovat mezi více tlačítek, pokud jsou všechny vázána na stejný `ICommand` vlastnost ViewModel.

`Command` a `CommandParameter` vlastnosti jsou také definovány následující třídy:

- [`MenuItem`](xref:Xamarin.Forms.MenuItem) a proto [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem), která je odvozena z `MenuItem`
- [`TextCell`](xref:Xamarin.Forms.TextCell) a proto [ `ImageCell` ](xref:Xamarin.Forms.ImageCell), která je odvozena z `TextCell`
- [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)

[`SearchBar`](xref:Xamarin.Forms.SearchBar) definuje [ `SearchCommand` ](xref:Xamarin.Forms.SearchBar.SearchCommand) vlastnost typu `ICommand` a [ `SearchCommandParameter` ](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) vlastnost. [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand) Vlastnost [ `ListView` ](xref:Xamarin.Forms.ListView) je také typu `ICommand`.

Tyto příkazy může být zpracována v rámci ViewModel v podobě, která nezávisí na objekt konkrétního uživatelského rozhraní v zobrazení.

## <a name="the-icommand-interface"></a>Rozhraní ICommand, které

<xref:System.Windows.Input.ICommand> Rozhraní není součástí Xamarin.Forms. Místo toho definovaný v [System.Windows.Input](xref:System.Windows.Input) obor názvů se skládá ze dvou metod a jednu událost:

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

Pokud chcete používat rozhraní příkazového řádku, vaše ViewModel obsahuje vlastnosti typu `ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

ViewModel musí také odkazovat na třídu, která implementuje `ICommand` rozhraní. Tato třída bude za chvíli popsané. V zobrazení `Command` vlastnost `Button` je vázán na tuto vlastnost:

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

Když uživatel stiskne klávesu `Button`, `Button` volání `Execute` metoda ve `ICommand` objekt vázán na jeho `Command` vlastnost. To je nejjednodušší část řídicího rozhraní.

`CanExecute` Metoda je složitější. Pokud nejprve definován vazbu na `Command` vlastnost `Button`, a při změně datové vazby v některých případech `Button` volání `CanExecute` metoda ve `ICommand` objektu. Pokud `CanExecute` vrátí `false`, pak bude `Button` sama deaktivuje. To znamená, že konkrétní příkaz je momentálně není k dispozici nebo je neplatný.

`Button` Také připojí obslužnou rutinu na `CanExecuteChanged` událost `ICommand`. Událost je aktivována z v rámci ViewModel. Když se aktivuje tuto událost, `Button` volání `CanExecute` znovu. `Button` Samotný umožňuje v případě `CanExecute` vrátí `true` a vypne, pokud `CanExecute` vrátí `false`.

> [!IMPORTANT]
> Nepoužívejte `IsEnabled` vlastnost `Button` Pokud používáte rozhraní příkazového řádku.  

## <a name="the-command-class"></a>Třídy příkazů

Pokud vaše ViewModel definuje prvku typu `ICommand`, ViewModel musí také obsahovat nebo odkazovat na třídu, která implementuje `ICommand` rozhraní. Tato třída musí obsahovat nebo odkazovat `Execute` a `CanExecute` metody a fire `CanExecuteChanged` událost pokaždé, když `CanExecute` metoda může vrátit jiné hodnoty.

Můžete napsat takové třídy sami nebo můžete použít třídu, která zapsala někdo jiný. Protože `ICommand` je součástí systému Microsoft Windows, bylo použito pro roky s aplikacemi Windows MVVM. Pomocí třídy Windows, která implementuje `ICommand` vám umožní sdílet vaše modely ViewModels mezi Windows a aplikace Xamarin.Forms.

Pokud modely ViewModels mezi Windows a Xamarin.Forms pro sdílení obsahu není žádný problém, pak můžete použít [ `Command` ](xref:Xamarin.Forms.Command) nebo [ `Command<T>` ](xref:Xamarin.Forms.Command`1) třídy zahrnuty v Xamarin.Forms pro implementaci `ICommand`rozhraní. Tyto třídy umožňují určit orgánů `Execute` a `CanExecute` metody v konstruktorech tříd. Použít `Command<T>` při použití `CommandParameter` vlastnost k rozlišení mezi více pohledy vázána na stejný `ICommand` vlastnosti a jednodušší `Command` třídy při, není to povinné.

## <a name="basic-commanding"></a>Základní příkazů

**Položky osoba** stránku [ **ukázky vazby dat** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) program ukazuje některé jednoduché příkazy implementované v ViewModel.

`PersonViewModel` Definuje tři vlastnosti s názvem `Name`, `Age`, a `Skills` , definovat osobu. Tato třída nemá *není* obsahovat žádný `ICommand` vlastnosti:

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

`PersonCollectionViewModel` Uvedené níže vytvoří nové objekty typu `PersonViewModel` a umožňuje uživatelům vyplnit data. Pro tento účel třídy definuje vlastnosti `IsEditing` typu `bool` a `PersonEdit` typu `PersonViewModel`. Kromě toho třída definuje tři vlastnosti typu `ICommand` a vlastnost s názvem `Persons` typu `IList<PersonViewModel>`:

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

Zkrácený výpis neobsahuje konstruktor třídy, která je tam, kde tři vlastnosti typu `ICommand` jsou definovány, který se zobrazí za chvíli. Všimněte si, že se změní na tři vlastnosti typu `ICommand` a `Persons` následek vlastnost `PropertyChanged` události se aktivoval. Tyto vlastnosti jsou nastavené při prvním vytvoření třídy a nemění po tomto datu.

Před zkoumání konstruktoru `PersonCollectionViewModel` třídy, Pojďme se podívat v souboru XAML **položky osoba** programu. Tady se nachází `Grid` s jeho `BindingContext` vlastnost nastavena na hodnotu `PersonCollectionViewModel`. `Grid` Obsahuje `Button` s textem **nový** s jeho `Command` vlastnost vázána na `NewCommand` vlastnost v ViewModel, formulář Položka s vlastnostmi vázána na `IsEditing` vlastnosti, jako také jako vlastnosti `PersonViewModel`, a dvě tlačítka Další vázaná na `SubmitCommand` a `CancelCommand` vlastnosti ViewModel. Finální `ListView` zobrazuje kolekci osob už zadali:

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

Zde je, jak to funguje: uživatel stiskne první **nový** tlačítko. To umožňuje položka formuláře, ale zakáže **nový** tlačítko. Uživatel zadá název, věk a dovednosti. Kdykoli během úpravy může uživatel stisknout **zrušit** tlačítko a začít od začátku. Pokud není zadaný název a platné stáří je jenom **odeslat** tlačítko povoleno. Klávesy to **odeslat** tlačítko převádí osoby do kolekce, zobrazí `ListView`. Po zavolání **zrušit** nebo **odeslat** stisknutí tlačítka, se vymaže formuláři pro zadávání a **nový** tlačítko opět povolena.

Na obrazovce iOS na levé straně se zobrazí rozložení před zadáním platné stáří. Android a UPW obrazovky zobrazit **odeslat** tlačítko povoleno po nastavení stáří:

[![Položka osoba](commanding-images/personentry-small.png "položky osoba")](commanding-images/personentry-large.png#lightbox "položka osoby")

Program nemá žádné zařízení pro úpravu existující položky a nedojde k uložení položky při navigaci pryč z stránky.

Veškerou logiku pro **nový**, **odeslat**, a **zrušit** tlačítka je zpracována v `PersonCollectionViewModel` prostřednictvím definice `NewCommand`, `SubmitCommand`, a `CancelCommand` vlastnosti. Konstruktor třídy `PersonCollectionViewModel` nastaví tyto tři vlastnosti pro objekty typu `Command`.  

A [konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) z `Command` třída umožňuje předat argumenty typu `Action` a `Func<bool>` odpovídající `Execute` a `CanExecute` metody. Je nejjednodušší definovat tyto akce a funkce jako lambda funkce přímo `Command` konstruktoru. Tady je definice `Command` objekt pro `NewCommand` vlastnost:

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

Pokud uživatel klikne **nový** tlačítko, `execute` funkce předány `Command` konstruktor provádí. Tím se vytvoří nový `PersonViewModel` objektu, nastaví obslužnou rutinu k tomuto objektu `PropertyChanged` událostí, nastaví `IsEditing` k `true`a volá `RefreshCanExecutes` metody definované po konstruktoru.

Kromě provádění `ICommand` rozhraní, `Command` třída také definuje metodu s názvem `ChangeCanExecute`. Vaše ViewModel by měly volat `ChangeCanExecute` pro `ICommand` vlastnost pokaždé, když se něco stane s, který může změnit návratový typ `CanExecute` metoda. Volání `ChangeCanExecute` způsobí, že `Command` třídy, která se aktivuje `CanExecuteChanged` metody. `Button` Obslužnou rutinu pro tuto událost se připojilo a odpovídá voláním `CanExecute` znovu a pak povolení založena na hodnotě vrácení této metody.

Při `execute` metoda `NewCommand` volání `RefreshCanExecutes`, `NewCommand` vlastnost získá volání `ChangeCanExecute`a `Button` volání `canExecute` metody, které nyní vrací `false` protože `IsEditing`vlastnost je nyní `true`.

`PropertyChanged` Obslužné rutiny pro novou `PersonViewModel` objektu volání `ChangeCanExecute` metoda `SubmitCommand`. Zde je, jak je implementovaná vlastnost tohoto příkazu:


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

`canExecute` Fungovat `SubmitCommand` je volána pokaždé, když se vlastnost změnil `PersonViewModel` objekt, který právě upravujete. Vrátí `true` pouze tehdy, když `Name` vlastnost je alespoň jeden znak, a `Age` je větší než 0. V tu chvíli **odeslat** aktivuje tlačítko.

`execute` Fungovat **odeslat** odebere obslužnou rutinu změny vlastnosti z `PersonViewModel`, přidá objekt, který má `Persons` kolekce a všechno, co vrátí počáteční podmínky.

`execute` Fungovat **zrušit** tlačítko dělá všechno, **odeslat** tlačítko nemá execept přidejte objekt do kolekce:

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

`canExecute` Vrátí metoda `true` kdykoli `PersonViewModel` se právě upravuje.

Tyto postupy by mohly být přizpůsobeny pro složitější scénáře: vlastnost v `PersonCollectionViewModel` může být vázaný na `SelectedItem` vlastnost `ListView` pro úpravu existující položky a **odstranit** může být přidán tlačítko Odstranit Tyto položky.

Není nutné definovat `execute` a `canExecute` metody jako funkce lambda. Můžete napsat je jako běžné privátní metody v ViewModel a odkazovat na nich `Command` konstruktory. Tento přístup však mají za následek mnoho metod, které jsou odkazovány pouze jednou v ViewModel.

## <a name="using-command-parameters"></a>Pomocí parametrů příkazu

Někdy je vhodné pro jedno nebo více tlačítek (nebo jiných objektů uživatelského rozhraní) sdílet stejný `ICommand` vlastnost ViewModel. V tomto případě použijete `CommandParameter` vlastnost k rozlišení mezi tlačítky.

Můžete dál používat `Command` třídy pro tyto sdílené `ICommand` vlastnosti. Definuje třídu [alternativní konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action{System.Object},System.Func{System.Object,System.Boolean})) , který přijme `execute` a `canExecute` metody s parametry typu `Object`. Toto je způsob, jakým `CommandParameter` je předán do těchto metod.

Ale při použití `CommandParameter`, je nejjednodušší museli používat obecná [ `Command<T>` ](xref:Xamarin.Forms.Command`1) tak, aby určovala typ objektu nastavena na `CommandParameter`. `execute` a `canExecute` metody, které zadáte mít parametry typu.

**Desítkové klávesnice** stránky znázorňuje tuto techniku ve kterém se naučíte implementovat klávesnice pro zadání desetinná čísla. `BindingContext` Pro `Grid` je `DecimalKeypadViewModel`. `Entry` Vlastnosti tohoto ViewModel je vázán na `Text` vlastnost `Label`. Všechny `Button` objekty jsou svázány s různými příkazy v ViewModel: `ClearCommand`, `BackspaceCommand`, a `DigitCommand`:

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

Sdílení tlačítka 11 pro 10 číslic a desetinná čárka vazbu na `DigitCommand`. `CommandParameter` Rozlišuje mezi tato tlačítka. Nastavte na hodnotu `CommandParameter` jsou obvykle stejné jako text, zobrazený na tlačítko s výjimkou desetinné místo, které v zájmu přehlednosti se zobrazí s tečkou uprostřed znak.

Tady je program v akci:

[![Desetinné klávesnice](commanding-images/decimalkeyboard-small.png "desítkové klávesnice")](commanding-images/decimalkeyboard-large.png#lightbox "desítkové klávesnice")

Všimněte si, že tlačítko pro desetinné čárky všechny tři snímcích obrazovky je zakázaná, protože zadané číslo již obsahuje desetinnou čárkou.

`DecimalKeypadViewModel` Definuje `Entry` vlastnost typu `string` (což je jediná vlastnost, která aktivuje `PropertyChanged` událostí) a tři vlastnosti typu `ICommand`:

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

Odpovídající tlačítko na `ClearCommand` je vždy povolena a jednoduše nastaví položku na "0":

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

Vzhledem k tomu tlačítko je vždy povoleno, není potřeba zadávat `canExecute` argumentu `Command` konstruktoru.

Logika pro zadání čísel a mazání pomocí klávesy BACKSPACE je trochu složité, protože pokud byly zadány žádné číslice, pak bude `Entry` vlastnosti je řetězec "0". Pokud uživatel zadá další nuly, pak bude `Entry` stále obsahuje pouze jeden nula. Pokud uživatel zadá jakékoli další číslice, nahradí tato číslice nula. Pokud uživatel zadá desetinné čárky před jakékoli další číslice, pak ale `Entry` je řetězec "0".

**Backspace** tlačítko je povoleno pouze v případě, že délka záznamu je větší než 1, nebo pokud `Entry` není neshoduje s řetězcem "0":

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

Logiku pro `execute` fungovat **Backspace** tlačítko zajišťuje, že `Entry` alespoň řetězec "0".

`DigitCommand` Vlastnost je vázána na 11 tlačítka, z nichž každý se identifikuje `CommandParameter` vlastnost. `DigitCommand` Může být nastaven na instanci běžné `Command` třídy, ale je jednodušší použít `Command<T>` obecnou třídu. Při používání řídicího rozhraní s XAML, `CommandParameter` vlastnosti jsou obvykle řetězce, a to je typ obecný argument. `execute` a `canExecute` pak obsahují funkce argumenty typu `string`:

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

`execute` Metoda přidá řetězec argumentu `Entry` vlastnost. Nicméně pokud výsledek začíná nulou (ale není nula a desetinná čárka) pak tento počáteční nuly musí odebrány `Substring` funkce.

`canExecute` Vrátí metoda `false` pouze v případě, že argument je desetinná čárka (značí, že stisknutí desetinná čárka) a `Entry` již obsahuje desetinnou čárkou.

Všechny `execute` volání metody `RefreshCanExecutes`, který pak volá `ChangeCanExecute` pro obě `DigitCommand` a `ClearCommand`. Tím se zajistí, že desetinné čárky a backspace tlačítka jsou povolené nebo zakázané v závislosti na aktuální pořadí zadaných číslic.

## <a name="adding-commands-to-existing-views"></a>Přidání příkazů pro stávající zobrazení

Pokud chcete použít se zobrazeními, s omezenou podporou řídicího rozhraní, je možné použít chování Xamarin.Forms, která převede událost do příkazu. Toto je popsáno v článku [ **opakovaně použitelného EventToCommandBehavior**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md).

## <a name="asynchronous-commanding-for-navigation-menus"></a>Asynchronní příkazů pro navigační nabídky

Příkazů je vhodné pro implementaci navigační nabídky, jako je například, že [ **ukázky vazby dat** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) program sám. Tady je součástí **MainPage.xaml**:


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

Při použití příkazů s XAML, `CommandParameter` vlastnosti jsou obvykle nastaveny na řetězce. V takovém případě však použít rozšíření značek XAML tak, aby `CommandParameter` je typu `System.Type`.

Každý `Command` vlastnost je vázána na vlastnost s názvem `NavigateCommand`. Že je vlastnost definována v souboru kódu na pozadí **MainPage.xaml.cs**:

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

Sady konstruktoru `NavigateCommand` vlastnost `execute` metodu, která vytvoří instanci `System.Type` parametr a pak přejde k němu. Protože `PushAsync` volání vyžaduje `await` operátor, `execute` metoda musí být označených jako o asynchronním. Toho se dosahuje pomocí `async` – klíčové slovo před seznam parametrů.

Konstruktor také nastaví `BindingContext` stránky na sebe sama tak, aby vazby odkazovat `NavigateCommand` v této třídě.

Pořadí kódu v tomto konstruktoru různá: `InitializeComponent` volání způsobí, že XAML, který má být analyzován, ale v tuto chvíli vazbu na vlastnost s názvem `NavigateCommand` nelze zpracovat, protože `BindingContext` je nastavena na `null`. Pokud `BindingContext` je nastavena v konstruktoru *před* `NavigateCommand` je nastavena, pak vazby lze vyřešit při `BindingContext` nastavena, ale v tuto chvíli `NavigateCommand` je stále `null`. Nastavení `NavigateCommand` po `BindingContext` nebude mít žádný vliv na vazby, protože změna `NavigateCommand` neaktivuje `PropertyChanged` události a vazby nebude vědět, že `NavigateCommand` je platný.

Nastavení obě `NavigateCommand` a `BindingContext` (v libovolném pořadí) před voláním `InitializeComponent` bude fungovat, protože obě komponenty vazby jsou nastaveny při analyzátoru XAML setká s jeho definicí vazby.

Datové vazby v některých případech může být velmi obtížné, ale jak už víte, v této sérii článků, jsou účinný a flexibilní a výrazně usnadňuje organizovat kód tak, že oddělíte logiku z uživatelského rozhraní.



## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Data vazby kapitola z knihy Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
