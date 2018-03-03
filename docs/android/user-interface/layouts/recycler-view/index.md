---
title: RecyclerView
description: "RecyclerView je skupina zobrazení pro zobrazení kolekcí; je určený flexibilnější náhrada za starší zobrazení skupin jako je například ListView a GridView.  Tato příručka vysvětluje, jak používat a přizpůsobit RecyclerView v aplikacích Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/03/2018
ms.openlocfilehash: ec8b3a4655c8e8d9e492c9f7a1807dd64ecc6ae7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView je skupina zobrazení pro zobrazení kolekcí; je určený flexibilnější náhrada za starší zobrazení skupin jako je například ListView a GridView.  Tato příručka vysvětluje, jak používat a přizpůsobit RecyclerView v aplikacích Xamarin.Android._

## <a name="recyclerview"></a>RecyclerView

Mnoho aplikací se třeba zobrazit kolekce stejného typu (například zpráv, kontaktů, obrázky nebo skladeb); Tato kolekce je často příliš velká na obrazovce, takže kolekce se zobrazí v malém okně, které lze hladce procházet všechny položky v kolekci.
`RecyclerView` je Android pomůcky, který se zobrazuje kolekce položek v seznamu nebo Mřížka, povolíte uživatelům procházet kolekce. Tady je snímek obrazovky příklad aplikaci, která používá `RecyclerView` zobrazit obsah složky Doručená pošta e-mailu v svislé posouvání seznamu:

[ ![Příklad aplikace pomocí RecyclerView do seznamu doručených zpráv](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png)

`RecyclerView` nabízí dvě poutavé funkce:

-  Je flexibilní architekturu, která umožňuje měnit své chování zapojením v vaše upřednostňované komponenty.

-  Je efektivní s rozsáhlých kolekcí, protože opětovně používá položky zobrazení a vyžaduje použití *zobrazit držitelé* mezipaměti zobrazení odkazů na.

Tato příručka vysvětluje, jak používat `RecyclerView` v aplikacích Xamarin.Android; vysvětluje, jak přidat `RecyclerView` balíčku do projektu Xamarin.Android a popisuje, jak `RecyclerView` funkce v typické aplikaci. Příklady kódu skutečné poskytnuto ukazují, jak integrovat `RecyclerView` do vaší aplikace, jak implementovat klikněte na položku zobrazení a jak aktualizovat `RecyclerView` až se změní jeho základní data. Tato příručka předpokládá, že jste obeznámeni s vývojem pro Xamarin.Android.


### <a name="requirements"></a>Požadavky

I když `RecyclerView` je často přidružené Android 5.0 typu Lupa, se jako podporu knihovna nabízí &ndash; `RecyclerView` spolupracuje s aplikacemi cílové rozhraní API úrovně 7 (Android 2.1) a novější. K použití se vyžaduje následující `RecyclerView` v aplikacích založených na Xamarin:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 nebo novější musí být nainstalovaná a nakonfigurovaná s Visual Studio nebo Visual Studio for Mac.

-  Musí zahrnovat projektu aplikace **Xamarin.Android.Support.v7.RecyclerView** balíčku. Další informace o instalaci balíčků NuGet najdete v tématu [návod: včetně NuGet ve vašem projektu](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


### <a name="overview"></a>Přehled

`RecyclerView` můžete představit jako náhrada za `ListView` a `GridView` pomůcky v Android. Jako jeho předchůdci, `RecyclerView` slouží k zobrazení velké datové sady v malém okně, ale `RecyclerView` nabízí další možnosti rozložení a je lepší optimalizovaný pro zobrazení rozsáhlých kolekcí. Pokud jste se seznámili s `ListView`, existuje několik důležitých rozdílů mezi `ListView` a `RecyclerView`:

-   `RecyclerView` je trochu složitější použít: budete muset napsat další kód pro použití `RecyclerView` ve srovnání s `ListView`.

-   `RecyclerView` neposkytuje předdefinované adaptér; adaptér kód, který přistupuje k zdroje dat musí implementovat. Ale Android obsahuje několik předdefinovaných adaptéry, které pracovat s `ListView` a `GridView`.

-   `RecyclerView` nenabízí položky kliknutím událost, když uživatel klepne na položku; Místo toho se události kliknutí na položku požádat o pomocných tříd. Naopak `ListView` nabízí událost kliknutí na položku.

-   `RecyclerView` zlepšuje výkon recyklace zobrazení a vynucování vzorek zobrazení vlastníka, který eliminuje vyhledávání prostředků nepotřebné rozložení. Použití vzoru držitel zobrazení je volitelné v `ListView`.

-   `RecyclerView` je založen na modulární návrh, který usnadňuje přizpůsobit. Například můžete zařadit v zásadách různých rozložení bez změny významné kódu do vaší aplikace.
    Naopak `ListView` je poměrně monolitický ve struktuře.

-   `RecyclerView` obsahuje integrovanou animací pro položku přidávat a odebírat. `ListView` animace vyžadovat některé další úsilí ze strany vývojáři aplikace.


### <a name="sections"></a>Oddíly

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView částí a funkce](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

Toto téma vysvětluje, jak `Adapter`, `LayoutManager`, a `ViewHolder` pracovní společně jako pomocné třídy pro podporu `RecyclerView`.
Poskytuje základní přehled jednotlivých těchto pomocných tříd a vysvětluje, jak je použít ve vaší aplikaci.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[Základní příklad RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

Toto téma je založený na informacích uvedených v [RecyclerView částí a funkce](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) tím, že poskytuje skutečné kód příklady různých `RecyclerView` elementy jsou implementované pro sestavení reálného fotku procházení aplikace.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[Příklad RecyclerView rozšíření](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

Toto téma přidá další kód do aplikace příklad uvedené v [A základní příklad RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) k předvedení způsobu zpracování události kliknutí na položku a aktualizace `RecyclerView` při změně zdroje v základních datech.


### <a name="summary"></a>Souhrn

Tato příručka zavedená Android `RecyclerView` pomůcky; ho vysvětlení najdete postup přidání `RecyclerView` knihovnu do projektů Xamarin.Android, jak podpory `RecyclerView` recykluje zobrazení, jak vynucuje zobrazení držitel vzor pro efektivitu a jak různé pomocné třídy, které tvoří `RecyclerView` spolupracovat zobrazíte kolekce. Je poskytovala ukázkový kód k předvedení jak `RecyclerView` je integrovaná do aplikace, je popsané postupy přizpůsobit `RecyclerView`je zásad rozložení zapojením v různých rozložení správce a popisuje způsob zpracování položky události kliknutí a upozornit `RecyclerView`změn pro zdroj dat.

Další informace o `RecyclerView`, najdete v článku [referenci třídy RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html).


## <a name="related-links"></a>Související odkazy

- [RecyclerViewer (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Úvod do typu Lupa](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
