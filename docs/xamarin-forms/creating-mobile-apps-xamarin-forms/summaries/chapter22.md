---
title: Souhrn kapitola 22. Animace
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitola 22. Animace'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c4d92784654db8e566b41c8270dbe2095bd28b94
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156597"
---
# <a name="summary-of-chapter-22-animation"></a>Souhrn kapitola 22. Animace

Viděli jsme, že můžete vytvořit vlastní animací pomocí časovače Xamarin.Forms nebo `Task.Delay`, ale je obvykle jednodušší díky animace poskytované systémem Xamarin.Forms. Tři třídy implementovat tyto animace budete:

- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions), vysoké úrovně přístupu
- [`Animation`](xref:Xamarin.Forms.Animation), ale obtížnější větší variabilitu.
- [`AnimationExtension`](xref:Xamarin.Forms.AnimationExtensions), nejvíce univerzální nejnižší úrovně přístupu

Obecně platí animací cílové vlastnosti, které se zálohují na vlastnosti umožňující vazbu. Toto není povinné, ale toto jsou pouze vlastnosti, které dynamicky reagovat na změny.

Neexistuje žádné rozhraní XAML pro tyto animace budete ale animace můžete integrovat do XAML pomocí technik popsaných v [ **kapitoly 23. Triggery a chování**](chapter23.md).

## <a name="exploring-basic-animations"></a>Zkoumání základní animace

Základní animace funkce jsou součástí rozšiřující metody [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) třídy. Tyto metody použít na libovolný objekt, který je odvozen od `VisualElement`. Nejjednodušší animace cílit transformace vlastností popsaných v [ `Chapter 21. Transforms` ](chapter21.md).

[ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) ukazuje, jak `Clicked` obslužné rutiny události pro `Button` můžete volat [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metodu rozšíření, abyste mohli spustit tlačítko v kruhu.

`RotateTo` Metoda změny `Rotation` vlastnost `Button` od 0 do 360 v průběhu jedna čtvrtina druhé (ve výchozím nastavení). Pokud `Button` klepnutí znovu, ale to nemá žádný účinek vzhledem k tomu, `Rotation` vlastnost je již 360 stupňů.

### <a name="setting-the-animation-duration"></a>Nastavení doby trvání animace

Druhý argument `RotateTo` je doba v milisekundách. Pokud nastavena na hodnotu velké klepnutím `Button` během animace spustí novou animace počínaje aktuální úhel.

### <a name="relative-animations"></a>Relativní animace

`RelRotateTo` Metoda provádí relativní otočení tak, že přidáte na existující hodnotu zadanou hodnotu. Tato metoda umožňuje `Button` řinou, více než jednou a pro každý čas zvýší `Rotation` vlastnost 360 stupňů.

### <a name="awaiting-animations"></a>Čeká se na animace

Všechny metody animace v `ViewExtensions` vrátit `Task<bool>` objekty. To znamená, že můžete definovat řadu sekvenční animací `ContinueWith` nebo `await`. `bool` Dokončení vrátit hodnotu `false` Pokud animace dokončena bez přerušení nebo `true` Pokud byla zrušena [ `CancelAnimation` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) metodu, která zruší všechny animace iniciovaných jiné metody v `ViewExtensions` , které jsou nastavené u stejného elementu.

### <a name="composite-animations"></a>Složený animace

Je možné kombinovat nejočekávanějších a očekáváno animace k vytvoření složeného animace. Jedná se o animací `ViewExtensions` , které se zaměřují `TranslatonX`, `TranslationY`, a `Scale` transformace vlastnosti:

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))

Všimněte si, že `TranslateTo` potenciálně ovlivňuje `TranslationX` a `TranslationY` vlastnosti.

### <a name="taskwhenall-and-taskwhenany"></a>Metody Task.WhenAll a Task.WhenAny

Je také možné spravovat současně animací [ `Task.WhenAll` ](xref:System.Threading.Tasks.Task.WhenAll*), což signalizuje, že více úkolů všechny uzavřeny, a [ `Task.WhenAny` ](xref:System.Threading.Tasks.Task.WhenAny*), což znamená, když první z několika úkoly se uzavřelo.

### <a name="rotation-and-anchors"></a>Otočení a ukotvení

Při volání `ScaleTo`, `RelScaleTo`, `RotateTo`, a `RelRotateTo` metody, můžete nastavit [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) a [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) vlastnosti k označení System Center měřítka a otočení.

[ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) ukazuje tuto techniku obkroužením `Button` kolem středu stránky.

### <a name="easing-functions"></a>Funkce usnadnění

Animace jsou obecně lineární z počáteční hodnotu pro koncovou hodnotu. Funkce usnadnění může způsobit, že animace k urychlení nebo zpomalit jejich průběhu. Poslední nepovinný argument pro animace metody je typu [ `Easing` ](xref:Xamarin.Forms.Easing), třídu, která definuje 11 statické pole jen pro čtení typu `Easing`:

