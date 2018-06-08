---
title: Styl dědičnosti
description: Styly může dědit vlastnosti z jiných styly snížit duplikování a povolit opakované použití.
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 7b489e90daec2659a6d11b2776731582bdf368ff
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848418"
---
# <a name="style-inheritance"></a>Styl dědičnosti

_Styly může dědit vlastnosti z jiných styly snížit duplikování a povolit opakované použití._

## <a name="style-inheritance-in-xaml"></a>Styl dědičnosti v jazyce XAML

Styl dědičnosti se provádí nastavením [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) vlastnost, která má existující [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/). V jazyce XAML, toho se dosáhne nastavení `BasedOn` vlastnosti `StaticResource` rozšíření značek, který odkazuje na dříve vytvořenou `Style`. V jazyce C#, toho se dosáhne nastavení `BasedOn` vlastnosti `Style` instance.

Styly, které dědí od základní styl může zahrnovat [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) instancí nové vlastnosti, nebo je používat k přepsání styly ze základní styl. Kromě toho stylů, které dědí od základního stylu musí být stejného typu nebo typ, který je odvozen od typu cílem základní styl. Například, pokud základní styl cíle [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) instancí, styly, které jsou založeny na základní styl můžete cílit na `View` instance nebo typy, které jsou odvozeny od `View` třídy, jako například [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) a [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instance.

Následující kód ukazuje *explicitní* styl dědičnosti na stránce XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
            </Style>
            <Style x:Key="labelStyle" TargetType="Label"
                   BasedOn="{StaticResource baseStyle}">
                ...
                <Setter Property="TextColor" Value="Teal" />
            </Style>
            <Style x:Key="buttonStyle" TargetType="Button"
                   BasedOn="{StaticResource baseStyle}">
                <Setter Property="BorderColor" Value="Lime" />
                ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelStyle}" />
            ...
            <Button Text="So is the button"
                    Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`baseStyle` Cíle [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) instance a nastaví [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) a [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastnosti. `baseStyle` Není nastaven přímo na všech ovládacích prvků. Místo toho `labelStyle` a `buttonStyle` , dědí nastavení hodnoty další vazbu vlastnosti. `labelStyle` a `buttonStyle` jsou poté použity [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instancí a [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instance, a to nastavením jejich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnosti. Výsledkem je vidět na následujících snímcích obrazovky vzhled:

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> Implicitní styl může být odvozen od explicitní styl, ale explicitní styl nemůže být odvozen od implicitní stylu.

### <a name="respecting-the-inheritance-chain"></a>Respektováním řetězu dědičnosti

Styl může dědit vlastnosti pouze z stylů na stejné úrovni nebo vyšší, v hierarchii zobrazení. To znamená, že:

- Prostředek na úrovni aplikace může dědit vlastnosti pouze z jiné úrovně prostředky aplikace.
- Úrovně prostředek stránky může dědit vlastnosti z úrovně prostředky aplikace a další prostředky úrovni stránky.
- Řízení úrovně prostředku může dědit vlastnosti z úrovně prostředky aplikace, prostředky na úrovni stránky a dalším prostředkům úrovni ovládacího prvku.

Tento řetězec dědičnosti je znázorněn v následujícím příkladu kódu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelStyle" TargetType="Label" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                    <Style x:Key="buttonStyle" TargetType="Button" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

V tomto příkladu `labelStyle` a `buttonStyle` prostředků řízení úrovně, zatímco `baseStyle` je úrovně prostředek stránky. Ale při `labelStyle` a `buttonStyle` dědí `baseStyle`, není možné pro `baseStyle` dědění z `labelStyle` nebo `buttonStyle`, kvůli jejich odpovídajících umístění v hierarchii zobrazení.

## <a name="style-inheritance-in-c35"></a>Styl dědičnosti v jazyce C&#35;

Ekvivalentní C# stránky, kde [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance přiřazené přímo na [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnosti požadované ovládacích prvků, je znázorněno v následujícím příkladu kódu:

```csharp
public class StyleInheritancePageCS : ContentPage
{
    public StyleInheritancePageCS ()
    {
        var baseStyle = new Style (typeof(View)) {
            Setters = {
                new Setter {
                    Property = View.HorizontalOptionsProperty, Value = LayoutOptions.Center    },
                ...
            }
        };

        var labelStyle = new Style (typeof(Label)) {
            BasedOn = baseStyle,
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Teal    }
            }
        };

        var buttonStyle = new Style (typeof(Button)) {
            BasedOn = baseStyle,
            Setters = {
                new Setter { Property = Button.BorderColorProperty, Value =    Color.Lime },
                ...
            }
        };
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelStyle },
                ...
                new Button { Text = "So is the button", Style = buttonStyle }
            }
        };
    }
}
```

`baseStyle` Cíle [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) instance a nastaví [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) a [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastnosti. `baseStyle` Není nastaven přímo na všech ovládacích prvků. Místo toho `labelStyle` a `buttonStyle` , dědí nastavení hodnoty další vazbu vlastnosti. `labelStyle` a `buttonStyle` jsou poté použity [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instancí a [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instance, a to nastavením jejich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnosti.

## <a name="summary"></a>Souhrn

Styly může dědit vlastnosti z jiných styly snížit duplikování a povolit opakované použití. Styl dědičnosti se provádí nastavením [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) vlastnost, která má existující [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/).


## <a name="related-links"></a>Související odkazy

- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Základní styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Práce se styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metoda setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
