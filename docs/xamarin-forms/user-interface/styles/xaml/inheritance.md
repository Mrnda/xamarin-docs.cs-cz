---
title: Dědičnost stylů v Xamarin.Forms
description: Styly může dědit z jiné styly snížit duplikace a umožňují opakované použití. Tento článek vysvětluje, jak provádět dědičnost stylů aplikace Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: f8cf3287c6d713d91a0217bd30ca2ee927534aea
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995330"
---
# <a name="style-inheritance-in-xamarinforms"></a>Dědičnost stylů v Xamarin.Forms

_Styly může dědit z jiné styly snížit duplikace a umožňují opakované použití._

## <a name="style-inheritance-in-xaml"></a>Dědičnost stylů v XAML

Dědičnost stylů se provádí nastavením [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) vlastnost do existujícího [ `Style` ](xref:Xamarin.Forms.Style). V XAML, toho dosáhnete pomocí nastavení `BasedOn` vlastnost `StaticResource` – rozšíření značek, který odkazuje na dříve vytvořené `Style`. V jazyce C#, toho dosáhnete pomocí nastavení `BasedOn` vlastnost `Style` instance.

Styly, které dědí ze základní stylu mohou zahrnovat [ `Setter` ](xref:Xamarin.Forms.Setter) instance pro nové vlastnosti, nebo je používat k přepsání styly ze základní stylu. Kromě toho styly, které dědí ze základní stylu musí cílit na stejný typ nebo typ, který je odvozen z typu cílem základní style. Například, pokud základní styl cíle [ `View` ](xref:Xamarin.Forms.View) instancí, styly, které jsou založeny na základní styl můžete cílit na `View` instance nebo typy, které jsou odvozeny z `View` třídy, jako například [ `Label` ](xref:Xamarin.Forms.Label) a [ `Button` ](xref:Xamarin.Forms.Button) instancí.

Následující kód ukazuje *explicitní* stylu dědičnosti v stránky XAML:

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

`baseStyle` Cíle [ `View` ](xref:Xamarin.Forms.View) instance a nastaví [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) a [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastnosti. `baseStyle` Není nastavena přímo na žádné ovládací prvky. Místo toho `labelStyle` a `buttonStyle` , dědí nastavení hodnot další vlastnost s vazbou. `labelStyle` a `buttonStyle` použijí se [ `Label` ](xref:Xamarin.Forms.Label) instancí a [ `Button` ](xref:Xamarin.Forms.Button) instanci, nastavením jejich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnosti. Výsledkem je vzhled je znázorněno na následujících snímcích obrazovky:

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> Implicitní styl může být odvozena z explicitní styl, ale explicitní styl nelze odvodit z implicitní styl.

### <a name="respecting-the-inheritance-chain"></a>Respektování řetězu dědičnosti

Styl může dědit jedině z styly na stejné úrovni nebo výše v hierarchii zobrazení. To znamená, že:

- Prostředek na úrovni aplikace může dědit pouze z jiných prostředků úrovni aplikace.
- Prostředek úrovni stránky může dědit z úrovně prostředků aplikace a dalších prostředků úrovni stránky.
- Prostředků s úrovní řízení může zdědit z úrovně prostředků aplikace, prostředky na úrovni stránky a dalších prostředků úrovně ovládacího prvku.

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

V tomto příkladu `labelStyle` a `buttonStyle` se ovládací prvek úrovni prostředků, zatímco `baseStyle` je prostředek úrovni stránky. Nicméně Přestože `labelStyle` a `buttonStyle` dědit z `baseStyle`, není možné pro `baseStyle` dědit z `labelStyle` nebo `buttonStyle`z důvodu jejich odpovídajících umístěních v zobrazení hierarchie.

## <a name="style-inheritance-in-c35"></a>Dědičnost stylů v jazyce C&#35;

Ekvivalentní C# stránku, kde [ `Style` ](xref:Xamarin.Forms.Style) jsou instance přiřazeny přímo [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnosti požadovaných kontrolních mechanismů, je znázorněno v následujícím příkladu kódu:

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

`baseStyle` Cíle [ `View` ](xref:Xamarin.Forms.View) instance a nastaví [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) a [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastnosti. `baseStyle` Není nastavena přímo na žádné ovládací prvky. Místo toho `labelStyle` a `buttonStyle` , dědí nastavení hodnot další vlastnost s vazbou. `labelStyle` a `buttonStyle` použijí se [ `Label` ](xref:Xamarin.Forms.Label) instancí a [ `Button` ](xref:Xamarin.Forms.Button) instanci, nastavením jejich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnosti.

## <a name="summary"></a>Souhrn

Styly může dědit z jiné styly snížit duplikace a umožňují opakované použití. Dědičnost stylů se provádí nastavením [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) vlastnost do existujícího [ `Style` ](xref:Xamarin.Forms.Style).


## <a name="related-links"></a>Související odkazy

- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Základní styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Práce se styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Styl](xref:Xamarin.Forms.Style)
- [Metoda setter](xref:Xamarin.Forms.Setter)
