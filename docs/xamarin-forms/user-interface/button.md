---
title: Tlačítko Xamarin.Forms
description: Tlačítko odpoví na klepněte nebo klikněte na tlačítko, která přesměruje aplikaci k provedení určité úlohy.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/01/2018
ms.openlocfilehash: 095736e77b2f502261f9b85ab73c45dce74309b9
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733989"
---
# <a name="xamarinforms-button"></a>Tlačítko Xamarin.Forms

_Tlačítko odpoví na klepněte nebo klikněte na tlačítko, která přesměruje aplikaci k provedení určité úlohy._ 

[ `Button` ](xref:Xamarin.Forms.Button) Nejzákladnější interaktivní ovládací prvek ve všech Xamarin.Forms. `Button` Obvykle zobrazuje krátký textový řetězec označující příkazu, ale můžete také zobrazit rastrový obrázek, nebo kombinaci textu a bitovou kopii. Stisknutí uživatele `Button` prstem nebo při kliknutí myší na tento příkaz spustit.

Většinu témat popsané níže odpovídají stránkám v [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) ukázka.

## <a name="handling-button-clicks"></a>Kliknutím na tlačítko řízení

`Button` definuje [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) událost, která je aktivována, když uživatel klepnutím `Button` pomocí ukazatele prstem nebo myši. Událost je aktivována, jestliže uvolnění tlačítka prstem nebo myš od plochy `Button`. `Button` Musí mít jeho [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) vlastnost nastavena na hodnotu `true` pro něj reagovat na odposlouchávání. 

**Základní klikněte na tlačítko** stránku [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) příklad ukazuje, jak vytvořit instanci `Button` v XAML a popisovač jeho `Clicked` událostí. **BasicButtonClickPage.xaml** soubor obsahuje `StackLayout` s oběma `Label` a `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.BasicButtonClickPage"
             Title="Basic Button Click">
    <StackLayout>
        
        <Label x:Name="label"
               Text="Click the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand" 
               HorizontalOptions="Center" />

        <Button Text="Click to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Clicked="OnButtonClicked" />
     
    </StackLayout>
</ContentPage>
```

`Button` Obvykle zabírají místo povolené pro ni. Například, pokud není nastavený `HorizontalOptions` vlastnost `Button` něco jiného než `Fill`, `Button` bude zabírat celou šířku jeho nadřazený objekt.

