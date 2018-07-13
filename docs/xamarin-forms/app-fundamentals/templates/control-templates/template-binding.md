---
title: Vazba v objektu Xamarin.Forms ControlTemplate
description: Vazby šablony umožňují svázat ovládací prvky v šabloně ovládacího prvku k datům veřejné vlastnosti, povolení hodnoty vlastností ovládacích prvků v šabloně ovládacího prvku snadno změnit. Tento článek ukazuje použití vazby šablony k provedení vazby dat ze šablony ovládacího prvku.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 86de2ad6009365b3debbe1a2310651002b023219
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995561"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Vazba v objektu Xamarin.Forms ControlTemplate

_Vazby šablony umožňují svázat ovládací prvky v šabloně ovládacího prvku k datům veřejné vlastnosti, povolení hodnoty vlastností ovládacích prvků v šabloně ovládacího prvku snadno změnit. Tento článek ukazuje použití vazby šablony k provedení vazby dat ze šablony ovládacího prvku._

A [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) slouží k vytvoření vazby ovládacího prvku v šabloně ovládacího prvku na vlastnost s vazbou na nadřazený prvek *cílové* zobrazení, které vlastní šablonu ovládacího prvku. Například místo definování samostatné výstrahy text, zobrazený [ `Label` ](xref:Xamarin.Forms.Label) instance uvnitř [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate), vazba šablony můžete použít k vytvoření vazby [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) vlastnost podporující vazby vlastnosti, které definují text, který se má zobrazit.

A [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) je podobný existující [ `Binding` ](xref:Xamarin.Forms.Binding), s tím rozdílem, že *zdroj* z `TemplateBinding` vždy automaticky nastaven na nadřazený *cílové* zobrazení, které vlastní šablonu ovládacího prvku. Mějte však na paměti, že při použití `TemplateBinding` mimo [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) se nepodporuje.

## <a name="creating-a-templatebinding-in-xaml"></a>Vytváření na TemplateBinding v XAML

V XAML [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) je vytvořený pomocí [ `TemplateBinding` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) – rozšíření značek, jak je ukázáno v následujícím příkladu kódu:

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

Místo nastavení [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) vlastnosti na statický text, slouží vlastnosti vazby šablony vytvořit vazbu vlastnosti umožňující vazbu na nadřazený *cílové* zobrazení, které vlastní [ `ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). Mějte však na paměti, že vazby šablony svázat `Parent.HeaderText` a `Parent.FooterText`, spíše než `HeaderText` a `FooterText`. Důvodem je, že v tomto příkladu jsou definovány vlastnosti umožňující vazbu na dvě úrovně nadřazený z *cílové* zobrazit, nikoli nadřazené, jak je ukázáno v následujícím příkladu kódu:

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

*Zdroje* šablony vazby vždy automaticky nastaven na nadřazený prvek *cílové* zobrazení, které vlastní šablonu ovládacího prvku, který následuje [ `ContentView` ](xref:Xamarin.Forms.ContentView) instance. Šablona používá vazba [ `Parent` ](xref:Xamarin.Forms.Element.Parent) vlastnost vrátit nadřazený element `ContentView` instanci, která je [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance. Proto použití [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) v [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) vytvořit vazbu na `Parent.HeaderText` a `Parent.FooterText` vyhledá vlastnosti umožňující vazbu, které jsou definovány na `ContentPage`, jako ukázáno v následujícím příkladu kódu:

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

Výsledkem je vzhled je znázorněno na následujících snímcích obrazovky:

![](template-binding-images/teal-theme.png "Používání vazeb šablony šedozelená šablony ovládacího prvku")

## <a name="creating-a-templatebinding-in-c35"></a>Vytváření na TemplateBinding v jazyce C&#35;

V jazyce C# [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) je vytvořená pomocí `TemplateBinding` konstruktoru, jak je ukázáno v následujícím příkladu kódu:

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

Místo nastavení [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) vlastnosti na statický text, slouží vlastnosti vazby šablony vytvořit vazbu vlastnosti umožňující vazbu na nadřazený *cílové* zobrazení, které vlastní [ `ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). Vazba šablony je vytvořena pomocí [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) metodu, zadáte [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) instance jako druhý parametr. Všimněte si, že vazby šablony svázat `Parent.HeaderText` a `Parent.FooterText`, protože umožňujících vazbu vlastnosti jsou definované na dvě úrovně nadřazený z *cílové* zobrazit, nikoli nadřazené, jak je ukázáno v následujícím příkladu kódu:

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

S možností vazby vlastnosti jsou definované na `ContentPage`, jak je uvedeno výše.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>Vytvoření vazby BindableProperty na vlastnost ViewModel

Jak už jsme uvedli, [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) vytvoří vazbu ovládacího prvku v šabloně ovládacího prvku na vlastnost s vazbou na nadřazený prvek *cílové* zobrazení, které vlastní šablonu ovládacího prvku. Tyto vlastnosti umožňující vazbu pak mohou být vázány na vlastnosti v modely ViewModel.

Následující příklad kódu definuje dvě vlastnosti na ViewModel:

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

`HeaderText` a `FooterText` ViewModel vlastnosti může být vázána na, jak je znázorněno v následujícím příkladu kódu XAML:

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

`HeaderText` a `FooterText` umožňujících vazbu vlastnosti, které jsou vázány na `HomePageViewModel.HeaderText` a `HomePageViewModel.FooterText` vlastnosti z důvodu nastavení [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) do instance `HomePageViewModel` třídy. Výsledkem je celkově vlastnosti ovládacího prvku v [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) svázaný s [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instancí na [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), která zase svázat Vlastnosti ViewModel.

Ekvivalentní kód jazyka C# můžete vidět v následujícím příkladu kódu:

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

Další informace o vytváření datových vazeb modely ViewModels najdete v tématu [od vazeb dat po MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Souhrn

Tento článek jsme vám ukázali pomocí vazby šablony k provedení vazby dat ze šablony ovládacího prvku. Vazby šablony umožňují svázat ovládací prvky v šabloně ovládacího prvku k datům veřejné vlastnosti, povolení hodnoty vlastností ovládacích prvků v šabloně ovládacího prvku snadno změnit.



## <a name="related-links"></a>Související odkazy

- [Základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Od vazeb dat po mvvm](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [Jednoduché motivu se vazba šablony (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [Jednoduché motiv se vazba šablony a ViewModel (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](xref:Xamarin.Forms.TemplateBinding)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentView](xref:Xamarin.Forms.ContentView)
