---
title: Vytváření Xamarin.Forms DataTemplateSelector
description: Tento článek ukazuje, jak vytvářet a využívat DataTemplateSelector, který slouží k výběru DataTemplate za běhu na základě hodnoty vlastnosti vázané na data.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: a72777c7e51e96a8e123ecd85ad0aa24fc60fc6c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994514"
---
# <a name="creating-a-xamarinforms-datatemplateselector"></a>Vytváření Xamarin.Forms DataTemplateSelector

_DataTemplateSelector je možné vybrat šablonu DataTemplate za běhu na základě hodnoty vlastnosti vázané na data. To umožňuje více DataTemplates uplatňovat stejný typ objektu, pro přizpůsobení vzhledu konkrétní objekty. Tento článek ukazuje, jak vytvářet a využívat DataTemplateSelector._

Výběr šablon dat umožňuje scénáře, jako [ `ListView` ](xref:Xamarin.Forms.ListView) vazby ke kolekci objektů, kde vzhled všech objektů `ListView` můžete zvolí za běhu výběrem šablony dat vrací konkrétní [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="creating-a-datatemplateselector"></a>Vytváření DataTemplateSelector

Výběr šablon dat je implementováno tak, že vytvoříte třídu, která dědí z [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector). `OnSelectTemplate` Poté přepsána metoda vrátit konkrétní [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), jak je znázorněno v následujícím příkladu kódu:

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

`OnSelectTemplate` Metoda vrátí odpovídající šablony založené na hodnotě `DateOfBirth` vlastnost. Hodnota je šablonu, kterou chcete vrátit `ValidTemplate` vlastnost nebo `InvalidTemplate` vlastnost, která se nastavují při využívání `PersonDataTemplateSelector`.

Instance třídy selektor šablony data pak můžete přiřadit k Xamarin.Forms vlastností ovládacích prvků, jako [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1). Seznam vlastností platný, naleznete v tématu [vytváření šablonu DataTemplate](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Omezení

[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) instance mají následující omezení:

- `DataTemplateSelector` Podtřídy musí vždycky vrátí stejnou šablonu na stejná data, když dotazovat více než jednou.
- `DataTemplateSelector` Podtřídy nesmí vrátit jiné `DataTemplateSelector` podtřídy.
- `DataTemplateSelector` Podtřídy nesmí vracet nových instancí `DataTemplate` při každém volání. Místo toho musí vrátit stejnou instanci. Pokud tak neučiníte vytvoří nevracení paměti a zakáže virtualizace.
- V systému Android se může existovat více než 20 různých datových šablon `ListView`.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>Využívání DataTemplateSelector v XAML

V XAML `PersonDataTemplateSelector` dá vytvořit instance deklarováním jako prostředek, jak je znázorněno v následujícím příkladu kódu:

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

Tato stránka úroveň [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) definuje dva [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) instancí a `PersonDataTemplateSelector` instance. `PersonDataTemplateSelector` Instance sady jeho `ValidTemplate` a `InvalidTemplate` vlastnosti na příslušné `DataTemplate` instance pomocí `StaticResource` – rozšíření značek. Všimněte si, že při prostředky jsou definovány na stránce [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), může být také definován na ovládací prvek úrovni nebo na úrovni aplikace.

`PersonDataTemplateSelector` Instance je využívána ji přiřadíte [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) vlastnost, jak je znázorněno v následujícím příkladu kódu:

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

V době běhu [ `ListView` ](xref:Xamarin.Forms.ListView) volání `PersonDataTemplateSelector.OnSelectTemplate` metoda pro každou z položek v kolekci podkladové volání předávání dat objektu jako `item` parametru. [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) , Který je vrácen metodu se následně použije na tento objekt.

Na následujících snímcích obrazovky zobrazit výsledek [ `ListView` ](xref:Xamarin.Forms.ListView) použití `PersonDataTemplateSelector` každému objektu ve zdrojové kolekce:

![](selector-images/data-template-selector.png "ListView s výběr šablon dat")

Žádné `Person` objekt, který má `DateOfBirth` hodnota vlastnosti větší než nebo rovna hodnotě 1980 se zobrazí zeleně, zbývající objekty, které se zobrazí červeně.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>Využívání DataTemplateSelector v jazyce C&num;

V jazyce C# `PersonDataTemplateSelector` můžete vytvořit instanci a přiřazená [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) vlastnost, jak je znázorněno v následujícím příkladu kódu:

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

`PersonDataTemplateSelector` Instance sady jeho `ValidTemplate` a `InvalidTemplate` vlastnosti na příslušné [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) instance vytvořené `SetupDataTemplates` metody. V době běhu [ `ListView` ](xref:Xamarin.Forms.ListView) volání `PersonDataTemplateSelector.OnSelectTemplate` metoda pro každou z položek v kolekci podkladové volání předávání dat objektu jako `item` parametru. `DataTemplate` , Který je vrácen metodu se následně použije na tento objekt.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvářet a využívat [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector). A `DataTemplateSelector` můžete použít k výběru [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) za běhu na základě hodnoty vlastnosti vázané na data. To umožňuje více `DataTemplate` instance, které chcete použít pro stejný typ objektu, pro přizpůsobení vzhledu konkrétní objekty.


## <a name="related-links"></a>Související odkazy

- [Výběr šablon dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](xref:Xamarin.Forms.DataTemplateSelector)
