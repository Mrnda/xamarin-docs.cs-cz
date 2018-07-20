---
title: Souhrn kapitoly 14. Absolutní rozložení
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitola 14. Absolutní rozložení'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 07774bb5d63b8c9fb9c48192744d383b37f64900
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156645"
---
# <a name="summary-of-chapter-14-absolute-layout"></a>Souhrn kapitoly 14. Absolutní rozložení

Stejně jako `StackLayout`, [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout) je odvozena z `Layout<View>` a dědí `Children` vlastnost. `AbsoluteLayout` implementuje systém rozložení, který vyžaduje programátor k určení pozic jeho podřízené položky a volitelně, jejich velikosti. Pozice je určená levém horním rohu podřízeného vzhledem k levého horního rohu `AbsoluteLayout` v jednotkách nezávislých na zařízení. `AbsoluteLayout` také implementuje proporcionální umístění a velikosti funkce.

`AbsoluteLayout` by měly být považovány za zvláštní účely rozložení systému se použije jenom v případě, že může často znamenat výrazný programátor velikost na podřízené objekty (například `BoxView` elementy) nebo pokud velikost prvku nemá vliv na umístění ostatní podřízené objekty. `HorizontalOptions` a `VerticalOptions` vlastnosti nemají žádný vliv na podřízené objekty `AbsoluteLayout`.

Tato kapitola také zavádí důležité funkce *připojené vlastnosti umožňující vazbu* , která umožňují vlastnosti definované v jedné třídy (v tomto případě `AbsoluteLayout`) bude připojený k jiné třídy (podřízený `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>AbsoluteLayout v kódu

Můžete přidat podřízené `Children` kolekce `AbsoluteLayout` pomocí standardní [ `Add` ](xref:System.Collections.Generic.ICollection`1.Add*) metody, ale `AbsoluteLayout` poskytuje také rozšířené [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Rectangle/Xamarin.Forms.AbsoluteLayoutFlags/) Metoda, která umožňuje určit [ `Rectangle` ](xref:Xamarin.Forms.Rectangle). Jiné [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Point/) metoda vyžaduje, jenom [ `Point` ](xref:Xamarin.Forms.Point), v takovém případě neomezeným podřízený objekt a velikosti samotného.

Můžete vytvořit `Rectangle` hodnotu [konstruktor](xref:Xamarin.Forms.Rectangle.%23ctor(System.Double,System.Double,System.Double,System.Double)) , která vyžaduje čtyři hodnoty &mdash; první dva označující polohu levého horního rohu podřízené relativně k nadřazenému a další dva, která velikost dítěte. Nebo můžete použít [konstruktor](xref:Xamarin.Forms.Rectangle.%23ctor(Xamarin.Forms.Point,Xamarin.Forms.Size)) , která vyžaduje `Point` a [ `Size` ](xref:Xamarin.Forms.Size) hodnotu.

Tyto `Add` metody je ukázán v [ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo), které pozice `BoxView` prvky pomocí `Rectangle` hodnoty a `Label` elementu s použitím pouze `Point` hodnotu.

[ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) ukázkový použití 32 `BoxView` prvky k vytvoření vzorku chessboard. Poskytuje program `BoxView` velikost prvků pevně zakódovaná čtverec 35 jednotky. `AbsoluteLayout` Má jeho `HorizontalOptions` a `VerticalOptions` nastavena na `LayoutOptions.Center`, které způsobí, že `AbsoluteLayout` mít celková velikost čtverec 280 jednotky.

## <a name="attached-bindable-properties"></a>Připojené vlastnosti umožňující vazbu

Je také možné nastavit umístění a volitelně velikost podřízenou `AbsoluteLayout` po přidání do `Children` kolekce pomocí statické metody [ `AbsoluteLayout.SetLayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds(Xamarin.Forms.BindableObject,Xamarin.Forms.Rectangle)). První argument je podřízeným; Druhým je `Rectangle` objektu. Můžete určit, že podřízené velikosti samotného vodorovně nebo svisle nastavením hodnoty šířky a výšky na [ `AbsoluteLayout.AutoSize` ](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) konstantní.

[ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) ukázkový vloží `AbsoluteLayout` v `ContentView` s `SizeChanged` obslužná rutina volat `AbsoluteLayout.SetLayoutBounds` na všechny podřízené objekty tak, aby byly co největší.  

Připojená vlastnost podporující vazby, který `AbsoluteLayout` definuje je statické pole jen pro čtení typu `BindableProperty` s názvem [ `AbsoluteLayout.LayoutBoundsProperty` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty). Statické `AbsoluteLayout.SetLayoutBounds` metoda je implementována pomocí volání `SetValue` podřízených s `AbsoluteLayout.LayoutBoundsProperty`. Podřízené obsahuje slovník, ve kterém jsou uložené připojená vlastnost podporující vazby a její hodnotu. Během rozložení `AbsoluteLayout` tuto hodnotu lze získat voláním [ `AbsoluteLayout.GetLayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutBounds(Xamarin.Forms.BindableObject)), které je implementováno s `GetValue` volání.

