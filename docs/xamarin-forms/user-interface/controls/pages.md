---
title: "Stránky Xamarin.Forms"
description: "Stránky Xamarin.Forms představují obrazovky napříč platformami mobilních aplikací."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 5f979d2dbb894107d8d606ec1f41de44c294cdd3
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/21/2018
---
# <a name="xamarinforms-pages"></a>Stránky Xamarin.Forms

_Stránky Xamarin.Forms představují obrazovky napříč platformami mobilních aplikací._

Všechny typy stránky, které jsou popsány níže jsou odvozeny od platformě Xamarin.Forms [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) třídy. Tyto vizuální prvky zabírají všechny nebo většinu na obrazovce. A `Page` objektu představuje `ViewController` v iOS a `Page` v univerzální platformu Windows. V systému Android se každé stránce zabírají obrazovku podobnou `Activity`, ale jsou stránky Xamarin.Forms *není* `Activity` objekty.

[ ![](pages-images/pages-sml.png "Stránka Xamarin.Forms typy")](pages-images/pages.png#lightbox "stránce Xamarin.Forms")

## <a name="pages"></a>Stránky

Xamarin.Forms podporuje následující typy stránky:

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     | 
| --- | --- | 
| [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) je nejjednodušší a nejběžnější typ stránky. Nastavte [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentPage.Content/) vlastnost na jednu [ `View` ](views.md) objekt, který je nejčastěji [ `Layout` ](layouts.md) například [ `StackLayout` ](layouts.md#stackLayout), [ `Grid` ](layouts.md#grid), nebo [ `ScrollView` ](layouts.md#scrollView).<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) | [![Příklad ContentPage](pages-images/ContentPage.png "ContentPage příklad")](pages-images/ContentPage-Large.png#lightbox "ContentPage příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     | 
| --- | --- | 
| A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) spravuje dvě podokna informací. Nastavte [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) vlastnost na stránku obecně zobrazující seznam nebo nabídku. Nastavte [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) vlastnosti stránky zobrazující vybrané položky ze stránky předlohy. [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) Vlastnost řídí, zda je viditelná stránce nebo podrobností.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) / [průvodce](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![Příklad MasterDetailPage](pages-images/MasterDetailPage.png "MasterDetailPage příklad")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) s [kódu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     | 
| --- | --- | 
| [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Spravuje navigace mezi dalších stránek pomocí architektury založené na zásobníku. Pokud používáte navigaci na stránce v aplikaci, instance domovské stránky by měla být předána konstruktoru `NavigationPage` objektu.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) / [průvodce](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [ukázkové 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/), [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/), a [3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![Příklad NavigationPage](pages-images/NavigationPage.png "NavigationPage příklad")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) s [kód = za](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     | 
| --- | --- | 
| [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) je odvozena z abstraktní [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) třídy a umožňuje navigace mezi podřízené stránky pomocí karty. Nastavte [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) vlastnosti kolekce stránky, nebo sady [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) vlastnosti kolekce datových objektů a [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) vlastnosti [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) popisující, jak má být vizuálně reprezentována každý objekt.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) / [průvodce](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [ukázkové 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/) a [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![Příklad TabbedPage](pages-images/TabbedPage.png "TabbedPage příklad")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     | 
| --- | --- | 
| [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) je odvozena z abstraktní [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) třídy a umožňuje navigace mezi podřízené stránky přes prstů k načtení. Nastavte [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) vlastnosti kolekce [ `ContentPage` ](#contentPage) objekty, nebo sady [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) vlastnosti kolekce datových objektů a [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) vlastnosti [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) popisující, jak má být vizuálně reprezentována každý objekt.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) / [průvodce](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [ukázkové 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/) a [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![Příklad CarouselPage](pages-images/CarouselPage.png "CarouselPage příklad")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     | 
| --- | --- | 
| [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) Zobrazí obsah přes celou obrazovku s šablony ovládacího prvku a je základní třídou pro [ `ContentPage` ](#contentPage).<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) / [Průvodce](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Příklad TemplatedPage](pages-images/TemplatedPage.png "TemplatedPage příklad")](pages-images/TemplatedPage.png "TemplatedPage příklad") |
|     |     |

## <a name="related-links"></a>Související odkazy

- [Úvod k platformě Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Ukázka Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentace rozhraní API Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
