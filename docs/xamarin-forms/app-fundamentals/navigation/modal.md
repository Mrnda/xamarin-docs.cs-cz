---
title: Modální stránky
description: Xamarin.Forms poskytuje podporu pro modální stránky. Modální stránky doporučuje uživatelům k dokončení samostatná úloha, která nemůže být opuštění dokud je úloha dokončena nebo zrušena. Tento článek ukazuje, jak přejděte na modální stránky.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 30d0371e0eaa31673561ae12c7a46b7a7819a647
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847416"
---
# <a name="modal-pages"></a>Modální stránky

_Xamarin.Forms poskytuje podporu pro modální stránky. Modální stránky doporučuje uživatelům k dokončení samostatná úloha, která nemůže být opuštění dokud je úloha dokončena nebo zrušena. Tento článek ukazuje, jak přejděte na modální stránky._

Tento článek popisuje v následujících tématech:

- [Provádění navigační](#Performing_Navigation) – když zavedete stránky modální zásobníku operace stránky z modální zásobníku, zakázání tlačítko Zpět a animace přechody stránek.
- [Předávání dat při přechodu](#Passing_Data_when_Navigating) – předávání dat prostřednictvím stránky konstruktor a prostřednictvím `BindingContext`.

## <a name="overview"></a>Přehled

Modální stránky může být libovolná z [stránky](~/xamarin-forms/user-interface/controls/pages.md) typy podporované systémem Xamarin.Forms. K zobrazení modální stránky aplikace předá ho do modální zásobníku, kde se stane aktivní stránku, jak je znázorněno v následujícím diagramu:

![](modal-images/pushing.png "Když zavedete stránky modální zásobníku")

Chcete-li vrátit na předchozí stránku aplikace, objeví se aktuální stránku z modální zásobníku a nová stránka nejhornější stane aktivní stránku, jak je znázorněno v následujícím diagramu:

![](modal-images/popping.png "Odebrání stránky z modální zásobníku")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Provádění navigace

Modální navigační metody jsou vystavené [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) vlastnost na žádném [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) odvozených typů. Tyto metody umožňují [push modální stránky](#Pushing_Pages_to_the_Modal_Stack) do modální zásobníku a [pop modální stránky](#Popping_Pages_from_the_Modal_Stack) z modální zásobníku.

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Vlastnost taky zpřístupňuje [ `ModalStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) vlastnost, ze kterého lze získat modální stránky v modální zásobníku. Však neexistuje žádná koncepce provádění modální zásobníku manipulaci nebo odebrání kořenovou stránku v modální navigaci. Je to proto, že tyto operace nepodporuje všeobecně základní platformy.

> [!NOTE]
> A [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) instance není vyžadován pro provádění navigace modální stránky.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>Vkládání stránky modální zásobníku

Přejděte na `ModalPage` je nutné vyvolat [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) metodu [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) vlastnost aktuální stránky, jako předvedenou v následujícím příkladu kódu:

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    ...
    await Navigation.PushModalAsync (detailPage);
  }
}
```

To způsobí, že `ModalPage` instance, která má být vloženy do modální zásobníku, kde bude aktivní stránku, zadat, že byla vybrána položka v [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) na `MainPage` instance. `ModalPage` Instance je vidět na následujících snímcích obrazovky:

![](modal-images/modalpage.png "Modální příklad stránky")

Když [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) je vyvolána, dojde k následujícím událostem:

- Stránka volání `PushModalAsync` má jeho [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) přepsání vyvolána, za předpokladu, že není základní platformu Android.
- Na stránce se přešli jeho [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) přepsání vyvolat.
- `PushAsync` Dokončení úlohy.

Přesné pořadí, že k těmto událostem dojde, je však platformy, které jsou závislé. Další informace najdete v tématu [kapitoly 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charlese Petzold Xamarin.Forms knihy.

> [!NOTE]
> Volání [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) a [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) přepsání nemohou být považovány za zaručenou indikace navigaci na stránce. Například v systému iOS `OnDisappearing` přepsání je na stránce active volána, když se aplikace ukončí.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>Operace stránky z modální zásobníku

Aktivní stránky může být odebrány z modální zásobníku stisknutím kombinace kláves *zpět* tlačítko na zařízení, bez ohledu na to, zda je na zařízení fyzické tlačítko nebo na obrazovce tlačítko.

Prostřednictvím kódu programu vrátit na stránku původní `ModalPage` musí vyvolání instance [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) metoda, jak je ukázáno v následujícím příkladu kódu:

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

To způsobí, že `ModalPage` instance, která má být odebrán, modální, s novou nejhornější stránku stane aktivní stránky. Když [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) je vyvolána, dojde k následujícím událostem:

- Stránka volání `PopModalAsync` má jeho [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) přepsání vyvolat.
- Na stránce jako odpověď na jeho [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) přepsání vyvolána, za předpokladu, že není základní platformu Android.
- `PopModalAsync` Úkolů vrátí.

Přesné pořadí, že k těmto událostem dojde, je však platformy, které jsou závislé. Další informace najdete v tématu [kapitoly 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charlese Petzold Xamarin.Forms knihy.

### <a name="disabling-the-back-button"></a>Zakázat tlačítko Zpět

V systému Android, uživatel může vždy vrátit na předchozí stránku stisknutím standardní *zpět* na zařízení tlačítko. Pokud stránce modální vyžaduje, aby uživatel k dokončení úlohy nezávislý před opuštěním stránky, musíte zakázat aplikaci *zpět* tlačítko. Toho lze dosáhnout přepsáním [ `Page.OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed/) metoda na stránce modální. Další informace najdete v části [kapitoly 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charlese Petzold Xamarin.Forms knihy.

### <a name="animating-page-transitions"></a>Animace přechody stránek

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Vlastnost každé stránce poskytuje taky přepsaného nabízené a pop metody, které zahrnují `boolean` parametr, který určuje, jestli se má zobrazit stránku animace během navigace, jak je znázorněno v následujícím kódu Příklad:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushModalAsync (new DetailPage (), false);
}

async void OnDismissButtonClicked (object sender, EventArgs args)
{
  // Page appearance not animated
  await Navigation.PopModalAsync (false);
}
```

Nastavení `boolean` parametru `false` zakáže animace přechod stránky, při nastavení parametru na `true` umožňuje animace přechod stránky za předpokladu, že ji podporuje základní platformy. Metody nabízené a pop, která nemají tento parametr však povolit animace ve výchozím nastavení.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Předávání dat při přechodu

Někdy je nezbytné pro stránku k předávání dat při navigaci na jinou stránku. Jsou dva postupy provádění to předávání dat pomocí konstruktoru stránky a nastavení nová stránka [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) k datům. Každý teď probereme naopak.

### <a name="passing-data-through-a-page-constructor"></a>Předávání dat prostřednictvím stránky konstruktor

Nejjednodušší způsob pro předávání dat při navigaci na jinou stránku je prostřednictvím parametru stránky konstruktor, který je znázorněno v následujícím příkladu kódu:

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

Tento kód vytvoří `MainPage` instance, předávání v aktuální datum a čas ve formátu ISO 8601.

`MainPage` Instance přijímá data prostřednictvím parametr konstruktoru, jak je znázorněno v následujícím příkladu kódu:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Data se následně zobrazí na stránce nastavení [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) vlastnost.

### <a name="passing-data-through-a-bindingcontext"></a>Předávání dat prostřednictvím vazby

Alternativní způsob pro předávání dat při navigaci na jinou stránku je nastavením nová stránka [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) k datům, jak je znázorněno v následujícím příkladu kódu:

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    detailPage.BindingContext = e.SelectedItem as Contact;
    listView.SelectedItem = null;
    await Navigation.PushModalAsync (detailPage);
  }
}
```

Nastaví tento kód [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) z `DetailPage` instance k `Contact` instance a pak přejde `DetailPage`.

`DetailPage` Pak použije k zobrazení datová vazba `Contact` instance dat, jak je znázorněno v následujícím příkladu kódu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ModalNavigation.DetailPage">
    <ContentPage.Padding>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,40,0,0" />
      </OnPlatform>
    </ContentPage.Padding>
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" FontSize="Medium" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
              ...
            <Button x:Name="dismissButton" Text="Dismiss" Clicked="OnDismissButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Následující příklad kódu ukazuje, jak lze provést datové vazby v C#:

```csharp
public class DetailPageCS : ContentPage
{
  public DetailPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var dismissButton = new Button { Text = "Dismiss" };
    dismissButton.Clicked += OnDismissButtonClicked;

    Thickness padding;
    switch (Device.RuntimePlatform)
    {
        case Device.iOS:
            padding = new Thickness(0, 40, 0, 0);
            break;
        default:
            padding = new Thickness();
            break;
    }

    Padding = padding;
    Content = new StackLayout {
      HorizontalOptions = LayoutOptions.Center,
      VerticalOptions = LayoutOptions.Center,
      Children = {
        new StackLayout {
          Orientation = StackOrientation.Horizontal,
          Children = {
            new Label{ Text = "Name:", FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)), HorizontalOptions = LayoutOptions.FillAndExpand },
            nameLabel
          }
        },
        ...
        dismissButton
      }
    };
  }

  async void OnDismissButtonClicked (object sender, EventArgs args)
  {
    await Navigation.PopModalAsync ();
  }
}
```

Data se následně zobrazí na stránce řadu [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládací prvky.

Další informace o vazbě dat najdete v tématu [základy vazby dat](~/xamarin-forms/xaml/xaml-basics/index.md).

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak se orientovat na modální stránky. Modální stránky doporučuje uživatelům k dokončení samostatná úloha, která nemůže být opuštění dokud je úloha dokončena nebo zrušena.


## <a name="related-links"></a>Související odkazy

- [Navigace stránky](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Modální (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
