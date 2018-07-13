---
title: Explicitní styly v Xamarin.Forms
description: Explicitní styl je ten, který je selektivně použít u ovládacích prvků tak, že nastavíte jejich vlastnosti Style. Tento článek vysvětluje, jak používat explicitní styly aplikace Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: fba00120ed9f5c74bec7622ae1914c43533e8579
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998567"
---
# <a name="explicit-styles-in-xamarinforms"></a>Explicitní styly v Xamarin.Forms

_Explicitní styl je ten, který je selektivně použít u ovládacích prvků tak, že nastavíte jejich vlastnosti Style._

## <a name="creating-an-explicit-style-in-xaml"></a>Vytvoření explicitní styl v XAML

Chcete-li deklarovat [ `Style` ](xref:Xamarin.Forms.Style) na úrovni stránky [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) musí být přidán do stránky a pak jednu nebo víc `Style` mohou být součástí deklarace `ResourceDictionary`. A `Style` tvoří *explicitní* tím, že jeho deklarace `x:Key` atribut, který poskytuje v klíči popisný `ResourceDictionary`. *Explicitní* styly musí být použijí se určité vizuální prvky nastavením jejich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnosti.

Následující příklad kódu ukazuje *explicitní* styly deklarované v XAML na stránce `ResourceDictionary` a použít na stránku [ `Label` ](xref:Xamarin.Forms.Label) instancí:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="labelRedStyle" TargetType="Label">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
            <Style x:Key="labelGreenStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Green" />
            </Style>
            <Style x:Key="labelBlueStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelRedStyle}" />
            <Label Text="are demonstrating"
                   Style="{StaticResource labelGreenStyle}" />
            <Label Text="explicit styles,"
                   Style="{StaticResource labelBlueStyle}" />
            <Label Text="and an explicit style override"
                   Style="{StaticResource labelBlueStyle}"
                   TextColor="Teal" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Definuje tři *explicitní* stylů, které se použijí na stránku [ `Label` ](xref:Xamarin.Forms.Label) instancí. Každý `Style` slouží k zobrazení textu v odlišnou barvou, při nastavování také písmo možnosti velikosti a vodorovné a svislé rozložení. Každý `Style` se použije na jiný `Label` nastavením jeho [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) pomocí vlastnosti `StaticResource` – rozšíření značek. Výsledkem je vzhled je znázorněno na následujících snímcích obrazovky:

[![](explicit-images/explicit-styles.png "Explicitní styly příklad")](explicit-images/explicit-styles-large.png#lightbox "příklad explicitní styly")

Kromě toho, koncový [ `Label` ](xref:Xamarin.Forms.Label) má [ `Style` ](xref:Xamarin.Forms.Style) použít, ale také přepisuje [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) vlastnost různých `Color`hodnotu.

### <a name="creating-an-explicit-style-at-the-control-level"></a>Vytváření explicitní styl v ovládacím prvku úrovni

Kromě vytvoření *explicitní* styly na úrovni stránky, je lze také vytvořit na úrovni ovládacího prvku, jak je znázorněno v následujícím příkladu kódu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelRedStyle" TargetType="Label">
                      ...
                    </Style>
                    ...
                </ResourceDictionary>
            </StackLayout.Resources>
            <Label Text="These labels" Style="{StaticResource labelRedStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

V tomto příkladu *explicitní* [ `Style` ](xref:Xamarin.Forms.Style) jsou instance přiřazeny do [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) kolekce [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) ovládacího prvku. Styly lze pak použít na ovládací prvek a jeho podřízené položky.

Informace o vytváření styly v aplikačním [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), naleznete v tématu [globální styly](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-explicit-style-in-c35"></a>Vytvoření explicitní stylu v jazyce C&#35;

[`Style`](xref:Xamarin.Forms.Style) instance lze přidat na stránku [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) kolekce v jazyce C# vytvořit nový [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)a potom přidáním `Style` instance na `ResourceDictionary`, jak je znázorněno Následující příklad kódu:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red    }
            }
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Green }
            }
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Blue }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("labelRedStyle", labelRedStyle);
        Resources.Add ("labelGreenStyle", labelGreenStyle);
        Resources.Add ("labelBlueStyle", labelBlueStyle);
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels",
                            Style = (Style)Resources ["labelRedStyle"] },
                new Label { Text = "are demonstrating",
                            Style = (Style)Resources ["labelGreenStyle"] },
                new Label { Text = "explicit styles,",
                            Style = (Style)Resources ["labelBlueStyle"] },
                new Label {    Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

Konstruktor definuje tři *explicitní* stylů, které se použijí na stránku [ `Label` ](xref:Xamarin.Forms.Label) instancí. Každý *explicitní* [ `Style` ](xref:Xamarin.Forms.Style) se přidá do [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) pomocí [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) metodu, zadáte `key` řetězec k odkazování `Style` instance. Každý `Style` se použije na jiný `Label` nastavením jejich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnosti.

Však neexistuje žádná výhoda pro použití [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) tady. Místo toho [ `Style` ](xref:Xamarin.Forms.Style) instance je možné přiřadit přímo [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnosti požadované vizuálních prvků a `ResourceDictionary` je možné odebrat, jak je znázorněno v následujícím Příklad kódu:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            ...
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            ...
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            ...
        };
        ...
        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelRedStyle },
                new Label { Text = "are demonstrating", Style = labelGreenStyle },
                new Label { Text = "explicit styles,", Style = labelBlueStyle },
                new Label { Text = "and an explicit style override", Style = labelBlueStyle,
                            TextColor = Color.Teal }
            }
        };
    }
}
```

Konstruktor definuje tři *explicitní* stylů, které se použijí na stránku [ `Label` ](xref:Xamarin.Forms.Label) instancí. Každý `Style` slouží k zobrazení textu v odlišnou barvou, při nastavování také písmo možnosti velikosti a vodorovné a svislé rozložení. Každý `Style` se použije na jiný `Label` nastavením jeho [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnosti. Kromě toho, koncový `Label` má `Style` použít, ale také přepisuje `TextColor` vlastnost na jiný `Color` hodnotu.

## <a name="summary"></a>Souhrn

A [ `Style` ](xref:Xamarin.Forms.Style) tvoří *explicitní* tím, že jeho deklarace `x:Key` atribut a potom selektivně jeho použití k ovládacím prvkům tak, že nastavíte jejich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnosti.



## <a name="related-links"></a>Související odkazy

- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Základní styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Práce se styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Styl](xref:Xamarin.Forms.Style)
- [Metoda setter](xref:Xamarin.Forms.Setter)
