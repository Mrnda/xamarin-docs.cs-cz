---
title: "Úvod"
description: "Šablony dat Xamarin.Forms poskytují možnost definovat prezentaci dat na podporované ovládací prvky. Tento článek obsahuje úvod do šablony dat, prozkoumání, proč jsou zapotřebí."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: e7c1a8b233538b7ad40a18e44747bef672210516
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="introduction"></a>Úvod

_Šablony dat Xamarin.Forms poskytují možnost definovat prezentaci dat na podporované ovládací prvky. Tento článek obsahuje úvod do šablony dat, prozkoumání, proč jsou zapotřebí._

Vezměte v úvahu [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) který zobrazí kolekce `Person` objekty. Následující příklad kódu ukazuje definici `Person` třídy:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

`Person` Třída definuje `Name`, `Age`, a `Location` vlastnosti, které můžete nastavit při zpracování `Person` je vytvořen objekt. [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Se používá k zobrazení kolekce `Person` objekty, jak je znázorněno v následujícím příkladu kódu XAML:

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

Položky budou přidány do [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) v jazyce XAML podle inicializace [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) vlastnost z pole `Person` instance.

> [!NOTE]
> Všimněte si, že `x:Array` prvek vyžaduje `Type` atribut, který určuje typ položky v poli.

Ekvivalentní stránky C# je vidět v následujícím příkladu kódu, který inicializuje [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) vlastnosti `List` z `Person` instancí:

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

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Volání `ToString` při zobrazení objektů v kolekci. Vzhledem k tomu, že neexistuje žádná `Person.ToString` přepsat, `ToString` vrátí název typu každý objekt, jak je vidět na následujících snímcích obrazovky:

![](introduction-images/no-data-template.png "ListView bez šablonu dat")

`Person` Objekt můžete přepsat `ToString` metodu pro zobrazení smysluplných dat, jak je znázorněno v následujícím příkladu kódu:

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

Výsledkem [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) zobrazení `Person.Name` hodnota vlastnosti pro každý objekt v kolekci, jak je vidět na následujících snímcích obrazovky:

![](introduction-images/override-tostring.png "ListView s šablonu dat")

`Person.ToString` Přepsání může vrátit formátovaný řetězec, který se skládá z `Name`, `Age`, a `Location` vlastnosti. Tento přístup však nabízí omezenou řídit vzhled každou položku dat. Pro větší flexibilitu [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) lze vytvořit, který definuje vzhled data.

## <a name="creating-a-datatemplate"></a>Vytváření šablonu DataTemplate

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) slouží k určení vzhledu dat a k zobrazení dat se většinou používá datové vazby. Jeho běžný scénář použití je při zobrazení dat z kolekce objektů [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Například, když `ListView` je vázána na kolekci `Person` objekty, `ListView.ItemTemplate` vlastnost bude nastavena pro `DataTemplate` , který definuje vzhled jednotlivých `Person` objekt v `ListView`. `DataTemplate` Bude obsahovat prvky, které vytvořit vazbu na hodnoty vlastností jednotlivých `Person` objektu. Další informace o vazbě dat najdete v tématu [základy vazby dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) lze použít jako hodnotu pro následující vlastnosti:

- [`ListView.HeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HeaderTemplate/)
- [`ListView.FooterTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.FooterTemplate/)
- [`ListView.GroupHeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/)
- [`ItemsView.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/), který zdědí [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/).
- [`MultiPage.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/), který zdědí [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), a [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/).

> [!NOTE]
> Všimněte si, že i když [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) díky použití [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) objekty, nepoužívá [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Důvodem je, že vazby dat jsou vždy nastavit přímo na `Cell` objekty.

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) který je umístěn jako přímými podřízenými vlastností uvedených výše je známý jako *vložené šablony*. Alternativně `DataTemplate` může být definováno jako prostředek řízení úrovni, stránky nebo aplikace. Výběr místo definice [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) ovlivňuje, kde je možné:

- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) definované v ovládacím prvku úroveň lze použít pouze k ovládacímu prvku.
- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) definované na stránce úroveň lze použít pro více platný ovládacích prvků na stránce.
- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) definované na úrovni aplikace může být použit na platný ovládací prvky v celé aplikaci.

Šablony dat níže v hierarchii zobrazení mají přednost před nastavením definovaným vyšší až při sdílení `x:Key` atributy. Například šablonu data na úrovni aplikace bude přepsáno šablonu data na úrovni stránky a šablonu úrovně stránky dat bude přepsáno šablonu úroveň kontroly dat nebo dat šablonu vložené.


## <a name="related-links"></a>Související odkazy

- [Vzhled buňky](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Šablony dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
