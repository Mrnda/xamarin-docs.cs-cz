---
title: Globální styly v Xamarin.Forms
description: Styly může být k dispozici globálně jejich přidáním do slovníku prostředků aplikace. To pomáhá vyhnout duplikaci styly napříč stránek a ovládacích prvků.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: e7b2a37b868ea03ca626ffd2dcddb006a235b0cc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995398"
---
# <a name="global-styles-in-xamarinforms"></a>Globální styly v Xamarin.Forms

_Styly může být k dispozici globálně jejich přidáním do slovníku prostředků aplikace. To pomáhá vyhnout duplikaci styly napříč stránek a ovládacích prvků._

## <a name="creating-a-global-style-in-xaml"></a>Vytvoření globální stylu v XAML

Ve výchozím nastavení, všechny aplikace Xamarin.Forms, které jsou vytvořené ze šablony pomocí **aplikace** třídu pro implementaci [ `Application` ](xref:Xamarin.Forms.Application) podtřídy. Chcete-li deklarovat [ `Style` ](xref:Xamarin.Forms.Style) na úrovni aplikace v aplikačním [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) pomocí XAML, výchozí **aplikace** třídy je nutné nahradit XAML **Aplikace** třídy a přidružený kód na pozadí. Další informace najdete v tématu [práce s třídu aplikace](~/xamarin-forms/app-fundamentals/application-class.md).

Následující příklad kódu ukazuje [ `Style` ](xref:Xamarin.Forms.Style) deklarované na úrovni aplikace:

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

To [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) definuje jedinou *explicitní* styl, `buttonStyle`, který se používá k nastavení vzhledu [ `Button` ](xref:Xamarin.Forms.Button) instancí. Však může být globální styly *explicitní* nebo *implicitní*.

Následující příklad kódu ukazuje použití stránky XAML `buttonStyle` na stránku [ `Button` ](xref:Xamarin.Forms.Button) instancí:

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

Výsledkem je vzhled je znázorněno na následujících snímcích obrazovky:

[![](application-images/application-styles-1.png "Globální styly příklad")](application-images/application-styles-1-large.png#lightbox "příklad globální styly")

Informace o vytváření styly na stránce [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), naleznete v tématu [explicitní styly](~/xamarin-forms/user-interface/styles/explicit.md) a [implicitní styly](~/xamarin-forms/user-interface/styles/implicit.md).

### <a name="overriding-styles"></a>Přepsání styly

Styly níže v hierarchii zobrazení přednost před těmi definovanými vyšší nahoru. Například nastavení [ `Style` ](xref:Xamarin.Forms.Style) , který nastavuje [ `Button.TextColor` ](xref:Xamarin.Forms.Button.TextColor) k `Red` na úrovni aplikace, úroveň budou přepsaná identifikátorem, který nastavuje styl úrovni stránky `Button.TextColor` k `Green`. Obdobně styl úrovni stránky, budou přepsaná identifikátorem úrovně stylu ovládacího prvku. Kromě toho pokud `Button.TextColor` je nastavena přímo na vlastnosti ovládacího prvku, to bude mít prioritu před všechny styly. Tato priorita je znázorněn v následujícím příkladu kódu:

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

Původní `buttonStyle`, definované na úrovni aplikace, je přepsán `buttonStyle` instance definované na úrovni stránky. Kromě toho stylu úrovni stránky je přepsán úroveň řízení `buttonStyle`. Proto [ `Button` ](xref:Xamarin.Forms.Button) instance se zobrazí se modrý text, jak je znázorněno na následujících snímcích obrazovky:

[![](application-images/application-styles-2.png "Přepsání styly příklad")](application-images/application-styles-2-large.png#lightbox "přepsání příklad styly")

## <a name="creating-a-global-style-in-c35"></a>Vytvoření globální stylu v jazyce C&#35;

[`Style`](xref:Xamarin.Forms.Style) instance lze přidat do vaší aplikace [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) kolekce v jazyce C# vytvořit nový [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)a potom přidáním `Style` instance na `ResourceDictionary`, jako můžete vidět v následujícím příkladu kódu:

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

Konstruktor definuje jedinou *explicitní* styl pro použití na [ `Button` ](xref:Xamarin.Forms.Button) instancí v celé aplikaci. *Explicitní* [ `Style` ](xref:Xamarin.Forms.Style) instance jsou přidány do [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) pomocí [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) metody zadávání `key`řetězec k odkazování `Style` instance. `Style` Instance lze pak použít na žádné ovládací prvky správného typu v aplikaci. Však může být globální styly *explicitní* nebo *implicitní*.

Následující příklad kódu ukazuje C# stránku použití `buttonStyle` na stránku [ `Button` ](xref:Xamarin.Forms.Button) instancí:

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

`buttonStyle` Platí pro [ `Button` ](xref:Xamarin.Forms.Button) instance nastavením jejich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnosti a řídí vzhled `Button` instancí.

## <a name="summary"></a>Souhrn

Styly může být k dispozici globálně jejich přidáním do vaší aplikace [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). To pomáhá vyhnout duplikaci styly napříč stránek a ovládacích prvků.



## <a name="related-links"></a>Související odkazy

- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Základní styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Práce se styly (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Styl](xref:Xamarin.Forms.Style)
- [Metoda setter](xref:Xamarin.Forms.Setter)
