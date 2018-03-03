---
title: "Část 2. Syntaxe nezbytné XAML"
description: "XAML je většinou určený k vytváření instancí a inicializace objektů. Ale často, vlastnosti musí být nastavené na komplexní objekty, které nelze snadno reprezentovat jako řetězce XML a někdy vlastnosti definované třídou jeden musí být nastavena na podřízené třídy. Tyto dvě potřeby vyžadují základní funkce syntaxe XAML vlastnost elementů a přidružené vlastnosti."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: f99d4b177f5957b2e5f8c22171fe92799af8505a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="part-2-essential-xaml-syntax"></a>Část 2. Syntaxe nezbytné XAML

_XAML je většinou určený k vytváření instancí a inicializace objektů. Ale často, vlastnosti musí být nastavené na komplexní objekty, které nelze snadno reprezentovat jako řetězce XML a někdy vlastnosti definované třídou jeden musí být nastavena na podřízené třídy. Tyto dvě potřeby vyžadují základní funkce syntaxe XAML vlastnost elementů a přidružené vlastnosti._

## <a name="property-elements"></a>Vlastnosti elementů

V jazyce XAML jsou obvykle jako atributy XML nastavit vlastnosti třídy:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

Je však alternativní způsob nastavení vlastnosti v jazyce XAML. Můžete vyzkoušet tento alternativní s `TextColor`, nejprve odstranit stávající `TextColor` nastavení:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

Otevřete prázdný element `Label` značky oddělením do počáteční a koncové značky:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

V rámci těchto značek přidejte počáteční a koncové značky, které se skládají z názvu třídy a odděleny tečkou název vlastnosti:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

Nastavte hodnotu vlastnosti jako obsah tyto nové značky, například takto:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

Tyto dva způsoby, jak zadat `TextColor` vlastnost jsou funkčně rovnocenné, ale nepoužívají dva způsoby pro stejnou vlastnost, protože, který by efektivně se nastavení vlastnosti dvakrát a může být nejednoznačný.

Pomocí této nové syntaxe můžete zavedl některé užitečné terminologie:

-  `Label` je *object element*. Jedná se o Xamarin.Forms objekt vyjádřený jako XML element.
-  `Text`, `VerticalOptions`, `FontAttributes` a `FontSize` jsou *vlastnost atributy*. Jsou vlastnosti Xamarin.Forms vyjádřený jako atributy XML.
-  V tomto fragmentu kódu konečné `TextColor` se stal *element vlastnosti*. Jde o vlastnost Xamarin.Forms, ale nyní je XML element.


Definici vlastnosti, které může elementy v nejprve se zdá, že se porušení syntaxe jazyka XML, ale není. Období nemá žádný speciální význam v kódu XML. K dekodér XML `Label.TextColor` je jednoduše normální podřízený element.

V jazyce XAML, ale tato syntaxe je velmi speciální. Jedním z pravidel pro vlastnost elementy je nic jiného může vyskytovat v `Label.TextColor` značky. Hodnota vlastnosti je vždy definována jako obsah mezi element vlastnosti počáteční a koncové značky.

Vlastnost element syntaxe můžete použít na více než jednu vlastnost:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center">
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

Nebo můžete použít element vlastnosti syntaxe pro všechny vlastnosti:

```xaml
<Label>
    <Label.Text>
        Hello, XAML!
    </Label.Text>
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
    <Label.VerticalOptions>
        Center
    </Label.VerticalOptions>
</Label>
```

Na první pohled syntaxe element vlastnosti jevily jako nepotřebné long-winded nahrazení pro něco poměrně úplně jednoduché a v těchto příkladech je to určitě tento případ.

Vlastnost element syntaxe změní však nezbytné, pokud hodnota vlastnosti je příliš složitý být vyjádřena jako jednoduchý řetězec. V rámci element vlastnosti značek můžete vytvořit instanci jiný objekt a nastavit jeho vlastnosti. Například můžete explicitně nastavit vlastnost například `VerticalOptions` k `LayoutOptions` hodnotu s nastavení vlastností:

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

Jiný příklad: `Grid` má dvě vlastnosti s názvem `RowDefinitions` a `ColumnDefinitions`. Tyto dvě vlastnosti se typu `RowDefinitionCollection` a `ColumnDefinitionCollection`, které jsou kolekce `RowDefinition` a `ColumnDefinition` objekty. Budete muset použít vlastnost element syntaxe nastavit těchto kolekcí.

Tady je na začátek souboru XAML pro `GridDemoPage` třídu zobrazující značky elementu vlastnost pro `RowDefinitions` a `ColumnDefinitions` kolekce:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

Všimněte si zkrácenou syntaxi pro definování velikosti automaticky buněk buněk pixelů šířky a výšky a hvězdičkami nastavení.

## <a name="attached-properties"></a>Přidružené vlastnosti

