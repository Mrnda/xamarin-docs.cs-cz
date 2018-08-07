---
title: Slovníky sloučených prostředků
description: Prostředky XAML jsou objekty, které lze sdílet a opakovaně používat v rámci aplikace Xamarin.Forms.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: d305de651f6e770d02ca35f5f8f8ffcf08424e28
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573643"
---
# <a name="resource-dictionaries"></a>Slovníky sloučených prostředků

_Prostředky XAML jsou definice objektů, které lze sdílet a opakovaně používat v rámci aplikace Xamarin.Forms._

Tyto objekty prostředků jsou uloženy ve slovníku prostředků. Tento článek popisuje, jak vytvářet a využívat slovník prostředků a jak to sloučit slovníky prostředků.

## <a name="overview"></a>Přehled

A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) je úložištěm pro prostředky, které jsou používány aplikací Xamarin.Forms. Mezi typické prostředky, které jsou uložené v `ResourceDictionary` zahrnují [styly](~/xamarin-forms/user-interface/styles/index.md), [řízení šablony](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [datové šablony](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), barvy a převaděče.

V XAML, prostředky, které jsou uložené v `ResourceDictionary` můžete pak načíst a použít pro prvky pomocí `StaticResource` – rozšíření značek. V jazyce C#, můžete prostředky také definováno v `ResourceDictionary` a načíst a použít pro prvky pomocí indexeru založené na řetězci. Existuje ale jen málo výhod použití `ResourceDictionary` v jazyce C#, jako sdílené objekty lze jednoduše uložené jako pole nebo vlastnosti a přistupovat přímo bez nutnosti první je načíst ze slovníku.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Vytváření a využívání ResourceDictionary

Prostředky jsou definovány v [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) , který je nastaven na jednu z následujících `Resources` vlastnosti:

- [ `Resources` ](xref:Xamarin.Forms.Application.Resources) Vlastnost třídy, která je odvozena z [`Application`](xref:Xamarin.Forms.Application)
- [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Vlastnost třídy, která je odvozena z [`VisualElement`](xref:Xamarin.Forms.Application)

Xamarin.Forms program obsahuje pouze jednu třídu, která je odvozena z `Application` ale často využívá mnoho tříd, které jsou odvozeny z `VisualElement`, včetně stránek, rozložení a ovládací prvky. Některé z těchto objektů můžete mít jeho `Resources` vlastnost nastavena na hodnotu `ResourceDictionary`. Volba umístění konkrétní `ResourceDictionary` dopady, ve kterém můžou použít prostředky:

- Prostředky v `ResourceDictionary` , který je připojen k zobrazení, jako `Button` nebo `Label` jde použít jedině pro daný objekt, aby to nebylo velmi užitečné.
- Prostředky v `ResourceDictionary` připojené k rozložení, jako `StackLayout` nebo `Grid` lze použít k rozložení a všechny podřízené objekty tohoto rozložení.
- Prostředky v `ResourceDictionary` definované na stránce úroveň lze na stránku a všechny jeho podřízené objekty.
- Prostředky v `ResourceDictionary` definované na úrovni aplikace, úroveň lze použít v celé aplikaci.

Následující XAML zobrazuje prostředky, které jsou definované úrovni aplikace `ResourceDictionary` v **App.xaml** soubor vytvořený jako součást standardní program Xamarin.Forms:

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

To `ResourceDictionary` definuje tři [ `Color` ](xref:Xamarin.Forms.Color) prostředky a [ `Style` ](xref:Xamarin.Forms.Style) prostředků. Další informace o `App` najdete v tématu [třídu aplikace](~/xamarin-forms/app-fundamentals/application-class.md).

Počínaje Xamarin.Forms 3.0, explicitní `ResourceDictionary` značky se nevyžadují. `ResourceDictionary` Objekt je automaticky vytvořen a je přímo mezi vložit prostředky `Resources` element vlastnosti značky:

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

Každý prostředek má klíč, který je určen pomocí `x:Key` atribut, který se stane klíč slovníku `ResourceDictionary`. Klíč se používá k načtení prostředku z `ResourceDictionary` podle [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) – rozšíření značek, jak je ukázáno v následujícím příkladu kódu XAML, který zobrazuje další prostředky, které jsou definované v rámci `StackLayout`:

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

První [ `Label` ](xref:Xamarin.Forms.Label) instanci načte a zpracuje `LabelPageHeadingStyle` prostředek definovaný na úrovni aplikace `ResourceDictionary`, s druhým `Label` instance načítání a pracovat s nimi `LabelNormalStyle`prostředek definovaný na úrovni řízení `ResourceDictionary`. Podobně [ `Button` ](xref:Xamarin.Forms.Button) instanci načte a zpracuje `NormalTextColor` prostředek definovaný na úrovni aplikace `ResourceDictionary`a `MediumBoldText` prostředek definovaný na úrovni řízení `ResourceDictionary`. Výsledkem je vzhled je znázorněno na následujících snímcích obrazovky:

[![](resource-dictionaries-images/screenshots-sml.png "Využívání prostředků ResourceDictionary")](resource-dictionaries-images/screenshots.png#lightbox "spotřebovávat ResourceDictionary prostředků")

> [!NOTE]
> Aplikace úrovně slovníku zdrojů, jako takové prostředky se poté analyzovat při spuštění aplikace místo v případě potřeby stránkou by neměl být zařazen prostředky, které jsou specifické pro jednu stránku. Další informace najdete v tématu [zmenšit velikost slovníku prostředků aplikace](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Přepisující prostředky.

Když `ResourceDictionary` sdílet prostředky `x:Key` hodnoty atributů, prostředky definované nižší v hierarchii zobrazení bude mít prioritu před těmi definovanými vyšší nahoru. Například nastavení `PageBackgroundColor` prostředek, který se `Blue` na úrovni aplikace, bude úroveň přepsat úrovni stránky `PageBackgroundColor` prostředku nastavena na `Yellow`. Obdobně úrovni stránky `PageBackgroundColor` prostředku se přepíše podle úrovně ovládacího prvku `PageBackgroundColor` prostředků. Tato priorita je znázorněn v následujícím příkladu kódu XAML:

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

Původní `PageBackgroundColor` a `NormalTextColor` jsou přepsány instancí, které jsou definované na úrovni aplikace `PageBackgroundColor` a `NormalTextColor` instancí, které jsou definované na úrovni stránky. Proto bude modrou barvu pozadí stránky a text na této stránce se vybarví žlutě, jak je ukázáno na následujících snímcích obrazovky:

[![](resource-dictionaries-images/overridding-screenshots-sml.png "Přepsání ResourceDictionary prostředky")](resource-dictionaries-images/overridding-screenshots.png#lightbox "přepisující prostředky ResourceDictionary")

Všimněte si však, že na pozadí panelu [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) stále žlutý, protože [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) je nastavena na hodnotu `PageBackgroundColor` prostředek definovaný v aplikaci úroveň `ResourceDictionary`.

Tady je další způsob, jak uvažovat o `ResourceDictionary` Priorita: při the XAML analyzátor narazí `StaticResource`, vyhledá odpovídající klíč pomocí procházející vizuálního stromu, použije se první odpovídající najde. Pokud je toto vyhledávání končí na stránce a klíč ještě nebyl nalezen, prohledá analyzátoru XAML `ResourceDictionary` připojené k `App` objektu. Pokud ještě není nalezen klíč, je vyvolána výjimka.

## <a name="stand-alone-resource-dictionaries"></a>Slovníky prostředků pro samostatné

Třída odvozená z `ResourceDictionary` může být také v samostatném samostatného souboru. (Vyšší přesností třídy odvozené od `ResourceDictionary` obvykle vyžaduje _pár_ souborů vzhledem k tomu, že prostředky jsou definovány v souboru XAML, ale soubor kódu na pozadí s `InitializeComponent` volání je také nutné.) Výsledný soubor je pak sdílet mezi aplikacemi.

Vytvořit takový soubor, přidejte nový **zobrazení obsahu** nebo **stránku obsahu** položky do projektu (ale ne **zobrazení obsahu** nebo **stránku obsahu** s pouze soubor jazyka C#). V XAML souboru i souboru jazyka C#, změňte název základní třídy z `ContentView` nebo `ContentPage` k `ResourceDictionary`. Název základní třídy v souboru XAML je element nejvyšší úrovně.

Následující příklad ukazuje XAML [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) s názvem `MyResourceDictionary`:

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

To `ResourceDictionary` obsahuje jeden prostředek, což je objekt typu `DataTemplate`.

Můžete vytvořit instanci `MyResourceDictionary` vložením mezi párem `Resources` element vlastnosti značky, například v `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

Instance `MyResourceDictionary` je nastavena na `Resources` vlastnost `ContentPage` objektu.

Tento přístup má však určitá omezení: `Resources` vlastnost `ContentPage` odkazuje na jenom tento jeden `ResourceDictionary`. Ve většině případů chcete zahrnout další `ResourceDictionary` instancí a případně dalších zdrojů, jakož i.

Tato úloha vyžaduje slovníky sloučených prostředků.

## <a name="merged-resource-dictionaries"></a>Slovníky sloučených prostředků

Slovníky sloučených prostředků kombinovat jeden nebo více `ResourceDictionary` do jiné instance `ResourceDictionary`. Můžete to provést v souboru XAML tak, že nastavíte [ `MergedDictionaries` ](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) vlastnost na jeden nebo více zdrojových slovnících, které se sloučí do aplikace, stránkovací nebo úroveň řízení `ResourceDictionary`.

> [!IMPORTANT]
> `ResourceDictionary` Definuje také [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) vlastnost. Pomocí této vlastnosti; se už nepoužívá od verze Xamarin.Forms 3.0.

Instance `MyResourceDictionary` lze sloučit do jakékoli aplikace, stránkovací nebo úroveň řízení `ResourceDictionary`. Následující příklad kódu XAML zobrazí do úrovně stránky `ResourceDictionary` pomocí `MergedDictionaries` vlastnost:

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

Tento kód ukazuje pouze instance `MyResourceDictionary` přidávaný do `ResourceDictionary` , ale také můžete odkazovat na jiné `ResourceDictionary` instancí v rámci `MergedDictionary` element vlastnosti značek a jiné prostředky vně tyto značky:

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

Může existovat pouze jeden `MergedDictionaries` tématu `ResourceDictionary`, ale můžete vložit libovolný počet `ResourceDictionary` instance v ní chcete.

Po sloučení [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) prostředky sdílet stejné `x:Key` hodnoty atributů, Xamarin.Forms používá následující priority prostředků:

1. Prostředky místní na slovník prostředků.
1. Prostředků obsažených ve slovníku prostředků, která se slučuje prostřednictvím zastaralá [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) vlastnost.
1. Prostředků obsažených ve slovnících prostředků, které byly sloučené přes `MergedDictionaries` kolekce, v opačném pořadí jsou uvedeny v `MergedDictionaries` vlastnost.

> [!NOTE]
> Hledání zdrojových slovnících může být výpočetně náročné úlohy, pokud aplikace obsahuje několik velkých zdrojové slovníky. Proto aby se zabránilo zbytečným vyhledávání, měli byste zajistit, že každá stránka v aplikaci využívá jenom slovníky prostředků, které jsou vhodné pro stránky.

## <a name="merging-dictionaries-in-xamarinforms-30"></a>Slučování slovníků v Xamarin.Forms 3.0

Od verze 3.0 Xamarin.Forms, proces sloučení [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) instancí se stal poněkud jednodušší a flexibilnější. `MergedDictionaries` Už nejsou potřebné značky element vlastnosti. Místo toho můžete přidat do slovníku prostředků Další `ResourceDictionary` značky s novými [ `Source` ](xref:Xamarin.Forms.ResourceDictionary.Source) nastavenou na název souboru XAML s prostředky:

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

Protože automaticky vytvoří Xamarin.Forms 3.0 `ResourceDictionary`, tyto dvě vnější `ResourceDictionary` značky se už nevyžadují:

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

Tato nová syntaxe nemá _není_ vytvořit instanci `MyResourceDictionary` třídy. Místo toho odkazoval na soubor XAML. Z toho důvodu použití modelu code-behind souboru (**MyResourceDictionary.xaml.cs**) se už nevyžaduje. Můžete také odebrat `x:Class` atribut z kořenové značky **MyResourceDictionary.xaml** souboru.

## <a name="summary"></a>Souhrn

Tento článek vysvětlil, jak vytvářet a využívat [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)a jak to sloučit slovníky prostředků. A `ResourceDictionary` umožňuje prostředky definované na jednom místě a opakovaně používat v rámci aplikace Xamarin.Forms.

## <a name="related-links"></a>Související odkazy

- [Slovníky sloučených prostředků (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Styly](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
