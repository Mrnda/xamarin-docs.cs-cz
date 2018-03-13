---
title: "Vazba režimu"
description: "Řízení toku informací mezi zdrojem a cílem"
ms.topic: article
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 887dc3cf710fb75d05d02af179bc218c15d31f97
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="binding-mode"></a>Vazba režimu

V [předchozí článek](basic-bindings.md), **alternativní kód vazby** a **alternativní XAML vazby** vybrané stránky `Label` s jeho `Scale` vlastnost vázána `Value` vlastnost `Slider`. Protože `Slider` počáteční hodnota je 0, příčinou `Scale` vlastnost `Label` být nastavena na 0, nikoli 1 a `Label` smazán.

V [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) ukázce **zpětná vazba** stránka je podobná programy v předchozím článku, s tím rozdílem, že datové vazby musí být definován `Slider` spíše než na `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.ReverseBindingPage"
             Title="Reverse Binding">
    <StackLayout Padding="10, 0">

        <Label x:Name="label" 
               Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                VerticalOptions="CenterAndExpand"
                Value="{Binding Source={x:Reference label},
                                Path=Opacity}" />
    </StackLayout>
</ContentPage>
```

Na první, může se to zdát zpětné: nyní `Label` je zdrojem vazby dat a `Slider` je cílem. Vazba odkazy `Opacity` vlastnost `Label`, který má výchozí hodnotu 1.

Podle očekávání, `Slider` je inicializováno z počáteční hodnotu 1 `Opacity` hodnotu `Label`. Můžete se podívat na snímku obrazovky iOS na levé straně:

[![Zpětná vazba](binding-mode-images/reversebinding-small.png "zpětná vazba")](binding-mode-images/reversebinding-large.png#lightbox "zpětná vazba")

Ale mohou být překvapeni, který `Slider` bude pořád fungovat, jak ukazují, Android a UWP snímky. Vypadá to naznačuje, že datové vazby funguje lépe, když `Slider` je cílem vazby místo `Label` protože inicializace funguje stejně, jako jsme by se dalo očekávat.

Rozdíl mezi **zpětná vazba** ukázka a starší ukázky zahrnuje *vazby režimu*.

## <a name="the-default-binding-mode"></a>Výchozí režim vazby

Vazba režimu se zadaným členem [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) výčtu: 

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) 
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) &ndash; data přejde obou směrech mezi zdrojem a cílem
- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) &ndash; data přejde ze zdroje k cíli
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) &ndash; data přejde z cíle na zdroj

Každý vazbu vlastnost má výchozí vazby režimu, který je nastavený při vytvoření vlastnost vazbu a který je k dispozici [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) vlastnost `BindableProperty` objektu. Tento režim vazby výchozí režim označuje platit při této vlastnosti je cílem datové vazby.

Výchozí režim vazby pro většinu vlastností, jako `Rotation`, `Scale`, a `Opacity` je `OneWay`. Pokud jsou tyto vlastnosti datové vazby cíle, je nastavit vlastnost target ze zdroje.

Nicméně výchozí režim vazby pro `Value` vlastnost `Slider` je `TwoWay`. To znamená, že pokud `Value` vlastnost je cílem vazby dat, pak cíl je nastaven ze zdroje (obvyklým) ale zdroj nastavený i z cíle. Toto je co umožňuje `Slider` nastavení z počáteční `Opacity` hodnotu.

Tato vazba obousměrná zdát, že vytvoří nekonečnou smyčku, ale který nedojde. Vazbu vlastnosti není signál změnu vlastnosti, pokud vlastnost ve skutečnosti změny. To zabraňuje nekonečné smyčce.

### <a name="two-way-bindings"></a>Obousměrné vazby

Většina vazbu vlastnosti mají výchozí režim vazba z `OneWay` , ale následující vlastnosti mají výchozí režim vazba z `TwoWay`:

- `Date` Vlastnost `DatePicker`
- `Text` Vlastnost `Editor`, `Entry`, `SearchBar`, a `EntryCell`
- `IsRefreshing` Vlastnost `ListView`
- `SelectedItem` Vlastnost `MultiPage`
- `SelectedIndex` a `SelectedItem` vlastnosti `Picker`
- `Value` Vlastnost `Slider` a `Stepper`
- `IsToggled` Vlastnost `Switch` 
- `On` Vlastnost `SwitchCell`
- `Time` Vlastnost `TimePicker`

Tyto konkrétní vlastnosti, které jsou definovány jako `TwoWay` velmi dobré důvodu: 

