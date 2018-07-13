---
title: Xamarin.Forms modální stránky
description: Xamarin.Forms poskytuje podporu pro modální stránky. Modální stránky vyzývá uživatele k dokončení samostatná úloha, která nemůže být opuštění dokud je úloha dokončena nebo zrušena. Tento článek ukazuje, jak přejít na modální stránky.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 44aee8500c7de2ae56b59049368d6025ec49cc5e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994814"
---
# <a name="xamarinforms-modal-pages"></a>Xamarin.Forms modální stránky

_Xamarin.Forms poskytuje podporu pro modální stránky. Modální stránky vyzývá uživatele k dokončení samostatná úloha, která nemůže být opuštění dokud je úloha dokončena nebo zrušena. Tento článek ukazuje, jak přejít na modální stránky._

Tento článek popisuje v následujících tématech:

- [Provádění navigace](#Performing_Navigation) – odesílání stránky do modální zásobníku, vyjímání stránky od modálních zásobníku, zakázáním tlačítka Zpět a animace přechody stránek.
- [Předávání dat při navigaci](#Passing_Data_when_Navigating) – předávání dat prostřednictvím konstruktor stránky a prostřednictvím `BindingContext`.

## <a name="overview"></a>Přehled

Modální stránky může být libovolná z [stránky](~/xamarin-forms/user-interface/controls/pages.md) typů podporuje Xamarin.Forms. Zobrazíte modální stránky aplikace zařadí ho do modální zásobníku, kde se tak stane aktivní stránkou., jak je znázorněno v následujícím diagramu:

![](modal-images/pushing.png "Odesílání stránky do modální zásobníku")

Vrátit na předchozí stránku aplikace zobrazte aktuální stránku od modálních zásobníku, a na novou stránku nejvyšší úrovně se stane aktivní stránkou., jak je znázorněno v následujícím diagramu:

![](modal-images/popping.png "Odebrání stránky z modální zásobníku")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Provádění navigace

Modální navigační metody jsou vystavené [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) vlastnost na žádném [ `Page` ](xref:Xamarin.Forms.Page) odvozené typy. Tyto metody poskytují možnost [push modální stránky](#Pushing_Pages_to_the_Modal_Stack) do modální zásobníku a [vyvolat přes pop modální stránky](#Popping_Pages_from_the_Modal_Stack) od modálních zásobníku.

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Také poskytuje vlastnost [ `ModalStack` ](xref:Xamarin.Forms.INavigation.ModalStack) vlastností, ze kterého můžete získat modální stránky v modálním zásobníku. Neexistuje však žádný koncept provádění modální zásobníku manipulaci nebo automaticky otevíraného kořenovou stránku v modálním navigaci. Je to proto, že tyto operace nejsou podporovány univerzálně na základní platformy.

> [!NOTE]
> A [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instance není vyžadován pro provádění navigace modální stránky.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>Odesílání stránky do modální zásobníku

Přejděte `ModalPage` je potřeba vyvolat [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) metodu na [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) vlastnost aktuální stránky jako předvedenou v následujícím příkladu kódu:

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

To způsobí, že `ModalPage` instance má být vloženy do zásobníku modální okno, kde stane aktivní stránkou, k dispozici, že byla vybrána položka v [ `ListView` ](xref:Xamarin.Forms.ListView) na `MainPage` instance. `ModalPage` Instance je znázorněno na následujících snímcích obrazovky:

![](modal-images/modalpage.png "Příklad modální stránky")

Když [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) je vyvolána, dojde k následujícím událostem:

- Volání funkce stránky `PushModalAsync` má jeho [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) přepsání vyvolána, za předpokladu, že se základní platformy Android.
- Na stránce se přejde poté, jeho [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) přepsání vyvolána.
- `PushAsync` Dokončení úkolu.

Přesné pořadí, že k těmto událostem dojde je však závislý na platformě. Další informace najdete v tématu [kapitoly 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms knihy.

> [!NOTE]
> Volání [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) a [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) přepsání nemůže být zpracován jako garantované údaje o navigaci na stránce. Například v Iosu `OnDisappearing` přepsání je volána na aktivní stránce, když se aplikace ukončí.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>Vyjímání stránky od modálních zásobníku

Aktivní stránkou. může být odebrány ze zásobníku modální stisknutím klávesy *zpět* tlačítko na zařízení, bez ohledu na to, zda se jedná o fyzického tlačítka na zařízení nebo tlačítko na obrazovce.

Programově vrátit na původní stránku `ModalPage` instance musí vyvolat [ `PopModalAsync` ](xref:Xamarin.Forms.INavigation.PopModalAsync) způsob, jak je ukázáno v následujícím příkladu kódu:

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

To způsobí, že `ModalPage` instance má být odebrán z modální zásobníku s novou stránku nejvyšší úrovně stane aktivní stránkou. Když [ `PopModalAsync` ](xref:Xamarin.Forms.INavigation.PopModalAsync) je vyvolána, dojde k následujícím událostem:

- Volání funkce stránky `PopModalAsync` má jeho [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) přepsání vyvolána.
- Na stránce se vrací do jeho [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) přepsání vyvolána, za předpokladu, že se základní platformy Android.
- `PopModalAsync` Úkolů vrátí.

Přesné pořadí, že k těmto událostem dojde je však závislý na platformě. Další informace najdete v tématu [kapitoly 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms knihy.

### <a name="disabling-the-back-button"></a>Zakázání tlačítka Zpět

V Androidu, uživatel se kdykoli vrátit k předchozí stránce stisknutím klávesy standardní *zpět* na zařízení tlačítko. Pokud se modální stránky vyžaduje, aby uživatel k dokončení úkolu samostatná před opustit stránku, musíte zakázat aplikaci *zpět* tlačítko. Toho můžete docílit tak, že přepíšete [ `Page.OnBackButtonPressed` ](xref:Xamarin.Forms.Page.OnBackButtonPressed) metody ve stránce modální okno. Další informace najdete v části [kapitoly 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms knihy.

### <a name="animating-page-transitions"></a>Animace přechody stránek

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Vlastnost jednotlivých stránek také poskytuje přepsané nabízená oznámení a pop metody, které obsahují `boolean` parametr, který určuje, zda se zobrazí stránka animace průběhu navigace, jak je znázorněno v následujícím kódu Příklad:

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

Nastavení `boolean` parametr `false` zakáže stránku přechodu animace, při nastavení parametru na `true` umožňuje přechod na stránky animace, za předpokladu, že ji podporuje základní platformy. Nabízená oznámení a pop metody, které nemají tento parametr však povolit animace ve výchozím nastavení.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Předání dat při navigaci

Někdy je nezbytné pro stránku k předávání dat při navigaci na jinou stránku. Jsou dvě techniky způsoby předávání dat prostřednictvím konstruktor stránky a nastavením nová stránka [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) k datům. Každý nyní probereme zase.

### <a name="passing-data-through-a-page-constructor"></a>Předání dat prostřednictvím konstruktoru stránky

Nejjednodušší techniku pro předávání dat na jinou stránku během navigace je prostřednictvím parametru konstruktor stránky, která je znázorněna v následujícím příkladu kódu:

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

Tento kód vytvoří `MainPage` instance předávání v aktuálním datem a časem ve formátu ISO8601.

`MainPage` Instance přijímá data prostřednictvím konstruktoru parametru, jak je znázorněno v následujícím příkladu kódu:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Data se následně zobrazí na stránce nastavení [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) vlastnost.

### <a name="passing-data-through-a-bindingcontext"></a>Předání dat prostřednictvím BindingContext

Alternativním přístupem k předávání dat při navigaci na jinou stránku, je nastavení nová stránka [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) k datům, jak je znázorněno v následujícím příkladu kódu:

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

Tento kód nastaví [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) z `DetailPage` instance na `Contact` instance a pak přejde do `DetailPage`.

`DetailPage` Pak datové vazby používá pro zobrazení `Contact` instance data, jak je znázorněno v následujícím příkladu kódu XAML:

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

Následující příklad kódu ukazuje, jak se dají naplnit datové vazby v C#:

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

Data se pak zobrazí na stránce řadou [ `Label` ](xref:Xamarin.Forms.Label) ovládacích prvků.

Další informace o datové vazbě naleznete v tématu [základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/index.md).

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak přejít na modální stránky. Modální stránky vyzývá uživatele k dokončení samostatná úloha, která nemůže být opuštění dokud je úloha dokončena nebo zrušena.


## <a name="related-links"></a>Související odkazy

- [Navigace po stránkách](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Modální okno (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
