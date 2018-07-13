---
title: Úvod do Xamarin.Forms
description: Tento článek obsahuje úvod do Xamarin.Forms a jak začít vytváření aplikací s ním.
ms.prod: xamarin
ms.assetid: f619595f-3ee7-439b-a1bc-d13e5106e6e9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 95b0744cdd52ac1c3f5d7c62c18139a30400ab04
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38999028"
---
# <a name="an-introduction-to-xamarinforms"></a>Úvod do Xamarin.Forms

_Xamarin.Forms je že platformově univerzální nativně podporovaný abstrakce sada nástrojů uživatelského rozhraní, která umožňuje vývojářům snadno vytvářet uživatelské rozhraní, které mohou být sdíleny napříč Android, iOS, Windows a univerzální platformu Windows. Uživatelská rozhraní jsou vykreslovány pomocí nativní ovládací prvky cílové platformy, povolení aplikace Xamarin.Forms pro zachování odpovídající vzhled a chování pro každou platformu. Tento článek obsahuje úvod do Xamarin.Forms a jak začít vytváření aplikací s ním._

<a name="Overview" />

## <a name="overview"></a>Přehled

Xamarin.Forms je architektura, která umožňuje vývojářům rychle vytvářet multiplatformní uživatelské rozhraní. Poskytuje vlastní abstrakci pro uživatelské rozhraní, které bude vykreslen pomocí nativní ovládací prvky v iOS, Android nebo univerzální platformu Windows (UPW). To znamená, že aplikace může sdílet velkou část kódu jejich uživatelské rozhraní a zachovat přitom přirozený vzhled a chování cílové platformy.

Xamarin.Forms umožňuje rychlé vytváření prototypů aplikací, které můžete průběžně vyvíjet komplexních aplikací. Vzhledem k tomu aplikace Xamarin.Forms nativních aplikací, nemají omezení další sady nástrojů, jako je například prohlížeč izolace (sandbox), rozhraní API omezený nebo nízký výkon. Jsou aplikace napsané v Xamarin.Forms moct využít některý z rozhraní API nebo funkce podkladovou platformu, například (ale nikoli výhradně) CoreMotion PassKit a StoreKit v Iosu; NFC a Google Play Services na Android a dlaždic na Windows. Kromě toho je možné vytvořit aplikace, které se mají části jejich uživatelské rozhraní vytvořené pomocí Xamarin.Forms, zatímco jiné části se vytvoří s použitím nativní sada nástrojů uživatelského rozhraní.

Aplikace Xamarin.Forms dokáže stejným způsobem jako tradiční aplikace pro různé platformy. Nejběžnější přístupem je použití [přenosné knihovny](~/cross-platform/app-fundamentals/pcl.md) nebo [sdílené projekty](~/cross-platform/app-fundamentals/shared-projects.md) organizace sdílený kód a vytvářejte aplikace pro konkrétní platformy, které budou využívat sdílený kód.

Existují dva postupy pro vytváření uživatelských rozhraní v Xamarin.Forms. První způsob je zcela se zdrojovým kódem jazyka C# vytvářet uživatelské rozhraní. Druhý postup je použití *Extensible Application Markup Language* (XAML), deklarativní značkovací jazyk, který se používá k popisu uživatelského rozhraní. Další informace o XAML najdete v tématu [XAML Základy](~/xamarin-forms/xaml/xaml-basics/index.md).

Tento článek popisuje základní informace o rozhraní Xamarin.Forms a obsahuje následující témata:

