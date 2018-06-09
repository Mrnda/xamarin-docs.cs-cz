---
title: Zdroje dat ListView
description: Tento článek vysvětluje, jak k naplnění Xamarin.Forms ListView s daty a použití vazby dat s prvku ListView.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: aa9c23266329c03b3b28c7795f67290bbc23c4bf
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245537"
---
# <a name="listview-data-sources"></a>Zdroje dat ListView

ListView se používá pro zobrazení zobrazí data. Jsme získáte informace o vyplnění ListView s daty, a jak jsme lze vázat k vybrané položce.

- **[Nastavení ItemsSource](#ItemsSource)**  &ndash; používá jednoduchých seznamů nebo pole.
- **[Datová vazba](#Data_Binding)**  &ndash; vytváří vztah mezi modelu a ListView. Vazba je ideální pro rozhraní MVVM vzor.

## <a name="itemssource"></a>Položka ItemsSource
ListView naplněný dat pomocí `ItemsSource` vlastnosti, která může přijmout všechny kolekce implementace `IEnumerable`. Nejjednodušší způsob, jak naplnit `ListView` zahrnuje použití pole řetězců:

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

Naplní výše uvedený přístup `ListView` seznam řetězců. Ve výchozím nastavení `ListView` zavolá `ToString` a zobrazit výsledky v `TextCell` pro každý řádek. Chcete-li přizpůsobit zobrazení dat, přečtěte si téma [vzhledu buněk](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Protože `ItemsSource` byl odeslán do pole, jako základní změny seznamu nebo pole nebude aktualizovat obsah. Pokud chcete ListView k automatické aktualizaci jak položky přidat, odebrat a změnit v seznamu základní, budete muset použít `ObservableCollection`. [`ObservableCollection`](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) je definována v `System.Collections.ObjectModel` a podobně jako `List`kromě toho, že může upozornit `ListView` všech změn:

```csharp
ObservableCollection<Employees> employeeList = new ObservableCollection<Employess>();
listView.ItemsSource = employeeList;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employeeList.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>Datová vazba
Datová vazba je "pojidlem", která se sváže s vlastností objektu některé CLR, jako je například třída ve vaší ViewModel vlastnosti objekt uživatelského rozhraní. Datová vazba je užitečné, protože ji tak, že nahradíte spoustu přítomnost často používaný kód zjednodušuje vývoj uživatelského rozhraní.

Datová vazba funguje tak, že udržování synchronizace objekty, jako jejich vázané hodnoty změnit. Místo nutnosti psaní obslužné rutiny události pro pokaždé, když se změní hodnota ovládacího prvku, můžete vytvořit vazbu a povolit vazby ve vaší ViewModel.

Další informace o vazbě dat naleznete v tématu [základy vazby dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) které je součástí čtyři [Xamarin.Forms XAML základy článek řady](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Vytvoření vazby buňky
Vlastnosti buněk (a podřízené položky buněk) mohou být vázány na vlastnosti objektů v `ItemsSource`. Například prvku ListView může nabídne seznam zaměstnanci s obrázky.

Třída zaměstnanců:

```csharp
public class Employee{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` je vytvořena a nastavena jako `ListView`na `ItemsSource`:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public EmployeeListPage()
{
  //defined in XAML to follow
  EmployeeView.ItemsSource = employees;
  ...
}
```

Naplnění seznamu s daty:

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

Následující fragment kódu ukazuje `ListView` vázána na seznam zaměstnanců:

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

Všimněte si, že byla vazba instalační program v kódu pro jednoduchost, i když ho může mít byla svázána v jazyce XAML.

Definuje předchozí verze jazyka XAML `ContentPage` obsahující `ListView`. Zdroj dat `ListView` jsou nastavena pomocí `ItemsSource` atribut. Rozložení každý řádek `ItemsSource`je definována v rámci `ListView.ItemTemplate` elementu.

Toto je výsledek:

![](data-and-databinding-images/bound-data.png "ListView pomocí datové vazby")

### <a name="binding-selecteditem"></a>Vazba SelectedItem

Často budete chtít vytvořit vazbu k vybrané položce `ListView`, spíš než využívat obslužné rutiny události a reagovat na změny. K tomu v jazyce XAML, vytvořit vazbu `SelectedItem` vlastnost:

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

Za předpokladu, že `listView`na `ItemsSource` je seznam řetězců, `SomeLabel` bude mít vlastnost text vázána `SelectedItem`.



## <a name="related-links"></a>Související odkazy

- [Dva způsobem vazby (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [Poznámky k verzi 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 poznámky](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
