---
title: Shrnutí kapitoly 26. Vlastní rozložení
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 26. Vlastní rozložení'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 1c8fec34c0bc7f38d360f76122d851ae653ce15e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241168"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Shrnutí kapitoly 26. Vlastní rozložení

Xamarin.Forms zahrnuje několik třídy odvozené od třídy [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout`, a
* `RelativeLayout`.

Tato kapitola popisuje, jak vytvořit vlastní třídy, které jsou odvozeny od `Layout<View>`.

## <a name="an-overview-of-layout"></a>Přehled rozložení

Neexistuje žádný centralizované systém, který zpracovává Xamarin.Forms rozložení. Každý prvek je zodpovědný za určení toho, co vlastní velikost by měla být a způsob vykreslení sám v určité oblasti.

### <a name="parents-and-children"></a>Nadřazené a podřízené položky

Každý element, který má podřízených prvků, je zodpovědná za umístění těchto podřízené objekty v rámci samotného. Je nadřazeného objektu, který určuje, co velikost podřízené by měla být založena na velikosti má k dispozici a chce být velikost podřízený objekt.

### <a name="sizing-and-positioning"></a>Změna velikosti a rozmístění

Rozložení začne v horní části stromu visual s hledanou stránkou a potom pokračuje prostřednictvím všechny větve. Je nejdůležitější veřejná metoda v rozložení [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) definované `VisualElement`. Každý element, který je nadřazený na další prvky volání `Layout` pro každý z jejích podřízených umožnit podřízené velikosti a pozice relativně k samotné ve formě [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) hodnotu. Tyto `Layout` volání rozšíří v rámci vizuálním stromu.

Volání `Layout` je vyžadována pro element se objeví na obrazovce a způsobí, že následující vlastnosti jen pro čtení nastavení. Jsou konzistentní s `Rectangle` předaný metodě:

- [`Bounds`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) typu `Rectangle`
- [`X`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.X/) typu `double`
- [`Y`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Y/) typu `double`
- [`Width`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) typu `double`
- [`Height`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) typu `double`

Před verzí `Layout` volat, `Height` a `Width` mít imitované hodnoty &ndash;1.

Volání `Layout` také aktivuje volání následující chráněné metody:

- [`SizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.SizeAllocated/p/System.Double/System.Double/), který volá
- [`OnSizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeAllocated/p/System.Double/System.Double/), který je možné přepsat.

Nakonec se aktivuje například následující událost:

- [`SizeChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/)

`OnSizeAllocated` Je metoda přepsat `Page` a `Layout`, které jsou pouze dvě třídy v Xamarin.Forms, která může mít podřízené objekty. Volání přepsaného – metoda

- [`UpdateChildrenLayout`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.UpdateChildrenLayout()/) pro `Page` odvozené konfigurace a [ `UpdateChildrenLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.UpdateChildrenLayout()/) pro `Layout` odvozené konfigurace, které volá
- [`LayoutChildren`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) pro `Page` odvozené konfigurace a [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) pro `Layout` odvozené konfigurace.

`LayoutChildren` pak zavolá `Layout` pro všechny podřízené objekty daného elementu. Pokud má alespoň jednu podřízenou novou `Bounds` nastavení, pak je aktivována například následující událost:

- [`LayoutChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.LayoutChanged/) pro `Page` odvozené konfigurace a [ `LayoutChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Layout.LayoutChanged/) pro `Layout` odvozené konfigurace

### <a name="constraints-and-size-requests"></a>Omezení a požadavky na velikost

Pro `LayoutChildren` inteligentně volat `Layout` na všechny její podřízené položky, musí znát *upřednostňované* nebo *požadované* velikost podřízené objekty. Proto volání `Layout` pro všechny podřízené objekty jsou obecně sebou volání

- [`GetSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/)

Po publikování knihy, `GetSizeRequest` metoda byla zastaralá a nahradit

- [`Measure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)

`Measure` Může metoda vyrovnávat [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) vlastnost i argument typu [ `MeasureFlag` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MeasureFlags/), který má dva členy:

- [`IncludeMargins`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.IncludeMargins/)
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.None/) možnost Nezahrnovat okraje

Pro mnoho prvků `GetSizeRequest` nebo `Measure` nativní velikost elementu získává z jeho zobrazovací jednotky. Obě metody mít parametry pro šířku a výšku *omezení*. Například `Label` budou používat omezení šířky určit, jak zabalit více řádků textu.

Obě `GetSizeRequest`a `Measure` vrátí hodnotu typu [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/), která má dvě vlastnosti:

- [`Request`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Request/) typu `Size`
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Minimum/) typu `Size`

Velmi často tyto dvě hodnoty jsou stejné a `Minimum` hodnotu lze obvykle ignorovat.

`VisualElement` Definuje také chráněná metoda, která je podobná `GetSizeRequest` která je volána z `GetSizeRequest`:

- [`OnSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeRequest/p/System.Double/System.Double/) Vrátí `SizeRequest` hodnota

