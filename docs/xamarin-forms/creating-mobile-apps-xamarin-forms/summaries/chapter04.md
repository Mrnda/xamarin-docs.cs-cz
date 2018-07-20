---
title: Souhrn kapitoly 4. Posouvání zásobníku
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 4. Posouvání zásobníku'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 49f2d96fb7f95ab880d5cfafa420afbbe933c1ad
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156714"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Souhrn kapitoly 4. Posouvání zásobníku

Tato kapitola je primárně věnována zavedení konceptu *rozložení*, což je celková doba splatnosti třídy a techniky, které využívá Xamarin.Forms pro uspořádání vizuální zobrazení více zobrazení na stránce.

Rozložení zahrnuje několik tříd, které jsou odvozeny z [ `Layout` ](xref:Xamarin.Forms.Layout) a [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1). Tato kapitola se zaměřuje na [ `StackLayout` ](xref:Xamarin.Forms.StackLayout).

> [!NOTE]
> [ `FlexLayout` ](~/xamarin-forms/user-interface/layouts/flex-layout.md) Zavedené v Xamarin.Forms 3.0 je možné způsoby, které jsou podobné `StackLayout` , ale s větší flexibilitou.

V této kapitole zavedli jsou [ `ScrollView` ](xref:Xamarin.Forms.ScrollView), [ `Frame` ](xref:Xamarin.Forms.Frame), a [ `BoxView` ](xref:Xamarin.Forms.BoxView) třídy.

## <a name="stacks-of-views"></a>Zobrazení zásobníků

[`StackLayout`](xref:Xamarin.Forms.StackLayout) je odvozen od `Layout<View>` a dědí [ `Children` ](xref:Xamarin.Forms.Layout`1) vlastnost typu `IList<View>`. Přidání více zobrazení položek do této kolekce a `StackLayout` zobrazí je v zásobníku vodorovně nebo svisle.

Nastavte [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) vlastnost `StackLayout` členovi [ `StackOrientation` ](xref:Xamarin.Forms.StackOrientation) výčet, buď [ `Vertical` ](xref:Xamarin.Forms.StackOrientation.Vertical) nebo [ `Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal). Výchozí hodnota je `Vertical`.

Nastavte [ `Spacing` ](xref:Xamarin.Forms.StackLayout.Spacing) vlastnost `StackLayout` k `double` hodnotu, která určuje mezery mezi podřízené objekty. Výchozí hodnota je 6.

