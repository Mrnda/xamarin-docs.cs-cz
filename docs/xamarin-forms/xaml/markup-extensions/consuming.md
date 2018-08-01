---
title: Používání rozšíření značek XAML
description: Tento článek vysvětluje, jak použít rozšíření značek XAML Xamarin.Forms vylepšit výkon a flexibilitu XAML tím, že element atributů, které mají být v rozsahu od široké škály zdrojů.
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: a630d7c2acb95b7551c9f5f870078a0efcfc075c
ms.sourcegitcommit: ecdc031e9e26bbbf9572885531ee1f2e623203f5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393669"
---
# <a name="consuming-xaml-markup-extensions"></a>Používání rozšíření značek XAML

Rozšíření značek XAML pomáhají zlepšit výkon a flexibilitu XAML tím, že element atributů, které mají být v rozsahu od široké škály zdrojů. Několik rozšíření značek XAML jsou součástí specifikace XAML 2009. Zobrazují se v souborech XAML s obvyklý `x` předponu oboru názvů a jsou obvykle označuje s touto předponou. Tyto možnosti jsou popsány v následujících částech:

- [`x:Static`](#static) &ndash; odkaz na statické vlastnosti, pole nebo členy výčtu.
- [`x:Reference`](#reference) &ndash; odkaz na pojmenované elementy na stránce.
- [`x:Type`](#type) &ndash; Nastavte atribut na `System.Type` objektu.
- [`x:Array`](#array) &ndash; Vytvoření pole objektů určitého typu.
- [`x:Null`](#null) &ndash; Nastavte atribut na `null` hodnotu.

Další rozšíření značek XAML v minulosti se nepodporuje v jiných implementacích XAML a jsou také podporovány Xamarin.Forms. Tyto možnosti jsou popsány podrobněji v jiných článcích:

- `StaticResource` &ndash; odkaz na objekty ze slovníku prostředků, jak je popsáno v článku [ **zdrojových slovnících**](~/xamarin-forms/xaml/resource-dictionaries.md).
- `DynamicResource` &ndash; reakce na změny v objektech ve slovníku prostředků, jak je popsáno v článku [ **dynamické styly**](~/xamarin-forms/user-interface/styles/dynamic.md).
- `Binding` &ndash; vytvořit odkaz mezi vlastnostmi dva objekty, jak je popsáno v článku [ **datové vazby**](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- `TemplateBinding` &ndash; provádí datové vazby v šabloně ovládacího prvku, jak je popsáno v článku [ **vazby ze šablony ovládacího prvku**](/guides/xamarin-forms/application-fundamentals/templates/control-templates/template-binding/).

[ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) Rozložení používá rozšíření vlastních značek [ `ConstraintExpression` ](xref:Xamarin.Forms.ConstraintExpression). Toto rozšíření značek je popsaný v článku [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md).

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static – rozšíření značek

`x:Static` – Rozšíření značek je podporován [ `StaticExtension` ](xref:Xamarin.Forms.Xaml.StaticExtension) třídy. Třída má jednu vlastnost s názvem [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member) typu `string` nastavit na název veřejné konstanty, statické vlastnosti, statické pole nebo člena výčtu.

Jedním z běžných způsobů použití `x:Static` , je nejprve definovat třídu s konstanty nebo statických proměnných, například tomto malý `AppConstants` třídy v [ **MarkupExtension** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/) program:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**X: Static ukázka** ukazuje několik způsobů, jak používat stránky `x:Static` – rozšíření značek. Vytvoří instanci nejpodrobnější přístup `StaticExtension` třídy mezi `Label.FontSize` element vlastnosti značky:

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

Také umožňuje analyzátoru XAML `StaticExtension` třídy zkracuje jako `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

To se může ještě více zjednodušit, ale tato změna přináší některé nové syntaxe: skládá se z umístění `StaticExtension` tříd a členů nastavení ve složených závorkách. Výsledný výraz nastavena přímo `FontSize` atribut:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Všimněte si, že existují *žádné* uvozovky uvnitř složených závorek. `Member` Vlastnost `StaticExtension` už není atribut XML. Místo toho je součástí výrazu pro rozšíření značek.

Stejně jako lze zkrátit `x:StaticExtension` k `x:Static` při použití jako element objektu, můžete taky zkrátit ho ve výrazu ve složených závorkách:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension` Třída nemá `ContentProperty` atribut odkazující na vlastnost `Member`, která označí tuto vlastnost jako vlastnost obsahu výchozí třídy. Pro rozšíření značek XAML vyjádřit pomocí složených závorek, můžete eliminovat `Member=` součástí výrazu:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Toto je nejběžnější forma `x:Static` – rozšíření značek.

**Statické ukázka** stránka obsahuje dvě další příklady. Kořenová značka souboru XAML obsahuje deklarace oboru názvů XML pro .NET `System` obor názvů:

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Díky tomu `Label` velikost písma nastavili na statické pole `Math.PI`. Který má za následek ale malé, takže `Scale` je nastavena na `Math.E`:

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

Toto je ukázka spuštěná na všech třech platformách:

[![x: Static ukázku](consuming-images/staticdemo-small.png "ukázku x: Static")](consuming-images/staticdemo-large.png#lightbox "x: Static – ukázka")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference – rozšíření značek

`x:Reference` – Rozšíření značek je podporován [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension) třídy. Třída má jednu vlastnost s názvem [ `Name` ](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) typu `string` nastavit na název elementu na stránce, která se předala název s `x:Name`. To `Name` vlastností je vlastnost content `ReferenceExtension`, takže `Name=` není potřeba při `x:Reference` se zobrazí ve složených závorkách.

`x:Reference` – Rozšíření značek se používá výhradně pomocí vazby dat, které jsou popsány podrobně v článku [ **datové vazby**](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**X: Reference ukázka** stránce se zobrazují dva způsoby použití `x:Reference` s datové vazby, první, kde se používá k nastavení `Source` vlastnost `Binding` objekt a druhá, kdy se používá k nastavení `BindingContext` Vlastnost pro dva datové vazby:

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

Obě `x:Reference` výrazy používají zkrácenou verzi `ReferenceExtension` název třídy a eliminovat `Name=` součástí výrazu. V prvním příkladu `x:Reference` – rozšíření značek se vloží do `Binding` – rozšíření značek. Všimněte si, `Source` a `StringFormat` nastavení jsou odděleny čárkami. Tady je program spuštěn na všech třech platformách:

[![x: Reference ukázka](consuming-images/referencedemo-small.png "x: Reference ukázka")](consuming-images/referencedemo-large.png#lightbox "ukázku x: Reference")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type – rozšíření značek

`x:Type` – Rozšíření značek jsou obdobou XAML jazyka C# [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) – klíčové slovo. Je podporován [ `TypeExtension` ](xref:Xamarin.Forms.Xaml.TypeExtension) třídu, která definuje jednu vlastnost s názvem [ `TypeName` ](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) typu `string` , která je nastavena na název třídy nebo struktury. `x:Type` Vrátí rozšíření značek [ `System.Type` ](xref:System.Type) objekt této třídy nebo struktury. `TypeName` je vlastnost content `TypeExtension`, takže `TypeName=` není potřeba při `x:Type` se zobrazí pomocí složených závorek.

V rámci Xamarin.Forms, existuje několik vlastností, které mít argumenty typu `Type`. Mezi příklady patří [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) vlastnost `Style`a [x: TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) atribut používá k určení argumentů v obecné třídy. Ale analyzátoru XAML provádí `typeof` operace automaticky a `x:Type` – rozšíření značek se nepoužívá v těchto případech.

Jedno místo kde `x:Type` *je* vyžaduje se `x:Array` – rozšíření značek, která je popsána v [další části](#array).

`x:Type` – Rozšíření značek je také užitečné při vytváření nabídky, kde každá položka nabídky odpovídá objektu určitého typu. Můžete přiřadit `Type` objekt s každou položku nabídky a potom vytvořte instanci objektu, pokud je vybrána položka nabídky.

To je jak v navigační nabídce `MainPage` v **– rozšíření značek** program funguje. **MainPage.xaml** obsahuje soubor `TableView` s každým `TextCell` odpovídající na určitou stránku v aplikaci:

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

Tady je hlavní stránky otevírání **– rozšíření značek**:

[![Hlavní stránka](consuming-images/mainpage-small.png "hlavní stránky")](consuming-images/mainpage-large.png#lightbox "hlavní stránky")

Každý `CommandParameter` je nastavena na `x:Type` – rozšíření značek, který odkazuje na jeden z ostatních stránek. `Command` Vlastnost je vázána na vlastnost s názvem `NavigateCommand`. Tato vlastnost je definována v `MainPage` soubor kódu na pozadí:

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

`NavigateCommand` Je vlastnost `Command` objekt, který implementuje příkaz execute se argument typu `Type` &mdash; hodnotu `CommandParameter`. Metoda používá `Activator.CreateInstance` pro vytvoření instance stránky a pak přejde k němu. Konstruktor končí tím, že nastavíte `BindingContext` stránky sám na sebe, což umožňuje `Binding` na `Command` pracovat. Najdete v článku [ **datové vazby** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) článku a zejména [ **Commanding** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) článku najdete další podrobnosti o tomto typu kódu.

**X: Type ukázka** stránka používá podobné techniky k vytvoření instance Xamarin.Forms prvky a jejich přidání do `StackLayout`. Soubor XAML původně se skládá ze tří `Button` prvky s jejich `Command` nastaveny `Binding` a `CommandParameter` vlastnosti nastavené na typech tři zobrazení Xamarin.Forms:

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

Použití modelu code-behind soubor definuje a inicializuje `CreateCommand` vlastnost:

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

Metoda, která je spuštěna při `Button` stisknutí vytvoří novou instanci argumentu, nastaví její `VerticalOptions` vlastnost a přidá jej do `StackLayout`. Tři `Button` elementy pak sdílet na stránce s dynamicky generovaný zobrazení:

[![x: Type ukázka](consuming-images/typedemo-small.png "x: Type ukázka")](consuming-images/typedemo-large.png#lightbox "x: Type ukázku")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array – rozšíření značek

`x:Array` – Rozšíření značek umožňuje definovat pole v kódu. Je podporován [ `ArrayExtension` ](xref:Xamarin.Forms.Xaml.ArrayExtension) třídu, která definuje dvě vlastnosti:

- `Type` typ `Type`, který určuje typ prvků v poli.
- `Items` typ `IList`, což je kolekce položek, sami. Toto je vlastnost content `ArrayExtension`.

`x:Array` – Rozšíření značek samotné nikdy se zobrazí ve složených závorkách. Místo toho `x:Array` počáteční a koncovou značku vymezení seznam položek. Nastavte `Type` vlastnost `x:Type` – rozšíření značek.

**X: Array ukázka** stránce ukazuje způsob použití `x:Array` položky, které chcete přidat `ListView` nastavením `ItemsSource` vlastnost pole:

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

`ViewCell` Vytvoří jednoduchý `BoxView` pro každou položku barvy:

[![x: Array ukázka](consuming-images/arraydemo-small.png "x: Array ukázka")](consuming-images/arraydemo-large.png#lightbox "x: Array Demo")

Existuje několik způsobů, jak určit jednotlivých `Color` položky v tomto poli. Můžete použít `x:Static` – rozšíření značek:

```xaml
<x:Static Member="Color.Blue" />
```

Nebo můžete použít `StaticResource` načíst barvu ze slovníku prostředků:

```xaml
<StaticResource Key="myColor" />
```

Na konci tohoto článku uvidíte vlastní rozšíření značek XAML, který vytvoří také novou hodnotu barvy:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

Při definování polí běžných typů, jako jsou řetězce nebo čísla, použití značek uvedené v [ **předávání argumentů konstruktoru** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) článku pro vymezení hodnoty.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null – rozšíření značek

`x:Null` – Rozšíření značek je podporován [ `NullExtension` ](xref:Xamarin.Forms.Xaml.NullExtension) třídy. Nemá žádné vlastnosti a odpovídá jednoduše XAML jazyka C# [ `null` ](/dotnet/csharp/language-reference/keywords/null/) – klíčové slovo.

`x:Null` Je málokdy potřeba a zřídka používají – rozšíření značek, ale pokud potřebujete ji najít, budete mít rádi, který existuje.

**X: Null ukázka** jeden scénář znázorňuje stránky při `x:Null` může být užitečné. Předpokládejme, že definujete implicitní `Style` pro `Label` , který obsahuje `Setter` , který nastavuje `FontFamily` vlastnost s názvem rodiny závislého na platformě:

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

Pak zjistíte, že pro jednu z `Label` prvky, mají nastavení vlastnosti v implicitní `Style` s výjimkou `FontFamily`, kterou chcete nastavit jako výchozí hodnota. Můžete například definovat další `Style` pro tento účel, ale jednodušší přístup je jednoduše nastavit `FontFamily` vlastnost konkrétní `Label` k `x:Null`, jak je ukázáno v centru `Label`.

Tady je program běžící na třech platformách:

[![x: Null ukázka](consuming-images/nulldemo-small.png "x: Null ukázka")](consuming-images/nulldemo-large.png#lightbox "ukázku x: Null")

Všimněte si, že čtyři z `Label` elementy mají serif písmo, ale centru `Label` má výchozí písmo sans-serif.

## <a name="define-your-own-markup-extensions"></a>Definovat vlastní rozšíření značek

Pokud jste se setkali potřebu rozšíření značek XAML, který není k dispozici v Xamarin.Forms, můžete si [vytvořit svoje vlastní](creating.md).


## <a name="related-links"></a>Související odkazy

- [Rozšíření značek (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Kapitola rozšíření značek XAML Xamarin.Forms knihy](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Slovníky prostředků](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamické styly](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Datová vazba](~/xamarin-forms/app-fundamentals/data-binding/index.md)
