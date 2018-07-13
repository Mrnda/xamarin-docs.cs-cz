---
title: Nástroj pro vykreslování základní třídy a nativní ovládací prvky
description: Každý ovládací prvek Xamarin.Forms má související zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Tento článek uvádí nástroj pro vykreslování a nativní ovládací prvek třídy, které implementují každé stránky Xamarin.Forms, rozložení, zobrazení a buňky.
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 01610581a0fd72401cb6daa4d5c8c2cb99fe6a2f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995105"
---
# <a name="renderer-base-classes-and-native-controls"></a>Nástroj pro vykreslování základní třídy a nativní ovládací prvky

_Každý ovládací prvek Xamarin.Forms má související zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Tento článek uvádí nástroj pro vykreslování a nativní ovládací prvek třídy, které implementují každé stránky Xamarin.Forms, rozložení, zobrazení a buňky._

S výjimkou produktů `MapRenderer` třídy, renderery specifické pro platformu najdete v následujících oborů názvů:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **Universal Windows Platform (UWP)** – Xamarin.Forms.Platform.UWP

`MapRenderer` Třídy najdete v následujících oborů názvů:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Universal Windows Platform (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Stránky

Následující tabulka uvádí nástroj pro vykreslování a nativní ovládací prvek třídy, které implementují každý Xamarin.Forms [stránky](~/xamarin-forms/user-interface/controls/pages.md) typu:

|Stránka|Nástroj pro vykreslování|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|Skupinu ViewGroup||FrameworkElement|
|[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)|PhoneMasterDetailRenderer (iOS – telefon), TabletMasterDetailPageRenderer (iOS – Tablet), MasterDetailRenderer (Android), MasterDetailPageRenderer (Android AppCompat), MasterDetailPageRenderer (UPW)|UIViewController (telefon), UISplitViewController (Tablet)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (vlastní ovládací prvek)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|NavigationRenderer (iOS a Android), NavigationPageRenderer (Android AppCompat), NavigationPageRenderer (UPW)|UIToolbar|Skupinu ViewGroup|Skupinu ViewGroup|FrameworkElement (vlastní ovládací prvek)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|TabbedRenderer (iOS a Android), TabbedPageRenderer (Android AppCompat), TabbedPageRenderer (UPW)|UIView|ViewPager|ViewPager|FrameworkElement (Pivot)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|UIViewController|Skupinu ViewGroup||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>Rozložení

Následující tabulka uvádí nástroj pro vykreslování a nativní ovládací prvek třídy, které implementují každý Xamarin.Forms [rozložení](~/xamarin-forms/user-interface/controls/layouts.md) typu:

|Rozložení|Nástroj pro vykreslování|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)|ViewRenderer|UIView|Zobrazit|FrameworkElement|
|[`ContentView`](xref:Xamarin.Forms.ContentView)|ViewRenderer|UIView|Zobrazit|FrameworkElement|
|[`Frame`](xref:Xamarin.Forms.Frame)|FrameRenderer|UIView|Skupinu ViewGroup|Ohraničení|
|[`ScrollView`](xref:Xamarin.Forms.ScrollView)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)|ViewRenderer|UIView|Zobrazit|FrameworkElement|
|[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)|ViewRenderer|UIView|Zobrazit|FrameworkElement|
|[`Grid`](xref:Xamarin.Forms.Grid)|ViewRenderer|UIView|Zobrazit|FrameworkElement|
|[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)|ViewRenderer|UIView|Zobrazit|FrameworkElement|
|[`StackLayout`](xref:Xamarin.Forms.StackLayout)|ViewRenderer|UIView|Zobrazit|FrameworkElement|

## <a name="views"></a>Zobrazení

Následující tabulka uvádí nástroj pro vykreslování a nativní ovládací prvek třídy, které implementují každý Xamarin.Forms [zobrazení](~/xamarin-forms/user-interface/controls/views.md) typu:

|Zobrazení|Nástroj pro vykreslování|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer (iOS a Android), BoxViewRenderer (UPW)|UIView|Skupinu ViewGroup||Obdélník|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|Tlačítka uživatelského rozhraní|Tlačítko|AppCompatButton|Tlačítko|
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||Image|
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|SeekBar||Posuvník|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||Ovládací prvek|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|Přepínač|SwitchCompat|ToggleSwitch|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](xref:Xamarin.Forms.WebView)|WebViewRenderer|UIWebView|Webové zobrazení||Webové zobrazení|

## <a name="cells"></a>Buňky

Následující tabulka uvádí nástroj pro vykreslování a nativní ovládací prvek třídy, které implementují každý Xamarin.Forms [buňky](~/xamarin-forms/user-interface/controls/cells.md) typu:

|Buňky|Nástroj pro vykreslování|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|UITableViewCell s UITextField|LinearLayout s TextView a EditText|DataTemplate u objektu TextBox|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|UITableViewCell s UISwitch|Přepínač|S tabulkou obsahující TextBlock a ToggleSwitch DataTemplate|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|LinearLayout s dvěma TextViews|DataTemplate s obsahující dva objekty TextBlock se objektu StackPanel|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|UITableViewCell s UIImage|LinearLayout s dvěma TextViews a ImageView|S tabulkou obsahující bitovou kopii a dva objekty TextBlock se DataTemplate|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|Zobrazit|S ContentPresenter DataTemplate|

## <a name="summary"></a>Souhrn

Tento článek uvádí nástroj pro vykreslování a nativní ovládací prvek třídy, které implementují každé stránky Xamarin.Forms, rozložení, zobrazení a buňky. Každý ovládací prvek Xamarin.Forms má související zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku.

## <a name="related-links"></a>Související odkazy

- [Vlastní Renderery (Xamarin University Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
