---
title: Souhrn kapitoly 27. Vlastní nástroji pro vykreslování
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 7a117373a11a057afabbad3c0c6005a9c2a49c3a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-27-custom-renderers"></a>Souhrn kapitoly 27. Vlastní nástroji pro vykreslování

Element Xamarin.Forms jako `Button` vykreslením s tlačítkem na příslušnou platformu zapouzdřené v třídy s názvem `ButtonRenderer`.  Tady je [verzi iOS `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), [Android verzi `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs)a [verzi prostředí Windows Runtime `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ButtonRenderer.cs).

Tato kapitola popisuje, jak můžete napsat vlastní nástroji pro vykreslování vytvářet vlastní zobrazení, které mapují na objekty specifické pro platformu.

## <a name="the-complete-class-hierarchy"></a>Hierarchie tříd dokončení

Nejsou sedm sestavení, které obsahují kód Xamarin.Forms specifické pro platformu.
Zdroj lze zobrazit na webu GitHub pomocí těchto odkazů:

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (velmi malé)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.WinRT** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT) (větší než další tři)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)
- [**Xamarin.Forms.Platform.WinRT.Tablet**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Tablet)
- [**Xamarin.Forms.Platform.WinRT.Phone**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Phone)

[ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) ukázka zobrazuje hierarchie tříd pro sestavení, které jsou platné pro provádění platformu.

Můžete si všimnout důležité třídu s názvem `ViewRenderer`. Toto je třída, kterou odvozujete od při vytváření vykreslovací modul specifické pro platformu. Tam ve třech různých verzích, protože je vázaný na tento systém zobrazení cílové platformy:

IOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L26) obsahuje obecné argumenty:

- `TView` omezená na [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` omezená na [`UIKit.UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)

Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L14) obsahuje obecné argumenty:

- `TView` omezená na [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` omezená na [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

Prostředí Windows Runtime [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ViewRenderer.cs#L12) má jinak s názvem obecné argumenty:

- `TElement` omezená na [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeElement` omezená na [`Windows.UI.Xaml.FrameworkElement`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx)

Při zápisu vykreslovací modul, můžete se být odvození třídy z `View`a pak zápis více `ViewRenderer` třídy, jeden pro každou podporovanou platformu. Každá specifické pro platformu implementace bude odkazovat nativní třídy odvozené od typu určíte jako `TNativeView` nebo `TNativeElement` parametr.

## <a name="hello-custom-renderers"></a>Hello, vlastní nástroji pro vykreslování!

[ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) program odkazuje na vlastní zobrazení s názvem `HelloView` v jeho [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) třídy.

[ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) Třída je součástí **HelloRenderers** projektu a jednoduše je odvozena z `View`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) Třídy v **HelloRenderers.iOS** projektu je odvozena z `ViewRenderer<HelloView, UILabel>`. V `OnElementChanged` přepsání, vytvoří nativní aplikace pro iOS `UILabel` a volání `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) Třídy v **HelloRenderers.Droid** projektu je odvozena z `ViewRenderer<HelloView, TextView>`. V `OnElementChanged` přepsání, dojde Android `TextView` a volání `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) Třídy v **HelloRenderers.UWP** a další projekty Windows je odvozena z `ViewRenderer<HelloView, TextBlock>`. V `OnElementChanged` přepsání, vytvoří Windows `TextBlock` a volání `SetNativeControl`.

Všechny `ViewRenderer` obsahovat odvozené konfigurace `ExportRenderer` atribut na úrovni sestavení, které přidružuje `HelloView` se konkrétní `HelloViewRenderer` třídy. Toto je, jak Xamarin.Forms vyhledává nástroji pro vykreslování v projektech pro jednotlivé platformy:

[![Trojitá snímek obrazovky zobrazení Hello](images/ch27fg02-small.png "nástroji pro vykreslování vlastní")](images/ch27fg02-large.png#lightbox "vlastní nástroji pro vykreslování")

## <a name="renderers-and-properties"></a>Nástroji pro vykreslování a vlastnosti

Další sadu pro vykreslování implementuje kreslení elipsy a se nachází v různých projektů [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) řešení.

[ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) Třída je v **Xamarin.FormsBook.Platform** platformy. Třída je podobná `BoxView` a definuje právě jednu vlastnost: `Color` typu `Color`.

Nástroji pro vykreslování můžou přenášet nastavena na hodnoty vlastností `View` k objektu nativní přepsáním `OnElementPropertyChanged` metoda v zobrazovací jednotky. V rámci této metody (a v rámci většinu zobrazovací jednotky) jsou k dispozici dvě vlastnosti:

- `Element`, Xamarin.Forms element
- `Control`, nativní zobrazení nebo objekt pomůcka nebo ovládací prvek

Obecné parametry, které určuje typy těchto vlastností `ViewRenderer`. V tomto příkladu `Element` je typu `EllipseView`.

`OnElementPropertyChanged` Přepsání proto přenést `Color` hodnotu `Element` do nativního `Control` objekt pravděpodobně s nějaký druh převodu. Jsou tři nástroji pro vykreslování:

- iOS: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs), které používá [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) třídu pro se třemi tečkami.
- Android: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs), které používá [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) třídu pro se třemi tečkami.
- Prostředí Windows Runtime: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), které můžete použít nativní Windows [ `Ellipse` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.ellipse.aspx) třídy.

[ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) třída zobrazí několik z těchto `EllipseView` objekty:

[![Trojitá snímek obrazovky ukázkové elipsy](images/ch27fg03-small.png "EllipseView vlastní nástroji pro vykreslování")](images/ch27fg03-large.png#lightbox "EllipseView vlastní nástroji pro vykreslování")

[ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) nedoručitelných zpráv `EllipseView` vypnout strany obrazovky.

## <a name="renderers-and-events"></a>Nástroji pro vykreslování a události

Je také možné, nástroji pro vykreslování nepřímo generovat události. [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) Třídy je podobná normální Xamarin.Forms `Slider` , ale umožní zadat počet diskrétní kroků mezi `Minimum` a `Maximum` hodnoty.

Jsou tři nástroji pro vykreslování:

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- Prostředí Windows Runtime: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

Nástroji pro vykreslování detekovat změny do ovládacího prvku nativní a pak zavolají `SetValueFromRenderer`, která odkazuje na vazbu vlastnost definovanou v `StepSlider`, změny, které způsobí `StepSlider` má provést, `ValueChanged` událostí.

[ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) příklad znázorňuje tento nový posuvníku.



## <a name="related-links"></a>Související odkazy

- [Úplný text 27 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [Ukázky kapitoly 27](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [Vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
