---
title: Xamarin.Forms Views
description: "Zobrazení Xamarin.Forms jsou jako stavební bloky pro různé platformy mobilních uživatelská rozhraní."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: f7d27c5226741ec2b105633206ebaa0ac73d9a7b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="xamarinforms-views"></a>Xamarin.Forms Views

_Zobrazení Xamarin.Forms jsou jako stavební bloky pro různé platformy mobilních uživatelská rozhraní._

Zobrazení jsou objekty uživatelského rozhraní, jako je například popisky, tlačítek a jezdce, které se běžně označují jako *ovládací prvky* nebo *pomůcky* v jiných prostředích grafické programování. Nepodporuje Xamarin.Forms dědí ze zobrazení [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) třídy. Je možné rozdělit do několika kategorií:

## <a name="views-for-presentation"></a>Zobrazení pro prezentace

### <a name="label"></a>Popisek

|     |     |
| --- | --- |
| [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Zobrazí řetězce textových nebo bloky více řádků textu, buď pomocí konstantní nebo proměnné formátování. Nastavte [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) na řetězec pro konstantní formátování, nebo sadu [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) vlastnosti [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/) objekt pro proměnnou formátování.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) / [průvodce](~/xamarin-forms/user-interface/text/label.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![Příklad popisku](views-images/Label.png "popisku příklad")](views-images/Label-Large.png#lightbox "příklad popisku")<br /> [Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Image

|     |     |
| --- | --- |
| [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Zobrazí rastrový obrázek. Rastrové obrázky si můžete stáhnout z webu, vložené jako prostředky v běžné projektu nebo projekty platformy, nebo vytvořený .NET `Stream` objektu.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) / [průvodce](~/xamarin-forms/user-interface/images.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![Obrázek příkladu](views-images/Image.png "obrázek příkladu")](views-images/Image-Large.png#lightbox "bitové kopie příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) Zobrazí plného obdélníku barevné [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) vlastnost. `BoxView` má výchozí žádost o velikost 40 x 40. Pro ostatní formáty přiřadit [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) a [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) vlastnosti.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) / [průvodce](~/xamarin-forms/user-interface/boxview.md) / [ukázkové 1](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView), [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration), [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife), [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), a [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![Příklad BoxView](views-images/BoxView.png "BoxView příklad")](views-images/BoxView-Large.png#lightbox "BoxView příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) Zobrazí webové stránky nebo HTML obsahu, podle toho, zda [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) je nastavena na [ `UriWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UrlWebViewSource/) nebo [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/) objektu.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) / [průvodce](~/xamarin-forms/user-interface/webview.md) / [ukázkové 1](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) a [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![Příklad webového zobrazení](views-images/WebView.png "příklad webového zobrazení")](views-images/WebView-Large.png#lightbox "příklad webového zobrazení")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/) Zobrazí OpenGL grafika v iOS a Android projekty. Neexistuje žádná podpora pro univerzální platformu Windows. IOS a Android projekty vyžadují odkaz na **OpenTK 1.0** sestavení nebo **OpenTK** verzi 1.0.0.0 sestavení. `OpenGLView` je jednodušší použít v projektu sdíleného; Pokud se používá v knihovny PCL nebo .NET Standard, pak závislostí služby se také bude vyžadovat (jak je znázorněno v ukázkovém kódu).<br /><br />Toto je pouze grafiky zařízení, která je integrována do Xamarin.Forms, ale Xamarin.Forms aplikace může také zpracovat, pomocí grafických [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md), [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), nebo [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/)<br /><br /> | [![Příklad OpenGLView](views-images/OpenGLView.png "OpenGLView příklad")](views-images/OpenGLView-Large.png#lightbox "OpenGLView příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) s [kódu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>mapy

|     |     |
| --- | --- |
| [`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) Zobrazí mapu. **Xamarin.Forms.Maps** musí být nainstalován balíček Nuget. Android a univerzální platformu Windows vyžadují autorizační klíč mapy.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) / [průvodce](~/xamarin-forms/user-interface/map.md) / [vzorku](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![Map – příklad](views-images/Map.png "mapování příklad")](views-images/Map-Large.png#lightbox "mapování příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Zobrazení, které zahájí příkazy

### <a name="button"></a>Tlačítko

|     |     |
| --- | --- |
| [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) je obdélníková objekt, který zobrazí text, a které aktivuje [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) událostí po stisknutí.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) | [![Tlačítko příklad](views-images/Button.png "tlačítko příklad")](views-images/Button-Large.png#lightbox "tlačítko příklad")<br /> [Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) s [kódu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Zobrazí oblast pro uživatele na typ textového řetězce a tlačítko (nebo na klávesnici klíč), který se dá signál aplikaci k provedení vyhledávání. [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.Text/) Vlastnost poskytuje přístup k text a [ `SearchButtonPressed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/) událost označuje, že byla stisknuta tlačítko.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) | [![Příklad SearchBar](views-images/SearchBar.png "SearchBar příklad")](views-images/SearchBar-Large.png#lightbox "SearchBar příklad")<br /> [Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) s [kódu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Zobrazení pro nastavení hodnot 

### <a name="slider"></a>Posuvník

|     |     |
| --- | --- |
| [`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) Umožňuje uživateli vybrat `double` hodnotu nepřetržitá rozsah zadaný [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) a [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) vlastnosti.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) / [průvodce](~/xamarin-forms/user-interface/slider.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) | [![Příklad posuvníku](views-images/Slider.png "posuvníku příklad")](views-images/Slider-Large.png#lightbox "Příklad posuvníku")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Krokovač

|     |     |
| --- | --- |
| [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) Umožňuje uživateli vybrat `double` hodnotu z rozsahu hodnot přírůstkové zadaným [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Minimum/), [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Maximum/), a [ `Increment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) vlastnosti.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) | [![Příklad krokovač](views-images/Stepper.png "krokovač příklad")](views-images/Stepper-Large.png#lightbox "krokovač příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>přepínače 

|     |     |
| --- | --- |
| [`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) má podobu přepínač zapnout nebo vypnout umožňuje uživateli vybrat logickou hodnotu. [ `IsToggled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) Vlastnost je stav přepínače a [ `Toggled` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) událost je aktivována například při změně stavu.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) | [![Příklad přepínače](views-images/Switch.png "přepínač příklad")](views-images/Stepper-Large.png#lightbox "přepínač příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) Umožňuje uživateli vybrat datum s výběr platformy data. Nastavit rozsah povolených data se [ `MinimumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) a [ `MaximumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) vlastnosti. [ `Date` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) Vlastnost je vybraným datem a [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) událost je aktivována, jestliže tuto vlastnost změny.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) / [průvodce](~/xamarin-forms/user-interface/datepicker.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![Ovládací prvek DatePicker příklad](views-images/DatePicker.png "ovládací prvek DatePicker příklad")](views-images/DatePicker-Large.png#lightbox "ovládací prvek DatePicker příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) Umožňuje uživateli vybrat dobu s výběr platformy času. [ `Time` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) Vlastnost je vybraný čas. Aplikace můžete sledovat změny v `Time` vlastnost nainstalováním obslužnou rutinu pro [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) událostí.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) | [![Příklad TimePicker](views-images/TimePicker.png "TimePicker příklad")](views-images/TimePicker-Large.png#lightbox "TimePicker příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Zobrazení pro úpravy textu

Tyto dvě třídy odvozovat [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) třídy, která definuje [ `Keyboard` ](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) vlastnost.

<a name="entry" />

### <a name="entry"></a>Položka

|     |     |
| --- | --- |
| [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Umožňuje uživateli zadat a upravit na jednom řádku textu. Je k dispozici jako text [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) vlastnost a [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) a [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) při vyvolání událostí, když text se změní nebo uživatel signály dokončení klepnutím klávesu enter.<br /><br />Použijte [ `Editor` ](#editor) pro zadávání a úpravy více řádků textu.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) / [průvodce](~/xamarin-forms/user-interface/text/entry.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Příklad položky](views-images/Entry.png "příklad položky")](views-images/Entry-Large.png#lightbox "příklad položky")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>Editor

|     |     |
| --- | --- |
| [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) Umožňuje uživateli zadat a upravit více řádků textu. Je k dispozici jako text [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Editor.Text/) vlastnost a [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) a [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) při vyvolání událostí, když text se změní nebo uživatel signály dokončení.<br /><br />Použijte [ `Entry` ](#entry) zobrazení pro zadávání a úpravy na jednom řádku textu.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) / [průvodce](~/xamarin-forms/user-interface/text/editor.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Příklad položky](views-images/Editor.png "Editor příklad")](views-images/Editor-Large.png#lightbox "příklad editoru")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Zobrazení k označení aktivity

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) Chcete-li zobrazit, že je aplikace zapojený do zdlouhavé aktivity bez nutnosti poskytnutí jakoukoli indikaci toho průběh používá animace. [ `IsRunning` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) Vlastnost ovládací prvky animace.<br /><br />Pokud se označuje aktivity průběh, použijte [ `ProgressBar` ](#progressbar) místo.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) | [![Příklad ActivityIndicator](views-images/ActivityIndicator.png "ActivityIndicator příklad")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) animace používá k zobrazení, že aplikace je pokročíte prostřednictvím zdlouhavé aktivity. Nastavte [ `Progress` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ProgressBar.Progress/) vlastnost hodnoty mezi 0 a 1 informace o průběhu.<br /><br />Pokud není znám aktivity průběh, použijte [ `ActivityIndicator` ](#activityindicator) místo.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) | [![ProgressBar – příklad](views-images/ProgressBar.png "ProgressBar – příklad")](views-images/ProgressBar-Large.png#lightbox "ProgressBar – příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) s [kódu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Zobrazení, která se zobrazí také kolekce

### <a name="picker"></a>Výběr.

|     |     |
| --- | --- |
| [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Zobrazí vybrané položky ze seznamu textových řetězců a umožňuje vybrat tuto položku, když je stisknuté zobrazení. Nastavte [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) vlastnost, která má seznam řetězců, nebo [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) vlastnost na kolekci objektů. [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) Událost je aktivována, když je položka vybrána.<br /><br />`Picker` Zobrazí seznam položek, pouze pokud je vybrána. Použití [ `ListView` ](#listView) nebo [ `TableView` ](#tableView) posuvný seznam, který zůstává na stránce.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) / [průvodce](~/xamarin-forms/user-interface/picker/index.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![Příklad výběru](views-images/Picker.png "výběr příklad")](views-images/Picker-Large.png#lightbox "příklad výběr.")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) s [kódu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) odvozená z [ `ItemsView[Cell]` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) a zobrazí posuvný seznam položek, které lze vybírat data. Nastavit [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) vlastnost na kolekci objektů a sadu [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) vlastnosti [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) objekt popisující, jak jsou položky Chcete-li být ve formátu. [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) Událost signalizuje, že výběr byl změněn, která je k dispozici jako [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) vlastnost.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) / [průvodce](~/xamarin-forms/user-interface/listview/index.md) / [vzorku](https://developer.xamarin.com/samples/WorkingWithListview) | [![Příklad ListView](views-images/ListView.png "ListView příklad")](views-images/ListView-Large.png#lightbox "příklad ListView")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) Zobrazí seznam řádků typu [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) s volitelné hlavičky a dílčí záhlaví. Nastavte [ `Root` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) vlastností pro objekt typu [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/)a přidejte [ `TableSection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) objekty, na který `TableRoot`. Každý `TableSection` je kolekce `Cell` objekty.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) / [průvodce](~/xamarin-forms/user-interface/tableview.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![Příklad zobrazení Tabulka](views-images/TableView.png "zobrazení Tabulka příklad")](views-images/TableView-Large.png#lightbox "příklad zobrazení Tabulka")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Související odkazy

- [Úvod k platformě Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Ukázka Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentace rozhraní API Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
