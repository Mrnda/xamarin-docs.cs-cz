---
title: Vytvoření objektu ControlTemplate
description: Šablony ovládacích prvků lze definovat na úrovni aplikace nebo na úrovni stránky. Tento článek ukazuje, jak vytvářet a využívat šablon ovládacích prvků.
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: b83668f6836b1d5d98f67592bf3e2b01e7319edc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998184"
---
# <a name="creating-a-controltemplate"></a>Vytvoření objektu ControlTemplate

_Šablony ovládacích prvků lze definovat na úrovni aplikace nebo na úrovni stránky. Tento článek ukazuje, jak vytvářet a využívat šablon ovládacích prvků._

## <a name="creating-a-controltemplate-in-xaml"></a>Vytvoření objektu ControlTemplate v XAML

K definování [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) na úrovni aplikace [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) musí být přidané do `App` třídy. Ve výchozím nastavení, všechny aplikace Xamarin.Forms, které jsou vytvořené ze šablony pomocí **aplikace** třídu pro implementaci [ `Application` ](xref:Xamarin.Forms.Application) podtřídy. Chcete-li deklarovat `ControlTemplate` na úrovni aplikace v aplikačním `ResourceDictionary` pomocí XAML, výchozí **aplikace** třídy je nutné nahradit XAML **aplikace** třídy a přidružené použití modelu code-behind, jako můžete vidět v následujícím příkladu kódu:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.App">
    <Application.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                <Grid>
                    ...
                    <BoxView ... />
                    <Label Text="Control Template Demo App"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                    <ContentPresenter ... />
                    <BoxView Color="Teal" ... />
                    <Label Text="(c) Xamarin 2016"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                </Grid>
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Každý [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) jako opakovaně použitelný objekt v je vytvořena instance [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary).  Toho můžete dosáhnout přidělením jedinečné deklaraci `x:Key` atribut, který poskytuje popisný klíčem v `ResourceDictionary`.

Následující příklad kódu ukazuje přidruženého `App` použití modelu code-behind:

```csharp
public partial class App : Application
{
  public App ()
  {
    InitializeComponent ();
    MainPage = new HomePage ();
  }
}
```

A také nastavení [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) vlastnost modelu code-behind musíte také zavolat `InitializeComponent` metoda načíst a analyzovat související XAML.