-  [Zkoumání aplikace Xamarin.Forms](#Examining_A_Xamarin.Forms_Application).
-  [Jak se používají Xamarin.Forms stránek a ovládacích prvků](#Views_and_Layouts).
-  [Jak používat zobrazení seznamu dat](#Lists_in_Xamarin.Forms).
-  [Jak vytvořit datovou vazbu](#Data_Binding).
-  [Jak pro navigaci mezi stránkami](#Navigation).
-  [Další kroky](#Next_Steps).

<a name="Examining_A_Xamarin_Forms_Application" />

### <a name="examining-a-xamarinforms-application"></a>Zkoumání aplikace Xamarin.Forms

V sadě Visual Studio pro Mac a Visual Studio vytvoří výchozí šablonu Xamarin.Forms app nejjednodušší řešení Xamarin.Forms je to možné, který zobrazí text pro uživatele. Pokud aplikaci spouštíte, by měl vypadat podobně jako na následujících snímcích obrazovky:

[![](introduction-to-xamarin-forms-images/image05-sml.png "Výchozí aplikace Xamarin.Forms")](introduction-to-xamarin-forms-images/image05.png#lightbox "výchozí aplikace Xamarin.Forms")

Odpovídá každou obrazovku na snímcích obrazovky *stránky* v Xamarin.Forms. A [ `Page` ](xref:Xamarin.Forms.Page) představuje *aktivity* v Androidu, *kontroler zobrazení* v Iosu, nebo *stránky* v Windows Universal Platformy (UPW). Vytvoří instanci ukázka výše na snímcích obrazovky [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) objektu a, který používá k zobrazení [ `Label` ](xref:Xamarin.Forms.Label).

Pokud chcete maximalizovat opětovné použití kódu po spuštění, aplikace Xamarin.Forms mají jednu třídu s názvem `App` , který je zodpovědný za vytváření instancí první [ `Page` ](xref:Xamarin.Forms.Page) , který se zobrazí. Příkladem `App` třídy můžete vidět v následujícím kódu:

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

Tento kód vytvoří novou [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) objekt, který se zobrazí jeden [ `Label` ](xref:Xamarin.Forms.Label) obě vodorovně a svisle na střed stránky.

<a name="Launching_the_Initial_Xamarin_Forms_Page_on_Each_Platform" />

### <a name="launching-the-initial-xamarinforms-page-on-each-platform"></a>Spouštění úvodní stránku Xamarin.Forms na jednotlivých platformách

Využít [ `Page` ](xref:Xamarin.Forms.Page) uvnitř aplikace, musí každá aplikace platformy inicializovat rozhraní Xamarin.Forms a poskytovat instance [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) jako je spouštěna. Tento krok inicializace se liší od platformách a je popsána v následujících částech.

<a name="Launching_in_iOS" />

#### <a name="ios"></a>iOS

Ke spuštění úvodní stránku Xamarin.Forms v Iosu, zahrnuje platformu projektu `AppDelegate` třídu odvozenou od `Xamarin.Forms.Platform.iOS.FormsApplicationDelegate` třídy, jak je ukázáno v následujícím příkladu kódu:

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

`FinishedLoading` Přepsání inicializuje rozhraní Xamarin.Forms pomocí volání `Init` metody. To způsobí, že implementace Xamarin.Forms, které mají být načteny v aplikaci předtím, než je ve volání nastavení kontroleru zobrazení kořenové iOS `LoadApplication` metody.

<a name="Launching_in_Android" />

#### <a name="android"></a>Android

Ke spuštění úvodní stránku Xamarin.Forms v Androidu, platforma projektu obsahuje kód, který vytvoří `Activity` s `MainLauncher` atributem inherting aktivitu z `FormsAppCompatActivity` třídy, jak je ukázáno v následujícím příkladu kódu:

```csharp
namespace HelloXamarinFormsWorld.Android
{
    [Activity(Label = "HelloXamarinFormsWorld", Theme = "@style/MainTheme", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : FormsAppCompatActivity
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

`OnCreate` Přepsání inicializuje rozhraní Xamarin.Forms pomocí volání `Init` metody. To způsobí, že implementace Xamarin.Forms, které mají být načteny v aplikaci před načtením aplikace Xamarin.Forms s Androidem.

#### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

V aplikacích pro univerzální platformu Windows (UPW) `Init` vyvolat metodu, která inicializuje rozhraní Xamarin.Forms pomocí `App` třídy:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

To způsobí, že implementace Xamarin.Forms, které mají být načteny v aplikaci UWP. Úvodní stránka Xamarin.Forms je spouštěn `MainPage` třídy, jak je ukázáno v následujícím příkladu kódu:

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

Načte se aplikace Xamarin.Forms s `LoadApplication` metody.

<a name="Views_and_Layouts" />

### <a name="views-and-layouts"></a>Zobrazení a rozložení

Existují čtyři hlavní ovládací prvek skupiny použité k vytvoření uživatelského rozhraní aplikace Xamarin.Forms.

1. **Stránky** – stránky Xamarin.Forms představují obrazovky multiplatformní mobilní aplikace. Další informace o stránkách naleznete v tématu [Xamarin.Forms stránky](~/xamarin-forms/user-interface/controls/pages.md).
1. **Rozložení** – rozložení Xamarin.Forms jsou kontejnery, které umožňuje sestavit zobrazení do logické struktury. Další informace o rozložení najdete v tématu [rozložení Xamarin.Forms](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Zobrazení** – zobrazení Xamarin.Forms jsou ovládací prvky zobrazí v uživatelském rozhraní, jako je například popisky, tlačítka a textová vstupní pole. Další informace o zobrazeních najdete v tématu [zobrazení Xamarin.Forms](~/xamarin-forms/user-interface/controls/views.md).
1. **Buňky** – Xamarin.Forms buňky jsou specializované prvky použitá pro položky v seznamu a popisují, jak má být vykreslena každou položku v seznamu. Další informace o buňky, naleznete v tématu [Xamarin.Forms buňky](~/xamarin-forms/user-interface/controls/cells.md).

Za běhu každý ovládací prvek se namapují na ekvivalentní nativní, což je, co bude vykreslen.

Ovládací prvky jsou hostována v rozložení. [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Třídy, která implementuje běžně používaných rozložení, bude nyní zkoumají.

<a name="StackLayout" />

#### <a name="stacklayout"></a>StackLayout

[ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Zjednodušuje vývoj multiplatformních aplikací tím, že automaticky uspořádání ovládacích prvků na obrazovce bez ohledu na velikost obrazovky. Každý podřízený prvek je umístěný za sebou, buď vodorovně nebo svisle v pořadí, v jakém byly přidány. Kolik místa `StackLayout` bude použití závisí na tom, [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) a [ `VerticalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) jsou nastaveny vlastnosti, ale ve výchozím nastavení `StackLayout` se pokusí použít celou obrazovku.

Následující kód XAML ukazuje příklad použití [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) uspořádat tři [ `Label` ](xref:Xamarin.Forms.Label) ovládacích prvků:

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

Ekvivalentní kód jazyka C# můžete vidět v následujícím příkladu kódu:

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

Ve výchozím nastavení [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) předpokládá svislou orientaci, jak je znázorněno na následujících snímcích obrazovky:

[![](introduction-to-xamarin-forms-images/image09-sml.png "Svislé StackLayout")](introduction-to-xamarin-forms-images/image09.png#lightbox "svislé StackLayout")

Orientace [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) lze změnit na vodorovnou orientaci, jak je ukázáno v následujícím příkladu kódu XAML:

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

Ekvivalentní kód jazyka C# můžete vidět v následujícím příkladu kódu:

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

[![](introduction-to-xamarin-forms-images/image10-sml.png "Vodorovné StackLayout")](introduction-to-xamarin-forms-images/image10.png#lightbox "vodorovné StackLayout")

Velikosti ovládacích prvků lze nastavit až `HeightRequest` a `WidthRequest` vlastnosti, jak je ukázáno v následujícím příkladu kódu XAML:

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

Ekvivalentní kód jazyka C# můžete vidět v následujícím příkladu kódu:

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

[![](introduction-to-xamarin-forms-images/image11-sml.png "Vodorovné StackLayout s LayoutOptions")](introduction-to-xamarin-forms-images/image11.png#lightbox "vodorovné StackLayout s LayoutOptions")

Další informace o [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) najdete v tématu [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

<a name="Lists_in_Xamarin_Forms" />

## <a name="lists-in-xamarinforms"></a>Seznamy v Xamarin.Forms

[ `ListView` ](xref:Xamarin.Forms.ListView) Zodpovídá ovládací prvek pro zobrazení na obrazovce – každá položka v kolekci položek `ListView` bude obsahovat jednu buňku. Ve výchozím nastavení `ListView` bude používat integrovanou [ `TextCell` ](xref:Xamarin.Forms.TextCell) šablony a vykreslování jeden řádek textu.

Následující příklad kódu ukazuje jednoduchý [ `ListView` ](xref:Xamarin.Forms.ListView) příkladu:

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

Následující snímek obrazovky ukazuje výsledný [ `ListView` ](xref:Xamarin.Forms.ListView):

 ![](introduction-to-xamarin-forms-images/image13.png "ListView")

Další informace o [ `ListView` ](xref:Xamarin.Forms.ListView) řídí, najdete v článku [ListView](~/xamarin-forms/user-interface/listview/index.md).

<a name="Binding_to_a_Custom_Class" />

### <a name="binding-to-a-custom-class"></a>Vytvoření vazby na vlastní třídy

[ `ListView` ](xref:Xamarin.Forms.ListView) Ovládací prvek mohl zobrazit také vlastní objekty pomocí výchozího [ `TextCell` ](xref:Xamarin.Forms.TextCell) šablony.

Následující příklad kódu ukazuje `TodoItem` třídy:

```csharp
public class TodoItem
{
    public string Name { get; set; }
    public bool Done { get; set; }
}
```

[ `ListView` ](xref:Xamarin.Forms.ListView) Ovládacího prvku je možné importovat, jak je ukázáno v následujícím příkladu kódu:

```csharp
listView.ItemsSource = new TodoItem [] {
    new TodoItem { Name = "Buy pears" },
    new TodoItem { Name = "Buy oranges", Done=true} ,
    new TodoItem { Name = "Buy mangos" },
    new TodoItem { Name = "Buy apples", Done=true },
    new TodoItem { Name = "Buy bananas", Done=true }
};
```

Nastavení, které je možné vytvořit vazbu `TodoItem` vlastnost se zobrazí [ `ListView` ](xref:Xamarin.Forms.ListView), jak je ukázáno v následujícím příkladu kódu:

```csharp
listView.ItemTemplate = new DataTemplate(typeof(TextCell));
listView.ItemTemplate.SetBinding(TextCell.TextProperty, "Name");
```

Tím se vytvoří vazbu, která určuje cestu k `TodoItem.Name` vlastnost a bude mít za následek na předchozím prohlíženém snímku obrazovky.

Další informace o vazbách na vlastní třídu naleznete v tématu [zdroje dat ListView](~/xamarin-forms/user-interface/listview/data-and-databinding.md).

<a name="Selecting_an_Item_in_a_ListView" />

### <a name="selecting-an-item-in-a-listview"></a>Výběr položky v ListView

Reakce na uživatelského zásahu do buňky v [ `ListView` ](xref:Xamarin.Forms.ListView), [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) události by měly být zpracovány, jak je ukázáno v následujícím příkladu kódu:

```csharp
listView.ItemSelected += async (sender, e) => {
    await DisplayAlert("Tapped!", e.SelectedItem + " was tapped.", "OK");
};
```

Pokud je obsažen v [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) metodu lze použít k otevření novou stránku s integrovanou navigace zpět. [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Událostí můžete přístup k objektu, který byl přidružen buňky prostřednictvím [ `e.SelectedItem` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) vlastnost vázat na novou stránku a zobrazí nová stránka pomocí `PushAsync`, jako ukázáno v následujícím příkladu kódu:

```csharp
listView.ItemSelected += async (sender, e) => {
    var todoItem = (TodoItem)e.SelectedItem;
    var todoPage = new TodoItemPage(todoItem); // so the new page shows correct data
    await Navigation.PushAsync(todoPage);
};
```

Jednotlivé platformy, implementuje integrované navigace zpět vlastním způsobem. Další informace najdete v tématu [navigace](#Navigation).

Další informace o [ `ListView` ](xref:Xamarin.Forms.ListView) výběr, naleznete v tématu [ListView interaktivitu](~/xamarin-forms/user-interface/listview/interactivity.md).

<a name="Customizing_the_appearance_of_a_cell" />

### <a name="customizing-the-appearance-of-a-cell"></a>Přizpůsobení vzhledu buňky

Vytváření podtříd může přizpůsobit vzhled buňky [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) třídy a typ této třídy pro nastavení [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) vlastnost [ `ListView` ](xref:Xamarin.Forms.ListView).

Buňka je znázorněno na následujícím snímku obrazovky se skládá z jedné [ `Image` ](xref:Xamarin.Forms.Image) a dva [ `Label` ](xref:Xamarin.Forms.Label) ovládacích prvků:

 ![](introduction-to-xamarin-forms-images/image14.png "Vzhled buňky vlastní ListView")

Chcete-li vytvořit tento vlastní rozložení [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) třída by měla být rozčlenit do podtříd, jak je ukázáno v následujícím příkladu kódu:

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

-  Přidá [ `Image` ](xref:Xamarin.Forms.Image) řídit a připojí ho k `ImageUri` vlastnost `Employee` objektu. Další informace o datové vazbě naleznete v tématu [datové vazby](#Data_Binding).
-  Vytvoří [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) při svislé orientaci k uložení obou [ `Label` ](xref:Xamarin.Forms.Label) ovládacích prvků. `Label` Na vazby ovládacích prvků `DisplayName` a `Twitter` vlastnosti `Employee` objektu.
-  Vytvoří [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) , který bude hostitelem existující [ `Image` ](xref:Xamarin.Forms.Image) a `StackLayout`. Uspořádá podřízené pomocí vodorovné orientaci.

Po vytvoření vlastních buňky mohou být využívány službou [ `ListView` ](xref:Xamarin.Forms.ListView) závěrečné v pod kontrolou [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), jak je ukázáno v následujícím příkladu kódu:

```csharp
List<Employee> myListOfEmployeeObjects = GetAListOfAllEmployees();
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = myListOfEmployeeObjects;
listView.ItemTemplate = new DataTemplate(typeof(EmployeeCell));
```

Tento kód bude poskytovat `List` z `Employee` k [ `ListView` ](xref:Xamarin.Forms.ListView). Každá buňka bude vykreslen pomocí `EmployeeCell` třídy. `ListView` Předá `Employee` objektu `EmployeeCell` jako jeho [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext).

Další informace o přizpůsobení vzhledu buněk v tématu [vzhled buňky](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="Using_XAML_to_Create_and_Customize_A_List" />

### <a name="using-xaml-to-create-and-customize-a-list"></a>Vytvoření a přizpůsobení seznamu pomocí XAML

Ekvivalent XAML [ `ListView` ](xref:Xamarin.Forms.ListView) v předchozí části je znázorněn v následujícím příkladu kódu:

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

Definuje tento XAML [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) , která obsahuje [ `ListView` ](xref:Xamarin.Forms.ListView). Zdroj dat `ListView` nastavené přes [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) atribut. Rozložení jednotlivých řádků `ItemsSource` je definována v rámci [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) elementu.

<a name="Data_Binding" />

## <a name="data-binding"></a>Datová vazba

Vytváření datových vazeb připojí dva objekty, volá se *zdroj* a *cílové*. *Zdroj* objekt, který poskytuje data. *Cílové* objektu se spotřebuje (a často zobrazení) data ze zdrojového objektu. Například [ `Label` ](xref:Xamarin.Forms.Label) (*cílové* objekt) běžně vytvoří vazbu jeho [ `Text` ](xref:Xamarin.Forms.Label.Text) veřejnou vlastnost `string` vlastnost *zdroj* objektu. Následující diagram znázorňuje vztah vazby:

![](introduction-to-xamarin-forms-images/data-binding.png "Vytváření datových vazeb")

Hlavní výhodou datové vazby je, že už nemusíte dělat starosti o synchronizaci dat mezi zobrazeními a zdroj dat. Se změnami *zdroj* objektu jsou automaticky vloženy do *cílové* objektu pozadí rozhraním, vazby a změny v cílovém objektu můžete volitelně doručit bez vyžádání zpět do *zdroj* objektu.

Vytvoření datové vazby je dvou krocích:

- [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Vlastnost *cílové* objekt musí být nastaveno na *zdroj*.
- Vazby musí navázat mezi *cílové* a *zdroj*. V XAML, tím se dosahuje prostřednictvím využití [ `Binding` ](xref:Xamarin.Forms.Xaml.BindingExtension) – rozšíření značek. V jazyce C#, to se provádí [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) metody.

Další informace o datové vazbě naleznete v tématu [základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

### <a name="xaml"></a>XAML

Následující kód ukazuje příklad provádění datové vazby v XAML:

```xaml
<Entry Text="{Binding FirstName}" ... />
```

Vytvoření vazby mezi [ `Entry.Text` ](xref:Xamarin.Forms.Entry.Text) vlastnost a `FirstName` vlastnost *zdroj* pokládáme stav objektu. Změny provedené v `Entry` ovládací prvek automaticky se rozšíří do `employeeToDisplay` objektu. Podobně pokud jsou provedeny změny do `employeeToDisplay.FirstName` vlastnost, modul vazby Xamarin.Forms se aktualizuje i obsah `Entry` ovládacího prvku. Jedná se *Obousměrná vazba*. V pořadí pro obousměrné vazby pro práci, musí implementovat třídu modelu `INotifyPropertyChanged` rozhraní.

I když [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) vlastnost `EmployeeDetailPage` třídy je možné nastavit v XAML, zde je nastavena v kódu na pozadí do instance `Employee` objektu:

```csharp
public EmployeeDetailPage(Employee employee)
{
    InitializeComponent();
    this.BindingContext = employee;
}
```

Zatímco [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) vlastnosti každého *cílové* objekt může mít individuálně nastavená, to není nutné. `BindingContext` je speciální vlastnost, která zdědí všechny podřízené. Proto, když `BindingContext` na [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) je nastavena na `Employee` instance, všechny podřízené objekty daného `ContentPage` mají stejné `BindingContext`a mohl vytvořit vazbu k veřejné vlastnosti `Employee`objektu.

### <a name="c35"></a>C&#35;

Následující kód ukazuje příklad provedení datové vazby v jazyce C#:

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

[ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Konstruktoru je předána instance `Employee` objekt a nastaví [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) na objekt k vytvoření vazby. [ `Entry` ](xref:Xamarin.Forms.Entry) Vytvořit instanci ovládacího prvku a vazby mezi [ `Entry.Text` ](xref:Xamarin.Forms.Entry.Text) vlastnost a `FirstName` vlastnost *zdroj* objektu je nastavena. Změny provedené v `Entry` ovládací prvek automaticky se rozšíří do `employeeToDisplay` objektu. Podobně pokud jsou provedeny změny do `employeeToDisplay.FirstName` vlastnost, modul vazby Xamarin.Forms se aktualizuje i obsah `Entry` ovládacího prvku. Jedná se *Obousměrná vazba*. V pořadí pro obousměrné vazby pro práci, musí implementovat třídu modelu `INotifyPropertyChanged` rozhraní.

`SetBinding` Metoda přebírá dva parametry. První parametr určuje informace o typu vazby. Druhý parametr slouží k zadání informací o co se má svázat nebo jak vytvořit vazbu. Druhý parametr je ve většině případů stačí řetězec uchovávající název vlastnosti [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext). Následující syntaxe se používá k vytvoření vazby `BindingContext` přímo:

```csharp
someLabel.SetBinding(Label.TextProperty, new Binding("."));
```

Říká Xamarin.Forms pomocí tečkové syntaxe [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) jako zdroj dat místo vlastnosti `BindingContext`. To je užitečné, když `BindingContext` , jako je jednoduchý typ, `string` nebo `int`.

<a name="INotifyPropertyChanged" />

### <a name="property-change-notification"></a>Oznámení změn vlastností

Ve výchozím nastavení *cílové* pouze přijímá hodnotu z objektu *zdroj* objektu, když se vytvoří vazba. Udržovat synchronizované se zdrojem dat uživatelského rozhraní, musí existovat způsob, jak upozornit *cílové* objektu při *zdroj* došlo ke změně objektu. Tento mechanismus je poskytován `INotifyPropertyChanged` rozhraní. Implementace tohoto rozhraní bude poskytovat oznámení na žádné ovládací prvky vázané na data, když se změní podkladové hodnoty vlastnosti.

Objekty, které implementují `INotifyPropertyChanged` musíte zvýšit `PropertyChanged` událost v případě, že jeden z jejich vlastností se aktualizuje s novou hodnotou, jak je ukázáno v následujícím příkladu kódu:

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

Když `MyObject.FirstName` změny vlastností `OnPropertyChanged` je vyvolána metoda, která vyvolá `PropertyChanged` událostí. Aby se zabránilo nepotřebné události ohlásí, `PropertyChanged` událost se vyvolá, pokud nedojde ke změně hodnoty vlastnosti.

Všimněte si, že v `OnPropertyChanged` metoda `propertyName` parametr je opatřený s `CallerMemberName` atribut. Tím se zajistí, že pokud `OnPropertyChanged` metoda je volána s `null` hodnota, `CallerMemberName` atribut poskytne název metody, která vyvolala `OnPropertyChanged`.

<a name="Navigation" />

## <a name="navigation"></a>Navigace

Xamarin.Forms nabízí celou řadu jiných stránek navigační prostředí, v závislosti na [ `Page` ](xref:Xamarin.Forms.Page) zadejte používá. Pro [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) existuje instance jsou dvě navigační prostředí:

- [Hierarchická navigace](#Hierarchical_Navigation)
- [Modální navigace](#Modal_Navigation)

[ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) a [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) třídy poskytují alternativní navigační prostředí. Další informace najdete v tématu [navigace](~/xamarin-forms/app-fundamentals/navigation/index.md).

<a name="Hierarchical_Navigation" />

### <a name="hierarchical-navigation"></a>Hierarchická navigace

[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Třída poskytuje hierarchické navigační prostředí, kde uživatel je možné procházet stránkách vpřed a zpět, podle potřeby. Třída implementuje navigace jako poslední dovnitř, první (ven LIFO) zásobníku [ `Page` ](xref:Xamarin.Forms.Page) objekty.

Hierarchická navigace [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) třída se používá k procházení zásobníku [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) objekty. Přesunutí z jedné stránky na jiný, aplikace budou nabízená oznámení nové stránky do navigačního zásobníku, kde se tak stane aktivní stránkou. Pokud chcete vrátit zpět na předchozí stránku, aplikaci zobrazte aktuální stránku z navigační zásobník a na novou stránku nejvyšší úrovně se stane aktivní stránkou.

První stránka Přidat do navigační zásobník se označuje jako *kořenové* stránku aplikace a následující příklad kódu ukazuje, jak to lze provést:

```csharp
public App ()
{
    MainPage = new NavigationPage(new EmployeeListPage());
}
```

Přejděte `LoginPage`, je potřeba vyvolat [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) metodu na [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) vlastnost aktuální stránky jako předvedenou v následujícím příkladu kódu:

```csharp
await Navigation.PushAsync(new LoginPage());
```

To způsobí, že nový `LoginPage` objektu doručit bez vyžádání do navigačního zásobníku, kde se stane aktivní stránkou.

Aktivní stránkou. může být odebrány z navigační zásobník stisknutím kombinace kláves *zpět* tlačítko na zařízení, bez ohledu na to, zda se jedná o fyzického tlačítka na zařízení nebo tlačítko na obrazovce. Programově vrátit na předchozí stránku, `LoginPage` instance musí vyvolat [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) způsob, jak je ukázáno v následujícím příkladu kódu:

```csharp
await Navigation.PopAsync();
```

Další informace o hierarchická navigace, naleznete v tématu [hierarchická navigace](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Modal_Navigation" />

### <a name="modal-navigation"></a>Modální navigace

Xamarin.Forms poskytuje podporu pro modální stránky. Modální stránky vyzývá uživatele k dokončení samostatná úloha, která nemůže být opuštění dokud je úloha dokončena nebo zrušena.

Modální stránky může být [ `Page` ](xref:Xamarin.Forms.Page) typů podporuje Xamarin.Forms. Zobrazíte modální stránky aplikace zařadí ho do navigačního zásobníku, kde se tak stane aktivní stránkou. Vrátit na předchozí stránku aplikace zobrazte aktuální stránku z navigační zásobník a na novou stránku nejvyšší úrovně se stane aktivní stránkou.

Modální navigační metody jsou vystavené [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) vlastnost na žádném [ `Page` ](xref:Xamarin.Forms.Page) odvozené typy. [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Také poskytuje vlastnost [ `ModalStack` ](xref:Xamarin.Forms.INavigation.ModalStack) vlastností, ze kterého můžete získat modální stránky v navigačním zásobníku. Neexistuje však žádný koncept provádění modální zásobníku manipulaci nebo automaticky otevíraného kořenovou stránku v modálním navigaci. Je to proto, že tyto operace nejsou podporovány univerzálně na základní platformy.

> [!NOTE]
> A [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instance není vyžadován pro provádění navigace modální stránky.

Modálně přejít na `LoginPage` je potřeba vyvolat [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) metodu [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) vlastnost aktuální stránky jako předvedenou v následujícím příkladu kódu :

```csharp
await Navigation.PushModalAsync(new LoginPage());
```

To způsobí, že `LoginPage` instance doručit bez vyžádání do navigačního zásobníku, kde se stane aktivní stránkou.

Aktivní stránkou. může být odebrány z navigační zásobník stisknutím kombinace kláves *zpět* tlačítko na zařízení, bez ohledu na to, zda se jedná o fyzického tlačítka na zařízení nebo tlačítko na obrazovce. Programově vrátit na původní stránku `LoginPage` instance musí vyvolat [ `PopModalAsync` ](xref:Xamarin.Forms.INavigation.PopModalAsync) způsob, jak je ukázáno v následujícím příkladu kódu:

```csharp
await Navigation.PopModalAsync();
```

To způsobí, že `LoginPage` instance má být odebrána z navigační zásobník s novou stránku nejvyšší úrovně stane aktivní stránkou.

Další informace o modální navigaci najdete v tématu [modální stránky](~/xamarin-forms/app-fundamentals/navigation/modal.md).

<a name="Next_Steps" />

## <a name="next-steps"></a>Další kroky

Tento úvodní článek by měl umožňují snadno začít psát aplikace Xamarin.Forms. Další navrhované kroky zahrnují výklad o následující funkce:

- Šablony ovládacích prvků umožňují snadno motivu a znovu motivu stránky aplikace za běhu. Další informace najdete v tématu [šablon ovládacích prvků](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).
- Datové šablony umožňují definovat prezentaci dat pro podporované ovládací prvky. Další informace najdete v tématu [datové šablony](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).
- Sdílený kód může přistupovat k nativním funkce prostřednictvím [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) třídy. Další informace najdete v tématu [přístup k nativním funkce s DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).
- Xamarin.Forms obsahuje jednoduché službou zasílání zpráv pro odesílání a příjem zpráv, snížíte párování mezi třídami. Další informace najdete v tématu [publikování a přihlášení k odběru s MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).
- Jednotlivé stránky, rozložení a ovládací prvek je vykreslen jinak na každé platformě pomocí `Renderer` třídu, která se pak vytvoří ovládací prvek nativní uspořádá na obrazovce a přidá chování určené v sdílený kód. Vývojáři můžou implementovat vlastní vlastní `Renderer` třídy k přizpůsobení vzhledu a chování ovládacího prvku. Další informace najdete v tématu [vlastními Renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).
- Účinky také povolit nativní ovládací prvky na jednotlivých platformách přizpůsobit. Efekty jsou vytvářeny v projektech pro konkrétní platformu vytváření podtříd [ `PlatformEffect` ](xref:Xamarin.Forms.PlatformEffect`2) ovládací prvek a používají se připojením na odpovídající ovládací prvek Xamarin.Forms. Další informace najdete v tématu [účinky](~/xamarin-forms/app-fundamentals/effects/index.md).

Vytváření mobilních aplikací pomocí Xamarin.Forms, kniha od Charles Petzold, případně je vhodné místo pro další informace o Xamarin.Forms. Další informace najdete v tématu [vytváření mobilních aplikací pomocí Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="summary"></a>Souhrn

Tento článek poskytuje úvod do Xamarin.Forms a jak začít vytváření aplikací s ním. Xamarin.Forms je že platformově univerzální nativně podporovaný abstrakce sada nástrojů uživatelského rozhraní, která umožňuje vývojářům snadno vytvářet uživatelské rozhraní, které mohou být sdíleny napříč Android, iOS a univerzální platformu Windows. Uživatelská rozhraní jsou vykreslovány pomocí nativní ovládací prvky cílové platformy, povolení aplikace Xamarin.Forms pro zachování odpovídající vzhled a chování pro každou platformu.


## <a name="related-links"></a>Související odkazy

- [XAML – základy](~/xamarin-forms/xaml/xaml-basics/index.md)
- [Referenční informace o ovládacích prvcích](~/xamarin-forms/user-interface/controls/index.md)
- [Uživatelské rozhraní](~/xamarin-forms/user-interface/index.md)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Úvodní ukázky služby](https://developer.xamarin.com/samples/xamarin-forms/GettingStarted/)
- [Xamarin.Forms](xref:Xamarin.Forms)
- [Bezplatné studijní přednáškám Lightning (video)](https://university.xamarin.com/self-guided)
- [Hello, Xamarin.Forms s Iosem sešitu](https://developer.xamarin.com/workbooks/xamarin-forms/getting-started/GettingStartedWithXamarinForms-ios.workbook)
