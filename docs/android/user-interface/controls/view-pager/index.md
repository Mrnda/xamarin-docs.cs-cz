---
title: ViewPager
description: ViewPager je rozložení správce, který umožňuje implementovat posunkové navigace. Posunkové navigační umožňuje uživateli prstem levé a pravé krok prostřednictvím stránky data. Tato příručka vysvětluje, jak implementovat posunkové navigace pomocí ViewPager a bez fragmenty. Také popisuje, jak přidat pomocí PagerTitleStrip a PagerTabStrip indikátory stránky.
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: bd687175048bb6a19dde21e66619667511a76796
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769000"
---
# <a name="viewpager"></a>ViewPager

_ViewPager je rozložení správce, který umožňuje implementovat posunkové navigace. Posunkové navigační umožňuje uživateli prstem levé a pravé krok prostřednictvím stránky data. Tato příručka vysvětluje, jak implementovat posunkové navigace pomocí ViewPager a bez fragmenty. Také popisuje, jak přidat pomocí PagerTitleStrip a PagerTabStrip indikátory stránky._

 
## <a name="overview"></a>Přehled

Běžný scénář v vývoj aplikací je potřeba uživatelům poskytnout posunkové přecházení mezi zobrazení na stejné úrovni. V tomto přístupu uživatele swipes doleva nebo doprava na stránky přístup k obsahu (například v Průvodci instalací nebo prezentace). Tato zobrazení prstem můžete vytvořit pomocí `ViewPager` pomůcky, k dispozici v [podporu knihovna pro Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). `ViewPager` Je widget rozložení skládá z více podřízených zobrazení, kde každé podřízené zobrazení, znamená to, na stránce v rozložení: 

[![Snímky obrazovky TreePager aplikace vodorovné prstem příklad](images/01-intro-sml.png)](images/01-intro.png#lightbox)

Obvykle `ViewPager` se používá ve spojení s [fragmenty](https://developer.xamarin.com/guides/android/platform_features/fragments/), nicméně některých situacích, kde můžete chtít použít `ViewPager` bez přidání složitosti `Fragment`s.

`ViewPager` využívá vzor adaptéru poskytnout pohledů pro zobrazení. Adaptér použít zde je koncepčně podobá používaného [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash; zadáte implementace `PagerAdapter` ke generování stránky, `ViewPager` zobrazí uživateli. Stránky zobrazí `ViewPager` může být `View`s nebo `Fragment`s. Když `View`s se zobrazí, adaptéru podtřídy Android na `PagerAdapter` základní třídy. Pokud `Fragment`s se zobrazí, adaptéru podtřídy Android na `FragmentPagerAdapter`. Knihovna pro Android, podporují také zahrnuje `FragmentPagerAdapter` (podtřídou třídy `PagerAdapter`) usnadní podrobnosti připojení `Fragment`s k datům. 

Tato příručka ukazuje, jak přístupy: 

-   V [Viewpager se zobrazeními](~/android/user-interface/controls/view-pager/viewpager-and-views.md), [TreePager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) je aplikace vyvinuté tak, aby ukazují, jak používat `ViewPager` -li zobrazit zobrazení katalogu stromu (Galerie Listnatý a stále zelený stromů bitové kopie). 
    `PagerTabStrip`  a `PagerTitleStrip` slouží k zobrazení názvů, které pomáhají s navigaci na stránce.

-   V [Viewpager fragmenty](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md), trochu složitější [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) je aplikace vyvinuté tak, aby ukazují, jak používat `ViewPager` s `Fragment`s sestavit aplikaci, která představuje problém matematické jako Flash karty a reaguje na vstup uživatele. 


## <a name="requirements"></a>Požadavky

Použít `ViewPager` v projektu aplikace musíte nainstalovat [podporu knihovna pro Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) balíčku. Další informace o instalaci balíčků NuGet najdete v tématu [návod: včetně NuGet ve vašem projektu](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 

 
## <a name="architecture"></a>Architektura

Tři součásti jsou používány pro implementaci posunkové navigace pomocí `ViewPager`:

-   ViewPager
-   Adaptér
-   Ukazatel na pager

Každou z těchto součástí je uveden níže.



### <a name="viewpager"></a>ViewPager

`ViewPager` je Správce rozložení, který zobrazí kolekce `View`s jeden najednou. Úlohy je zjistit prstem gesto uživatele a přejděte do zobrazení další nebo předchozí podle potřeby. Například následující snímek obrazovky ukazuje `ViewPager` přechodu z jedné bitové kopie na další v reakci na gesto uživatele: 

[![Aplikace Closeup TreePager zobrazení přechod mezi zobrazení](images/02-transition-sml.png)](images/02-transition.png#lightbox)


### <a name="adapter"></a>Adaptér

`ViewPager` získává jeho data ze *adaptér*. Úloha adaptéru je vytvoření `View`s zobrazuje `ViewPager`, poskytovat jim podle potřeby. Následující obrázek znázorňuje tento koncept &ndash; adaptéru vytvoří a naplní `View`s a zajišťuje, aby `ViewPager`. Jako `ViewPager` zjistí gesta prstem uživatele, požádá adaptér, který má poskytnout příslušné `View` k zobrazení: 

[![Diagram ilustrující způsob adaptér připojení k ViewPager obrázky a názvy](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

V tomto konkrétním příkladu každý `View` sestavený ze stromu image a název větve předtím, než je předán `ViewPager`. 



### <a name="pager-indicator"></a>Ukazatel na pager

`ViewPager` může použít k zobrazení velké datové sady (například Galerie obrázků může obsahovat stovky bitové kopie). Uživatelům usnadňují procházení velkých datových sad, `ViewPager` je často přiložena *pager indikátor* který zobrazí řetězec. Název bitové kopie, popisek nebo jednoduše aktuální zobrazení pozici v rámci sady dat, může být řetězec. 

Existují dvě zobrazení, které mohou vytvořit tyto informace navigace pro vás: `PagerTabStrip` a `PagerTitleStrip.` každá řetězec zobrazí v horní části `ViewPager`, a každou získává jeho data ze `ViewPager`na adaptéru tak, aby vždy stále synchronizované s aktuálně zobrazený `View`. Rozdíl mezi nimi je, že `PagerTabStrip` zahrnuje vizuální označení pro řetězec "aktuální" při `PagerTitleStrip` nemá (jak je znázorněno v tyto snímky obrazovky): 

[![Snímky obrazovky aplikace TreePager s PagerTitleStrip a PagerTabStrip](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

Tato příručka ukazuje, jak immplement `ViewPager`, adaptér a komponenty Indikátor aplikací a integrovat je pro podporu posunkové navigace. 



## <a name="related-links"></a>Související odkazy

- [TreePager (ukázka)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
- [FlashCardPager (ukázka)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
