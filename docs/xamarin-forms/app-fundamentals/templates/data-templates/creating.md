---
title: Vytváří se Xamarin.Forms DataTemplate
description: Data šablony lze vytvořit jako vložené, ResourceDictionary, nebo z vlastního typu nebo odpovídající typ buňky Xamarin.Forms. Tento článek se věnuje každou technikou.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 63f9bf82bc8e637aced1afa5d5699ac1e8dc3f8c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994612"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Vytváří se Xamarin.Forms DataTemplate

_Data šablony lze vytvořit jako vložené, ResourceDictionary, nebo z vlastního typu nebo odpovídající typ buňky Xamarin.Forms. Tento článek se věnuje každou technikou._

Běžný scénář využití [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) je zobrazení dat z kolekce objektů v [ `ListView` ](xref:Xamarin.Forms.ListView). Zobrazení dat pro každou buňku v [ `ListView` ](xref:Xamarin.Forms.ListView) je možné spravovat pomocí nastavení [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) vlastnost [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Existuje několik postupů, které lze použít k provedení této:

- [Vytvoření DataTemplate vložené](#inline).
- [Vytvořit šablonu DataTemplate s typem](#type).
- [Vytváření DataTemplate jako prostředek](#resource).

Bez ohledu na to techniku používá, výsledkem je, že vzhled každá buňka [ `ListView` ](xref:Xamarin.Forms.ListView) je definován [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), jak je znázorněno na následujících snímcích obrazovky:

![](creating-images/data-template-appearance.png "ListView s šablonu DataTemplate")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>Vytvoření vložený šablonou DataTemplate

[ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) Vlastnost lze nastavit na vložené [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Vložená šablona, což je ten, který je umístěn jako přímý podřízený ovládací prvek odpovídající vlastnosti, by měl použít, pokud není nutné opakovaně používat šablony jinde. Prvky určené ve `DataTemplate` definujte vzhled buňky, jak je znázorněno v následujícím příkladu kódu XAML:

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

Podřízený vložený [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) musí být typu, nebo jsou odvozeny z, zadejte [ `ViewCell` ](xref:Xamarin.Forms.ViewCell). Rozložení uvnitř `ViewCell` spravuje tady [ `Grid` ](xref:Xamarin.Forms.Grid). `Grid` Obsahuje tři [ `Label` ](xref:Xamarin.Forms.Label) instance této vazby jejich [ `Text` ](xref:Xamarin.Forms.Label.Text) vlastnosti odpovídající vlastnosti každého `Person` objekt v kolekci.

Ekvivalentní kód jazyka C# můžete vidět v následujícím příkladu kódu:

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

V jazyce C#, vložený [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) je vytvořený pomocí přetížení konstruktoru, který určuje `Func` argument.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>Vytváření s typem šablonu DataTemplate

[ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) Vlastnost může být také nastavena na [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) , která je vytvořena z typu buňky. Výhodou tohoto přístupu je, že vzhled definován typ buňky lze opakovaně využít pro více šablon data v celé aplikaci. Následující kód XAML ukazuje příklad tohoto přístupu:

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

Tady [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) je nastavena na [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) , který je vytvořen z vlastního typu, která definuje vzhled buňky. Vlastní typ musí být odvozen od typu [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), jak je znázorněno v následujícím příkladu kódu:

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

V rámci [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), rozložení je tady spravuje [ `Grid` ](xref:Xamarin.Forms.Grid). `Grid` Obsahuje tři [ `Label` ](xref:Xamarin.Forms.Label) instance této vazby jejich [ `Text` ](xref:Xamarin.Forms.Label.Text) vlastnosti odpovídající vlastnosti každého `Person` objekt v kolekci.

Ekvivalentní kód jazyka C# můžete vidět v následujícím příkladu:

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

V jazyce C# [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) je vytvořený pomocí přetížení konstruktoru, který určuje typ buňky jako argument. Typ buňky musí být odvozen od typu [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), jak je znázorněno v následujícím příkladu kódu:

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
> Mějte na paměti, že Xamarin.Forms také zahrnuje typy buněk, které slouží k zobrazení jednoduchého dat v [ `ListView` ](xref:Xamarin.Forms.ListView) buňky. Další informace najdete v tématu [vzhled buňky](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>Vytvořit šablonu DataTemplate jako prostředek

Datové šablony lze také vytvořit opakovaně použitelné v jako objekty [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Toho můžete dosáhnout přidělením jedinečné deklaraci `x:Key` atribut, který poskytuje popisný klíčem v `ResourceDictionary`, jak je znázorněno v následujícím příkladu kódu XAML:

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

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Přiřazen [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) vlastnost s použitím `StaticResource` – rozšíření značek. Všimněte si, že při `DataTemplate` na stránce je definován [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), by se mohl definovat také na ovládací prvek úrovni nebo na úrovni aplikace.

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

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Se přidá do [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) pomocí [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) metodu, která určuje `Key` řetězec, který se používá k odkaz `DataTemplate` při jeho načítání.

## <a name="summary"></a>Souhrn

Tento článek obsahuje bylo vysvětleno, jak vytvořit datové šablony, jako vložené, z vlastního typu, nebo v [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Vložená šablona by měla použít, pokud není nutné opakovaně používat šablony jinde. Alternativně můžete použít opakovaně datové šablony tak, že definujete ho jako vlastní typ, nebo jako ovládací prvek úrovni úrovni stránky nebo aplikace na úrovni prostředků.


## <a name="related-links"></a>Související odkazy

- [Vzhled buňky](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Datové šablony (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
