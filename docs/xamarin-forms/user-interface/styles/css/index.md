---
title: Práce se styly Xamarin.Forms aplikace pomocí šablony stylů CSS
description: Xamarin.Forms podporuje vizuální prvky stylu pomocí stylů CSS (Cascading Style).
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 811abacff330bf7b6e6240691cb6a15ebbd9d242
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets"></a>Práce se styly Xamarin.Forms aplikace pomocí šablony stylů CSS

_Xamarin.Forms podporuje vizuální prvky stylu pomocí stylů CSS (Cascading Style)._

Xamarin.Forms 3.0 uvádí možnost styl aplikace pomocí šablon stylů CSS. Šablony stylů se skládá z seznam pravidel, se každé pravidlo skládající se z jednoho nebo více selektory a blok deklarace. Blok deklarace se skládá z seznam ve složené závorky deklarace s každou deklarace vlastnosti, dvojtečkou a hodnotu. Když jsou více deklarací v bloku, vloží se jako oddělovač středníkem. Následující příklad kódu ukazuje některé Xamarin.Forms kompatibilní šablon stylů CSS:

```css
^contentpage {
    background-color: lightgray;
}

#listView {
    background-color: lightgray;
}

stacklayout {
    margin: 20;
}

.mainPageTitle {
    font-style: bold;
    font-size: medium;
}

.mainPageSubtitle {
    margin-top: 15;
}

.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}

listview image {
    height: 60;
    width: 60;
}

stacklayout>image {
    height: 200;
    width: 200;
}
```

V Xamarin.Forms analyzovat a vyhodnocovány v modulu runtime, nikoli době potřebné ke kompilaci šablony stylů CSS a jsou znovu Analyzovaná při použití šablony stylů.

> [!NOTE]
> Všechny stylů, který je možné pomocí XAML stylů v současné době nelze provést s šablon stylů CSS. Styly XAML lze však doplnit šablon stylů CSS pro vlastnosti, které jsou aktuálně nepodporuje Xamarin.Forms. Další informace o styly XAML najdete v tématu [styly Xamarin.Forms aplikací pomocí jazyka XAML styly](~/xamarin-forms/user-interface/styles/xaml/index.md).

[MonkeyAppCSS](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/) ukázka ukazuje, jak pomocí šablon stylů CSS k úpravě stylu jednoduchou aplikaci a zobrazí se na následujících snímcích obrazovky:

[![Hlavní stránka MonkeyApp s stylů CSS](css-images/MonkeyAppMainPage.png "MonkeyApp hlavní stránky pomocí stylů CSS")](css-images/MonkeyAppMainPage-Large.png#lightbox "MonkeyApp hlavní stránky pomocí stylů CSS")

[![Stránka Podrobnosti MonkeyApp s stylů CSS](css-images/MonkeyAppDetailPage.png "stránku s podrobnostmi MonkeyApp s stylů CSS")](css-images/MonkeyAppDetailPage-Large.png#lightbox "stránku s podrobnostmi MonkeyApp s stylů CSS")

> [!NOTE]
> Není možné aktuálně k úpravě stylu barvu pozadí [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) pomocí šablony stylů. Proto v ukázkové aplikaci [ `NavigationPage.BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) je nastavena v kódu.

## <a name="consuming-a-style-sheet"></a>Použití šablony stylů

Proces pro přidání šablony stylů na řešení, vypadá takto:

1. Přidejte do projektu knihovny .NET standardní prázdný soubor CSS.
1. Nastavení akce sestavení souboru CSS, který se **EmbeddedResource**.

### <a name="loading-a-style-sheet"></a>Načítání šablony stylů

Existuje několik přístupů, které slouží k načtení šablony stylů.

### <a name="xaml"></a>XAML

Šablony stylů můžete načíst a analyzovat s [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) třída před se přidává do [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) stránky:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    ...
</ContentPage>
```

[ `StyleSheet.Source` ](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) Vlastnost určuje šablony stylů jako identifikátor URI relativní k umístění souboru XAML nadřazených nebo relativní vůči kořenovému adresáři projektu, pokud identifikátor URI začíná `/`.

> [!WARNING]
> Soubor CSS se nepodaří načíst, pokud je akce sestavení není nastavena na **EmbeddedResource**.

Alternativně můžete načíst a analyzovat pomocí šablony stylů [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) třídy podle vložené v `CDATA` části:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet>
            <![CDATA[
            ^contentpage {
                background-color: lightgray;
            }
            ]]>
        </StyleSheet>
    </ContentPage.Resources>
    ...
</ContentPage>
```

### <a name="c"></a>C#

V jazyce C#, můžete načíst jako vložený prostředek šablony stylů a přidat do [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) stránky:

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        this.Resources.Add(StyleSheet.FromAssemblyResource(
            IntrospectionExtensions.GetTypeInfo(typeof(MyPage)).Assembly,
            "MyProject.Assets.styles.css"));
    }
}
```

První argument `StyleSheet.FromAssemblyResource` metoda je sestavení obsahující šablony stylů, zatímco druhý argument je `string` představující identifikátor prostředku. Identifikátor prostředku můžete získat **vlastnosti** okno, pokud je vybrána soubor CSS.

Alternativně šablony stylů mohou být načteny z `StringReader` a přidají se do [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) stránky:

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        using (var reader = new StringReader("^contentpage { background-color: lightgray; }"))
        {
            this.Resources.Add(StyleSheet.FromReader(reader));
        }
    }
}
```

Argument `StyleSheet.FromReader` metoda je `TextReader` který má číst šablony stylů.

## <a name="selecting-elements-and-applying-properties"></a>Výběr elementy a použití vlastností

Šablon stylů CSS selektory používá k určení prvky, které chcete cílit. Styly s odpovídajícím selektory se použijí po sobě, v pořadí definice. Styly definované na konkrétní položku jsou vždy použito jako poslední. Další informace o podporovaných selektory najdete v tématu [selektor odkaz](#selector-reference).

Šablon stylů CSS používá vlastnosti k formátování vybraného elementu. Každá vlastnost má sadu možných hodnot, a některé vlastnosti může ovlivnit libovolný typ elementu, kdežto jiné jenom pro skupiny elementů. Další informace o podporovaných vlastností najdete v tématu [odkaz na vlastnost](#property-reference).

### <a name="selecting-elements-by-type"></a>Výběr prvků podle typu

Elementy ve vizuální strojové struktuře lze vybrat podle typu s malá a velká písmena `element` selektor:

```css
stacklayout {
    margin: 20;
}
```

Tento selektor identifikuje všechny [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) elementů na stránky, které využívají šablony stylů a nastaví jejich okraje uniform tloušťka 20.

> [!NOTE]
> `element` Selektor neidentifikuje dílčí třídy zadaného typu.

### <a name="selecting-elements-by-base-class"></a>Výběr prvků podle základní třídy

Elementy ve vizuální strojové struktuře lze vybrat základní třídou s malá a velká písmena `^base` selektor:

```css
^contentpage {
    background-color: lightgray;
}
```

Tento selektor identifikuje všechny [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) prvky, které využívají šablony stylů a nastaví jejich pozadí barvu, která má `lightgray`.

> [!NOTE]
> `^base` Selektor je specifický pro Xamarin.Forms a není součástí specifikace šablon stylů CSS.

### <a name="selecting-an-element-by-name"></a>Výběr elementu podle názvu

Jednotlivé elementy ve vizuální strojové struktuře lze vybrat pomocí malá a velká písmena `#id` selektor:

```css
#listView {
    background-color: lightgray;
}
```

Tento výběr určuje element jejichž [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) je nastavena na `listView`. Ale pokud `StyleId` není nastavena vlastnost, modulu pro výběr se vrátit k použití `x:Name` elementu. Proto v následujícím příkladu XAML `#listView` určují selektor [ `ListView` ](xref:Xamarin.Forms.ListView) jejichž `x:Name` je atribut nastaven na `listView`a nastaví barvu pozadí jeho `lightgray`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView x:Name="listView" ...>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

### <a name="selecting-elements-with-a-specific-class-attribute"></a>Výběr prvků s atributem určité třídy

Elementy s atributem určité třídy lze vybrat pomocí malá a velká písmena `.class` selektor:

```css
.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}
```

Třídu CSS lze přiřadit k elementu XAML nastavením [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) vlastnost elementu název třídy CSS. Proto v následujícím příkladu XAML styly definované `.detailPageTitle` třídy jsou přiřazeny k první [ `Label` ](xref:Xamarin.Forms.Label), při styly definované `.detailPageSubtitle` třídy jsou přiřazeny k druhý `Label`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            <Label ... StyleClass="detailPageTitle" />
            <Label ... StyleClass="detailPageSubtitle"/>
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

### <a name="selecting-child-elements"></a>Výběr podřízených elementů

Podřízené elementy ve vizuální strojové struktuře lze vybrat pomocí malá a velká písmena `element element` selektor:

```css
listview image {
    height: 60;
    width: 60;
}
```

Tento selektor identifikuje všechny [ `Image` ](xref:Xamarin.Forms.Image) prvky, které jsou podřízené objekty [ `ListView` ](xref:Xamarin.Forms.ListView) prvky a nastaví jejich výšky a šířky na 60. Proto v následujícím příkladu XAML `listview image` určují selektor [ `Image` ](xref:Xamarin.Forms.Image) která je podřízená [ `ListView` ](xref:Xamarin.Forms.ListView)a nastaví jeho výška a šířka na 60.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView ...>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid>
                            ...
                            <Image ... />
                            ...
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> `element element` Selektor nevyžaduje podřízený element být _přímé_ podřízeného člena nadřazeného – podřízený element může mít jiné nadřazené. Za předpokladu, že nadřazený prvek je zadaný první prvek, dojde k výběru.

### <a name="selecting-direct-child-elements"></a>Výběr přímé podřízené elementy

Přímé podřízené elementy ve vizuální strojové struktuře lze vybrat pomocí malá a velká písmena `element>element` selektor:

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

Tento selektor identifikuje všechny [ `Image` ](xref:Xamarin.Forms.Image) prvky, které jsou přímo podřízené [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) prvky a nastaví jejich výšky a šířky na 200. Proto v následujícím příkladu XAML `stacklayout>image` určují selektor [ `Image` ](xref:Xamarin.Forms.Image) , je přímé podřízená [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)a nastaví jeho výška a šířka na 200.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            ...
            <Image ... />
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

> [!NOTE]
> `element>element` Selektor vyžaduje, aby o podřízený prvek _přímé_ podřízeného člena nadřazeného.

## <a name="selector-reference"></a>Selektor odkaz

Xamarin.Forms podporuje následující Selektory šablon stylů CSS:

|Selektor|Příklad|Popis|
|---|---|---|
|`.class`|`.header`|Vybere všechny prvky s `StyleClass` vlastnost obsahující 'záhlaví'. Upozorňujeme, že tento selektor se velká a malá písmena.|
|`#id`|`#email`|Vybere všechny prvky s `StyleId` nastavena na `email`. Pokud `StyleId` není nastavena, přechod do režimu `x:Name`. Při použití XAML, `x:Name` upřednostňuje přes `StyleId`. Upozorňujeme, že tento selektor se velká a malá písmena.|
|`*`|`*`|Vybere všechny elementy.|
|`element`|`label`|Vybere všechny elementy typu `Label`, ale není dílčí třídy. Upozorňujeme, že tento selektor se malá a velká písmena.|
|`^base`|`^contentpage`|Vybere všechny prvky s `ContentPage` jako základní třídy, včetně `ContentPage` sám sebe. Všimněte si, že tento selektor se malá a velká písmena a není součástí specifikace šablon stylů CSS.|
|`element,element`|`label,button`|Vybere všechny `Button` elementy a všechny `Label` elementy. Upozorňujeme, že tento selektor se malá a velká písmena.|
|`element element`|`stacklayout label`|Vybere všechny `Label` elementy uvnitř `StackLayout`. Upozorňujeme, že tento selektor se malá a velká písmena.|
|`element>element`|`stacklayout>label`|Vybere všechny `Label` prvky s `StackLayout` jako přímý nadřazený objekt. Upozorňujeme, že tento selektor se malá a velká písmena.|
|`element+element`|`label+entry`|Vybere všechny `Entry` elementy přímo po `Label`. Upozorňujeme, že tento selektor se malá a velká písmena.|
|`element~element`|`label~entry`|Vybere všechny `Entry` elementy před sebou `Label`. Upozorňujeme, že tento selektor se malá a velká písmena.|

Styly s odpovídajícím selektory se použijí po sobě, v pořadí definice. Styly definované na konkrétní položku jsou vždy použito jako poslední.

> [!TIP]
> Selektory mohou být kombinovány bez omezení, jako například `StackLayout>ContentView>label.email`.

Na následující selektory nejsou aktuálně podporovány:

- `[attribute]`
- `@media` A `@supports`
- `:` A `::`

> [!NOTE]
> Specifické podobě a přepsání zvláštní vlastnosti nejsou podporovány.

## <a name="property-reference"></a>Odkaz na vlastnost

Podporuje následující vlastnosti CSS Xamarin.Forms (v **hodnoty** sloupce, typy jsou _Kurzíva_, zatímco jsou textové literály `gray`):

|Vlastnost|Platí pro|Hodnoty|Příklad|
|---|---|---|---|
|`background-color`|`VisualElement`|_Barva_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_Řetězec_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`|_Barva_ \| `initial`|`border-color: #9acd32;`|
|`border-width`|`Button`|_Double_ \| `initial` |`border-width: .5;`|
|`color`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`|_Barva_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Řetězec_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_dvojité_ \| _namedsize_  \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_Double_ \| `initial` |`min-height: 250;`|
|`margin`|`View`|_Tloušťka_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_Tloušťka_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_Tloušťka_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_Tloušťka_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_Tloušťka_ \| `initial` |`margin-bottom: 6;`|
|`min-height`|`VisualElement`|_Double_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_Double_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_Double_ \| `initial` |`opacity: .3;`|
|`padding`|`Layout`, `Page`|_Tloušťka_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Layout`, `Page`|_Double_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Layout`, `Page`| _Double_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Layout`, `Page`| _Double_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Layout`, `Page`| _Double_ \| `initial` |`padding-bottom: 6;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `right` \| `center` \| `start` \| `end` \| `initial`. `left` a `right` by se mělo zabránit v prostředích zprava doleva.| `text-align: right;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial `|`visibility: hidden;`|
|`width`|`VisualElement`|_Double_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` není platná hodnota pro všechny vlastnosti. Vypne hodnotu (výchozí nastavení), která byla nastavena z jiný styl.

Tyto vlastnosti nejsou aktuálně podporovány:

- `all: initial`.
- Rozložení vlastnosti (pole a mřížky).
- Sdružená vlastnost vlastnosti, jako například `font`, a `border`.

Kromě toho není žádná `inherit` hodnota a tak dědičnosti není podporován. Proto nelze, například nastavit `font-size` vlastnost rozložení a očekávají, že všechny [ `Label` ](xref:Xamarin.Forms.Label) instancí v rozložení pro dědění hodnota. Jedinou výjimkou je `direction` vlastnosti, která má výchozí hodnotu z `inherit`.

### <a name="color"></a>Barva

Následující `color` hodnoty jsou podporovány:

- `X11` [barvy](https://en.wikipedia.org/wiki/X11_color_names/), které odpovídají barvy šablon stylů CSS, UWP barvy předem definovaných a Xamarin.Forms barvy. Všimněte si, že jsou tyto hodnoty barev malá a velká písmena.
- Hex barev: `#rgb`, `#argb`, `#rrggbb`, `#aarrggbb`
- RGB barvy: `rgb(255,0,0)`, `rgb(100%,0%,0%)`. Hodnoty jsou v rozsahu 0 až 255 nebo 0-100 %.
- barvy rgba: `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`. Hodnota neprůhlednosti je v rozsahu 0,0 1.0.
- HSL barev: `hsl(120, 100%, 50%)`. H hodnota je v rozsahu 0-360, zatímco s a l jsou v rozsahu 0 % - 100 %.
- barvy hsla: `hsla(120, 100%, 50%, .8)`. Hodnota neprůhlednosti je v rozsahu 0,0 1.0.

### <a name="thickness"></a>Tloušťka

Jeden, dva, tři nebo čtyři `thickness` hodnoty jsou podporovány, jsou odděleny mezer:

- Jedna hodnota označuje uniform tloušťka.
- Dvě hodnoty znamenat tloušťka svislých pak vodorovné.
- Tři hodnoty udávají nahoře, a vodorovných (levé a pravé) a potom vyberte tloušťka dolní.
- Čtyři hodnoty udávají nahoře, pak vpravo a dole a potom levém tloušťka.

> [!NOTE]
> Šablon stylů CSS `thickness` hodnoty liší od XAML [ `Thickness` ](/api/type/Xamarin.Forms.Thickness/) hodnoty. Například v jazyce XAML hodnotu dva `Thickness` určuje vodorovné pak svislé tloušťka při hodnotu čtyři `Thickness` označuje levé straně a horní a potom vyberte doprava dolů tloušťka. Kromě toho XAML `Thickness` hodnoty jsou oddělený čárkami.

### <a name="namedsize"></a>NamedSize

Následující malá a velká písmena `namedsize` hodnoty jsou podporovány:

- `default`
- `micro`
- `small`
- `medium`
- `large`

Přesný význam jednotlivých `namedsize` hodnota je závislé na platformu a závislé na zobrazení.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>Šablon stylů CSS v Xamarin.Forms s Xamarin.University

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.Forms 3.0 šablon stylů CSS, pomocí [univerzity Xamarin](https://university.xamarin.com/)**

## <a name="related-links"></a>Související odkazy

- [MonkeyAppCSS (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)
- [Práce se styly Xamarin.Forms aplikací pomocí jazyka XAML styly](~/xamarin-forms/user-interface/styles/xaml/index.md)
