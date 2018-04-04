---
title: Styly zařízení
description: Xamarin.Forms obsahuje šest dynamické styly, označované jako styly zařízení ve třídě Device.Styles.
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: c52de431faa8e6d17b281ece87c862a49d30e886
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="device-styles"></a>Styly zařízení

_Xamarin.Forms obsahuje šest dynamické styly, označované jako styly zařízení ve třídě Device.Styles._

*Zařízení* styly jsou:

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)

Všech šest styly lze použít pouze k [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance. Například `Label` , že je zobrazení textu odstavce může nastavit jeho [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnost [ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/).

Následující příklad kódu ukazuje, jak pomocí *zařízení* stylů na stránce XAML:

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

Styly zařízení je vázána k použití `DynamicResource` – rozšíření značek. Dynamické povaha stylů se zobrazí v iOS změnou **usnadnění** nastavení pro velikost textu. Vzhled *zařízení* styly se na jednotlivých platformách liší, jak je vidět na následujících snímcích obrazovky:

![](device-images/device-styles.png "Styly zařízení na jednotlivých platformách")

*Zařízení* styly může být také odvozen od nastavením [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) vlastnost na název klíče pro styl zařízení. V příkladu výše `myBodyStyle` dědí z [ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/) a nastaví barvu textu s diakritikou. Další informace o dědičnosti dynamické styl najdete v tématu [dynamické dědičnosti styl](~/xamarin-forms/user-interface/styles/dynamic.md#dynamic-style-inheritance).

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

[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Vlastnost jednotlivých [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance je nastaven na příslušnou vlastnost z [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) třídy.

## <a name="accessibility"></a>Usnadnění

*Zařízení* styly respektují usnadnění předvolby, takže velikosti písem budou měnit, protože jsou na jednotlivých platformách změnit předvolby usnadnění přístupu. Proto pro podporu přístupný text, zajistěte, aby *zařízení* styly jsou použity jako základ pro všechny styly textu v rámci vaší aplikace.

Tyto snímky obrazovky ukazují styly zařízení na každou platformu, s nejnižší velikost písma dostupné:

[![](device-images/minimum-size.png "Styly přístupné malé zařízení na jednotlivých platformách")](device-images/minimum-size-large.png#lightbox "přístupné malé zařízení stylů pro každou platformu")

Tyto snímky obrazovky ukazují styly zařízení na každou platformu, s největší velikost písma dostupné:

![](device-images/maximum-size.png "Styly přístupné velké zařízení na jednotlivých platformách")

## <a name="summary"></a>Souhrn

Xamarin.Forms obsahuje šest *dynamické* styly, označované jako *zařízení* styly v [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) třídy. Všech šest styly lze použít pouze k [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance.


## <a name="related-links"></a>Související odkazy

- [Styly textu](~/xamarin-forms/user-interface/text/styles.md)
- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dynamické styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Práce se styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metoda setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
