---
title: Na kartách rozložení
description: Přehled záložkách rozložení v Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/23/2017
ms.openlocfilehash: 4095944bb630637a2e761af18796dacdef17785c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="tabbed-layouts"></a>Na kartách rozložení


## <a name="overview"></a>Přehled

Karty jsou oblíbených uživatelského rozhraní vzoru v mobilních aplikacích. z důvodu jejich jednoduchost a použitelnost. Poskytují konzistentní a snadný způsob, jak přecházet mezi různými obrazovky v aplikaci. Android má několik rozhraní API pro rozhraní s kartami: 

-   **TabHost** &ndash; Toto je původní rozhraní API pro vytvoření záložkách uživatelská rozhraní. A `TabHost` pomůcky, přidá se do rozložení a slouží jako kontejner pro zobrazení na kartách. Toto rozhraní API od už nepoužívá a jeho použití se nedoporučuje. 

-   **Nadřízených členů** &ndash; Toto je část novou sadu rozhraní API byla zavedena v systému Android 3.0 (API úrovně 11) s cílem poskytnout jednotnou navigaci a rozhraní přepnutí zobrazení. Byly zpět přesně do Android 2.2 (úroveň rozhraní API 8) pomocí [podporu knihovna pro Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; Určuje aktuální, další a předchozí stránky `ViewPager`. `ViewPager` je k dispozici pouze prostřednictvím [podporu knihovna pro Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Další informace o `PagerTabStrip`, najdete v části [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Panel nástrojů** &ndash; `Toolbar` je součástí panelu novější a flexibilnější akce, který nahrazuje `ActionBar`. `Toolbar` je k dispozici v typu Lupa Android 5.0 nebo novější, a je také k dispozici pro starší verze systému Android pomocí [podporu knihovna pro Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) balíček NuGet. 
    `Toolbar` právě komponentu panelu doporučenou akci pro použití v aplikace pro Android.
    Další informace najdete v tématu [nástrojů](~/android/user-interface/controls/tool-bar/index.md). 


Těmto rozhraním API vizuálně liší a nejsou vzájemně kompatibilní. Následující obrazovce zobrazí obrázek `TabHost` a `ActionBar` vedle sebe: 

![Snímky obrazovky TabHost na levé straně a nadřízených členů na pravé straně](images/image01.png)

Tyto nekompatibilní API existovat kvůli významné změny uživatelského rozhraní od Android 3.0 (API úrovně 11). Byl jeden z těchto změn uživatelského rozhraní [akce panelu vzoru návrhu](http://www.androidpatterns.com/uap_pattern/action-bar), vzor určená k poskytování snadného přístupu k funkci navigační a klíč v aplikaci. `ActionBar` Rozhraní API byla zavedena tento vzor podporovat. 

`ActionBar` Rozhraní API je jednodušší a pravděpodobně nabízí lepší uživatelské prostředí. Byly přesně zpět na Android 2.2 a je upřednostňovaný volbou pro aplikace Xamarin.Android. 

`TabHost` Rozhraní API je kompatibilní mezi všemi verzemi systému Android, ale vyžaduje další úsilí použití a není konzistentní s aktuálním [Android pokyny uživatelského rozhraní](http://developer.android.com/design/index.html). Vývojáři se nedoporučuje používat toto rozhraní API a by měl upřednostňovat novější nadřízených členů pro své aplikace Xamarin.Android. 



## <a name="actionbarsherlock"></a>ActionBarSherlock

Než rozhraní API nadřízených členů byly přeneseny zpět do Android 2.2, vývojáři, kteří chtěli novější vzhledu a chování rozhraní API nadřízených členů, ale může použít knihovnu třetích stran [ActionBarSherlock](http://actionbarsherlock.com). ActionBarSherlock je že rozšířením Android knihovna podpory navržené tak, aby backport vzoru návrhu panelu Akce pro Android 2.x. Při spuštění na Android 3.0 nebo vyšší, ActionBarSherlock budou automaticky používat nativního `ActionBar` poskytovaný Androidem rozhraní API. Starší verze systému Android se bude používat vlastní čerpat, které bude napodobovat vzhledu a chování systému novější `ActionBar` rozhraní API. [ActionBarSherlock součást](https://www.nuget.org/packages/xamstore-XamarinActionBarSherlock/) umožňuje snadno přidat ActionBarSherlock do aplikace pro Xamarin.Android. 



## <a name="related-links"></a>Související odkazy

- [TabHost – přehled](tab-host.md)
- [Návod TabHost](~/android/user-interface/layouts/tab-layout/creating-a-tabbed-ui.md)
- [Panel akcí](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Balíček NuGet Android, podporují knihovny v7 kompatibility aplikace](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Knihovna v7 kompatibility aplikace](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
