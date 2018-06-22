---
title: Základní třídy zobrazovací jednotky a nativní ovládací prvky
description: Každý prvek Xamarin.Forms musí doprovodné zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. V tomto článku jsou uvedené zobrazovací jednotky a nativní řízení třídy, které implementují každou stránku Xamarin.Forms, rozložení, zobrazení a buňky.
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 513b9b3738b9481444cdad10daa9b11a8441c9dd
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
ms.locfileid: "32019682"
---
# <a name="renderer-base-classes-and-native-controls"></a>Základní třídy zobrazovací jednotky a nativní ovládací prvky

_Každý prvek Xamarin.Forms musí doprovodné zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. V tomto článku jsou uvedené zobrazovací jednotky a nativní řízení třídy, které implementují každou stránku Xamarin.Forms, rozložení, zobrazení a buňky._

S výjimkou produktů `MapRenderer` třídy pro vykreslování specifických pro platformy najdete v následujících oborů názvů:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (kompatibility aplikace)** – Xamarin.Forms.Platform.Android.AppCompat
- **Univerzální platformu Windows (UWP)** – Xamarin.Forms.Platform.UWP

`MapRenderer` Třída naleznete v následujících oborů názvů:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Univerzální platformu Windows (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Stránky

Následující tabulka uvádí zobrazovací jednotky a nativní řízení třídy, které implementují každý Xamarin.Forms [stránky](~/xamarin-forms/user-interface/controls/pages.md) typu:

|Stránka|Zobrazovací jednotky|iOS|Android|Android (kompatibility aplikace)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|Skupinu ViewGroup||FrameworkElement|
|[`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)|PhoneMasterDetailRenderer (iOS – telefon), TabletMasterDetailPageRenderer (iOS – Tablet), MasterDetailRenderer (Android), MasterDetailPageRenderer (Android kompatibility aplikace), MasterDetailPageRenderer (UWP)|UIViewController (telefon), (Tablet) UISplitViewController|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (vlastní kontrola)|
|[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)|NavigationRenderer (iOS a Android), NavigationPageRenderer (Android kompatibility aplikace), NavigationPageRenderer (UWP)|UIToolbar|Skupinu ViewGroup|Skupinu ViewGroup|FrameworkElement (vlastní kontrola)|
|[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)|TabbedRenderer (iOS a Android), TabbedPageRenderer (Android kompatibility aplikace), TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (Pivot)|
|[`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)|PageRenderer|UIViewController|Skupinu ViewGroup||FrameworkElement|
|[`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>Rozložení

Následující tabulka uvádí zobrazovací jednotky a nativní řízení třídy, které implementují každý Xamarin.Forms [rozložení](~/xamarin-forms/user-interface/controls/layouts.md) typu:

|Rozložení|Zobrazovací jednotky|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)|ViewRenderer|UIView|Zobrazit|FrameworkElement|
|[`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)|ViewRenderer|UIView|Zobrazit|FrameworkElement|
|[`Frame`](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/)|FrameRenderer|UIView|Skupinu ViewGroup|Ohraničení|
|[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)|ViewRenderer|UIView|Zobrazit|FrameworkElement|
|[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)|ViewRenderer|UIView|Zobrazit|FrameworkElement|
|[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)|ViewRenderer|UIView|Zobrazit|FrameworkElement|
|[`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)|ViewRenderer|UIView|Zobrazit|FrameworkElement|
|[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)|ViewRenderer|UIView|Zobrazit|FrameworkElement|

## <a name="views"></a>zobrazení

Následující tabulka uvádí zobrazovací jednotky a nativní řízení třídy, které implementují každý Xamarin.Forms [zobrazení](~/xamarin-forms/user-interface/controls/views.md) typu:

|zobrazení|Zobrazovací jednotky|iOS|Android|Android (kompatibility aplikace)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)|BoxRenderer (iOS a Android), BoxViewRenderer (UWP)|UIView|Skupinu ViewGroup||rámeček|
|[`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)|ButtonRenderer|UIButton|Tlačítko|AppCompatButton|Tlačítko|
|[`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/)|CarouselViewRenderer|UIScrollView|RecyclerView||FlipView|
|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)|ImageRenderer|UIImageView|ImageView||Image|
|[`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)|SliderRenderer|UISlider|SeekBar||Posuvník|
|[`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|StepperRenderer|UIStepper|LinearLayout||Ovládací prvek|
|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|SwitchRenderer|UISwitch|přepínače|SwitchCompat|ToggleSwitch|
|[`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)|WebViewRenderer|UIWebView|Webové zobrazení||Webové zobrazení|

## <a name="cells"></a>Buněk

Následující tabulka uvádí zobrazovací jednotky a nativní řízení třídy, které implementují každý Xamarin.Forms [buňky](~/xamarin-forms/user-interface/controls/cells.md) typu:

|Buněk|Zobrazovací jednotky|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/)|EntryCellRenderer|UITableViewCell s UITextField|LinearLayout s TextView a EditText|DataTemplate s textové pole|
|[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/)|SwitchCellRenderer|UITableViewCell s UISwitch|přepínače|DataTemplate mřížku obsahující TextBlock a ToggleSwitch|
|[`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/)|TextCellRenderer|UITableViewCell|LinearLayout s dvěma TextViews|DataTemplate s StackPanel, který obsahuje dva objekty TextBlocks|
|[`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/)|ImageCellRenderer|UITableViewCell s UIImage|LinearLayout s dvěma TextViews a ImageView|DataTemplate mřížku obsahující bitovou kopii a dva objekty TextBlocks|
|[`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|Zobrazit|DataTemplate s ContentPresenter|

## <a name="summary"></a>Souhrn

Tento článek uvádí zobrazovací jednotky a nativní řízení třídy, které implementují každou stránku Xamarin.Forms, rozložení, zobrazení a buňky. Každý prvek Xamarin.Forms musí doprovodné zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku.

## <a name="related-links"></a>Související odkazy

- [Vlastní nástroji pro vykreslování (Xamarin univerzity Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
