---
title: Slovnících prostředků
description: XAML prostředky jsou definice objektů, které může být použit více než jednou. ResourceDictionary umožňuje prostředky definované na jednom místě, a znovu použít v celé aplikaci Xamarin.Forms. Tento článek vysvětluje, jak vytvářet a využívat ResourceDictionary a způsob sloučení slovnících prostředků.
ms.topic: article
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/17/2017
ms.openlocfilehash: aa3ae9fed67b6cd7521e5c59edcb54f05cc6b7c5
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/29/2018
---
# <a name="resource-dictionaries"></a>Slovnících prostředků

_XAML prostředky jsou definice objektů, které může být použit více než jednou. ResourceDictionary umožňuje prostředky definované na jednom místě, a znovu použít v celé aplikaci Xamarin.Forms. Tento článek vysvětluje, jak vytvářet a využívat ResourceDictionary a způsob sloučení slovnících prostředků._

## <a name="overview"></a>Přehled

A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) je úložiště pro prostředky, které jsou používány k aplikaci Xamarin.Forms. Typické prostředky, které jsou uložené v `ResourceDictionary` zahrnují [styly](~/xamarin-forms/user-interface/styles/index.md), [řízení šablony](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [dat šablony](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), barvy a převaděče.

V jazyce XAML, prostředky jsou definovány v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) a načíst a použít na elementy pomocí `StaticResource` – rozšíření značek. V jazyce C# zdroje jsou definovány v `ResourceDictionary` a načíst a použít na elementy pomocí indexeru se na základě řetězce. Je však jen málo výhod pomocí `ResourceDictionary` v jazyce C#, jako prostředky lze snadno přímo přiřadit k vlastnosti vizuální prvky bez nutnosti nejprve je ze načíst `ResourceDictionary`.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Vytvoření a použití ResourceDictionary

Prostředky lze definovat v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) připojená k [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) kolekce stránka nebo ovládací prvek, nebo do [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) kolekce aplikace. Výběr místo definice [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) ovlivňuje, kde je možné:

- Prostředky v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definované v ovládacím prvku úrovně jde použít jenom pro ovládací prvek a jeho podřízených objektů.
- Prostředky v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definované na stránce úrovně jde použít jenom na stránku a jeho podřízených objektů.
- Prostředky v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definované v aplikaci úroveň lze použít v celé aplikaci.