Ve výchozím nastavení `Button` je obdélníková, ale můžete udělit it zaokrouhlené rozích pomocí [ `CornerRadius` ](xref:Xamarin.Forms.Button.CornerRadius) vlastnost, jak je popsáno níže v části [ **tlačítko vzhled** ](#button-appearance).

[ `Text` ](xref:Xamarin.Forms.Button.Text) Vlastnost určuje text, který se zobrazí v `Button`. [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) Událostí je nastaven na obslužnou rutinu události s názvem `OnButtonClicked`. Tato obslužná rutina se nachází v souboru kódu **BasicButtonClickPage.xaml.cs**:

```csharp
public partial class BasicButtonClickPage : ContentPage
{
    public BasicButtonClickPage ()
    {
        InitializeComponent ();
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        await label.RelRotateTo(360, 1000);
    }
}
```

Když `Button` je stisknuté, `OnButtonClicked` metody. `sender` Argument je `Button` objekt odpovídající pro tuto událost. Můžete použít pro přístup `Button` objekt, nebo k rozlišení mezi několika `Button` objekty sdílení stejné `Clicked` událostí.

Tato konkrétní `Clicked` obslužnou rutinu volání funkce animace, otočí `Label` 360 stupňů v 1000 milisekund. Tady je programy spuštěné na zařízení iOS a Android a jako aplikace pro univerzální platformu Windows (UWP) na Windows 10 desktop:

[![Klikněte na tlačítko základní](button-images/BasicButtonClick.png "klikněte na tlačítko základní")](button-images/BasicButtonClick-Large.png#lightbox "klikněte na tlačítko základní")

Všimněte si, že `OnButtonClicked` metoda zahrnuje `async` modifikátor protože `await` se používá v rámci obslužné rutiny události. A `Clicked` vyžaduje obslužné rutiny události `async` modifikátor pouze v případě, že tělo obslužná rutina používá `await`.

Vykreslí jednotlivé platformy `Button` vlastní konkrétním způsobem. V [ **tlačítko vzhled** ](#button-appearance) části uvidíte, jak nastavit barvy a ujistěte se, `Button` viditelné pro více přizpůsobené vzhledy ohraničení. `Button` implementuje [ `IFontElement` ](xref:Xamarin.Forms.Internals.IFontElement) rozhraní, tak, aby zahrnovala [ `FontFamily` ](xref:Xamarin.Forms.Button.FontFamily), [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize), a [ `FontAttributes` ](xref:Xamarin.Forms.Button.FontAttributes)vlastnosti.

## <a name="creating-a-button-in-code"></a>Vytváření tlačítek v kódu

Je běžné k vytváření instancí `Button` v jazyce XAML, ale můžete také vytvořit `Button` v kódu. To může být vhodné, pokud aplikace potřebuje k vytvoření více tlačítek na základě dat, která je vyčíslitelná s `foreach` smyčky.

**Klikněte na tlačítko kódu** stránky ukazuje, jak vytvořit stránku, která je funkčně srovnatelný **základní klikněte na tlačítko** stránky ale zcela v jazyce C#:

```csharp
public class CodeButtonClickPage : ContentPage
{
    public CodeButtonClickPage ()
    {
        Title = "Code Button Click";

        Label label = new Label
        {
            Text = "Click the Button below",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };

        Button button = new Button
        {
            Text = "Click to Rotate Text!",
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };
        button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);

        Content = new StackLayout
        {
            Children =
            {
                label,
                button
            }
        };
    }
}
```

Všechno, co se provádí v konstruktoru třídy. Protože `Clicked` obslužná rutina je pouze jeden příkaz dlouhý, může být připojen k události velmi jednoduše:

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

Samozřejmě můžete také definovat obslužné rutiny události jako samostatnou metodu (stejně jako `OnButtonClick` metoda v **základní klikněte na tlačítko**) a připojte dané metody k události:

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>Zakázat tlačítko

V některých případech aplikace je v určitém stavu, kde konkrétní `Button` kliknutím není platná operace. V takových případech `Button` by mělo být zakázáno nastavením jeho `IsEnabled` vlastnost `false`. Classic příkladem je `Entry` řízení pro název souboru doplněny otevření souboru `Button`: `Button` by měla být povolená jenom v případě, že zadal nějaký text do `Entry`. Můžete použít `DataTrigger` pro tuto úlohu, jak je znázorněno [ **Data aktivační události** ](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) článku.

## <a name="using-the-command-interface"></a>Pomocí rozhraní příkazového

Je možné pro aplikaci reagovat na `Button` odposlouchávání bez zpracování `Clicked` událostí. `Button` Implementuje mechanismus alternativní oznámení, která je volána _příkaz_ nebo _tvorba příkazů_ rozhraní. Tento postup se skládá ze dvou vlastností:

- [`Command`](xref:Xamarin.Forms.Button.Command) typu [ `ICommand` ](xref:System.Windows.Input.ICommand), rozhraní definované v [ `System.Windows.Input` ](xref:System.Windows.Input) oboru názvů.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) vlastnost typu [ `Object` ](xref:System.Object).

Tento přístup je vhodný v souvislosti s datové vazby a, zvláště pokud implementace architektury Model-View-ViewModel (modelem MVVM). Tato témata jsou popsané v článcích [datové vazby](~/xamarin-forms/app-fundamentals/data-binding/index.md), [z vazby dat na rozhraní MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), a [rozhraní MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md).

V aplikaci rozhraní MVVM ViewModel definuje vlastnosti typu `ICommand` pak připojených k XAML `Button` elementů pomocí vazby na data. Také definuje Xamarin.Forms [ `Command` ]((xref:Xamarin.Forms.Command`1)) a [ `Command<T>` ](xref:Xamarin.Forms.Command`1) třídy implementující `ICommand` rozhraní a pomáhají ViewModel při definování vlastností typu `ICommand`.

Tvorba příkazů je podrobněji popsána v v článku [ **rozhraní příkazového** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) ale **základní tlačítko příkaz** stránku [  **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) příklad ukazuje základní přístup.

`CommandDemoViewModel` Třída je velmi jednoduchý ViewModel, který definuje vlastnost typu `double` s názvem `Number`a dvě vlastnosti typu `ICommand` s názvem `MultiplyBy2Command` a `DivideBy2Command`:

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    double number = 1;

    public event PropertyChangedEventHandler PropertyChanged;

    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(() => Number *= 2);

        DivideBy2Command = new Command(() => Number /= 2);
    }

    public double Number
    {
        set
        {
            if (number != value)
            {
                number = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Number"));
            }
        }
        get
        {
            return number;
        }
    }

    public ICommand MultiplyBy2Command { private set; get; }

    public ICommand DivideBy2Command { private set; get; }
}
```

Dva `ICommand` vlastnosti jsou inicializovány v konstruktoru třídy s dva objekty typu `Command`. `Command` Konstruktory zahrnují trochu – funkce (volat `execute` argument konstruktoru), zdvojnásobí nebo poloviny `Number` vlastnost.

**BasicButtonCommand.xaml** souboru nastaví jeho `BindingContext` na instanci `CommandDemoViewModel`. `Label` Elementu a dvě `Button` elementy obsahovat vazby na tři vlastnosti v `CommandDemoViewModel`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.BasicButtonCommandPage"
             Title="Basic Button Command">
    
    <ContentPage.BindingContext>
        <local:CommandDemoViewModel />
    </ContentPage.BindingContext>
    
    <StackLayout>
        <Label Text="{Binding Number, StringFormat='Value is now {0}'}"
               FontSize="Large"
               VerticalOptions="CenterAndExpand" 
               HorizontalOptions="Center" />

        <Button Text="Multiply by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding MultiplyBy2Command}" />

        <Button Text="Divide by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding DivideBy2Command}" />
    </StackLayout>
</ContentPage>
```

Jako dvě `Button` stisknuté elementy, jsou-li provést příkazy, a počet změn hodnotu:

[![Příkaz základní tlačítko](button-images/BasicButtonCommand.png "příkaz základní tlačítko")](button-images/BasicButtonCommand-Large.png#lightbox)

Výhodou tohoto přístupu přes `Clicked` obslužné rutiny je, že veškerou logiku zahrnující funkci části této stránky je umístěna v ViewModel spíše než souboru kódu na pozadí, dosažení lepší oddělit uživatelské rozhraní z obchodní logiky.

Je také možné, `Command` objekty, které se řídí povolení a zakázání `Button` elementy. Předpokládejme například, že chcete omezit rozsah číselné hodnoty mezi 2<sup>10</sup> a 2<sup>&ndash;10</sup>. Můžete přidat jinou funkci konstruktoru (volat `canExecute` argument), který vrací `true` Pokud `Button` by měla být povolená. Tady je změny `CommandDemoViewModel` konstruktor:

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    ···
    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(
            execute: () =>
            {
                Number *= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number < Math.Pow(2, 10));

        DivideBy2Command = new Command(
            execute: () =>
            {
                Number /= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number > Math.Pow(2, -10));
    }
    ···
}
```

Volání `ChangeCanExecute` metodu `Command` jsou nezbytné tak, aby `Command` můžete volat metodu `canExecute` metoda a zjistěte, zda `Button` by mělo být zakázáno nebo není. Díky této změně kódu jako číslo dosáhne limitu, `Button` je zakázaná:

[![Příkaz základní tlačítko - Upravit](button-images/BasicButtonCommandModified.png "příkaz základní tlačítko: Upravit")](button-images/BasicButtonCommandModified-Large.png#lightbox)

Je možné pro dva nebo více `Button` elementy bylo vázané na stejné `ICommand` vlastnost. `Button` Elementy lze rozlišit pomocí [ `CommandParameter` ](xref:Xamarin.Forms.Button.CommandParameter) vlastnost `Button`. V takovém případě budete chtít používat obecná [ `Command<T>` ](xref:Xamarin.Forms.Command`1) třídy. `CommandParameter` Objektu je předána jako argument pro `execute` a `canExecute` metody. Tento postup je uveden v podrobně [ **základní tvorba příkazů** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) části [ **rozhraní příkazového** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) článku.

[ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) ukázce se také používá tato technika v jeho `MainPage` třídy. **MainPage.xaml** soubor obsahuje `Button` pro jednotlivé stránky vzorku:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.MainPage"
             Title="Button Demos">
    <ScrollView>
        <FlexLayout Direction="Column"
                    JustifyContent="SpaceEvenly"
                    AlignItems="Center">

            <Button Text="Basic Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonClickPage}" />

            <Button Text="Code Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:CodeButtonClickPage}" />

            <Button Text="Basic Button Command"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonCommandPage}" />

            <Button Text="Press and Release Button"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:PressAndReleaseButtonPage}" />

            <Button Text="Button Appearance"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ButtonAppearancePage}" />

            <Button Text="Toggle Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ToggleButtonDemoPage}" />

            <Button Text="Image Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ImageButtonDemoPage}" />

        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Každý `Button` má jeho `Command` vlastnost vázána na vlastnost s názvem `NavigateCommand`a `CommandParameter` je nastaven na [ `Type` ](xref:System.Type) objekt odpovídající jedné třídy stránky v projektu.

