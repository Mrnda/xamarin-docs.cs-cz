---
title: "Řetězec formátování"
description: "Použití datových vazeb pro formátování a zobrazení objektů jako řetězce"
ms.topic: article
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 3b54ed876857f0cd04d7a304ff05b9710fbd9e04
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="string-formatting"></a>Řetězec formátování

Někdy je vhodnější použít datové vazby k zobrazení řetězcovou reprezentaci objektu nebo hodnota. Například můžete chtít použít `Label` zobrazit aktuální hodnoty `Slider`. V této vazbě dat `Slider` je zdroj a cíl `Text` vlastnost `Label`.

Při zobrazování řetězců v kódu, nástroj nejúčinnějších je statických [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) metoda. Formátovací řetězec obsahuje formátování kódy, které jsou specifické pro různé typy objektů, a může obsahovat další text společně s hodnoty, který je formátován. Najdete v článku [typy formátování v .NET](/dotnet/standard/base-types/formatting-types/) článku Další informace o formátování řetězce.

## <a name="the-stringformat-property"></a>StringFormat – vlastnost

Pokud tuto funkci se přenášejí do datové vazby: nastavíte [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) vlastnost `Binding` (nebo [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.StringFormat/) vlastnost `Binding` – rozšíření značek) pro formátování řetězce s jeden zástupný symbol standardní .NET:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

Všimněte si, že formátovací řetězec je oddělená jednoduchou uvozovku (apostrof) znaků, který má pomoci vyhnout práce složené závorky jako jiné XAML – rozšíření značek analyzátor jazyka XAML. Jinak, který řetězec bez jednoduchou uvozovku znak je do jednoho řetězce, které byste použili k zobrazení hodnoty s plovoucí desetinnou čárkou v volání `String.Format`. Formátování specifikaci `F2` způsobí, že hodnota zobrazí dvě desetinná místa.

`StringFormat` Vlastnost má smysl jenom, když je vlastnost cílového typu `string`, a režim vazba je `OneWay` nebo `TwoWay`. Pro obousměrné vazby `StringFormat` platí jenom pro předávání ze zdrojové do cílové hodnoty.

Jak uvidíte v článku na další na [cesta vazby](binding-path.md), vazby dat se může stát poměrně složitá a convoluted. Při ladění tyto vazby dat, můžete přidat `Label` do souboru XAML s `StringFormat` zobrazíte některé mezilehlých výsledků. I když ji použít pouze k zobrazení typ objektu, který může být užitečné.

**Formátování řetězce** stránky znázorňuje několik použití `StringFormat` vlastnost:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="DataBindingDemos.StringFormattingPage"
             Title="String Formatting">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Blue" />
                <Setter Property="HeightRequest" Value="2" />
                <Setter Property="Margin" Value="0, 5" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <Slider x:Name="slider" />
        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='The slider value is {0:F2}'}" />

        <BoxView />

        <TimePicker x:Name="timePicker" />
        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time,
                              StringFormat='The TimeSpan is {0:c}'}" />

        <BoxView />

        <Entry x:Name="entry" />
        <Label Text="{Binding Source={x:Reference entry},
                              Path=Text,
                              StringFormat='The Entry text is &quot;{0}&quot;'}" />

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:DateTime.Now}">
            <Label Text="{Binding}" />
            <Label Text="{Binding Path=Ticks,
                                  StringFormat='{0:N0} ticks since 1/1/1'}" />
            <Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
            <Label Text="{Binding StringFormat='The long date is {0:D}'}" />
        </StackLayout>

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:Math.PI}">
            <Label Text="{Binding}" />
            <Label Text="{Binding StringFormat='PI to 4 decimal points = {0:F4}'}" />
            <Label Text="{Binding StringFormat='PI in scientific notation = {0:E7}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Vazby na `Slider` a `TimePicker` zobrazit použití – specifikace formátu pro konkrétní `double` a `TimeSpan` datové typy. `StringFormat` Který zobrazí text z `Entry` zobrazení ukazuje, jak k určení dvojité uvozovky v řetězci formátování s použitím `&quot;` HTML entity.

Další oddíl v souboru XAML `StackLayout` s `BindingContext` nastavena na `x:Static` rozšíření značek, který odkazuje na statickou `DateTime.Now` vlastnost. První vazba nemá žádné vlastnosti:

```xaml
<Label Text="{Binding}" />
```

Jednoduše zobrazí `DateTime` hodnotu `BindingContext` s výchozí formátování. Zobrazí druhé vazby `Ticks` vlastnost `DateTime`, zatímco zobrazí dvě vazby `DateTime` s konkrétní formátování. Všimněte si to `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

Pokud potřebujete zobrazit levé nebo pravé složené závorky v formátovací řetězec, jednoduše použijte pár z nich.

Poslední část nastaví `BindingContext` na hodnotu `Math.PI` a zobrazí se výchozí formátování a dva různé typy formátování čísel.

Tady je programy spuštěné na všech tří platformách:

[![Řetězec formátování](string-formatting-images/stringformatting-small.png "řetězec formátování")](string-formatting-images/stringformatting-large.png "řetězec formátování")

## <a name="viewmodels-and-string-formatting"></a>ViewModels a formátování řetězce

Pokud používáte `Label` a `StringFormat` Chcete-li zobrazit hodnotu zobrazení, která je také cíl ViewModel, můžete buď definovat vazby ze zobrazení `Label` nebo z ViewModel k `Label`. Druhý postup je obecně osvědčených vzhledem k tomu, tak ověří, že vazby mezi zobrazením a ViewModel funguje.

Tento postup je uveden v **lepší výběr barvy** vzorku, který používá stejné ViewModel jako **jednoduchý selektor barva** program zobrazený v [ **vazby režimu** ](binding-mode.md) článku:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.BetterColorSelectorPage"
             Title="Better Color Selector">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Hue}" />
            <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

            <Slider Value="{Binding Saturation}" />
            <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

            <Slider Value="{Binding Luminosity}" />
            <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

Existují tři páry `Slider` a `Label` prvky, které jsou vázány na stejnou vlastnost v zdroje `HslColorViewModel` objektu. Jediným rozdílem je, že `Label` má `StringFormat` vlastnost zobrazíte všechny `Slider` hodnotu.

[![Lépe barvu selektor](string-formatting-images/bettercolorselector-small.png "lépe barvu selektor")](string-formatting-images/bettercolorselector-large.png "lépe barvu selektor")

Možná se ptáte, jak můžete zobrazit (červená, zelená, modré) hodnoty RGB v šestnáctkovém formátu tradiční dvou číslic. Tyto celočíselné hodnoty nejsou přímo dostupné z `Color` struktura. Jedno řešení by vypočítat celočíselné hodnoty součástí barev v rámci ViewModel a umístěte je jako vlastnosti. Může pak naformátovat pomocí `X2` formátování specifikace.

Další možností je další obecné: můžete napsat *převaděč hodnoty vazby* popsané v článku na novější [ **převodníky hodnot vazby**](converters.md).

Další článek, ale jsou zde popsány [ **cesta vazby** ](binding-path.md) v další podrobnosti a zobrazit, jak můžete použít k odkazování dílčí vlastností a položek v kolekcích.


## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Kapitola vazby dat z adresáře Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