V případě vazby dat používají s architekturou aplikace Model-View-ViewModel (modelem MVVM), třída ViewModel je zdroji datové vazby a zobrazení, která se skládá ze zobrazení, jako `Slider`, jsou datová vazba cíle. Rozhraní MVVM vazby vypadat **zpětná vazba** ukázka víc než v předchozích ukázkách vazby. Je velmi pravděpodobné, že chcete jednotlivých zobrazení na stránce inicializovat pomocí hodnoty odpovídající vlastnosti v ViewModel, ale v zobrazení by měl ovlivní také vlastnost ViewModel.

Vlastnosti s režimy vazby výchozí `TwoWay` jsou tyto vlastnosti nejpravděpodobnější, který se má použít ve scénářích rozhraní MVVM.

### <a name="one-way-to-source-bindings"></a>Více-Way-na Source vazby

Jen pro čtení vazbu vlastnosti mají výchozí režim vazba z `OneWayToSource`. Je pouze jednu vlastnost vazbu pro čtení a zápis, který má výchozí režim vazby z `OneWayToSource`:

- `SelectedItem` Vlastnost `ListView`

Logický základ hlediska je, že vazbu na `SelectedItem` vlastnost by měla mít za následek nastavení zdroji vazby. Příklad později v tomto článku přepsání tohoto chování.

## <a name="viewmodels-and-property-change-notifications"></a>ViewModels a změnu vlastnosti oznámení

**Jednoduchý selektor barva** stránky demonstruje použití jednoduchého ViewModel. Datové vazby povolit uživateli vybrat barvu pomocí tří `Slider` prvky pro hue, sytost a Světlost.

ViewModel je zdrojem datové vazby. Nemá ViewModel *není* definovat vlastnosti vazbu, ale ho implementovat oznámení mechanismus, který umožňuje infrastruktura vazeb oznámení o změně hodnoty vlastnosti. Tento mechanismus oznámení je [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) rozhraní, který definuje jednu vlastnost s názvem [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/). Třídy, které toto rozhraní implementuje obecně aktivuje událost, pokud jeden z jeho veřejné vlastnosti změní hodnotu. Událost nemusí být aktivována, pokud se vlastnost nikdy změní. ( `INotifyPropertyChanged` Rozhraní je implementováno také podle `BindableObject` a `PropertyChanged` událost je aktivována například při každé změně hodnoty vazbu vlastnosti.)

`HslColorViewModel` Třída definuje pěti vlastností: `Hue`, `Saturation`, `Luminosity`, a `Color` vlastnosti spolu souvisejí. Pokud takové jsou tři hodnoty změny barev součásti `Color` vlastnost jsou přepočítána, a `PropertyChanged` při vyvolání událostí pro všechny čtyři vlastnosti:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name; 

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

Když `Color` změny vlastností, statické `GetNearestColorName` metoda v `NamedColor` – třída (také obsaženy v **DataBindingDemos** řešení) nejbližší pojmenovaná barva získá a nastaví `Name` vlastnost. To `Name` vlastnost má privátního `set` přistupujícího objektu, takže ji nelze nastavit z mimo třídu.

Pokud jako zdroj vazba je nastavená ViewModel, infrastruktura vazeb připojí obslužnou rutinu do `PropertyChanged` událostí. Tímto způsobem vazby můžete upozornění na změny vlastnosti a pak můžete nastavit vlastnosti cíle v změněné hodnoty.

**Jednoduchý selektor barva** souboru XAML vytvoří `HslColorViewModel` ve slovníku prostředků a inicializuje stránky `Color` vlastnost. `BindingContext` Vlastnost `Grid` je nastaven na `StaticResource` vazby rozšíření, chcete-li tento prostředek:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SimpleColorSelectorPage">

    <ContentPage.Resources>
        <ResourceDictionary>
            <local:HslColorViewModel x:Key="viewModel" 
                                     Color="MediumTurquoise" />

            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
        
    <Grid BindingContext="{StaticResource viewModel}">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <BoxView Color="{Binding Color}"
                 Grid.Row="0" />

        <StackLayout Grid.Row="1"
                     Margin="10, 0">

            <Label Text="{Binding Name}"
                   HorizontalTextAlignment="Center" />

            <Slider Value="{Binding Hue}" />
    
            <Slider Value="{Binding Saturation}" />

            <Slider Value="{Binding Luminosity}" />
        </StackLayout>
    </Grid>
