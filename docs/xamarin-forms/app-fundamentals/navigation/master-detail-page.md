---
title: Stránky podrobností Xamarin.Forms
description: Xamarin.Forms MasterDetailPage je stránka, která spravuje dvě související stránky informací – stránku předlohy, která uvede počet položek a podrobnosti o stránku, která uvede podrobnosti o položkách ve stránce předlohy. Tento článek vysvětluje, jak používat MasterDetailPage a přecházet mezi její stránky informací.
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: a3d0edbd933339ee8b8a0a277a4f2493cc8dc70e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997462"
---
# <a name="xamarinforms-master-detail-page"></a>Stránky podrobností Xamarin.Forms

_Xamarin.Forms MasterDetailPage je stránka, která spravuje dvě související stránky informací – stránku předlohy, která uvede počet položek a podrobnosti o stránku, která uvede podrobnosti o položkách ve stránce předlohy. Tento článek vysvětluje, jak používat MasterDetailPage a přecházet mezi její stránky informací._

## <a name="overview"></a>Přehled

Stránky předlohy se obvykle zobrazuje seznam položek, jak je znázorněno na následujících snímcích obrazovky:

[![](master-detail-page-images/masterpage-components.png "Hlavní stránka součásti")](master-detail-page-images/masterpage-components-large.png#lightbox "komponenty stránky předlohy")

Umístění seznamu položek je stejný jako na jednotlivých platformách a výběrem jedné z položek přejdete na odpovídající stránce s podrobnostmi. Stránky předlohy kromě toho obsahuje taky navigační panel, který obsahuje tlačítko, které je možné přejít na stránku podrobností aktivní:

- V Iosu na navigačním panelu je k dispozici v horní části stránky a tlačítko, které přejde na stránku podrobností. Kromě toho na stránce aktivní podrobností se dá Navigovat potáhnutím prstem na levé straně stránky předlohy.
- Na navigačním panelu na Androidu, se nachází v horní části stránky a zobrazí název, ikonu a tlačítko, které přejde na stránku podrobností. Ikona je definována v `[Activity]` atribut, který upraví `MainActivity` třídu v projektu pro specifické pro platformu Android. Kromě toho na stránce aktivní podrobností se dá Navigovat potáhnutím prstem na levé straně stránky předlohy, klepnutím na stránce podrobností na pravé straně obrazovky a klepnutím *zpět* tlačítko v dolní části obrazovky.
- Na Universal Windows Platform (UWP), na navigačním panelu je k dispozici v horní části stránky a tlačítko, které přejde na stránku podrobností.

Podrobná data zobrazí stránky, která odpovídá položce vybrané na hlavní stránce a hlavní součástí na stránce podrobností se zobrazují na následujících snímcích obrazovky:

![](master-detail-page-images/detailpage-components.png "Součásti podrobností stránky")

Na stránce podrobností obsahuje navigační panel, jehož obsahem je závislé na platformě:

- V systémech iOS, na navigačním panelu se nachází v horní části stránky a zobrazí název a obsahuje tlačítko, které vrací na stránku předlohy, za předpokladu, že instance stránky podrobností je zabalena v [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instance. Kromě toho na hlavní stránce může být vrácen do potažení prstem na stránce podrobností na pravé straně.
- Navigační panel v Androidu, se nachází v horní části stránky a zobrazí název, ikonu a tlačítko, které se vrátí k hlavní stránce. Ikona je definována v `[Activity]` atribut, který upraví `MainActivity` třídu v projektu pro specifické pro platformu Android.
- Na navigačním panelu na UPW, se nachází v horní části stránky a zobrazí název a obsahuje tlačítko, které se vrátí k hlavní stránce.

### <a name="navigation-behavior"></a>Chování navigace

Chování navigaci mezi stránkami a podrobností je závislý na platformě:

- V systémech iOS, na stránce podrobností *snímky* napravo jako stránky předlohy snímků z levé straně a levé části podrobností je stále zobrazená stránka.
- V Androidu, podrobnosti a hlavní stránky jsou *buňka* na sobě navzájem.
- Na UPW, podrobnosti a hlavní stránky jsou *Prohodit*.

Podobného chování zůstanou zachována v režimu na šířku, s tím rozdílem, že na hlavní stránce v Iosu a Androidu má podobné šířku jako hlavní stránky v režimu na výšku, tak víc podrobností stránky se nebude zobrazovat.

Informace o řízení chování navigace, naleznete v tématu [řízení chování zobrazení stránky podrobností](#Controlling_the_Detail_Page_Display_Behavior).

## <a name="creating-a-masterdetailpage"></a>Vytváření MasterDetailPage

A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) obsahuje [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) a [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) vlastnosti, které jsou typu [ `Page` ](xref:Xamarin.Forms.Page), které se používají k získání a nastavení stránek a podrobností v uvedeném pořadí.

> [!IMPORTANT]
> A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) byla navržena jako kořenový stránky a použijte ho jako podřízenou stránku v jiných typech stránka by mohlo způsobit neočekávané a nekonzistentní chování. Kromě toho doporučujeme na hlavní stránce [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) by vždycky měla být [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance a že na stránce podrobností mělo být vyplněno pouze pomocí [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), a `ContentPage` instancí. To vám pomůže zajistit konzistentní uživatelské prostředí na všech platformách.

Následující příklad ukazuje kód XAML [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) , který nastavuje [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) a [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) vlastnosti:

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

Následující příklad kódu ukazuje ekvivalent [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) vytvořené v jazyce C#:

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

[ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) Je nastavena na [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) instance. [ `MasterDetailPage.Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) Je nastavena na [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) obsahující `ContentPage` instance.

### <a name="creating-the-master-page"></a>Vytvoření stránky předlohy

Následující příklad kódu XAML ukazuje deklarace `MasterPage` objektu, který se odkazuje prostřednictvím [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) vlastnost:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             Icon="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView" x:FieldModifier="public">
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

Na stránce se skládá z [ `ListView` ](xref:Xamarin.Forms.ListView) , který je naplněný daty v XAML tak, že nastavíte její [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) vlastnost pole `MasterPageItem` instancí. Každý `MasterPageItem` definuje `Title`, `IconSource`, a `TargetType` vlastnosti.

A [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) je přiřazen [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) vlastnost pro zobrazení jednotlivých `MasterPageItem`. `DataTemplate` Obsahuje [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) , který se skládá z [ `Image` ](xref:Xamarin.Forms.Image) a [ `Label` ](xref:Xamarin.Forms.Label). [ `Image` ](xref:Xamarin.Forms.Image) Zobrazí `IconSource` hodnotu vlastnosti a [ `Label` ](xref:Xamarin.Forms.Label) zobrazí `Title` hodnota vlastnosti pro každý `MasterPageItem`.

Na stránce jeho [ `Title` ](xref:Xamarin.Forms.Page.Title) a [ `Icon` ](xref:Xamarin.Forms.Page.Icon) set vlastnosti. Ikona se zobrazí na stránce s podrobnostmi, za předpokladu, že na stránce podrobností záhlaví okna. Toto musí být povolené na iOS obalením instanci stránky podrobností [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instance.

> [!NOTE]
> [ `MasterDetailPage.Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) Stránka musí mít jeho [ `Title` ](xref:Xamarin.Forms.Page.Title) vlastnost nastavena, nebo dojde k výjimce.

Následující příklad kódu ukazuje na stejnou stránku vytvořené v jazyce C#:

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

### <a name="creating-and-displaying-the-detail-page"></a>Vytváření a zobrazování podrobností stránky

`MasterPage` Instance obsahuje `ListView` vlastnost, která zveřejňuje jeho [ `ListView` ](xref:Xamarin.Forms.ListView) instance tak, aby `MainPage` [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) můžete zaregistrovat instanci obslužné rutiny události pro zpracování [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) událostí. Díky tomu `MainPage` instance nastavit [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) vlastnost na stránce, která představuje vybrané `ListView` položky. Následující příklad kódu ukazuje obslužné rutiny události:

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.listView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.listView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

`OnItemSelected` Metoda provede následující akce:

- Načítá [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) z [ `ListView` ](xref:Xamarin.Forms.ListView) instance a že není k dispozici `null`, nastaví do nové instance typu stránky uložené v stráncespodrobnostmi`TargetType`vlastnost `MasterPageItem`. Typ stránky není zabalené ve [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instanci a zkontrolujte, že na ikonu odkazovány prostřednictvím [ `Icon` ](xref:Xamarin.Forms.Page.Icon) vlastnost `MasterPage` se zobrazí na stránce s podrobnostmi v iOS.
- Na vybranou položku v [ `ListView` ](xref:Xamarin.Forms.ListView) je nastavena na `null` a zkontrolujte, že žádná z `ListView` příště se vybrané položky `MasterPage` se zobrazí.
- Na stránce podrobností se zobrazí uživateli tím, že nastavíte [ `MasterDetailPage.IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) vlastnost `false`. Tato vlastnost určuje, zda se zobrazí na stránce nebo podrobností. By mělo být nastavené `true` pro zobrazení stránky předlohy a `false` zobrazíte na stránce s podrobnostmi.

Následující snímky obrazovky zobrazit `ContactPage` stránku s podrobnostmi, které se zobrazí poté, co byl určen na hlavní stránce:

![](master-detail-page-images/detailpage.png "Příklad podrobností stránky")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>Řízení chování podrobnosti zobrazení stránky

Jak [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) spravuje stránky a podrobností závisí na tom, jestli aplikace běží na telefonu nebo tabletu, orientace zařízení a hodnota [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) Vlastnost. Tato vlastnost určuje, jak se zobrazí na stránce podrobností. Možné hodnoty jsou:

- **Výchozí** – stránky se zobrazí ve výchozí platformu.
- **Popover** – na stránce podrobností pokrývá nebo částečně pokrývá stránky předlohy.
- **Rozdělení** – stránky předlohy se zobrazuje na levé straně a na stránce podrobností je na pravé straně.
- **SplitOnLandscape** – rozdělená obrazovka se používá, když je zařízení v orientaci na šířku.
- **SplitOnPortrait** – rozdělená obrazovka se používá, když je zařízení v orientaci na výšku.

Následující příklad kódu XAML ukazuje, jak nastavit [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) vlastnosti [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

Následující příklad kódu ukazuje ekvivalent [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) vytvořené v jazyce C#:

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

Však hodnoty [ `MasterBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) vlastnost ovlivňuje pouze aplikace běžící na tabletech nebo plochy. Aplikace spuštěné na telefonech vždy mít *Popover* chování.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak používat [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) a přecházet mezi její stránky informací. Xamarin.Forms `MasterDetailPage` je stránka, která spravuje dvě stránky související informace – stránku předlohy, která uvede počet položek a podrobnosti o stránku, která uvede podrobnosti o položkách ve stránce předlohy.


## <a name="related-links"></a>Související odkazy

- [Variace stránek](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/)
- [MasterDetailPage](xref:Xamarin.Forms.MasterDetailPage)