## <a name="proportional-sizing-and-positioning"></a>Proporční Změna velikosti a polohování

`AbsoluteLayout` implementuje přímo úměrná velikosti a polohování funkce. Třída definuje druhý připojená vlastnost podporující vazby, [ `LayoutFlagsProperty` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty), s související statické metody [ `AbsoluteLayout.SetLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutFlags(Xamarin.Forms.BindableObject,Xamarin.Forms.AbsoluteLayoutFlags)) a [ `AbsoluteLayout.GetLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutFlags(Xamarin.Forms.BindableObject)).

Argument `AbsoluteLayout.SetLayoutFlags` a návratová hodnota `AbsoluteLayout.GetLayoutFlags` je hodnota typu [ `AbsoluteLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayoutFlags), výčet s následující členy:

- [`None`](xref:Xamarin.Forms.AbsoluteLayoutFlags.None) (roven 0)
- [`XProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.XProportional) (1)
- [`YProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.YProportional) (2)
- [`PositionProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional) (3)
- [`WidthProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional) (4)
- [`HeightProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional) (8)
- [`SizeProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional) (12)
- [`All`](xref:Xamarin.Forms.AbsoluteLayoutFlags.All) (\xFFFFFFFF)

Můžete kombinovat s C# bitového operátoru OR.

Sadou tyto příznaky určitých vlastností objektu `Rectangle` rozložení hranice struktura používaná k umístění a velikost podřízené proporcionálně interpretovány.

Když `WidthProportional` je příznak nastaven, `Width` hodnota 1 znamená, že podřízená je stejnou šířku jako `AbsoluteLayout`. Podobný přístup se používá pro výšku.

Umístění přímo úměrná velikosti bere v úvahu. Když `XProportional` je příznak nastaven, `X` vlastnost `Rectangle` hranice rozložení je přímo úměrná. Hodnota 0 znamená, že podřízené levým edge je umístěn na levém okraji `AbsoluteLayout`, ale pozice 1 znamená, že pravý okraj prvku je umístěn na pravé straně `AbsoluteLayout`, není nad rámec pravého okraje `AbsoluteLayout` jak může expec t. `X` Vlastnost 0,5 centra vodorovně v podřízené `AbsoluteLayout`.

[ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) ukázka demonstruje použití přímo úměrná velikosti a umístění.

## <a name="working-with-proportional-coordinates"></a>Práce s proporcionální souřadnice

V některých případech je snazší představit proporcionální jinak, než jak je implementován v umístění `AbsoluteLayout`. Budete pravděpodobně chtít pracovat přímo úměrná souřadnice kde `X` vlastnost 1 umístí na pravém okraji levého okraje podřízené (spíše než pravého okraje) `AbsoluteLayout`.

Toto alternativní umístění schéma může být volána "coordinates desetinné podřízené." Můžete převést ze souřadnice desetinné podřízené rozložení hranice, vyžaduje se pro `AbsoluteLayout` pomocí těchto vzorců:

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

[ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) příklad ukazuje to.

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout a XAML

Můžete použít `AbsoluteLayout` v XAML a připojené vlastnosti umožňující vazbu na podřízené objekty daného `AbsoluteLayout` pomocí hodnoty atributu `AbsoluteLayout.LayoutBounds` a `AbsoluteLayout.LayoutFlags`. To je patrné [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) a [ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) ukázky. Druhý program obsahuje 32 `BoxView` elementů, ale používá implicitní `Style` , který obsahuje `AbsoluteLayout.LayoutFlags` vlastnost uchovávat značky na minimum.

Atribut v XAML, který se skládá z názvu třídy, tečku a název vlastnosti je *vždy* připojené vlastnosti umožňující vazbu.

## <a name="overlays"></a>Překrytí

Můžete použít `AbsoluteLayout` k vytvoření *překrytí*, které možná zahrnuje stránky s jinými ovládacími prvky pro ochranu uživateli hlasovou interakci s normální ovládací prvky na stránce.

[ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) ukázka demonstruje tento postup a také ukazuje [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar), zobrazuje v rozsahu, do kterého byla dokončena programu úloha.

## <a name="some-fun"></a>Trochu zábavy

[ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) ukázka zobrazí aktuální čas s Simulovaná 5 x 7 jehličkové tiskárny zobrazení. Každé tečce je `BoxView` (existují 228 z nich) velikost a umístění na `AbsoluteLayout`.

[![Trojitá snímek obrazovky s orientací hodiny](images/ch14fg08-small.png "orientací hodiny")](images/ch14fg08-large.png#lightbox "orientací hodiny")

[ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) program animuje dvě `Label` objekty kolísají vodorovně a svisle na obrazovce.



## <a name="related-links"></a>Související odkazy

- [Kapitola 14 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [Ukázky kapitola 14](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
- [Připojené vlastnosti](~/xamarin-forms/xaml/attached-properties.md)