Aby `NavigateCommand` vlastnost je typu `ICommand` a je definována v souboru kódu na pozadí:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Konstruktor inicializuje `NavigateCommand` vlastnosti `Command<Type>` objekt, protože `Type` je typ `CommandParameter` objekt nastavený v souboru XAML. To znamená, že `execute` metoda má argument typu `Type` odpovídající tomuto `CommandParameter` objektu. Funkce vytvoří stránku a poté přejde k němu.

Všimněte si, že dojde k závěru, konstruktoru nastavením jeho `BindingContext` na sebe sama. To je nezbytné pro vlastnosti v souboru XAML pro vazbu `NavigateCommand` vlastnost.

## <a name="pressing-and-releasing-the-button"></a>Stisknutím a uvolněním tlačítka

Kromě `Clicked` událostí, `Button` také definuje [ `Pressed` ](xref:Xamarin.Forms.Button.Pressed) a [ `Released` ](xref:Xamarin.Forms.Button.Released) události. `Pressed` Události dojde, když stiskem tlačítka prstem `Button`, nebo stisknutí tlačítka myši se umístěte ukazatel `Button`. `Released` Při uvolnění tlačítka prstem nebo myš, dojde k události. Obecně platí `Clicked` také vyvolání události ve stejnou dobu jako `Released` událostí, ale pokud je ukazatel prstem nebo myš snímky od povrchu `Button` před uvolněním, `Clicked` nemusí být událostí.

