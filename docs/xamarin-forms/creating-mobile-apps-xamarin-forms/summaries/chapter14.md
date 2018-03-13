---
title: "Shrnutí kapitoly 14. Absolutní rozložení"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 394e1722c79bac5f034e9ad88eb1fed7e5090f8c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-14-absolute-layout"></a>Shrnutí kapitoly 14. Absolutní rozložení

Jako `StackLayout`, [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) je odvozena z `Layout<View>` a dědí `Children` vlastnost. `AbsoluteLayout` implementuje rozložení systém, který vyžaduje programátory k určení pozice své podřízené objekty a volitelně jejich velikost. Je zadána pozice levého horního rohu podřízených relativně k levém horním rohu `AbsoluteLayout` v jednotkách nezávislé na zařízení. `AbsoluteLayout` implementuje také přímo úměrná umístění a velikost funkce.

`AbsoluteLayout` považuje se za speciální rozložení systému použije jenom v případě, že programátorů použít velikost na podřízené objekty (například `BoxView` elementy) nebo pokud velikost elementu nemá vliv, umístění jiné podřízené objekty. `HorizontalOptions` a `VerticalOptions` vlastnosti mít žádný vliv na podřízené objekty `AbsoluteLayout`.

Tato kapitola také představuje důležitou součást *přidružené vazbu vlastnosti* které umožňují vlastnosti definované v jedné třídy (v tomto případě `AbsoluteLayout`) být připojené k jiné třídě (podřízenou `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>AbsoluteLayout v kódu

Můžete přidat podřízené `Children` kolekce `AbsoluteLayout` přes standardní [ `Add` ](https://developer.xamarin.com/api/member/System.Collections.Generic.ICollection%3CT%3E.Add/p/T/) metoda, ale `AbsoluteLayout` také poskytuje rozšířená [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Rectangle/Xamarin.Forms.AbsoluteLayoutFlags/) Metoda, která umožňuje určit [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/). Jiné [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Point/) metoda vyžaduje, jenom [ `Point` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Point/), v takovém případě neomezeným podřízený objekt a velikosti sám sebe.

Můžete vytvořit `Rectangle` hodnotu s [konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/System.Double/System.Double/System.Double/System.Double/) čtyři hodnoty & #x 2014; které vyžaduje první dvě označující pozice levého horního rohu podřízených relativně k nadřazenému a další dva označující velikost dítěte. Nebo můžete použít [konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/Xamarin.Forms.Point/Xamarin.Forms.Size/) vyžadující `Point` a [ `Size` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/) hodnotu.

Tyto `Add` metody je ukázán v [ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo), které pozic `BoxView` elementů pomocí `Rectangle` hodnoty a `Label` elementu s použitím právě `Point` hodnotu.

[ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) ukázkové používá 32 `BoxView` elementy pro vytvoření chessboard vzorku. Poskytuje program `BoxView` velikost elementů pevně hranaté 35 jednotky. `AbsoluteLayout` Má jeho `HorizontalOptions` a `VerticalOptions` nastavena na `LayoutOptions.Center`, který spustí `AbsoluteLayout` tak, aby měl celková velikost hranaté 280 jednotky.

## <a name="attached-bindable-properties"></a>Připojené vlastnosti vazbu

Je také možné nastavit pozice a volitelně velikost podřízenou `AbsoluteLayout` po byl přidán do `Children` kolekce s použitím statickou metodu [ `AbsoluteLayout.SetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutBounds/p/Xamarin.Forms.BindableObject/Xamarin.Forms.Rectangle/). První argument je podřízeným; druhý `Rectangle` objektu. Můžete určit, že podřízená velikostí samotné vodorovně nebo svisle nastavením hodnoty šířky a výšky na [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) konstantní.

[ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) ukázkové PUT `AbsoluteLayout` v `ContentView` s `SizeChanged` obslužná rutina volat `AbsoluteLayout.SetLayoutBounds` na všechny podřízené objekty tak, aby byly co největší.  

Připojená vlastnost vazbu, `AbsoluteLayout` definuje je statické pole jen pro čtení typu `BindableProperty` s názvem [ `AbsoluteLayout.LayoutBoundsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/). Statické `AbsoluteLayout.SetLayoutBounds` voláním je implementována metoda `SetValue` na podřízených s `AbsoluteLayout.LayoutBoundsProperty`. Podřízená obsahuje slovník, ve kterém jsou uloženy připojená vazbu vlastnost a její hodnotu. Během rozložení `AbsoluteLayout` tuto hodnotu můžete získat voláním [ `AbsoluteLayout.GetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutBounds/p/Xamarin.Forms.BindableObject/), které je implementováno s `GetValue` volání.

