---
title: Globální styly
description: Styly může být dostupné globálně jejich přidáním do slovníku prostředků aplikace. To pomáhá zamezit duplicitních stylů stránky a ovládací prvky.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 219973e26c5ee25accec57f1bebbd1753391e6de
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847901"
---
# <a name="global-styles"></a>Globální styly

_Styly může být dostupné globálně jejich přidáním do slovníku prostředků aplikace. To pomáhá zamezit duplicitních stylů stránky a ovládací prvky._

## <a name="creating-a-global-style-in-xaml"></a>Vytvoření globální styl v jazyce XAML

Ve výchozím nastavení, použijte všechny Xamarin.Forms aplikace vytvořené ze šablony **aplikace** třídu pro implementaci [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) podtřídy. Deklarovat [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) na úrovni aplikace, do aplikace [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) pomocí XAML, výchozí **aplikace** třídy je nutné nahradit XAML **Aplikace** třídy a přidružené kódu. Další informace najdete v tématu [práce – třída aplikace](~/xamarin-forms/app-fundamentals/application-class.md).

Následující příklad kódu ukazuje [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) deklarovat na úrovni aplikace:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.App">
    <Application.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BorderColor" Value="Lime" />
                <Setter Property="BorderRadius" Value="5" />
                <Setter Property="BorderWidth" Value="5" />
                <Setter Property="WidthRequest" Value="200" />
                <Setter Property="TextColor" Value="Teal" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

To [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definuje jeden *explicitní* styl, `buttonStyle`, který se používá k nastavení vzhledu [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instance. Však může být globální styly *explicitní* nebo *implicitní*.

Následující příklad kódu ukazuje použití stránky XAML `buttonStyle` na stránku [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instancí:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Výsledkem je vidět na následujících snímcích obrazovky vzhled:

[![](application-images/application-styles-1.png "Příklad globální styly")](application-images/application-styles-1-large.png#lightbox "příklad globální styly")

Informace o vytváření stylů na stránce [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), najdete v části [explicitní styly](~/xamarin-forms/user-interface/styles/explicit.md) a [implicitní styly](~/xamarin-forms/user-interface/styles/implicit.md).

### <a name="overriding-styles"></a>Přepsání styly

Styly níže v hierarchii zobrazení mají přednost před nastavením definovaným vyšší nahoru. Například nastavení [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) , který nastavuje [ `Button.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/) k `Red` na aplikaci bude přepsáno úroveň úrovně styl stránky, která nastavuje `Button.TextColor` k `Green`. Podobně bude přepsáno úrovně styl stránky úrovně stylu ovládacího prvku. Kromě toho pokud `Button.TextColor` není nastaven, přímo na vlastnost ovládacího prvku to bude mít prioritu před všechny styly. Následující příklad kódu ukazují tuto prioritu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                ...
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="buttonStyle" TargetType="Button">
                        ...
                        <Setter Property="TextColor" Value="Blue" />
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Původní `buttonStyle`, definované na úrovni aplikace, přepíše se `buttonStyle` instance definované na úrovni stránky. Kromě toho přepsat úroveň řízení úrovně styl stránky `buttonStyle`. Proto [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instance jsou zobrazeny s modrý text, jak je vidět na následujících snímcích obrazovky:

[![](application-images/application-styles-2.png "Přepsání styly příklad")](application-images/application-styles-2-large.png#lightbox "přepsání příklad styly")

## <a name="creating-a-global-style-in-c35"></a>Vytvoření globální styl v jazyce C&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance lze přidat do aplikace [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekce v jazyce C# tak, že vytvoříte novou [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)a pak přidáním `Style` instance k `ResourceDictionary`, jako Zobrazuje následující příklad kódu:

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,    Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

V konstruktoru definuje jeden *explicitní* styl pro použití na [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instancí v celé aplikaci. *Explicitní* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance jsou přidány do [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) pomocí [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) metoda, zadání `key`řetězec, který má odkazovat `Style` instance. `Style` Instance potom mohou být použity pro všechny ovládací prvky v aplikaci správného typu. Však může být globální styly *explicitní* nebo *implicitní*.

Následující příklad kódu ukazuje, C# stránky použití `buttonStyle` na stránku [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instancí:

```csharp
public class ApplicationStylesPageCS : ContentPage
{
    public ApplicationStylesPageCS ()
    {
        ...
        Content = new StackLayout {
            Children = {
                new Button { Text = "These buttons", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "are demonstrating", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "application styles", Style = (Style)Application.Current.Resources ["buttonStyle"]
                }
            }
        };
    }
}
```

`buttonStyle` Se použije na [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instance nastavením jejich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnosti a řídí vzhled `Button` instance.

## <a name="summary"></a>Souhrn

Styly může být k dispozici globálně jejich přidáním do aplikace [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). To pomáhá zamezit duplicitních stylů stránky a ovládací prvky.



## <a name="related-links"></a>Související odkazy

- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Základní styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Práce se styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metoda setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
