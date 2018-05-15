---
title: Na kartách rozložení
description: Přehled záložkách rozložení v Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/08/2017
ms.openlocfilehash: 53ed5f91583d43839e96388194aea8c0d6ac5315
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="tabbed-layouts"></a>Na kartách rozložení


## <a name="overview"></a>Přehled

Karty jsou oblíbených uživatelského rozhraní vzoru v mobilních aplikacích. z důvodu jejich jednoduchost a použitelnost. Poskytují konzistentní a snadný způsob, jak přecházet mezi různými obrazovky v aplikaci. Android má několik rozhraní API pro rozhraní s kartami: 

-   **Nadřízených členů** &ndash; Toto je část novou sadu rozhraní API byla zavedena v systému Android 3.0 (API úrovně 11) s cílem poskytnout jednotnou navigaci a rozhraní přepnutí zobrazení. Byly zpět přesně do Android 2.2 (úroveň rozhraní API 8) pomocí [podporu knihovna pro Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; Určuje aktuální, další a předchozí stránky `ViewPager`. `ViewPager` je k dispozici pouze prostřednictvím [podporu knihovna pro Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Další informace o `PagerTabStrip`, najdete v části [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Panel nástrojů** &ndash; `Toolbar` je součástí panelu novější a flexibilnější akce, který nahrazuje `ActionBar`. `Toolbar` je k dispozici v typu Lupa Android 5.0 nebo novější, a je také k dispozici pro starší verze systému Android pomocí [podporu knihovna pro Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) balíček NuGet. 
    `Toolbar` právě komponentu panelu doporučenou akci pro použití v aplikace pro Android.
    Další informace najdete v tématu [nástrojů](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="related-links"></a>Související odkazy

- [Karty podstatným návrhu -](https://material.io/guidelines/components/tabs.html)- [nadřízených členů.](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Balíček NuGet Android, podporují knihovny v7 kompatibility aplikace](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Knihovna v7 kompatibility aplikace](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
