---
title: Karta rozložení s TabHost
description: Tento článek poskytuje podrobný přehled TabHost, starší API používá k vytvoření záložkách rozložení v aplikaci Xamarin.Android.
ms.prod: xamarin
ms.assetid: 77B890A4-27A6-41DF-81BA-22C6116A8FB2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: ae5b9020e08575bcd453703f3df14f63b288d2f5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="tab-layout-with-tabhost"></a>Karta rozložení s TabHost

_Tento článek poskytuje podrobný přehled TabHost, starší API používá k vytvoření záložkách rozložení v aplikaci Xamarin.Android._


## <a name="overview"></a>Přehled

> [!NOTE]
> `TabHost` je starý rozhraní API, které se už nepoužívá Google. Vývojáři se doporučuje vytvořit záložkách aplikací pomocí [nadřízených členů](~/android/user-interface/controls/action-bar.md). `ActionBar` Je k dispozici ve všech verzí systému Android. To bylo poprvé dostupné ve Android 3.0 (API úrovně 11) a byl přesně zpět do Android 2.2 (úroveň rozhraní API 8) a Android 2.3 (API úrovně 10) v [V7 kompatibility aplikace knihovny](http://developer.android.com/tools/support-library/features.html#v7-appcompat), což je k dispozici pro Xamarin.Android prostřednictvím [Xamarin Knihovna pro Android podporu - V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) balíčku.

`TabHost` Je starší, původní rozhraní API pro vytváření interfacesIt záložkách uživatele je nejvhodnější pro aplikace Xamarin.Android, která musí podporovat Android 2.2 a Android 2.3 a nemůžete použít **ActionBarSherlock**.
Následujících pět součástí jsou všechny spojené s `TabHost` rozhraní API:

-  **TabActivity** &ndash; Toto je specializovaná zobrazení, který slouží jako kontejner pro `TabHost` (popsaný níže).

-  **TabHost** &ndash; Toto je kontejner pro záložkách uživatelského rozhraní. Počítač hostuje dvě podřízených prvků: seznam kartě popisky a `FrameLayout` , zobrazí obsah karty.

-  **TabWidget** &ndash; tento ovládací prvek zobrazí seznam karta štítků, jeden pro každé kartě v `TabHost` . V každé kartě `TabHost` jsou reprezentované pomocí `TabHost.TabSpec` objektu. Tento objekt obsahuje metadata pro každé kartě. Když uživatel vybere na kartě `TabHost` odpoví na výběr zobrazením příslušnou kartu.

-  **FrameLayout** &ndash; záložkách uživatelského rozhraní musí mít `FrameLayout` obsažené uvnitř `TabHost`. Slouží k zobrazení obsahu karty.

-  **Aktivity nebo zobrazení** &ndash; při výběru karty zobrazí aktivitu nebo zobrazení ve `FrameLayout`.

Následující diagram znázorňuje, jak všechny tyto součásti vztahují společně:

![Diagram ilustrující rozložení rámce v rámci TabWidget v rámci TabHost](tab-host-images/image03.png)

Karta obsah může být aktivity nebo zobrazení. Zobrazení jsou poměrně jednoduché a jednoduchý, ale může vést k velkému množství co habitating nesouvisejícími kód v aktivitě. Tato akce způsobí nízký oddělit otázky a opakovaném třídu, která je obtížné spravovat. Naproti tomu aktivity vyžadují systémových prostředků, ale povolit pro více modulární přístup s logiku pro každé kartě zapouzdřené v odlišné třída vlastní.


## <a name="summary"></a>Souhrn

Tento článek vysvětlené nejvyšší úrovni součástí starší `TabHost` rozhraní API pro Android a jak spolu souvisí společně.



## <a name="related-links"></a>Související odkazy

- [Panel akcí](http://developer.android.com/guide/topics/ui/actionbar.html)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabHost.TabSpec](https://developer.xamarin.com/api/type/Android.Widget.TabHost+TabSpec/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [Panel akcí](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Balíček NuGet Android, podporují knihovny v7 kompatibility aplikace](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Knihovna v7 kompatibility aplikace](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
