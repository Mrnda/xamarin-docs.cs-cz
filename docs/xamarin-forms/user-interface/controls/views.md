---
title: Zobrazení Xamarin.Forms
description: Zobrazení Xamarin.Forms jsou stavební bloky mobilních multiplatformních uživatelských rozhraní. Tento článek obsahuje seznam zobrazení, které jsou zahrnuty v Xamarin.Forms.
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 52d8d5f6eb38e5cb501d6284d08f7317981e0dcf
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998970"
---
# <a name="xamarinforms-views"></a>Zobrazení Xamarin.Forms

_Zobrazení Xamarin.Forms jsou stavební bloky mobilních multiplatformních uživatelských rozhraní._

Zobrazení jsou objekty uživatelského rozhraní, jako jsou popisky, tlačítka a jezdce, které se běžně označují jako *ovládací prvky* nebo *widgety* v jiných prostředích grafické programování. Zobrazení podporována Xamarin.Forms dědí ze [ `View` ](xref:Xamarin.Forms.View) třídy. Je možné rozdělit do několika kategorií:

## <a name="views-for-presentation"></a>Zobrazení pro prezentaci

### <a name="label"></a>Popisek

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label) Zobrazí řetězce jedním řádkem textu nebo víceřádkových bloků textu, buď pomocí konstanty nebo proměnné formátování. Nastavte [ `Text` ](xref:Xamarin.Forms.Label.Text) vlastnost řetězce pro konstantní formátování, nebo sadu [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) vlastnost [ `FormattedString` ](xref:Xamarin.Forms.FormattedString) objektu pro proměnnou formátování.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.Label) / [průvodce](~/xamarin-forms/user-interface/text/label.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![Příklad popisku](views-images/Label.png "popisek příklad")](views-images/Label-Large.png#lightbox "popisek příklad")<br /> [Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Image

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image) Zobrazuje rastrový obrázek. Rastrové obrázky si můžete stáhnout prostřednictvím webu, vloženy jako prostředky v běžných projekt nebo projekty platformy nebo vytvořeny pomocí .NET `Stream` objektu.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.Image) / [průvodce](~/xamarin-forms/user-interface/images.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![Obrázek příkladu](views-images/Image.png "obrázek příkladu")](views-images/Image-Large.png#lightbox "obrázek příkladu")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) Zobrazí plný pravoúhelník barevné [ `Color` ](xref:Xamarin.Forms.BoxView.Color) vlastnost. `BoxView` má výchozí žádost o velikosti 40 x 40. Pro ostatní formáty přiřadit [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) a [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) vlastnosti.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.BoxView) / [průvodce](~/xamarin-forms/user-interface/boxview.md) / [ukázkový 1](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView), [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration), [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife), [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), a [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![Příklad BoxView](views-images/BoxView.png "BoxView příklad")](views-images/BoxView-Large.png#lightbox "BoxView příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>Webové zobrazení

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) Zobrazí webové stránky nebo HTML obsah, podle toho, jestli [ `Source` ](xref:Xamarin.Forms.WebView.Source) je nastavena na [ `UriWebViewSource` ](xref:Xamarin.Forms.UrlWebViewSource) nebo [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource) objektu.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.WebView) / [průvodce](~/xamarin-forms/user-interface/webview.md) / [ukázkový 1](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) a [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![Příklad WebView](views-images/WebView.png "WebView příklad")](views-images/WebView-Large.png#lightbox "příklad WebView")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView) Zobrazí OpenGL grafiky v iOS a Android projekty. Není dostupná podpora pro univerzální platformu Windows. IOS a Android projekty vyžadují odkaz na **OpenTK 1.0** sestavení nebo **OpenTK** sestavení verze 1.0.0.0. `OpenGLView` je jednodušší použít v rámci sdíleného projektu, Pokud se používá při knihovny .NET Standard, pak závislostí služby také nutné (jak je uvedeno ve vzorovém kódu).<br /><br />Toto je pouze grafiky zařízení, která je integrována do Xamarin.Forms, ale aplikace Xamarin.Forms může taky mít za následek grafiky pomocí [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md), [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), nebo [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![Příklad OpenGLView](views-images/OpenGLView.png "OpenGLView příklad")](views-images/OpenGLView-Large.png#lightbox "OpenGLView příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) s [použití modelu code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>Mapa

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map) Zobrazí se mapa s. **Xamarin.Forms.Maps není úspěšný kvůli** musí být nainstalován balíček Nuget. Android a univerzální platformu Windows vyžadují autorizační klíč map.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.Maps.Map) / [průvodce](~/xamarin-forms/user-interface/map.md) / [vzorku](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![Mapování příklad](views-images/Map.png "mapování příklad")](views-images/Map-Large.png#lightbox "mapování příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Zobrazení, které se zahájí příkazy

### <a name="button"></a>Tlačítko

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button) je obdélníková objekt, který zobrazí text, a který se spustí [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) událost, když se stiskne.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.Button) / [průvodce](~/xamarin-forms/user-interface/button.md) / [vzorku](https://developer.xamarin.com/samples/UserInterface/ButtonDemos/) | [![Tlačítko příklad](views-images/Button.png "tlačítko příklad")](views-images/Button-Large.png#lightbox "tlačítko příklad")<br /> [Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) s [použití modelu code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar) Zobrazí uživateli typ textového řetězce a tlačítko (nebo kláves), která signalizuje aplikace můžete vyhledávat v oblasti. [ `Text` ](xref:Xamarin.Forms.SearchBar.Text) Vlastnost poskytuje přístup k textu a [ `SearchButtonPressed` ](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) událost ukazuje na to, že se stiskla tlačítka.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.SearchBar) | [![Příklad SearchBar](views-images/SearchBar.png "SearchBar příklad")](views-images/SearchBar-Large.png#lightbox "SearchBar příklad")<br /> [Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) s [použití modelu code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Zobrazení pro nastavení hodnot

### <a name="slider"></a>Posuvník

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider) Umožňuje uživateli vybrat `double` hodnotu z průběžné rozsahu zadaný [ `Minimum` ](xref:Xamarin.Forms.Slider.Minimum) a [ `Maximum` ](xref:Xamarin.Forms.Slider.Maximum) vlastnosti.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.Slider) / [průvodce](~/xamarin-forms/user-interface/slider.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) | [![Příklad posuvník](views-images/Slider.png "posuvník příklad")](views-images/Slider-Large.png#lightbox "Příklad posuvníku")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Krokovač

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper) Umožňuje uživateli vybrat `double` hodnotu z rozsahu hodnot přírůstkové zadaným [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum), [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum), a [ `Increment` ](xref:Xamarin.Forms.Stepper.Increment) vlastnosti.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.Stepper) | [![Příklad krokovač](views-images/Stepper.png "krokovač příklad")](views-images/Stepper-Large.png#lightbox "krokovač příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Přepínač

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch) má formu přepínač zapnuto/vypnuto, aby uživatel mohl vybrat hodnotu typu Boolean. [ `IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled) Vlastnost je stav přepínače a [ `Toggled` ](xref:Xamarin.Forms.Switch.Toggled) událost je aktivována při změně stavu.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.Switch) | [![Přepnout příklad](views-images/Switch.png "přepnout příklad")](views-images/Switch-Large.png#lightbox "přepnout příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker) Umožňuje uživateli vybrat datum pomocí nástroje pro výběr data platform. Nastavte rozsah povolených s [ `MinimumDate` ](xref:Xamarin.Forms.DatePicker.MinimumDate) a [ `MaximumDate` ](xref:Xamarin.Forms.DatePicker.MaximumDate) vlastnosti. [ `Date` ](xref:Xamarin.Forms.DatePicker.Date) Vybraným datem, je vlastnost a [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) událost je aktivována při změně této vlastnosti.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.DatePicker) / [průvodce](~/xamarin-forms/user-interface/datepicker.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![Příklad DatePicker](views-images/DatePicker.png "DatePicker příklad")](views-images/DatePicker-Large.png#lightbox "DatePicker – příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker) Umožňuje uživateli vybrat dobu s výběr času platformy. [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) Vlastnost je ve vybraném čase. Aplikace můžete sledovat změny v `Time` vlastnost nainstalováním obslužnou rutinu pro [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) událostí.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.TimePicker) | [![Příklad TimePicker](views-images/TimePicker.png "TimePicker příklad")](views-images/TimePicker-Large.png#lightbox "TimePicker příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Zobrazení pro úpravy textu

Tyto dvě třídy odvozovat [ `InputView` ](xref:Xamarin.Forms.InputView) třídu, která definuje [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) vlastnost.

<a name="entry" />

### <a name="entry"></a>Položka

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry) Umožňuje uživateli zadat a upravit jeden řádek textu. Je k dispozici jako text [ `Text` ](xref:Xamarin.Forms.Entry.Text) vlastnost a [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) a [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) jsou vyvolávány události, když změny textu nebo uživatel signalizuje dokončení klepnutím klávesu enter.<br /><br />Použití [ `Editor` ](#editor) pro zadávání a úpravy více řádků textu.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.Entry) / [průvodce](~/xamarin-forms/user-interface/text/entry.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Příklad položky](views-images/Entry.png "příklad položky")](views-images/Entry-Large.png#lightbox "příklad položky")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>Editor

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor) Umožňuje uživateli zadat a upravit více řádků textu. Je k dispozici jako text [ `Text` ](xref:Xamarin.Forms.Editor.Text) vlastnost a [ `TextChanged` ](xref:Xamarin.Forms.Editor.TextChanged) a [ `Completed` ](xref:Xamarin.Forms.Editor.Completed) jsou vyvolávány události, když změny textu nebo uživatel signalizuje dokončení.<br /><br />Použití [ `Entry` ](#entry) zobrazení pro zadávání a úpravy jeden řádek textu.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.Editor) / [průvodce](~/xamarin-forms/user-interface/text/editor.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Příklad položky](views-images/Editor.png "Editor příklad")](views-images/Editor-Large.png#lightbox "příklad editoru")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Zobrazení indikovat aktivitu

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) Chcete-li zobrazit, že aplikace se zabývají zdlouhavé aktivity bez ztráty žádné informace o průběhu používá animace. [ `IsRunning` ](xref:Xamarin.Forms.ActivityIndicator.IsRunning) Vlastnost ovládací prvky animace.<br /><br />Pokud se označuje průběh aktivity, použijte [ `ProgressBar` ](#progressbar) místo.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.ActivityIndicator) | [![Příklad ActivityIndicator](views-images/ActivityIndicator.png "ActivityIndicator příklad")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) využívá animace k zobrazení, že aplikace probíhá prostřednictvím dlouhé aktivity. Nastavte [ `Progress` ](xref:Xamarin.Forms.ProgressBar.Progress) vlastnost na hodnoty v rozmezí 0 až 1 informace o průběhu.<br /><br />Pokud probíhá aktivity není znám, použijte [ `ActivityIndicator` ](#activityindicator) místo.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.ProgressBar) | [![ProgressBar – příklad](views-images/ProgressBar.png "ProgressBar příklad")](views-images/ProgressBar-Large.png#lightbox "příklad ProgressBar")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) s [použití modelu code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Zobrazení, která se zobrazí kolekce

### <a name="picker"></a>Výběr

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker) Zobrazí vybrané položky ze seznamu textových řetězců a umožňuje vybrat položky zobrazení je klepnutí. Nastavte [ `Items` ](xref:Xamarin.Forms.Picker.Items) vlastnost do seznam řetězců, nebo [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) na kolekci objektů. [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Událost je aktivována, když je položka vybrána.<br /><br />`Picker` Zobrazí seznam položek, pouze pokud je vybrána. Použití [ `ListView` ](#listView) nebo [ `TableView` ](#tableView) posuvný seznam, který zůstane na stránce.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.Picker) / [průvodce](~/xamarin-forms/user-interface/picker/index.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![Příklad výběru](views-images/Picker.png "výběr příklad")](views-images/Picker-Large.png#lightbox "příklad výběr")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) s [použití modelu code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView) je odvozen od [ `ItemsView[Cell]` ](xref:Xamarin.Forms.ItemsView`1) a zobrazí posuvný seznam volitelných datových položek. Nastavte [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) na kolekci objektů a nastavení [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) vlastnost [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) objekt popisující, jak jsou položky má být formátováno. [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Události signalizuje, že výběr provedl, která je k dispozici jako [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) vlastnost.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.ListView) / [průvodce](~/xamarin-forms/user-interface/listview/index.md) / [vzorku](https://developer.xamarin.com/samples/WorkingWithListview) | [![Příklad ListView](views-images/ListView.png "ListView příklad")](views-images/ListView-Large.png#lightbox "příklad ListView")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>Zobrazení Tabulka

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView) Zobrazí seznam řádků typu [ `Cell` ](xref:Xamarin.Forms.Cell) volitelná záhlaví a podhlavičky. Nastavte [ `Root` ](xref:Xamarin.Forms.TableView.Root) vlastnost na objekt typu [ `TableRoot` ](xref:Xamarin.Forms.TableRoot)a přidejte [ `TableSection` ](xref:Xamarin.Forms.TableSection) objektů, které `TableRoot`. Každý `TableSection` je kolekce `Cell` objekty.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.TableView) / [průvodce](~/xamarin-forms/user-interface/tableview.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![Příklad zobrazení Tabulka](views-images/TableView.png "příklad zobrazení Tabulka")](views-images/TableView-Large.png#lightbox "příklad zobrazení Tabulka")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Související odkazy

- [Úvod do Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Ukázka Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentace k rozhraní Xamarin.Forms API](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