Tuto metodu je nyní zastaralé a nahradí:

- [`OnMeasure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)

Každá třída, která je odvozena z `Layout` nebo `Layout<T>` musí přepsat `OnSizeRequest` nebo `OnMeasure`. Toto je, kde třídu rozložení určuje vlastní velikost, která je obecně založena na velikosti jeho podřízených položek, které se získá voláním `GetSizeRequest` nebo `Measure` na podřízené objekty. Před a po volání `OnSizeRequest` nebo `OnMeasure`, `GetSizeRequest` nebo `Measure` provádí úpravy podle následující vlastnosti:

- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)typu `double`, má vliv `Request` vlastnost `SizeRequest`
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) typu `double`, má vliv `Request` vlastnost `SizeRequest`
- [`MinimumWidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumWidthRequest/) typu `double`, má vliv `Minimum` vlastnost `SizeRequest`
- [`MinimumHeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumHeightRequest/) typu `double`, má vliv `Minimum` vlastnost `SizeRequest`

### <a name="infinite-constraints"></a>Nekonečné omezení

Předaný argumentů omezení `GetSizeRequest` (nebo `Measure`) a `OnSizeRequest` (nebo `OnMeasure`) může být nekonečné (tj, hodnoty `Double.PositiveInfinity`). Ale `SizeRequest` vrácená z těchto metod nemůže obsahovat nekonečné dimenzí.

Nekonečné omezení znamenat, že požadovaná velikost by měl odrážet přirozenou velikostí elementu. Svislé `StackLayout` volání `GetSizeRequest` (nebo `Measure`) na své podřízené objekty s omezením nekonečné výšku. Rozložení vodorovné zásobníku volání `GetSizeRequest` (nebo `Measure`) na své podřízené objekty s omezením neomezenou šířku. `AbsoluteLayout` Volání `GetSizeRequest` (nebo `Measure`) na své podřízené objekty s neomezenou šířku a výšku omezení.

### <a name="peeking-inside-the-process"></a>Prohlížení uvnitř proces

[ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) zobrazí omezení a velikost žádostí o informace pro jednoduché rozložení.

## <a name="deriving-from-layoutview"></a>Odvozování z rozložení<View>

Vlastní rozložení třída odvozená z `Layout<View>`. Má dva zodpovědnosti:

- Přepsání `OnMeasure` volat `Measure` na všechny rozložení podřízené objekty. Požadovaná velikost pro samotné rozložení
- Přepsání `LayoutChildren` volat `Layout` na všechny rozložení podřízené objekty

`for` Nebo `foreach` smyčky v tato přepsání by měla přeskočit všechny podřízené jejichž `IsVisible` je nastavena na `false`.

Volání `OnMeasure` není zaručena. `OnMeasure` nebude volána, pokud nadřazená rozložení je řídících velikosti pro rozložení (například rozložení, které vyplní celé stránky). Z tohoto důvodu `LayoutChildren` nelze závisí na velikosti podřízené získané při `OnMeasure` volání. Velmi často `LayoutChildren` musí sám volat `Measure` na podřízené objekty na rozložení, nebo můžete implementovat nějaký druh velikost ukládání do mezipaměti logiku (a později popsané).

### <a name="an-easy-example"></a>Příklad snadno

[ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) ukázka obsahuje zjednodušená [ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) třídy a předvedení jeho použití.

### <a name="vertical-and-horizontal-positioning-simplified"></a>Svislého a vodorovného umístění zjednodušená

Jedna z úloh, `VerticalStack` musíte provést spadá `LayoutChildren` přepsat. Metoda používá dítěte `HorizontalOptions` vlastnost určit, jak na pozici v rámci jeho slot v podřízených `VerticalStack`. Můžete místo toho zavolejte statickou metodu [ `Layout.LayoutChildIntoBoundingRect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/). Tato metoda volá `Measure` na podřízené a používá jeho `HorizontalOptions` a `VerticalOptions` vlastnosti, které chcete umístit podřízené v rámci zadaného rámeček.

### <a name="invalidation"></a>Zneplatnění

Ke změně v elementu vlastnost často ovlivňuje, jak tento prvek se zobrazuje v rozložení. Rozložení musí být zneplatněné k aktivaci nové rozložení.

