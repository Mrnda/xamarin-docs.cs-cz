---
title: Stránka s kartami Xamarin.Forms
description: Xamarin.Forms TabbedPage se skládá ze seznamu karty a větší oblasti podrobností, každá karta načítání obsahu do oblasti podrobností. Tento článek ukazuje, jak používat TabbedPage procházení kolekce stránek.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 3eb978780222da2050fc91dfa41c68ef4bd3b6f4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996292"
---
# <a name="xamarinforms-tabbed-page"></a>Stránka s kartami Xamarin.Forms

_Xamarin.Forms TabbedPage se skládá ze seznamu karty a větší oblasti podrobností, každá karta načítání obsahu do oblasti podrobností. Tento článek ukazuje, jak používat TabbedPage procházení kolekce stránek._

## <a name="overview"></a>Přehled

Zobrazit následující snímky obrazovky [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) na jednotlivých platformách:

![](tabbed-page-images/tab1.png "Příklad TabbedPage")

Na následujících snímcích obrazovky zaměřit se na kartě Formát na jednotlivých platformách:

![](tabbed-page-images/tabbedpage-components.png "TabbedPage karta součásti")

Rozložení [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), jeho karty a je závislý na platformě:

- V systémech iOS zobrazí se seznam karet v dolní části obrazovky a oblasti podrobností je uveden výše. Každá karta obsahuje také obrázku ikony, které by se měly 30 x 30 PNG s průhlednost pro běžné řešení, 60 x 60 pro vysokým rozlišením a 90 x 90 pro iPhone 6 Plus řešení. Pokud je více než pět záložek *Další* se zobrazí karta, který lze použít pro přístup k další záložky. Další informace o načtení obrázků v aplikaci Xamarin.Forms, naleznete v tématu [práce s obrázky](~/xamarin-forms/user-interface/images.md). Další informace o požadavcích na ikonu najdete v tématu [vytváření aplikací s kartami](~/ios/user-interface/controls/creating-tabbed-applications.md).

    > [!NOTE]
  > Všimněte si, že `TabbedRenderer` pro iOS musí přepisovatelným `GetIcon` metodu, která slouží k načtení ikony karet ze zadaného zdroje. Toto přepsání díky tomu je možné použít Image SVG jako ikony na `TabbedPage`. Kromě toho lze zadat vybrané a nevybrané verze ikony.

- V Androidu seznam karet se zobrazí v horní části obrazovky ve výchozím nastavení a oblasti podrobností je níže. Seznam karet však můžete přesunout k dolnímu okraji obrazovky s konkrétní platformu. Další informace najdete v tématu [nastavení TabbedPage nástrojů umístění a barvy](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar).

    > [!NOTE]
  > Všimněte si, že při použití AppCompat na Androidu, každá karta také zobrazena ikona. Kromě toho `TabbedPageRenderer` pro Android AppCompat má přepisovatelným `SetTabIcon` metodu, která slouží k načtení ikony karet z vlastního `Drawable`. Toto přepsání díky tomu je možné použít Image SVG jako ikony na `TabbedPage`.

- Na tablety Windows – zařízení, na kartách nejsou vždy viditelné a uživatelé nemusí potažením dolů (nebo klikněte pravým tlačítkem, pokud mají připojena myš) Chcete-li zobrazit karty v `TabbedPage` (jak je vidět níže).

![](tabbed-page-images/windows-tabs.png "TabbedPage karet ve Windows")

## <a name="creating-a-tabbedpage"></a>Vytváření TabbedPage

Dva přístupy lze použít k vytvoření [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage):