- [`Linear`](xref:Xamarin.Forms.Easing.Linear), výchozí hodnota
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn), [ `SinOut` ](xref:Xamarin.Forms.Easing.SinOut), a [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn), [ `CubicOut` ](xref:Xamarin.Forms.Easing.CubicOut), a [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)
- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) a [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) a [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)

`In` Příponou označuje, že efekt je na začátku animace, `Out` znamená, že na konci a `InOut` znamená, že je na začátku a konci animace.

[ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) ukázka demonstruje použití funkce usnadnění.

### <a name="your-own-easing-functions"></a>Funkce usnadnění

Můžete také definovat je vlastní funkcí usnadnění předáním [ `Func<double, double>` ](xref:System.Func`2) k [ `Easing` konstruktor](xref:Xamarin.Forms.Easing.%23ctor(System.Func{System.Double,System.Double})). `Easing` Definuje také implicitní převod z `Func<double, double>` k `Easing`. Argument pro funkci přechodu je vždycky v rozsahu 0 až 1 jako animace lineárně pokračuje od začátku do konce. Funkce *obvykle* vrací hodnotu v rozmezí 0 až 1, ale může být stručně záporná nebo větší než 1 (jako je tomu u `SpringIn` a `SpringOut` funkce) nebo pravidla může poškodit, pokud víte, co děláte.

[ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) ukázce vlastní funkci uvolnění, a [ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) ukazuje jiné.

[ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) také ukázce vlastní funkci uvolnění a také zahrnuje změnu `AnchorX` a `AnchorY` vlastnosti v rámci jednoho pořadí otočení animací.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna má [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) pohyb sem a tam tlačítka, když dojde ke kliknutí na třídu, která používá vlastní usnadnění funkce. [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) ukázce ho.

### <a name="entrance-animations"></a>Animace vstupu

Oblíbené typu animace nastane, pokud se nejprve zobrazí na stránce. Takové animaci lze spustit [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) přepsat stránky. Pro tyto animace budete, je nejlepší nastavení XAML pro způsob na stránce se zobrazí *po* animace, inicializovat a animovat rozložení z kódu.

[ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) ukázkový používá [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) rozšiřující metoda, která má vyblednout v obsahu stránky.

[ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) ukázkový používá [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) metodu rozšíření k snímku v obsah stránky ze strany.

[ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) ukázkový používá [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metodu rozšíření k animace `RotationY` vlastnost. A [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metoda je také k dispozici.

### <a name="forever-animations"></a>Navždy animace

V jiných extreme "navždy" animace spustit až do ukončení programu. Obecně jsou určeny pro demonstrační účely.

[ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) ukázkový používá [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) animace, která má vyblednout dva druhy textu a oddálení.

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) zobrazí palindrom a pak postupně otočí jednotlivé písmena o 180 stupňů tak, aby všechny vzhůru nohama. Poté se celý řetězec obráceně 180stupňový rozsah s orientací přečíst stejný jako původní řetězec.

[ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) ukázka otočí jednoduchý `BoxView` vrtulníku při obkroužení kolem středu obrazovky.

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) zásadní `BoxView` paprsky kolem středu obrazovky a pak otočí každého paprsku, samotný vytvoření zajímavých vzorců:

[![Trojitá snímek otáčení paprsky](images/ch22fg21-small.png "otočení paprsky")](images/ch22fg21-large.png#lightbox "otočení paprsky")

Ale postupně zvyšuje `Rotation` vlastnost elementu nemusí fungovat v dlouhodobém horizontu, jako [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) ukázce.

[ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) ukázkový používá [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), a [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) k němu vypadá to, že jako rastrový obrázek je otáčení v 3D prostoru.

### <a name="animating-the-bounds-property"></a>Animace vlastností hranice

Jedinou metodou rozšíření v `ViewExtensions` ještě není ukázáno je [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)), což účinně animuje jen pro čtení [ `Bounds` ](xref:Xamarin.Forms.VisualElement.Bounds) vlastnost voláním [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metody. Tato metoda je obvykle volána `Layout` odvozené jako probereme v [ **kapitole 26. CustomLayouts**](chapter26.md).

`LayoutTo` Metoda by měla být omezena na zvláštní účely. [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) program používá komprese a rozšiřovat `BoxView` jako nedoručitelných zpráv vypnout okraji stránky.

[ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) ukázkový používá `LayoutTo` přesunutí dlaždice na implementaci classic 15 až 16 skládačky, který zobrazuje utajená image místo číselného dlaždice:

[![Trojitá snímek Xamarin Xuzzle](images/ch22fg26-small.png "Xuzzle skládankou hru")](images/ch22fg26-large.png#lightbox "Xuzzle skládankou hry")

### <a name="your-own-awaitable-animations"></a>Očekávatelný animace

[ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) vzorovým kódem se vytvoří očekávatelný animace. Zásadní význam třídy, která se můžete vrátit `Task` objekt z metody a signálu po dokončení animace [ `TaskCompletionSource` ](xref:System.Threading.Tasks.TaskCompletionSource`1).

