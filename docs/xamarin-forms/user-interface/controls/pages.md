---
title: Stránky Xamarin.Forms
description: Xamarin.Forms stránky představují obrazovky multiplatformní mobilní aplikace. Tento článek obsahuje seznam stránek, které jsou zahrnuty v Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: fea1db6c65e533692601439f4712d15371740644
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995895"
---
# <a name="xamarinforms-pages"></a>Stránky Xamarin.Forms

_Xamarin.Forms stránky představují obrazovky multiplatformní mobilní aplikace._

Všechny typy stránek, které jsou popsány níže jsou odvozeny z Xamarin.Forms [ `Page` ](xref:Xamarin.Forms.Page) třídy. Tyto vizuální prvky zabírat všechny nebo většinu obrazovky. A `Page` objekt představuje `ViewController` v Iosu a `Page` na univerzální platformě Windows. V Androidu, každá stránka zabírá obrazovku `Activity`, ale Xamarin.Forms stránky jsou *není* `Activity` objekty.

[ ![](pages-images/pages-sml.png "Stránka Xamarin.Forms typy")](pages-images/pages.png#lightbox "typy stránky Xamarin.Forms")

## <a name="pages"></a>Stránky

Xamarin.Forms podporuje následující typy stránek:

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage) je nejjednodušším a nejběžnějším typem stránky. Nastavte [ `Content` ](xref:Xamarin.Forms.ContentPage.Content) vlastnost do jediné [ `View` ](views.md) objekt, který je nejčastěji [ `Layout` ](layouts.md) například [ `StackLayout` ](layouts.md#stackLayout), [ `Grid` ](layouts.md#grid), nebo [ `ScrollView` ](layouts.md#scrollView).<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.ContentPage) | [![Příklad ContentPage](pages-images/ContentPage.png "ContentPage příklad")](pages-images/ContentPage-Large.png#lightbox "příklad ContentPage")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) spravuje dvě podokna informace. Nastavte [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) vlastnost obecně zobrazení seznamu nebo v nabídce. Nastavte [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) vlastnost stránky zobrazující vybranou položku ze stránky předlohy. [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) Vlastnost řídí, jestli je viditelný na stránce nebo podrobností.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.MasterDetailPage) / [průvodce](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![Příklad MasterDetailPage](pages-images/MasterDetailPage.png "MasterDetailPage příklad")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) s [použití modelu code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Řídí navigaci mezi nimi další stránky s využitím architektury založené na zásobníku. Při použití navigace stránky v aplikaci, instance na domovské stránce by měly být předány konstruktoru `NavigationPage` objektu.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.NavigationPage) / [průvodce](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [ukázkový 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/), [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/), a [3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![Příklad NavigationPage](pages-images/NavigationPage.png "NavigationPage příklad")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) s [kód = za](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) je odvozen od abstraktní [ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1) třídy a umožňuje navigace mezi podřízené stránky s využitím karet. Nastavte [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children) na kolekci stránek nebo sadu [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) vlastností do kolekce datových objektů a [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) Vlastnost [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) popisující, jak má být vizuálně reprezentována každého objektu.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.TabbedPage) / [průvodce](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [ukázkový 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/) a [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![Příklad TabbedPage](pages-images/TabbedPage.png "TabbedPage příklad")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) je odvozen od abstraktní [ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1) třídy a umožňuje navigace mezi podřízené stránky prostřednictvím potažením prstem. Nastavte [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children) vlastnost kolekce [ `ContentPage` ](#contentPage) objekty nebo nastavte [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) vlastností do kolekce datových objektů a [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) vlastnost [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) popisující, jak má být vizuálně reprezentována každého objektu.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.CarouselPage) / [průvodce](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [ukázkový 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/) a [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![Příklad CarouselPage](pages-images/CarouselPage.png "CarouselPage příklad")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) Zobrazí obsah celé obrazovky pomocí šablony ovládacího prvku a je základní třídou pro [ `ContentPage` ](#contentPage).<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.TemplatedPage) / [Průvodce](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Příklad TemplatedPage](pages-images/TemplatedPage.png "TemplatedPage příklad")](pages-images/TemplatedPage.png "TemplatedPage příklad") |
|     |     |

## <a name="related-links"></a>Související odkazy

- [Úvod do Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Ukázka Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentace k rozhraní Xamarin.Forms API](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