Následující příklad kódu ukazuje [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) použití `TealTemplate` k [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0"
                 ControlTemplate="{StaticResource TealTemplate}">
        <StackLayout VerticalOptions="CenterAndExpand">
            <Label Text="Welcome to the app!" HorizontalOptions="Center" />
            <Button Text="Change Theme" Clicked="OnButtonClicked" />
        </StackLayout>
    </ContentView>
</ContentPage>
```

`TealTemplate` Přiřazen [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate) vlastnost s použitím `StaticResource` – rozšíření značek. [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) Je nastavena na [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) , který definuje obsah, který se má zobrazit na [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). Tento obsah se zobrazí podle [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) součástí `TealTemplate`. Výsledkem je vzhled je znázorněno na následujících snímcích obrazovky:

![](creating-images/teal-theme.png "Šablony ovládacího prvku šedozelená")

### <a name="re-theming-an-application-at-runtime"></a>RE – motivy aplikace za běhu

Kliknutím **změnit motiv** tlačítko spustí `OnButtonClicked` metoda, která je znázorněna v následujícím příkladu kódu:

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

Tato metoda nahrazuje aktivní [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) instance s alternativním `ControlTemplate` instance, což vede na následujícím snímku obrazovky:

![](creating-images/aqua-theme.png "Šablony akvamarínový ovládacího prvku")

> [!NOTE]
> Na `ContentPage`, `Content` je možné přiřadit vlastnosti a `ControlTemplate` může být nastavena také vlastnost. Když k tomu dojde, pokud `ControlTemplate` obsahuje `ContentPresenter` instance obsahu přiřazeného k `Content` vlastnost se zobrazí ve `ContentPresenter` v rámci `ControlTemplate`.

### <a name="setting-a-controltemplate-with-a-style"></a>Nastavení šablony ControlTemplate stylem

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) lze také použít prostřednictvím [ `Style` ](xref:Xamarin.Forms.Style) dále rozbalte možnost motiv. Toho lze dosáhnout vytvořením *implicitní* nebo *explicitní* styl zobrazení cíle v [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)a nastavení `ControlTemplate` vlastnosti cíle Zobrazit [ `Style` ](xref:Xamarin.Forms.Style) instance. Následující příklad kódu ukazuje *implicitní* styl, který je byly přidány na úrovni aplikace [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

Protože [ `Style` ](xref:Xamarin.Forms.Style) instance je *implicitní*, se použijí na všechny `ContentView` instancí aplikace. Proto je již nebude nutné nastavit [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate) vlastnost, jak je ukázáno v následujícím příkladu kódu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

Další informace o stylech najdete v tématu [styly](~/xamarin-forms/user-interface/styles/index.md).

### <a name="creating-a-controltemplate-at-page-level"></a>Vytvoření objektu ControlTemplate na úrovni stránky

Kromě vytvoření [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) instance na úrovni aplikace, je lze také vytvořit na úrovni stránky, jak je znázorněno v následujícím příkladu kódu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                ...
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
        ...
    </ContentView>
</ContentPage>
```

Při přidávání [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) na úrovni stránky [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) se přidá do [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)a pak `ControlTemplate` instance jsou zahrnuté v `ResourceDictionary`.

## <a name="creating-a-controltemplate-in-c35"></a>Vytvoření objektu ControlTemplate v jazyce C&#35;

Chcete-li definovat [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) na úrovni aplikace `class` musí být vytvořeny, které představuje `ControlTemplate`. Třída musí být odvozený z: [rozložení](~/xamarin-forms/user-interface/layouts/index.md) používá pro šablonu, jak je znázorněno v následujícím příkladu kódu:

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var contentPresenter = new ContentPresenter ();
    Children.Add (contentPresenter, 0, 1);
    Grid.SetColumnSpan (contentPresenter, 2);
    ...
  }
}

class AquaTemplate : Grid
{
  ...
}
```

`AquaTemplate` Třída je stejné jako `TealTemplate` třídy, s tím rozdílem, že se používají různé barvy pro [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) a [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) vlastnosti.

Následující příklad kódu ukazuje [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) použití `TealTemplate` k [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
public class HomePageCS : ContentPage
{
  ...
  ControlTemplate tealTemplate = new ControlTemplate (typeof(TealTemplate));
  ControlTemplate aquaTemplate = new ControlTemplate (typeof(AquaTemplate));

  public HomePageCS ()
  {
    var button = new Button { Text = "Change Theme" };
    var contentView = new ContentView {
      Padding = new Thickness (0, 20, 0, 0),
      Content = new StackLayout {
        VerticalOptions = LayoutOptions.CenterAndExpand,
        Children = {
          new Label { Text = "Welcome to the app!", HorizontalOptions = LayoutOptions.Center },
          button
        }
      },
      ControlTemplate = tealTemplate
    };
    ...
    Content = contentView;
  }
}
```

[ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) Zadáním typu třídy, které definují šablon ovládacích prvků v se vytvářejí instance `ControlTemplate` konstruktoru.

[ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) Je nastavena na [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) , který definuje obsah, který se má zobrazit na [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). Tento obsah se zobrazí podle [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) součástí `TealTemplate`. Změna motivu za běhu, abyste dříve se používá stejný mechanismus uvedených `AquaTheme`.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak vytvářet a využívat šablon ovládacích prvků. Šablony ovládacích prvků lze definovat na úrovni aplikace nebo na úrovni stránky.


## <a name="related-links"></a>Související odkazy

- [Styly](~/xamarin-forms/user-interface/styles/index.md)
- [Jednoduché motivu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
- [ContentView](xref:Xamarin.Forms.ContentView)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
