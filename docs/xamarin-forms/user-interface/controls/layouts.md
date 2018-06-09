---
title: Rozložení Xamarin.Forms
description: Rozložení Xamarin.Forms se používají k uspořádání ovládacích prvků uživatelského rozhraní do visual struktury. Tento článek obsahuje seznam rozložení součástí Xamarin.Forms.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 6e9889bf8ec748ed2034d63acfec9784d074ca44
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243087"
---
# <a name="xamarinforms-layouts"></a>Rozložení Xamarin.Forms

_Rozložení Xamarin.Forms se používají k uspořádání ovládacích prvků uživatelského rozhraní do visual struktury._

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) a [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) v Xamarin.Forms třídy jsou specializované podtypů zobrazení, které fungují jako kontejnery pro zobrazení a další rozložení. `Layout` Třída odvozená z [ `View` ](views.md). A `Layout` odvozených obvykle obsahuje logiku a nastavit umístění a velikost podřízených elementů v aplikacích Xamarin.Forms.

[![Typy rozložení Xamarin.Forms](layouts-images/layouts-sml.png "Xamarin.Forms rozložení typy")](layouts-images/layouts.png#lightbox "typy rozložení Xamarin.Forms")

Třídy, které jsou odvozeny od `Layout` je možné rozdělit do dvou kategorií:

## <a name="layouts-with-single-content"></a>Rozložení pomocí jednoho obsahu

Tyto třídy jsou odvozeny od [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/), která definuje [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) a [ `IsClippedToBounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.IsClippedToBounds/) vlastnosti.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) obsahuje jednu podřízenou, který je nastavený s [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) vlastnost. `Content` Vlastnost můžete nastavit na libovolnou `View` odvozených, včetně dalších `Layout` odvozené konfigurace. `ContentView` většinou se používá jako element strukturální a slouží jako základní třída pro [ `Frame` ](#frame).<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) | [![Příklad ContentView](layouts-images/ContentView.png "ContentView příklad")](layouts-images/ContentView-Large.png#lightbox "ContentView příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Rámec

|     |     |
| --- | --- |
| [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) Třída odvozená z [ `ContentView` ](#contentView) a zobrazí obdélníkový snímek kolem jeho podřízené. `Frame` má výchozí [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) hodnotu 20 a také definuje [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/), [ `CornerRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.CornerRadius/), a [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/)vlastnosti.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) | [![Příklad s rámečkem](layouts-images/Frame.png "rámce příklad")](layouts-images/Frame-Large.png#lightbox "příklad s rámečkem")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) je schopen posouvání její obsah. Nastavte [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) vlastnost k zobrazení nebo rozložení příliš velký, aby se vešel na obrazovce. (Obsah `ScrollView` je velmi často [ `StackLayout` ](#stackLayout).) Nastavte [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) vlastnost k označení, pokud posouvání by měla být svislé, vodorovných nebo obojí.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) / [průvodce](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Příklad ScrollView](layouts-images/ScrollView.png "ScrollView příklad")](layouts-images/ScrollView-Large.png#lightbox "ScrollView příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) Zobrazí obsah pomocí ovládacího prvku šablony, a je základní třídou pro [ `ContentView` ](#contentView).<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) / [Průvodce](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Příklad TemplatedView](layouts-images/TemplatedView.png "TemplatedView příklad")](layouts-images/TemplatedView.png#lightbox "TemplatedView příklad") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) je Správce rozložení pro šablony zobrazení, používaných v rámci [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) k označení, kde se zobrazí obsah, který je dodáván.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) / [Průvodce](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Příklad ContentPresenter](layouts-images/ContentPresenter.png "ContentPresenter příklad")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter příklad") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Rozložení s více podřízených položek

Tyto třídy jsou odvozeny od [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/).

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) umisťuje podřízených elementů v zásobníku, buď vodorovně nebo svisle na základě [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) vlastnost. [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) Vlastnost řídí mezery mezi podřízené objekty a nemá výchozí hodnotu 6.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) / [průvodce](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)| [![Příklad StackLayout](layouts-images/StackLayout.png "StackLayout příklad")](layouts-images/StackLayout-Large.png#lightbox "StackLayout příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Mřížka

|     |     |
| --- | --- |
| [`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) umisťuje jeho podřízených elementů v mřížce řádků a sloupců. Podřízená pozice je vytvářen [přidružené vlastnosti](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/), [ `Column` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/), [ `RowSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/), a [ `ColumnSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/).<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) / [průvodce](~/xamarin-forms/user-interface/layouts/grid.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Příklad mřížky](layouts-images/Grid.png "mřížky příklad")](layouts-images/Grid-Large.png#lightbox "příklad mřížky")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) podřízené elementy se umístí na konkrétní umístění relativně k jeho nadřazený objekt. Podřízená pozice je vytvářen [přidružené vlastnosti](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/) a [ `LayoutFlags` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/). `AbsoluteLayout` Je užitečné pro animace pozice zobrazení.<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) / [průvodce](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Příklad AbsoluteLayout](layouts-images/AbsoluteLayout.png "AbsoluteLayout příklad")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) s [kódu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) umisťuje podřízené elementy vzhledem k `RelativeLayout` sám sebe nebo jejich na stejné úrovni. Podřízená pozice je vytvářen [přidružené vlastnosti](~/xamarin-forms/xaml/attached-properties.md) nastavených pro objekty typu [ `Constraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/) a [ `BoundsConstraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/).<br /><br />[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) / [průvodce](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Příklad RelativeLayout](layouts-images/RelativeLayout.png "RelativeLayout příklad")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) je založena na CSS [modulu rozložení flexibilní pole](http://www.w3.org/TR/css-flexbox-1/), běžně označovaný jako _flexibilní rozložení_ nebo _flexibilního pole_. `FlexLayout` definuje vlastnosti šesti vazbu a pět přidružené vlastnosti vazbu, které umožňují podřízené objekty skládaný nebo obklopeno mnoho možností zarovnání a orientaci.<br /><br />[Dokumentaci k rozhraní API](xref:Xamarin.Forms.FlexLayout) / [průvodce](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [vzorku](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/) | [![Příklad FlexLayout](layouts-images/FlexLayout.png "FlexLayout příklad")](layouts-images/FlexLayout-Large.png#lightbox "FlexLayout příklad")<br />[Kód jazyka C# pro tuto stránku](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) / [XAML – stránka](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Související odkazy

- [Úvod k platformě Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Ukázka Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentace rozhraní API Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
