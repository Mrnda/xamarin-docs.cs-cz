---
title: Vytvoření vazby ze ControlTemplate
description: Vazby šablony umožňují vytvořit vazbu na veřejné vlastnosti ovládacích prvků v šabloně ovládacího prvku k datům povolení hodnot vlastností ovládacích prvků v šabloně řízení snadno změnit. Tento článek ukazuje použití šablony vazby k provedení vazby dat z šablony ovládacího prvku.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 3b306c79aea9bd2192aa73eddcf95790a9b24353
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="binding-from-a-controltemplate"></a>Vytvoření vazby ze ControlTemplate

_Vazby šablony umožňují vytvořit vazbu na veřejné vlastnosti ovládacích prvků v šabloně ovládacího prvku k datům povolení hodnot vlastností ovládacích prvků v šabloně řízení snadno změnit. Tento článek ukazuje použití šablony vazby k provedení vazby dat z šablony ovládacího prvku._

A [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) se používá k vytvoření vazby ovládacího prvku v šabloně ovládacího prvku vazbu vlastnosti na nadřazeného *cíl* zobrazení, která vlastní šablonu ovládacího prvku. Například místo definování textu zobrazovaného [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance uvnitř [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/), můžete použít vazbu šablony k vytvoření vazby [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) vlastnost pro vazbu vlastnosti, které definují text, který se zobrazí.

A [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) je podobná existující [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/)kromě toho, že *zdroj* z `TemplateBinding` je vždy automaticky nastavena na hodnotu parent z *cíl* zobrazení, která vlastní šablonu ovládacího prvku. Ale Všimněte si, že pomocí `TemplateBinding` mimo [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) není podporován.

## <a name="creating-a-templatebinding-in-xaml"></a>Vytváření TemplateBinding v jazyce XAML

V jazyce XAML [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) je vytvořený pomocí [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) – rozšíření značek, jak je ukázáno v následujícím příkladu kódu:

```xaml
<ControlTemplate x:Key="TealTemplate">
  <Grid>
    ...
    <Label Text="{TemplateBinding Parent.HeaderText}" ... />
    ...
    <Label Text="{TemplateBinding Parent.FooterText}" ... />
  </Grid>
</ControlTemplate>
```

Místo sady [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) vlastnosti na statický text vlastnosti můžete použít šablonu vazby pro vazbu vlastnosti vazbu na nadřazeného *cíl* zobrazení, která vlastní [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). Ale Všimněte si, že šablona vazby vázat na `Parent.HeaderText` a `Parent.FooterText`, místo `HeaderText` a `FooterText`. Důvodem je, že v tomto příkladu jsou definovány vlastnosti vazbu na nadřazený z *cíl* zobrazit, místo nadřazeným prvkem, jak je ukázáno v následujícím příkladu kódu:

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

*Zdroj* šablony je vazba vždy automaticky nastavena na hodnotu parent z *cíl* zobrazení, které vlastní ovládací prvek šablony, která tady je [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) instance. Šablona používá vazba [ `Parent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Parent/) vlastnost vrátit nadřazený element `ContentView` instanci, která je [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance. Proto pomocí [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) v [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) k vytvoření vazby `Parent.HeaderText` a `Parent.FooterText` vyhledá vazbu vlastnosti, které jsou definovány na `ContentPage`, jako ukázáno v následujícím příkladu kódu:

```csharp
public static readonly BindableProperty HeaderTextProperty =
  BindableProperty.Create ("HeaderText", typeof(string), typeof(HomePage), "Control Template Demo App");
public static readonly BindableProperty FooterTextProperty =
  BindableProperty.Create ("FooterText", typeof(string), typeof(HomePage), "(c) Xamarin 2016");

public string HeaderText {
  get { return (string)GetValue (HeaderTextProperty); }
}

public string FooterText {
  get { return (string)GetValue (FooterTextProperty); }
}
```

Výsledkem je vidět na následujících snímcích obrazovky vzhled:

![](template-binding-images/teal-theme.png "Pomocí šablony vazeb šedozelená šablony ovládacího prvku")

## <a name="creating-a-templatebinding-in-c35"></a>Vytváření TemplateBinding v jazyce C&#35;

V jazyce C# [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) je vytvořená pomocí `TemplateBinding` konstruktoru, jak je ukázáno v následujícím příkladu kódu:

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var topLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    topLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.HeaderText"));
    ...
    var bottomLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    bottomLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.FooterText"));
    ...
  }
}
```

Místo sady [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) vlastnosti na statický text vlastnosti můžete použít šablonu vazby pro vazbu vlastnosti vazbu na nadřazeného *cíl* zobrazení, která vlastní [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). Vazba šablony je vytvořena pomocí [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) metoda, zadání [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) instance jako druhý parametr. Všimněte si, že vazby šablony vytvořit vazbu na `Parent.HeaderText` a `Parent.FooterText`, protože jsou definovány vlastnosti vazbu na nadřazený z *cíl* zobrazit, místo nadřazeným prvkem, jak je ukázáno v následujícím příkladu kódu:

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    Content = new ContentView {
      ControlTemplate = tealTemplate,
      Content = new StackLayout {
        ...
      },
      ...
    };
    ...
  }
}
```

Vazbu vlastnosti jsou definované na `ContentPage`, jak je uvedeno výše.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>Vytvoření vazby BindableProperty ViewModel vlastnost

Jak už jsme si říkali, [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) vazbu vlastnosti na nadřazeného vytvoří vazbu ovládacího prvku v šabloně ovládacího prvku *cíl* zobrazení, která vlastní šablonu ovládacího prvku. Tyto vlastnosti vazbu pak mohou být vázány na vlastnosti v ViewModels.

Následující příklad kódu definuje dvě vlastnosti na ViewModel:

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

`HeaderText` a `FooterText` ViewModel vlastnosti mohou být vázány na, jak je znázorněno v následujícím příkladu kódu XAML:

```xaml
<ContentPage xmlns:local="clr-namespace:SimpleTheme;assembly=SimpleTheme"
             HeaderText="{Binding HeaderText}" FooterText="{Binding FooterText}" ...>
    <ContentPage.BindingContext>
        <local:HomePageViewModel />
    </ContentPage.BindingContext>
    <ContentView ControlTemplate="{StaticResource TealTemplate}" ...>
    ...
    </ContentView>