- [Naplnění](#Populating_a_TabbedPage_with_a_Page_Collection) [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) sbírka podřízené [ `Page` ](xref:Xamarin.Forms.Page) objekty, jako například kolekci [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instancí.
- [Přiřadit](#Populating_a_TabbedPage_with_a_Template) kolekci [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) vlastnosti a přiřazení [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) k [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) vlastnost vrátit stránky pro objekty v kolekci.

Pomocí obou metod [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) zobrazí každé stránky uživatelé vyberou každé kartě.

> [!NOTE]
> Doporučujeme [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) mělo být vyplněno pomocí [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) a [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)pouze instance. To vám pomůže zajistit konzistentní uživatelské prostředí na všech platformách.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>Naplnění TabbedPage s kolekcí stránky

Následující příklad ukazuje kód XAML [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) vytvářený naplnění kolekce podřízených [ `Page` ](xref:Xamarin.Forms.Page) objekty:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:TabbedPageWithNavigationPage;assembly=TabbedPageWithNavigationPage"
            x:Class="TabbedPageWithNavigationPage.MainPage">
    <local:TodayPage />
    <NavigationPage Title="Schedule" Icon="schedule.png">
        <x:Arguments>
            <local:SchedulePage />
        </x:Arguments>
    </NavigationPage>
</TabbedPage>
```

Následující příklad kódu ukazuje ekvivalent [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) vytvořené v jazyce C#:

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    var navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.Icon = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Se naplní dva podřízené [ `Page` ](xref:Xamarin.Forms.Page) objekty. Je prvním podřízeným objektem [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance a druhá karta je [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) obsahující `ContentPage` instance.

> [!NOTE]
> [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Nepodporuje uživatelského rozhraní virtualizace. Proto se ovlivnil výkon, pokud `TabbedPage` obsahuje příliš mnoho podřízených elementů.

Následující snímky obrazovky zobrazit `TodayPage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instanci, která je znázorněna na *Dnes* kartu:

![](tabbed-page-images/today-page.png "ContentPage v TabbedPage")

Výběr *plán* kartě zobrazí `SchedulePage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instanci, která je zabalena v [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instanci a je zobrazena ve Následující snímek obrazovky:

![](tabbed-page-images/schedule-page.png "NavigationPage v TabbedPage")

Informace o rozložení [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), naleznete v tématu [provádí navigaci](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

> [!NOTE]
> I když umístit [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) do [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), nedoporučujeme umístit `TabbedPage` do `NavigationPage`. Důvodem je, že v Iosu `UITabBarController` vždy funguje jako obálka pro `UINavigationController`. Další informace najdete v tématu [kombinované zobrazení rozhraní řadiče](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) v knihovně iOS Developer Library.

#### <a name="navigation-inside-a-tab"></a>Navigace v kartě

Navigace lze provádět z druhé kartě vyvoláním [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) metodu [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) vlastnost [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance, jak je ukázáno v následujícím příkladu kódu:

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

To způsobí, že `UpcomingAppointmentsPage` instance doručit bez vyžádání do navigačního zásobníku, kde se stane aktivní stránkou. To můžete vidět na následujících snímcích obrazovky:

![](tabbed-page-images/navigationpage.png "Navigace v kartě")

Další informace o navigaci pomocí provádí [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) najdete v tématu [hierarchická navigace](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>Naplnění TabbedPage pomocí šablony

Následující příklad ukazuje kód XAML [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) vytvořený pomocí přiřazení [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) k [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) vlastnost vrátit stránky pro objekty v kolekci:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="clr-namespace:TabbedPageDemo;assembly=TabbedPageDemo"
            x:Class="TabbedPageDemo.TabbedPageDemoPage">
  <TabbedPage.Resources>
    <ResourceDictionary>
      <local:NonNullToBooleanConverter x:Key="booleanConverter" />
    </ResourceDictionary>
  </TabbedPage.Resources>
  <TabbedPage.ItemTemplate>
    <DataTemplate>
      <ContentPage Title="{Binding Name}" Icon="monkeyicon.png">
        <StackLayout Padding="5, 25">
          <Label Text="{Binding Name}" Font="Bold,Large" HorizontalOptions="Center" />
          <Image Source="{Binding PhotoUrl}" WidthRequest="200" HeightRequest="200" />
          <StackLayout Padding="50, 10">
            <StackLayout Orientation="Horizontal">
              <Label Text="Family:" HorizontalOptions="FillAndExpand" />
              <Label Text="{Binding Family}" Font="Bold,Medium" />
            </StackLayout>
            ...
          </StackLayout>
        </StackLayout>
      </ContentPage>
    </DataTemplate>
  </TabbedPage.ItemTemplate>
</TabbedPage>
```

[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Naplněný daty tak, že nastavíte [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) vlastnost v konstruktoru pro soubor kódu na pozadí:

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

Následující příklad kódu ukazuje ekvivalent [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) vytvořené v jazyce C#:

```csharp
public class TabbedPageDemoPageCS : TabbedPage
{
  public TabbedPageDemoPageCS ()
  {
    var booleanConverter = new NonNullToBooleanConverter ();

    ItemTemplate = new DataTemplate (() => {
      var nameLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label)),
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
      };
      nameLabel.SetBinding (Label.TextProperty, "Name");

      var image = new Image { WidthRequest = 200, HeightRequest = 200 };
      image.SetBinding (Image.SourceProperty, "PhotoUrl");

      var familyLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
        FontAttributes = FontAttributes.Bold
      };
      familyLabel.SetBinding (Label.TextProperty, "Family");
      ...

      var contentPage = new ContentPage {
        Icon = "monkeyicon.png",
        Content = new StackLayout {
          Padding = new Thickness (5, 25),
          Children = {
            nameLabel,
            image,
            new StackLayout {
              Padding = new Thickness (50, 10),
              Children = {
                new StackLayout {
                  Orientation = StackOrientation.Horizontal,
                  Children = {
                    new Label { Text = "Family:", HorizontalOptions = LayoutOptions.FillAndExpand },
                    familyLabel
                  }
                },
                ...
              }
            }
          }
        }
      };
      contentPage.SetBinding (TitleProperty, "Name");
      return contentPage;
    });
    ItemsSource = MonkeyDataModel.All;
  }
}
```

Každá karta zobrazuje [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) , která používá řadu [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) a [ `Label` ](xref:Xamarin.Forms.Label) instance, které chcete zobrazit data pro kartu. Na následujících snímcích obrazovky zobrazit obsah *Tamarin* kartu:

![](tabbed-page-images/tab3.png "Naplnění TabbedPage pomocí šablony")

Že vyberete jinou kartu a pak zobrazí obsah pro dané karty.

> [!NOTE]
> [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Nepodporuje uživatelského rozhraní virtualizace. Proto se ovlivnil výkon, pokud `TabbedPage` obsahuje příliš mnoho podřízených elementů.

Další informace o [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), naleznete v tématu [kapitoly 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold Xamarin.Forms knihy.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak používat TabbedPage k procházení kolekce stránek. Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) se skládá ze seznamu karty a větší oblasti podrobností, každá karta načítání obsahu do oblasti podrobností.


## <a name="related-links"></a>Související odkazy

- [Variace stránek](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](xref:Xamarin.Forms.TabbedPage)
