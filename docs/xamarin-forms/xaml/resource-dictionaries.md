---
title: Slovnících prostředků
description: XAML prostředky jsou objekty, které můžete sdílet a znovu použít v celé aplikaci Xamarin.Forms.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/02/2018
ms.openlocfilehash: ee3e4c984072fc019fe3719aab650a44d3899911
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="resource-dictionaries"></a>Slovnících prostředků

_Definice objektů, které můžete sdílet a znovu použít v celé aplikaci Xamarin.Forms jsou prostředky XAML._

Tyto objekty prostředků jsou uloženy ve slovníku prostředků. Tento článek popisuje, jak vytvářet a využívat slovník prostředků a způsob sloučení slovnících prostředků.

## <a name="overview"></a>Přehled

A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) je úložiště pro prostředky, které jsou používány k aplikaci Xamarin.Forms. Typické prostředky, které jsou uložené v `ResourceDictionary` zahrnují [styly](~/xamarin-forms/user-interface/styles/index.md), [řízení šablony](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [dat šablony](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), barvy a převaděče.

V jazyce XAML, prostředky, které jsou uložené v `ResourceDictionary` lze poté načíst a použít na elementy pomocí `StaticResource` – rozšíření značek. V jazyce C#, může být také definováno prostředky v `ResourceDictionary` a načíst a použít na elementy pomocí indexeru se na základě řetězce. Je však jen málo výhod pomocí `ResourceDictionary` v jazyce C#, jako sdílené objekty můžete jednoduše ukládat jako pole nebo vlastnosti a získat přístup přímo bez nutnosti první je načtou ze slovníku.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Vytvoření a použití ResourceDictionary

Prostředky jsou definovány v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) který je pak nastaven na jednu z následujících `Resources` vlastnosti:

