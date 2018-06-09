---
title: Stránka Xamarin.Forms seznam podrobnosti
description: Xamarin.Forms MasterDetailPage je stránka, která spravuje dvě související stránky informací – hlavní stránky, který představuje položky a podrobnosti o stránku, která zobrazí podrobné informace o položky na hlavní stránce. Tento článek vysvětluje, jak používat MasterDetailPage a přecházet mezi její stránky informace.
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 80d86e1aa6a00d4a55c0fdba1b858bfef7bcbc84
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241341"
---
# <a name="xamarinforms-master-detail-page"></a>Stránka Xamarin.Forms seznam podrobnosti

_Xamarin.Forms MasterDetailPage je stránka, která spravuje dvě související stránky informací – hlavní stránky, který představuje položky a podrobnosti o stránku, která zobrazí podrobné informace o položky na hlavní stránce. Tento článek vysvětluje, jak používat MasterDetailPage a přecházet mezi její stránky informace._

## <a name="overview"></a>Přehled

Stránky předlohy obvykle zobrazí seznam položek, jak je vidět na následujících snímcích obrazovky:

[![](master-detail-page-images/masterpage-components.png "Hlavní stránka součásti")](master-detail-page-images/masterpage-components-large.png#lightbox "hlavní stránky součásti")

Umístění seznamu položek je stejná na každou platformu a výběrem jedné z položek bude přejít na stránku odpovídající podrobnosti. Kromě toho stránky předlohy také funkce navigační panel, který obsahuje tlačítko, které lze přejít na stránku podrobností aktivní:

- V systému iOS navigačním panelu je k dispozici v horní části stránky a má tlačítko, které odkazuje na stránku podrobností. Kromě toho stránce active podrobností lze procházet k potažením na levé straně stránky předlohy.
- Navigační panel v systému Android se nachází v horní části stránky a zobrazí název, ikonu a tlačítko, která přejde na stránku podrobností. Ikona je definována v `[Activity]` atribut, který upraví `MainActivity` třídy v projektu pro specifické pro platformu Android. Kromě toho stránce active podrobností můžete přesměrováni do potažením stránky předlohy na levé straně, klepnutím na stránce podrobností na pravé straně obrazovky a klepnutím *zpět* tlačítko v dolní části obrazovky.
- Na univerzální platformu Windows (UWP), navigačním panelu je k dispozici v horní části stránky a má tlačítko, které odkazuje na stránku podrobností.

Podrobná data zobrazí stránku, která odpovídá položce vybrané na hlavní stránce a komponenty hlavní stránky podrobností se zobrazují na následujících snímcích obrazovky:

![](master-detail-page-images/detailpage-components.png "Součásti stránka podrobností")

Stránka podrobností obsahuje navigačním panelu, jejichž obsah je závislé na platformě:

- V systému iOS, navigačním panelu je k dispozici v horní části stránky zobrazí název a má tlačítko, které se vrátí k hlavní stránce, za předpokladu, že instance stránky podrobností je uzavřen do [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) instance. Kromě toho můžete stránky předlohy vrácen do potažením stránce podrobností vpravo.
- Navigační panel v systému Android se nachází v horní části stránky a zobrazí název, ikonu a tlačítko, které se vrátí k hlavní stránce. Ikona je definována v `[Activity]` atribut, který upraví `MainActivity` třídy v projektu pro specifické pro platformu Android.
- Navigačním panelu na UPW, je k dispozici v horní části stránky a zobrazí název a má tlačítko, které se vrátí k hlavní stránce.

### <a name="navigation-behavior"></a>Navigační chování

Chování prostředí navigace mezi stránkami seznamu a podrobností je platforma závislé:

- V systému iOS, stránku s podrobnostmi *snímky* vpravo jako snímky stránky předlohy vlevo a levé části podrobností stránka je stále viditelné.
- V systému Android, podrobnosti a hlavní stránky jsou *buňka* na sobě navzájem.
- Na UPW, podrobnosti a hlavní stránky jsou *prohodily*.

V režimu na šířku, zůstanou zachována podobné chování, s tím rozdílem, že hlavní stránky na iOS a Android má podobné šířka jako hlavní stránky v režimu na výšku, takže další stránky podrobností budou viditelné.

Informace o řízení navigace chování najdete v tématu [řízení chování stránky zobrazení podrobností](#Controlling_the_Detail_Page_Display_Behavior).

## <a name="creating-a-masterdetailpage"></a>Vytváření MasterDetailPage

A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) obsahuje [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) a [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) vlastnosti, které jsou obě typu [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), které se používají k získání a nastavení seznamu a podrobností stránky v uvedeném pořadí.

> [!IMPORTANT]
> A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) je navržený jako kořenové stránky a použijte ho jako podřízenou stránku v jiných typech stránky může způsobit neočekávané a nekonzistentní chování. Kromě toho se doporučuje, na hlavní stránce [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) by měla být vždy [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance, a že by měl stránce podrobností vyplněna pouze s [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), a `ContentPage` instance. To vám pomůže zajistit konzistentní uživatelské prostředí pro všechny platformy.

Následující příklad ukazuje kód XAML [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) , který nastavuje [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) a [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) vlastnosti:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  xmlns:local="clr-namespace:MasterDetailPageNavigation;assembly=MasterDetailPageNavigation"
                  x:Class="MasterDetailPageNavigation.MainPage">
    <MasterDetailPage.Master>
        <local:MasterPage x:Name="masterPage" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage>
            <x:Arguments>
                <local:ContactsPage />
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Následující příklad kódu ukazuje ekvivalent [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) vytvořené v C#:

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        masterPage = new MasterPageCS ();
        Master = masterPage;
        Detail = new NavigationPage (new ContactsPageCS ());
        ...
    }
    ...
}
```

[ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) Je nastavena na [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance. [ `MasterDetailPage.Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) Je nastavena na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) obsahující `ContentPage` instance.

