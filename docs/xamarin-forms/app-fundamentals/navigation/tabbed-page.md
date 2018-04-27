---
title: Na kartách stránky
description: Xamarin.Forms TabbedPage se skládá ze seznamu karty a větší oblast podrobností, každé kartě načítání obsahu do oblasti podrobností. Tento článek ukazuje, jak pomocí TabbedPage můžete přejít přes kolekci stránek.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 4210f672cdc68acc45b1f547dcc2e6933298df93
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="tabbed-page"></a>Na kartách stránky

_Xamarin.Forms TabbedPage se skládá ze seznamu karty a větší oblast podrobností, každé kartě načítání obsahu do oblasti podrobností. Tento článek ukazuje, jak pomocí TabbedPage můžete přejít přes kolekci stránek._

## <a name="overview"></a>Přehled

Následující zobrazení snímky obrazovky [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) na jednotlivých platformách:

![](tabbed-page-images/tab1.png "Příklad TabbedPage")

Na následujících snímcích obrazovky zaměřit se na kartě Formát na jednotlivých platformách:

![](tabbed-page-images/tabbedpage-components.png "Karta TabbedPage součásti")

Rozložení [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)a jeho karty je závislý na platformě:

- V systému iOS seznam karet se zobrazí v dolní části obrazovky a oblasti podrobností překračuje prahovou hodnotu. Každé kartě má také obrázku ikony, které by se měly 30 x 30 PNG s průhlednost pro běžné řešení, 60 x 60 pro s vysokým rozlišením a 90 x 90 pro iPhone 6 Plus řešení. Pokud máte více než pět karty *Další* se zobrazí karta, která lze použít pro přístup k další karty. Další informace o načítání obrazů v aplikaci Xamarin.Forms najdete v tématu [práce s obrázky](~/xamarin-forms/user-interface/images.md). Další informace o požadavcích na ikonu najdete v tématu [vytváření aplikací – záložkami](~/ios/user-interface/controls/creating-tabbed-applications.md).

    > [!NOTE]
  > Všimněte si, že `TabbedRenderer` pro iOS má přepisovatelným `GetIcon` metoda, která slouží k načtení ikony karta ze zadaného zdroje. Toto přepsání umožňuje používat Image SVG jako ikony na `TabbedPage`. Kromě toho lze zadat nezaškrtnuté a vybrané verze ikonu.

- V systému Android zobrazí se seznam karty v horní části obrazovky a oblasti podrobností je nižší než. Názvy karet jsou automaticky aktivované a uživatel může posuňte kolekce karty, pokud jsou moc pro jednu obrazovku.

    > [!NOTE]
  > Všimněte si, že při použití kompatibility aplikace v systému Android, každé kartě také zobrazit ikony. Kromě toho `TabbedPageRenderer` pro Android kompatibility aplikace má přepisovatelným `SetTabIcon` metoda, která slouží k načtení ikony karta z vlastní `Drawable`. Toto přepsání umožňuje používat Image SVG jako ikony na `TabbedPage`.

- Na faktorech – Windows tablet formuláře, karty nejsou vždy viditelné a uživatelé nemusí prstem dolů (nebo klikněte pravým tlačítkem, pokud mají myši připojen) k zobrazení karet v `TabbedPage` (jak je znázorněno níže).

![](tabbed-page-images/windows-tabs.png "TabbedPage karet v systému Windows")

## <a name="creating-a-tabbedpage"></a>Vytváření TabbedPage

Dva přístupy lze použít k vytvoření [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/):

- [Naplnění](#Populating_a_TabbedPage_with_a_Page_Collection) [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) s kolekcí podřízených [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objekty, například v kolekci [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance.
- [Přiřadit](#Populating_a_TabbedPage_with_a_Template) kolekce [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) vlastnost a přiřadit [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) k [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) vlastnost vrátit stránky pro objekty v kolekci.

Pomocí obou přístupů [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) každé stránce se zobrazí, protože uživatel vybere každé kartě.

> [!NOTE]
> Je doporučeno, [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) by měl být naplněný [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) a [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)pouze instance. To vám pomůže zajistit konzistentní uživatelské prostředí pro všechny platformy.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>Naplnění TabbedPage s shromažďování stránky.

Následující příklad ukazuje kód XAML [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) sestavený naplněním s kolekcí podřízených [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objekty:

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

Následující příklad kódu ukazuje ekvivalent [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) vytvořené v C#:

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

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Se naplní dva podřízené [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objekty. Je prvním podřízeným objektem [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance a druhé kartě [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) obsahující `ContentPage` instance.

> [!NOTE]
> [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Nepodporuje virtualizace uživatelského rozhraní. Proto být ovlivněn výkon, pokud `TabbedPage` obsahuje příliš mnoho podřízené elementy.

Tyto snímky obrazovky zobrazit `TodayPage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instanci, která se zobrazí na *Dnes* karty:

![](tabbed-page-images/today-page.png "ContentPage v TabbedPage")

Výběr *plán* kartě zobrazí `SchedulePage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance, kterou je uzavřen do [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) instance a je zobrazen v nabídce Následující snímek obrazovky:

![](tabbed-page-images/schedule-page.png "NavigationPage v TabbedPage")

Informace o rozložení [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), najdete v části [provádění navigační](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

> [!NOTE]
> I když je přijatelné umístit [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) do [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), se nedoporučuje umístit `TabbedPage` do `NavigationPage`. Důvodem je, že v systému iOS, `UITabBarController` vždy funguje jako obálka pro `UINavigationController`. Další informace najdete v tématu [kombinaci rozhraní řadiče zobrazení](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) v knihovně iOS Developer Library.

#### <a name="navigation-inside-a-tab"></a>Navigace v kartě

Navigace lze provést z druhé kartě vyvoláním [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) metodu [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) vlastnost [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instanci, jak je ukázáno v následujícím příkladu kódu:

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

To způsobí, že `UpcomingAppointmentsPage` instance, která má být vloženy do zásobníku navigace, kde bude aktivní stránku. Můžete se podívat na následujících snímcích obrazovky:

![](tabbed-page-images/navigationpage.png "Navigace v kartě")

Další informace o provádění, navigace pomocí [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) třídy najdete v tématu [hierarchické navigační](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>Naplnění TabbedPage pomocí šablony

Následující příklad ukazuje kód XAML [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) sestavený přiřazením [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) k [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) vlastnost vrátit stránky pro objekty v kolekci:

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

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Naplněný daty nastavením [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) vlastnost v konstruktoru pro soubor kódu:

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

Následující příklad kódu ukazuje ekvivalent [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) vytvořené v C#:

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

Zobrazí na každé kartě [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) , který používá řadu [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) a [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance, které chcete zobrazit data pro kartu. Na následujících snímcích obrazovky zobrazit obsah *Tamarin* karty:

![](tabbed-page-images/tab3.png "Naplnění TabbedPage pomocí šablony")

Výběr jiné kartě pak zobrazí obsah pro dané kartě.

> [!NOTE]
> [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Nepodporuje virtualizace uživatelského rozhraní. Proto být ovlivněn výkon, pokud `TabbedPage` obsahuje příliš mnoho podřízené elementy.

Další informace o [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), najdete v části [kapitoly 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charlese Petzold Xamarin.Forms knihy.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak lze pomocí TabbedPage můžete přejít přes kolekci stránek. Platformě Xamarin.Forms [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) se skládá z seznam karty a větší oblast podrobností, s každé kartě načítání obsahu do oblasti podrobností.


## <a name="related-links"></a>Související odkazy

- [Stránka typy](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)
