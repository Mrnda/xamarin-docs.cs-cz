---
title: Rozložení Xamarin.Forms
description: Rozložení Xamarin.Forms se používají k uspořádání ovládacích prvků uživatelského rozhraní do visual struktur. Tento článek uvádí rozložení součástí Xamarin.Forms.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 224eb2ee3958e5979382a3dc5e70110fdce51879
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994599"
---
# <a name="xamarinforms-layouts"></a>Rozložení Xamarin.Forms

_Rozložení Xamarin.Forms se používají k uspořádání ovládacích prvků uživatelského rozhraní do visual struktur._

[ `Layout` ](xref:Xamarin.Forms.Layout) a [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) třídy v Xamarin.Forms jsou specializované podtypy typu zobrazení, které fungují jako kontejnery pro zobrazení a jiné rozložení. `Layout` Třída je odvozena z [ `View` ](views.md). A `Layout` odvozených děl na základě obvykle obsahuje logiku pro nastavení pozice a velikost podřízených elementů v aplikacích Xamarin.Forms.

[![Typy rozložení Xamarin.Forms](layouts-images/layouts-sml.png "rozložení Xamarin.Forms typy")](layouts-images/layouts.png#lightbox "typy rozložení Xamarin.Forms")

Třídy, které jsou odvozeny z `Layout` je možné rozdělit do dvou kategorií:

## <a name="layouts-with-single-content"></a>Rozložení s jednoduchým obsahem

Tyto třídy odvozovat z [ `Layout` ](xref:Xamarin.Forms.Layout), která definuje [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) a [ `IsClippedToBounds` ](xref:Xamarin.Forms.Layout.IsClippedToBounds) vlastnosti.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView) obsahuje jednu podřízenou, který je nastaven s [ `Content` ](xref:Xamarin.Forms.ContentView.Content) vlastnost. `Content` Vlastnost lze nastavit na kteroukoli `View` odvozená díla, včetně dalších `Layout` vy. `ContentView` se většinou používá jako prvek strukturální a slouží jako základní třídu na [ `Frame` ](#frame).<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.ContentView) | [![Příklad ContentView](layouts-images/ContentView.png "ContentView příklad")](layouts-images/ContentView-Large.png#lightbox "ContentView příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Rámec

|     |     |
| --- | --- |
| [ `Frame` ](xref:Xamarin.Forms.Frame) Třída odvozena z [ `ContentView` ](#contentView) a zobrazí obdélníkový snímek po jeho podřízených. `Frame` Výchozí nastavení [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) hodnotu 20 a také definuje [ `OutlineColor` ](xref:Xamarin.Forms.Frame.OutlineColor), [ `CornerRadius` ](xref:Xamarin.Forms.Frame.CornerRadius), a [ `HasShadow` ](xref:Xamarin.Forms.Frame.HasShadow)vlastnosti.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.Frame) | [![Rámec příklad](layouts-images/Frame.png "rámec příklad")](layouts-images/Frame-Large.png#lightbox "rámec příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView) je schopen posouvání jeho obsah. Nastavte [ `Content` ](xref:Xamarin.Forms.ScrollView.Content) vlastnosti k zobrazení nebo rozložení příliš velký a nevejde se na obrazovce. (Obsah `ScrollView` je velmi často [ `StackLayout` ](#stackLayout).) Nastavte [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation) vlastnost umožňující označit, pokud se posouvání by měl být svislý, vodorovný nebo obojí.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.ScrollView) / [průvodce](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Příklad ScrollView](layouts-images/ScrollView.png "ScrollView příklad")](layouts-images/ScrollView-Large.png#lightbox "příklad ScrollView")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) Zobrazí obsah pomocí šablony ovládacího prvku a je základní třídou pro [ `ContentView` ](#contentView).<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.TemplatedView) / [Průvodce](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Příklad TemplatedView](layouts-images/TemplatedView.png "TemplatedView příklad")](layouts-images/TemplatedView.png#lightbox "TemplatedView příklad") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) je Správce rozložení pro zobrazení bez vizuálního vzhledu, použít v rámci [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) k označení, kde se zobrazí obsah, který je zobrazovat.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.ContentPresenter) / [Průvodce](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Příklad ContentPresenter](layouts-images/ContentPresenter.png "ContentPresenter příklad")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter příklad") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Rozložení s více podřízených položek

Tyto třídy odvozovat z [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1).

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout) umístění podřízených elementů v zásobníku, buď vodorovně nebo svisle na základě [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) vlastnost. [ `Spacing` ](xref:Xamarin.Forms.StackLayout.Spacing) Vlastnost řídí mezery mezi podřízené objekty a má výchozí hodnotu 6.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.StackLayout) / [průvodce](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)| [![Příklad StackLayout](layouts-images/StackLayout.png "StackLayout příklad")](layouts-images/StackLayout-Large.png#lightbox "StackLayout příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Mřížka

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid) umístí jeho podřízené prvky v mřížce řádků a sloupců. Podřízená pozice je označen pomocí [připojených vlastností](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](xref:Xamarin.Forms.Grid.RowProperty), [ `Column` ](xref:Xamarin.Forms.Grid.ColumnProperty), [ `RowSpan` ](xref:Xamarin.Forms.Grid.RowSpanProperty), a [ `ColumnSpan` ](xref:Xamarin.Forms.Grid.ColumnSpanProperty).<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.Grid) / [průvodce](~/xamarin-forms/user-interface/layouts/grid.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Grid – příklad](layouts-images/Grid.png "Grid – příklad")](layouts-images/Grid-Large.png#lightbox "Grid – příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) umístění podřízených elementů na konkrétní umístění relativně k nadřazenému. Podřízená pozice je označen pomocí [připojených vlastností](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) a [ `LayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty). `AbsoluteLayout` Je užitečné pro animace umístění zobrazení.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.AbsoluteLayout) / [průvodce](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Příklad AbsoluteLayout](layouts-images/AbsoluteLayout.png "AbsoluteLayout příklad")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) s [použití modelu code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) umístění podřízených elementů vzhledem k `RelativeLayout` samotné nebo jejich na stejné úrovni. Podřízená pozice je označen pomocí [připojených vlastností](~/xamarin-forms/xaml/attached-properties.md) nastavenými na objekty typu [ `Constraint` ](xref:Xamarin.Forms.Constraint) a [ `BoundsConstraint` ](xref:Xamarin.Forms.Constraint).<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.RelativeLayout) / [průvodce](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Příklad RelativeLayout](layouts-images/RelativeLayout.png "RelativeLayout příklad")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) je založen na šabloně stylů CSS [flexibilní modul rozložení pole](http://www.w3.org/TR/css-flexbox-1/), běžně označované jako _flex rozložení_ nebo _flex-box_. `FlexLayout` definuje šest umožňujících vazbu vlastnosti a pět připojené umožňujících vazbu vlastnosti, které umožňují podřízené objekty skládaný nebo zabaleny s mnoha možnostmi zarovnání a orientaci.<br /><br />[Dokumentace k rozhraní API](xref:Xamarin.Forms.FlexLayout) / [průvodce](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/) | [![Příklad FlexLayout](layouts-images/FlexLayout.png "FlexLayout příklad")](layouts-images/FlexLayout-Large.png#lightbox "FlexLayout příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) / [stránky XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Související odkazy

- [Úvod do Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Ukázka Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentace k rozhraní Xamarin.Forms API](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
