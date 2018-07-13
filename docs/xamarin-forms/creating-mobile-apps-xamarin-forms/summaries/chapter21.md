---
title: Souhrn kapitole 21. Transformace
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitole 21. Transformace'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 885a281d57a8ec83a949eba199667ea95679c5eb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998285"
---
# <a name="summary-of-chapter-21-transforms"></a>Souhrn kapitole 21. Transformace

Xamarin.Forms zobrazení se zobrazí na obrazovce v umístění a velikost dáno jeho nadřazeným prvkem, který je obvykle `Layout` nebo `Layout<View>` odvozených děl na základě. *Transformace* je funkce Xamarin.Forms, která úpravou tohoto umístění, velikost nebo dokonce orientace.

Xamarin.Forms podporuje tři typy transformací:

- *Překlad* &mdash; prvek posunout vodorovně nebo svisle
- *Škálování* &mdash; změnit velikost elementu
- *Otočení* &mdash; zapnout element kolem bodu nebo osy

Xamarin.Forms je škálování isotropic; To má vliv na šířku a výšku jednotně. Otočení je podporované jak na povrchu dvojrozměrné na obrazovce a v 3D prostoru. Neexistuje žádná transformace zkosení (nebo velkému) a žádná transformace zobecněný matice.

Transformace jsou podporované s osmi vlastností typu `double` určené `VisualElement` třídy:

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)
- [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)
- [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)
- [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)
- [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)
- [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

Všechny tyto vlastnosti se zálohují na vlastnosti umožňující vazbu. Může být terčem datové vazby a styl. [**Kapitola 22. Animace** ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) ukazuje, jak tyto vlastnosti lze animovat, ale některé ukázky v této kapitole ukazují, jak lze animovat pomocí Xamarin.Forms [časovače](~/xamarin-forms/platform/device.md#Device_StartTimer).

Transformace vlastnosti ovlivňují pouze jak se vykreslí element a proveďte *není* ovlivňují, jak je element vnímané v rozložení.

## <a name="the-translation-transform"></a>Překlad transformace

Nenulové hodnoty [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) a [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) vlastnosti posunutí elementu vodorovně nebo svisle.

[ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) program umožňuje experimentovat s těmito vlastnostmi se dvěma `Slider` prvky, které řídí `TranslationX` a `TranslationY` vlastnosti `Frame`. Transformace ovlivní také všechny podřízené objekty, které `Frame`.

### <a name="text-effects"></a>Textových efektů

Jedno společné použití vlastnosti překladu je posun mírně vykreslování textu. To je patrné [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) vzorku:

[![Trojitá snímek textu posune](images/ch21fg03-small.png "textu posune")](images/ch21fg03-large.png#lightbox "posouvá Text")

Jiné efekt je k vykreslení více kopií `Label` tak, aby připomínaly 3D blok, jak je ukázáno v [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) vzorku.

### <a name="jumps-and-animations"></a>Přeskočí a animace

[ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) Ukázka používá překladu pro přesun `Button` pokaždé, když je klepnutí, ale je primárním cílem prokázat, že `Button` přijímá vstup uživatele v umístění kde tlačítko se vykreslí.

[ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) vzorek je podobný, ale používá časovač pro animaci `Button` z jednoho místa do jiného.

## <a name="the-scale-transform"></a>Transformace měřítka

[ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Transformace můžete zvýšit nebo snížit velikost vykreslené elementu. Výchozí hodnota je 1. Hodnota 0 způsobí, že element byla neviditelná. Záporné hodnoty způsobit elementu, který chcete se zdají být otočený o 180 stupňů. `Scale` Vlastnost nemá vliv `Width` nebo `Height` vlastnosti daného elementu. Tyto hodnoty zůstávají stejné.

Můžete experimentovat s `Scale` pomocí vlastnosti [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) vzorku.

[ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) příklad ukazuje rozdíl mezi animace `Scale` vlastnost `Button` a animace `FontSize` vlastnost. `FontSize` Vlastnost ovlivňuje způsob, jakým `Button` vnímané v rozložení; `Scale` vlastnost nepodporuje.

[ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) ukázka vypočítá `Scale` vlastnost, která se použije pro `Label` element, aby byl co největší při stále přizpůsobení v rámci stránky.

### <a name="anchoring-the-scale"></a>Ukotvení stupnice

Škálovat v předchozích ukázkách tři prvky mají všechny zvýší nebo sníží velikost vzhledem ke středu elementu. Jinými slovy element zvětší nebo zmenší velikost stejné ve všech směrech. Pouze bod v centru elementu, který zůstane ve stejném umístění při škálování.

Můžete změnit centrum škálovat tak, že nastavíte [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) a [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) vlastnosti. Tyto vlastnosti jsou relativní vzhledem k elementu samotného. Pro `AnchorX`, hodnota 0 odkazuje na levou stranu elementu a 1 odkazuje na pravé straně. Podobně jako pro `AnchorY`, 0 je horní a dolní je 1. Obě vlastnosti mají výchozí hodnoty 0,5, což je centru.

[ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) ukázky můžete experimentovat `AnchorX` a `AnchorY` vlastnosti stejně jako `Scale` vlastnost.

V Iosu pomocí jiné než výchozí hodnoty `AnchorX` a `AnchorY` vlastnosti je obecně kompatibilní s změny orientace telefonu.

## <a name="the-rotation-transform"></a>Transformace rotace

[ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) Vlastnost zadány ve stupních a určuje otočení po směru hodinových ručiček kolem bodu elementu určené `AnchorX` a `AnchorY`. [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) umožňuje vyzkoušet tyto tři vlastnosti.

### <a name="rotated-text-effects"></a>Otočený text efekty

[ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) ukázka demonstruje matematických potřeba nakreslit kruh pomocí 64 malý otočen `BoxView` elementy.

[ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) ukázka zobrazí více `Label` otočit elementy se stejným textový řetězec vypadá jako paprsky.

[ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) ukázka zobrazí textový řetězec, který se zobrazí při zabalení v kruhu.

### <a name="an-analog-clock"></a>Analogové hodiny

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna obsahuje [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) třídu, která vypočítá úhly pro rukou hodin. Aby se zabránilo závislostí platformy v ViewModel třídě používá `Task.Delay` místo časovače pro vyhledání nový `DateTime` hodnotu.

Také součástí **Xamarin.FormsBook.Toolkit** je [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) třídu, která implementuje `IValueConverter` a slouží k Zakulacení druhého úhlu na nejbližší sekundu.

[ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) používá tři otáčení `BoxView` prvky k vykreslení analogové hodiny.

[ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) používá `BoxView` pro více rozsáhlou grafikou, včetně značek označí kolem rozpoznávání tváře hodin a předá to otočit na malou vzdálenost od konce:

[![Trojitá snímek BoxView hodiny](images/ch21fg17-small.png "obdobu jmenovek hodiny pro rozpoznávání tváře")](images/ch21fg17-large.png#lightbox "obdobu jmenovek hodiny pro rozpoznávání tváře")

Kromě toho [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) třídy v **Xamarin.FormsBook.Toolkit** způsobí, že druhé straně se trochu vrátit než si řekneme, počkáme tady a pak přejděte zpět do správné polohy.

### <a name="vertical-sliders"></a>Svislé posuvníky?

[ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) ukázka ukazuje, že `Slider` prvků může být otočenou o 90 stupňů a fungují i. Je však obtížné pozici tyto otočen `Slider` prvky vzhledem k tomu, že v rozložení, stále se zdají být horizontální.

## <a name="3d-ish-rotations"></a>Dánština 3D otáčení

[ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX) Vlastnost se zobrazí na otočit element kolem 3D osy x tak, aby se horní a dolní část elementu zdá se, že přesun směrem k nebo mimo prohlížeč. Podobně platí [ `RotationY` ](xref:Xamarin.Forms.VisualElement.RotationY) zdá se, že otočit element okolo osy y, aby levé a pravé straně elementu, vypadá to, že pro přesun směrem k nebo mimo prohlížeč.

`AnchorX` Vlastnost ovlivňuje `RotationY` , ale ne `RotationX`. `AnchorY` Vlastnost ovlivňuje `RotationX` , ale ne `RotationY`. Můžete experimentovat s [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) ukázka prozkoumat interakce tyto vlastnosti.

3D souřadnicový systém implikována v Xamarin.Forms je levou. Pokud přejdete ukazováčkem vaše levém ve směru X rostoucí koordinuje (napravo) a střední prstu ve směru Y rostoucí koordinuje (dolů), pak body thumb vaše směr zvyšované souřadnice Z (mimo obrazovky).

Také pro všechny tři osy, pokud bod vlevo jezdce v směr zvyšované hodnoty, pak křivky prsty označuje směru kladné otáčení úhlů.



## <a name="related-links"></a>Související odkazy

- [Kapitola 21 celý text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [Ukázky kapitole 21](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
