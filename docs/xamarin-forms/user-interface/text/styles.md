---
title: Styly
description: Styl textu v Xamarin.Forms
ms.topic: article
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: d1aadd808ea18023ee0a22259ddb36c0a8daa802
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/17/2018
---
# <a name="styles"></a>Styly

_Styl textu v Xamarin.Forms_


Styly lze upravit vzhled popisky, položek a editory. Styly můžete definovat jednou a používané mnoha zobrazení, ale styl lze použít pouze se zobrazeními jednoho typu.
Je možné přidělit styly `Key` a použije selektivně pomocí určitý ovládací prvek `Style` vlastnost.

Tento článek obsahuje následující témata:

- **[Předdefinované styly](#Built-In_Styles)**  &ndash; použít předdefinované stylů pro styl zobrazení založený na textu v celé vaší aplikace.
- **[Vlastní styly](#Custom_Styles)**  &ndash; při integrovaných možností nejsou dost definovat vlastní styly.
- **[Použití styly](#Applying_Styles)**  &ndash; používání vlastních a vestavěných stylů pro zobrazení.
- **[Usnadnění](#Accessibility)**  &ndash; Ujistěte se, že text respektuje nastavení usnadnění.

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>Předdefinované styly

Xamarin.Forms obsahuje několik [předdefinované](http://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) styly pro běžné scénáře:

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

Chcete-li použít jeden z předdefinovaných stylů, použijte `DynamicResource` – rozšíření značek k určení stylu:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

V jazyce C#, jsou předdefinované styly vybrané z `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![](styles-images/builtinstyles.png "Příklad styly zařízení")

<a name="Custom_Styles" />

## <a name="custom-styles"></a>Vlastní styly

Styly obsahovat setter a setter sestávají z vlastnosti a hodnoty vlastnosti bude nastavena pro.

V jazyce C# by vlastní styl popisku červeně velikosti 30 definovány takto:

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

V jazyce XAML:

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

Všimněte si, že prostředky (včetně všech styly) jsou definovány v rámci `ContentPage.Resources`, který je na stejné úrovni známější `ContentPage.Content` elementu.

![](styles-images/customstyle.png "Příklad vlastní styly")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>Aplikování stylů

Po vytvoření styl lze použít na všechny odpovídající zobrazení jeho `TargetType`.

V jazyce XAML, vlastní styly použité zobrazení zadáním jejich `Style` vlastnost s `StaticResource` odkazující na požadovaný styl – rozšíření značek:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

V jazyce C#, styly lze buď použitých přímo na zobrazení nebo přidat do a načítat z na stránce `ResourceDictionary`. Chcete-li přidat přímo:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

Přidat a získat ze stránky `ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

Předdefinované styly jsou jinak, použít, protože je nutné vyplnit nastavení usnadnění. Chcete-li použít předdefinované styly v jazyce XAML, `DynamicResource` – rozšíření značek se používá:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

V jazyce C#, jsou předdefinované styly vybrané z `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>Usnadnění

Vestavěné styly existovat usnadnění respektují Předvolby Usnadnění přístupu. Při použití některého z předdefinovaných stylů, velikosti písem bude automaticky zvýší, pokud uživatel nastaví jejich předvoleb usnadnění odpovídajícím způsobem.

Podívejte se na následující příklad stejné stránky zobrazení pomocí předdefinovaných stylů stylem nastavení usnadnění povolených a zakázaných:

Zakázáno:

![](styles-images/pre-access.png "Styly zařízení s usnadnění zakázáno")

Povoleno:

![](styles-images/post-access.png "Styly zařízení s usnadnění povoleno")

Pro zajištění přístupnosti, ujistěte se, že integrované styly jsou použity jako základ pro všechny související s textem styly v aplikaci a že styly používáte konzistentně. V tématu [styly](~/xamarin-forms/user-interface/styles/index.md) další podrobnosti o rozšíření a práce se styly obecně.


## <a name="related-links"></a>Související odkazy

- [Vytváření mobilních aplikací s Xamarin.Forms, kapitoly 12](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [Styly](~/xamarin-forms/user-interface/styles/index.md)
- [Text (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
