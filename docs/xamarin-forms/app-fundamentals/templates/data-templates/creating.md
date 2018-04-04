---
title: Vytváření šablonu DataTemplate
description: Data šablony lze vytvořit vložením, v ResourceDictionary nebo z vlastního typu nebo příslušného typu buňky Xamarin.Forms. Tento článek popisuje postupy.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: cd4266fb8e7d68a9f93bd169ca70c6a0f516a357
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-datatemplate"></a>Vytváření šablonu DataTemplate

_Data šablony lze vytvořit vložením, v ResourceDictionary nebo z vlastního typu nebo příslušného typu buňky Xamarin.Forms. Tento článek popisuje postupy._

Běžné scénáře využití [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) je zobrazení dat z kolekce objektů [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Vzhled data u jednotlivých buněk [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) je možné spravovat pomocí nastavení [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) vlastnosti [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Existuje několik postupů, které můžete k tomu použít:

- [Vytvoření vložené DataTemplate](#inline).
- [Vytváření šablonu DataTemplate s typem](#type).
- [Vytváření DataTemplate jako prostředek](#resource).

Bez ohledu na to techniku, používá, výsledkem je, že vzhled jednotlivých buněk [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) je definované [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), jak je vidět na následujících snímcích obrazovky:

![](creating-images/data-template-appearance.png "ListView s šablonu DataTemplate")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>Vytvoření vložené DataTemplate

[ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Vlastnost lze nastavit vložené [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Vložené šablony, což je ten, který je umístěn jako přímými podřízenými vlastnosti vhodný ovládací prvek, by měl použijí, pokud není nutné znovu použít šablonu dat jinam. Prvky určené ve `DataTemplate` definujte vzhled jednotlivých buněk, jak je znázorněno v následujícím příkladu kódu XAML:

```xaml
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
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Label Text="{Binding Name}" FontAttributes="Bold" />
                    <Label Grid.Column="1" Text="{Binding Age}" />
                    <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Podřízená o vložený [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) musí být, nebo jsou odvozeny od, zadejte [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/). Rozložení uvnitř `ViewCell` spravuje sem [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). `Grid` Obsahuje tři [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance této vazby jejich [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) vlastnosti na odpovídající vlastnosti každého `Person` objekt v kolekci.

Ekvivalentní kódu C# je znázorněno v následujícím příkladu kódu:

```csharp
public class WithDataTemplatePageCS : ContentPage
{
    public WithDataTemplatePageCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        var personDataTemplate = new DataTemplate(() =>
        {
            var grid = new Grid();
            ...
            var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
            var ageLabel = new Label();
            var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

            nameLabel.SetBinding(Label.TextProperty, "Name");
            ageLabel.SetBinding(Label.TextProperty, "Age");
            locationLabel.SetBinding(Label.TextProperty, "Location");

            grid.Children.Add(nameLabel);
            grid.Children.Add(ageLabel, 1, 0);
            grid.Children.Add(locationLabel, 2, 0);

            return new ViewCell { View = grid };
        });

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemsSource = people, ItemTemplate = personDataTemplate, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

V jazyce C#, vložený [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) je vytvořený pomocí konstruktoru přetížení, které určuje `Func` argument.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>Vytváření šablonu DataTemplate s typem

[ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Vlastnost může být také nastavena na [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) vytvořený z typu buňky. Výhodou tohoto přístupu je, že lze opětovně použít vzhled definované typem buňky ve více šablonách data v celé aplikaci. Následující kód XAML ukazuje příklad tohoto přístupu:

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
                    ...
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <local:PersonCell />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Zde [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) je nastavena na [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) vytvořený z vlastního typu, která definuje vzhled buňky. Vlastní typ musí být odvozeny od typu [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), jak ukazuje následující příklad kódu:

```xaml
<ViewCell xmlns="http://xamarin.com/schemas/2014/forms"
          xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
          x:Class="DataTemplates.PersonCell">
     <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.2*" />
            <ColumnDefinition Width="0.3*" />
        </Grid.ColumnDefinitions>
        <Label Text="{Binding Name}" FontAttributes="Bold" />
        <Label Grid.Column="1" Text="{Binding Age}" />
        <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
    </Grid>
</ViewCell>
```

V rámci [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), rozložení je zde spravuje [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). `Grid` Obsahuje tři [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance této vazby jejich [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) vlastnosti na odpovídající vlastnosti každého `Person` objekt v kolekci.

Ekvivalentní kódu C# je znázorněno v následujícím příkladu:

```csharp
public class WithDataTemplatePageFromTypeCS : ContentPage
{
    public WithDataTemplatePageFromTypeCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemTemplate = new DataTemplate(typeof(PersonCellCS)), ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

V jazyce C# [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) je vytvořený pomocí konstruktoru přetížení, které určuje typ buňky jako argument. Typ buňky musí být odvozeny od typu [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), jak ukazuje následující příklad kódu:

```csharp
public class PersonCellCS : ViewCell
{
    public PersonCellCS()
    {
        var grid = new Grid();
        ...
        var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        var ageLabel = new Label();
        var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

        nameLabel.SetBinding(Label.TextProperty, "Name");
        ageLabel.SetBinding(Label.TextProperty, "Age");
        locationLabel.SetBinding(Label.TextProperty, "Location");

        grid.Children.Add(nameLabel);
        grid.Children.Add(ageLabel, 1, 0);
        grid.Children.Add(locationLabel, 2, 0);

        View = grid;
    }
}
```

> [!NOTE]
> Všimněte si, že Xamarin.Forms také zahrnuje typy buněk, které lze použít k zobrazení Jednoduché datové v [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) buněk. Další informace najdete v tématu [vzhledu buněk](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>Vytváření šablonu DataTemplate jako prostředek

Data šablony lze vytvořit také jako opakovaně použitelné objekty v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Toho dosáhnete tím, že každá deklarace jedinečný `x:Key` atribut, který poskytuje popisný klíč v `ResourceDictionary`, jak je znázorněno v následujícím příkladu kódu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="personTemplate">
                 <ViewCell>
                    <Grid>
                        ...
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        ...
        <ListView ItemTemplate="{StaticResource personTemplate}" Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Je přiřazena k [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) vlastnost pomocí `StaticResource` – rozšíření značek. Všimněte si, že chvíli `DataTemplate` na stránce je definován [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), by se mohl definovat také na úrovni ovládací prvek nebo na úrovni aplikace.

Následující příklad kódu ukazuje na stejnou stránku v jazyce C#:

```csharp
public class WithDataTemplatePageCS : ContentPage
{
  public WithDataTemplatePageCS ()
  {
    ...
    var personDataTemplate = new DataTemplate (() => {
      var grid = new Grid ();
      ...
      return new ViewCell { View = grid };
    });

    Resources = new ResourceDictionary ();
    Resources.Add ("personTemplate", personDataTemplate);

    Content = new StackLayout {
      Margin = new Thickness(20),
      Children = {
        ...
        new ListView { ItemTemplate = (DataTemplate)Resources ["personTemplate"], ItemsSource = people };
      }
    };
  }
}
```

[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Je přidán do [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) pomocí [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) metoda, která určuje `Key` řetězec, který se používá ke odkaz `DataTemplate` při načítání ho.

## <a name="summary"></a>Souhrn

Tento článek obsahuje vysvětlení způsob vytvoření šablon dat, vložené, z vlastního typu, nebo v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Šablonu vložené by měl použijí, pokud není nutné znovu použít šablonu dat jinam. Šablonu dat můžete alternativně znovu použít tak, že definujete jej jako vlastní typ nebo jako prostředek řízení úrovni úrovně stránky nebo na úrovni aplikace.


## <a name="related-links"></a>Související odkazy

- [Vzhled buňky](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Šablony dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
