---
title: Shrnutí kapitole 4. Procházení zásobníku
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 09a9d21b7979e0eb3f12d3b1207f2185e059f65c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Shrnutí kapitole 4. Procházení zásobníku

Tato kapitola je primárně věnována představení koncept *rozložení*, což je celkové termín pro třídy a techniky, které Xamarin.Forms používá k uspořádání visual zobrazení více zobrazení na stránce.

Rozložení zahrnuje několik tříd, které jsou odvozeny od [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) a [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/). Tato kapitola se zaměřuje na [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

Také zavedená v této kapitole jsou [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/), [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/), a [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) třídy.

## <a name="stacks-of-views"></a>Zásobníky zobrazení

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) odvozená z `Layout<View>` a dědí [ `Children` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) vlastnost typu `IList<View>`. Přidání více zobrazení položek do této kolekce, a `StackLayout` zobrazí je v zásobníku vodorovně nebo svisle.

Nastavte [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) vlastnost `StackLayout` k členem [ `StackOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackOrientation/) výčtu, buď [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Vertical/) nebo [ `Horizontal`](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Horizontal/). Výchozí hodnota je `Vertical`.

Nastavte [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) vlastnost `StackLayout` k `double` hodnotu, která určuje mezery mezi podřízené objekty. Výchozí hodnota je 6.

V kódu, můžete přidat položky do `Children` kolekce `StackLayout` v `for` nebo `foreach` cykly, jak je předvedeno v [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) vzorku, nebo můžete inicializovat `Children` kolekci se seznamem jednotlivých zobrazení prokázaná v [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Podřízené objekty musí být odvozeny od `View` , ale může obsahovat jiné `StackLayout` objekty.

## <a name="scrolling-content"></a>Posouvání obsahu

Pokud `StackLayout` obsahuje příliš mnoho podřízené položky zobrazíte na stránce, můžete umístit `StackLayout` v [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) umožňující posouvání.

Nastavte [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) vlastnost `ScrollView` chcete přejděte do zobrazení. Tento problém je často `StackLayout`, ale může být všechna zobrazení.

Nastavte [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) vlastnost `ScrollView` k členem [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/) vlastnost [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Vertical/), [ `Horizontal` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Horizontal/), nebo [ `Both` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Both/). Výchozí hodnota je `Vertical`. Pokud obsah `ScrollView` je `StackLayout`, orientaci ve dvou by měla být konzistentní.

[ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) příklad ukazuje použití `ScrollView` a `StackLayout` na rozevírací seznam. Ukázka také ukazuje, jak získat všechny veřejné statické vlastnosti a pole pomocí reflexe .NET `Color` struktury, aniž by bylo nutné explicitně jejich seznam.

## <a name="the-expands-option"></a>Možnost Expands

Když `StackLayout` zásobníky podřízené jednotlivých podřízených zabírá konkrétní pozici v rámci celková výška `StackLayout` to závisí na velikosti dítěte a nastavení jeho `HorizontalOptions` a `VerticalOptions` vlastnosti. Tyto vlastnosti jsou přiřazené hodnoty typu [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/).

`LayoutOptions` Struktura definuje dvě vlastnosti:

- [`Alignment`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) typu výčtu [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/) s čtyři členy [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), a [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/)
- [`Expands`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) typu `bool`

Pro usnadnění vaší práce `LayoutOptions` struktura také definuje osm statická jen pro čtení pole typu `LayoutOptions` který zahrnuje všechny kombinace vlastnosti dvě instance:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

Zahrnuje následující diskusi `StackLayout` s výchozí svislé orientace. Ve vodorovném `StackLayout` podobá.

Pro svislé `StackLayout`, `HorizontalOptions` nastavení určuje, jak je podřízená vodorovně umístěn v rámci šířku `StackLayout`. `Alignment` Nastavení `Start`, `Center`, nebo `End` způsobí, že podřízená být vodorovně bez omezení. Určuje vlastní šířka podřízený objekt a je umístěn na střed, vlevo nebo vpravo od `StackLayout`. `Fill` Možnost způsobí, že podřízená vodorovně omezovaný a výplní šířku `StackLayout`.

Pro svislé `StackLayout`, je svisle neomezeným jednotlivých podřízených a získá svislou pozici v závislosti na výšku dítěte, v takovém případě `VerticalOptions` nastavení je důležité.

Pokud svislice `StackLayout` sám o sobě představuje neomezeným&mdash;tedy pokud jeho `VerticalOptions` nastavení je `Start`, `Center`, nebo `End`, pak výšku `StackLayout` je celková výška jeho podřízených položek.

Ale pokud svislice `StackLayout` je svisle omezené&mdash;pokud jeho `VerticalOptions` nastavení je `Fill` &mdash;pak výšku `StackLayout` bude výšku jeho kontejneru, který může být větší než celkový počet výška jeho podřízených položek. Pokud je to tento případ, který má alespoň jednu podřízenou `VerticalOptions` nastavení se `Expands` příznak `true`, pak volné místo v `StackLayout` je přidělen rovnoměrně mezi všechny podřízené s `Expands` příznak `true`. Celková výška podřízených rovnat výšku `StackLayout`a `Alignment` součástí `VerticalOptions` nastavení určuje, jak je podřízená ve svislém směru umístěn v jeho slot.

Tento postup je znázorněn v [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) ukázka.

## <a name="frame-and-boxview"></a>Rámec a BoxView

Tyto dvě obdélníková zobrazení se často používají pro účely prezentace.

[ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) Zobrazení zobrazí obdélníková rámečku kolem jiné zobrazení, které může být například rozložení `StackLayout`. `Frame` dědí [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) vlastnost z [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) , kterou nastavit na zobrazení, který se má zobrazit v rámci `Frame`. `Frame` Je transparentní ve výchozím nastavení. Nastavte následující tři vlastnosti k přizpůsobení vzhledu rámečku:

- [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/) Vlastnost vytvořit viditelné. Je běžné, chcete-li nastavit `OutlineColor` k `Color.Accent` Pokud nevíte, základní barevné schéma.
- [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/) Může být nastavena `true` zobrazíte černým stínové na zařízení s iOS.
- Nastavte [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) vlastnosti `Thickness` hodnotu do textového rámečku a rámečku je obsahu. Výchozí hodnota je 20 jednotky ze všech stran.

`Frame` Má výchozí `HorizontalOptions` a `VerticalOptions` hodnoty `LayoutOptions.Fill`, to znamená, že `Frame` naplní jejímu kontejneru. S jiným nastavením velikost `Frame` je založena na velikosti jeho obsah.

`Frame` Ukazují [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) ukázka.

[ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) Zobrazí obdélníkovou oblast barvu uvedené podle jeho [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) vlastnost.

Pokud `BoxView` je omezené (jeho `HorizontalOptions` a `VerticalOptions` vlastnosti mají výchozí nastavení z `LayoutOptions.Fill`), `BoxView` doplní dostupného volného místa pro ni. Pokud `BoxView` neomezeným (s `HorizontalOptions` a `LayoutOptions` nastavení `Start`, `Center`, nebo `End`), má výchozí dimenze hranaté 40 jednotky. A `BoxView` může být omezené v jednou dimenzí a neomezeného v dalších.

Často budete nastavíte [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) a [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) vlastnosti `BoxView` umožnit konkrétní velikost. Tento koncept je znázorněn pomocí [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) ukázka.

Můžete použít několik instancí `StackLayout` kombinovat `BoxView` a několik `Label` instancí v `Frame` zobrazovat určité barvy, a potom se spojí každé z těchto zobrazení v `StackLayout` v `ScrollView` vytvořit atraktivní Seznam barev ukazuje [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) ukázka:

[![Trojitá snímek obrazovky barevné bloky](images/ch04fg11-small.png "seznamu barvy")](images/ch04fg11-large.png#lightbox "seznamu barvy")

## <a name="a-scrollview-in-a-stacklayout"></a>ScrollView v StackLayout?

Vložení `StackLayout` v `ScrollView` je běžné, ale uváděním `ScrollView` v `StackLayout` je také někdy vhodné. Teoreticky, nemělo by se jednat možné vzhledem k tomu, děti svislého `StackLayout` jsou svisle neomezeným. Ale `ScrollView` svisle omezené. Udává se určité výšky tak, aby ji potom můžete určit velikost jeho podřízených pro posouvání.

Základem je poskytnout `ScrollView` podřízeným `StackLayout` `VerticalOptions` nastavení `FillAndExpand`. Tento postup je znázorněn v [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) ukázka.

**BlackCat** ukázka také ukazuje, jak definovat a přístup k prostředkům programu, které jsou vloženy v přenosné knihovny tříd (PCL). Toho lze dosáhnout také s sdílený prostředek projekty (protokoly SAP), ale proces je trochu trickier, jako [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) příklad znázorňuje.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 4 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Ukázky kapitoly 4](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Kapitola 4 F # – ukázky](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