</ContentPage>
```

`BoxView`, `Label`a tři `Slider` zobrazení dědit z kontextu vazby `Grid`. Tato zobrazení se všechny vazby cíle, které odkazují na vlastnosti zdroje v ViewModel. Pro `Color` vlastnost `BoxView`a `Text` vlastnost `Label`, vazby dat jsou `OneWay`: z vlastností v ViewModel jsou nastaveny vlastnosti v zobrazení.

`Value` Vlastnost `Slider`, je však `TwoWay`. To umožňuje každý `Slider` možné nastavit ViewModel a také pro ViewModel nastavení z každé `Slider`. 

Při prvním spuštění programu, `BoxView`, `Label`a tři `Slider` prvky jsou všechny sady z ViewModel podle počáteční `Color` vlastnost nastavena, když ViewModel byla vytvořena instance. Můžete se podívat na snímku obrazovky iOS na levé straně:

[![Výběr barvy jednoduché](binding-mode-images/simplecolorselector-small.png "výběr barvy jednoduché")](binding-mode-images/simplecolorselector-large.png#lightbox "výběr jednoduché barvy")

Při manipulaci s posuvníků, `BoxView` a `Label` se aktualizují podle toho, které jsou popsány v Android a UWP snímky.

Vytváření instancí ViewModel ve slovníku prostředků je jedním z běžných postupů. Je také možné vytvořit instanci ViewModel v rámci značky elementu vlastnost pro `BindingContext` vlastnost. V **jednoduchý selektor barva** XAML souboru, zkuste odebrat `HslColorViewModel` ze slovníku prostředků a nastavte ji na `BindingContext` vlastnost `Grid` podobné výjimky:

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>
        
    ···

</Grid>
```

Kontext vazby můžete nastavit v mnoha různými způsoby. V některých případech kódu soubor vytvoří ViewModel a nastaví na `BindingContext` vlastnost stránky. Toto jsou platné všechny přístupy.

## <a name="overriding-the-binding-mode"></a>Přepsání režimu vazby

Pokud výchozí režim vazby na vlastnost target není vhodná pro konkrétní datové vazby, je možné přepsat jeho nastavení [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) vlastnost `Binding` (nebo [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Mode/) vlastnost `Binding` – rozšíření značek) do jednoho z členů `BindingMode` výčtu.

Nastavení však `Mode` vlastnost `TwoWay` vždycky nebude fungovat podle očekávání. Zkuste například úprava **alternativní XAML vazby** souboru XAML `TwoWay` v definici vazby:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Může být očekávána, `Slider` bude inicializována tak, aby počáteční hodnota `Scale` vlastnost, která je 1, ale který nedojde. Když `TwoWay` vazba je inicializován, cíl nastavená ze zdroje nejprve, to znamená, že `Scale` je nastavena na `Slider` výchozí hodnota 0. Při `TwoWay` vazba je nastavený na `Slider`, pak se `Slider` zpočátku nastavena ze zdroje.

Můžete nastavit režim vazbu na `OneWayToSource` v **alternativní XAML vazby** ukázka:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

Nyní `Slider` je inicializováno 1 (výchozí hodnota `Scale`) ale manipulace s těmito `Slider` nemá vliv `Scale` vlastnost, tak toto není velmi užitečné.

Velmi užitečné uplatňování přepsání výchozí režim vazba s `TwoWay` zahrnuje `SelectedItem` vlastnost `ListView`. Je výchozí režim vazby `OneWayToSource`. Když je nastavená datová vazba na `SelectedItem` vlastnost, která má odkazovat na vlastnost zdroje ve ViewModel, tuto vlastnost zdroj bude nastavena z `ListView` výběr. Ale v některých případech můžete také `ListView` inicializované ze ViewModel.

**Nastavení ukázkových** stránky ukazuje tento postup. Tato stránka představuje jednoduchou implementaci nastavení aplikace, které jsou velmi často definované v ViewModel, jako je tato `SampleSettingsViewModel` souboru:

```csharp
public class SampleSettingsViewModel : INotifyPropertyChanged
{
    string name;
    DateTime birthDate;
    bool codesInCSharp;
    double numberOfCopies;
    NamedColor backgroundNamedColor;

    public event PropertyChangedEventHandler PropertyChanged;

    public SampleSettingsViewModel(IDictionary<string, object> dictionary)
    {
        Name = GetDictionaryEntry<string>(dictionary, "Name");
        BirthDate = GetDictionaryEntry(dictionary, "BirthDate", new DateTime(1980, 1, 1));
        CodesInCSharp = GetDictionaryEntry<bool>(dictionary, "CodesInCSharp");
        NumberOfCopies = GetDictionaryEntry(dictionary, "NumberOfCopies", 1.0);
        BackgroundNamedColor = NamedColor.Find(GetDictionaryEntry(dictionary, "BackgroundNamedColor", "White"));
    }

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public DateTime BirthDate
    {
        set { SetProperty(ref birthDate, value); }
        get { return birthDate; }
    }

    public bool CodesInCSharp
    {
        set { SetProperty(ref codesInCSharp, value); }
        get { return codesInCSharp; }
    }

    public double NumberOfCopies
    {
        set { SetProperty(ref numberOfCopies, value); }
        get { return numberOfCopies; }
    }

    public NamedColor BackgroundNamedColor
    {
        set
        {
            if (SetProperty(ref backgroundNamedColor, value))
            {
                OnPropertyChanged("BackgroundColor");
            }
        }
        get { return backgroundNamedColor; }
    }

    public Color BackgroundColor
    {
        get { return BackgroundNamedColor?.Color ?? Color.White; }
    }

    public void SaveState(IDictionary<string, object> dictionary)
    {
        dictionary["Name"] = Name;
        dictionary["BirthDate"] = BirthDate;
        dictionary["CodesInCSharp"] = CodesInCSharp;
        dictionary["NumberOfCopies"] = NumberOfCopies;
        dictionary["BackgroundNamedColor"] = BackgroundNamedColor.Name;
    }

    T GetDictionaryEntry<T>(IDictionary<string, object> dictionary, string key, T defaultValue = default(T))
    {
        return dictionary.ContainsKey(key) ? (T)dictionary[key] : defaultValue;
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

Každé nastavení aplikace je vlastnost, která je uložena do slovníku Xamarin.Forms vlastnosti v metodu s názvem `SaveState` a načíst z tohoto slovníku v konstruktoru. Směrem dolní třídy jsou dvě metody, které pomáhají zjednodušit ViewModels a aby byly méně náchylná k chybám. `OnPropertyChanged` Metoda v dolní části je volitelný parametr, který je nastaven na vlastnost volání. Tím je zabráněno pravopisné chyby při zadávání názvu vlastnosti jako řetězec. 

`SetProperty` Metodu v třídě nemá i více: porovnává hodnotu, která je nastavena na vlastnosti s hodnotou uloženou jako pole a pouze volá `OnPropertyChanged` Pokud nejsou tyto dvě hodnoty rovny. 

`SampleSettingsViewModel` Třída definuje dvě vlastnosti pro barvu pozadí: `BackgroundNamedColor` vlastnost je typu `NamedColor`, což je třída také součástí **DataBindingDemos** řešení. `BackgroundColor` Vlastnost je typu `Color`a se získávají z `Color` vlastnost `NamedColor` objektu.

`NamedColor` Třída používá reflexe .NET výčet všech statická veřejná pole platformě Xamarin.Forms `Color` struktura a jejich uložení s jejich názvy v kolekci, která je přístupná ze statické `All` vlastnost:

```csharp
public class NamedColor : IEquatable<NamedColor>, IComparable<NamedColor>
{
    // Instance members
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    public bool Equals(NamedColor other)
    {
        return Name.Equals(other.Name);
    }

    public int CompareTo(NamedColor other)
    {
        return Name.CompareTo(other.Name);
    }

    // Static members
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof(Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                                (int)(255 * color.R),
                                                (int)(255 * color.G),
                                                (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        all.Sort();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }

    public static NamedColor Find(string name)
    {
        return ((List<NamedColor>)All).Find(nc => nc.Name == name);
    }

    public static string GetNearestColorName(Color color)
    {
        double shortestDistance = 1000;
        NamedColor closestColor = null;

        foreach (NamedColor namedColor in NamedColor.All)
        {
            double distance = Math.Sqrt(Math.Pow(color.R - namedColor.Color.R, 2) +
                                        Math.Pow(color.G - namedColor.Color.G, 2) +
                                        Math.Pow(color.B - namedColor.Color.B, 2));

            if (distance < shortestDistance)
            {
                shortestDistance = distance;
                closestColor = namedColor;
            }
        }
        return closestColor.Name;
    }
}
```

`App` Třídy v **DataBindingDemos** projektu definuje vlastnost s názvem `Settings` typu `SampleSettingsViewModel`. Tato vlastnost je inicializován při `App` vytvoření instance třídy a `SaveState` metoda je volána, když `OnSleep` metoda je volána:

```csharp
public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        Settings = new SampleSettingsViewModel(Current.Properties);