`Pressed` a `Released` často události nevyužívá, ale mohou být použity pro zvláštní účely, jak je předvedeno v **tlačítko verze a stiskněte klávesu** stránky. Obsahuje soubor XAML `Label` a `Button` s obslužnými rutinami pro `Pressed` a `Released` události:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.PressAndReleaseButtonPage"
             Title="Press and Release Button">
    <StackLayout>

        <Label x:Name="label"
               Text="Press and hold the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand" 
               HorizontalOptions="Center" />

        <Button Text="Press to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Pressed="OnButtonPressed"
                Released="OnButtonReleased" />

    </StackLayout>
</ContentPage>
```

Animuje souboru kódu `Label` při `Pressed` události dojde, ale je oběh pozastaví při `Released` dojde k události:

```csharp
public partial class PressAndReleaseButtonPage : ContentPage
{
    bool animationInProgress = false;
    Stopwatch stopwatch = new Stopwatch();

    public PressAndReleaseButtonPage ()
    {
        InitializeComponent ();
    }

    void OnButtonPressed(object sender, EventArgs args)
    {
        stopwatch.Start();
        animationInProgress = true;

        Device.StartTimer(TimeSpan.FromMilliseconds(16), () =>
        {
            label.Rotation = 360 * (stopwatch.Elapsed.TotalSeconds % 1);

            return animationInProgress;
        });
    }

