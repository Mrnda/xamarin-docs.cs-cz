---
title: Souhrn kapitoly 22. Animace
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 22. Animace'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b25fed9a86b82e56cb3b2bf5e3276c8ff63f4e35
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241966"
---
# <a name="summary-of-chapter-22-animation"></a>Souhrn kapitoly 22. Animace

Už víte, že můžete vytvořit své vlastní animace pomocí časovač Xamarin.Forms nebo `Task.Delay`, ale je obvykle jednodušší pomocí animace poskytované systémem Xamarin.Forms. Tři třídy implementovat tyto animací:

- [`ViewExtensions`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/), vysoké úrovně přístupu
- [`Animation`](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/), ale těžší rozmanitější
- [`AnimationExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/), nejvíce univerzální, nejnižší úroveň přístupu

Obecně platí animací cíle vlastnosti, které jsou zajišťované vazbu vlastnosti. To není povinné, ale toto jsou pouze vlastnosti, které dynamicky reagovat na změny.

Neexistuje žádné rozhraní jazyka XAML pro tyto animací, ale animací můžete integrovat do XAML pomocí technik popsaných v [ **kapitoly 23. Aktivační události a chování**](chapter23.md).

## <a name="exploring-basic-animations"></a>Zkoumání základního animace

Funkce základní animace se rozšiřující metody, které jsou součástí [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) třídy. Tyto metody platí pro libovolný objekt, který je odvozen od `VisualElement`. Nejjednodušší animací cíle transformace vlastnosti popsané v [ `Chapter 21. Transforms` ](chapter21.md).

[ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) ukazuje, jak `Clicked` obslužné rutiny události pro `Button` můžete volat [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metody rozšíření pro číselníku tlačítka na kruh.

`RotateTo` Metoda změny `Rotation` vlastnost `Button` od 0 do 360 v průběhu jedné čtvrtletí sekundu (ve výchozím nastavení). Pokud `Button` je stisknuté znovu, ale jeho neprovede žádnou akci protože `Rotation` vlastnost je již 360 stupňů.

### <a name="setting-the-animation-duration"></a>Nastavení doby trvání animace

Druhý argument `RotateTo` je doba v milisekundách. Pokud nastavená na hodnotu velké klepnutím `Button` během animace spustí novinkou animace v aktuální úhlu.

### <a name="relative-animations"></a>Relativní animace

`RelRotateTo` Metoda provádí rotaci relativní přidáním zadanou hodnotu do stávající hodnotu. Tato metoda umožňuje `Button` k být stisknuté několikrát a pro každou čas zvýší `Rotation` vlastnost o 360 stupňů.

### <a name="awaiting-animations"></a>Systém čeká na animace

Všechny metody animace v `ViewExtensions` vrátit `Task<bool>` objekty. To znamená, že můžete definovat řadu sekvenční animace pomocí `ContinueWith` nebo `await`. `bool` Dokončení vrátit hodnota je `false` Pokud animace skončil bez přerušení nebo `true` Pokud bylo zrušeno podle [ `CancelAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) metodu, která zruší všechny animace zahájili jiné metody v `ViewExtensions` , jsou nastavené u stejného elementu.

### <a name="composite-animations"></a>Složené animace

