---
title: Shrnutí kapitoly 21. Transformace
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d9c135191fc8143ca932fec5129f1b35af4b29b4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-21-transforms"></a>Shrnutí kapitoly 21. Transformace

Xamarin.Forms se zobrazí na obrazovce v umístění a velikost dáno jeho nadřazeným prvkem, který je obvykle `Layout` nebo `Layout<View>` odvozených. *Transformace* je funkce Xamarin.Forms, která můžete upravit, že umístění, velikost nebo i orientace.

Xamarin.Forms podporuje tři základní typy transformací:

- *Překlad* &mdash; posunutí elementu vodorovně nebo svisle
- *Škálování* &mdash; změnit velikost elementu
- *Otočení* &mdash; zapnout element kolem bodu nebo osy

Xamarin.Forms je škálování isotropic; ovlivňuje šířku a výšku jednotně. Otočení je podporováno jak na povrchu dvourozměrná obrazovky a v 3D prostoru. Neexistuje žádná transformace zkosení (nebo velkému) a žádná transformace zobecněný matice.

Transformace jsou podporovány s osmi vlastnosti typu `double` definované `VisualElement` třídy:

- [`TranslationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)
- [`TranslationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)
- [`Scale`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)
- [`Rotation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)
- [`RotationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/)
- [`RotationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/)
- [`AnchorX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/)
- [`AnchorY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)

Tyto vlastnosti jsou zajišťované vazbu vlastnosti. Tyto může být cíle datové vazby a ve. [**Kapitola 22. Animace** ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) ukazuje, jak může být animovaný tyto vlastnosti, ale některé ukázky v této kapitole ukazují, jak je platformě Xamarin.Forms pomocí animace [časovače](~/xamarin-forms/platform/device.md#Device_StartTimer).

Transformace vlastnosti vliv pouze jak vykreslení elementu a proveďte *není* vliv na tom, jak je element vnímaný v rozložení.

## <a name="the-translation-transform"></a>Překlad transformace

Nenulové hodnoty [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) a [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) vlastnosti posunutí elementu vodorovně nebo svisle.

[ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) programu můžete experimentovat s těmito vlastnostmi se dvěma `Slider` elementy, které řídí `TranslationX` a `TranslationY` vlastnosti `Frame`. Transformovat také ovlivní všechny podřízené objekty této `Frame`.

### <a name="text-effects"></a>Textové efekty

Jeden běžné překlad vlastnosti slouží k posunutí mírně vykreslování textu. Tento postup je znázorněn v [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) ukázka:

[![Trojitá snímek obrazovky posune Text](images/ch21fg03-small.png "posune Text")](images/ch21fg03-large.png#lightbox "posune Text")

Další efekt je k vykreslení více kopií `Label` tak, aby připomínaly 3D bloku, jak je předvedeno v [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) ukázka.

### <a name="jumps-and-animations"></a>Přeskočí a animací

[ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) Ukázka používá překlad přesunout `Button` vždy, když je stisknuté, ale primární je cílem ukažte, že `Button` obdrží uživatelský vstup v umístění kde tlačítko je vykreslen.

[ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) ukázka je podobný ale používá časovač pro animaci `Button` z jednoho bodu do jiného.

## <a name="the-scale-transform"></a>Transformace škálování

[ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Transformace můžete zvýšit nebo snížit velikost vykreslené elementu. Výchozí hodnota je 1. Hodnota 0 způsobí, že element byla neviditelná. Záporné hodnoty způsobit elementu, který chcete zobrazí jako otočený o 180 stupňů. `Scale` Vlastnost nemá vliv `Width` nebo `Height` vlastnosti elementu. Tyto hodnoty zůstávají stejné.

Můžete vyzkoušet `Scale` pomocí vlastnosti [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) ukázka.

[ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) ukázka představuje rozdíl mezi animace `Scale` vlastnost `Button` a animace `FontSize` vlastnost. `FontSize` Vlastnost ovlivňuje jak `Button` je zaznamenatelného v rozložení; `Scale` vlastnost neexistuje.

[ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) ukázka vypočítá `Scale` vlastnost, která se použije pro `Label` elementu, který chcete vytvořit co největší při stále hodí v rámci dané stránky.

### <a name="anchoring-the-scale"></a>Ukotvení měřítka

Elementy škálovat v předchozích ukázkách tři všechny zvětšit nebo zmenšit velikost relativně k centru elementu. Jinými slovy element zvyšuje nebo snižuje velikost stejná ve všech pokynů. Pouze k bodu v centru elementu zůstává ve stejném umístění během změny velikosti.

Můžete změnit na střed škálování nastavením [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) a [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) vlastnosti. Tyto vlastnosti jsou relativní vzhledem k elementu samotného. Pro `AnchorX`, hodnota 0 odkazuje na levé straně elementu a 1 odkazuje na pravé straně. Podobně jako pro `AnchorY`, 0 je horní a dolní je 1. Obě vlastnosti mají výchozí hodnoty 0,5, což je centru.

[ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) ukázka můžete experimentovat s `AnchorX` a `AnchorY` vlastnosti a taky `Scale` vlastnost.

V systému iOS, pomocí jiné než výchozí hodnoty `AnchorX` a `AnchorY` vlastnosti je obecně kompatibilní s phone orientaci změny.

## <a name="the-rotation-transform"></a>Otočení transformace

[ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Vlastnost je zadána ve stupních a určuje otočení po směru hodinových ručiček kolem bodu elementu definované `AnchorX` a `AnchorY`. [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) můžete experimentovat s tyto tři vlastnosti.

### <a name="rotated-text-effects"></a>Účinky otočený text

[ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) příklad znázorňuje potřebné k vykreslení kruh pomocí 64 jen nepatrnou otáčet Matematika `BoxView` elementy.

[ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) ukázka zobrazuje více `Label` vypadá jako koncových otočený o elementy do jednoho textového řetězce.

[ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) ukázka zobrazuje textový řetězec, který se zobrazí při zabalení v kruh.

### <a name="an-analog-clock"></a>Analogovým hodiny

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna obsahuje [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) třídu, která vypočítá úhly pro do nesprávných rukou hodiny. Aby se zabránilo platformy závislostí v ViewModel, používá třída `Task.Delay` místo časovač pro hledání novou `DateTime` hodnotu.

Také součástí **Xamarin.FormsBook.Toolkit** je [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) třídu, která implementuje `IValueConverter` a slouží k zaokrouhlení druhého úhlu nejbližší druhou.

[ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) používá tři otáčení `BoxView` elementy k vykreslení analogovým hodiny.

[ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) používá `BoxView` pro více rozsáhlou grafikou, včetně značek označí kolem tučné hodiny a předá to otáčení malé vzdálenosti z jejich konce:

[![Trojitá snímek obrazovky hodiny BoxView](images/ch21fg17-small.png "analogovým hodiny vzhled")](images/ch21fg17-large.png#lightbox "vzhled analogovým hodiny")

Kromě toho [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) třídy v **Xamarin.FormsBook.Toolkit** způsobí, že sekundu dlaně se objeví na stáhnout trochu před přechod dopředu a pak přesuňte zpět do správné polohy.

### <a name="vertical-sliders"></a>Svislé jezdce?

[ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) příklad ukazuje, který `Slider` prvky můžete otočený o 90 stupňů a fungují i. Je však obtížné umístit tyto otáčet `Slider` prvky, protože v rozložení stále objeví horizontální.

## <a name="3d-ish-rotations"></a>Dánština 3D otočení

[ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) Vlastnost pravděpodobně otočit element kolem 3D osy x tak, aby se horní a dolní elementu se zdá, že přesuňte směrem nebo nebývají v prohlížeči. Podobně [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) zdá se, že otočit element kolem osy y aby levé a pravé straně elementu, zdá se, že chcete přesunout směrem nebo mimo prohlížeč.

`AnchorX` Vlastnost ovlivňuje `RotationY` ale ne `RotationX`. `AnchorY` Vlastnost ovlivňuje `RotationX` ale ne `RotationY`. Můžete vyzkoušet [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) ukázková interakce tyto vlastnosti prozkoumat.

3D systém souřadnic implikované Xamarin.Forms je levou rukou. Pokud jste bodu ukazováčkem levé ruky ve směru osy X se zvyšující koordinuje (napravo) a prstu střední ve směru osy y roste koordinuje (dolů), pak bodům jezdec ve směru zvýšení souřadnice Z (mimo na obrazovce).

Také pro všechny tři osy, pokud bodu levé palec ve směru zvýšení hodnoty, pak křivku prsty udává směr otočení pro pozitivní rotační úhly.



## <a name="related-links"></a>Související odkazy

- [Úplný text 21 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [Ukázky kapitoly 21](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