    void OnButtonReleased(object sender, EventArgs args)
    {
        animationInProgress = false;
        stopwatch.Stop();
    }
}
```

Výsledkem je, že `Label` pouze otočí při prstem v kontaktu s `Button`a zastaví, když je vydána prstu:

[![Stisknutím a uvolněním tlačítka](button-images/PressAndReleaseButton.png "stisknutím a uvolněním tlačítka")](button-images/PressAndReleaseButton-Large.png)

Tento druh chování má aplikací pro hry: prstu, které jsou uložené v `Button` může nastavení na obrazovce objektu přesunout v konkrétní směr. 

<a name="button-appearance" />

## <a name="button-appearance"></a>Vzhled tlačítka

`Button` Dědí nebo definuje několik vlastností, které ovlivňují její vzhled:

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) Barva je `Button` textu
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) Barva pozadí na tento text je
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) je barvu oblasti okolního `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) slouží pro text rodiny písem
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) je velikost textu
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) Určuje, zda je výběr kurzívy nebo bold text
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) Šířka ohraničení 
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) Zaokrouhlí rozích

Důsledky šest z těchto vlastností (s výjimkou `FontFamily` a `FontAttributes`) je ukázán v **vzhled tlačítka** stránky. Jinou vlastnost [ `Image` ](xref:Xamarin.Forms.Button.Image), je popsané v části [ **bitmap pomocí tlačítka**](#image-button).

Všechna zobrazení a datové vazby v **vzhled tlačítka** stránky jsou definovány v souboru XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ButtonAppearancePage"
             Title="Button Appearance">
    <StackLayout>
        <Button x:Name="button"
                Text="Button"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                TextColor="{Binding Source={x:Reference textColorPicker},
                                    Path=SelectedItem.Color}"
                BackgroundColor="{Binding Source={x:Reference backgroundColorPicker},
                                          Path=SelectedItem.Color}"
                BorderColor="{Binding Source={x:Reference borderColorPicker},
                                      Path=SelectedItem.Color}" />

        <StackLayout BindingContext="{x:Reference button}"
                     Padding="10">
            
            <Slider x:Name="fontSizeSlider"
                    Maximum="48"
                    Minimum="1"
                    Value="{Binding FontSize}" />

            <Label Text="{Binding Source={x:Reference fontSizeSlider},
                                  Path=Value,
                                  StringFormat='FontSize = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="borderWidthSlider"
                    Minimum="-1"
                    Maximum="12"
                    Value="{Binding BorderWidth}" />
            
            <Label Text="{Binding Source={x:Reference borderWidthSlider}, 
                                  Path=Value,
                                  StringFormat='BorderWidth = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="cornerRadiusSlider"
                    Minimum="-1"
                    Maximum="24"
                    Value="{Binding CornerRadius}" />

            <Label Text="{Binding Source={x:Reference cornerRadiusSlider}, 
                                  Path=Value,
                                  StringFormat='CornerRadius = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.Resources>
                    <Style TargetType="Label">
                        <Setter Property="VerticalOptions" Value="Center" />
                    </Style>
                </Grid.Resources>

                <Label Text="Text Color:"
                       Grid.Row="0" Grid.Column="0" />

                <Picker x:Name="textColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="0" Grid.Column="1" />

                <Label Text="Background Color:"
                       Grid.Row="1" Grid.Column="0" />

                <Picker x:Name="backgroundColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="1" Grid.Column="1" />

                <Label Text="Border Color:"
                       Grid.Row="2" Grid.Column="0" />

                <Picker x:Name="borderColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="2" Grid.Column="1" />
            </Grid>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

`Button` v horní části stránky se jeho tři `Color` vlastnosti vázána na `Picker` prvky v dolní části stránky. Položky v `Picker` prvky jsou barvy z `NamedColor` třída zahrnutý v projektu. Tři `Slider` elementy obsahovat obousměrné vazby na `FontSize`, `BorderWidth`, a `CornerRadius` vlastnosti `Button`.

Tento program můžete experimentovat s kombinací těchto vlastností:

[![Tlačítko vzhled](button-images/ButtonAppearance.png "tlačítko vzhledu")](button-images/ButtonAppearance-Large.png)