Je možné kombinovat očekávaná a očekáváno animací k vytvoření složeného animace. Jedná se o animace v `ViewExtensions` cílených `TranslatonX`, `TranslationY`, a `Scale` transformace vlastnosti:

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`ScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.ScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)

Všimněte si, že `TranslateTo` potenciálně ovlivňuje i `TranslationX` a `TranslationY` vlastnosti.

### <a name="taskwhenall-and-taskwhenany"></a>Metody Task.WhenAll a Task.WhenAny

Je také možné spravovat pomocí souběžných animací [ `Task.WhenAll` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAll%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), což signalizuje, že více úkolů všechny uzavřeny, a [ `Task.WhenAny` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAny%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), což signalizuje při první den několik úlohy uzavřelo.

### <a name="rotation-and-anchors"></a>Otočení a ukotvení

Při volání metody `ScaleTo`, `RelScaleTo`, `RotateTo`, a `RelRotateTo` metod, můžete nastavit [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) a [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) vlastnosti udávajících Centrum změny velikosti a oběh.

[ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) ukazuje tato technika obkroužením `Button` kolem center stránky.

### <a name="easing-functions"></a>Funkce usnadnění

Animace jsou obecně lineární z počáteční hodnotu na hodnotu end. Funkce usnadnění může způsobit, že animace zrychlit nebo zpomalit jejich průběhu. Poslední nepovinný argument metody animace je typu [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/), třídu, která definuje 11 statická jen pro čtení pole typu `Easing`:

- [`Linear`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/), výchozí
- [`SinIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/), [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/), a [`SinInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/)
- [`CubicIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/), [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/), a [`CubicInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/)
- [`BounceIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) A [`BounceOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/)
- [`SpringIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) A [`SpringOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/)

`In` Příponu označuje, že účinek je na začátku animační, `Out` znamená na konci, a `InOut` znamená, že je na začátku a konci animace.

[ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) příklad znázorňuje použití funkce usnadnění.

### <a name="your-own-easing-functions"></a>Vlastní funkce usnadnění

Můžete také definovat můžete vlastní funkce usnadnění předáním [ `Func<double, double>` ](https://developer.xamarin.com/api/type/System.Func%3CT,TResult%3E/) k [ `Easing` konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Easing.Easing/p/System.Func%7BSystem.Double,System.Double%7D/). `Easing` Definuje také implicitní převod z `Func<double, double>` k `Easing`. Argument pro funkci nejvýraznější je vždy v rozsahu 0 až 1 jako animace pokračuje lineárně od začátku do konce. Funkce *obvykle* vrátí hodnotu v rozsahu 0 až 1, ale může být stručně záporný nebo větší než 1 (jako je tomu u `SpringIn` a `SpringOut` funkce) nebo by mohlo způsobit narušení pravidla, pokud víte, jaké úlohy.

[ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) příklad znázorňuje vlastní nejvýraznější funkcí, a [ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) ukazuje jiné.

[ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) příklad také ukazuje vlastní nejvýraznější funkcí a také o techniku změny `AnchorX` a `AnchorY` vlastnosti v rámci posloupnost otočení animace.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna má [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) třídu, která používá vlastní zmírnit funkci, aby se pohyb sem a tam tlačítko po klepnutí. [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) příklad znázorňuje ho.

### <a name="entrance-animations"></a>Animace vstupu

Jeden typ oblíbených animace nastane, když se nejprve zobrazí stránka s. Takové animace lze spustit [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) přepsat stránky. Pro tyto animací, je nejlepší nastavit XAML pro způsob stránky zobrazí *po* animace, inicializovat a použije animaci rozložení z kódu.

[ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) ukázkové používá [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metody rozšíření pro objevovat se obsah stránky.

[ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) ukázkové používá [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) metoda rozšíření na snímek v obsahu stránce vzdálené.

[ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) ukázkové používá [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metody rozšíření pro animaci `RotationY` vlastnost. A [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metoda je také k dispozici.

### <a name="forever-animations"></a>Navždy animace

V jiných extreme "navždy" animací spustit, dokud nebudou program je ukončen. Obecně jsou určené pro demonstrační účely.

[ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) ukázkové používá [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) animace a odhlašování objevovat dvě části textu.

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) zobrazí palindrome a pak postupně otočí jednotlivých písmena o 180 stupňů tak, aby všechny obráceným. Potom celý řetězec je stejný jako původní řetězec číst obráceně 180 stupňů.

[ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) ukázka otočí jednoduchou `BoxView` vrtulníku při obkroužení kolem středu obrazovky.

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) zásadní `BoxView` paprsky kolem středu obrazovky a pak otočí každý paprsek samotné k vytvoření zajímavých vzorců:

[![Trojitá snímek obrazovky otáčení paprsky](images/ch22fg21-small.png "otáčení paprsky")](images/ch22fg21-large.png#lightbox "otáčení paprsky")

Ale postupně zvýšení `Rotation` vlastnost elementu nemusí fungovat na dlouhou dobu, jako [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) příklad znázorňuje.

[ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) ukázkové používá [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), a [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Chcete-li působí, jako kdyby je rastrový obrázek otáčení v 3D prostoru.

### <a name="animating-the-bounds-property"></a>Animace vlastnost hranice

Jedinou metodou rozšíření v `ViewExtensions` ještě nebyla ukázán je [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/), které efektivně animuje jen pro čtení [ `Bounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) vlastnost voláním [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metoda. Tato metoda je volána normálně `Layout` odvozené konfigurace, jak se bude zabývat [ **kapitoly 26. CustomLayouts**](chapter26.md).

`LayoutTo` Metoda by měla být omezená na zvláštní účely. [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) program používá při komprimaci a rozbalte `BoxView` jako se odrazí od postranní stránky.

[ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) ukázkové používá `LayoutTo` přesunutí dlaždice na implementaci classic stavebnice 15-16, která zobrazuje kódované bitové kopie, nikoli číslem dlaždice:

[![Trojitá snímek obrazovky Xamarin Xuzzle](images/ch22fg26-small.png "Xuzzle skládankou herní")](images/ch22fg26-large.png#lightbox "Xuzzle skládankou hra")

### <a name="your-own-awaitable-animations"></a>Může používat await animace

[ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) ukázka vytvoří může používat await animace. Zásadní třídu, která se můžete vrátit `Task` objekt z metoda a signál po dokončení animace [ `TaskCompletionSource` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskCompletionSource%3CTResult%3E/).

## <a name="deeper-into-animations"></a>Hlouběji do animace

Systém animace Xamarin.Forms může být trochu matoucí. Kromě `Easing` zahrnuje třídy, systém animace `ViewExtensions`, `Animation`, a `AnimationExtension` classses.

### <a name="viewextensions-class"></a>ViewExtensions – třída

Už jste viděli [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/). Definuje devět metody, které vracejí `Task<bool>` a [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/). 7 cíle devět metody transformace vlastnosti. Další dvě jsou [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), které cílí [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) vlastnost, a [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/), který volá [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metoda.

### <a name="animation-class"></a>Animace – třída

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) Třída má [konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Animation.Animation/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Action/) pomocí pěti argumentů k definování zpětného volání a dokončení metody a parametry animace.

Podřízené animací lze přidat s [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/), a a přetížení z [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Double/System.Double/).

Objekt animace je pak spuštěna pomocí volání [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BSystem.Double,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) metoda.

### <a name="animationextensions-class"></a>AnimationExtensions – třída

[ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) Třída obsahuje metody, hlavně rozšíření. Existuje několik verzí `Animate` metoda a Obecné [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) metoda je univerzální tak, že je ve skutečnosti jenom animace funkce, je nutné.

## <a name="working-with-the-animation-class"></a>Práce s animace – třída

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) příklad ukazuje [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) třídy pomocí několika různých animace.

### <a name="child-animations"></a>Podřízené animace

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) příklad také ukazuje podřízené animací, které využívají (velmi podobné) [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) a [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/) metody.

