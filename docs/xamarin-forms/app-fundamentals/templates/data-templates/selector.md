---
title: Vytváření DataTemplateSelector
description: DataTemplateSelector lze vybrat šablonu DataTemplate za běhu na základě hodnoty vlastnosti vázané na data. To umožňuje více DataTemplates má být použita pro stejný typ objektu, chcete-li přizpůsobit vzhled konkrétní objekty. Tento článek ukazuje, jak vytvářet a využívat DataTemplateSelector.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: ad8cce32cb9cfe2ea5e78f11b1250440371a9851
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847189"
---
# <a name="creating-a-datatemplateselector"></a>Vytváření DataTemplateSelector

_DataTemplateSelector lze vybrat šablonu DataTemplate za běhu na základě hodnoty vlastnosti vázané na data. To umožňuje více DataTemplates má být použita pro stejný typ objektu, chcete-li přizpůsobit vzhled konkrétní objekty. Tento článek ukazuje, jak vytvářet a využívat DataTemplateSelector._

Selektor šablony dat umožňuje scénáře, jako například [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) vazby ke kolekci objektů, kde vzhled všech objektů `ListView` mohou být vybrána v době běhu výběrem šablony dat, vrácení konkrétní [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/).

## <a name="creating-a-datatemplateselector"></a>Vytváření DataTemplateSelector

Selektor šablony dat je implementováno modulem vytváření třídy, která dědí z [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/). `OnSelectTemplate` Metoda je pak potlačena za účelem vrátit konkrétní [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), jak ukazuje následující příklad kódu:

```csharp
public class PersonDataTemplateSelector : DataTemplateSelector
{
  public DataTemplate ValidTemplate { get; set; }
  public DataTemplate InvalidTemplate { get; set; }

  protected override DataTemplate OnSelectTemplate (object item, BindableObject container)
  {
    return ((Person)item).DateOfBirth.Year >= 1980 ? ValidTemplate : InvalidTemplate;
  }
}
```

`OnSelectTemplate` Metoda vrátí příslušné šablony založené na hodnotu `DateOfBirth` vlastnost. Pro šablonu, vrátí se hodnota `ValidTemplate` vlastnost nebo `InvalidTemplate` vlastnosti, která jsou nastavena při využívání `PersonDataTemplateSelector`.

Instance třídy pro výběr šablony dat lze potom přiřadit k vlastnosti ovládacích prvků Xamarin.Forms, jako [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/). Seznam vlastností platná najdete v tématu [vytváření šablonu DataTemplate](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Omezení

[`DataTemplateSelector`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) instance mají následující omezení:

- `DataTemplateSelector` Podtřídami musí vracet vždycky stejné šablony pro stejná data Pokud dotaz vícekrát.
- `DataTemplateSelector` Podtřídami nesmí vrátit jiné `DataTemplateSelector` podtřídy.
- `DataTemplateSelector` Podtřídami nesmí vrátit nové instance třídy `DataTemplate` při každém volání. Místo toho musí vrátit stejnou instanci. Tak neučiníte vytvoří nevrácenou pamětí a zakážete virtualizace.
- V systému Android, může být více než 20 šablony různých datových za `ListView`.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>Využívání DataTemplateSelector v jazyce XAML

V jazyce XAML `PersonDataTemplateSelector` se dá vytvořit instance deklarováním jako prostředek, jak je znázorněno v následujícím příkladu kódu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Selector;assembly=Selector" x:Class="Selector.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="validPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <DataTemplate x:Key="invalidPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <local:PersonDataTemplateSelector x:Key="personDataTemplateSelector"
                ValidTemplate="{StaticResource validPersonTemplate}"
                InvalidTemplate="{StaticResource invalidPersonTemplate}" />
        </ResourceDictionary>
    </ContentPage.Resources>
  ...
</ContentPage>
```

Tato stránka úroveň [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definuje dvě [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) instancí a `PersonDataTemplateSelector` instance. `PersonDataTemplateSelector` Instance sady jeho `ValidTemplate` a `InvalidTemplate` vlastnosti na příslušné `DataTemplate` instance pomocí `StaticResource` – rozšíření značek. Všimněte si, že při prostředky jsou definovány na stránce [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), mohou být také definovány na úrovni ovládací prvek nebo na úrovni aplikace.

`PersonDataTemplateSelector` Instance je spotřebovávají jeho pro přiřazení [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) vlastnost, jak je znázorněno v následujícím příkladu kódu:

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

V době běhu [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) volání `PersonDataTemplateSelector.OnSelectTemplate` metoda pro každou položku v kolekci základní pomocí volání předávání datový objekt jako `item` parametr. [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) , Vrátí metoda se potom použije pro tento objekt.

Na následujících snímcích obrazovky zobrazit výsledek [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) použití `PersonDataTemplateSelector` pro každý objekt v kolekci základní:

![](selector-images/data-template-selector.png "ListView s selektor šablony dat")

Všechny `Person` objekt, který má `DateOfBirth` hodnotu vlastnosti větší než nebo rovna hodnotě 1980 se zobrazí zeleně, se zbývající objekty, které se zobrazí červeně.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>Využívání DataTemplateSelector v jazyce C&num;

V jazyce C# `PersonDataTemplateSelector` může být vytvořena instance a přiřazení [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) vlastnost, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class HomePageCS : ContentPage
{
  DataTemplate validTemplate;
  DataTemplate invalidTemplate;

  public HomePageCS ()
  {
    ...
    SetupDataTemplates ();
    var listView = new ListView {
      ItemsSource = people,
      ItemTemplate = new PersonDataTemplateSelector {
        ValidTemplate = validTemplate,
        InvalidTemplate = invalidTemplate }
    };

    Content = new StackLayout {
      Margin = new Thickness (20),
      Children = {
        ...
        listView
      }
    };
  }
  ...  
}
```

`PersonDataTemplateSelector` Instance sady jeho `ValidTemplate` a `InvalidTemplate` vlastnosti na příslušné [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) instance vytvořené `SetupDataTemplates` metoda. V době běhu [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) volání `PersonDataTemplateSelector.OnSelectTemplate` metoda pro každou položku v kolekci základní pomocí volání předávání datový objekt jako `item` parametr. `DataTemplate` , Vrátí metoda se potom použije pro tento objekt.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvářet a využívat [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/). A `DataTemplateSelector` je možné vybrat [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) za běhu na základě hodnoty vlastnosti vázané na data. To umožňuje více `DataTemplate` instance, který bude použit na stejný typ objektu, chcete-li přizpůsobit vzhled konkrétní objekty.


## <a name="related-links"></a>Související odkazy

- [Výběr šablony dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)