Chcete-li zobrazit `Button` ohraničení, budete muset nastavit `BorderColor` něco jiného než `Default`a `BorderWidth` na kladnou hodnotu.

V systému iOS, můžete si všimnout, že velké ohraničení šířek pronikat do vnitřek `Button` a narušovat zobrazení textu. Pokud chcete použít ohraničení s iOS `Button`, budete pravděpodobně chtít začínat a končit `Text` vlastnost s prostory a zachovat jeho viditelnosti.

Na UPW výběru `CornerRadius` který překračuje poloviční výšku `Button` vyvolá výjimku.

## <a name="creating-a-toggle-button"></a>Vytváření přepínacích tlačítek

Je možné podtřídami `Button` , který bude fungovat jako přepínač zapnutí a vypnutí: klepněte na tlačítko jednou přepnutí na tlačítko a klepněte na ji znovu přepnout vypnout.

Následující `ToggleButton` třída odvozená z `Button` a definuje novou událost s názvem `Toggled` a vlastnost typu Boolean s názvem `IsToggled`. Jedná se o stejné dvě vlastnosti definované platformě Xamarin.Forms [ `Switch` ](xref:Xamarin.Forms.Switch):

```csharp
class ToggleButton : Button
{
    public event EventHandler<ToggledEventArgs> Toggled;

    public static BindableProperty IsToggledProperty =
        BindableProperty.Create("IsToggled", typeof(bool), typeof(ToggleButton), false,
                                propertyChanged: OnIsToggledChanged);

    public ToggleButton()
    {
        Clicked += (sender, args) => IsToggled ^= true;
    }

    public bool IsToggled
    {
        set { SetValue(IsToggledProperty, value); }
        get { return (bool)GetValue(IsToggledProperty); }
    }

    protected override void OnParentSet()
    {
        base.OnParentSet();
        VisualStateManager.GoToState(this, "ToggledOff");
    }

    static void OnIsToggledChanged(BindableObject bindable, object oldValue, object newValue)
    {
        ToggleButton toggleButton = (ToggleButton)bindable;
        bool isToggled = (bool)newValue;

        // Fire event
        toggleButton.Toggled?.Invoke(toggleButton, new ToggledEventArgs(isToggled));

        // Set the visual state
        VisualStateManager.GoToState(toggleButton, isToggled ? "ToggledOn" : "ToggledOff");
    }
}
```

`ToggleButton` Konstruktor připojí, aby obslužná rutina `Clicked` událostí, které můžete změnit hodnotu `IsToggled` vlastnost. `OnIsToggledChanged` Metoda aktivuje `Toggled` událostí. 

Poslední řádek `OnIsToggledChanged` metoda volá statických `VisualStateManager.GoToState` metoda s dva textové řetězce "ToggledOn" a "ToggledOff". Můžete si přečíst o tuto metodu a jak vaše aplikace může reagovat na visual stavů v článku [ **nástroje stavu Manager Visual Xamarin.Forms**](~/xamarin-forms/user-interface/visual-state-manager.md). 

Protože `ToggleButton` provede volání `VisualStateManager.GoToState`, vlastní třídy nemusí obsahovat jakákoli další zařízení, chcete-li změnit vzhled tlačítka na základě jeho `IsToggled` stavu. To znamená odpovědnost XAML, který je hostitelem `ToggleButton`. 

**Přepínací tlačítko ukázkový** stránka obsahuje dvě instance `ToggleButton`, včetně Visual správce stavu kód, který nastaví `Text`, `BackgroundColor`, a `TextColor` tlačítka na základě visual stavu: 

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ToggleButtonDemoPage"
             Title="Toggle Button Demo">
    
    <ContentPage.Resources>
        <Style TargetType="local:ToggleButton">
            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            <Setter Property="HorizontalOptions" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <local:ToggleButton Toggled="OnItalicButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Italic Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>
                    
                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Italic On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <local:ToggleButton Toggled="OnBoldButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Bold Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>
                    
                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Bold On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <Label x:Name="label"
               Text="Just a little passage of some sample text that can be formatted in italic or boldface by toggling the two buttons."
               FontSize="Large"
               HorizontalTextAlignment="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

