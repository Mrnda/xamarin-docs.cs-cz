---
title: Karuselu stránky
description: Xamarin.Forms CarouselPage je stránka, která uživatelé mohou prstem stranu procházet stránky obsahu, jako je galerie. Tento článek ukazuje, jak pomocí CarouselPage můžete přejít přes kolekci stránek.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 9259e2a85a7375106891eaae5fe22d6babfa2fcf
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846455"
---
# <a name="carousel-page"></a>Karuselu stránky

_Xamarin.Forms CarouselPage je stránka, která uživatelé mohou prstem stranu procházet stránky obsahu, jako je galerie. Tento článek ukazuje, jak pomocí CarouselPage můžete přejít přes kolekci stránek._

## <a name="overview"></a>Přehled

Následující zobrazení snímky obrazovky [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) na jednotlivých platformách:

![](carousel-page-images/thirdpage.png "CarouselPage Thid položky")

Rozložení [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) je stejná na každou platformu. Stránky lze procházet přes potažením zprava doleva přejděte předávání prostřednictvím kolekce a potažením zleva doprava přejděte zpětné prostřednictvím kolekce. Na následujících snímcích obrazovky zobrazit na první stránku [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) instance:

![](carousel-page-images/firstpage.png "První CarouselPage položky")

K načtení zprava doleva přesune na druhou stránku, jak je vidět na následujících snímcích obrazovky:

![](carousel-page-images/secondpage.png "Položka CarouselPage sekundu")

K načtení zprava doleva znovu přesune na třetí stránce při načtení zleva doprava vrátí na předchozí stránku.

<!--
> [!NOTE]
> The [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>Vytváření CarouselPage

Dva přístupy lze použít k vytvoření [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/):

- [Naplnění](#Populating_a_CarouselPage_with_a_Page_Collection) `CarouselPage` s kolekcí podřízených [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance.
- [Přiřadit](#Populating_a_CarouselPage_with_a_Template) kolekce [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) vlastnost a přiřadit [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) k [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) vlastnost vrátit [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instancí pro objekty v kolekci.

Pomocí obou přístupů `CarouselPage` bude potom zobrazení každé stránce se pak s prstem interakce, Přesun na další stránku, který se má zobrazit. 

> [!NOTE]
> A [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) pouze možné naplnit [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instancí, nebo `ContentPage` odvozené konfigurace.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>Naplnění CarouselPage s shromažďování stránky.

Následující příklad ukazuje kód XAML [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) zobrazující tři [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instancí:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <ContentPage>
        <ContentPage.Padding>
          <OnPlatform x:TypeArguments="Thickness">
              <On Platform="iOS, Android" Value="0,40,0,0" />
          </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
            <Label Text="Red" FontSize="Medium" HorizontalOptions="Center" />
            <BoxView Color="Red" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
</CarouselPage>
```

Následující příklad kódu ukazuje ekvivalentní uživatelského rozhraní v jazyce C#:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        var redContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                Children = {
                    new Label {
                        Text = "Red",
                        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                        HorizontalOptions = LayoutOptions.Center
                    },
                    new BoxView {
                        Color = Color.Red,
                        WidthRequest = 200,
                        HeightRequest = 200,
                        HorizontalOptions = LayoutOptions.Center,
                        VerticalOptions = LayoutOptions.CenterAndExpand
                    }
                }
            }
        };
        var greenContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };
        var blueContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };

        Children.Add (redContentPage);
        Children.Add (greenContentPage);
        Children.Add (blueContentPage);
    }
}
```

Každý [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) jednoduše zobrazí [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) pro konkrétní barvy a [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) této barvy.

> [!NOTE]
> [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Nepodporuje virtualizace uživatelského rozhraní. Proto být ovlivněn výkon, pokud `CarouselPage` obsahuje příliš mnoho podřízené elementy.

Pokud [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) se vloží do [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) stránky [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) Vlastnost by měla být nastavená na `false` zabránit konfliktům gesto mezi `CarouselPage` a `MasterDetailPage`.

Další informace o [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), najdete v části [kapitoly 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charlese Petzold Xamarin.Forms knihy.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>Naplnění CarouselPage pomocí šablony

Následující příklad ukazuje kód XAML [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) sestavený přiřazením [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) k [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) vlastnost vrátit stránky pro objekty v kolekci:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <CarouselPage.ItemTemplate>
        <DataTemplate>
            <ContentPage>
                <ContentPage.Padding>
                  <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS, Android" Value="0,40,0,0" />
                  </OnPlatform>
                </ContentPage.Padding>
                <StackLayout>
                    <Label Text="{Binding Name}" FontSize="Medium" HorizontalOptions="Center" />
                    <BoxView Color="{Binding Color}" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
                </StackLayout>
            </ContentPage>
        </DataTemplate>
    </CarouselPage.ItemTemplate>
</CarouselPage>
```

[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Naplněný daty nastavením [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) vlastnost v konstruktoru pro soubor kódu:

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

Následující příklad kódu ukazuje ekvivalent [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) vytvořené v C#:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        ItemTemplate = new DataTemplate (() => {
            var nameLabel = new Label {
                FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                HorizontalOptions = LayoutOptions.Center
            };
            nameLabel.SetBinding (Label.TextProperty, "Name");

            var colorBoxView = new BoxView {
                WidthRequest = 200,
                HeightRequest = 200,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.CenterAndExpand
            };
            colorBoxView.SetBinding (BoxView.ColorProperty, "Color");

            return new ContentPage {
                Padding = padding,
                Content = new StackLayout {
                    Children = {
                        nameLabel,
                        colorBoxView
                    }
                }
            };
        });

        ItemsSource = ColorsDataModel.All;
    }
}
```

Každý [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) jednoduše zobrazí [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) pro konkrétní barvy a [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) této barvy.

> [!NOTE]
> [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Nepodporuje virtualizace uživatelského rozhraní. Proto být ovlivněn výkon, pokud `CarouselPage` obsahuje příliš mnoho podřízené elementy.

Pokud [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) se vloží do [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) stránky [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) Vlastnost by měla být nastavená na `false` zabránit konfliktům gesto mezi `CarouselPage` a `MasterDetailPage`.

Další informace o [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), najdete v části [kapitoly 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charlese Petzold Xamarin.Forms knihy.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak používat [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) procházet kolekce stránek. `CarouselPage` Je stránka, která uživatelé mohou prstem stranu procházet stránky obsahu, podobně jako galerie.


## <a name="related-links"></a>Související odkazy

- [Stránka typy](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)
