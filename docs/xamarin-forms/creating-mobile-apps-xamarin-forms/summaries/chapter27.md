---
title: Souhrn kapitoly 27. Vlastní renderery
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 27. Vlastní renderery'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0497770909b33108eaac0fa5044e98febeb61763
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996305"
---
# <a name="summary-of-chapter-27-custom-renderers"></a>Souhrn kapitoly 27. Vlastní renderery

Element Xamarin.Forms, jako `Button` je vykreslen pomocí tlačítka pro konkrétní platformu zapouzdřené v třídě s názvem `ButtonRenderer`.  Tady je [verze iOS `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), [verze Androidu `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs)a [verze prostředí Windows Runtime `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ButtonRenderer.cs).

Tato kapitola popisuje, jak můžete napsat vlastní renderery k vytváření vlastních zobrazení, které se mapují na objekty specifické pro platformu.

## <a name="the-complete-class-hierarchy"></a>Hierarchie úplnou třídu.

Existuje sedm sestavení, které obsahují kód specifický pro platformu Xamarin.Forms.
Zdroj lze zobrazit na Githubu pomocí těchto odkazů:

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (velmi malý)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.WinRT** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT) (větší než příštích 3)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)
- [**Xamarin.Forms.Platform.WinRT.Tablet**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Tablet)
- [**Xamarin.Forms.Platform.WinRT.Phone**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Phone)

[ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) ukázka zobrazí hierarchie tříd pro sestavení, které jsou platné pro provádění platformu.

Všimněte si důležité třídu s názvem `ViewRenderer`. Toto je třída, které odvozujete od při vytváření rendereru pro konkrétní platformu. To existuje ve třech různých verzích, protože se váže k zobrazení systému cílové platformy:

IOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L26) má obecných argumentů:

- `TView` s omezením [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` s omezením [`UIKit.UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)

Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L14) má obecných argumentů:

- `TView` s omezením [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` s omezením [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

Modul Windows Runtime [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ViewRenderer.cs#L12) jinak s názvem obecných argumentů:

- `TElement` s omezením [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeElement` s omezením [`Windows.UI.Xaml.FrameworkElement`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx)

Při zápisu vykreslovací modul, můžete se být odvození třídy z `View`a potom psát více `ViewRenderer` třídy, jeden pro každou podporovanou platformu. Každá implementace specifické pro platformu bude odkazovat na nativních tříd, který je odvozen z typu, který zadáte jako `TNativeView` nebo `TNativeElement` parametru.

## <a name="hello-custom-renderers"></a>Dobrý den, vlastní renderery!

[ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) program odkazuje na vlastní zobrazení s názvem `HelloView` v jeho [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) třídy.

[ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) Třída je zahrnuta v **HelloRenderers** projektu a jednoduše je odvozena z `View`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) Třídy v **HelloRenderers.iOS** projektu je odvozena z `ViewRenderer<HelloView, UILabel>`. V `OnElementChanged` přepsání, vytváření nativních aplikací pro iOS `UILabel` a volání `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) Třídy v **HelloRenderers.Droid** projektu je odvozena z `ViewRenderer<HelloView, TextView>`. V `OnElementChanged` přepsání, vytvoří aplikaci pro Android `TextView` a volání `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) Třídy v **HelloRenderers.UWP** a jiné projekty Windows je odvozena z `ViewRenderer<HelloView, TextBlock>`. V `OnElementChanged` přepsání, vytvoří Windows `TextBlock` a volání `SetNativeControl`.

Všechny `ViewRenderer` obsahují odvozené `ExportRenderer` atribut na úrovni sestavení, které přidružuje `HelloView` třídy s konkrétní `HelloViewRenderer` třídy. Je to, jak Xamarin.Forms vyhledává renderery v projektech pro jednotlivé platformy:

[![Trojitá snímek obrazovky zobrazení Hello](images/ch27fg02-small.png "vlastními Renderery")](images/ch27fg02-large.png#lightbox "vlastními Renderery")

## <a name="renderers-and-properties"></a>Renderery a vlastnosti

Další sadu renderery implementuje kreslení elipsy a je umístěn v různých projektech [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) řešení.

[ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) Hodina může začít **Xamarin.FormsBook.Platform** platformy. Třída je podobně jako `BoxView` a definuje pouze jednu vlastnost: `Color` typu `Color`.

Zobrazovací jednotku můžete přenášet hodnoty vlastností nastavené na `View` nativní objekt tak, že přepíšete `OnElementPropertyChanged` metoda ve zobrazovací jednotky. V této metodě (a v rámci většinu zobrazovací jednotky) jsou k dispozici dvě vlastnosti:

- `Element`, Xamarin.Forms element
- `Control`, nativní zobrazení nebo widget nebo ovládací prvek objektu

Typy těchto vlastností jsou určeny parametry obecného `ViewRenderer`. V tomto příkladu `Element` je typu `EllipseView`.

`OnElementPropertyChanged` Přepsání proto můžou přenášet `Color` hodnotu `Element` na nativní `Control` objekt pravděpodobně s nějaký druh převodu. Jsou tři zobrazovací jednotku:

- iOS: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs), který používá [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) třídy elipsy.
- Android: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs), který používá [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) třídy elipsy.
- Windows Runtime: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), které můžete použít nativní Windows [ `Ellipse` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.ellipse.aspx) třídy.

[ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) třída zobrazí několik z nich `EllipseView` objekty:

[![Trojitá snímek obrazovky ukázkové aplikace Elipsa](images/ch27fg03-small.png "EllipseView vlastními Renderery")](images/ch27fg03-large.png#lightbox "EllipseView vlastními Renderery")

[ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) nedoručitelných zpráv `EllipseView` vypnuto stranách obrazovky.

## <a name="renderers-and-events"></a>Renderery a události

Je také možné, renderery nepřímo generovat události. [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) Třída je podobně jako normální Xamarin.Forms `Slider` , ale umožňuje zadat několik samostatných kroků mezi `Minimum` a `Maximum` hodnoty.

Jsou tři zobrazovací jednotku:

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- Windows Runtime: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

Zobrazovací jednotku zjištění změn do nativního ovládacího prvku a následně zavolat `SetValueFromRenderer`, odkazuje na vlastnost s vazbou definované v `StepSlider`, změnu způsobující `StepSlider` dovoleno vyvolávat `ValueChanged` událostí.

[ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) ukázka demonstruje tento nový posuvník.



## <a name="related-links"></a>Související odkazy

- [Kapitola 27 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [Ukázky kapitoly 27](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [Vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
