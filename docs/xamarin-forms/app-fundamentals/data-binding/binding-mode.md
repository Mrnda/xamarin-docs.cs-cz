---
title: Režim vazeb Xamarin.Forms
description: Tento článek vysvětluje, jak řídit tok informací mezi zdrojem a cílem pomocí režimu vazby, který je zadán s členem výčtu BindingMode. Každý vázanou vlastnost má výchozí režim vazby, označující režim ve skutečnosti když tuto vlastnost je cílem datové vazby.
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/01/2018
ms.openlocfilehash: a6eaf08d17f70c43f451361e27555a09c39f26a9
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935664"
---
# <a name="xamarinforms-binding-mode"></a>Režim vazeb Xamarin.Forms

V [předchozím článku](basic-bindings.md), **alternativní vazby kód** a **alternativní vazby XAML** vybrané stránky `Label` s jeho `Scale` vlastnost vázán `Value` vlastnost `Slider`. Protože `Slider` počáteční hodnota je 0, důvodem `Scale` vlastnost `Label` být nastavena na 0, ne 1 a `Label` zmizel.

V [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) ukázce **zpětná vazba** stránka je podobně jako programy v předchozím článku, s tím rozdílem, že datová vazba je definován na `Slider` spíše než na `Label`:

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

To může zdát zpětně: nyní `Label` je zdrojové datové vazby a `Slider` je cílem. Vazební odkazy `Opacity` vlastnost `Label`, který má výchozí hodnotu 1.

Jak byste asi očekávali, `Slider` je inicializován na hodnotu 1 v úvodním `Opacity` hodnotu `Label`. To můžete vidět na snímku obrazovky iOS na levé straně:

