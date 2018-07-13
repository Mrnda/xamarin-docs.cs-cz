---
title: Úvod do Xamarin.Forms styly
description: Styly povolit vzhled vizuální prvky, které lze přizpůsobit. Styly jsou definovány pro konkrétní typ a obsahují hodnoty pro vlastnosti dostupné u tohoto typu.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 8f84c960f17f56fce2a1bba143a215ce930f6f4e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996106"
---
# <a name="introduction-to-xamarinforms-styles"></a>Úvod do Xamarin.Forms styly

_Styly povolit vzhled vizuální prvky, které lze přizpůsobit. Styly jsou definovány pro konkrétní typ a obsahují hodnoty pro vlastnosti dostupné u tohoto typu._

Aplikace Xamarin.Forms často obsahují více ovládacích prvků, které mají stejné vzhled. Například aplikace může mít více [ `Label` ](xref:Xamarin.Forms.Label) instancí, které mají stejné možnosti písma a možnosti rozložení, jak je znázorněno v následujícím příkladu kódu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Následující příklad kódu ukazuje na stejnou stránku vytvořené v jazyce C#:

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        Icon = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

Každý [ `Label` ](xref:Xamarin.Forms.Label) instance má stejné vlastnosti hodnoty pro řízení vzhledu text, zobrazený `Label`. Výsledkem je vzhled je znázorněno na následujících snímcích obrazovky:

[![](introduction-images/no-styles.png "Popisek vzhled bez styly")](introduction-images/no-styles-large.png#lightbox "popisek vzhled bez styly")

Nastavení vzhledu jednotlivých ovládacích prvků může být opakované a náchylné k chybám. Místo toho styl lze vytvořit, která definuje vzhled a použijí se požadovaných kontrolních mechanismů.

## <a name="creating-a-style"></a>Vytvoření stylu

[ `Style` ](xref:Xamarin.Forms.Style) Skupiny tříd kolekce hodnot vlastností do jednoho objektu, který lze následně použít na několik instancí vizuální prvek. To pomáhá omezit opakované značek a umožňuje snadněji změnit vzhled aplikace.

I když se styly byly navrženy hlavně pro aplikace založené na XAML, je lze také vytvořit v jazyce C#:

- [`Style`](xref:Xamarin.Forms.Style) instance vytvořené v XAML jsou obvykle definovány v [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) přiřazené [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) kolekci ovládacího prvku, stránky, nebo [ `Resources` ](xref:Xamarin.Forms.Application.Resources) kolekce aplikace.
- [`Style`](xref:Xamarin.Forms.Style) instance vytvořené v jazyce C# jsou obvykle definovány ve třídě na stránce nebo ve třídě, která je přístupná globálně.

Volba místo definice [ `Style` ](xref:Xamarin.Forms.Style) dopady, kde je možné:

- [`Style`](xref:Xamarin.Forms.Style) instancí, které jsou definované na úrovni ovládací prvek lze použít pouze do ovládacího prvku a jeho podřízené položky.
- [`Style`](xref:Xamarin.Forms.Style) instance, které jsou definované na úrovni stránky lze použít pouze na stránku a na jeho podřízené položky.
- [`Style`](xref:Xamarin.Forms.Style) instance, které jsou definované na úrovni aplikace je možné použít v celé aplikaci.

Každý [ `Style` ](xref:Xamarin.Forms.Style) instance obsahuje kolekce jednoho nebo více [ `Setter` ](xref:Xamarin.Forms.Setter) objekty, s každým `Setter` s [ `Property` ](xref:Xamarin.Forms.Setter.Property) a [`Value`](xref:Xamarin.Forms.Setter.Value). `Property` Je název umožňujících vazbu vlastnosti elementu, který je styl použit, a `Value` je hodnota, která se použije na vlastnost.

Každý [ `Style` ](xref:Xamarin.Forms.Style) instance může být *explicitní*, nebo *implicitní*:

- *Explicitní* [ `Style` ](xref:Xamarin.Forms.Style) instanci je definován tak, že zadáte [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) a `x:Key` hodnotu a nastavením elementu target [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnost `x:Key` odkaz. Další informace o *explicitní* stylů, najdete v článku [explicitní styly](~/xamarin-forms/user-interface/styles/explicit.md).
- *Implicitní* [ `Style` ](xref:Xamarin.Forms.Style) instanci je definován tak, že zadáte jenom [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType). `Style` Instance se pak automaticky použijí na všechny elementy tohoto typu. Poznámka: Tento podtřídy třídy `TargetType` nemají automaticky `Style` použít. Další informace o *implicitní* stylů, najdete v článku [implicitní styly](~/xamarin-forms/user-interface/styles/implicit.md).

Při vytváření [ `Style` ](xref:Xamarin.Forms.Style), [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) vlastnost je vždy vyžadován. Následující příklad kódu ukazuje *explicitní* styl (Poznámka: `x:Key`) vytvořené v XAML:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Použít `Style`, cílový objekt musí být [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) , který odpovídá [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) hodnotu vlastnosti `Style`, jak je znázorněno v následujícím příkladu kódu XAML:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Styly níže v hierarchii zobrazení přednost před těmi definovanými vyšší nahoru. Například nastavení [ `Style` ](xref:Xamarin.Forms.Style) , který nastavuje [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) k `Red` na úrovni aplikace, úroveň budou přepsaná identifikátorem, který nastavuje styl úrovni stránky `Label.TextColor` k `Green`. Obdobně styl úrovni stránky, budou přepsaná identifikátorem úrovně stylu ovládacího prvku. Kromě toho pokud `Label.TextColor` je nastavena přímo na vlastnosti ovládacího prvku, to má přednost před všechny styly.

Články v této části ukazují a vysvětluje, jak vytvořit a použít *explicitní* a *implicitní* styly, vytvoření globální styly, stylu dědičnosti, jak reagovat na změny stylu v době běhu a jak pomocí integrovaného styly součástí Xamarin.Forms.

> [!NOTE]
> **Co je StyleId?**
>
> Před Xamarin.Forms 2.2 [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) vlastnost se používá k identifikaci jednotlivých prvků v aplikaci pro identifikaci při testování uživatelského rozhraní a v motivu modulů, jako je Pixate. Ale zavedla Xamarin.Forms 2.2 [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId) vlastnost, která má platnost nahrazených [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) vlastnost. Další informace najdete v tématu [Xamarin.Forms automatizovat testování pomocí Xamarin.UITest a Test Cloud](~/xamarin-forms/deploy-test/uitest-and-test-cloud.md).

## <a name="summary"></a>Souhrn

Aplikace Xamarin.Forms často obsahují více ovládacích prvků, které mají stejné vzhled. Nastavení vzhledu jednotlivých ovládacích prvků může být opakované a náchylné k chybám. Místo toho styly lze vytvořit, které přizpůsobení vzhledu ovládacího prvku tak, že seskupování a nastavení vlastností dostupných pro typ ovládacího prvku.


## <a name="related-links"></a>Související odkazy

- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Styl](xref:Xamarin.Forms.Style)
- [Metoda setter](xref:Xamarin.Forms.Setter)