Seznámili jste se právě, `Grid` vyžaduje vlastnost prvky pro `RowDefinitions` a `ColumnDefinitions` kolekce k definování řádků a sloupců. Ale musí být nějakým způsobem pro programátory označíte, řádků a sloupců kde každou podřízenou `Grid` nachází.

V rámci značky pro každou podřízenou `Grid` zadáte řádků a sloupců podřazených pomocí následující atributy:

-  `Grid.Row`
-  `Grid.Column`

Výchozí hodnoty těchto atributů mají hodnotu 0. Můžete také určit, pokud podřízená zahrnuje více než jeden řádek nebo sloupec s tyto atributy:

-  `Grid.RowSpan`
-  `Grid.ColumnSpan`

Tyto dva atributy mají výchozí hodnoty 1.

Tady je kompletní GridDemoPage.xaml souboru:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>

        <Label Text="Autosized cell"
               Grid.Row="0" Grid.Column="0"
               TextColor="White"
               BackgroundColor="Blue" />

        <BoxView Color="Silver"
                 HeightRequest="0"
                 Grid.Row="0" Grid.Column="1" />

        <BoxView Color="Teal"
                 Grid.Row="1" Grid.Column="0" />

        <Label Text="Leftover space"
               Grid.Row="1" Grid.Column="1"
               TextColor="Purple"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two rows (or more if you want)"
               Grid.Row="0" Grid.Column="2" Grid.RowSpan="2"
               TextColor="Yellow"
               BackgroundColor="Blue"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two columns"
               Grid.Row="2" Grid.Column="0" Grid.ColumnSpan="2"
               TextColor="Blue"
               BackgroundColor="Yellow"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Fixed 100x100"
               Grid.Row="2" Grid.Column="2"
               TextColor="Aqua"
               BackgroundColor="Red"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

    </Grid>
</ContentPage>
```

`Grid.Row` a `Grid.Column` nastavení 0 nejsou vyžadovány, ale jsou obecně zahrnuté pro účely přehlednosti.

Zde je, jak vypadá na všech tří platformách:

[ ![](essential-xaml-syntax-images/griddemo.png "Rozložení mřížky")](essential-xaml-syntax-images/griddemo-large.png "rozložení mřížky")

Posuzování výhradně z syntaxe, tyto `Grid.Row`, `Grid.Column`, `Grid.RowSpan`, a `Grid.ColumnSpan` atributy se zdají být statická pole nebo vlastnosti `Grid`, ale dostatečně interestingly `Grid` nedefinuje nic s názvem `Row`, `Column`, `RowSpan`, nebo `ColumnSpan`.

Místo toho `Grid` definuje čtyři vazbu vlastnosti s názvem `RowProperty`, `ColumnProperty`, `RowSpanProperty`, a `ColumnSpanProperty`. Toto jsou speciální typy vazbu vlastnosti, které jsou známé jako *přidružené vlastnosti*. Jsou definované `Grid` třída ale nastavit na podřízené objekty `Grid`.

Pokud chcete použít tyto přidružené vlastnosti v kódu `Grid` třída poskytuje statické metody s názvem `SetRow`, `GetColumn`, a tak dále. V jazyce XAML, jsou tyto připojené vlastnosti nastaveny jako atributy v podřízených prvků, ale `Grid` pomocí názvů jednoduché vlastnosti.

Přidružené vlastnosti jsou vždy rozpoznatelném v souborech XAML jako atributy obsahující třídy a název vlastnosti odděleny tečkou. Označují se jako *přidružené vlastnosti* vzhledem k tomu, že jsou definované pomocí jednu třídu (v takovém případě `Grid`), ale připojené k jiné objekty (v tomto případě podřízené objekty `Grid`). Během rozložení `Grid` můžete prověřte hodnoty těchto vlastností připojené vědět, kam umístit všechny podřízené.

`AbsoluteLayout` Třída definuje dva přidružené vlastnosti s názvem `LayoutBounds` a `LayoutFlags`. Tady je šachovnicový vzor uvědomili si pomocí přímo úměrná umístění a velikost funkce `AbsoluteLayout`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.AbsoluteDemoPage"
             Title="Absolute Demo Page">

    <AbsoluteLayout BackgroundColor="#FF8080">
        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

  </AbsoluteLayout>
</ContentPage>
```

A tady je:

[ ![](essential-xaml-syntax-images/absolutedemo-large.png "Absolutní rozložení")](essential-xaml-syntax-images/absolutedemo-large.png "absolutní rozložení")

Pro přibližně takto může otázka moudrý použití XAML. Určitě opakování a správnost `LayoutBounds` obdélníku naznačuje, že ho může lépe uskutečňovat v kódu.

Který obavu, jistě legitimní a žádný problém se službou Vyrovnávání použití kódu a kódu při definování uživatelských rozhraní. Je snadné definovat některých vizuálech v jazyce XAML a přidání některé další vizuální prvky, které může lépe generovat v smyčky pomocí konstruktoru souboru kódu na pozadí.

## <a name="content-properties"></a>Vlastnosti obsahu