- [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) Vlastnosti třídy, která je odvozena z [`Application`](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)
- [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) Vlastnosti třídy, která je odvozena z ['VisualElement.](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)

Xamarin.Forms program obsahuje pouze jednu třídu, která je odvozena z `Application` , ale často využívá mnoho tříd, které jsou odvozeny od `VisualElement`, včetně stránky, rozložení a ovládací prvky. Každý z těchto objektů může mít jeho `Resources` vlastnost nastavena na hodnotu `ResourceDictionary`. Volba umístění konkrétní `ResourceDictionary` ovlivňuje, kde je možné prostředky:

- Prostředky v `ResourceDictionary` připojená k zobrazení, jako `Button` nebo `Label` lze použít pouze pro daný objekt, tak toto není velmi užitečné.
- Prostředky v `ResourceDictionary` připojené k rozložení, jako `StackLayout` nebo `Grid` lze použít k rozložení a všechny podřízené objekty tohoto rozložení. 
- Prostředky v `ResourceDictionary` definované na stránce úroveň provádět na stránku a všechny její podřízené položky.
- Prostředky v `ResourceDictionary` definované v aplikaci úroveň lze použít v celé aplikaci.

Následující XAML zobrazuje prostředky, které jsou definované úrovni aplikace `ResourceDictionary` v **App.xaml** soubor vytvořen jako součást standardní program Xamarin.Forms:

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

To `ResourceDictionary` definuje tři [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) prostředky a [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) prostředků. Další informace o `App` třídy najdete v tématu [– třída aplikace](~/xamarin-forms/app-fundamentals/application-class.md).

Počínaje Xamarin.Forms 3.0, explicitní `ResourceDictionary` značky nejsou potřeba. `ResourceDictionary` Objekt se vytvoří automaticky a můžete vložit přímo mezi prostředky `Resources` element vlastnosti značky:

```xaml
<Application ...>
    <Application.Resources>
        <Color x:Key="PageBackgroundColor">Yellow</Color>
        <Color x:Key="HeadingTextColor">Black</Color>
        <Color x:Key="NormalTextColor">Blue</Color>
        <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
            <Setter Property="FontAttributes" Value="Bold" />
            <Setter Property="HorizontalOptions" Value="Center" />
            <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
        </Style>
    </Application.Resources>
</Application>
```

Každý prostředek má klíč, který je zadán pomocí `x:Key` atribut, který se stane klíče slovníku v `ResourceDictionary`. Klíč se používá k načtení prostředku z `ResourceDictionary` podle [ `StaticResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/) – rozšíření značek, jak je ukázáno v následujícím příkladu kódu XAML, který ukazuje další prostředky, které jsou definované v rámci `StackLayout`:

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

První [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance načte a zpracuje `LabelPageHeadingStyle` prostředků, které jsou definované na úrovni aplikace `ResourceDictionary`, s druhou `Label` instance načítání a využívají `LabelNormalStyle`prostředků, které jsou definované na úrovni řízení `ResourceDictionary`. Podobně [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instance načte a zpracuje `NormalTextColor` prostředků, které jsou definované na úrovni aplikace `ResourceDictionary`a `MediumBoldText` prostředků, které jsou definované na úrovni řízení `ResourceDictionary`. Výsledkem je vidět na následujících snímcích obrazovky vzhled:

[![](resource-dictionaries-images/screenshots-sml.png "Využívání prostředků ResourceDictionary")](resource-dictionaries-images/screenshots.png#lightbox "spotřebovávat ResourceDictionary prostředků")

> [!NOTE]
> Prostředky, které jsou specifické pro jednu stránku by neměly být obsažené v aplikaci prostředků s úrovní slovníku, jako například prostředků bude analyzovat poté při spuštění aplikace místo, pokud to vyžaduje na stránce. Další informace najdete v tématu [snížit velikost slovník prostředků aplikace](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Přepsání prostředky

Když `ResourceDictionary` prostředky sdílet `x:Key` hodnoty atributu, prostředky nižší definované v hierarchii zobrazení bude mít prioritu před nastavením definovaným vyšší nahoru. Například nastavení `PageBackgroundColor` prostředek `Blue` na aplikaci bude přepsáno úroveň úrovně stránky `PageBackgroundColor` prostředků nastavit na `Yellow`. Podobně úrovně stránky `PageBackgroundColor` prostředků bude přepsáno úroveň kontroly `PageBackgroundColor` prostředků. Následující příklad kódu XAML ukazují tuto prioritu:

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

Všimněte si však, že na pozadí panelu [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) stále žlutý, protože [ `BarBackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/) je nastavena na hodnotu `PageBackgroundColor` prostředků, které jsou definované v aplikaci úroveň `ResourceDictionary`.

Zde je další způsob myslet `ResourceDictionary` přednost před: při XAML analyzátor narazí `StaticResource`, vyhledá odpovídající klíč pomocí procházející vizuální strojové struktuře, použití na první shodu najde. Pokud toto hledání končí na stránce a klíč ještě nebyl nalezen, analyzátor XAML vyhledá `ResourceDictionary` připojené k `App` objektu. Pokud ještě není nalezen klíč, je vyvolána výjimka.

## <a name="stand-alone-resource-dictionaries"></a>Slovnících samostatné prostředků

Třída odvozená z `ResourceDictionary` může být také v samostatném samostatného souboru. (Přesněji řečeno, třídy odvozené od `ResourceDictionary` obvykle vyžaduje _pár_ souborů, protože prostředky jsou definovány v souboru XAML, ale soubor kódu se `InitializeComponent` volání je také nutné.) Výsledný soubor může být sdílen pak aplikace.

Pokud chcete vytvořit takový soubor, přidejte nový **zobrazení obsahu** nebo **obsahu stránce** položku do projektu (ale ne **zobrazení obsahu** nebo **obsahu stránce** s jenom soubor jazyka C#). V souboru XAML i v souboru C#, změňte název základní třídy z `ContentView` nebo `ContentPage` k `ResourceDictionary`. Název základní třídy v souboru XAML je element nejvyšší úrovně.

Následující příklad ukazuje XAML [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) s názvem `MyResourceDictionary`:

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

To `ResourceDictionary` obsahuje jediný zdroj, který je objektem typu `DataTemplate`.

Můžete vytvořit instanci `MyResourceDictionary` vložením mezi dvěma `Resources` element vlastnosti značky, například v `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>  
```

Instance `MyResourceDictionary` je nastaven na `Resources` vlastnost `ContentPage` objektu.

Tento přístup má však určitá omezení: `Resources` vlastnost `ContentPage` odkazuje na pouze tento jeden `ResourceDictionary`. Ve většině případů chcete zahrnout další `ResourceDictionary` instance a případně dalších prostředků také.

Tato úloha vyžaduje slovnících sloučené prostředků.

## <a name="merged-resource-dictionaries"></a>Slovníky sloučených prostředků

Slovnících prostředků sloučené kombinovat jeden nebo více `ResourceDictionary` do jiné instance `ResourceDictionary`. To provedete v souboru XAML nastavením [ `MergedDictionaries` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedDictionaries/) vlastnost, která má jeden nebo více slovnících prostředků, které budou sloučena do aplikace, stránka nebo ovládací prvek úroveň `ResourceDictionary`.

> [!IMPORTANT]
> `ResourceDictionary` také definuje [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) vlastnost. Nepoužívejte tuto vlastnost; se už nepoužívá od Xamarin.Forms 3.0.

A instanci `MyResourceDictionary` by se daly sloučit do aplikací, stránka nebo ovládací prvek úroveň `ResourceDictionary`. Následující příklad kódu XAML ukazuje ho slučování do úrovně stránky `ResourceDictionary` pomocí `MergedDictionaries` vlastnost:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Tento kód ukazuje pouze instance `MyResourceDictionary` se přidává do `ResourceDictionary` ale můžete taky odkazovat navzájem `ResourceDictionary` instancí v rámci `MergedDictionary` element vlastnosti značek a dalším prostředkům mimo tyto značky:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary.MergedDictionaries>

                <!-- Add more resource dictionaries here -->

                <local:MyResourceDictionary />

                <!-- Add more resource dictionaries here -->

            </ResourceDictionary.MergedDictionaries>

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Může být jen jedna `MergedDictionaries` tématu `ResourceDictionary`, ale můžete dávat tolik `ResourceDictionary` instance zde tak, jak chcete.

Při slučování [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) prostředky sdílet stejné `x:Key` hodnoty atributu, Xamarin.Forms používá následující prioritu prostředků:

1. Prostředky místní do slovníku prostředků.
1. Prostředky obsažené v slovník prostředků, který byl sloučit prostřednictvím nepoužívané [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) vlastnost.
1. Prostředky obsažené ve slovnících prostředků, které byly slučovány prostřednictvím `MergedDictionaries` kolekce, v pořadí uvedeném v `MergedDictionaries` vlastnost.

> [!NOTE]
> Hledání slovnících prostředků může být výpočetně náročné úlohy, pokud aplikace obsahuje více slovnících velké prostředků. Proto aby se zabránilo zbytečným vyhledávání, by měla zajistit, aby každé stránce v aplikaci pouze používal slovnících prostředků, které jsou vhodné na stránku.

## <a name="merging-dictionaries-in-xamarinforms-30"></a>Slučování slovníků v Xamarin.Forms 3.0

Od verze 3.0 Xamarin.Forms, proces sloučení `ResourceDictionaries` se stal poněkud snadnější a flexibilnější. `MergedDictionaries` Již nejsou potřebné značky element vlastnosti. Místo toho můžete přidat do slovníku prostředků jiného `ResourceDictionary` značky s novým [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.Source/) vlastnost nastavena na název souboru XAML s prostředky:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary Source="MyResourceDictionary.xaml" />

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Protože Xamarin.Forms 3.0 automaticky vytvoří `ResourceDictionary`, tyto dvě vnější `ResourceDictionary` značky se už nevyžadují:

```xaml
<ContentPage ...>
    <ContentPage.Resources>

        <!-- Add more resources here -->

        <ResourceDictionary Source="MyResourceDictionary.xaml" />

        <!-- Add more resources here -->

    </ContentPage.Resources>
    ...
</ContentPage>
```

Toto nové syntaxe nemá _není_ doložit `MyResourceDictionary` – třída. Místo toho odkazuje souboru XAML. Z toho důvodu souboru kódu na pozadí (**MyResourceDictionary.xaml.cs**) se už nevyžaduje. Rovněž můžete odebrat `x:Class` atribut z kořenové značky **MyResourceDictionary.xaml** souboru. 

## <a name="summary"></a>Souhrn

Jak vytvářet a využívat popsané v tomto článku [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)a způsob sloučení slovnících prostředků. A `ResourceDictionary` umožňuje prostředky definované na jednom místě, a znovu použít v celé aplikaci Xamarin.Forms.

## <a name="related-links"></a>Související odkazy

- [Slovnících prostředků (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Styly](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