</ContentPage>
```

`HeaderText` a `FooterText` vazbu vlastnosti je vázána na `HomePageViewModel.HeaderText` a `HomePageViewModel.FooterText` vlastnosti z důvodu nastavení [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) na instanci systému `HomePageViewModel` – třída. Výsledkem je obecně – vlastnosti ovládacích prvků v [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) je vázána na [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instance na [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), která zase navázat na ViewModel vlastnosti.

Ekvivalentní kódu C# je znázorněno v následujícím příkladu kódu:

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    BindingContext = new HomePageViewModel ();
    this.SetBinding (HeaderTextProperty, "HeaderText");
    this.SetBinding (FooterTextProperty, "FooterText");
    ...
  }
}
```

Další informace o vazbě dat na ViewModels najdete v tématu [z vazby dat na rozhraní MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Souhrn

Tento článek ukázán pomocí šablony vazby k provedení vazby dat z šablony ovládacího prvku. Vazby šablony umožňují vytvořit vazbu na veřejné vlastnosti ovládacích prvků v šabloně ovládacího prvku k datům povolení hodnot vlastností ovládacích prvků v šabloně řízení snadno změnit.



## <a name="related-links"></a>Související odkazy

- [Základy vazba dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Z vazby dat na rozhraní MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [Jednoduché motiv šablony vazby (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [Jednoduché motiv šablony vazby a ViewModel (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)
- [Element ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