V předchozích příkladech `StackLayout`, `Grid`, a `AbsoluteLayout` objekty jsou nastaveny na `Content` vlastnost `ContentPage`, a podřízené objekty těchto rozložení jsou ve skutečnosti položky v `Children` kolekce. Ještě tyto `Content` a `Children` vlastnosti jsou nikde v souboru XAML.

Určitě můžete zahrnout `Content` a `Children` vlastnosti jako vlastnosti prvky, například jako v **XamlPlusCode** ukázka:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout.Children>
                <Slider VerticalOptions="CenterAndExpand"
                        ValueChanged="OnSliderValueChanged" />

                <Label x:Name="valueLabel"
                       Text="A simple Label"
                       FontSize="Large"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand" />

                <Button Text="Click Me!"
                      HorizontalOptions="Center"
                      VerticalOptions="CenterAndExpand"
                      Clicked="OnButtonClicked" />
            </StackLayout.Children>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Skutečné otázka se: Proč se tyto prvky vlastnost *není* potřebné v souboru XAML?

Elementy, které jsou definované v Xamarin.Forms pro použití v jazyce XAML mohou mít jednu vlastnost příznakem v `ContentProperty` atribut na třídu. Pokud hledáte `ContentPage` – třída v online dokumentaci Xamarin.Forms, zobrazí se tento atribut:

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

To znamená, že `Content` značky elementu vlastností se nevyžaduje. Veškerý obsah XML, který se zobrazí mezi počáteční a koncová `ContentPage` značky se předpokládá, že má být přiřazen `Content` vlastnost.

 `StackLayout`, `Grid`, `AbsoluteLayout`, a `RelativeLayout` dědí ze `Layout<View>`, a pokud jste vyhledat `Layout<T>` Xamarin.Forms dokumentace, uvidíte jiné `ContentProperty` atribut:

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

Umožňuje obsah rozložení automaticky přidat do `Children` kolekce bez explicitní `Children` značky element vlastnosti.

Ostatní třídy mají také `ContentProperty` atribut definice. Například obsahu vlastnost `Label` je `Text`. Zkontrolujte dokumentaci rozhraní API pro ostatní.

## <a name="platform-differences-with-onplatform"></a>Rozdíly mezi platformy OnPlatform

V jednostránkové aplikace je běžné, chcete-li nastavit `Padding` vlastnost na stránce, aby nedošlo k přepsání iOS stavový řádek. V kódu, můžete použít `Device.RuntimePlatform` vlastnost pro tento účel:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

Můžete také provést něco podobného jako v jazyce XAML pomocí `OnPlatform` a `On` třídy. Nejprve zahrnout vlastnost prvky pro `Padding` vlastnost v horní části stránky:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

V rámci těchto značek patří `OnPlatform` značky. `OnPlatform` je obecná třída. Je třeba zadat argument obecného typu, v takovém případě `Thickness`, což je typ `Padding` vlastnost. Naštěstí je atribut XAML konkrétně k definování obecné argumenty názvem `x:TypeArguments`. Mělo by to odpovídat typu vlastnosti, kterou jste nastavení:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">

        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`OnPlatform` má vlastnost s názvem `Platforms` tedy `IList` z `On` objekty. Použijte značky elementu vlastnosti pro tuto vlastnost:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>

            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Nyní přidejte `On` elementy. Pro každý onem nastavit `Platform` vlastnost a `Value` vlastnost, která má kód pro `Thickness` vlastnost:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>
                <On Platform="iOS" Value="0, 20, 0, 0" />
                <On Platform="Android" Value="0, 0, 0, 0" />
                <On Platform="UWP" Value="0, 0, 0, 0" />
            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Tento kód můžete zjednodušit. Vlastnost obsahu `OnPlatform` je `Platforms`, takže tyto značky element vlastnosti lze odebrat:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android" Value="0, 0, 0, 0" />
            <On Platform="UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`Platform` Vlastnost `On` je typu `IList<string>`, takže může obsahovat více platforem, pokud jsou hodnoty stejné:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android, UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Protože Android a Windows jsou nastavené na výchozí hodnotu `Padding`, že značka lze odebrat:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Toto je standardní způsob, jak nastavit platformy závislé `Padding` vlastnost v jazyce XAML. Pokud `Value` nastavení nemůže být reprezentovaná jednoho řetězce, můžete definovat vlastnost elementy pro ni:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS">
                <On.Value>
                    0, 20, 0, 0
                </On.Value>
            </On>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

## <a name="summary"></a>Souhrn

Vlastnosti elementů a přidružené vlastnosti mnohem základní syntaxe XAML bylo úspěšně navázáno. Ale někdy musíte nastavit vlastnosti objektů nepřímých způsobem, například ze slovníku prostředků. Tento přístup je popsaná v další části, část [3. XAML – rozšíření značek](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).



## <a name="related-links"></a>Související odkazy

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Část 1. Začínáme s jazykem XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Část 3. Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Část 4. Základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Část 5. Z datové vazby k rozhraní MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
