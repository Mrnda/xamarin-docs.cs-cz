---
title: Hierarchická navigace
description: Třída NavigationPage poskytuje hierarchické navigační prostředí, kde je možné procházet stránky, dopředný a podle potřeby zpětné uživatele. Třída implementuje navigační jako zásobník ven (LIFO) last-in objektů stránky. Tento článek ukazuje, jak používat třídu NavigationPage k provádění navigace v zásobníku stránek.
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 3fc5b24474230fd2b2477f020ac24cd72996d7b1
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="hierarchical-navigation"></a>Hierarchická navigace

_Třída NavigationPage poskytuje hierarchické navigační prostředí, kde je možné procházet stránky, dopředný a podle potřeby zpětné uživatele. Třída implementuje navigační jako zásobník ven (LIFO) last-in objektů stránky. Tento článek ukazuje, jak používat třídu NavigationPage k provádění navigace v zásobníku stránek._

Tento článek popisuje v následujících tématech:

- [Provádění navigační](#Performing_Navigation) – vytváření stránce kořenové, vkládání stránky se zásobníkem navigace, odebrání stránky z navigační zásobníku a animace přechody stránek.
- [Předávání dat při přechodu](#Passing_Data_when_Navigating) – předávání dat prostřednictvím stránky konstruktor a prostřednictvím `BindingContext`.
- [Manipulace s zásobníku navigační](#Manipulating_the_Navigation_Stack) – manipulace s zásobníku vložení nebo odebráním stránky.

## <a name="overview"></a>Přehled

Pokud chcete přesunout z jedné stránky na jiný, aplikace předá novou stránku do zásobníku navigace, kde se stane aktivní stránku, jak je znázorněno v následujícím diagramu:

![](hierarchical-images/pushing.png "Vkládání stránky navigační zásobníku")

Pokud chcete vrátit zpět na předchozí stránku, aplikace bude pop aktuální stránku z navigační zásobníku a nová stránka nejhornější stane aktivní stránku, jak je znázorněno v následujícím diagramu:

![](hierarchical-images/popping.png "Odebrání stránky v navigačním zásobníku")

Navigační metody jsou vystavené [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) vlastnost na žádném [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) odvozených typů. Tyto metody poskytují možnost tak, aby nabízel stránek do zásobníku navigace na pop stránky v zásobníku navigační a k provádění manipulace s zásobníku.

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Provádění navigace

V hierarchické navigaci [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) třída se používá k procházení zásobníku z [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) objekty. Na následujících snímcích obrazovky zobrazit hlavními součástmi `NavigationPage` na jednotlivých platformách:

![](hierarchical-images/navigationpage-components.png "NavigationPage součásti")

Rozložení [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) je závislý na platformě:

- V systému iOS, se nachází v horní části stránky, který zobrazí název, a který má navigačním panelu *zpět* tlačítko, které vrátí na předchozí stránku.
- V systému Android, se nachází v horní části stránky, který zobrazuje název, ikonu, navigačním panelu a *zpět* tlačítko, které vrátí na předchozí stránku. Ikona je definována v `[Activity]` atribut, který upraví `MainActivity` třídy v projektu pro specifické pro platformu Android.
- Na univerzální platformu Windows nachází v horní části stránky, který zobrazuje název navigačním panelu. 

Na všech platformách hodnotu [ `Page.Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) vlastnosti se zobrazí jako nadpis stránky.

> [!NOTE]
> Je doporučeno, `NavigationPage` by měl být naplněný `ContentPage` pouze instance.

### <a name="creating-the-root-page"></a>Vytváření kořenové stránky

Na první stránku přidat navigační zásobníku se označuje jako *kořenové* stránku aplikace a následující příklad kódu ukazuje, jak toho dosahuje:

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

To způsobí, že `Page1Xaml` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance, která má být vloženy do zásobníku navigace, kde bude aktivní stránky a stránky kořenové aplikace. Můžete se podívat na následujících snímcích obrazovky:

![](hierarchical-images/mainpage.png "Hlavní stránka navigační zásobníku")

> [!NOTE]
> [ `RootPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.RootPage/) Vlastnost [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) instance poskytuje přístup na první stránku v zásobníku navigace.

### <a name="pushing-pages-to-the-navigation-stack"></a>Vkládání stránky navigační zásobníku

Přejděte na `Page2Xaml`, je nutné vyvolat [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) metodu [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) vlastnost aktuální stránky, jako předvedenou v následujícím příkladu kódu:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

To způsobí, že `Page2Xaml` instance, která má být vloženy do zásobníku navigace, kde bude aktivní stránku. Můžete se podívat na následujících snímcích obrazovky:

![](hierarchical-images/secondpage.png "Stránka nabídnutých do zásobníku navigace")

Když [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) metoda je volána, dojde k následujícím událostem:

- Stránka volání `PushAsync` má jeho [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) přepsání vyvolat.
- Na stránce se přešli jeho [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) přepsání vyvolat.
- `PushAsync` Dokončení úlohy.

Přesné pořadí, ve kterém k těmto událostem dojde, je však platformy, které jsou závislé. Další informace najdete v tématu [kapitoly 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charlese Petzold Xamarin.Forms knihy.

> [!NOTE]
> Volání [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) a [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) přepsání nemohou být považovány za zaručenou indikace navigaci na stránce. Například v systému iOS `OnDisappearing` přepsání je na stránce active volána, když se aplikace ukončí.

### <a name="popping-pages-from-the-navigation-stack"></a>Operace stránky z navigační zásobníku

Aktivní stránku může být odebrány ze zásobníku navigační stisknutím kombinace kláves *zpět* tlačítko na zařízení, bez ohledu na to, zda je na zařízení fyzické tlačítko nebo na obrazovce tlačítko.

Prostřednictvím kódu programu vrátit na stránku původní `Page2Xaml` musí vyvolání instance [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) metoda, jak je ukázáno v následujícím příkladu kódu:

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

To způsobí, že `Page2Xaml` instance, která má být odebrán, navigace, s novou nejhornější stránku stane aktivní stránku. Když [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) metoda je volána, dojde k následujícím událostem:

- Stránka volání `PopAsync` má jeho [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) přepsání vyvolat.
- Na stránce jako odpověď na jeho [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) přepsání vyvolat.
- `PopAsync` Úkolů vrátí.

Přesné pořadí, ve kterém k těmto událostem dojde, je však platformy, které jsou závislé. Další informace najdete v části [kapitoly 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charlese Petzold Xamarin.Forms knihy.

A také [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) a [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) metody, [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) také obsahuje vlastnost každé stránce [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopToRootAsync()/) metodu, která je znázorněno v následujícím příkladu kódu:

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

Tato metoda se zobrazí všechny kromě kořenové [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) vypnout navigační zásobníku, takže stránku kořenové aplikace aktivní stránky.

### <a name="animating-page-transitions"></a>Animace přechody stránek

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Vlastnost každé stránce poskytuje taky přepsaného nabízené a pop metody, které zahrnují `boolean` parametr, který určuje, jestli se má zobrazit stránku animace během navigace, jak je znázorněno v následujícím kódu Příklad:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushAsync (new Page2Xaml (), false);
}

async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopAsync (false);
}

async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopToRootAsync (false);
}
```

Nastavení `boolean` parametru `false` zakáže animace přechod stránky, při nastavení parametru na `true` umožňuje animace přechod stránky za předpokladu, že ji podporuje základní platformy. Metody nabízené a pop, která nemají tento parametr však povolit animace ve výchozím nastavení.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Předávání dat při přechodu

Někdy je nezbytné pro stránku k předávání dat při navigaci na jinou stránku. Dva postupy provádění to jsou předávání dat prostřednictvím stránky konstruktor a nastavením nová stránka [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) k datům. Každý teď probereme naopak.

### <a name="passing-data-through-a-page-constructor"></a>Předávání dat prostřednictvím stránky konstruktor

Nejjednodušší způsob pro předávání dat při navigaci na jinou stránku je prostřednictvím parametru stránky konstruktor, který je znázorněno v následujícím příkladu kódu:

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

Tento kód vytvoří `MainPage` instance, předávání v aktuální datum a čas ve formátu ISO 8601, který je uzavřen do [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) instance.

`MainPage` Instance přijímá data prostřednictvím parametr konstruktoru, jak je znázorněno v následujícím příkladu kódu:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Data se následně zobrazí na stránce nastavení [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) vlastnost, jak je vidět na následujících snímcích obrazovky:

![](hierarchical-images/passing-data-constructor.png "Data předána stránce konstruktor")

### <a name="passing-data-through-a-bindingcontext"></a>Předávání dat prostřednictvím vazby

Alternativní způsob pro předávání dat při navigaci na jinou stránku je nastavením nová stránka [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) k datům, jak je znázorněno v následujícím příkladu kódu:

```csharp
async void OnNavigateButtonClicked (object sender, EventArgs e)
{
  var contact = new Contact {
    Name = "Jane Doe",
    Age = 30,
    Occupation = "Developer",
    Country = "USA"
  };

  var secondPage = new SecondPage ();
  secondPage.BindingContext = contact;
  await Navigation.PushAsync (secondPage);
}
```

Nastaví tento kód [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) z `SecondPage` instance k `Contact` instance a pak přejde `SecondPage`.

`SecondPage` Pak použije k zobrazení datová vazba `Contact` instance dat, jak je znázorněno v následujícím příkladu kódu XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="PassingData.SecondPage"
             Title="Second Page">
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
            ...
            <Button x:Name="navigateButton" Text="Previous Page" Clicked="OnNavigateButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Následující příklad kódu ukazuje, jak lze provést datové vazby v C#:

```csharp
public class SecondPageCS : ContentPage
{
  public SecondPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var navigateButton = new Button { Text = "Previous Page" };
    navigateButton.Clicked += OnNavigateButtonClicked;

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
        navigateButton
      }
    };
  }

  async void OnNavigateButtonClicked (object sender, EventArgs e)
  {
    await Navigation.PopAsync ();
  }
}
```

Data se následně zobrazí na stránce řadu [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) řídí, jak je vidět na následujících snímcích obrazovky:

![](hierarchical-images/passing-data-bindingcontext.png "Data předána vazby")

Další informace o vazbě dat najdete v tématu [základy vazby dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>Manipulace s navigační zásobníku

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Zpřístupňuje vlastnost [ `NavigationStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) ze kterého můžete získat na stránkách v zásobníku navigační vlastnost. Při Xamarin.Forms udržuje přístup k zásobníku navigace `Navigation` vlastnost poskytuje [ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) a [ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) metody pro práci s zásobníku vložením stránky nebo jejich odebrání.

[ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) Metoda vloží zadanou stránku v zásobníku navigační před existující zadané stránky, jak je znázorněno v následujícím diagramu:

![](hierarchical-images/insert-page-before.png "Vložení stránky v zásobníku navigace")

[ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) Metoda odebere zadané stránky zásobník navigace, jak je znázorněno v následujícím diagramu:

![](hierarchical-images/remove-page.png "Na stránce odebráním navigační zásobníku")

Tyto metody povolit vlastní navigační prostředí, jako je například přihlašovací stránku nahraďte novou stránku, po úspěšném přihlášení. Následující příklad kódu ukazuje tento scénář:

```csharp
async void OnLoginButtonClicked (object sender, EventArgs e)
{
  ...
  var isValid = AreCredentialsCorrect (user);
  if (isValid) {
    App.IsUserLoggedIn = true;
    Navigation.InsertPageBefore (new MainPage (), this);
    await Navigation.PopAsync ();
  } else {
    // Login failed
  }
}

```

Za předpokladu, že přihlašovací údaje uživatele jsou správné, `MainPage` instance se vloží do zásobníku navigační před aktuální stránku. [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) Metoda pak odstraní aktuální stránku v zásobníku navigační s `MainPage` instance stane aktivní stránky.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak používat [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) třída provádět navigace v zásobníku stránek. Tato třída poskytuje hierarchické navigační prostředí, kde je možné procházet stránky, dopředný a podle potřeby zpětné uživatele. Třída implementuje navigační jako zásobník ven (LIFO) last-in služby [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objekty.


## <a name="related-links"></a>Související odkazy

- [Navigace stránky](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Hierarchické (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [Postup vytvoření znaménkem v toku obrazovky v ukázce Xamarin.Forms (Xamarin univerzity Video)](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Postup vytvoření znaménkem v toku obrazovky v Xamarin.Forms (Xamarin univerzity Video)](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)
