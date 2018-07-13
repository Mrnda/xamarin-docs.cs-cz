---
title: Styly zařízení v Xamarin.Forms
description: Xamarin.Forms obsahuje šest dynamické styly, označované jako styly zařízení ve třídě Device.Styles. Tento článek vysvětluje, jak používat styly zařízení v aplikaci Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: bba42c966c6a606790655751db8b294d9ca7b6f9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994374"
---
# <a name="device-styles-in-xamarinforms"></a>Styly zařízení v Xamarin.Forms

_Xamarin.Forms obsahuje šest dynamické styly, označované jako styly zařízení ve třídě Device.Styles._

*Zařízení* styly jsou:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)

Všech šest styly může používat jedině pro [ `Label` ](xref:Xamarin.Forms.Label) instancí. Například `Label` , který zobrazuje text odstavce může nastavit jeho [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnost [ `BodyStyle` ](xref:Xamarin.Forms.Device.Styles.BodyStyle).

Následující příklad kódu ukazuje použití *zařízení* styly stránky XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DeviceStylesPage" Title="Device" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="myBodyStyle" TargetType="Label"
              BaseResourceKey="BodyStyle">
                <Setter Property="TextColor" Value="Accent" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="Title style"
              Style="{DynamicResource TitleStyle}" />
            <Label Text="Subtitle text style"
              Style="{DynamicResource SubtitleStyle}" />
            <Label Text="Body style"
              Style="{DynamicResource BodyStyle}" />
            <Label Text="Caption style"
              Style="{DynamicResource CaptionStyle}" />
            <Label Text="List item detail text style"
              Style="{DynamicResource ListItemDetailTextStyle}" />
            <Label Text="List item text style"
              Style="{DynamicResource ListItemTextStyle}" />
            <Label Text="No style" />
            <Label Text="My body style"
              Style="{StaticResource myBodyStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Styly zařízení je vázána k použití `DynamicResource` – rozšíření značek. Dynamické povaze styly si můžete prohlédnout ve iOS tak, že změníte **usnadnění** nastavení pro velikost textu. Vzhled *zařízení* styly se liší na jednotlivých platformách, jak je znázorněno na následujících snímcích obrazovky:

![](device-images/device-styles.png "Styly zařízení na jednotlivých platformách")

*Zařízení* styly může být také odvozena z tak, že nastavíte [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) nastavte na název klíče pro styl zařízení. V příkladu výše `myBodyStyle` dědí z [ `BodyStyle` ](xref:Xamarin.Forms.Device.Styles.BodyStyle) a nastaví barvu textu s diakritikou. Další informace o dynamické styl dědičnosti, naleznete v tématu [dynamické dědičnost stylů](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance).

Následující příklad kódu ukazuje na stejnou stránku v jazyce C#:

```csharp
public class DeviceStylesPageCS : ContentPage
{
    public DeviceStylesPageCS ()
    {
        var myBodyStyle = new Style (typeof(Label)) {
            BaseResourceKey = Device.Styles.BodyStyleKey,
            Setters = {
                new Setter {
                    Property = Label.TextColorProperty,
                    Value = Color.Accent
                }
            }
        };

        Title = "Device";
        Icon = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label { Text = "Title style", Style = Device.Styles.TitleStyle },
                new Label { Text = "Subtitle style", Style = Device.Styles.SubtitleStyle },
                new Label { Text = "Body style", Style = Device.Styles.BodyStyle },
                new Label { Text = "Caption style", Style = Device.Styles.CaptionStyle },
                new Label { Text = "List item detail text style",
                  Style = Device.Styles.ListItemDetailTextStyle },
                new Label { Text = "List item text style", Style = Device.Styles.ListItemTextStyle },
                new Label { Text = "No style" },
                new Label { Text = "My body style", Style = myBodyStyle }
            }
        };
    }
}
```

[ `Style` ](xref:Xamarin.Forms.VisualElement.Style) Vlastnosti každého [ `Label` ](xref:Xamarin.Forms.Label) instance je nastavena na odpovídající vlastnosti z [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) třídy.

## <a name="accessibility"></a>Usnadnění

*Zařízení* styly respektovat Předvolby Usnadnění přístupu, tak velikosti písma se změní podle předvolby usnadnění přístupu se změní na jednotlivých platformách. Proto se na podporu přístupný text, ujistěte se, že *zařízení* styly jsou použity jako základ pro všechny styly textu v rámci vaší aplikace.

Na následujících snímcích obrazovky ukazují styly zařízení na jednotlivých platformách, s nejmenší velikost dostupné písma:

[![](device-images/minimum-size.png "Styly zařízení přístupné malé na jednotlivých platformách")](device-images/minimum-size-large.png#lightbox "styly přístupné malé zařízení na jednotlivých platformách")

Na následujících snímcích obrazovky ukazují styly zařízení na jednotlivých platformách, s největší velikost dostupné písma:

![](device-images/maximum-size.png "Styly zařízení přístupné velké na jednotlivých platformách")

## <a name="summary"></a>Souhrn

Xamarin.Forms obsahuje šest *dynamické* styly, označované jako *zařízení* styly, v [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) třídy. Všech šest styly může používat jedině pro [ `Label` ](xref:Xamarin.Forms.Label) instancí.


## <a name="related-links"></a>Související odkazy

- [Styly textu](~/xamarin-forms/user-interface/text/styles.md)
- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dynamické styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Práce se styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Styl](xref:Xamarin.Forms.Style)
- [Metoda setter](xref:Xamarin.Forms.Setter)