### <a name="beyond-the-high-level-animation-methods"></a>Nad rámec metod vysoké úrovně animace

[ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) ukázka také ukazuje, jak provést animace, které jdou nad rámec směrována vlastnosti pomocí `ViewExtensions` metody. V jedním z příkladů získat sérii období delší; jiný příklad `BackgroundColor` je animovaný vlastnost.

### <a name="more-of-your-own-awaitable-methods"></a>Z vlastní může používat await metod

[ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metodu `ViewExtensions` nefunguje s [ `Easing.SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) funkce. Zastaví při nejvýraznější výstup získá vyšší než 1.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna obsahuje [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) třídy s [ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) a [ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) rozšiřující metody, které nemají tyto potíže odstranit, a také [ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) a [ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) metody pro ty zrušení animace.

[ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) ukazuje `TranslateXTo` metoda.

`MoreExtensions` Třída také obsahuje [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) metody rozšíření, která kombinuje X a Y překlad a [ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) metoda.

### <a name="implementing-a-bezier-animation"></a>Implementace animace Bézierovy křivky

Je také možné vyvíjet animace, která přesune element Bézierovy křivky v cestě. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna obsahuje [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) struktura, který zapouzdřuje Bézierovy křivky a [ `BezierTangent` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) výčtu pro řízení orientace.

[ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) Třída obsahuje [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) metoda rozšíření a [ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) metoda.

[ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) příklad znázorňuje animace element společně Beizer cestu.

## <a name="working-with-animationextensions"></a>Práce s AnimationExtensions

Jeden typ animace chybí standardní kolekce je barva animace. Problém je, že žádné správný způsob promítnout mezi dvěma `Color` hodnoty. Je možné promítnout jednotlivé hodnoty RGB, ale interpolace HSL – hodnoty stejně jako platná.

Z tohoto důvodu [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna obsahuje dvě `Color` metody animace: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)a [ `HslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188). (Také existují dvě metody zrušení: [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) a [ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

Ujistěte se, obě metody použití [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), který provede animace voláním rozsáhlé obecná [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) metoda v [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/).

[ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) příklad ukazuje použití těchto dvou typů barvu animace.

## <a name="structuring-your-animations"></a>Strukturování své animace

Někdy je užitečné express animace v jazyce XAML a použít je ve spojení s modelem MVVM. To je popsaná v další kapitoly [ **kapitoly 23. Aktivační události a chování**](chapter23.md).



## <a name="related-links"></a>Související odkazy

- [Úplný text 22 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [Kapitola 22 ukázky](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [Animace](~/xamarin-forms/user-interface/animation/index.md)
