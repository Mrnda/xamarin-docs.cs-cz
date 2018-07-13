---
title: Hierarchická navigace
description: Tento článek ukazuje, jak použít třídu NavigationPage provádět navigace v zásobníku poslední dovnitř, první (ven LIFO) stránky.
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: f8f8f9b4e5755e8b1707178fef633321b64e4e94
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994673"
---
# <a name="hierarchical-navigation"></a>Hierarchická navigace

_Třída NavigationPage poskytuje hierarchické navigační prostředí, kde uživatel je možné procházet stránkách vpřed a zpět, podle potřeby. Třída implementuje navigace jako poslední dovnitř, první (ven LIFO) zásobníku objekty stránky. Tento článek ukazuje, jak použít třídu NavigationPage provádět navigace v zásobníku stránek._

Tento článek popisuje v následujících tématech:

- [Provádění navigace](#Performing_Navigation) – vytvoření stránky kořenový, odesílání stránky do navigačního zásobníku, odebrání stránky z navigační zásobník a animace přechody stránek.
- [Předávání dat při navigaci](#Passing_Data_when_Navigating) – předávání dat prostřednictvím konstruktor stránky a prostřednictvím `BindingContext`.
- [Manipulace s navigační zásobník](#Manipulating_the_Navigation_Stack) – manipulace s zásobníku pomocí vložení nebo odstranění stránky.

## <a name="overview"></a>Přehled

Přesunutí z jedné stránky na jiný, aplikace budou nabízená oznámení nové stránky do navigačního zásobníku, kde se tak stane aktivní stránkou., jak je znázorněno v následujícím diagramu:

![](hierarchical-images/pushing.png "Odesílání stránky do navigačního zásobníku")

Pokud chcete vrátit zpět na předchozí stránku, aplikaci zobrazte aktuální stránku z navigační zásobník a na novou stránku nejvyšší úrovně se stane aktivní stránkou., jak je znázorněno v následujícím diagramu:

![](hierarchical-images/popping.png "Odebrání stránky z navigační zásobník")

Jsou navigační metody vystavené [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) vlastnost na žádném [ `Page` ](xref:Xamarin.Forms.Page) odvozené typy. Tyto metody poskytují možnost Vložit stránky do navigačního zásobníku pop stránky z navigační zásobník a, provádět manipulace s zásobníku.

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Provádění navigace

Hierarchická navigace [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) třída se používá k procházení zásobníku [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) objekty. Na následujících snímcích obrazovky zobrazit základních komponent `NavigationPage` na jednotlivých platformách:

![](hierarchical-images/navigationpage-components.png "NavigationPage komponenty")

Rozložení [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) je závislý na platformě:

- V Iosu se nachází v horní části stránky, která zobrazí název a má navigační panel *zpět* tlačítko, které vrátí na předchozí stránku.
- V systému Android se navigační panel nachází v horní části stránky, která zobrazuje název, ikonu, a *zpět* tlačítko, které vrátí na předchozí stránku. Ikona je definována v `[Activity]` atribut, který upraví `MainActivity` třídu v projektu pro specifické pro platformu Android.
- Na univerzální platformu Windows se nachází v horní části stránky, která zobrazuje název navigační panel.

Na všech platformách se hodnota [ `Page.Title` ](xref:Xamarin.Forms.Page.Title) vlastnost se zobrazí jako nadpis stránky.

> [!NOTE]
> Doporučujeme `NavigationPage` mělo být vyplněno pomocí `ContentPage` pouze instance.

### <a name="creating-the-root-page"></a>Vytvoření kořenové stránky

První stránka Přidat do navigační zásobník se označuje jako *kořenové* stránku aplikace a následující příklad kódu ukazuje, jak to lze provést:

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

To způsobí, že `Page1Xaml` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance doručit bez vyžádání do navigačního zásobníku, kde bude aktivní stránce a stránce kořenové aplikace. To můžete vidět na následujících snímcích obrazovky:

![](hierarchical-images/mainpage.png "Hlavní stránka navigačním zásobníku")

> [!NOTE]
> [ `RootPage` ](xref:Xamarin.Forms.NavigationPage.RootPage) Vlastnost [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instance poskytuje přístup k první stránka v navigační zásobník.

### <a name="pushing-pages-to-the-navigation-stack"></a>Odesílání stránky do navigačního zásobníku

Přejít na `Page2Xaml`, je potřeba vyvolat [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) metodu [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) vlastnost aktuální stránky jako předvedenou v následujícím příkladu kódu:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

To způsobí, že `Page2Xaml` instance doručit bez vyžádání do navigačního zásobníku, kde se stane aktivní stránkou. To můžete vidět na následujících snímcích obrazovky:

![](hierarchical-images/secondpage.png "Stránka vloženy do navigačního zásobníku")

Když [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) je metoda vyvolána, dojde k následujícím událostem:

- Volání funkce stránky `PushAsync` má jeho [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) přepsání vyvolána.
- Na stránce se přejde poté, jeho [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) přepsání vyvolána.
- `PushAsync` Dokončení úkolu.

Přesné pořadí, ve kterém se tyto události nastanou je však závislý na platformě. Další informace najdete v tématu [kapitoly 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms knihy.

> [!NOTE]
> Volání [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) a [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) přepsání nemůže být zpracován jako garantované údaje o navigaci na stránce. Například v Iosu `OnDisappearing` přepsání je volána na aktivní stránce, když se aplikace ukončí.

### <a name="popping-pages-from-the-navigation-stack"></a>Vyjímání stránky v navigačním zásobníku

Aktivní stránkou. může být odebrány z navigační zásobník stisknutím kombinace kláves *zpět* tlačítko na zařízení, bez ohledu na to, zda se jedná o fyzického tlačítka na zařízení nebo tlačítko na obrazovce.

Programově vrátit na původní stránku `Page2Xaml` instance musí vyvolat [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) způsob, jak je ukázáno v následujícím příkladu kódu:

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

To způsobí, že `Page2Xaml` instance má být odebrána z navigační zásobník s novou stránku nejvyšší úrovně stane aktivní stránkou. Když [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) je metoda vyvolána, dojde k následujícím událostem:

- Volání funkce stránky `PopAsync` má jeho [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) přepsání vyvolána.
- Na stránce se vrací do jeho [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) přepsání vyvolána.
- `PopAsync` Úkolů vrátí.

Přesné pořadí, ve kterém se tyto události nastanou je však závislý na platformě. Další informace najdete v části [kapitoly 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms knihy.

Stejně jako [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) a [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) metody, [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) také poskytuje vlastnost jednotlivých stránek [ `PopToRootAsync` ](xref:Xamarin.Forms.NavigationPage.PopToRootAsync) metoda, která je znázorněna v následujícím příkladu kódu:

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

Tato metoda se zobrazí všechny kromě kořenové [ `Page` ](xref:Xamarin.Forms.Page) vypnout navigační zásobník, takže kořenové stránky aplikace aktivní stránkou.

### <a name="animating-page-transitions"></a>Animace přechody stránek

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Vlastnost jednotlivých stránek také poskytuje přepsané nabízená oznámení a pop metody, které obsahují `boolean` parametr, který určuje, zda se zobrazí stránka animace průběhu navigace, jak je znázorněno v následujícím kódu Příklad:

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

Nastavení `boolean` parametr `false` zakáže stránku přechodu animace, při nastavení parametru na `true` umožňuje přechod na stránky animace, za předpokladu, že ji podporuje základní platformy. Nabízená oznámení a pop metody, které nemají tento parametr však povolit animace ve výchozím nastavení.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Předání dat při navigaci

Někdy je nezbytné pro stránku k předávání dat při navigaci na jinou stránku. Dvě techniky způsoby předávání dat prostřednictvím konstruktor stránky a nastavením nová stránka [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) k datům. Každý nyní probereme zase.

### <a name="passing-data-through-a-page-constructor"></a>Předání dat prostřednictvím konstruktoru stránky

Nejjednodušší techniku pro předávání dat na jinou stránku během navigace je prostřednictvím parametru konstruktor stránky, která je znázorněna v následujícím příkladu kódu:

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

Tento kód vytvoří `MainPage` instance, předávání v aktuálním datem a časem ve formátu ISO8601, která je zabalena v [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instance.

`MainPage` Instance přijímá data prostřednictvím konstruktoru parametru, jak je znázorněno v následujícím příkladu kódu:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Data se následně zobrazí na stránce nastavení [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) vlastnost, jak je znázorněno na následujících snímcích obrazovky:

![](hierarchical-images/passing-data-constructor.png "Data předaná pomocí konstruktoru stránky")

### <a name="passing-data-through-a-bindingcontext"></a>Předání dat prostřednictvím BindingContext

Alternativním přístupem k předávání dat při navigaci na jinou stránku, je nastavení nová stránka [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) k datům, jak je znázorněno v následujícím příkladu kódu:

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

Tento kód nastaví [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) z `SecondPage` instance na `Contact` instance a pak přejde do `SecondPage`.

`SecondPage` Pak datové vazby používá pro zobrazení `Contact` instance data, jak je znázorněno v následujícím příkladu kódu XAML:

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

Následující příklad kódu ukazuje, jak se dají naplnit datové vazby v C#:

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

Data se pak zobrazí na stránce řadou [ `Label` ](xref:Xamarin.Forms.Label) řídí, jak je znázorněno na následujících snímcích obrazovky:

![](hierarchical-images/passing-data-bindingcontext.png "Data předaná prostřednictvím BindingContext")

Další informace o datové vazbě naleznete v tématu [základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>Manipulace s navigačním zásobníku

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Zpřístupňuje vlastnost [ `NavigationStack` ](xref:Xamarin.Forms.INavigation.NavigationStack) vlastností, ze kterého můžete získat na stránkách v navigačním zásobníku. Při přístupu k navigační zásobník udržuje Xamarin.Forms `Navigation` poskytuje vlastnost [ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*) a [ `RemovePage` ](xref:Xamarin.Forms.INavigation.RemovePage*) metody pro práci s zásobníku vložením stránky nebo jejich odebrání.

[ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*) Metoda vloží zadanou stránku v navigačním zásobníku před existující zadané stránky, jak je znázorněno v následujícím diagramu:

![](hierarchical-images/insert-page-before.png "Vložení stránky v navigačním zásobníku")

[ `RemovePage` ](xref:Xamarin.Forms.INavigation.RemovePage*) Metoda odstraní zadanou stránku z navigačního zásobníku, jak je znázorněno v následujícím diagramu:

![](hierarchical-images/remove-page.png "Odebrání stránky z navigační zásobník")

Tyto metody umožňují vlastní navigační prostředí, jako je například nahradit novou stránku, po úspěšném přihlášení přihlašovací stránku. Následující příklad kódu ukazuje tento scénář:

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

Za předpokladu, že přihlašovací údaje uživatele jsou správné, `MainPage` instance je vložen do navigační zásobník před aktuální stránku. [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) Metoda pak odstraní aktuální stránku z navigační zásobník s `MainPage` instance stane aktivní stránkou.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak používat [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) pro provádění navigace v zásobníku stránek. Tato třída poskytuje hierarchické navigační prostředí, kde uživatel je možné procházet stránkách vpřed a zpět, podle potřeby. Třída implementuje navigace jako poslední dovnitř, první (ven LIFO) zásobníku [ `Page` ](xref:Xamarin.Forms.Page) objekty.


## <a name="related-links"></a>Související odkazy

- [Navigace po stránkách](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Hierarchické (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [Jak vytvořit přihlášení ve službě Flow obrazovky v ukázce Xamarin.Forms (Xamarin University Video)](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Postup vytvoření přihlášení v toku obrazovky v Xamarin.Forms (Xamarin University Video)](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](xref:Xamarin.Forms.NavigationPage)