`Toggled` Obslužné rutiny událostí se v souboru kódu na pozadí. Odpovídají pro nastavení `FontAttributes` vlastnost `Label` na základě stavu tlačítka:

```csharp
public partial class ToggleButtonDemoPage : ContentPage
{
    public ToggleButtonDemoPage ()
    {
        InitializeComponent ();
    }

    void OnItalicButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Italic;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Italic;
        }
    }

    void OnBoldButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Bold;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Bold;
        }
    }
}
```

Tady je program spuštěný v iOS, Android a UWP:

[![Přepněte tlačítko ukázkový](button-images/ToggleButtonDemo.png "přepnutí tlačítko Demo")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>Rastrové obrázky pomocí tlačítka

`Button` Třída definuje [ `Image` ](xref:Xamarin.Forms.Button.Image) vlastnost, která umožňuje zobrazit rastrový obrázek na `Button`, samostatně nebo v kombinaci s textem. Můžete také zadat, jak jsou uspořádány textových a obrázkových.

`Image` Vlastnost je typu [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource), což znamená, že rastrových obrázků, které musí být uložen jako prostředky v projektech pro jednotlivé platformy a nikoli v rozhraní .NET standardní projektu knihovny. 

Každou platformu podporovanou nástrojem Xamarin.Forms umožňuje bitové kopie k uložení do více velikostí pro různé pixelů rozlišení různých zařízení, které aplikace může spustit na. Tyto jsou více bitmap s názvem nebo uložené tak, že operační systém, můžete vybrat nejlepší shodu pro zařízení video rozlišení obrazovky. 

Rastrového obrázku na `Button`nejlepší velikost je obvykle mezi 32 až 64 jednotky nezávislé na zařízení, v závislosti na tom, jak velký se má být. Obrázků použitých v tomto příkladu jsou založené na velikost 48 jednotky nezávislé na zařízení.

V projektu iOS **prostředky** složka obsahuje tři velikosti této bitové kopie:

- Odmocnina rastrový obrázek 48 pixelů uložené jako **/Resources/MonkeyFace.png**
- Odmocnina rastrový obrázek 96 pixelů uložené jako **/Resource/MonkeyFace@2x.png**
- Odmocnina rastrový obrázek 144 pixelů uložené jako **/Resource/MonkeyFace@3x.png**

Byly zadány všechny tři rastrové obrázky **akce sestavení** z **BundleResource**.

Pro projekt pro Android, všechny bitmap mají stejný název, ale jsou uložené v různých podsložkách **prostředky** složky:

- Odmocnina rastrový obrázek 72 pixelů uložené jako **/Resources/drawable-hdpi/MonkeyFace.png**
- Odmocnina rastrový obrázek 96 pixelů uložené jako **/Resources/drawable-xhdpi/MonkeyFace.png**
- Odmocnina rastrový obrázek 144 pixelů uložené jako **/Resources/drawable-xxhdpi/MonkeyFace.png**
- Odmocnina rastrový obrázek 192 pixelů uložené jako **/Resources/drawable-xxxhdpi/MonkeyFace.png**

Tyto byly zadány **akce sestavení** z **AndroidResource**.

V projektu UPW bitmap lze uložit kdekoli v projektu, ale jsou obvykle uložena ve vlastní složce nebo **prostředky** existující složku. Projekt UWP obsahuje tyto Bitmap:

- Odmocnina rastrový obrázek 48 pixelů uložené jako **/Assets/MonkeyFace.scale-100.png**
- Odmocnina rastrový obrázek 96 pixelů uložené jako **/Assets/MonkeyFace.scale-200.png**
- Odmocnina rastrový obrázek 192 pixelů uložené jako **/Assets/MonkeyFace.scale-400.png**

Byly všechny, získá **akce sestavení** z **obsahu**.

Můžete zadat jak `Text` a `Image` vlastnosti jsou uspořádány na `Button` pomocí [ `ContentLayout` ](xref:Xamarin.Forms.Button.ContentLayout) vlastnost `Button`. Tato vlastnost je typu [ `ButtonContentLayout` ](xref:Xamarin.Forms.Button.ButtonContentLayout), který je vložený třídy v `Button`. [Konstruktor](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double)) má dva argumenty:

