---
title: Stránka – Carousel Xamarin.Forms
description: Xamarin.Forms CarouselPage je stránka, která uživatelé můžou potažením prstem přejděte na stranu pro navigaci mezi stránkami obsahu, jako je galerie. Tento článek ukazuje, jak používat CarouselPage procházení kolekce stránek.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: bce3a60f3647a537906cfa11fc1dcfcc6f5cf365
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998601"
---
# <a name="xamarinforms-carousel-page"></a>Stránka – Carousel Xamarin.Forms

_Xamarin.Forms CarouselPage je stránka, která uživatelé můžou potažením prstem přejděte na stranu pro navigaci mezi stránkami obsahu, jako je galerie. Tento článek ukazuje, jak používat CarouselPage procházení kolekce stránek._

## <a name="overview"></a>Přehled

Zobrazit následující snímky obrazovky [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) na jednotlivých platformách:

![](carousel-page-images/thirdpage.png "CarouselPage Thid položky")

Rozložení [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) je stejný jako na jednotlivých platformách. Stránky se dá Navigovat prostřednictvím potáhnutím zleva doprava pro navigaci vpřed prostřednictvím kolekce a potáhnutím zleva doprava pro navigaci zpět prostřednictvím kolekce. Na následujících snímcích obrazovky zobrazit na první stránce [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) instance:

![](carousel-page-images/firstpage.png "CarouselPage první položka")

Potažení prstem zprava levé přesune na druhou stránku, jak je znázorněno na následujících snímcích obrazovky:

![](carousel-page-images/secondpage.png "CarouselPage druhé položky")

Na třetí stránce potažení prstem zprava doleva znovu pohybuje, zatímco potažení prstem zleva doprava vrátí na předchozí stránku.

<!--
> [!NOTE]
> The [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](xref:Xamarin.Forms.CarouselView) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>Vytváření CarouselPage

Dva přístupy lze použít k vytvoření [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage):

- [Naplnění](#Populating_a_CarouselPage_with_a_Page_Collection) `CarouselPage` sbírka podřízené [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instancí.
- [Přiřadit](#Populating_a_CarouselPage_with_a_Template) kolekci [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) vlastnosti a přiřazení [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) k [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) vlastnost vrátit [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance pro objekty v kolekci.

Pomocí obou metod `CarouselPage` bude poté zobrazte jednotlivé stránky zase potáhnutí prstem zásahu Přesun na další stránku, který se má zobrazit.

> [!NOTE]
> A [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) je možné naplnit pouze [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instancí, nebo `ContentPage` vy.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>Naplnění CarouselPage s kolekcí stránky

Následující příklad ukazuje kód XAML [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) , který zobrazí tři [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instancí:

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

Každý [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) jednoduše zobrazí [ `Label` ](xref:Xamarin.Forms.Label) pro určitou barvu a [ `BoxView` ](xref:Xamarin.Forms.BoxView) této barvy.

> [!NOTE]
> [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) Nepodporuje uživatelského rozhraní virtualizace. Proto se ovlivnil výkon, pokud `CarouselPage` obsahuje příliš mnoho podřízených elementů.

Pokud [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) je vložen do [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) stránce [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) Vlastnost musí být nastavena na `false` které zabrání konfliktům gesta `CarouselPage` a `MasterDetailPage`.

Další informace o [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), naleznete v tématu [kapitoly 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold Xamarin.Forms knihy.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>Naplnění CarouselPage pomocí šablony

Následující příklad ukazuje kód XAML [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) vytvořený pomocí přiřazení [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) k [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) vlastnost vrátit stránky pro objekty v kolekci:

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

[ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) Naplněný daty tak, že nastavíte [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) vlastnost v konstruktoru pro soubor kódu na pozadí:

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

Následující příklad kódu ukazuje ekvivalent [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) vytvořené v jazyce C#:

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

Každý [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) jednoduše zobrazí [ `Label` ](xref:Xamarin.Forms.Label) pro určitou barvu a [ `BoxView` ](xref:Xamarin.Forms.BoxView) této barvy.

> [!NOTE]
> [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) Nepodporuje uživatelského rozhraní virtualizace. Proto se ovlivnil výkon, pokud `CarouselPage` obsahuje příliš mnoho podřízených elementů.

Pokud [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) je vložen do [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) stránce [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) Vlastnost musí být nastavena na `false` které zabrání konfliktům gesta `CarouselPage` a `MasterDetailPage`.

Další informace o [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), naleznete v tématu [kapitoly 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold Xamarin.Forms knihy.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak používat [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) procházení kolekce stránek. `CarouselPage` Je stránka, která uživatelé můžou potažením prstem přejděte na stranu pro navigaci mezi stránkami obsahu, podobně jako galerie.


## <a name="related-links"></a>Související odkazy

- [Variace stránek](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](xref:Xamarin.Forms.CarouselPage)
