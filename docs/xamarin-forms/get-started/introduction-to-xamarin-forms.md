---
title: "Úvod do Xamarin.Forms"
description: "Xamarin.Forms je že napříč platformami nativně podporovaný abstrakce nástrojů uživatelského rozhraní, která umožňuje vývojářům snadno vytvářet uživatelského rozhraní, které můžete sdílet mezi Android, iOS, Windows a Windows Phone. Uživatelská rozhraní jsou vykreslovány pomocí nativní ovládací prvky typu cílovou platformu, povolíte Xamarin.Forms aplikací zachovat odpovídající vzhledu a chování pro každou platformu. Tento článek obsahuje úvod do Xamarin.Forms a jak začít pracovat psaní aplikací s ním."
ms.topic: article
ms.prod: xamarin
ms.assetid: f619595f-3ee7-439b-a1bc-d13e5106e6e9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 593e720e4a6125e2ef4a1c9488186cb2c04dcd66
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="an-introduction-to-xamarinforms"></a>Úvod do Xamarin.Forms

_Xamarin.Forms je že napříč platformami nativně podporovaný abstrakce nástrojů uživatelského rozhraní, která umožňuje vývojářům snadno vytvářet uživatelského rozhraní, které můžete sdílet mezi Android, iOS, Windows a Windows Phone. Uživatelská rozhraní jsou vykreslovány pomocí nativní ovládací prvky typu cílovou platformu, povolíte Xamarin.Forms aplikací zachovat odpovídající vzhledu a chování pro každou platformu. Tento článek obsahuje úvod do Xamarin.Forms a jak začít pracovat psaní aplikací s ním._

<a name="Overview" />

## <a name="overview"></a>Přehled

Xamarin.Forms je rozhraní, které umožňuje vývojářům umožní rychle vytvořit křížové platformy uživatelská rozhraní. Poskytuje, že se že jedná vlastní abstrakci pro uživatelské rozhraní, které bude vykresleno pomocí nativní ovládací prvky na iOS, Android, Windows nebo Windows Phone. To znamená, že aplikace můžete sdílet velkou část svůj kód uživatelského rozhraní a zachovat přitom přirozený vzhled a chování cílové platformy.

Xamarin.Forms umožňuje rychlé vytváření prototypů aplikací, které můžete rozvíjet v čase pro komplexní aplikace. Protože aplikace Xamarin.Forms nativních aplikací, nemají omezení jiných sadách například sandboxing prohlížeče, omezené rozhraní API nebo snížený výkon. Aplikace napsané v Xamarin.Forms se můžou využívat některé z rozhraní API nebo funkce základní platformy, jako například (ale nikoli výhradně) CoreMotion, PassKit a StoreKit v systému iOS; NFC a služby Google Play v systému Android; a dlaždice v systému Windows. Kromě toho je možné vytvořit aplikace, které se mají částí jejich uživatelské rozhraní vytvořen s Xamarin.Forms, zatímco ostatní části jsou vytvořené pomocí nativních nástrojů uživatelského rozhraní.

Xamarin.Forms aplikace jsou navržen stejným způsobem jako tradiční aplikace napříč platformami. Nejběžnější možných přístupů je použít [přenosné knihovny](~/cross-platform/app-fundamentals/pcl.md) nebo [sdílených projektů](~/cross-platform/app-fundamentals/shared-projects.md) levné sdíleného kódu a vytvořit platformy konkrétní aplikace, které bude využívat sdílené kódu.

Vytvoření uživatelského rozhraní v Xamarin.Forms dvěma způsoby. První způsob je zcela s zdrojový kód C# vytvoření uživatelská rozhraní. Druhý způsob spočívá, je použít *Extensible Application Markup Language* (XAML), deklarativní jazyk, který se používá k popisu uživatelského rozhraní. Další informace o XAML najdete v tématu [XAML Základy](~/xamarin-forms/xaml/xaml-basics/index.md).

Tento článek popisuje základní informace o rozhraní Xamarin.Forms a obsahuje následující témata:

