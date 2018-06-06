---
title: Běžné vzory a idioms v Xamarin.Mac
description: Tento dokument popisuje běžné vzory návrhu použít při vytváření Xamarin.Mac aplikace. Popisuje model-view-controller vzor, datové zdroje a delegáta vzory a protokoly.
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/17/2016
ms.openlocfilehash: f6bba5575edf2dcbddbd354b590e03f9fed06291
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791534"
---
# <a name="common-patterns-and-idioms-in-xamarinmac"></a>Běžné vzory a idioms v Xamarin.Mac

V celém rozhraní API Apple zveřejňovány prostřednictvím jazyka C# určitých idioms a vzory se spustit znovu a znovu. Pokud máte zkušenosti s programování s Xamarin.iOS, může to vypadat povědomě. Dokumentace bude často odkazovat tyto vzory a idioms opakovaně, tak mít solidní znalosti z nich vám pomůže smysl, které můžete najít dokumentaci.

## <a name="mvc---model-view-controller"></a>MVC – Model View Controller

Model View Controller nebo MVC pro zkrácení, je velmi běžné vzor nalézt v celém kakao. Podrobnou diskuzi je nad rámec tohoto dokumentu, ale Stručný postup je způsob vytváření struktury vaší aplikace do komponenty:

- **Model** objekty představují základní se data zobrazit a upravit (např. adresy v adresáři)
- **Zobrazení** objekty zpracování kreslení daný objekt na obrazovce a zpracování interakce uživatele (pole textu na obrazovce zobrazuje adresu)
- **Řadič** objekty zpracování interakce mezi modelu a zobrazení. Jejich změny modelu push "až" push "dolů" změny ze zobrazení, když uživatelé provádět změny v uživatelském rozhraní a aktualizovat zobrazení.

Pokud jste obeznámeni s modelem MVVM (Model View ViewModel) z jiné knihovny například WPF, řadičem funguje podobně jako ViewModel, ale je často přesněji vázaný na konkrétní prvky uživatelského rozhraní.

Další informace naleznete zde:

- [Učení MVC na webu společnosti Apple](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Model View Controller v Objective-C](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Práce s Windows](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>Zdroj dat / delegovat / vytvoření podtřídy

Jiné velmi běžný vzor v kakao se zabývá poskytuje data k elementům uživatelského rozhraní a reaguje na akce uživatelů. Pomocí `NSTableView` jako příklad, musíte pro každý řádek nějakým způsobem zadat data, některé nastavte prvků uživatelského rozhraní představující řádek, některé sadu chování reagování na interakce uživatele a pravděpodobně určitou část vlastního nastavení. Datový zdroj a delegáta vzory umožňují zpracování většinou bez nutnosti uchýlit k vytváření podtříd `NSTableView` sami.

- `DataSource` Vlastnost je přiřazen k instanci vlastní podtřídou třídy `NSTableViewDataSource` tzv. k vyplnění tabulky s daty (prostřednictvím `GetRowCount` a `GetObjectValue`).

- `Delegate` Vlastnost je přiřazen k instanci vlastní podtřídou třídy `NSTableViewDelegate` která poskytuje zobrazení pro daný model objektu (prostřednictvím `GetViewForItem`) a zpracovává interakce uživatelského rozhraní (prostřednictvím `DidClickTableColumn`, `MouseDownInHeaderOfTableColumn`atd).

V některých případech budete chtít nastavit ovládacího prvku způsobem nad rámec háky zadaný ve zdroji delegáta nebo data a podtřídami zobrazení můžete přímo. Ale buďte opatrní, v mnoha případech přepsání výchozího chování se pak vyžadují, abyste zpracovávaly všechny tohoto chování sami (přizpůsobení chování výběru může vyžadovat k implementaci všech chování výběru sami).

V Xamarin.iOS, některé rozhraní API, například `UITableView` jsou teď rozšířené o vlastnost, která implementuje delegát a zdroje dat (`UITableViewSource`). To je běžné omezení, která jedné třídy jazyka C# může mít pouze jeden základní třídy a naše zpřístupnění protokolů můžete obejít se provádí prostřednictvím základní třídy.

Další informace o práci se zobrazení tabulek v aplikaci Xamarin.Mac, najdete v tématu naše [zobrazení tabulky](~/mac/user-interface/table-view.md) dokumentaci.

## <a name="protocols"></a>protokoly

Protokoly v Objective C je možné porovnávat na rozhraní v jazyce C# a v mnoha případech se používají v situacích, podobně jako. Například `NSTableView` příkladu výše, delegát a zdroji dat jsou ve skutečnosti protokoly. Xamarin.Mac zpřístupní jako základní třídy s virtuální metody, které můžete změnit. Hlavní rozdíl mezi rozhraní jazyka C# a protokoly jazyka Objective-C je, že některé metody v protokolu může být volitelné k implementaci. Můžete si prohlédnout si dokumentaci a definice rozhraní API, chcete-li zjistit, co je volitelné.

Další informace najdete v tématu naše [delegáti, protokoly a události](~/ios/app-fundamentals/delegates-protocols-and-events.md) dokumentaci.



## <a name="related-links"></a>Související odkazy

- [Zobrazení tabulek](~/mac/user-interface/table-view.md)
- [Práce s windows](~/mac/user-interface/window.md)
- [Delegáti, protokoly a události](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
