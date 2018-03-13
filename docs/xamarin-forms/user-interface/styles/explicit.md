---
title: "Explicitní styly"
description: "Explicitní styl je ten, který je selektivně použít u ovládacích prvků nastavením své vlastnosti stylu."
ms.topic: article
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 1fbc12288527c053a24041aa6c49cc1a4abdde55
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="explicit-styles"></a>Explicitní styly

_Explicitní styl je ten, který je selektivně použít u ovládacích prvků nastavením své vlastnosti stylu._

## <a name="creating-an-explicit-style-in-xaml"></a>Vytváření explicitní styl v jazyce XAML

Deklarovat [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) na úrovni stránky [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) musí být přidaný do stránky a pak jeden nebo více `Style` můžou být součástí deklarace `ResourceDictionary`. A `Style` přišla *explicitní* tím, že jeho deklaraci `x:Key` atribut, který poskytuje v popisný klíč `ResourceDictionary`. *Explicitní* pak je nutné použít styly na konkrétní vizuální prvky nastavením jejich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnosti.

Následující příklad kódu ukazuje *explicitní* styly deklarované v jazyce XAML na stránce `ResourceDictionary` a použít na stránku [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instancí:

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

[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Definuje tři *explicitní* stylů, které se použijí na stránku [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance. Každý `Style` slouží k zobrazení textu v barvu, při velikosti a vodorovného a svislého rozložení možnosti také nastavení písma. Každý `Style` se použije na jiný `Label` nastavením jeho [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastností pomocí `StaticResource` – rozšíření značek. Výsledkem je vidět na následujících snímcích obrazovky vzhled:

[![](explicit-images/explicit-styles.png "Příklad explicitní styly")](explicit-images/explicit-styles-large.png#lightbox "příklad explicitní styly")

Kromě toho konečné [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) má [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) na něho použít, ale také přepsání [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) vlastnost, která má jiný `Color`hodnotu.

### <a name="creating-an-explicit-style-at-the-control-level"></a>Vytváření explicitní styl v ovládacím prvku úrovně

Kromě vytváření *explicitní* styly na úrovni stránky je lze také vytvořit na úrovni ovládacího prvku, jak je znázorněno v následujícím příkladu kódu:

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

V tomto příkladu *explicitní* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance jsou přiřazeny k [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekce [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) ovládacího prvku. Styly je pak použít ovládací prvek a jeho podřízených položek.

Informace o vytváření stylů v aplikace [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), najdete v části [globální styly](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-explicit-style-in-c35"></a>Vytváření explicitní styl v jazyce C&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance lze přidat na stránku [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekce v jazyce C# tak, že vytvoříte novou [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)a pak přidáním `Style` instance k `ResourceDictionary`, jak je znázorněno v Následující příklad kódu:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red  }
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
                new Label { Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

V konstruktoru definuje tři *explicitní* stylů, které se použijí na stránku [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance. Každý *explicitní* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) je přidán do [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) pomocí [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) metoda, zadání `key` řetězec, který má odkazovat `Style` instance. Každý `Style` se použije na jiný `Label` nastavením jejich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnosti.

Neexistuje však žádný výhodou používání [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) sem. Místo toho [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance může být přiřazen přímo na [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnosti požadované vizuální prvky a `ResourceDictionary` může být odebrán, jak je znázorněno v následujícím Příklad kódu:

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

V konstruktoru definuje tři *explicitní* stylů, které se použijí na stránku [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance. Každý `Style` slouží k zobrazení textu v barvu, při velikosti a vodorovného a svislého rozložení možnosti také nastavení písma. Každý `Style` se použije na jiný `Label` nastavením jeho [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnosti. Kromě toho konečné `Label` má `Style` na něho použít, ale také přepsání `TextColor` vlastnost na jiný `Color` hodnotu.

## <a name="summary"></a>Souhrn

A [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) přišla *explicitní* tím, že jeho deklaraci `x:Key` atributů a selektivně jeho použitím k ovládacím prvkům nastavením jejich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnosti.



## <a name="related-links"></a>Související odkazy

- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Základní styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Práce se styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metoda setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
