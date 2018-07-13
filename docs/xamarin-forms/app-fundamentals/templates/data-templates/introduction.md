---
title: Úvod do Xamarin.Forms datové šablony
description: Xamarin.Forms datové šablony umožňují definovat prezentaci dat pro podporované ovládací prvky. Tento článek obsahuje úvod do šablon dat, posouzení důvod, proč jsou nezbytné.
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 129ce7a04b93bfb3cb1b9a1639aee61cd56d09d5
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998911"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Úvod do Xamarin.Forms datové šablony

_Xamarin.Forms datové šablony umožňují definovat prezentaci dat pro podporované ovládací prvky. Tento článek obsahuje úvod do šablon dat, posouzení důvod, proč jsou nezbytné._

Vezměte v úvahu [ `ListView` ](xref:Xamarin.Forms.ListView) , který zobrazí kolekci `Person` objekty. Následující příklad kódu ukazuje definici `Person` třídy:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

`Person` Třída definuje `Name`, `Age`, a `Location` vlastnosti, které můžete nastavit při `Person` je vytvořen objekt. [ `ListView` ](xref:Xamarin.Forms.ListView) Slouží k zobrazení kolekce `Person` objekty, jak je znázorněno v následujícím příkladu kódu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataTemplates"
             ...>
    <StackLayout Margin="20">
        ...
        <ListView Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    <local:Person Name="John" Age="37" Location="USA" />
                    <local:Person Name="Tom" Age="42" Location="UK" />
                    <local:Person Name="Lucas" Age="29" Location="Germany" />
                    <local:Person Name="Tariq" Age="39" Location="UK" />
                    <local:Person Name="Jane" Age="30" Location="USA" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

Položky jsou přidány do [ `ListView` ](xref:Xamarin.Forms.ListView) v XAML pomocí inicializace [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) vlastnost z pole `Person` instancí.

> [!NOTE]
> Všimněte si, že `x:Array` element vyžaduje `Type` atribut určující typ položky v poli.

Na stejnou stránku C# je uveden v následujícím příkladu kódu, který inicializuje [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) vlastnost `List` z `Person` instancí:

```csharp
public WithoutDataTemplatePageCS()
{
    ...
    var people = new List<Person>
    {
        new Person { Name = "Steve", Age = 21, Location = "USA" },
        new Person { Name = "John", Age = 37, Location = "USA" },
        new Person { Name = "Tom", Age = 42, Location = "UK" },
        new Person { Name = "Lucas", Age = 29, Location = "Germany" },
        new Person { Name = "Tariq", Age = 39, Location = "UK" },
        new Person { Name = "Jane", Age = 30, Location = "USA" }
    };

    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children = {
            ...
            new ListView { ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
        }
    };
}
```

[ `ListView` ](xref:Xamarin.Forms.ListView) Volání `ToString` při zobrazení objektů v kolekci. Protože neexistuje žádný `Person.ToString` přepsat, `ToString` vrátí název typu každého objektu, jak je znázorněno na následujících snímcích obrazovky:

![](introduction-images/no-data-template.png "ListView bez datové šablony")

`Person` Objektu můžete přepsat `ToString` metodu pro zobrazení smysluplná data, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class Person
{
  ...
  public override string ToString ()
  {
    return Name;
  }
}
```

Výsledkem [ `ListView` ](xref:Xamarin.Forms.ListView) zobrazení `Person.Name` hodnota vlastnosti pro každý objekt v kolekci, jak je znázorněno na následujících snímcích obrazovky:

![](introduction-images/override-tostring.png "ListView s využitím šablony dat")

`Person.ToString` Přepsání může vrátit formátovaný řetězec, který se skládá z `Name`, `Age`, a `Location` vlastnosti. Tento přístup však nabízí pouze omezené řídit vzhled každou položku dat. V zájmu větší flexibility [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) je možné vytvořit, která definuje vzhled elementů data.

## <a name="creating-a-datatemplate"></a>Vytvořit šablonu DataTemplate

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) slouží k určení vzhledu dat a obvykle používá k zobrazení dat datové vazby. Jeho běžný scénář využití je při zobrazení dat z kolekce objektů [ `ListView` ](xref:Xamarin.Forms.ListView). Například, když `ListView` je vázán na kolekci `Person` objekty, `ListView.ItemTemplate` vlastnost bude nastavena na `DataTemplate` , která definuje vzhled elementů každý `Person` objekt `ListView`. `DataTemplate` Bude obsahovat prvky, které svázat hodnoty vlastností každého `Person` objektu. Další informace o datové vazbě naleznete v tématu [základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) můžete použít jako hodnotu pro následující vlastnosti:

- [`ListView.HeaderTemplate`](xref:Xamarin.Forms.ListView.HeaderTemplate)
- [`ListView.FooterTemplate`](xref:Xamarin.Forms.ListView.FooterTemplate)
- [`ListView.GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate)
- [`ItemsView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1), který zdědí [ `ListView` ](xref:Xamarin.Forms.ListView).
- [`MultiPage.ItemTemplate`](xref:Xamarin.Forms.MultiPage`1), který zdědí [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), a [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage).

> [!NOTE]
> Všimněte si, že i když [ `TableView` ](xref:Xamarin.Forms.TableView) využívá [ `Cell` ](xref:Xamarin.Forms.Cell) objekty, nepoužívá [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Důvodem je, že datové vazby jsou vždy nastavena přímo na `Cell` objekty.

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) , který je umístěn jako přímý podřízený vlastností uvedených výše se označuje jako *vložená šablona*. Můžete také `DataTemplate` lze definovat jako prostředek úrovni ovládací prvek, stránky nebo aplikace. Volba místo definice [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) dopady, kde je možné:

- A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) definované v ovládacím prvku úroveň jde použít jedině do ovládacího prvku.
- A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) definované na stránce úroveň lze použít u více platný ovládacích prvků na stránce.
- A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) definované na úrovni aplikace může být použitý k platný ovládací prvky v celé aplikaci.

Níže v hierarchii zobrazení datové šablony přednost před těmi definovanými vyšší až při sdílení `x:Key` atributy. Například šablonu data na úrovni aplikace, budou přepsaná identifikátorem šablonu data na úrovni stránky a šablony data na úrovni stránek, budou přepsaná identifikátorem šablonu úroveň řízení dat nebo šablony vložená data.


## <a name="related-links"></a>Související odkazy

- [Vzhled buňky](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Datové šablony (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
