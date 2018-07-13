---
title: Zdroje dat ListView
description: Tento článek vysvětluje, jak k naplnění ListView Xamarin.Forms s daty a použití datových vazeb s ListView.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 17c353844a7ddc808e5d9f0632434472913170a4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995203"
---
# <a name="listview-data-sources"></a>Zdroje dat ListView

ListView je používána pro seznamy dat zobrazení. Seznámíme naplnění ListView s daty a jak jsme vázat na vybranou položku.

- **[Nastavení vlastnost ItemsSource](#ItemsSource)**  &ndash; používá jednoduchý seznam nebo pole.
- **[Vytváření datových vazeb](#Data_Binding)**  &ndash; vytváří vztah mezi modelem a ListView. Vazba je ideální pro vzor MVVM.

## <a name="itemssource"></a>Vlastnost ItemsSource
ListView se vyplní pomocí dat `ItemsSource` vlastnost, která může přijmout všechny kolekce implementace `IEnumerable`. Nejjednodušší způsob, jak naplnit `ListView` zahrnuje použití pole řetězců:

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]{
  "mono",
  "monodroid",
  "monotouch",
  "monorail",
  "monodevelop",
  "monotone",
  "monopoly",
  "monomodal",
  "mononucleosis"
};

//monochrome will not appear in the list because it was added
//after the list was populated.
listView.ItemsSource.Add("monochrome");
```

![](data-and-databinding-images/itemssource-simple.png "ListView zobrazení seznamu řetězců")

Výše uvedený přístup se vyplní `ListView` seznam řetězců. Ve výchozím nastavení `ListView` zavolá `ToString` a výsledek se zobrazí `TextCell` pro každý řádek. Přizpůsobit způsob zobrazení dat, přečtěte si článek [vzhled buňky](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Protože `ItemsSource` byl odeslán do pole, nedojde k aktualizaci obsahu se změnami podkladových seznamu nebo pole. Pokud chcete ListView se automaticky aktualizovat, protože položky přidat, odebrat a změnit v nadřízeném seznamu, budete muset použít `ObservableCollection`. [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1) je definován v `System.Collections.ObjectModel` a je stejně jako `List`, s tím rozdílem, že jej můžete upozornit `ListView` všechny změny:

```csharp
ObservableCollection<Employees> employeeList = new ObservableCollection<Employess>();
listView.ItemsSource = employeeList;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employeeList.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>Datová vazba
Datová vazba je "skutečným pojidlem", která vytvoří vazbu vlastnosti objekt uživatelského rozhraní na vlastnosti objektu některé CLR, jako je například třída ve vaší ViewModel. Datová vazba je užitečné, protože zjednodušuje vývoj uživatelského rozhraní tak, že nahradíte spoustu nudné často používaný kód.

Vytváření datových vazeb funguje tak, že udržování synchronizace objektů podle jejich vazby hodnoty. Namísto nutnosti psát obslužné rutiny událostí pro pokaždé, když se změní hodnota ovládacího prvku, vytvořit vazbu a povolení vazby ve vaší ViewModel.

Další informace o vytváření datových vazeb, naleznete v tématu [základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) který je čtvrtou částí [série článků Xamarin.Forms XAML Základy](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Vazba buňky
Vlastnosti buňky (a podřízené položky buněk) může být vázaný na vlastnosti objektů v `ItemsSource`. Například ListView může zobrazit seznam zaměstnanci s obrázky.

Třídy zaměstnanců:

```csharp
public class Employee{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` je vytvořena a nastavena jako `ListView`společnosti `ItemsSource`:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public EmployeeListPage()
{
  //defined in XAML to follow
  EmployeeView.ItemsSource = employees;
  ...
}
```

Seznam se naplní daty:

```csharp
public EmployeeListPage()
{
  ...
  employees.Add(new Employee{ DisplayName="Rob Finnerty"});
  employees.Add(new Employee{ DisplayName="Bill Wrestler"});
  employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
  employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
  employees.Add(new Employee{ DisplayName="Sheri Spruce"});
  employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

Následující fragment kódu ukazuje `ListView` vázán na seznam zaměstnanců:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
Title="Employee List">
  <ListView x:Name="EmployeeView">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

Všimněte si, že byla vazba instalační program v kódu pro jednoduchost, i když ho by byly připojeny v XAML.

Předchozí verze XAML definuje `ContentPage` , která obsahuje `ListView`. Zdroj dat `ListView` nastavené přes `ItemsSource` atribut. Rozložení jednotlivých řádků `ItemsSource`je definována v rámci `ListView.ItemTemplate` elementu.

Toto je výsledek:

![](data-and-databinding-images/bound-data.png "Pomocí datové vazby ListView")

### <a name="binding-selecteditem"></a>Vazba SelectedItem

Často budete chtít vytvořit vazbu na vybranou položku `ListView`, spíše než použijte obslužnou rutinu události reakce na změny. K tomu v XAML vytvořit vazbu `SelectedItem` vlastnost:

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

Za předpokladu, že `listView`společnosti `ItemsSource` je seznam řetězců, `SomeLabel` bude mít vlastnost text vázán na `SelectedItem`.



## <a name="related-links"></a>Související odkazy

- [Dvě vazby způsobem (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [zpráva k vydání verze 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [Poznámky k verzi 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
