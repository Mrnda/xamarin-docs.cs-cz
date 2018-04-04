---
title: Využívání XAML – rozšíření značek
description: Použití značek rozšíření XAML, které jsou k dispozici v Xamarin.Forms
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: 25eada483e8bd2ce95cb3101dfe873ea38b283ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-xaml-markup-extensions"></a>Využívání XAML – rozšíření značek

XAML – rozšíření značek zlepšení výkonu a flexibilitě XAML tím, že atributy prvků nastavit z různých zdrojů. Několik rozšíření značek XAML jsou součástí specifikace jazyka XAML 2009. Zobrazují se v XAML soubory s obvyklé `x` předponu oboru názvů a jsou obvykle označuje s touto předponou. Tyto možnosti jsou popsány v následujících částech:

- [`x:Static`](#static) &ndash; odkaz na statické vlastnosti, pole nebo členové výčtu.
- [`x:Reference`](#reference) &ndash; referenční dokumentace elementů na stránce s názvem.
- [`x:Type`](#type) &ndash; Nastavte atribut na `System.Type` objektu.
- [`x:Array`](#array) &ndash; Vytvořte pole objektů, určitého typu.
- [`x:Null`](#null) &ndash; Nastavte atribut na `null` hodnotu.

Tři další rozšíření značek XAML, mají v minulosti podporuje jiných implementacích XAML a také podporuje Xamarin.Forms. Tyto možnosti jsou popsány podrobněji v jiných článcích:

- `StaticResource` &ndash; odkazování objektů z slovník prostředků, jak je popsáno v článku [ **slovnících prostředků**](~/xamarin-forms/xaml/resource-dictionaries.md).
- `DynamicResource` &ndash; odpověď na změny v objektech v slovník prostředků, jak je popsáno v článku [ **dynamické styly**](~/xamarin-forms/user-interface/styles/dynamic.md).
- `Binding` &ndash; Vytvoření vazby mezi vlastnosti dva objekty, jak je popsáno v článku [ **datové vazby**](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- `TemplateBinding` &ndash; provádí datová vazba ze šablony ovládacího prvku, jak je popsáno v článku [**vazby ze šablony řízení**] / příručky nebo xamarin-forms nebo aplikace – základy nebo šablony nebo ovládací prvek šablony nebo šablony vazba nebo)

[ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) Rozložení využívá rozšíření vlastních značek [ `ConstraintExpression` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/). Toto rozšíření značek je popsána v článku [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md).

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static – rozšíření značek

`x:Static` Podporuje – rozšíření značek [ `StaticExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) třídy. Třída má jednu vlastnost s názvem [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) typu `string` nastavit na název veřejné konstanta, statické vlastnosti, statické pole nebo člen výčtu.

Běžný způsob použití `x:Static` je nejdříve definovat třídu s některé konstanty nebo statické proměnné, jako například tomto jen nepatrnou `AppConstants` třídy v [ **MarkupExtensions** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/) program:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**X: Static ukázku** stránky ukazuje několik způsobů, jak používat `x:Static` – rozšíření značek. Vytvoří instanci nejpodrobnější přístup `StaticExtension` třídy mezi `Label.FontSize` element vlastnosti značky:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.StaticDemoPage"
             Title="x:Static Demo">
    <StackLayout Margin="10, 0">
        <Label Text="Label No. 1">
            <Label.FontSize>
                <x:StaticExtension Member="local:AppConstants.NormalFontSize" />
            </Label.FontSize>
        </Label>

        ···

    </StackLayout>
</ContentPage>
```

Analyzátor jazyka XAML také umožňuje `StaticExtension` třídy zkrátit na `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

To může ještě víc zjednodušit, ale změnu zavádí některé nové syntaxe: skládá se z umístění `StaticExtension` třídy a člen nastavení do složených závorek. Výsledný výraz nastavena přímo na `FontSize` atribut:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Všimněte si, že existují *žádné* uvozovek do složených závorek. `Member` Vlastnost `StaticExtension` již není atribut XML. Místo toho je součástí výraz pro rozšíření značek.

Stejně jako parametr lze zapsat `x:StaticExtension` k `x:Static` při použití jako element objekt, můžete také zkrátit ji ve výrazu v rámci složené závorky:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension` Třída má `ContentProperty` atribut odkazování na vlastnost `Member`, který označuje tato vlastnost jako vlastnost obsahu výchozí třídy. Pro rozšíření značek pro jazyk XAML vyjádřit s složené závorky, můžete eliminovat `Member=` část výrazu:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Toto je nejběžnější formu `x:Static` – rozšíření značek.

**Statické ukázku** stránka obsahuje dva další příklady. Kořenovou značku souboru XAML obsahuje deklarace oboru názvů XML pro .NET `System` obor názvů:

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

To umožňuje `Label` velikost písma být nastavena na statické pole `Math.PI`. Který výsledkem místo malé textové proto `Scale` je nastavena na `Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

V posledním příkladu se zobrazí `Device.RuntimePlatform` hodnotu. `Environment.NewLine` Statickou vlastnost slouží k vložení znak nového řádku mezi těmito dvěma `Span` objekty:

```xaml
<Label HorizontalTextAlignment="Center"
       FontSize="{x:Static local:AppConstants.NormalFontSize}">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Runtime Platform: " />
            <Span Text="{x:Static sys:Environment.NewLine}" />
            <Span Text="{x:Static Device.RuntimePlatform}" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Zde je ukázka běžící na všechny tři platformách:

[![x:Static Demo](consuming-images/staticdemo-small.png "x:Static Demo")](consuming-images/staticdemo-large.png#lightbox "x:Static Demo")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference – rozšíření značek

`x:Reference` Podporuje – rozšíření značek [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) třídy. Třída má jednu vlastnost s názvem [ `Name` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.ReferenceExtension.Name/) typu `string` nastavit na název elementu na stránce, která byla poskytnuta název s `x:Name`. To `Name` vlastnost je vlastnost obsahu `ReferenceExtension`, takže `Name=` není nutné, když `x:Reference` se zobrazí v složené závorky.

`x:Reference` – Rozšíření značek se používá výhradně pomocí vazby dat, které jsou popsány podrobněji v článku [ **datové vazby**](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**X: Reference ukázku** stránky zobrazí dvě použití `x:Reference` s vazby dat, první, kde se používá k nastavení `Source` vlastnost `Binding` objekt a druhá, kde se používá k nastavení `BindingContext` Vlastnost pro dvě datové vazby:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ReferenceDemoPage"
             x:Name="page"
             Title="x:Reference Demo">

    <StackLayout Margin="10, 0">

        <Label Text="{Binding Source={x:Reference page},
                              StringFormat='The type of this page is {0}'}"
               FontSize="18"
               VerticalOptions="CenterAndExpand"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="Center" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='{0:F0}&#x00B0; rotation'}"
               Rotation="{Binding Value}"
               FontSize="24"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

Obě `x:Reference` výrazy použít zkrácenou verzi `ReferenceExtension` název třídy a eliminovat `Name=` část výrazu. V prvním příkladu `x:Reference` součástí – rozšíření značek `Binding` – rozšíření značek. Všimněte si, že `Source` a `StringFormat` nastavení jsou odděleny čárkami. Tady je programy spuštěné na všech tří platformách:

[![x: Reference ukázku](consuming-images/referencedemo-small.png "ukázku x: Reference")](consuming-images/referencedemo-large.png#lightbox "x: Reference Demo")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type – rozšíření značek

`x:Type` – Rozšíření značek je ekvivalentem XAML jazyka C# [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) – klíčové slovo. Je podporována [ `TypeExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) třídy, která definuje jednu vlastnost s názvem [ `TypeName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.TypeExtension.TypeName/) typu `string` , je nastavena na název třídu nebo strukturu. `x:Type` Vrátí rozšíření značek [ `System.Type` ](https://developer.xamarin.com/api/type/System.Type/) objekt tuto třídu nebo strukturu. `TypeName` Vlastnost obsahu je `TypeExtension`, takže `TypeName=` není nutné, když `x:Type` se zobrazí s složené závorky.

V rámci Xamarin.Forms, existuje několik vlastností, které mají argumenty typu `Type`. Mezi příklady patří [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) vlastnost `Style`a [x: TypeArguments –](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) atribut používaný k zadání argumentů v obecné třídy. Ale analyzátor XAML provádí `typeof` operace automaticky a `x:Type` – rozšíření značek v těchto případech nepoužívá.

Jednom místě kde `x:Type` *je* požadované je `x:Array` – rozšíření značek, který je popsán v [další části](#array).

`x:Type` – Rozšíření značek je také užitečné při vytváření nabídky kde každá položka nabídky odpovídá objekt určitého typu. Můžete přidružit `Type` objektu s každou položku nabídky a potom vytvořte instanci objektu, pokud je vybraná položka nabídky.

Jedná se jak v navigační nabídce `MainPage` v **rozšíření značek** programu funguje. **MainPage.xaml** soubor obsahuje `TableView` s jednotlivými `TextCell` odpovídající na konkrétní stránku v programu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.MainPage"
             Title="Markup Extensions"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection>
                <TextCell Text="x:Static Demo"
                          Detail="Access constants or statics"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:StaticDemoPage}" />

                <TextCell Text="x:Reference Demo"
                          Detail="Reference named elements on the page"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ReferenceDemoPage}" />

                <TextCell Text="x:Type Demo"
                          Detail="Associate a Button with a Type"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:TypeDemoPage}" />

                <TextCell Text="x:Array Demo"
                          Detail="Use an array to fill a ListView"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ArrayDemoPage}" />

                ···                          

        </TableRoot>
    </TableView>
</ContentPage>
```

Zde je hlavní stránce otevírání v **rozšíření značek**:

[![Hlavní stránka](consuming-images/mainpage-small.png "hlavní stránky")](consuming-images/mainpage-large.png#lightbox "hlavní stránky")

Každý `CommandParameter` je nastavena na `x:Type` rozšíření značek, který odkazuje na jednu z dalších stránek. `Command` Vlastnost je vázána na vlastnost s názvem `NavigateCommand`. Tato vlastnost je definována v `MainPage` souboru kódu na pozadí:

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

`NavigateCommand` Vlastnost je `Command` objekt, který implementuje příkaz execute s parametrem typu `Type` &mdash; hodnotu `CommandParameter`. Používá metodu `Activator.CreateInstance` k vytváření instancí stránku a poté přejde k němu. Konstruktor se ukončí nastavení `BindingContext` stránky na sebe, což umožňuje `Binding` na `Command` pracovat. Najdete v článku [ **datové vazby** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) článku a zvláště [ **Commanding** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) článku Další podrobnosti o tomto typu kódu.

**X: Type ukázku** stránka používá podobný postup k vytvoření instance Xamarin.Forms elementy a pro jejich přidávání k `StackLayout`. Souboru XAML původně se skládá ze tří `Button` elementy s jejich `Command` vlastnosti nastavené `Binding` a `CommandParameter` vlastnosti nastavena na typy tři Xamarin.Forms zobrazení:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.TypeDemoPage"
             Title="x:Type Demo">

    <StackLayout x:Name="stackLayout"
                 Padding="10, 0">

        <Button Text="Create a Slider"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Slider}" />

        <Button Text="Create a Stepper"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Stepper}" />

        <Button Text="Create a Switch"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Switch}" />
    </StackLayout>
</ContentPage>
```

Definuje a inicializuje v souboru kódu `CreateCommand` vlastnost:

```csharp
public partial class TypeDemoPage : ContentPage
{
    public TypeDemoPage()
    {
        InitializeComponent();

        CreateCommand = new Command<Type>((Type viewType) =>
        {
            View view = (View)Activator.CreateInstance(viewType);
            view.VerticalOptions = LayoutOptions.CenterAndExpand;
            stackLayout.Children.Add(view);
        });

        BindingContext = this;
    }

    public ICommand CreateCommand { private set; get; }
}
```

Metoda, která je spuštěna při `Button` stisknutí vytvoří novou instanci třídy argument, nastaví její `VerticalOptions` vlastnost a přidává ji k `StackLayout`. Tří `Button` stránku elementy potom sdílet s dynamicky vytvořené zobrazení:

[![x: Type ukázku](consuming-images/typedemo-small.png "ukázku x: Type")](consuming-images/typedemo-large.png#lightbox "x: Type Demo")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array – rozšíření značek

`x:Array` – Rozšíření značek umožňuje definovat pole v kódu. Je podporována [ `ArrayExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) třídy, která definuje dvě vlastnosti:

- `Type` typu `Type`, který určuje typ elementů v poli.
- `Items` typu `IList`, což je kolekce položek, sami. Toto je vlastnost obsahu `ArrayExtension`.

`x:Array` – Rozšíření značek samotné nikdy se zobrazí v složené závorky. Místo toho `x:Array` počáteční a koncové značky vymezení seznam položek. Nastavte `Type` vlastnost, která má `x:Type` – rozšíření značek.

**X: Array ukázku** stránky ukazuje způsob použití `x:Array` k přidávání položek do `ListView` nastavením `ItemsSource` vlastnost, která má pole:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ArrayDemoPage"
             Title="x:Array Demo Page">
    <ListView Margin="10">
        <ListView.ItemsSource>
            <x:Array Type="{x:Type Color}">
                <Color>Aqua</Color>
                <Color>Black</Color>
                <Color>Blue</Color>
                <Color>Fuchsia</Color>
                <Color>Gray</Color>
                <Color>Green</Color>
                <Color>Lime</Color>
                <Color>Maroon</Color>
                <Color>Navy</Color>
                <Color>Olive</Color>
                <Color>Pink</Color>
                <Color>Purple</Color>
                <Color>Red</Color>
                <Color>Silver</Color>
                <Color>Teal</Color>
                <Color>White</Color>
                <Color>Yellow</Color>
            </x:Array>
        </ListView.ItemsSource>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <BoxView Color="{Binding}"
                             Margin="3" />    
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>        
```

`ViewCell` Vytvoří jednoduchou `BoxView` pro každou položku barev:

[![x: Array ukázku](consuming-images/arraydemo-small.png "ukázku x: Array")](consuming-images/arraydemo-large.png#lightbox "x: Array Demo")

Existuje několik způsobů určení jednotlivých `Color` položky v toto pole. Můžete použít `x:Static` – rozšíření značek:

```xaml
<x:Static Member="Color.Blue" />
```

Nebo můžete použít `StaticResource` načíst barvy z slovník prostředků:

```xaml
<StaticResource Key="myColor" />
```

Na konci tohoto článku uvidíte vlastní rozšíření značek XAML, který také vytvoří novou hodnotu barev:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

Při definování pole běžné typy jako řetězce nebo čísla, používat značky uvedené v [ **předávání argumenty konstruktoru** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) článku omezíte hodnoty.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null – rozšíření značek

`x:Null` Podporuje – rozšíření značek [ `NullExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) třídy. Nemá žádné vlastnosti a jednoduše ekvivalentní XAML jazyka C# [ `null` ](/dotnet/csharp/language-reference/keywords/null/) – klíčové slovo.

`x:Null` – Rozšíření značek je nutná pouze zřídka a málokdy použít, ale pokud potřeba ji najít, budete rádi, existuje.

**X: Null ukázku** stránky znázorňuje jeden scénář při `x:Null` může být vhodné. Předpokládejme, že definujete implicitní `Style` pro `Label` , který obsahuje `Setter` , který nastavuje `FontFamily` vlastnost s názvem rodiny závislé na platformě:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.NullDemoPage"
             Title="x:Null Demo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="48" />
                <Setter Property="FontFamily">
                    <Setter.Value>
                        <OnPlatform x:TypeArguments="x:String">
                            <On Platform="iOS" Value="Times New Roman" />
                            <On Platform="Android" Value="serif" />
                            <On Platform="UWP" Value="Times New Roman" />
                        </OnPlatform>
                    </Setter.Value>
                </Setter>
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.Content>
        <StackLayout Padding="10, 0">
            <Label Text="Text 1" />
            <Label Text="Text 2" />

            <Label Text="Text 3"
                   FontFamily="{x:Null}" />

            <Label Text="Text 4" />
            <Label Text="Text 5" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>   
```

Pak můžete zjistíte, že pro jeden z `Label` elementy, chcete, aby všechny hodnoty vlastností v implicitní `Style` s výjimkou `FontFamily`, které chcete nastavit jako výchozí hodnota. Můžete třeba definovat jiné `Style` pro tento účel, ale jednodušší je jednoduše nastavit `FontFamily` vlastnost konkrétní `Label` k `x:Null`, jak je ukázáno v centru `Label`.

Tady je program běžící na třech platformách:

[![x: Null ukázku](consuming-images/nulldemo-small.png "ukázku x: Null")](consuming-images/nulldemo-large.png#lightbox "x: Null Demo")

Všimněte si, že čtyři z `Label` prvky mít serif písmo, ale centru `Label` má výchozí sans-serif písmo.

## <a name="define-your-own-markup-extensions"></a>Definovat vlastní rozšíření značek

Pokud byla zjištěna potřebu rozšíření značek XAML, která není k dispozici v Xamarin.Forms, můžete [vytvořit vlastní](creating.md).


## <a name="related-links"></a>Související odkazy

- [Rozšíření značek (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Kapitola rozšíření značek XAML z adresáře Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Slovníky prostředků](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamické styly](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Datová vazba](~/xamarin-forms/app-fundamentals/data-binding/index.md)
