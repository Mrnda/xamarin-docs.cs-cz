---
title: Styly textu Xamarin.Forms
description: Tento článek vysvětluje, jak pro používání stylů pro text v aplikacích Xamarin.Forms. Styly lze definovat jednou a používané mnoha zobrazení, ale styl jde použít jenom se zobrazeními jednoho typu.
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 73aa3115e92d1e3954f5ae3eb8dcb84abf9d9efb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998765"
---
# <a name="xamarinforms-text-styles"></a>Styly textu Xamarin.Forms

_Používání stylů pro text v Xamarin.Forms_

Styly lze použít k úpravě vzhledu popisků, položky a editory. Styly lze definovat jednou a používané mnoha zobrazení, ale styl jde použít jenom se zobrazeními jednoho typu.
Styly může dostat `Key` a použije selektivně pomocí určitý ovládací prvek `Style` vlastnost.

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>Předdefinované styly

Xamarin.Forms obsahuje několik [integrované](xref:Xamarin.Forms.Device.Styles) stylů pro běžné scénáře:

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

Chcete-li použít jednu z předdefinovaných styly, použijte `DynamicResource` – rozšíření značek k určení stylu:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

V jazyce C#, jsou předdefinované styly vybrány z `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![](styles-images/builtinstyles.png "Příklad styly zařízení")

<a name="Custom_Styles" />

## <a name="custom-styles"></a>Vlastní styly

Styly skládat z metody setter a setter se skládají z vlastnosti a hodnoty vlastností se nastaví na.

V jazyce C# by vlastní styl popisku červeně velikost 30 definovaná následujícím způsobem:

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

V XAML:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style x:Key="LabelStyle" TargetType="Label">
            <Setter Property="TextColor" Value="Red"/>
            <Setter Property="FontSize" Value="30"/>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<ContentPage.Content>
    <StackLayout>
        <Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
    </StackLayout>
</ContentPage.Content>
```

Všimněte si, že prostředky (včetně všechny styly) jsou definovány v rámci `ContentPage.Resources`, což je na stejné úrovni více do hloubky `ContentPage.Content` elementu.

![](styles-images/customstyle.png "Příklad vlastní styly")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>Aplikování stylů

Po vytvoření stylu lze použít na libovolné zobrazení odpovídající jeho `TargetType`.

V XAML, vlastní styly použijí na zobrazení zadáním jejich `Style` vlastnost s `StaticResource` odkazování na požadovaný styl – rozšíření značek:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

V jazyce C#, styly můžete buď použít přímo pro zobrazení nebo přidávat a získaných na stránce `ResourceDictionary`. Chcete-li přidat přímo:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

Přidat a získat ze stránky na `ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

Předdefinované styly jsou jinak, použít, protože je nutné odpovědět na nastavení usnadnění. Chcete-li použít předdefinované styly v XAML, `DynamicResource` – rozšíření značek se používá:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

V jazyce C#, jsou předdefinované styly vybrány z `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>Usnadnění

Předdefinované styly existovat usnadnit dodržovat Předvolby Usnadnění přístupu. Pokud používáte některý z předdefinovaných styly, velikosti písma automaticky zvýší Pokud uživatel nastaví své předvolby usnadnění odpovídajícím způsobem.

Vezměte v úvahu následující příklad provaz zobrazení stylem nastavení usnadnění aktivovaných a deaktivovaných předdefinované styly:

Zakázáno:

![](styles-images/pre-access.png "Styly zařízení se zakázanou usnadnění přístupu")

Povoleno:

![](styles-images/post-access.png "Styly zařízení s přístupností povoleno")

Pro zajištění přístupnosti, ujistěte se, že předdefinované styly jsou použity jako základ pro všechny styly textu v rámci vaší aplikace a musíte používat styly konzistentně. Zobrazit [styly](~/xamarin-forms/user-interface/styles/index.md) podrobné informace o rozšíření a práce se styly obecně.


## <a name="related-links"></a>Související odkazy

- [Vytváření mobilních aplikací s Xamarin.Forms, kapitole 12](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [Styly](~/xamarin-forms/user-interface/styles/index.md)
- [Text (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Styl](xref:Xamarin.Forms.Style)
