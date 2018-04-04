---
title: Vytváření ControlTemplate
description: Ovládací prvek šablony může být definováno na úrovni aplikace nebo na úrovni stránky. Tento článek ukazuje, jak vytvářet a využívat šablon ovládacích prvků.
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: e8a0695969609a4b0bbeb38896adae9a7c16ed07
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-controltemplate"></a>Vytváření ControlTemplate

_Ovládací prvek šablony může být definováno na úrovni aplikace nebo na úrovni stránky. Tento článek ukazuje, jak vytvářet a využívat šablon ovládacích prvků._

## <a name="creating-a-controltemplate-in-xaml"></a>Vytváření ControlTemplate v jazyce XAML

K definování [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) na úrovni aplikace, [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) musí být přidaný do `App` třídy. Ve výchozím nastavení, použijte všechny Xamarin.Forms aplikace vytvořené ze šablony **aplikace** třídu pro implementaci [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) podtřídy. Deklarovat `ControlTemplate` na úrovni aplikace, do aplikace `ResourceDictionary` pomocí XAML, výchozí **aplikace** třídy je nutné nahradit XAML **aplikace** třídy a přidružené kódu jako Zobrazuje následující příklad kódu:

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

Každý [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) instance je vytvořit jako objekt opakovaně použitelné v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/).  Toho dosáhnete tím, že každá deklarace jedinečný `x:Key` atribut, který poskytuje popisný klíč v `ResourceDictionary`.

Následující příklad kódu ukazuje přidruženého `App` kódu:

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

I nastavení [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) vlastnost modelu code-behind musíte také zavolat `InitializeComponent` metoda se načíst a analyzovat přidružené XAML.

Následující příklad kódu ukazuje [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) použití `TealTemplate` k [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

`TealTemplate` Je přiřazena k [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) vlastnost pomocí `StaticResource` – rozšíření značek. [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Je nastavena na [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) , který definuje obsah, který se má zobrazit [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Zobrazí se tento obsah pomocí [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) obsažené v `TealTemplate`. Výsledkem je vidět na následujících snímcích obrazovky vzhled:

![](creating-images/teal-theme.png "Šablona šedozelená ovládacího prvku")

### <a name="re-theming-an-application-at-runtime"></a>Opětovná motivů aplikace za běhu

Kliknutím **změnit motiv** tlačítko spustí `OnButtonClicked` metodu, která je znázorněno v následujícím příkladu kódu:

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

Tato metoda nahrazuje aktivní [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) instance s alternativním `ControlTemplate` instance, což vede k na následujícím snímku obrazovky:

![](creating-images/aqua-theme.png "Šablona aqua ovládacího prvku")

> [!NOTE]
> Na `ContentPage`, `Content` vlastnost lze přiřadit a `ControlTemplate` může být nastavena také vlastnost. Pokud k tomu dojde, pokud `ControlTemplate` obsahuje `ContentPresenter` instance obsahu přiřazeného k `Content` vlastnost bude předložený `ContentPresenter` v rámci `ControlTemplate`.

### <a name="setting-a-controltemplate-with-a-style"></a>Nastavení ControlTemplate s styl

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) lze použít také prostřednictvím [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) další rozbalte možnost motivu. Toho lze dosáhnout vytvořením *implicitní* nebo *explicitní* styl cílové zobrazení v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)a nastavení `ControlTemplate` vlastnost cíle zobrazení v [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance. Následující příklad kódu ukazuje *implicitní* styl, který je přidán na úrovni aplikace [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

Protože [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) instance je *implicitní*, se použijí na všechny `ContentView` instancí v aplikaci. Proto je již nebude nutné nastavit [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) vlastnosti, jak je ukázáno v následujícím příkladu kódu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

Další informace o styly najdete v tématu [styly](~/xamarin-forms/user-interface/styles/index.md).

### <a name="creating-a-controltemplate-at-page-level"></a>Vytváření ControlTemplate na úrovni stránky

Kromě vytváření [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) instance na úrovni aplikace, je lze také vytvořit na úrovni stránky, jak je znázorněno v následujícím příkladu kódu:

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

Při přidávání [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) na úrovni stránky [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) je přidán do [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)a potom `ControlTemplate` instance jsou zahrnuty v `ResourceDictionary`.

## <a name="creating-a-controltemplate-in-c35"></a>Vytváření ControlTemplate v jazyce C&#35;

K definování [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) na úrovni aplikace, `class` musí být vytvořen tento představuje `ControlTemplate`. Třída by měl být odvozen z [rozložení](~/xamarin-forms/user-interface/layouts/index.md) používá pro šablonu, jak je znázorněno v následujícím příkladu kódu:

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

`AquaTemplate` Třída je stejný jako `TealTemplate` třídy, s tím rozdílem, že různých barev, které se používají pro [ `BoxView.Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) a [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) vlastnosti.

Následující příklad kódu ukazuje [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) použití `TealTemplate` k [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

[ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) Vytváření instancí zadáním typu třídy, které definují šablon ovládacích prvků v `ControlTemplate` konstruktor.

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Je nastavena na [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) , který definuje obsah, který se má zobrazit [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Zobrazí se tento obsah pomocí [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) obsažené v `TealTemplate`. Chcete-li změnit motiv za běhu, aby dříve se používá stejné mechanismus uvedených `AquaTheme`.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak vytvářet a využívat šablon ovládacích prvků. Ovládací prvek šablony může být definováno na úrovni aplikace nebo na úrovni stránky.


## <a name="related-links"></a>Související odkazy

- [Styly](~/xamarin-forms/user-interface/styles/index.md)
- [Jednoduché motiv (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [Element ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
