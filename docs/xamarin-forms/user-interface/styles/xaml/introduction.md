---
title: Úvod do styly
description: Styly povolit vzhled vizuální prvky k přizpůsobení. Styly jsou definovány pro konkrétní typ a obsahovat hodnoty pro vlastnosti dostupné u tohoto typu.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: f878641f9266da74d61eda8a85d4e16de193be0e
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847963"
---
# <a name="introduction-to-styles"></a>Úvod do styly

_Styly povolit vzhled vizuální prvky k přizpůsobení. Styly jsou definovány pro konkrétní typ a obsahovat hodnoty pro vlastnosti dostupné u tohoto typu._

Xamarin.Forms aplikace často obsahují více ovládacích prvků, které mají stejné vzhled. Aplikace může mít například více [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instancí, které mají stejné možnosti písma a rozložení možnosti, jak je znázorněno v následujícím příkladu kódu XAML:

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

Následující příklad kódu ukazuje vytvořené v C# ekvivalentní stránky:

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

Každý [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance má stejné vlastnosti hodnoty pro řízení vzhledu textu zobrazovaného `Label`. Výsledkem je vidět na následujících snímcích obrazovky vzhled:

[![](introduction-images/no-styles.png "Označení vzhled bez styly")](introduction-images/no-styles-large.png#lightbox "popisku vzhled bez styly")

Může být nastavení vzhled jednotlivých ovládacích prvků opakovaných a chyba náchylné k chybám. Místo toho styl umožňuje vytvořit, která definuje vzhled a potom se použijí na požadovaných ovládacích prvků.

## <a name="creating-a-style"></a>Vytváření styl

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Třída skupiny kolekce hodnot vlastností do jednoho objektu, který lze použít na více instancí vizuální prvek. To pomáhá snížit opakovaných značek a umožňuje snadno změnit vzhled aplikace.

I když styly byly navrženy především pro aplikace založené na jazyce XAML, můžete se také vytvořit v jazyku C#:

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instancích vytvořených v jazyce XAML, které jsou obvykle definovány v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) přiřazené [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekce ovládacího prvku stránky, nebo [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) kolekce aplikace.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance vytvořené v C# jsou obvykle definovány v třídy stránky, nebo třídu, která je přístupná globálně.

Výběr místo definice [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) ovlivňuje, kde je možné:

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance definované na úrovni řízení lze použít pouze pro ovládací prvek a jeho podřízených objektů.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance definované na úrovni stránky lze použít pouze na stránku a jeho podřízených objektů.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance definované na úrovni aplikace je možné použít v celé aplikaci.

Každý [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance obsahuje kolekce jednoho či více [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) objekty s jednotlivými `Setter` s [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) a [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/). `Property` Je název vazbu vlastnosti elementu, který je použit, a `Value` je hodnota, která se použije pro vlastnost.

Každý [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance může být *explicitní*, nebo *implicitní*:

- *Explicitní* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instanci je definován zadáním [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) a `x:Key` hodnotu a pomocí nastavení cílového elementu [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnost, která má `x:Key` odkaz. Další informace o *explicitní* styly, najdete v části [explicitní styly](~/xamarin-forms/user-interface/styles/explicit.md).
- *Implicitní* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instanci je definován zadáním pouze [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/). `Style` Instance se pak automaticky použijí pro všechny elementy daného typu. Poznámka: Tento měly podtřídy `TargetType` nemají automaticky `Style` použít. Další informace o *implicitní* styly, najdete v části [implicitní styly](~/xamarin-forms/user-interface/styles/implicit.md).

Při vytváření [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/), [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) vždy je požadována vlastnost. Následující příklad kódu ukazuje *explicitní* styl (Poznámka: `x:Key`) vytvořit v jazyce XAML:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Použít `Style`, cílový objekt musí být [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) odpovídající [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) hodnota vlastnosti `Style`, jak je znázorněno v následujícím příkladu kódu XAML:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Styly níže v hierarchii zobrazení mají přednost před nastavením definovaným vyšší nahoru. Například nastavení [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) , který nastavuje [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) k `Red` na aplikaci bude přepsáno úroveň úrovně styl stránky, která nastavuje `Label.TextColor` k `Green`. Podobně bude přepsáno úrovně styl stránky úrovně stylu ovládacího prvku. Kromě toho pokud `Label.TextColor` není nastaven, přímo na vlastnost ovládacího prvku to má přednost před všechny styly.

Články v této části ukazují a popisují, jak vytvořit a použít *explicitní* a *implicitní* styly, jak vytvořit globální styly, styl dědičnosti, jak reagovat na styl změny v době běhu a jak používat v vytvořená stylů součástí Xamarin.Forms.

> [!NOTE]
> **Co je StyleId?**
>
> Před Xamarin.Forms 2.2 [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) vlastnost se používá k určení jednotlivých elementů v aplikaci pro identifikaci v testování uživatelského rozhraní a v motivu stroje například Pixate. Však obsahuje zavedla Xamarin.Forms 2.2 [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) vlastností, která se má nahradit [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) vlastnost. Další informace najdete v tématu [automatizovat Xamarin.Forms testování s Xamarin.UITest a testovací Cloud](~/xamarin-forms/deploy-test/uitest-and-test-cloud.md).

## <a name="summary"></a>Souhrn

Xamarin.Forms aplikace často obsahují více ovládacích prvků, které mají stejné vzhled. Může být nastavení vzhled jednotlivých ovládacích prvků opakovaných a chyba náchylné k chybám. Místo toho styly lze vytvořit které přizpůsobení vzhledu ovládacího prvku pomocí seskupení a nastavení vlastnosti, které jsou k dispozici na typ ovládacího prvku.


## <a name="related-links"></a>Související odkazy

- [Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Styl](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Metoda setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