`VisualElement` Definuje chráněná metoda [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/), obecně tzv. obslužná rutina vlastnost změnit libovolné vazbu vlastnosti jejichž změna má vliv velikost elementu. `InvalidateMeasure` Metoda aktivuje [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) událostí.

`Layout` Třída definuje podobně jako chráněnou metodu s názvem [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/), které `Layout` odvozených by měly volat pro všechny změny, která má vliv na způsob umisťuje a velikosti své podřízené objekty.

### <a name="some-rules-for-coding-layouts"></a>Některá pravidla pro kódování rozložení

1. Vlastnosti definované `Layout<T>` odvozené konfigurace by měl být zálohovaný vazbu vlastnosti a obslužné rutiny vlastnost změnit by měly volat `InvalidateLayout`.

2. A `Layout<T>` by měly přepsat odvozených, který definuje vazbu přidružené vlastnosti [ `OnAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnAdded/p/T/) pro přidání obslužné rutiny vlastnost změnit jeho podřízených objektů a [ `OnRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnRemoved/p/T/) odebrat Obslužná rutina. Obslužná rutina by měla zkontrolujte změny v těchto přidružené vazbu vlastnosti a reagovat voláním `InvalidateLayout`.

3. A `Layout<T>` by měly přepsat odvozených, který implementuje mezipaměti o velikosti podřízené `InvalidateLayout` a [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) a vymažte mezipaměť, pokud jsou volány tyto metody.

### <a name="a-layout-with-properties"></a>Rozložení s vlastnostmi

[ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) předpokládá, že se všechny její podřízené položky stejné velikost a dojde k zalomení podřízené objekty z jeden řádek (nebo sloupec) na další. Definuje, `Orientation` vlastnost jako `StackLayout`, a `ColumnSpacing` a `RowSpacing` vlastnosti, například `Grid`, a ukládá do mezipaměti podřízené velikosti.

[ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) ukázkové PUT `WrapLayout` v `ScrollView` pro zobrazení uložené fotografie.

### <a name="no-unconstrained-dimensions-allowed"></a>Žádné neomezeným dimenze povoleny!

[ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna je určená zobrazíte všechny její podřízené položky v rámci samotného. Proto nelze zpracovat s neomezeným dimenzemi a vyvolá výjimku, pokud jeden došlo k.

[ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) příklad znázorňuje `UniformGridLayout`:

[![Trojitá snímek obrazovky Mřížka fotografií](images/ch26fg08-small.png "Uniform rozložení mřížky")](images/ch26fg08-large.png#lightbox "Uniform rozložení mřížky")

### <a name="overlapping-children"></a>Překrývající se podřízené objekty

A `Layout<T>` odvozených může dojít k překrytí své podřízené objekty. Ale podřízené objekty jsou vykreslovány v pořadí podle jejich v `Children` kolekce a ne pořadí, v jakém jejich `Layout` metody jsou volány.

`Layout` Třída definuje dvě metody, které umožňují přesunout podřízenou v kolekci:

- [`LowerChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LowerChild/p/Xamarin.Forms.View/) Přesunout podřízenou na začátek kolekce
- [`RaiseChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.RaiseChild/p/Xamarin.Forms.View/) Přesunout podřízenou na konec kolekce

Pro překrývající se děti děti na konec kolekce vizuálně zobrazují nad podřízené objekty na začátek kolekce.

[ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny definuje přidružená vlastnost označují pořadí vykreslování a proto povolit jeden z jeho má zobrazit nad jiné podřízené objekty. [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) příklad znázorňuje toto:

[![Trojitá snímek obrazovky Student karta soubor mřížky](images/ch26fg10-small.png "překrývání podřízené objekty rozložení")](images/ch26fg10-large.png#lightbox "překrývání rozložení podřízené objekty")

### <a name="more-attached-bindable-properties"></a>Víc připojený vazbu vlastnosti

[ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny definuje připojené vazbu vlastnosti k určení dva `Point` hodnoty a Tloušťka hodnotu a zpracovává `BoxView` elementy tak, aby připomínaly řádky.

[ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) Ukázka používá k vykreslení 3D datové krychle.

### <a name="layout-and-layoutto"></a>Rozložení a LayoutTo

A `Layout<T>` můžete volat odvozených `LayoutTo` místo `Layout` pro animaci rozložení. [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) Třída nepodporuje a [ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) příklad znázorňuje ho.



## <a name="related-links"></a>Související odkazy

- [Úplný text 26 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [Ukázky kapitoly 26](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [Vytváření vlastní rozložení](~/xamarin-forms/user-interface/layouts/custom.md)
