---
title: Souhrn kapitole 26. Vlastní rozložení
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitole 26. Vlastní rozložení'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: bdd86595c3c0805d50241eac3a131a50656a9985
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156584"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Souhrn kapitole 26. Vlastní rozložení

Xamarin.Forms zahrnuje několik tříd odvozených z [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout`, a
* `RelativeLayout`.

Tato kapitola popisuje, jak vytvořit vlastní třídy, které jsou odvozeny z `Layout<View>`.

## <a name="an-overview-of-layout"></a>Základní informace o rozložení

Neexistuje žádné centralizovaného systému, který zpracovává rozložení Xamarin.Forms. Každý prvek je odpovědností co vlastní velikost by měla být a samotné vykreslování v konkrétní oblasti.

### <a name="parents-and-children"></a>Nadřazené a podřízené položky

Každý element, který obsahuje podřízené položky je odpovědná za umístění tyto podřízené objekty v rámci samotného. Je nadřazené položky, která určuje, co velikost jejích potomků by měla být založena na velikosti má k dispozici a velikost podřízené chce mít.

### <a name="sizing-and-positioning"></a>Změna velikosti a polohování

Rozložení začíná v horní části stránky z vizuálního stromu se stránkou a pak pokračuje přes všechny větve. Nejdůležitější veřejnou metodu v rozložení je [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) určené `VisualElement`. Každý element, který je nadřazený objekt jiných prvků volání `Layout` pro každý z jejích podřízených poskytnout podřízené, velikost a umístění vzhledem k samotné ve formě [ `Rectangle` ](xref:Xamarin.Forms.Rectangle) hodnotu. Tyto `Layout` volání šířila vizuálního stromu.

Volání `Layout` je vyžadován pro element, který má na obrazovce a způsobí, že vlastnosti jen pro čtení, následující nastavení. Jsou v souladu s `Rectangle` předaný metodě:

- [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds) typu `Rectangle`
- [`X`](xref:Xamarin.Forms.VisualElement.X) typu `double`
- [`Y`](xref:Xamarin.Forms.VisualElement.Y) typu `double`
- [`Width`](xref:Xamarin.Forms.VisualElement.Width) typu `double`
- [`Height`](xref:Xamarin.Forms.VisualElement.Height) typu `double`

Před verzí `Layout` volání, `Height` a `Width` mají hodnoty mock &ndash;1.

Volání `Layout` také aktivuje volání na následující chráněné metody:

- [`SizeAllocated`](xref:Xamarin.Forms.VisualElement.SizeAllocated(System.Double,System.Double)), který volá
- [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated(System.Double,System.Double)), který se dá přepsat.

Nakonec se aktivuje následující událost:

- [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

`OnSizeAllocated` Je přepsána metoda `Page` a `Layout`, které jsou jenom dvě třídy v Xamarin.Forms, která může mít podřízené objekty. Volání přepsaných metod

- [`UpdateChildrenLayout`](xref:Xamarin.Forms.Page.UpdateChildrenLayout) pro `Page` vy a [ `UpdateChildrenLayout` ](xref:Xamarin.Forms.Layout.UpdateChildrenLayout) pro `Layout` odvozené konfigurace, které volá
- [`LayoutChildren`](xref:Xamarin.Forms.Page.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) pro `Page` vy a [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) pro `Layout` vy.

`LayoutChildren` pak zavolá `Layout` pro každý z podřízené objekty daného elementu. Pokud má alespoň jeden podřízený prvek nový `Bounds` nastavení, a aktivuje následující událost:

- [`LayoutChanged`](xref:Xamarin.Forms.Page.LayoutChanged) pro `Page` vy a [ `LayoutChanged` ](xref:Xamarin.Forms.Layout.LayoutChanged) pro `Layout` odvozené konfigurace

### <a name="constraints-and-size-requests"></a>Omezení a požadavky na velikost

Pro `LayoutChildren` inteligentně volat `Layout` na všech jejích potomků, musíte znát *upřednostňované* nebo *požadované* velikost pro podřízené položky. Proto volání `Layout` pro každou podřízenou položku obecně předchází volání

- [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))

Po publikování knihy, `GetSizeRequest` byl zastaralý a nahradí – metoda

- [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))

`Measure` Metoda přizpůsobuje [ `Margin` ](xref:Xamarin.Forms.View.Margin) vlastnost a zahrnuje argument typu [ `MeasureFlag` ](xref:Xamarin.Forms.MeasureFlags), který má dva členy:

- [`IncludeMargins`](xref:Xamarin.Forms.MeasureFlags.IncludeMargins)
- [`None`](xref:Xamarin.Forms.MeasureFlags.None) tak, aby nezahrnovala okraje

Pro mnoho prvků `GetSizeRequest` nebo `Measure` nativní velikost elementu získává z jeho zobrazovací jednotky. Obě metody mají parametry pro šířku a výšku *omezení*. Například `Label` budou používat omezení šířky určit, jak zabalit více řádků textu.

Obě `GetSizeRequest`a `Measure` vrátit hodnotu typu [ `SizeRequest` ](xref:Xamarin.Forms.SizeRequest), která má dvě vlastnosti:

- [`Request`](xref:Xamarin.Forms.SizeRequest.Request) typu `Size`
- [`Minimum`](xref:Xamarin.Forms.SizeRequest.Minimum) typu `Size`

Velmi často se tyto dvě hodnoty stejné a `Minimum` hodnotu lze obvykle ignorovat.

`VisualElement` Definuje také chráněné metody podobné `GetSizeRequest` , která je volána z `GetSizeRequest`:

- [`OnSizeRequest`](xref:Xamarin.Forms.VisualElement.OnSizeRequest(System.Double,System.Double)) Vrátí `SizeRequest` hodnota

Tato metoda je nyní zastaralé a nahradit:

- [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))

Každá třída, která je odvozena z `Layout` nebo `Layout<T>` musí přepsat `OnSizeRequest` nebo `OnMeasure`. To je, kde rozložení třídy určuje vlastní velikost, která je obecně podle velikosti jeho podřízené položky, která se získá voláním `GetSizeRequest` nebo `Measure` na podřízené objekty. Před a po volání `OnSizeRequest` nebo `OnMeasure`, `GetSizeRequest` nebo `Measure` provádí úpravy na základě následujících vlastností:

- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)typu `double`, má vliv `Request` vlastnost `SizeRequest`
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) typu `double`, má vliv `Request` vlastnost `SizeRequest`
- [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest) typu `double`, má vliv `Minimum` vlastnost `SizeRequest`
- [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest) typu `double`, má vliv `Minimum` vlastnost `SizeRequest`

### <a name="infinite-constraints"></a>Nekonečné omezení

Omezení argumenty předané `GetSizeRequest` (nebo `Measure`) a `OnSizeRequest` (nebo `OnMeasure`) může být nekonečné (například hodnoty `Double.PositiveInfinity`). Ale `SizeRequest` vrátilo tyto metody nemůže obsahovat nekonečné dimenze.

Nekonečné omezení znamenat, že požadovaná velikost by měly odrážet fyzická velikost prvku. Svislé `StackLayout` volání `GetSizeRequest` (nebo `Measure`) na jeho podřízené objekty s omezením nekonečné výšku. Volá vodorovný zásobníku rozložení `GetSizeRequest` (nebo `Measure`) na jeho podřízené objekty s omezením neomezenou šířku. `AbsoluteLayout` Volání `GetSizeRequest` (nebo `Measure`) na jeho podřízené objekty s neomezenou šířku a výšku omezení.

### <a name="peeking-inside-the-process"></a>Prohlížení uvnitř procesu

[ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) zobrazí omezení a velikost vyžádat informace pro jednoduché rozložení.

## <a name="deriving-from-layoutview"></a>Odvozování z rozložení<View>

Vlastní rozložení třídy je odvozen z `Layout<View>`. Má dva odpovědnosti:

- Přepsat `OnMeasure` volat `Measure` na podřízené položky všechna rozložení. Vrátí požadovaná velikost pro samotný rozložení
- Přepsat `LayoutChildren` volat `Layout` na podřízené objekty všechny rozložení

`for` Nebo `foreach` smyčky v tato přepsání přeskočte všech podřízených jehož `IsVisible` je nastavena na `false`.

Volání `OnMeasure` není zaručena. `OnMeasure` nebude volat, pokud se nadřazený prvek rozložení se kterými se řídí velikost na rozložení (například rozložení, který vyplní stránka). Z tohoto důvodu `LayoutChildren` nelze spoléhat na podřízené velikosti zjišťovala během `OnMeasure` volání. Velmi často `LayoutChildren` zavolat samotného `Measure` na podřízené položky na rozložení, nebo můžete implementovat nějaký druh velikost ukládání do mezipaměti logiku (pro prodiskutována později).

### <a name="an-easy-example"></a>Příklad jednoduché

[ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) obsahuje vzorek je zjednodušená [ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) třídy a ukázka jeho použití.

### <a name="vertical-and-horizontal-positioning-simplified"></a>Vodorovné a svislé umístění zjednodušená

Jedna z úloh, která `VerticalStack` musí provádět spadá `LayoutChildren` přepsat. Metoda používá dítěte `HorizontalOptions` vlastnost určit, jak na pozici v rámci jeho pozice v podřízené `VerticalStack`. Místo toho můžete volat statickou metodu [ `Layout.LayoutChildIntoBoundingRect` ](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle)). Tato metoda volá `Measure` na podřízené a používá svůj `HorizontalOptions` a `VerticalOptions` vlastnosti, které chcete umístit podřízené v rámci určeného obdélníku.

### <a name="invalidation"></a>Zrušení

Změna vlastnosti elementu často ovlivňuje, jak se zobrazuje tento prvek v rozložení. Rozložení musí být zneplatněné k aktivaci nového rozložení.

`VisualElement` Definuje chráněná metoda [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure), což obecně volá obslužná rutina změny vlastnosti libovolné umožňujících vazbu vlastnosti jehož změna ovlivňuje velikost prvku. `InvalidateMeasure` Aktivuje se metoda [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) událostí.

`Layout` Třída definuje podobně jako chráněnou metodu s názvem [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout), který `Layout` odvozených děl na základě by měly volat pro všechny změny, který má vliv jak umístění a velikosti své podřízené objekty.

### <a name="some-rules-for-coding-layouts"></a>Některá pravidla pro kódování rozložení

1. Vlastnosti definované `Layout<T>` odvozené by měl být založená na vlastnosti umožňující vazbu a obslužné rutiny změny vlastnosti by měly volat `InvalidateLayout`.

2. A `Layout<T>` by měly přepsat jeho odvozených děl, která definuje připojené vlastnosti umožňující vazbu [ `OnAdded` ](xref:Xamarin.Forms.Layout`1.OnAdded*) pro přidání obslužné rutiny změny vlastnosti na podřízené a [ `OnRemoved` ](xref:Xamarin.Forms.Layout`1.OnRemoved*) odebrat Obslužná rutina. By měla obslužná rutina zkontrolujte, zda změny v těchto připojené vlastnosti umožňující vazbu a reagovat voláním `InvalidateLayout`.

3. A `Layout<T>` by měly přepsat jeho odvozených děl, která implementuje mezipaměť podřízené velikostí `InvalidateLayout` a [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) a vymažte její mezipaměť při volání těchto metod.

### <a name="a-layout-with-properties"></a>Rozložení s vlastnostmi

[ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) předpokládá, že jsou všechny jeho podřízené objekty stejné velikosti a přesahuje šířku ovládacího prvku podřízené položky z jednoho řádku (nebo sloupce) na další. Definuje `Orientation` vlastnost jako `StackLayout`, a `ColumnSpacing` a `RowSpacing` vlastnosti, jako je `Grid`, a ukládá do mezipaměti podřízené velikosti.

[ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) ukázkový vloží `WrapLayout` v `ScrollView` pro zobrazování fotografií akcie.

### <a name="no-unconstrained-dimensions-allowed"></a>Žádné neomezeným dimenze povoleny!

[ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna je určená zobrazíte všechny jeho podřízené objekty v rámci samotného. Proto nelze zacházet s neomezeným dimenzí a vyvolá výjimku, pokud nebude nalezen jeden.

[ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) ukázce `UniformGridLayout`:

[![Trojitá snímek Mřížka fotografií](images/ch26fg08-small.png "Uniform rozložení mřížky")](images/ch26fg08-large.png#lightbox "Uniform rozložení mřížky")

### <a name="overlapping-children"></a>Překrývající se podřízené objekty

A `Layout<T>` odvozených děl na základě může dojít k překrytí své podřízené objekty. Ale podřízené objekty jsou vykreslovány v jejich pořadí v `Children` kolekce a ne pořadí, ve kterém jejich `Layout` metody jsou volány.

`Layout` Třída definuje dvě metody, které umožňují přesunout podřízené v rámci kolekce:

- [`LowerChild`](xref:Xamarin.Forms.Layout.LowerChild(Xamarin.Forms.View)) Přesunout podřízenou na začátek kolekce
- [`RaiseChild`](xref:Xamarin.Forms.Layout.RaiseChild(Xamarin.Forms.View)) Přesunout podřízenou na konec kolekce

Pro překrývající se děti podřízené položky na konec kolekce vizuálně zobrazují nad podřízené objekty na začátku kolekce.

[ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna definuje připojené vlastnosti k určení pořadí vykreslování a tím umožní jeden z jeho podřízené položky, který se má zobrazit nad rámec ostatních. [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) ukázce toto:

[![Trojitá snímek mřížky souboru karta studenta](images/ch26fg10-small.png "překrývající se děti rozložení")](images/ch26fg10-large.png#lightbox "překrývající se děti rozložení")

### <a name="more-attached-bindable-properties"></a>Další připojené vlastnosti umožňující vazbu

[ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna definuje připojené s možností vazby vlastnosti k určení dvou `Point` hodnoty a Hodnota thickness a manipuluje s `BoxView` prvků, které se podobají řádky.

[ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) ukázka, která používá pro kreslení 3D krychle.

### <a name="layout-and-layoutto"></a>Rozložení a LayoutTo

A `Layout<T>` můžete volat odvozených děl na základě `LayoutTo` spíše než `Layout` pro animaci rozložení. [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) Třídy se k tomu a [ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) ukázce ho.



## <a name="related-links"></a>Související odkazy

- [Kapitola 26 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [Ukázky kapitole 26](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [Vytvoření vlastního rozložení](~/xamarin-forms/user-interface/layouts/custom.md)