## <a name="proportional-sizing-and-positioning"></a>Přímo úměrná velikosti a rozmístění

`AbsoluteLayout` implementuje přímo úměrná velikosti a rozmístění funkce. Třída definuje druhý přidružená vlastnost vazbu, [ `LayoutFlagsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/), pomocí související statických metod [ `AbsoluteLayout.SetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutFlags/p/Xamarin.Forms.BindableObject/Xamarin.Forms.AbsoluteLayoutFlags/) a [ `AbsoluteLayout.GetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutFlags/p/Xamarin.Forms.BindableObject/).

Argument `AbsoluteLayout.SetLayoutFlags` a vrátí hodnotu, která `AbsoluteLayout.GetLayoutFlags` je hodnota typu [ `AbsoluteLayoutFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/), výčet s následující členy:

- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.None/) (rovna 0)
- [`XProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.XProportional/) (1)
- [`YProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.YProportional/) (2)
- [`PositionProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional/) (3)
- [`WidthProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional/) (4)
- [`HeightProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional/) (8)
- [`SizeProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional/) (12)
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.All/) (\xFFFFFFFF)

Tato nastavení u C# bitový operátor OR můžete kombinovat.

Sadou tyto příznaky určité vlastnosti `Rectangle` rozložení hranice struktura používaná k umístění a velikost podřízená vyhodnocovány úměrně.

Když `WidthProportional` nastavený příznak, `Width` hodnota 1 znamená, že podřízená šířku stejné jako `AbsoluteLayout`. Podobný postup se používá k výšce.

Velikost úměrná umístění bere v úvahu. Když `XProportional` nastavený příznak, `X` vlastnost `Rectangle` rozložení hranice je úměrná. Hodnota 0 znamená, že podřízená levého kraje je nastavený na levém okraji `AbsoluteLayout`, ale na pozici 1 znamená, že její pravý okraj je nastavený na pravý okraj `AbsoluteLayout`, není nad rámec podle pravé hrany `AbsoluteLayout` jako může expec t. `X` Vlastnost 0,5 centra vodorovně v podřízených `AbsoluteLayout`.

[ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) příklad znázorňuje použití úměrná velikosti a umístění.

## <a name="working-with-proportional-coordinates"></a>Práce přímo úměrná souřadnice

V některých případech je jednodušší zamyslet nad přímo úměrná jinak než jak jsou implementované v umístění `AbsoluteLayout`. Chcete pracovat přímo úměrná souřadnice kde `X` vlastnost 1 umisťuje vůči pravému okraji levého okraje dítěte (nikoli pravého okraje) `AbsoluteLayout`.

Tento alternativní umísťovací systém lze volat "zlomkové podřízené souřadnice." Můžete převést z zlomkové podřízené souřadnice rozložení hranice požadované pro `AbsoluteLayout` pomocí následujícího vzorce:

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

[ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) příklad znázorňuje to.

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout a XAML

Můžete použít `AbsoluteLayout` v jazyce XAML a nastavte vlastnosti připojené vazbu na podřízené objekty daného `AbsoluteLayout` pomocí hodnoty atributu `AbsoluteLayout.LayoutBounds` a `AbsoluteLayout.LayoutFlags`. Tento postup je znázorněn v [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) a [ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) ukázky. Druhé program obsahuje 32 `BoxView` elementů, ale používá implicitní `Style` , který obsahuje `AbsoluteLayout.LayoutFlags` vlastnost zachovat kód dolů minimální.

Atribut v jazyce XAML, která se skládá z název třídy, tečku a název vlastnosti je *vždy* ve připojené vazbu vlastnosti.

## <a name="overlays"></a>Překryvy

Můžete použít `AbsoluteLayout` vytvořit *překrytí*, který možná obsahuje stránku s dalšími kontrolami chránit uživatele před interakci s normální ovládací prvky na stránce. 

[ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) ukázka ukazuje tato technika a také ukazuje [ `ProgressBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/), zobrazuje v rozsahu, který program dokončení úloha.

## <a name="some-fun"></a>Některé fun

[ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) ukázka zobrazuje aktuální čas s hodnotou display simulované matice 5 x 7 tečkou. Každý bod je `BoxView` (existují zde 228 je) velikost a umístění na `AbsoluteLayout`.

[![Trojitá snímek obrazovky tečkou matice hodiny](images/ch14fg08-small.png "tečkou matice hodiny")](images/ch14fg08-large.png#lightbox "tečkou matice hodiny")

[ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) program animuje dva `Label` objekty kolísají vodorovně a svisle po obrazovce.



## <a name="related-links"></a>Související odkazy

- [Úplný text 14 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [Ukázky kapitoly 14](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
