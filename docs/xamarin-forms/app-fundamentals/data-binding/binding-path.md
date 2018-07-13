---
title: Cesta vazby Xamarin.Forms
description: Tento článek vysvětluje, jak pomocí Xamarin.Forms datové vazby pro přístup k podvlastností a členové kolekce se cesta k vlastnosti třídy vazby.
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 887a20f1791a190c182e6d179cfabb46c6e0eb48
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998944"
---
# <a name="xamarinforms-binding-path"></a>Cesta vazby Xamarin.Forms

Ve všech předchozích příkladech vázání dat [ `Path` ](xref:Xamarin.Forms.Binding.Path) vlastnost `Binding` třídy (nebo [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path) vlastnost `Binding` – rozšíření značek) byl nastaven do vlastnosti jediné. Je skutečně možné nastavit `Path` k *dílčí vlastnost* (vlastnost vlastnosti), nebo který je členem kolekce.

Předpokládejme například, že stránka obsahuje `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

`Time` Vlastnost `TimePicker` je typu `TimeSpan`, ale možná budete chtít vytvořit datovou vazbu, která odkazuje `TotalSeconds` vlastnost, která `TimeSpan` hodnotu. Tady je datové vazby:

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

`Time` Vlastnost je typu `TimeSpan`, který má `TotalSeconds` vlastnost. `Time` a `TotalSeconds` vlastnosti jsou simply spojení s tečkou. Položky v `Path` řetězec vždycky odkazovat na vlastnosti a ne na typy těchto vlastností.

Že příklad a několik dalších jsou uvedeny v **cesta variace** stránky:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:globe="clr-namespace:System.Globalization;assembly=mscorlib"
             x:Class="DataBindingDemos.PathVariationsPage"
             Title="Path Variations"
             x:Name="page">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="HorizontalTextAlignment" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10, 0">
        <TimePicker x:Name="timePicker" />

        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time.TotalSeconds,
                              StringFormat='{0} total seconds'}" />

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children.Count,
                              StringFormat='There are {0} children in this StackLayout'}" />

        <Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                              Path=DateTimeFormat.DayNames[3],
                              StringFormat='The middle day of the week is {0}'}" />

        <Label>
            <Label.Text>
                <Binding Path="DateTimeFormat.DayNames[3]"
                         StringFormat="The middle day of the week in France is {0}">
                    <Binding.Source>
                        <globe:CultureInfo>
                            <x:Arguments>
                                <x:String>fr-FR</x:String>
                            </x:Arguments>
                        </globe:CultureInfo>
                    </Binding.Source>
                </Binding>
            </Label.Text>
        </Label>

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children[1].Text.Length,
                              StringFormat='The first Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

Ve druhém `Label`, zdroj vazby je samotné stránky. `Content` Vlastnost je typu `StackLayout`, který má `Children` vlastnost typu `IList<View>`, který má `Count` vlastnost určující počet podřízených.

## <a name="paths-with-indexers"></a>Cesty pomocí indexerů

Vazby ve třetím `Label` v **cesta variace** stránky odkazy [ `CultureInfo` ](xref:System.Globalization.CultureInfo) třídy v `System.Globalization` obor názvů:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

Zdroj je nastavena na statickou `CultureInfo.CurrentCulture` vlastnost, která je objekt typu `CultureInfo`. Že třída definuje vlastnost s názvem `DateTimeFormat` typu [ `DateTimeFormatInfo` ](xref:System.Globalization.DateTimeFormatInfo) , která obsahuje `DayNames` kolekce. Index vybere čtvrtá položka.

Čtvrtý `Label` dělá něco podobného, ale pro jazykovou verzi přidružené k (Francie). `Source` Vazby je nastavena na `CultureInfo` objektu s konstruktorem:

```xaml
<Label>
    <Label.Text>
        <Binding Path="DateTimeFormat.DayNames[3]"
                 StringFormat="The middle day of the week in France is {0}">
            <Binding.Source>
                <globe:CultureInfo>
                    <x:Arguments>
                        <x:String>fr-FR</x:String>
                    </x:Arguments>
                </globe:CultureInfo>
            </Binding.Source>
        </Binding>
    </Label.Text>
</Label>
```

Zobrazit [předávání argumentů konstruktoru](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) podrobné informace o určení argumenty konstruktoru v XAML.

A konečně poslední příklad je podobný sekundu, s výjimkou toho, aby odkazoval na jeden z podřízených prvků `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Podřazených je `Label`, který má `Text` vlastnost typu `String`, který má `Length` vlastnost. První `Label` sestavy `TimeSpan` nastavit `TimePicker`, takže když se tento text se změní, finální `Label` změní se i.

Tady je program spuštěn na všech třech platformách:

[![Cesta variace](binding-path-images/pathvariations-small.png "cesta variace")](binding-path-images/pathvariations-large.png#lightbox "variace cesta")

## <a name="debugging-complex-paths"></a>Ladění složitých cesty

Definice komplexní cesta může být obtížné k sestavení kompletních: je nutné znát typ každý dílčí vlastnosti nebo typem položek v kolekci pro správné přidání další dílčí vlastnosti, ale samotné typy se nezobrazují v cestě. Jedna z technik dobré je přírůstkové sestavování cestu a podívejte se na průběžné výsledky. Poslední například může začít s ne `Path` definice na všechny:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

Který zobrazí typ zdroje vazby nebo `DataBindingDemos.PathVariationsPage`. Znáte `PathVariationsPage` je odvozena z `ContentPage`, takže má `Content` vlastnost:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

Typ `Content` vlastnost teď bude zobrazení `Xamarin.Forms.StackLayout`. Přidat `Children` vlastnost `Path` a typ je `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, což je třída interní Xamarin.Forms, ale samozřejmě typ kolekce. Přidat index, a typ je `Xamarin.Forms.Label`. Pokračujte tímto způsobem.

Jak Xamarin.Forms zpracovává cesta vazby, nainstaluje `PropertyChanged` obslužné rutiny na libovolný objekt v cestě, která implementuje `INotifyPropertyChanged` rozhraní. Například konečné vazby reaguje na změnu v prvním `Label` protože `Text` změny vlastností.

Pokud vlastnost v cestě vazba neimplementuje `INotifyPropertyChanged`, ignorují se všechny změny vlastnosti. Některé změny může zcela zneplatnit cesta vazby, abyste tento postup měly používat jenom v případě, že řetězec vlastností a dílčí nikdy zneplatní.



## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Data vazby kapitola z knihy Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