Následující příklad kódu XAML ukazuje prostředky, které jsou definované úrovni aplikace [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Application ...>
    <Application.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Yellow</Color>
            <Color x:Key="HeadingTextColor">Black</Color>
            <Color x:Key="NormalTextColor">Blue</Color>
            <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
                <Setter Property="FontAttributes" Value="Bold" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

To [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definuje tři [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) prostředky a [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) prostředků. Další informace o vytváření XAML `App` třídy najdete v tématu [– třída aplikace](~/xamarin-forms/app-fundamentals/application-class.md).

Každý prostředek má klíč, který je zadán pomocí `x:Key` atribut, který poskytuje v popisný klíč `ResourceDictionary`. Klíč se používá k načtení prostředku z [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) pomocí `StaticResource` – rozšíření značek, jak je ukázáno v následujícím příkladu kódu XAML, který ukazuje další prostředky, které jsou definované v ovládacím prvku úrovně `ResourceDictionary`:

```xaml
<StackLayout Margin="0,20,0,0">
  <StackLayout.Resources>
    <ResourceDictionary>
      <Style x:Key="LabelNormalStyle" TargetType="Label">
        <Setter Property="TextColor" Value="{StaticResource NormalTextColor}" />
      </Style>
      <Style x:Key="MediumBoldText" TargetType="Button">
        <Setter Property="FontSize" Value="Medium" />
        <Setter Property="FontAttributes" Value="Bold" />
      </Style>
    </ResourceDictionary>
  </StackLayout.Resources>
  <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
    <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
           Margin="10,20,10,0"
           Style="{StaticResource LabelNormalStyle}" />
    <Button Text="Navigate"
            Clicked="OnNavigateButtonClicked"
            TextColor="{StaticResource NormalTextColor}"
            Margin="0,20,0,0"
            HorizontalOptions="Center"
            Style="{StaticResource MediumBoldText}" />
</StackLayout>
```

První [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance načte a zpracuje `LabelPageHeadingStyle` prostředků, které jsou definované na úrovni aplikace [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), s druhou `Label` instance načítání a využívají `LabelNormalStyle` prostředků, které jsou definované na úrovni řízení `ResourceDictionary`. Podobně [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instance načte a zpracuje `NormalTextColor` prostředků, které jsou definované na úrovni aplikace `ResourceDictionary`a `MediumBoldText` prostředků, které jsou definované na úrovni řízení `ResourceDictionary`. Výsledkem je vidět na následujících snímcích obrazovky vzhled:

[![](resource-dictionaries-images/screenshots-sml.png "Využívání prostředků ResourceDictionary")](resource-dictionaries-images/screenshots.png#lightbox "spotřebovávat ResourceDictionary prostředků")

> [!NOTE]
> Prostředky, které jsou specifické pro jednu stránku by neměly být obsažené v aplikaci prostředků s úrovní slovníku, jako například prostředků bude analyzovat poté při spuštění aplikace místo, pokud to vyžaduje na stránce. Další informace najdete v tématu [snížit velikost slovník prostředků aplikace](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Přepsání prostředky

Když [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) prostředky sdílet `x:Key` hodnoty atributu, prostředky nižší definované v hierarchii zobrazení bude mít prioritu před nastavením definovaným vyšší nahoru. Například nastavení `PageBackgroundColor` prostředek `Blue` na aplikaci bude přepsáno úroveň úrovně stránky `PageBackgroundColor` prostředků nastavit na `Yellow`. Podobně úrovně stránky `PageBackgroundColor` prostředků bude přepsáno úroveň kontroly `PageBackgroundColor` prostředků. Následující příklad kódu XAML ukazují tuto prioritu:

```xaml
<ContentPage ... BackgroundColor="{StaticResource PageBackgroundColor}">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Blue</Color>
            <Color x:Key="NormalTextColor">Yellow</Color>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="0,20,0,0">
        ...
        <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
               Margin="10,20,10,0"
               Style="{StaticResource LabelNormalStyle}" />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked"
                TextColor="{StaticResource NormalTextColor}"
                Margin="0,20,0,0"
                HorizontalOptions="Center"
                Style="{StaticResource MediumBoldText}" />
    </StackLayout>
</ContentPage>
```

Původní `PageBackgroundColor` a `NormalTextColor` instance, které jsou definované na úrovni aplikace, jsou přepsány `PageBackgroundColor` a `NormalTextColor` instance definované na úrovni stránky. Proto se stane modrou barvu pozadí stránky a text na této stránce se změní na žlutý, jak je předvedeno na následujících snímcích obrazovky:

[![](resource-dictionaries-images/overridding-screenshots-sml.png "Přepsání ResourceDictionary prostředky")](resource-dictionaries-images/overridding-screenshots.png#lightbox "přepsání ResourceDictionary prostředky")

Všimněte si však, že na pozadí panelu [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) stále žlutý, protože [ `BarBackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/) je nastavena na hodnotu `PageBackgroundColor` prostředků, které jsou definované v aplikaci úroveň [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/).

## <a name="merged-resource-dictionaries"></a>Slovníky sloučených prostředků

Slovnících prostředků sloučené kombinovat jeden nebo více [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) instance do jiné. To se provádí nastavením `ResourceDictionary.MergedDictionaries` vlastnost, která má jeden nebo více slovnících prostředků, které budou sloučena do aplikace, stránka nebo ovládací prvek úroveň `ResourceDictionary`.

> [!IMPORTANT]
> [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Typ má také [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) vlastnost, která slouží k sloučení jedné `ResourceDictionary` do jiné, s `ResourceDictionary` zadaný jako hodnota `MergedWith`vlastnost slučování do aktuální `ResourceDictionary` instance. Při slučování prostřednictvím `MergedWith` vlastnost, všechny prostředky v aktuální `ResourceDictionary` tuto sdílenou složku `x:Key` hodnoty s prostředky v atributu `ResourceDictionary` slučované, budou nahrazeny. Ale `MergedWith` přestanou vlastností v budoucí verzi Xamarin.Forms. Proto se doporučuje použít `MergedDictionaries` vlastnost sloučit `ResourceDictionary` instance.

Následující příklad ukazuje kód XAML [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) s názvem `MyResourceDictionary` obsahující jediný zdroj:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ResourceDictionaryDemo.MyResourceDictionary">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}" TextColor="{StaticResource NormalTextColor}" FontAttributes="Bold" />
                <Label Grid.Column="1" Text="{Binding Age}" TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2" Text="{Binding Location}" TextColor="{StaticResource NormalTextColor}" HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

`MyResourceDictionary` mohou být sloučeny do aplikací, stránka nebo ovládací prvek úroveň `ResourceDictionary`. Následující příklad kódu XAML ukazuje ho slučování do úrovně stránky `ResourceDictionary` pomocí `MergedDictionaries` vlastnost:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Při slučování [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) prostředky sdílet stejné `x:Key` hodnoty atributu, Xamarin.Forms používá následující prioritu prostředků:

1. Prostředky místní do slovníku prostředků.
1. Prostředky obsažené v slovník prostředků, který byl sloučit prostřednictvím [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) vlastnost.
1. Prostředky obsažené ve slovnících prostředků, které byly slučovány prostřednictvím `MergedDictionaries` kolekce, v pořadí uvedeném v `MergedDictionaries` vlastnost.

> [!NOTE]
> Hledání slovnících prostředků může být výpočetně náročné úlohy, pokud aplikace obsahuje více slovnících velké prostředků. Proto zajistěte, aby každé stránce v aplikaci jenom používala slovnících prostředků, které jsou vhodné na stránku, aby se zabránilo zbytečným vyhledávání.

## <a name="summary"></a>Souhrn

Jak vytvářet a využívat popsané v tomto článku [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)a způsob sloučení slovnících prostředků. A `ResourceDictionary` umožňuje prostředky definované na jednom místě, a znovu použít v celé aplikaci Xamarin.Forms.


## <a name="related-links"></a>Související odkazy

- [Slovnících prostředků (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Styly](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