-  [Zkoumání aplikaci Xamarin.Forms](#Examining_A_Xamarin.Forms_Application).
-  [Jak se používají Xamarin.Forms stránek a ovládacích prvků](#Views_and_Layouts).
-  [Použití zobrazení seznam dat](#Lists_in_Xamarin.Forms).
-  [Jak nastavit datová vazba](#Data_Binding).
-  [Jak navigace mezi stránkami](#Navigation).
-  [Další kroky](#Next_Steps).

<a name="Examining_A_Xamarin_Forms_Application" />

### <a name="examining-a-xamarinforms-application"></a>Zkoumání aplikaci na platformě Xamarin.Forms

V sadě Visual Studio pro Mac a Visual Studio vytvoří výchozí šablony aplikaci Xamarin.Forms nejjednodušší řešení Xamarin.Forms možné, který zobrazí text pro uživatele. Pokud aplikaci spouštíte, by měla vypadat podobně jako na následujících snímcích obrazovky:

[ ![](introduction-to-xamarin-forms-images/image05-sml.png "Výchozí aplikaci Xamarin.Forms")](introduction-to-xamarin-forms-images/image05.png "výchozí aplikaci Xamarin.Forms")

Každý obrazovky na snímcích obrazovky odpovídá *stránky* v Xamarin.Forms. A [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) představuje *aktivity* v systému Android, *View Controller* v iOS, nebo *stránky* v univerzální pro Windows Platforma (UWP). Vytvoří instanci ukázce výše na snímcích obrazovky [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) objektu a použije ho k zobrazení [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/).

Pokud chcete maximalizovat opakované použití kódu spuštění, Xamarin.Forms aplikací mít jednu třídu s názvem `App` který zodpovídá za vytvoření instance první [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) které se bude zobrazovat. Příkladem `App` třídy můžete vidět v následujícím kódu:

```csharp
public class App : Application
{
  public App ()
  {
    MainPage = new ContentPage {
      Content =  new Label
      {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
      }
    };
  }
}
```

Tento kód vytvoří novou [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) objekt, který se zobrazí jeden [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) obě vodorovně a svisle na střed stránky.

<a name="Launching_the_Initial_Xamarin_Forms_Page_on_Each_Platform" />

### <a name="launching-the-initial-xamarinforms-page-on-each-platform"></a>Spouští se počáteční stránka Xamarin.Forms na jednotlivých platformách

Chcete-li použít [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) v aplikaci, musí každá aplikace platformy inicializovat rozhraní Xamarin.Forms a poskytovat instance [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) jako spouštění. Tento krok inicializace se liší od platformách a je popsána v následujících částech.

<a name="Launching_in_iOS" />

#### <a name="ios"></a>iOS

Spusťte úvodní stránky Xamarin.Forms v iOS, tento projekt platformy zahrnuje `AppDelegate` třídu, která dědí z `Xamarin.Forms.Platform.iOS.FormsApplicationDelegate` třídy, jak je ukázáno v následujícím příkladu kódu:

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
      global::Xamarin.Forms.Forms.Init ();
      LoadApplication (new App ());
      return base.FinishedLaunching (app, options);
    }
}
```

`FinishedLoading` Přepsání inicializuje pomocí volání rozhraní Xamarin.Forms `Init` metoda. To způsobí, že implementace specifické pro iOS Xamarin.Forms má být načten v aplikaci, než je ve volání nastavení řadiče zobrazení kořenové `LoadApplication` metoda.

<a name="Launching_in_Android" />

#### <a name="android"></a>Android

Spusťte úvodní stránky Xamarin.Forms v Android, projektu platformy zahrnuje kód, který vytvoří `Activity` s `MainLauncher` atributem inherting aktivity z `FormsApplicationActivity` třídy, jak je ukázáno v následujícím příkladu kódu:

```csharp
namespace HelloXamarinFormsWorld.Android
{
    [Activity(Label = "HelloXamarinFormsWorld", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication (new App ());
        }
    }
}
```

`OnCreate` Přepsání inicializuje pomocí volání rozhraní Xamarin.Forms `Init` metoda. To způsobí, že implementace specifické pro Android Xamarin.Forms má být načten v aplikaci, než je aplikace Xamarin.Forms načtena.

<a name="Launching_in_Windows_Phone" />

#### <a name="windows-phone-81-winrt"></a>Windows Phone 8.1 (WinRT)

V prostředí Windows Runtime aplikace `Init` metoda, která inicializuje rozhraní Xamarin.Forms se volá z `App` – třída:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

To způsobí, že implementace specifické pro Windows Phone Xamarin.Forms načíst v aplikaci. Úvodní stránky Xamarin.Forms je spuštěn `MainPage` třídy, jak je ukázáno v následujícím příkladu kódu:

```csharp
public partial class MainPage
{
    public MainPage()
    {
      this.InitializeComponent();
      this.NavigationCacheMode = NavigationCacheMode.Required;
      this.LoadApplication(new HelloXamarinFormsWorld.App());
    }
}
```

Xamarin.Forms aplikace je načtena s `LoadApplication` metoda.

Xamarin.Forms má také podpora pro Windows 8.1. Informace o tom, jak nakonfigurovat tuto funkci typy projektů naleznete v tématu [projekty Windows instalace](~/xamarin-forms/platform/windows/installation/index.md).

#### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

V aplikacích pro univerzální platformu Windows (UWP) `Init` metoda, která inicializuje rozhraní Xamarin.Forms se volá z `App` třídy:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

To způsobí, že implementace specifické pro UPW Xamarin.Forms načíst v aplikaci. Úvodní stránky Xamarin.Forms je spuštěn `MainPage` třídy, jak je ukázáno v následujícím příkladu kódu:

```csharp
public partial class MainPage
{
    public MainPage()
    {
      this.InitializeComponent();
      this.LoadApplication(new HelloXamarinFormsWorld.App());
    }
}
```

Xamarin.Forms aplikace je načtena s `LoadApplication` metoda.

<a name="Views_and_Layouts" />

### <a name="views-and-layouts"></a>Zobrazení a rozložení

Existují čtyři hlavní řízení skupiny použít k vytvoření uživatelského rozhraní aplikace Xamarin.Forms.

1. **Stránky** – Xamarin.Forms stránky představují obrazovky napříč platformami mobilních aplikací. Další informace o stránky najdete v tématu [Xamarin.Forms stránky](~/xamarin-forms/user-interface/controls/pages.md).
1. **Rozložení** – Xamarin.Forms rozložení jsou kontejnery použitý k sestavení zobrazení do logické struktury. Další informace o rozložení najdete v tématu [Xamarin.Forms rozložení](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Zobrazení** – Xamarin.Forms zobrazení se zobrazí v uživatelském rozhraní, jako je například popisky, tlačítek a textová vstupní pole ovládacích prvků. Další informace o zobrazení najdete v tématu [Xamarin.Forms zobrazení](~/xamarin-forms/user-interface/controls/views.md).
1. **Buněk** – Xamarin.Forms buněk jsou specializované prvky používané pro položky v seznamu a popisují, jak mají být vykresleny každou položku v seznamu. Další informace o buněk najdete v tématu [Xamarin.Forms buněk](~/xamarin-forms/user-interface/controls/cells.md).

Za běhu budou každý ovládací prvek mapována na ekvivalentní nativní, což je co bude vykreslen.

Ovládací prvky jsou hostované v rámci rozložení. [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) Třídy, která implementuje běžně používané rozložení, teď prozkoumá.

<a name="StackLayout" />

#### <a name="stacklayout"></a>StackLayout

[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) Zjednodušuje vývoj aplikací pro různé platformy tím, že automaticky uspořádání ovládacích prvků na obrazovce bez ohledu na velikost obrazovky. Každý podřízený element je umístěného jeden za druhým buď vodorovně nebo svisle v pořadí, v jakém byly přidány. Kolik místa `StackLayout` bude použití závisí na tom [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) a [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) jsou nastaveny vlastnosti, ale ve výchozím nastavení `StackLayout` se pokusí použít celou obrazovku.

Následující kód XAML ukazuje příklad použití [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) uspořádat tři [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládací prvky:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample1" Padding="20">
  <StackLayout Spacing="10">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

Ekvivalentní kódu C# je znázorněno v následujícím příkladu kódu:

```csharp
public class StackLayoutExample : ContentPage
{
    public StackLayoutExample()
    {
        Padding = new Thickness(20);
        var red = new Label
        {
            Text = "Stop", BackgroundColor = Color.Red, FontSize = 20
        };
        var yellow = new Label
        {
            Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20
        };
        var green = new Label
        {
            Text = "Go", BackgroundColor = Color.Green, FontSize = 20
        };

        Content = new StackLayout
        {
            Spacing = 10,
            Children = { red, yellow, green }
        };
    }
}
```

Ve výchozím nastavení [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) předpokládá svislou orientaci, jak je vidět na následujících snímcích obrazovky:

[ ![](introduction-to-xamarin-forms-images/image09-sml.png "Svislé StackLayout")](introduction-to-xamarin-forms-images/image09.png "svislé StackLayout")

Orientaci [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) můžete změnit na vodorovné orientaci, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample2" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

Ekvivalentní kódu C# je znázorněno v následujícím příkladu kódu:

```csharp
public class StackLayoutExample: ContentPage
{
    public StackLayoutExample()
    {
        // Code that creates labels removed for clarity
        Content = new StackLayout
        {
            Spacing = 10,
            VerticalOptions = LayoutOptions.End,
            Orientation = StackOrientation.Horizontal,
            HorizontalOptions = LayoutOptions.Start,
            Children = { red, yellow, green }
        };
    }
}
```

Na následujících snímcích obrazovky zobrazit výsledné rozložení:

[ ![](introduction-to-xamarin-forms-images/image10-sml.png "Vodorovné StackLayout")](introduction-to-xamarin-forms-images/image10.png "vodorovné StackLayout")

Je možné nastavit velikost ovládacích prvků prostřednictvím `HeightRequest` a `WidthRequest` vlastnosti, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample3" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" WidthRequest="100" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" WidthRequest="100" />
    <Label Text="Go" BackgroundColor="Green" Font="20" WidthRequest="200" />
  </StackLayout>
</ContentPage>
```

Ekvivalentní kódu C# je znázorněno v následujícím příkladu kódu:

```csharp
var red = new Label
{
    Text = "Stop", BackgroundColor = Color.Red, FontSize = 20, WidthRequest = 100
};
var yellow = new Label
{
    Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20, WidthRequest = 100
};
var green = new Label
{
    Text = "Go", BackgroundColor = Color.Green, FontSize = 20, WidthRequest = 200
};

Content = new StackLayout
{
    Spacing = 10,
    VerticalOptions = LayoutOptions.End,
    Orientation = StackOrientation.Horizontal,
    HorizontalOptions = LayoutOptions.Start,
    Children = { red, yellow, green }
};
```

Na následujících snímcích obrazovky zobrazit výsledné rozložení:

[ ![](introduction-to-xamarin-forms-images/image11-sml.png "Vodorovné StackLayout s LayoutOptions")](introduction-to-xamarin-forms-images/image11.png "vodorovné StackLayout s LayoutOptions")

Další informace o [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) třídy najdete v tématu [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

<a name="Lists_in_Xamarin_Forms" />

## <a name="lists-in-xamarinforms"></a>Seznamy v Xamarin.Forms

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Ovládací prvek je zodpovědná za zobrazení kolekce položek na obrazovce – jednotlivé položky `ListView` bude obsažená v jedné buňky. Ve výchozím nastavení `ListView` budou používat integrované [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) šablony a vykreslování na jednom řádku textu.

Následující příklad kódu ukazuje jednoduchý [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) příklad:

```csharp
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = new string []
{
    "Buy pears", "Buy oranges", "Buy mangos", "Buy apples", "Buy bananas"
};
Content = new StackLayout
{
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = { listView }
};
```

Následující snímek obrazovky ukazuje výsledná [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/):

 ![](introduction-to-xamarin-forms-images/image13.png "ListView")

Další informace o [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) řízení najdete v tématu [ListView](~/xamarin-forms/user-interface/listview/index.md).

<a name="Binding_to_a_Custom_Class" />

### <a name="binding-to-a-custom-class"></a>Vytvoření vazby na vlastní třídy

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Řízení lze také zobrazit vlastní objekty pomocí výchozího [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) šablony.

Následující příklad kódu ukazuje `TodoItem` třídy:

```csharp
public class TodoItem
{
    public string Name { get; set; }
    public bool Done { get; set; }
}
```

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Řízení je možné importovat, jak je ukázáno v následujícím příkladu kódu:

```csharp
listView.ItemsSource = new TodoItem [] {
    new TodoItem { Name = "Buy pears" },
    new TodoItem { Name = "Buy oranges", Done=true} ,
    new TodoItem { Name = "Buy mangos" },
    new TodoItem { Name = "Buy apples", Done=true },
    new TodoItem { Name = "Buy bananas", Done=true }
};
```

Chcete-li nastavit, které lze vytvořit vazbu `TodoItem` vlastnosti se zobrazí [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), jak je znázorněno v následujícím příkladu kódu:

```csharp
listView.ItemTemplate = new DataTemplate(typeof(TextCell));
listView.ItemTemplate.SetBinding(TextCell.TextProperty, "Name");
```

Tím se vytvoří vazbu, která určuje cestu k `TodoItem.Name` vlastnost a bude mít za následek na předchozím prohlíženém snímku obrazovky.

Další informace o vazbě na vlastní třídu najdete v tématu [zdroje dat ListView](~/xamarin-forms/user-interface/listview/data-and-databinding.md).

<a name="Selecting_an_Item_in_a_ListView" />

### <a name="selecting-an-item-in-a-listview"></a>Když vyberete položku v prvku ListView

Reakce na uživatele klepnou na buňku v [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) události by měly být zpracovány, jak je ukázáno v následujícím příkladu kódu:

```csharp
listView.ItemSelected += async (sender, e) => {
    await DisplayAlert("Tapped!", e.SelectedItem + " was tapped.", "OK");
};
```

Když obsažené v [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) metoda slouží otevře novou stránku s integrovanou back navigace. [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) Událostí můžete přístup k objektu, který je přidružen buňky prostřednictvím [ `e.SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem/) vlastnost navázat jej na novou stránku a zobrazit nová stránka pomocí `PushAsync`, jako ukázáno v následujícím příkladu kódu:

```csharp
listView.ItemSelected += async (sender, e) => {
    var todoItem = (TodoItem)e.SelectedItem;
    var todoPage = new TodoItemPage(todoItem); // so the new page shows correct data
    await Navigation.PushAsync(todoPage);
};
```

Každou platformu implementuje předdefinované back navigační vlastní způsobem. Další informace najdete v tématu [navigační](#Navigation).

Další informace o [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) výběr, najdete v části [ListView interaktivity](~/xamarin-forms/user-interface/listview/interactivity.md).

<a name="Customizing_the_appearance_of_a_cell" />

### <a name="customizing-the-appearance-of-a-cell"></a>Přizpůsobení vzhledu buňky

Vytváření podtříd může přizpůsobit vzhled buňky [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) třídy a nastavení typ této třídy lze [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) vlastnost [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/).

Buňky ukazuje následující snímek obrazovky se skládá z jednoho [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) a dvě [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládací prvky:

 ![](introduction-to-xamarin-forms-images/image14.png "Vlastní ListView vzhledu buněk")

Chcete-li vytvořit tuto vlastní rozložení [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) třída by měla rozčlenění, jak je ukázáno v následujícím příkladu kódu:

```csharp
class EmployeeCell : ViewCell
{
    public EmployeeCell()
    {
        var image = new Image
        {
            HorizontalOptions = LayoutOptions.Start
        };
        image.SetBinding(Image.SourceProperty, new Binding("ImageUri"));
        image.WidthRequest = image.HeightRequest = 40;

        var nameLayout = CreateNameLayout();
        var viewLayout = new StackLayout()
        {
           Orientation = StackOrientation.Horizontal,
           Children = { image, nameLayout }
        };
        View = viewLayout;
    }

    static StackLayout CreateNameLayout()
    {
        var nameLabel = new Label
        {
            HorizontalOptions= LayoutOptions.FillAndExpand
        };
        nameLabel.SetBinding(Label.TextProperty, "DisplayName");

        var twitterLabel = new Label
        {
           HorizontalOptions = LayoutOptions.FillAndExpand,
           Font = Fonts.Twitter
        };
        twitterLabel.SetBinding(Label.TextProperty, "Twitter");

        var nameLayout = new StackLayout()
        {
           HorizontalOptions = LayoutOptions.StartAndExpand,
           Orientation = StackOrientation.Vertical,
           Children = { nameLabel, twitterLabel }
        };
        return nameLayout;
    }
}
```

Kód provede následující úlohy:

-  Přidá [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) řízení a váže se k `ImageUri` vlastnost `Employee` objektu. Další informace o vazbě dat najdete v tématu [datová vazba](#Data_Binding).
-  Vytvoří [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) s vertikální orientace k uchování dva [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládací prvky. `Label` Je svázána `DisplayName` a `Twitter` vlastnosti `Employee` objektu.
-  Vytvoří [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) který bude hostitelem existující [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) a `StackLayout`. Bude umožňuje uspořádat podřízené pomocí vodorovné orientaci.

Po vytvoření vlastní buňky mohou být využívány službou [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ovládacího prvku pomocí zabalení v [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), jak je znázorněno v následujícím příkladu kódu:

```csharp
List<Employee> myListOfEmployeeObjects = GetAListOfAllEmployees();
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = myListOfEmployeeObjects;
listView.ItemTemplate = new DataTemplate(typeof(EmployeeCell));
```

Tento kód bude poskytovat `List` z `Employee` k [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Každá buňka bude vykreslen pomocí `EmployeeCell` třídy. `ListView` Předá `Employee` do objektu `EmployeeCell` jako jeho [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/).

Další informace o přizpůsobení vzhledu buněk najdete v tématu [vzhledu buněk](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="Using_XAML_to_Create_and_Customize_A_List" />

### <a name="using-xaml-to-create-and-customize-a-list"></a>Vytvářet a přizpůsobovat seznam použitím jazyka XAML

Ekvivalent XAML [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) v předchozí části je znázorněn v následujícím příkladu kódu:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:XamarinFormsXamlSample;assembly=XamarinFormsXamlSample"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="listView" IsVisible="false" ItemsSource="{x:Static local:App.Employees}" ItemSelected="EmployeeListOnItemSelected">
    <ListView.ItemTemplate>
      <DataTemplate>
        <ViewCell>
          <ViewCell.View>
            <StackLayout Orientation="Horizontal">
              <Image Source="{Binding ImageUri}" WidthRequest="40" HeightRequest="40" />
              <StackLayout Orientation="Vertical" HorizontalOptions="StartAndExpand">
                <Label Text="{Binding DisplayName}" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Twitter}" Font="{x:Static constants:Fonts.Twitter}"/>
              </StackLayout>
            </StackLayout>
          </ViewCell.View>
        </ViewCell>
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

Definuje tento XAML [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) obsahující [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Zdroj dat `ListView` jsou nastavena pomocí [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) atribut. Rozložení každý řádek `ItemsSource` je definována v rámci [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) element.

<a name="Data_Binding" />

## <a name="data-binding"></a>Datová vazba

Datová vazba připojí dva objekty, volá se *zdroj* a *cíl*. *Zdroj* objekt poskytuje data. *Cíl* objektu bude využívat (a často zobrazení) data ze zdrojového objektu. Například [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) (*cílový* objekt) vytvoří vazbu běžně jeho [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) vlastnosti veřejné `string` vlastnost *zdroj* objektu. Následující diagram znázorňuje vztah vazby:

![](introduction-to-xamarin-forms-images/data-binding.png "Datová vazba")

Hlavní výhodou datová vazba je, že už máte starat o synchronizaci dat mezi zobrazení a zdroj dat. Změny v *zdroj* objekt se automaticky instaluje do *cíl* objektu servisní rámcem vazby a změny v cílový objekt můžete volitelně vloží zpět do *zdroj* objektu.

Vytvoření datové vazby je dvou krocích:

- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Vlastnost *cíl* objekt musí být nastavena na *zdroj*.
- Je nutné vytvořit vazbu mezi *cíl* a *zdroj*. V jazyce XAML, dosahuje pomocí [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) – rozšíření značek. V jazyce C#, můžete toho dosáhnout ve [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) metoda.

Další informace o vazbě dat najdete v tématu [základy vazby dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

### <a name="xaml"></a>XAML

Následující kód ukazuje příklad provedení vazby dat v jazyce XAML:

```xaml
<Entry Text="{Binding FirstName}" ... />
```

Vytvoření vazby mezi [ `Entry.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) vlastnost a `FirstName` vlastnost *zdroj* vytvořil objekt. Změny provedené v `Entry` řízení se automaticky rozšíří do `employeeToDisplay` objektu. Podobně pokud jsou provedeny změny do `employeeToDisplay.FirstName` vlastnost, modul vazby Xamarin.Forms se také aktualizovat obsah `Entry` ovládacího prvku. To se označuje jako *vazba obousměrná*. Aby obousměrné vazby pro práci, musí implementovat třídu modelu `INotifyPropertyChanged` rozhraní.

I když [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) vlastnost `EmployeeDetailPage` třídy lze nastavit v jazyce XAML, zde je nastaveno v kódu na instanci `Employee` objektu:

```csharp
public EmployeeDetailPage(Employee employee)
{
    InitializeComponent();
    this.BindingContext = employee;
}
```

Při [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) vlastnost jednotlivých *cíl* objektu může být nastavena samostatně, to není nezbytné. `BindingContext` je speciální vlastnost, která zdědí všechny její podřízené položky. Proto když `BindingContext` na [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) je nastaven na `Employee` instance, všechny podřízené objekty daného `ContentPage` mít stejný `BindingContext`a můžete vázat na veřejné vlastnosti `Employee`objektu.

### <a name="c35"></a>C&#35;

Následující kód ukazuje příklad provedení vazby dat v jazyce C#:

```csharp
public EmployeeDetailPage(Employee employeeToDisplay)
{
    this.BindingContext = employeeToDisplay;
    var firstName = new Entry()
    {
        HorizontalOptions = LayoutOptions.FillAndExpand
    };
    firstName.SetBinding(Entry.TextProperty, "FirstName");
    ...
}
```

[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Konstruktor instanci předávána `Employee` objekt a nastaví [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) k objektu pro vazbu. [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Vytvořit instanci ovládacího prvku a vazbu mezi [ `Entry.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) vlastnost a `FirstName` vlastnost *zdroj* objektu je nastavena. Změny provedené v `Entry` řízení se automaticky rozšíří do `employeeToDisplay` objektu. Podobně pokud jsou provedeny změny do `employeeToDisplay.FirstName` vlastnost, modul vazby Xamarin.Forms se také aktualizovat obsah `Entry` ovládacího prvku. To se označuje jako *vazba obousměrná*. Aby obousměrné vazby pro práci, musí implementovat třídu modelu `INotifyPropertyChanged` rozhraní.

`SetBinding` Metoda přebírá dva parametry. První parametr určuje informace o typu vazby. Druhý parametr slouží k zadání informací o co k vytvoření vazby nebo postupy pro vazbu. Druhý parametr je ve většině případů právě řetězec obsahující název vlastnosti [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Následující syntaxí slouží k vytvoření vazby `BindingContext` přímo:

```csharp
someLabel.SetBinding(Label.TextProperty, new Binding("."));
```

Tečkové syntaxe informuje Xamarin.Forms používat [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) jako zdroj dat místo vlastnost `BindingContext`. To je užitečné, když `BindingContext` , jako je jednoduchý typ, `string` nebo `int`.

<a name="INotifyPropertyChanged" />

### <a name="property-change-notification"></a>Oznámení o změně vlastností

Ve výchozím nastavení *cíl* objekt přijímá pouze hodnotu *zdroj* objektu při vytváření vazby. Zachovat rozhraní synchronizované se zdrojem dat, musí být způsob, jak upozornit *cíl* objekt při *zdroj* objektu se změnila. Tento mechanismus zajišťuje `INotifyPropertyChanged` rozhraní. Implementace tohoto rozhraní bude poskytovat oznámení pro všechny ovládací prvky vázané na data, když základní hodnota vlastnosti.

Objekty, které implementují `INotifyPropertyChanged` musíte zvýšit `PropertyChanged` událost, pokud jeden z jejich vlastnosti se aktualizuje s novou hodnotou, jak je ukázáno v následujícím příkladu kódu:

```csharp
public class MyObject : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    string _firstName;
    public string FirstName
    {
        get { return _firstName; }
        set
        {
            if (value.Equals(_firstName, StringComparison.Ordinal))
            {
                // Nothing to do - the value hasn't changed;
                return;
            }
            _firstName = value;
            OnPropertyChanged();

        }
    }

    void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        var handler = PropertyChanged;
        if (handler != null)
        {
            handler(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Když `MyObject.FirstName` změny vlastností `OnPropertyChanged` je volána metoda, která bude vyvolána `PropertyChanged` událostí. Aby se zabránilo nepotřebné události, která iniciovala, `PropertyChanged` není vyvolána událost, pokud se hodnota vlastnosti nemění.

Všimněte si, že v `OnPropertyChanged` metoda `propertyName` parametr je ozdobené s `CallerMemberName` atribut. Zajistíte tak, že pokud `OnPropertyChanged` metoda je volána s `null` hodnota, `CallerMemberName` atribut bude zadejte název metody, která volána `OnPropertyChanged`.

<a name="Navigation" />

## <a name="navigation"></a>Navigace

Poskytuje řadu prostředí navigační různé stránky, v závislosti na platformě Xamarin.Forms [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) zadejte používá. Pro [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) existuje instance jsou dvě navigační prostředí:

- [Hierarchická navigace](#Hierarchical_Navigation)
- [Modální navigace](#Modal_Navigation)

[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) a [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) třídy poskytují alternativní navigační prostředí. Další informace najdete v tématu [navigační](~/xamarin-forms/app-fundamentals/navigation/index.md).

<a name="Hierarchical_Navigation" />

### <a name="hierarchical-navigation"></a>Hierarchická navigace

[ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Třída poskytuje hierarchické navigační prostředí, kde je možné procházet stránky, dopředný a podle potřeby zpětné uživatele. Třída implementuje navigační jako zásobník ven (LIFO) last-in služby [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objekty.

V hierarchické navigaci [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) třída se používá k procházení zásobníku z [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) objekty. Pokud chcete přesunout z jedné stránky na jiný, aplikace předá novou stránku do zásobníku navigace, kde se stane aktivní stránky. Pokud chcete vrátit zpět na předchozí stránku, aplikace bude pop aktuální stránku z navigační zásobníku a nová stránka nejhornější stane aktivní stránku.

Na první stránku přidat navigační zásobníku se označuje jako *kořenové* stránku aplikace a následující příklad kódu ukazuje, jak toho dosahuje:

```csharp
public App ()
{
    MainPage = new NavigationPage(new EmployeeListPage());
}
```

Přejděte na `LoginPage`, je nutné vyvolat [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) metodu [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) vlastnost aktuální stránky, jako předvedenou v následujícím příkladu kódu:

```csharp
await Navigation.PushAsync(new LoginPage());
```

To způsobí, že nové `LoginPage` objekt, který chcete poslat v zásobníku navigace, kde bude aktivní stránky.

Aktivní stránku může být odebrány ze zásobníku navigační stisknutím kombinace kláves *zpět* tlačítko na zařízení, bez ohledu na to, zda je na zařízení fyzické tlačítko nebo na obrazovce tlačítko. Prostřednictvím kódu programu vrátit na předchozí stránku, `LoginPage` musí vyvolání instance [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) metoda, jak je ukázáno v následujícím příkladu kódu:

```csharp
await Navigation.PopAsync();
```

Další informace o hierarchické navigační najdete v tématu [hierarchické navigační](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Modal_Navigation" />

### <a name="modal-navigation"></a>Modální navigace

Xamarin.Forms poskytuje podporu pro modální stránky. Modální stránky doporučuje uživatelům k dokončení samostatná úloha, která nemůže být opuštění dokud je úloha dokončena nebo zrušena.

Modální stránky může být libovolná z [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) typy podporované systémem Xamarin.Forms. K zobrazení modální stránky aplikace předá ho do zásobníku navigace, kde se stane aktivní stránky. Vrátit na předchozí stránku budou aplikace pop aktuální stránku z navigační zásobníku a nová stránka nejhornější stane aktivní stránky.

Modální navigační metody jsou vystavené [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) vlastnost na žádném [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) odvozených typů. [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Vlastnost taky zpřístupňuje [ `ModalStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) ze kterého lze získat modální stránky v zásobníku navigační vlastnost. Však neexistuje žádná koncepce provádění modální zásobníku manipulaci nebo odebrání kořenovou stránku v modální navigaci. Je to proto, že tyto operace nepodporuje všeobecně základní platformy.

> [!NOTE]
> A [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) instance není vyžadován pro provádění navigace modální stránky.

Modálně přejděte na `LoginPage` je nutné vyvolat [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) metodu [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) vlastnost aktuální stránky, jako v následujícím příkladu kódu předvedenou :

```csharp
await Navigation.PushModalAsync(new LoginPage());
```

To způsobí, že `LoginPage` instance, která má být vloženy do zásobníku navigace, kde bude aktivní stránku.

Aktivní stránku může být odebrány ze zásobníku navigační stisknutím kombinace kláves *zpět* tlačítko na zařízení, bez ohledu na to, zda je na zařízení fyzické tlačítko nebo na obrazovce tlačítko. Prostřednictvím kódu programu vrátit na stránku původní `LoginPage` musí vyvolání instance [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) metoda, jak je ukázáno v následujícím příkladu kódu:

```csharp
await Navigation.PopModalAsync();
```

To způsobí, že `LoginPage` instance, která má být odebrán, navigace, s novou nejhornější stránku stane aktivní stránku.

Další informace o modální navigační najdete v tématu [modální stránky](~/xamarin-forms/app-fundamentals/navigation/modal.md).

<a name="Next_Steps" />

## <a name="next-steps"></a>Další kroky

Tento úvodní článek by měl umožňují Zahájit zápis Xamarin.Forms aplikace. Navrhované další kroky obsahovat výklad o následující funkce:

- Ovládací prvek šablony poskytují možnost snadno motiv a opětovná motivu stránek aplikací za běhu. Další informace najdete v tématu [šablon ovládacích](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).
- Data šablony poskytují možnost definovat prezentaci dat na podporované ovládací prvky. Další informace najdete v tématu [dat šablony](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).
- Sdílené kódu můžete získat přístup k nativní funkce prostřednictvím [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) třídy. Další informace najdete v tématu [přístup k nativní funkce s DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).
- Xamarin.Forms obsahuje jednoduché službou zasílání zpráv odesílat a přijímat zprávy, snížíte párování mezi třídami. Další informace najdete v tématu [publikování a přihlásit k odběru s MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).
- Každé stránky, rozložení a řízení vykreslením jinak na každé platformě pomocí `Renderer` třídu, která zase vytvoří ovládacího prvku nativní uspořádá na obrazovce a přidá zadané v sdíleného kódu chování. Vývojářům můžete implementovat vlastní vlastní `Renderer` třídy k přizpůsobení vzhledu a chování ovládacího prvku. Další informace najdete v tématu [nástroji pro vykreslování vlastní](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).
- Účinky také povolit nativní ovládací prvky na každou platformu, která lze přizpůsobit. Účinky jsou vytvořené v projektech specifické pro platformu vytváření podtříd [ `PlatformEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/) řízení a používají se podle jejich připojením k příslušné prvek Xamarin.Forms. Další informace najdete v tématu [důsledky](~/xamarin-forms/app-fundamentals/effects/index.md).

Vytváření mobilních aplikací s Xamarin.Forms, seznamu pomocí Charlese Petzold, případně je místo pro další informace o Xamarin.Forms. Další informace najdete v tématu [vytváření mobilních aplikací s Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="summary"></a>Souhrn

Tento článek poskytuje úvod do Xamarin.Forms a jak začít pracovat psaní aplikací s ním. Xamarin.Forms je že napříč platformami nativně podporovaný abstrakce nástrojů uživatelského rozhraní, která umožňuje vývojářům snadno vytvářet uživatelského rozhraní, které můžete sdílet mezi Android, iOS, Windows a Windows Phone. Uživatelská rozhraní jsou vykreslovány pomocí nativní ovládací prvky typu cílovou platformu, povolíte Xamarin.Forms aplikací zachovat odpovídající vzhledu a chování pro každou platformu.


## <a name="related-links"></a>Související odkazy

- [XAML – základy](~/xamarin-forms/xaml/xaml-basics/index.md)
- [Referenční informace o ovládacích prvcích](~/xamarin-forms/user-interface/controls/index.md)
- [Uživatelské rozhraní](~/xamarin-forms/user-interface/index.md)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Začínáme ukázky](https://developer.xamarin.com/samples/xamarin-forms/GettingStarted/)
- [Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
- [Volné Self-Guided Learning (video)](https://university.xamarin.com/self-guided)
- [Hello, Xamarin.Forms iOS sešitu](https://developer.xamarin.com/workbooks/xamarin-forms/getting-started/GettingStartedWithXamarinForms-ios.workbook)