V kódu, můžete přidat položky, které chcete `Children` kolekce `StackLayout` v `for` nebo `foreach` smyčce, jak je ukázáno v [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) vzorku, nebo můžete inicializovat `Children` kolekce se seznamem jednotlivá zobrazení jako předvedenou v [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Podřízené objekty musí být odvozen od `View` ale mohou zahrnovat další `StackLayout` objekty.

## <a name="scrolling-content"></a>Posouvání obsahu

Pokud `StackLayout` obsahuje příliš mnoho podřízených objektů se zobrazí na stránce, můžete umístit `StackLayout` v [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) umožňující posouvání.

Nastavte [ `Content` ](xref:Xamarin.Forms.ScrollView.Content) vlastnost `ScrollView` chcete přejít do zobrazení. Často se jedná `StackLayout`, ale může být libovolné zobrazení.

Nastavte [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation) vlastnost `ScrollView` členovi [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) vlastnost [ `Vertical` ](xref:Xamarin.Forms.ScrollOrientation.Vertical), [ `Horizontal` ](xref:Xamarin.Forms.ScrollOrientation.Horizontal), nebo [ `Both` ](xref:Xamarin.Forms.ScrollOrientation.Both). Výchozí hodnota je `Vertical`. Pokud obsah `ScrollView` je `StackLayout`, dva orientace by měl být konzistentní vzhledem k aplikacím.

[ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) ukázka demonstruje použití `ScrollView` a `StackLayout` k zobrazení dostupných barev. Ukázka také ukazuje, jak získat všechny veřejné statické vlastnosti a pole pomocí reflexe .NET `Color` struktury bez nutnosti explicitně uvádět na seznamu.

## <a name="the-expands-option"></a>Možnost Expands

Když `StackLayout` zásobníky podřízených jednotlivých podřízených zabírá konkrétní pozici v rámci celková výška `StackLayout` , který závisí na velikosti dítěte a nastavení jeho `HorizontalOptions` a `VerticalOptions` vlastnosti. Tyto vlastnosti jsou přiřazeny hodnoty typu [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/).

`LayoutOptions` Struktury definuje dvě vlastnosti:

- [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) výčet typu [ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment) se čtyři členy [ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start), [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center), [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End), a [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) typu `bool`

Pro usnadnění práce `LayoutOptions` struktury také definuje osm statické pole jen pro čtení typu `LayoutOptions` , který zahrnuje všechny kombinace dvě instance vlastnosti:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

Zahrnuje následující diskuse `StackLayout` při svislé orientaci výchozí. Vodorovné `StackLayout` je obdobou.

Svisle `StackLayout`, `HorizontalOptions` nastavení určuje, jak je podřízený ve vodorovném směru umístěn v rámci šířku `StackLayout`. `Alignment` Nastavení `Start`, `Center`, nebo `End` způsobí, že podřízené bude vodorovně bez omezení. Určuje vlastní šířku podřízený objekt a je umístěn na střed, vlevo nebo vpravo od `StackLayout`. `Fill` Možnost způsobí, že na podřízenou k vodorovně omezené a vyplní šířku `StackLayout`.

Svisle `StackLayout`, každá podřízená položka svisle bez omezení a získá a jsou odděleny svislou pozici v závislosti na výšku dítěte v takovém případě `VerticalOptions` je bezvýznamná nastavení.

Pokud svislé `StackLayout` sám o sobě představuje neomezeným&mdash;, který je-li jeho `VerticalOptions` nastavení je `Start`, `Center`, nebo `End`, pak výšku `StackLayout` je celková výška své podřízené objekty.

Ale pokud svislé `StackLayout` svisle omezené&mdash;pokud jeho `VerticalOptions` nastavení je `Fill` &mdash;pak výšku `StackLayout` bude výšku svého kontejneru, což může být větší než celkový počet Výška své podřízené objekty. Pokud je to tento případ, a pokud má alespoň jeden podřízený prvek `VerticalOptions` nastavení pomocí `Expands` příznak `true`, pak přebytečné místo v `StackLayout` se rozdělí rovnoměrně mezi všechny tyto podřízené objekty s `Expands` příznak `true`. Celková výška podřízené objekty se bude rovnat pak výšku `StackLayout`a `Alignment` součástí `VerticalOptions` nastavení určuje, jak je podřízený svisle umístěn ve slotu.

To je patrné [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) vzorku.

## <a name="frame-and-boxview"></a>Rámce a BoxView

Tyto dvě obdélníkové zobrazení se často používají pro účely prezentace.

[ `Frame` ](xref:Xamarin.Forms.Frame) Zobrazení zobrazí obdélníkové rámeček kolek jiné zobrazení, která může být například rozložení `StackLayout`. `Frame` dědí [ `Content` ](xref:Xamarin.Forms.ContentView.Content) vlastnost z [ `ContentView` ](xref:Xamarin.Forms.ContentView) , kterou jste nastavili na zobrazení, který se má zobrazit v rámci `Frame`. `Frame` Je ve výchozím nastavení transparentní. Nastavte následující tři vlastnosti pro přizpůsobení vzhledu rámce:

- [ `OutlineColor` ](xref:Xamarin.Forms.Frame.OutlineColor) Vlastnost, aby byla viditelná. Je běžné nastavit `OutlineColor` k `Color.Accent` Pokud si nejste jisti, základní barevném schématu.
- [ `HasShadow` ](xref:Xamarin.Forms.Frame.HasShadow) Nastavenou na `true` zobrazíte černé stín na zařízeních s Iosem.
- Nastavte [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) vlastnost `Thickness` obsah vašeho hodnotu nechte mezeru mezi snímek a snímek. Výchozí hodnota je 20 jednotek na všech stranách.

`Frame` Nemá výchozí hodnotu `HorizontalOptions` a `VerticalOptions` hodnoty `LayoutOptions.Fill`, což znamená, že `Frame` vyplní svého kontejneru. S jinými nastaveními, velikost `Frame` vychází z velikosti svého obsahu.

`Frame` Je patrné [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) vzorku.

[ `BoxView` ](xref:Xamarin.Forms.BoxView) Zobrazí obdélníkovou oblast barva určená jeho [ `Color` ](xref:Xamarin.Forms.BoxView.Color) vlastnost.

Pokud `BoxView` je omezená (jeho `HorizontalOptions` a `VerticalOptions` vlastnosti mají jejich výchozí nastavení pro položky `LayoutOptions.Fill`), `BoxView` vyplní dostupné místo pro něj. Pokud `BoxView` je bez omezení (s `HorizontalOptions` a `LayoutOptions` nastavení `Start`, `Center`, nebo `End`), má výchozí dimenzi 40 jednotek čtverce. A `BoxView` můžete omezené v jedné dimenzi a vstupy bez omezení v jiném.

Často, bude nastavena [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) a [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) vlastnosti `BoxView` nabízí určité velikosti. To je znázorněn ve [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) vzorku.

Můžete použít několik instancí `StackLayout` zkombinovat `BoxView` a několik `Label` instance v `Frame` k zobrazení určité barvy a potom se spojí každé z těchto zobrazení v `StackLayout` v `ScrollView` vytvořit atraktivní Seznam barev zobrazených v [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) vzorku:

[![Trojitá snímek barevné bloky](images/ch04fg11-small.png "seznamu barev")](images/ch04fg11-large.png#lightbox "seznamu barev")

## <a name="a-scrollview-in-a-stacklayout"></a>ScrollView v StackLayout?

Vložení `StackLayout` v `ScrollView` je běžné, ale uváděním `ScrollView` v `StackLayout` také někdy je vhodné. Teoreticky vzato to by neměl být možné protože podřízených objektů a jsou odděleny svislou `StackLayout` jsou svisle bez omezení. Ale `ScrollView` svisle omezené. To se musí předávat určité výšky tak, aby ho můžete určit velikost podřízeným pro posouvání.

Trik, jak zajistit je umožnit `ScrollView` podřízený `StackLayout` `VerticalOptions` nastavení `FillAndExpand`. To je patrné [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) vzorku.

**BlackCat** ukázka také ukazuje, jak definovat a přístup k prostředkům programu, které jsou vloženy do sdílené knihovny. Toho lze dosáhnout také pomocí sdílených projektů Asset (protokoly SAP), ale postup je trochu trickier, jako [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) ukázce.



## <a name="related-links"></a>Související odkazy

- [Kapitola 4 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Ukázky kapitole 4](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Ukázky kapitole 4 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