        MainPage = new NavigationPage(new MainPage());
    }

    public SampleSettingsViewModel Settings { private set; get; }

    protected override void OnStart()
    {
        // Handle when your app starts
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Settings.SaveState(Current.Properties);
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
    }
}
```

Další informace o metody životního cyklu aplikací, najdete v článku [ **životní cyklus aplikace**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Téměř všechny ostatní zpracovává při **SampleSettingsPage.xaml** souboru. `BindingContext` Stránky se nastavuje pomocí `Binding` – rozšíření značek: zdroji vazba je statických `Application.Current` vlastnost, která je instance z `App` – třída v projektu a `Path` je nastaven na `Settings` vlastnost, která je `SampleSettingsViewModel` objektu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SampleSettingsPage"
             Title="Sample Settings"
             BindingContext="{Binding Source={x:Static Application.Current},
                                      Path=Settings}">

    <StackLayout BackgroundColor="{Binding BackgroundColor}"
                 Padding="10"
                 Spacing="10">

        <StackLayout Orientation="Horizontal">
            <Label Text="Name: "
                   VerticalOptions="Center" />

            <Entry Text="{Binding Name}"
                   Placeholder="your name"
                   HorizontalOptions="FillAndExpand"
                   VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Birth Date: "
                   VerticalOptions="Center" />

            <DatePicker Date="{Binding BirthDate}"
                        HorizontalOptions="FillAndExpand"
                        VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Do you code in C#? "
                   VerticalOptions="Center" />

            <Switch IsToggled="{Binding CodesInCSharp}"
                    VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Number of Copies: "
                   VerticalOptions="Center" />

            <Stepper Value="{Binding NumberOfCopies}"
                     VerticalOptions="Center" />

            <Label Text="{Binding NumberOfCopies}"
                   VerticalOptions="Center" />
        </StackLayout>

        <Label Text="Background Color:" />

        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
                  VerticalOptions="FillAndExpand"
                  RowHeight="40">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     HeightRequest="32"
                                     WidthRequest="32"
                                     VerticalOptions="Center" />

                            <Label Text="{Binding FriendlyName}"
                                   FontSize="24"
                                   VerticalOptions="Center" />
                        </StackLayout>                        
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Všechny podřízené objekty stránky zdědí kontextu vazby. Většina vazby na této stránce jsou vlastnosti v `SampleSettingsViewModel`. `BackgroundColor` Vlastnost se používá k nastavení `BackgroundColor` vlastnost `StackLayout`a `Entry`, `DatePicker`, `Switch`, a `Stepper` vlastnosti jsou vázány na jiné vlastnosti v ViewModel.

`ItemsSource` Vlastnost `ListView` nastavena na statickou `NamedColor.All` vlastnost. To doplní `ListView` se všemi `NamedColor` instance. Pro každou položku v `ListView`, kontext vazby pro položku je nastavena na `NamedColor` objektu. `BoxView` a `Label` v `ViewCell` je vázána na vlastnosti v `NamedColor`.

`SelectedItem` Vlastnost `ListView` je typu `NamedColor`a je vázána `BackgroundNamedColor` vlastnost `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Výchozí režim vazby pro `SelectedItem` je `OneWayToSource`, která nastaví vlastnost ViewModel z vybrané položky. `TwoWay` Režim umožňuje `SelectedItem` inicializované ze ViewModel. 

Ale, když `SelectedItem` nastavena tímto způsobem `ListView` automaticky neposouvá zobrazení vybrané položky. Je třeba trochu kód v souboru kódu na pozadí:

```csharp
public partial class SampleSettingsPage : ContentPage
{
    public SampleSettingsPage()
    {
        InitializeComponent();

        if (colorListView.SelectedItem != null)
        {
            colorListView.ScrollTo(colorListView.SelectedItem, 
                                   ScrollToPosition.MakeVisible, 
                                   false);
        }
    }
}
``` 

Snímek obrazovky iOS na levé straně ukazuje program při prvním spuštění. Konstruktor pro `SampleSettingsViewModel` inicializuje barva pozadí na bílou, na kterém jsou položky vybrané v `ListView`:

[![Ukázkové nastavení](binding-mode-images/samplesettings-small.png "ukázkové nastavení")](binding-mode-images/samplesettings-large.png#lightbox "ukázkové nastavení")

Na dva snímcích obrazovky zobrazit změnit nastavení. Při experimentování se tuto stránku, nezapomeňte uvést do režimu spánku nebo ho ukončit na zařízení nebo emulátor, který je spuštěn program. Ukončení programu z ladicího programu sady Visual Studio nezpůsobí `OnSleep` potlačení v `App` třída, která se má volat.

V další článku uvidíte určení [ **formátování řetězce** ](string-formatting.md) vazeb dat, které jsou nastaveny na `Text` vlastnost `Label`.


## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Kapitola vazby dat z adresáře Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