[![Zpětná vazba](binding-mode-images/reversebinding-small.png "zpětná vazba")](binding-mode-images/reversebinding-large.png#lightbox "zpětná vazba")

Ale může být překvapeni, který `Slider` i nadále fungovat, jak ukazují, snímky obrazovky pro Android a UPW. Vypadá to, že pro návrh, datové vazby funguje lepší v případě `Slider` je cíl vazby místo `Label` protože inicializace funguje jako bychom předpokládali.

Rozdíl mezi **zpětná vazba** ukázky a starší ukázky zahrnuje *vazby režimu*.

## <a name="the-default-binding-mode"></a>Výchozí režim vazeb

Je zadaný režim vazbu se členem [ `BindingMode` ](xref:Xamarin.Forms.BindingMode) výčtu:

- [`Default`](xref:Xamarin.Forms.BindingMode.Default)
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) &ndash; data prochází obou směrech mezi zdrojem a cílem
- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) &ndash; data přejde ze zdroje do cíle
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; data přejde z cíle na zdroj
- [`OneTime`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; data přejde ze zdroje na cíl, ale pouze tehdy, když `BindingContext` změny (nové s Xamarin.Forms 3.0)

Každý vázanou vlastnost má výchozí režim, který je nastaven, když se vytvoří vlastnost s vazbou vazeb a který je k dispozici [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) vlastnost `BindableProperty` objektu. Tento režim výchozí vazby označuje režim ve skutečnosti když tuto vlastnost je cílem datové vazby.

Výchozí režim vazby pro většinu vlastností, jako `Rotation`, `Scale`, a `Opacity` je `OneWay`. Pokud tyto vlastnosti jsou datové vazby cíle, je nastavena vlastnost target ze zdroje.

Však vazba výchozí režim pro `Value` vlastnost `Slider` je `TwoWay`. To znamená, že `Value` vlastnost je cíl vazby dat, pak cíl je nastavena ze zdroje (obvyklým) ale zdroj je také nastavena z cíle. Toto je co umožňuje `Slider` nastavit v úvodním `Opacity` hodnotu.

Tato Obousměrná vazba zdát, že k vytvoření nekonečnou smyčku, ale který nestane. Vlastnosti umožňující vazbu nevydá signál změnu vlastnosti, pokud vlastnost skutečně změní. To zabraňuje nekonečnou smyčku.

### <a name="two-way-bindings"></a>Obousměrné vazby

Nejvíce umožňujících vazbu vlastnosti mají výchozí režim vazby `OneWay` , ale následující vlastnosti mají výchozí režim vazby `TwoWay`:

- `Date` Vlastnost `DatePicker`
- `Text` Vlastnost `Editor`, `Entry`, `SearchBar`, a `EntryCell`
- `IsRefreshing` Vlastnost `ListView`
- `SelectedItem` Vlastnost `MultiPage`
- `SelectedIndex` a `SelectedItem` vlastnosti `Picker`
- `Value` Vlastnost `Slider` a `Stepper`
- `IsToggled` Vlastnost `Switch`
- `On` Vlastnost `SwitchCell`
- `Time` Vlastnost `TimePicker`

Tyto konkrétní vlastnosti, které jsou definovány jako `TwoWay` velmi dobré z důvodu:

Datové vazby se používají s architekturou aplikací Model-View-ViewModel (MVVM), tříd ViewModel při vázání dat zdroje a zobrazení, které se skládá ze zobrazení, jako `Slider`, jsou datové vazby cíle. Vazby MVVM vypadat podobně jako **zpětná vazba** ukázka více než vazby v předchozích ukázkách. Je velmi pravděpodobné, že chcete, aby každé zobrazení na stránce inicializovat pomocí hodnoty odpovídající vlastnosti v ViewModel, ale v zobrazení by měl ovlivní také vlastnost ViewModel.

Vlastnosti s výchozí vazby režimy `TwoWay` jsou tyto vlastnosti nejpravděpodobnější, který se má použít ve scénářích MVVM.

### <a name="one-way-to-source-bindings"></a>Více-Way-na zdroje vazby

Vlastnosti jen pro čtení s možností vazby mít režim výchozí vazby `OneWayToSource`. Existuje pouze jednu vlastnost podporující vazby r/w, který má režim výchozí vazby `OneWayToSource`:

- `SelectedItem` Vlastnost `ListView`

Důvody, který se zúčastňuje vazby `SelectedItem` vlastnost by měla mít za následek nastavení zdroje připojení. Příklad dále v tomto článku přepsání tohoto chování.

### <a name="one-time-bindings"></a>Jednorázové vazby

Režim výchozí vazby mít několik vlastností `OneTime`. Toto jsou:

- `IsTextPredictionEnabled` Vlastnost `Entry`
- `Text`, `BackgroundColor`, a `Style` vlastnosti `Span`.

Cílové vlastnosti vazby režim `OneTime` jsou aktualizovány pouze v případě změny kontextu vazby. U vazeb na tyto vlastnosti cíle zjednodušuje infrastruktura vazeb, protože to není nezbytné pro sledování změn ve vlastnosti zdroje.

## <a name="viewmodels-and-property-change-notifications"></a>Změna vlastnosti oznámení a modely ViewModels

**Jednoduchý výběr barvy** stránky demonstruje použití jednoduchého ViewModel. Datové vazby umožní uživateli vybrat barvu použitím tří `Slider` prvky pro odstín, sytost a světelnost.

ViewModel je zdrojové datové vazby. Nepodporuje ViewModel *není* definovat vlastnosti umožňující vazbu, ale to implementovat mechanismus oznámení, která umožňuje infrastruktura vazeb chcete být upozorňováni při změně hodnoty vlastnosti. Tento mechanismus oznámení je [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) rozhraní, který definuje jednu vlastnost s názvem [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged). Třídu, která implementuje toto rozhraní obvykle vyvolá událost, pokud jeden z jeho veřejné vlastnosti změní hodnotu. Událost nemusí být odesláno, pokud vlastnost nikdy nemění. ( `INotifyPropertyChanged` Rozhraní je také implementováno pomocí `BindableObject` a `PropertyChanged` událost se aktivuje pokaždé, když se změní hodnotu vázanou vlastnost.)

`HslColorViewModel` Třída definuje pěti vlastností: `Hue`, `Saturation`, `Luminosity`, a `Color` vlastnosti spolu souvisejí. Když některý tři změny hodnoty barev komponenty `Color` přepočítá. vlastnost a `PropertyChanged` jsou vyvolávány události pro všechny čtyři vlastnosti:

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

Když `Color` změny vlastností, statické `GetNearestColorName` metoda ve `NamedColor` třídy (také součástí **DataBindingDemos** řešení) nejbližší pojmenovaná barva získá a nastaví `Name` vlastnost. To `Name` vlastnost má privátní `set` přístupový objekt, proto ji nelze nastavit z mimo třídu.

Není-li jako zdroj vazby je nastavena ViewModel, infrastruktura vazeb připojí obslužná rutina `PropertyChanged` událostí. Tímto způsobem vazby můžete být upozorněni na změny vlastností a pak můžete nastavit vlastnosti cíle v změněné hodnoty.

Ale když cílová vlastnost (nebo `Binding` definice na cílovou vlastnost) má `BindingMode` z `OneTime`, není nutné pro infrastruktura vazeb připojit obslužnou rutinu na `PropertyChanged` událostí. Vlastnost target je aktualizovat pouze tehdy, když `BindingContext` změny a ne samotné vlastnosti zdroje změny.

**Jednoduchý výběr barvy** vytvoří soubor XAML `HslColorViewModel` ve slovníku prostředků a inicializuje stránky `Color` vlastnost. `BindingContext` Vlastnost `Grid` je nastavena na `StaticResource` rozšíření odkazují na tento prostředek připojení:

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

`BoxView`, `Label`a tři `Slider` zobrazení dědit z kontextu vazby `Grid`. Tato zobrazení jsou všechny cíle vazby, které odkazují na vlastnosti zdroje v ViewModel. Pro `Color` vlastnost `BoxView`a `Text` vlastnost `Label`, jsou datové vazby `OneWay`: z vlastností v ViewModel jsou nastaveny vlastnosti v zobrazení.

`Value` Vlastnost `Slider`, ale `TwoWay`. To umožňuje každému `Slider` nastavit z ViewModel a také pro ViewModel nastavit z každého `Slider`.

Při prvním spuštění programu `BoxView`, `Label`a tři `Slider` prvky jsou všechny sady z ViewModel podle počáteční `Color` vlastnost nastavit, pokud byla vytvořena instance ViewModel. To můžete vidět na snímku obrazovky iOS na levé straně:

[![Výběr jednoduchých barvy](binding-mode-images/simplecolorselector-small.png "výběr barvy jednoduché")](binding-mode-images/simplecolorselector-large.png#lightbox "výběr jednoduchého barvy")

Při manipulaci s posuvníky, `BoxView` a `Label` se aktualizují odpovídajícím způsobem, jak je znázorněno v snímky obrazovky pro Android a UPW.

Vytvoření instance ViewModel ve slovníku prostředků je jeden běžný postup. Je také možné vytvořit instanci ViewModel v rámci elementu tagy vlastnosti `BindingContext` vlastnost. V **jednoduchý výběr barvy** XAML souboru, zkuste odebrat `HslColorViewModel` ze slovníku prostředků a nastavte ho na `BindingContext` vlastnost `Grid` tímto způsobem:

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>

    ···

</Grid>
```

Kontext vazby, je možné nastavit v mnoha různými způsoby. V některých případech použití modelu code-behind soubor vytvoří instanci ViewModel a nastaví ji na `BindingContext` vlastnosti stránky. Jedná se o všechno platné přístupy.

## <a name="overriding-the-binding-mode"></a>Režim vazby přepsání

Pokud výchozí režim vazby na vlastnost target není vhodný pro konkrétní datové vazby, je možné přepsat tak, že nastavíte [ `Mode` ](xref:Xamarin.Forms.BindingBase.Mode) vlastnost `Binding` (nebo [ `Mode` ](xref:Xamarin.Forms.Xaml.BindingExtension.Mode) vlastnost `Binding` – rozšíření značek) na jeden z členů `BindingMode` výčtu.

Však nastavení `Mode` vlastnost `TwoWay` vždy nefunguje podle očekávání. Například, pokuste se změnit **alternativní XAML vazby** souboru XAML `TwoWay` v definici vazby:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Lze očekávat, který `Slider` by být inicializovány na počáteční hodnotu `Scale` vlastnost, což je 1, ale který nestane. Když `TwoWay` vazby je inicializována, cíl je ze zdroje nejprve nastavit, což znamená, že `Scale` je nastavena na `Slider` výchozí hodnota 0. Když `TwoWay` vazby je nastavena na `Slider`, pak bude `Slider` je zpočátku nastaven ze zdroje.

Můžete nastavit režim vazby `OneWayToSource` v **alternativní XAML vazby** vzorku:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

Nyní `Slider` je inicializován na hodnotu 1 (výchozí hodnota `Scale`) ale na manipulaci `Slider` nemá vliv `Scale` vlastnost, aby to nebylo velmi užitečné.

Velmi užitečné použití přepsání výchozí režim vazby s `TwoWay` zahrnuje `SelectedItem` vlastnost `ListView`. Je výchozí režim vazby `OneWayToSource`. Pokud je datové vazby nastavená na `SelectedItem` vlastnost tak, aby odkazovaly na vlastnost zdroje ve ViewModel, nastavte tuto vlastnost zdroje z `ListView` výběr. Nicméně v některých případech můžete také `ListView` inicializované ze ViewModel.

**Nastavení ukázkových** tuto techniku ukazuje stránky. Tato stránka představuje jednoduché provedení nastavení aplikace, které jsou často definovány v ViewModel, jako je například to `SampleSettingsViewModel` souboru:

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

Každé nastavení aplikace je vlastnost, která se uloží do slovníku vlastností Xamarin.Forms v metodu s názvem `SaveState` a načíst z tohoto slovníku v konstruktoru. Směrem k dolní části třídy jsou dvě metody, které pomáhají zjednodušit modely ViewModels a aby byly méně náchylná k chybám. `OnPropertyChanged` Volitelný parametr, který je nastaven na volajícím vlastnost má metodu ve spodní části. Tím se vyhnete pravopisné chyby při zadávání názvu vlastnosti jako řetězec.

`SetProperty` Metody ve třídě nemá ještě víc: porovnává hodnotu, která je nastavena na vlastnost s hodnotou uloženou jako pole a jen volá `OnPropertyChanged` při dvě hodnoty nejsou shodné.

`SampleSettingsViewModel` Třída definuje dvě vlastnosti pro barvu pozadí: `BackgroundNamedColor` vlastnost je typu `NamedColor`, což je třída také součástí **DataBindingDemos** řešení. `BackgroundColor` Vlastnost je typu `Color`a získat z `Color` vlastnost `NamedColor` objektu.

`NamedColor` Třída používá reflexi .NET vytvořit výčet všech statická veřejná pole Xamarin.Forms `Color` strukturu a slouží k uložení s jejich názvy v kolekci přístupné ze statické `All` vlastnost:

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

`App` Třídy v **DataBindingDemos** projekt definuje vlastnost s názvem `Settings` typu `SampleSettingsViewModel`. Tato vlastnost je inicializován při `App` je vytvořena instance třídy a `SaveState` metoda je volána, když `OnSleep` volání metody:

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

Další informace o metodách životního cyklu aplikací, najdete v článku [ **životní cyklus aplikace**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Téměř vše, co jiného se v zpracovává **SampleSettingsPage.xaml** souboru. `BindingContext` Stránky se nastavuje pomocí `Binding` – rozšíření značek: Zdroj vazby je statické `Application.Current` vlastnost, která je instance z `App` třídy v projektu a `Path` je nastavena na `Settings` vlastnost, která je `SampleSettingsViewModel` objektu:

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

Všechny podřízené stránky zdědí kontextu vazby. Většina vazby na této stránce jsou vlastnosti v `SampleSettingsViewModel`. `BackgroundColor` Vlastnost se používá k nastavení `BackgroundColor` vlastnost `StackLayout`a `Entry`, `DatePicker`, `Switch`, a `Stepper` vlastnosti jsou vázány na jiné vlastnosti v ViewModel.

`ItemsSource` Vlastnost `ListView` je nastavena na statickou `NamedColor.All` vlastnost. To vyplní `ListView` se všemi `NamedColor` instancí. Pro každou položku v `ListView`, kontextu vazby pro položku nastavena `NamedColor` objektu. `BoxView` a `Label` v `ViewCell` je vázána na vlastnosti v `NamedColor`.

`SelectedItem` Vlastnost `ListView` je typu `NamedColor`a je svázaná `BackgroundNamedColor` vlastnost `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Výchozí režim vazby pro `SelectedItem` je `OneWayToSource`, která nastaví vlastnost ViewModel z vybrané položky. `TwoWay` Režim umožňuje `SelectedItem` inicializované ze ViewModel.

Nicméně, když `SelectedItem` nastavit tímto způsobem `ListView` automaticky neposouvá zobrazení vybrané položky. Je nutné trochu kód v souboru kódu na pozadí:

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

Snímek obrazovky s Iosem na levé straně znázorňuje program při prvním spuštění. Konstruktor v `SampleSettingsViewModel` inicializuje barvu pozadí na bílou a které je vybrané v `ListView`:

[![Ukázkové nastavení](binding-mode-images/samplesettings-small.png "ukázkový nastavení")](binding-mode-images/samplesettings-large.png#lightbox "ukázkový nastavení")

Na dvou snímcích obrazovky zobrazit změněné nastavení. Při experimentování se tuto stránku, nezapomeňte vložit program do režimu spánku nebo ukončit v zařízení nebo emulátor, který je spuštěn. Ukončení programu v ladicím programu sady Visual Studio nebude způsobovat `OnSleep` přepsat v `App` třída, která se má volat.

V následujícím článku uvidíte, jak určit [ **formátování řetězce** ](string-formatting.md) nad vázáním dat, které jsou nastaveny na `Text` vlastnost `Label`.


## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Data vazby kapitola z knihy Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
