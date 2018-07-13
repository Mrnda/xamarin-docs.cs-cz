---
title: Implicitní styly v Xamarin.Forms
description: Implicitní styl je ten, který používá všechny ovládací prvky stejného TargetType, aniž by každý ovládací prvek tak, aby odkazovaly styl.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 277be51c242521f52e9b1e162226ae8137e7b133
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995511"
---
# <a name="implicit-styles-in-xamarinforms"></a>Implicitní styly v Xamarin.Forms

_Implicitní styl je ten, který používá všechny ovládací prvky stejného TargetType, aniž by každý ovládací prvek tak, aby odkazovaly styl._

## <a name="creating-an-implicit-style-in-xaml"></a>Vytváření implicitní styl v XAML

Chcete-li deklarovat [ `Style` ](xref:Xamarin.Forms.Style) na úrovni stránky [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) musí být přidán do stránky a pak jednu nebo víc `Style` mohou být součástí deklarace `ResourceDictionary`. A `Style` tvoří *implicitní* bez zadání `x:Key` atribut. Styl se pak použije k vizuální prvky, které odpovídají `TargetType` přesně, ale ne na elementy, které jsou odvozeny z `TargetType` hodnotu.

Následující příklad kódu ukazuje *implicitní* styl deklarované v XAML na stránce `ResourceDictionary`a použít na stránku [ `Entry` ](xref:Xamarin.Forms.Entry) instancí:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Entry">
                <Setter Property="HorizontalOptions" Value="Fill" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BackgroundColor" Value="Yellow" />
                <Setter Property="FontAttributes" Value="Italic" />
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Entry Text="These entries" />
            <Entry Text="are demonstrating" />
            <Entry Text="implicit styles," />
            <Entry Text="and an implicit style override" BackgroundColor="Lime" TextColor="Red" />
            <local:CustomEntry Text="Subclassed Entry is not receiving the style" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Definuje jedinou *implicitní* styl, který se použije na stránku [ `Entry` ](xref:Xamarin.Forms.Entry) instancí. `Style` Slouží k zobrazení modrý text na žlutým pozadím při nastavování také další možnosti vzhled. `Style` Je přidán na stránku [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) bez zadání `x:Key` atribut. Proto `Style` platí pro všechny `Entry` instance implicitně, jak budou odpovídat [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) vlastnost `Style` přesně. Ale `Style` nebude použito `CustomEntry` instanci, která je rozčleněných do podtříd `Entry`. Výsledkem je vzhled je znázorněno na následujících snímcích obrazovky:

[![](implicit-images/implicit-styles.png "Příklad implicitní styly")](implicit-images/implicit-styles-large.png#lightbox "příklad implicitní styly")

Kromě toho, čtvrtý [ `Entry` ](xref:Xamarin.Forms.Entry) přepsání [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) a [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) vlastnosti implicitní styl různých `Color`hodnoty.

### <a name="creating-an-implicit-style-at-the-control-level"></a>Vytváření implicitní styl v ovládacím prvku úrovni

Kromě vytvoření *implicitní* styly na úrovni stránky, je lze také vytvořit na úrovni ovládacího prvku, jak je znázorněno v následujícím příkladu kódu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style TargetType="Entry">
                        <Setter Property="HorizontalOptions" Value="Fill" />
                        ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Entry Text="These entries" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

V tomto příkladu *implicitní* [ `Style` ](xref:Xamarin.Forms.Style) přiřazen [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) kolekce [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)ovládacího prvku. *Implicitní* styl lze pak použít na ovládací prvek a jeho podřízené položky.

Informace o vytváření styly v aplikačním [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), naleznete v tématu [globální styly](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-implicit-style-in-c35"></a>Vytváření implicitní styl v jazyce C&#35;

[`Style`](xref:Xamarin.Forms.Style) instance lze přidat na stránku [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) kolekce v jazyce C# vytvořit nový [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)a potom přidáním `Style` instance na `ResourceDictionary`, jak je znázorněno Následující příklad kódu:

```csharp
public class ImplicitStylesPageCS : ContentPage
{
    public ImplicitStylesPageCS ()
    {
        var entryStyle = new Style (typeof(Entry)) {
            Setters = {
                ...
                new Setter { Property = Entry.TextColorProperty, Value = Color.Blue }
            }
        };

        ...
        Resources = new ResourceDictionary ();
        Resources.Add (entryStyle);

        Content = new StackLayout {
            Children = {
                new Entry { Text = "These entries" },
                new Entry { Text = "are demonstrating" },
                new Entry { Text = "implicit styles," },
                new Entry { Text = "and an implicit style override", BackgroundColor = Color.Lime, TextColor = Color.Red },
                new CustomEntry  { Text = "Subclassed Entry is not receiving the style" }
            }
        };
    }
}
```

Konstruktor definuje jedinou *implicitní* styl, který se použije na stránku [ `Entry` ](xref:Xamarin.Forms.Entry) instancí. `Style` Slouží k zobrazení modrý text na žlutým pozadím při nastavování také další možnosti vzhled. `Style` Je přidán na stránku [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) bez zadání `key` řetězec. Proto `Style` platí pro všechny `Entry` instance implicitně, jak budou odpovídat [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) vlastnost `Style` přesně. Ale `Style` nebude použito `CustomEntry` instanci, která je rozčleněných do podtříd `Entry`.

## <a name="summary"></a>Souhrn

*Implicitní* style je ten, který se používá ve všech vizuálních prvků stejného [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType), aniž by každý ovládací prvek tak, aby odkazovaly styl. A `Style` tvoří *implicitní* bez zadání `x:Key` atribut. Místo toho `x:Key` atribut se automaticky stane hodnota [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) vlastnost.



## <a name="related-links"></a>Související odkazy

- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Základní styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Práce se styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Styl](xref:Xamarin.Forms.Style)
- [Metoda setter](xref:Xamarin.Forms.Setter)
