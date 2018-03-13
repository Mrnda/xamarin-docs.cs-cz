---
title: Cesta vazby
description: "Použití datových vazeb dílčí vlastnosti přístup a členy kolekce"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: fb385a9c7d1dfd01d95691b77122cdbb84d814e5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="binding-path"></a>Cesta vazby

Ve všech předchozích příkladech vazby dat [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) vlastnost `Binding` – třída (nebo [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) vlastnost `Binding` – rozšíření značek) byla nastavena do vlastnosti jediné. Ve skutečnosti lze nastavit `Path` k *dílčí vlastnosti* (vlastnost vlastnost), nebo členem kolekce.

Předpokládejme například, obsahuje stránku `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

`Time` Vlastnost `TimePicker` je typu `TimeSpan`, ale možná budete chtít vytvořit vazbu dat, která odkazuje `TotalSeconds` vlastnost této `TimeSpan` hodnotu. Tady je datové vazby:

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```
         
`Time` Vlastnost je typu `TimeSpan`, který má `TotalSeconds` vlastnost. `Time` a `TotalSeconds` vlastnosti jsou simply připojené tečkou. Položky v `Path` řetězec vždy odkazovat na vlastnosti a ne na typy těchto vlastností.

Jestli třeba a některé další se zobrazují v **cesta variace** stránky:

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

Ve druhém `Label`, zdroj vazba je vlastní stránky. `Content` Vlastnost je typu `StackLayout`, který má `Children` vlastnost typu `IList<View>`, na kterém je `Count` vlastnost určující počet podřízených prvků.

## <a name="paths-with-indexers"></a>Cesty se indexery

Vazby ve třetím `Label` v **cesta variace** stránky odkazy [ `CultureInfo` ](https://developer.xamarin.com/api/type/System.Globalization.CultureInfo/) třídy v `System.Globalization` obor názvů:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

Zdroj je nastavena na statickou `CultureInfo.CurrentCulture` vlastnost, která je objekt typu `CultureInfo`. Aby třída definuje vlastnost s názvem `DateTimeFormat` typu [ `DateTimeFormatInfo` ](https://developer.xamarin.com/api/type/System.Globalization.DateTimeFormatInfo/) obsahující `DayNames` kolekce. Index vybere čtvrtý položky.

Čtvrtý `Label` dělá něco podobného, ale pro jazykovou verzi přidružené Francie. `Source` Vazby je nastavena na `CultureInfo` objekt s konstruktor:

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

V tématu [předávání argumenty konstruktoru](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) další podrobnosti o zadání argumentů konstruktor v jazyce XAML.

Nakonec je poslední příklad podobná druhý, s tím rozdílem, že se odkazuje na jeden podřízených objektů `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Podřazených je `Label`, který má `Text` vlastnost typu `String`, který má `Length` vlastnost. První `Label` sestavy `TimeSpan` nastavit `TimePicker`, takže když tento text se změní, konečné `Label` také změny.

Tady je programy spuštěné na všech tří platformách:

[![Cesta variace](binding-path-images/pathvariations-small.png "cesta variace")](binding-path-images/pathvariations-large.png#lightbox "variace cesta")

## <a name="debugging-complex-paths"></a>Ladění komplexní cesty

Definice komplexní cesta může být obtížné sestavit: je nutné znát typ každou dílčí vlastnost nebo typ položky v kolekci správně přidání další dílčí vlastnosti, ale typy sami se nezobrazí v cestě. Jeden dobrý technika je přírůstkové sestavování cestu a podívejte se na mezilehlých výsledků. Poslední například, můžete začít s ne `Path` definice v všechny:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

Který typ vazby zdroje, zobrazí nebo `DataBindingDemos.PathVariationsPage`. Znáte `PathVariationsPage` je odvozena z `ContentPage`, takže má `Content` vlastnost:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

Typ `Content` vlastnost je nyní odhalen být `Xamarin.Forms.StackLayout`. Přidat `Children` vlastnost, která má `Path` a typ je `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, což je třída interní Xamarin.Forms, ale samozřejmě typu kolekce. Přidat index, a typ je `Xamarin.Forms.Label`. Pokračujte tímto způsobem.

Jako Xamarin.Forms zpracovává cestu vazby, nainstaluje `PropertyChanged` obslužná rutina libovolného objektu v cestě, která implementuje `INotifyPropertyChanged` rozhraní. Například konečné vazby reaguje na změny v prvním `Label` protože `Text` změny vlastností. 

Pokud vlastnost v cestě vazba neimplementuje `INotifyPropertyChanged`, všechny změny tuto vlastnost bude ignorována. Některé změny může zcela zneplatnit cestu vazby, proto použijte tento postup pouze v případě, že řetězec vlastnosti a dílčí vlastnosti nikdy zneplatní.



## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Kapitola vazby dat z adresáře Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
