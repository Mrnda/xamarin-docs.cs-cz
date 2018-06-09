---
title: Dynamické styly v Xamarin.Forms
description: Tento článek vysvětluje, jak reagovat na aplikaci Xamarin.Forms na styl změny dynamicky za běhu pomocí dynamické prostředky.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 0f82e0cfde29921ea768000f17b93d04f8ad307e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245218"
---
# <a name="dynamic-styles-in-xamarinforms"></a>Dynamické styly v Xamarin.Forms

_Styly nezadávejte reagovat na změny vlastností a zůstanou nezměněny po dobu trvání aplikace. Například po přiřazení styl visual elementu, pokud jedna z instancí Setter změněna, odebrána nebo přidat novou instanci Setter, změny se nepoužije pro vizuální prvek. Ale aplikace reagovat na změny styl dynamicky za běhu pomocí dynamické prostředky._

`DynamicResource` Je podobná – rozšíření značek `StaticResource` rozšíření značek v obě načíst hodnotu z použití klíče slovníku [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Ale při `StaticResource` vyhledá jeden slovník, `DynamicResource` udržuje odkaz na klíče slovníku. Proto pokud slovník přidružené ke klíči se nahrazuje, změny se použijí pro vizuální prvek. To umožňuje styl změny v modulu runtime v rámci aplikace.

Následující příklad kódu ukazuje *dynamické* stylů na stránce XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle"
                   TargetType="SearchBar"
                   BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle"
                   TargetType="SearchBar">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Placeholder="These SearchBar controls"
                       Style="{DynamicResource searchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Instance použití `DynamicResource` – rozšíření značek tak, aby odkazovaly [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) s názvem `searchBarStyle`, která není definována v XAML. Ale protože [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnosti `SearchBar` instance jsou nastaveny pomocí `DynamicResource`, chybí klíč slovník nevede se došlo k výjimce.

Místo toho v souboru kódu na pozadí konstruktoru vytvoří [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) položka s klíčem `searchBarStyle`, jak ukazuje následující příklad kódu:

```csharp
public partial class DynamicStylesPage : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPage ()
    {
        InitializeComponent ();
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
    }

    void OnButtonClicked (object sender, EventArgs e)
    {
        if (originalStyle) {
            Resources ["searchBarStyle"] = Resources ["greenSearchBarStyle"];
            originalStyle = false;
        } else {
            Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
            originalStyle = true;
        }
    }
}
```

Když `OnButtonClicked` obslužné rutiny události proveden, `searchBarStyle` bude přepínat mezi `blueSearchBarStyle` a `greenSearchBarStyle`. Výsledkem je vidět na následujících snímcích obrazovky vzhled:

[![](dynamic-images/dynamic-style-blue.png "Modrá dynamické styl příklad")](dynamic-images/dynamic-style-blue-large.png#lightbox "modrá dynamické styl příklad")
[![](dynamic-images/dynamic-style-green.png "zelená dynamické styl příklad") ] (dynamic-images/dynamic-style-green-large.png#lightbox "Zelená příklad dynamické styl")

Následující příklad kódu ukazuje na stejnou stránku v jazyce C#:

```csharp
public class DynamicStylesPageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        ...
        var searchBar1 = new SearchBar { Placeholder = "These SearchBar controls" };
        searchBar1.SetDynamicResource (VisualElement.StyleProperty, "searchBarStyle");
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button    }
        };
    }
    ...
}
```

V jazyce C# [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) instance použití [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) metoda tak, aby odkazovaly `searchBarStyle`. `OnButtonClicked` Kód obslužné rutiny událostí je stejný jako v příkladu XAML a po provedení `searchBarStyle` bude přepínat mezi `blueSearchBarStyle` a `greenSearchBarStyle`.

<a name="dynamic-style-inheritance">

## <a name="dynamic-style-inheritance"></a>Dynamické styl dědičnosti

Odvozování z dynamické styl styl nelze dosáhnout pomocí [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) vlastnost. Místo toho [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) třída zahrnuje [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) může dynamicky měnit vlastnosti, která může být nastaveno na klíč slovník jehož hodnota.

Následující příklad kódu ukazuje *dynamické* styl dědičnosti na stránce XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle" TargetType="SearchBar" BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle" TargetType="SearchBar">
              ...
            </Style>
            <Style x:Key="tealSearchBarStyle" TargetType="SearchBar" BaseResourceKey="searchBarStyle">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Text="These SearchBar controls" Style="{StaticResource tealSearchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Instance použití `StaticResource` – rozšíření značek tak, aby odkazovaly [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) s názvem `tealSearchBarStyle`. To `Style` nastaví některé další vlastnosti a používá [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) vlastnost tak, aby odkazovaly `searchBarStyle`. `DynamicResource` – Rozšíření značek není vyžadována, protože `tealSearchBarStyle` nedojde ke změně, s výjimkou `Style` je odvozena z. Proto `tealSearchBarStyle` udržuje odkaz na `searchBarStyle` a je změněn, když se změní základní styl.

V souboru kódu na pozadí konstruktoru vytvoří [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) položka s klíčem `searchBarStyle`, jak na předchozí příklad, který ukázky dynamické stylů. Když `OnButtonClicked` obslužné rutiny události proveden, `searchBarStyle` bude přepínat mezi `blueSearchBarStyle` a `greenSearchBarStyle`. Výsledkem je vidět na následujících snímcích obrazovky vzhled:

[![](dynamic-images/dynamic-style-inheritance-blue.png "Modrá dynamické styl dědičnosti příklad")](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox "modrá dynamické styl dědičnosti příklad")
[![](dynamic-images/dynamic-style-inheritance-green.png "zelená dynamické styl Příklad dědičnosti")](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox "zelená příklad dynamické styl dědičnosti")

Následující příklad kódu ukazuje na stejnou stránku v jazyce C#:

```csharp
public class DynamicStylesInheritancePageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesInheritancePageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var tealSearchBarStyle = new Style (typeof(SearchBar)) {
            BaseResourceKey = "searchBarStyle",
            ...
        };
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = {
                new SearchBar { Text = "These SearchBar controls", Style = tealSearchBarStyle },
                ...
            }
        };
    }
    ...
}
```

`tealSearchBarStyle` Je přiřazen přímo na [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnost [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) instance. To `Style` nastaví některé další vlastnosti a používá [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) vlastnost tak, aby odkazovaly `searchBarStyle`. [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) Metoda požadované zde není protože `tealSearchBarStyle` nedojde ke změně, s výjimkou `Style` je odvozena z. Proto `tealSearchBarStyle` udržuje odkaz na `searchBarStyle` a je změněn, když se změní základní styl.

## <a name="summary"></a>Souhrn

Styly nezadávejte reagovat na změny vlastností a zůstanou nezměněny po dobu trvání aplikace. Ale aplikace reagovat na změny styl dynamicky za běhu pomocí dynamické prostředky. Kromě toho *dynamické* styly může být odvozen od s [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) vlastnost.



## <a name="related-links"></a>Související odkazy

- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dynamické styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Práce se styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metoda setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
