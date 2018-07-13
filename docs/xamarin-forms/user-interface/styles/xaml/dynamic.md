---
title: Dynamické styly v Xamarin.Forms
description: Tento článek vysvětluje, jak může aplikace Xamarin.Forms reakce na změny stylu dynamicky za běhu pomocí dynamické prostředky.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: cedf9e3daed9a2d5f8bfa0962bf66510748b592a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997143"
---
# <a name="dynamic-styles-in-xamarinforms"></a>Dynamické styly v Xamarin.Forms

_Styly není reagovat na změny vlastností a zůstanou nezměněny po dobu trvání aplikace. Například po přidělení styl vizuální prvek, pokud jedna z instancí Setter změněna, odebrána nebo nová instance Setter přidání, změny se nepoužije pro vizuální prvek. Ale aplikace reagovat na změny stylu dynamicky za běhu pomocí dynamické prostředky._

`DynamicResource` – Rozšíření značek se podobá `StaticResource` – rozšíření značek v obou použít klíč slovníku se načíst hodnotu z [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Nicméně Přestože `StaticResource` provede vyhledávání jednoho slovníku `DynamicResource` udržuje odkaz na klíč slovníku. Proto pokud se nahradí slovníku přidružený ke klíči, použití této změny na vizuální prvek. To umožňuje modulu runtime styl změny provedené v aplikaci.

Následující příklad kódu ukazuje *dynamické* styly stránky XAML:

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

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar) Instance použití `DynamicResource` – rozšíření značek tak, aby odkazovaly [ `Style` ](xref:Xamarin.Forms.Style) s názvem `searchBarStyle`, která není definovaná v XAML. Ale protože [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnosti `SearchBar` instance jsou nastaveny pomocí `DynamicResource`, chybí klíč slovníku nevede k vyvolání výjimky.

Místo toho v souboru kódu na pozadí, vytvoří konstruktor [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) položka s klíčem `searchBarStyle`, jak je znázorněno v následujícím příkladu kódu:

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

Když `OnButtonClicked` obslužná rutina události je spuštěn, `searchBarStyle` Přepne mezi `blueSearchBarStyle` a `greenSearchBarStyle`. Výsledkem je vzhled je znázorněno na následujících snímcích obrazovky:

[![](dynamic-images/dynamic-style-blue.png "Modrá příklad dynamické styl")](dynamic-images/dynamic-style-blue-large.png#lightbox "modrou příklad dynamické styl")
[![](dynamic-images/dynamic-style-green.png "zelená příklad dynamické styl") ] (dynamic-images/dynamic-style-green-large.png#lightbox "Zelená příklad dynamické styl")

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

V jazyce C# [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) instance použití [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource*) metodu tak, aby odkazovaly `searchBarStyle`. `OnButtonClicked` Kód obslužné rutiny události je stejný jako v příkladu XAML a při spuštění `searchBarStyle` Přepne mezi `blueSearchBarStyle` a `greenSearchBarStyle`.

<a name="dynamic-style-inheritance">

## <a name="dynamic-style-inheritance"></a>Dědičnost stylů dynamické

Styl odvozený od dynamické styl není možné dosáhnout použitím [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) vlastnost. Místo toho [ `Style` ](xref:Xamarin.Forms.Style) třída zahrnuje [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) vlastnost, která může být nastaven na klíč slovníku jehož hodnota může dynamicky měnit.

Následující příklad kódu ukazuje *dynamické* stylu dědičnosti v stránky XAML:

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

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar) Instance použití `StaticResource` – rozšíření značek tak, aby odkazovaly [ `Style` ](xref:Xamarin.Forms.Style) s názvem `tealSearchBarStyle`. To `Style` nastaví některé další vlastnosti a používá [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) vlastnost tak, aby odkazovaly `searchBarStyle`. `DynamicResource` – Rozšíření značek se nevyžaduje, protože `tealSearchBarStyle` nedojde ke změně, s výjimkou `Style` se odvozuje od. Proto `tealSearchBarStyle` udržuje odkaz na `searchBarStyle` a je změnit při změně stylu základní.

V souboru kódu na pozadí, vytvoří konstruktor [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) položka s klíčem `searchBarStyle`, protože za předchozí příklad, který jsme vám ukázali dynamické styly. Když `OnButtonClicked` obslužná rutina události je spuštěn, `searchBarStyle` Přepne mezi `blueSearchBarStyle` a `greenSearchBarStyle`. Výsledkem je vzhled je znázorněno na následujících snímcích obrazovky:

[![](dynamic-images/dynamic-style-inheritance-blue.png "Modrá dynamické styl dědičnosti příklad")](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox "modrou dynamické styl dědičnosti příklad")
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

`tealSearchBarStyle` Se přiřadí přímo [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnost [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) instancí. To `Style` nastaví některé další vlastnosti a používá [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) vlastnost tak, aby odkazovaly `searchBarStyle`. [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource*) Metoda není vyžadována zde protože `tealSearchBarStyle` nedojde ke změně, s výjimkou `Style` se odvozuje od. Proto `tealSearchBarStyle` udržuje odkaz na `searchBarStyle` a je změnit při změně stylu základní.

## <a name="summary"></a>Souhrn

Styly není reagovat na změny vlastností a zůstanou nezměněny po dobu trvání aplikace. Ale aplikace reagovat na změny stylu dynamicky za běhu pomocí dynamické prostředky. Kromě toho *dynamické* styly může být odvozena z s [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) vlastnost.



## <a name="related-links"></a>Související odkazy

- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dynamické styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Práce se styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Styl](xref:Xamarin.Forms.Style)
- [Metoda setter](xref:Xamarin.Forms.Setter)