### <a name="creating-the-master-page"></a>Vytvoření stránky předlohy

Následující příklad kódu XAML ukazuje deklaraci `MasterPage` objekt, který se odkazuje prostřednictvím [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) vlastnost:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             Icon="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:MasterPageItem}">
                    <local:MasterPageItem Title="Contacts" IconSource="contacts.png" TargetType="{x:Type local:ContactsPage}" />
                    <local:MasterPageItem Title="TodoList" IconSource="todo.png" TargetType="{x:Type local:TodoListPage}" />
                    <local:MasterPageItem Title="Reminders" IconSource="reminders.png" TargetType="{x:Type local:ReminderPage}" />
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid Padding="5,10">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="30"/>
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            <Image Source="{Binding IconSource}" />
                            <Label Grid.Column="1" Text="{Binding Title}" />
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Stránky se skládá z [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) který naplněný daty v jazyce XAML nastavením jeho [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) vlastnost, která má pole `MasterPageItem` instance. Každý `MasterPageItem` definuje `Title`, `IconSource`, a `TargetType` vlastnosti.

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) je přiřazena k [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) vlastnost, k zobrazení jednotlivých `MasterPageItem`. `DataTemplate` Obsahuje [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) , se skládá z [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) a [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/). [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Zobrazí `IconSource` hodnotu vlastnosti a [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) zobrazí `Title` hodnota vlastnosti pro každou `MasterPageItem`.

Na stránce jeho [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) a [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) sadu vlastností. Na ikonu se zobrazí na stránce podrobností za předpokladu, že stránka podrobností záhlaví. Toto musí být povolená na iOS pomocí zabalení Podrobnosti instance stránky v [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) instance.

> [!NOTE]
> [ `MasterDetailPage.Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) Stránka musí mít jeho [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) vlastnost nastavena, nebo dojde k výjimce.

Následující příklad kódu ukazuje vytvořené v C# ekvivalentní stránky:

```csharp
public class MasterPageCS : ContentPage
{
  public ListView ListView { get { return listView; } }

  ListView listView;

  public MasterPageCS ()
  {
    var masterPageItems = new List<MasterPageItem> ();
    masterPageItems.Add (new MasterPageItem {
      Title = "Contacts",
      IconSource = "contacts.png",
      TargetType = typeof(ContactsPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "TodoList",
      IconSource = "todo.png",
      TargetType = typeof(TodoListPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "Reminders",
      IconSource = "reminders.png",
      TargetType = typeof(ReminderPageCS)
    });

    listView = new ListView {
      ItemsSource = masterPageItems,
      ItemTemplate = new DataTemplate (() => {
        var grid = new Grid { Padding = new Thickness(5, 10) };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(30) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });

        var image = new Image();
        image.SetBinding(Image.SourceProperty, "IconSource");
        var label = new Label { VerticalOptions = LayoutOptions.FillAndExpand };
        label.SetBinding(Label.TextProperty, "Title");

        grid.Children.Add(image);
        grid.Children.Add(label, 1, 0);

        return new ViewCell { View = grid };
      }),
      SeparatorVisibility = SeparatorVisibility.None
    };

    Icon = "hamburger.png";
    Title = "Personal Organiser";
    Content = new StackLayout
    {
      Children = { listView }
    };
  }
}
```

Na následujících snímcích obrazovky zobrazit stránky předlohy na jednotlivých platformách:

![](master-detail-page-images/masterpage.png "Příklad stránky předlohy")

### <a name="creating-and-displaying-the-detail-page"></a>Vytváření a zobrazování stránce podrobností

`MasterPage` Instance obsahuje `ListView` vlastnost, která zveřejňuje jeho [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) instance tak, aby `MainPage` [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) můžete zaregistrovat instance obslužné rutiny události pro zpracování [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) událostí. Díky tomu `MainPage` instance nastavit [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) vlastnost na stránku, který představuje vybrané `ListView` položky. Následující příklad kódu ukazuje obslužné rutiny události:

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.ListView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.ListView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

`OnItemSelected` Metoda provede následující akce:

- Načítání [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) z [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) instance a zadat, že není `null`, nastaví stránce podrobností na novou instanci třídy typ stránky, které jsou uložené v `TargetType`vlastnost `MasterPageItem`. Typ stránky je uzavřen do [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) instance, abyste ověřili, že na ikonu odkazuje prostřednictvím [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) vlastnost `MasterPage` je zobrazený na stránce podrobností v iOS.
- Vybranou položku v [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) je nastaven na `null` zajistit, aby žádný z `ListView` příštím budou vybrány položky `MasterPage` se zobrazí.
- Stránka podrobností se zobrazí uživateli nastavením [ `MasterDetailPage.IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) vlastnost `false`. Tato vlastnost určuje, zda se zobrazí stránka nebo podrobností. Musí být nastavena na `true` pro zobrazení stránky předlohy a `false` k zobrazení podrobností stránky.

Tyto snímky obrazovky zobrazit `ContactPage` stránku s podrobnostmi, které se zobrazí po je vybraná na hlavní stránce:

![](master-detail-page-images/detailpage.png "Příklad stránky podrobností")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>Řízení chování stránky zobrazení podrobností

Jak [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) spravuje stránky seznamu a podrobností závisí na tom, zda je aplikace spuštěna na telefon nebo tablet, orientaci zařízení a hodnota [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) Vlastnost. Tato vlastnost určuje, jak se bude zobrazovat stránku s podrobnostmi. Možné hodnoty jsou:

- **Výchozí** – stránky se zobrazí, použijte výchozí nastavení platformy.
- **Popover** – stránka podrobností popisuje nebo částečně obsahuje stránky předlohy.
- **Rozdělení** – stránka předlohy se zobrazuje na levé straně a stránce podrobností je na pravé straně.
- **SplitOnLandscape** – rozdělené obrazovce se používá, když je zařízení v orientaci na šířku.
- **SplitOnPortrait** – rozdělené obrazovce se používá, když je zařízení v orientaci na výšku.

Následující příklad kódu XAML ukazuje, jak nastavit [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) vlastnost [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

Následující příklad kódu ukazuje ekvivalent [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) vytvořené v C#:

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        MasterBehavior = MasterBehavior.Popover;
        ...
    }
}
```

Ale hodnotu [ `MasterBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) vlastnost ovlivňuje pouze aplikací běžících na tablet nebo plochy. Aplikace spuštěné na telefonech vždy *Popover* chování.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak používat [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) a přecházet mezi její stránky informace. Platformě Xamarin.Forms `MasterDetailPage` je stránka, která spravuje dvou stránkách související informace – hlavní stránky, který představuje položky a podrobnosti o stránku, která zobrazí podrobné informace o položky na hlavní stránce.


## <a name="related-links"></a>Související odkazy

- [Stránka typy](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)
