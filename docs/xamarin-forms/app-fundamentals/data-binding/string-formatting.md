---
title: Formátování řetězců Xamarin.Forms
description: Tento článek vysvětluje, jak pomocí Xamarin.FOrms datové vazby pro formátování a zobrazení objektů jako řetězce. Toho dosáhnete nastavením StringFormat vazby na standardní formátovací řetězec .NET s zástupný symbol.
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: a876e81c67b6ec61a2cb29143cb001a7d6160032
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998144"
---
# <a name="xamarinforms-string-formatting"></a>Formátování řetězců Xamarin.Forms

Někdy je vhodné použít datové vazby k zobrazení řetězcová reprezentace objektu nebo hodnoty. Například můžete chtít použít `Label` zobrazíte aktuální hodnotu `Slider`. V této vazbě dat `Slider` je zdrojem a cílem `Text` vlastnost `Label`.

Při zobrazování řetězců v kódu, nástroj procesorově nejvýkonnější je statické [ `String.Format` ](xref:System.String.Format(System.String,System.Object)) metody. Formátovací řetězec obsahuje formátování specifické pro různé typy objektů kódy a může obsahovat další text spolu s hodnotami, který je formátován. Zobrazit [formátovací typy v .NET](/dotnet/standard/base-types/formatting-types/) najdete další informace o formátování řetězce.

## <a name="the-stringformat-property"></a>Vlastnost StringFormat

Je tato zařízení přenesou do datové vazby: můžete nastavit [ `StringFormat` ](xref:Xamarin.Forms.BindingBase.StringFormat) vlastnost `Binding` (nebo [ `StringFormat` ](xref:Xamarin.Forms.Xaml.BindingExtension.StringFormat) vlastnost `Binding` – rozšíření značek) do Standardní formátovací řetězec s jeden zástupný symbol .NET:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

Všimněte si, že formátovací řetězec je oddělen složenými znaky uvozovek (apostrof) umožňující analyzátoru XAML, vyhněte se zpracuje složené závorky jako další rozšíření značek XAML. Jinak, který řetězec bez uvozovek znak, který je stejný řetězec, můžete použít k zobrazení hodnoty s plovoucí desetinnou čárkou ve volání `String.Format`. Formátování specifikace `F2` způsobí, že hodnota má být zobrazen na dvě desetinná místa.

`StringFormat` Vlastnost má smysl jenom když je vlastnost cílového typu `string`, a režim vazby je `OneWay` nebo `TwoWay`. Pro obousměrné vazby `StringFormat` jde použít jenom pro předávání ze zdrojové do cílové hodnoty.

Jak uvidíte v následujícím článku na [cesta vazby](binding-path.md), datové vazby může být poměrně složité a složitými. Při ladění tyto vazby dat, můžete přidat `Label` do souboru XAML s `StringFormat` zobrazíte některé mezilehlých výsledků. I když ho používáte jenom k zobrazení typ objektu, který může být užitečné.

**Formátování řetězce** stránce ukazuje několik použití `StringFormat` vlastnost:

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

Vazby na `Slider` a `TimePicker` zobrazit konkrétní k použití specifikace formátu `double` a `TimeSpan` datové typy. `StringFormat` , Který zobrazí text z `Entry` zobrazení ukazuje, jak vyplnit formátovací řetězec s použitím dvojitých uvozovek `&quot;` entita HTML.

V následující části najdete v souboru XAML `StackLayout` s `BindingContext` nastavena na `x:Static` – rozšíření značek, který odkazuje na statickou `DateTime.Now` vlastnost. První vazba nemá žádné vlastnosti:

```xaml
<Label Text="{Binding}" />
```

Jednoduše zobrazí `DateTime` hodnotu `BindingContext` s výchozím nastavením. Zobrazí druhé vazby `Ticks` vlastnost `DateTime`, zatímco zobrazují dvě vazby `DateTime` s konkrétní formátování. Všimněte si, že to `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

Pokud potřebujete zobrazit levou nebo pravou složenou složené závorky v formátovací řetězec, jednoduše použijte pár z nich.

Poslední část sady `BindingContext` hodnotě `Math.PI` a zobrazí se výchozí formátování a dva různé typy formátování čísel.

Tady je program spuštěn na všech třech platformách:

[![Řetězce formátování](string-formatting-images/stringformatting-small.png "řetězce formátování")](string-formatting-images/stringformatting-large.png#lightbox "řetězce formátování")

## <a name="viewmodels-and-string-formatting"></a>Řetězec, formátování a modely ViewModels

Když používáte `Label` a `StringFormat` k zobrazení hodnoty zobrazení, která je také cíl ViewModel, můžete buď definovat vazby ze zobrazení tak, aby `Label` nebo z ViewModel k `Label`. Obecně platí druhý postup je nejlepší, jelikož ověřuje, že vazby mezi zobrazením a ViewModel fungují.

Tento přístup je zobrazena ve **lepší výběr barvy** vzorku, který používá stejné ViewModel jako **jednoduchý výběr barvy** programu zobrazeného [ **vazby režimu** ](binding-mode.md) článku:

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

Nyní existují tři páry `Slider` a `Label` prvky, které jsou vázány na stejné source – vlastnost v `HslColorViewModel` objektu. Jediným rozdílem je, že `Label` má `StringFormat` vlastnost pro zobrazení jednotlivých `Slider` hodnotu.

[![Lepší barva selektor](string-formatting-images/bettercolorselector-small.png "lépe barva selektor")](string-formatting-images/bettercolorselector-large.png#lightbox "lépe barva selektor")

Možná se ptáte, jak můžete zobrazit (červená, zelená, modrá) hodnoty RGB v šestnáctkovém formátu tradiční dvěma číslicemi. Nejsou přímo dostupné z těchto celočíselných hodnot `Color` struktury. Jedním řešením může být pro výpočet celočíselné hodnoty barevných složek v rámci ViewModel a vystavit jako vlastnosti. Může pak naformátovat pomocí `X2` formátování specifikace.

Další možností je obecnější: můžete napsat *převaděč hodnoty vazby* jak je popsáno v pozdější článku [ **převaděče hodnot vazeb**](converters.md).

Další článek, ale prozkoumává [ **cesta vazby** ](binding-path.md) ve více podrobností a jeho použití k odkazování na dílčí vlastnosti a položky v kolekci.


## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Data vazby kapitola z knihy Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
