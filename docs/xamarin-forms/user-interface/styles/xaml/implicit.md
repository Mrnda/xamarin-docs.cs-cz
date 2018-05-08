---
title: Implicitní styly
description: Implicitní styl je ten, který je používán všechny ovládací prvky typu stejné TargetType, bez nutnosti každý ovládací prvek tak, aby odkazovaly styl.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: d9c9f8816ea45ac122829739ad5134cc740263df
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="implicit-styles"></a>Implicitní styly

_Implicitní styl je ten, který je používán všechny ovládací prvky typu stejné TargetType, bez nutnosti každý ovládací prvek tak, aby odkazovaly styl._

## <a name="creating-an-implicit-style-in-xaml"></a>Vytváření implicitní styl v jazyce XAML

Deklarovat [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) na úrovni stránky [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) musí být přidaný do stránky a pak jeden nebo více `Style` můžou být součástí deklarace `ResourceDictionary`. A `Style` přišla *implicitní* není zadáním `x:Key` atribut. Styl se pak použijí pro vizuální prvky, které odpovídají `TargetType` přesně, ale ne na elementy, které jsou odvozeny od `TargetType` hodnotu.

Následující příklad kódu ukazuje *implicitní* styl deklarované v jazyce XAML na stránce `ResourceDictionary`a použít na stránku [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) instancí:

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

[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Definuje jeden *implicitní* styl, který se použije na stránku [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) instance. `Style` Se používá k zobrazení modrý text na pozadí žluté, při nastavení také další možnosti vzhledu. `Style` Se přidá na stránku [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) bez zadání `x:Key` atribut. Proto `Style` se použije pro všechny `Entry` implicitně instance jako odpovídají [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) vlastnost `Style` přesně. Však `Style` u není použitá `CustomEntry` instanci, která je rozčleněné `Entry`. Výsledkem je vidět na následujících snímcích obrazovky vzhled:

[![](implicit-images/implicit-styles.png "Příklad implicitní styly")](implicit-images/implicit-styles-large.png#lightbox "příklad implicitní styly")

Kromě toho čtvrtý [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) přepsání [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) a [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) vlastnosti styl implicitní do různých `Color`hodnoty.

### <a name="creating-an-implicit-style-at-the-control-level"></a>Vytváření implicitní styl v ovládacím prvku úrovně

Kromě vytváření *implicitní* styly na úrovni stránky je lze také vytvořit na úrovni ovládacího prvku, jak je znázorněno v následujícím příkladu kódu:

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

V tomto příkladu *implicitní* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) je přiřazena k [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekce [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)ovládacího prvku. *Implicitní* styl pak můžete použít k řízení a její podřízené položky.

Informace o vytváření stylů v aplikace [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), najdete v části [globální styly](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-implicit-style-in-c35"></a>Vytváření implicitní styl v jazyce C&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance lze přidat na stránku [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekce v jazyce C# tak, že vytvoříte novou [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)a pak přidáním `Style` instance k `ResourceDictionary`, jak je znázorněno v Následující příklad kódu:

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

V konstruktoru definuje jeden *implicitní* styl, který se použije na stránku [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) instance. `Style` Se používá k zobrazení modrý text na pozadí žluté, při nastavení také další možnosti vzhledu. `Style` Se přidá na stránku [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) bez zadání `key` řetězec. Proto `Style` se použije pro všechny `Entry` implicitně instance jako odpovídají [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) vlastnost `Style` přesně. Však `Style` u není použitá `CustomEntry` instanci, která je rozčleněné `Entry`.

## <a name="summary"></a>Souhrn

*Implicitní* styl je ten, který se používá ve všech vizuálních prvků na stejné [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), bez nutnosti každý ovládací prvek tak, aby odkazovaly styl. A `Style` přišla *implicitní* není zadáním `x:Key` atribut. Místo toho `x:Key` atribut se automaticky stane hodnotu [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) vlastnost.



## <a name="related-links"></a>Související odkazy

- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Základní styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Práce se styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metoda setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