## <a name="deeper-into-animations"></a>Hlouběji do animace

Animace Xamarin.Forms systému může být trochu matoucí. Kromě `Easing` zahrnuje třídy, systém animace `ViewExtensions`, `Animation`, a `AnimationExtension` classses.

### <a name="viewextensions-class"></a>Třída ViewExtensions

Seznámili jste se už [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions). Definuje devět metody, které vracejí `Task<bool>` a [ `CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)). Sedm cíle devět metody transformace vlastnosti. Další dvě [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), cíle, které [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) vlastnost, a [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)), který volá [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metody.

### <a name="animation-class"></a>Třídy animace

[ `Animation` ](xref:Xamarin.Forms.AnimationExtensions) Třída nemá [konstruktor](xref:Xamarin.Forms.Animation.%23ctor(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Action)) pomocí pěti argumentů pro definování zpětného volání a dokončení metody a parametrů animace.

Podřízené animace lze přidat pomocí [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)), [ `Insert` ](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)), [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)), a a přetížení [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Double,System.Double)).

Animace objektu je pak spuštěna voláním [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) metody.

### <a name="animationextensions-class"></a>Třída AnimationExtensions

[ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions) Třída obsahuje metody, které většinou rozšíření. Existuje několik verzí `Animate` metody a obecný [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) metoda je tak, že je ve skutečnosti pouze animace funkce, je třeba univerzální.

## <a name="working-with-the-animation-class"></a>Práce s třídou animace

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) ukázce [ `Animation` ](xref:Xamarin.Forms.Animation) třídy s použitím animací několik různých.

### <a name="child-animations"></a>Podřízené animace

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) také ukázce podřízené animace, která zjednodušují využívání (velmi podobný) [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) a [ `Insert` ](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)) metody.

### <a name="beyond-the-high-level-animation-methods"></a>Nad rámec metod základní animace

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) ukázka také ukazuje, jak provádět animace, která přesahují vlastnosti zacílené podle `ViewExtensions` metody. V jedním z příkladů získáte řadu období delší; například `BackgroundColor` animovat vlastnost.

### <a name="more-of-your-own-awaitable-methods"></a>Několik vlastních očekávatelný metod

[ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda `ViewExtensions` nefunguje při využití [ `Easing.SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) funkce. Zastaví při uvolnění výstup výše 1.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna obsahuje [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) třídy s [ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) a [ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) rozšiřující metody, které nemají tento problém, stejně jako [ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) a [ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) metody pro ty zrušení animace.

[ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) ukazuje, `TranslateXTo` metody.

`MoreExtensions` Třída také obsahuje [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) rozšiřující metoda, která kombinuje X a Y překlad a [ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) metody.

### <a name="implementing-a-bezier-animation"></a>Implementace animace Bézierovy křivky

Je také možné vyvíjet animace, který přesouvá element na cestě Bézierovy křivky. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna obsahuje [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) struktura, která zapouzdřuje Bézierovy křivky a [ `BezierTangent` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) výčet pro orientaci ovládacího prvku.

[ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) Obsahuje třídy [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) – metoda rozšíření a [ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) metody.

[ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) animace elementu cestě Beizer ukázce.

## <a name="working-with-animationextensions"></a>Práce s AnimationExtensions

Animace barev je jeden typ animace chybí standardní kolekce. Problém je, že je správný způsob, jak lze interpolovat mezi dvěma `Color` hodnoty. Je možné promítnout jednotlivé hodnoty RGB, ale interpolace HSL – hodnoty stejně jako platná.

Z tohoto důvodu [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny obsahuje dvě `Color` animace metody: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)a [ `HslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188). (Existují také dva způsoby zrušení: [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) a [ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

Ujistěte se, obě metody využívání [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), která provede animace voláním rozsáhlé Obecné [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) metoda v [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions).

[ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) ukázka znázorňuje použití tyto dva druhy barevné animace.

## <a name="structuring-your-animations"></a>Strukturování animace

Někdy je užitečné pro express animace v XAML a jejich použití ve spojení s modelem MVVM. Tento proces je popsán v následující kapitole [ **kapitoly 23. Triggery a chování**](chapter23.md).



## <a name="related-links"></a>Související odkazy

- [Kapitola 22 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [Ukázky kapitola 22](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [Animace](~/xamarin-forms/user-interface/animation/index.md)