- Člen [ `ImagePosition` ](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) – výčet: `Left`, `Top`, `Right`, nebo `Bottom` určující, jak se zobrazuje bitmapy relativně k text.
- A `double` hodnota mezery mezi bitovou mapu a text.

Výchozí hodnoty jsou `Left` a 10 jednotek. Dvě vlastnosti jen pro čtení z `ButtonContentLayout` s názvem [ `Position` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) a [ `Spacing` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) zadejte hodnoty těchto vlastností.

V kódu, můžete vytvořit `Button` a nastavte `ContentLayout` vlastnost takto:

```csharp
Button button = new Button
{
    Text = "button text",
    Image = new FileImageSource
    {
        File = "image filename"
    },
    ContentLayout = new Button.ButtonContentLayout(Button.ButtonContentLayout.ImagePosition.Right, 20)
};
```

V jazyce XAML je nutné zadat jenom člen výčtu nebo mezery nebo oba seznamy v libovolném pořadí oddělené čárkami:

```xaml
<Button Text="button text"
        Image="image filename"
        ContentLayout="Right, 20" />
```

**Image tlačítko ukázkový** stránka používá `OnPlatform` zadat různé názvy souborů pro iOS, Android a UWP bitmap soubory. Pokud chcete použít stejný název souboru pro všechny tři platformy a nepoužívejte `OnPlatform`, budete potřebovat k ukládání bitmap UWP v kořenovém adresáři projektu.

První `Button` na **Image tlačítko ukázkový** stránky nastaví `Image` vlastnost ale ne `Text` vlastnost:

```xaml
<Button>
    <Button.Image>
        <OnPlatform x:TypeArguments="FileImageSource">
            <On Platform="iOS, Android" Value="MonkeyFace.png" />
            <On Platform="UWP" Value="Assets/MonkeyFace.png" />
        </OnPlatform>
    </Button.Image>
</Button>
```

Pokud bitmap UWP jsou uložené v kořenovém adresáři projektu, může výrazně zjednodušit tento kód:

```xaml
<Button Image="MonkeyFace.png" />
```

Aby se zabránilo spoustu automatizujete značek v **ImageButtonDemo.xaml** souboru implicitní `Style` je také definován nastavit `Image` vlastnost. To `Style` se automaticky použije na pět dalších `Button` elementy. Tady je kompletní souboru XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.ImageButtonDemoPage">

    <FlexLayout Direction="Column"
                JustifyContent="SpaceEvenly"
                AlignItems="Center">
        
        <FlexLayout.Resources>
            <Style TargetType="Button">
                <Setter Property="Image">
                    <OnPlatform x:TypeArguments="FileImageSource">
                        <On Platform="iOS, Android" Value="MonkeyFace.png" />
                        <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                    </OnPlatform>
                </Setter>
            </Style>
        </FlexLayout.Resources>

        <Button>
            <Button.Image>
                <OnPlatform x:TypeArguments="FileImageSource">
                    <On Platform="iOS, Android" Value="MonkeyFace.png" />
                    <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                </OnPlatform>
            </Button.Image>
        </Button>

        <Button Text="Default" />

        <Button Text="Left - 10"
                ContentLayout="Left, 10" />

        <Button Text="Top - 10"
                ContentLayout="Top, 10" />

        <Button Text="Right - 20"
                ContentLayout="Right, 20" />

        <Button Text="Bottom - 20" 
                ContentLayout="Bottom, 20" />
    </FlexLayout>
</ContentPage>
```

Poslední čtyři `Button` elementy Zkontrolujte použití `ContentLayout` vlastnosti k určení pozice a mezery mezi text a rastrový obrázek:

[![Obrázek ukázkové tlačítko](button-images/ImageButtonDemo.png "obrázku tlačítka Demo")](button-images/ImageButtonDemo-Large.png#lightbox)

Nyní jste viděli různé způsoby, které může zpracovat `Button` události a změnit `Button` vzhled.

## <a name="related-links"></a>Související odkazy

- [Ukázka ButtonDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)
- [Tlačítko rozhraní API](xref:Xamarin.Forms.Button)
