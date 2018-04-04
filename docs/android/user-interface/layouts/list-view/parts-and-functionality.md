---
title: ListView částí a funkce
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: ec9b6583cac9478957e14f709c7c7ee8eb273cbc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="listview-parts-and-functionality"></a>ListView částí a funkce


## <a name="overview"></a>Přehled

A `ListView` se skládá z následujících částí:

- **Řádky** &ndash; viditelné reprezentace dat v seznamu.

- **Adaptér** &ndash; nevizuálních třídu, která váže zdroj dat k zobrazení seznamu.

- **Rychlé posouvání** &ndash; popisovač, který umožňuje uživatelům posuňte délka seznamu.

- **Část Index** &ndash; element uživatelského rozhraní, která překrývat posouvání řádků k označení, kde v seznamu aktuální řádky se nacházejí.

Tyto snímky obrazovky použijte základní `ListView` řízení zobrazit, jak jsou zpracovávány rychlého posouvání a Index oddílu:

[![Snímky obrazovek aplikací pomocí prostý staré řádky, rychlé posouvání a index oddílu](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

Prvky, které tvoří `ListView` jsou podrobněji popsané v následující:


## <a name="rows"></a>Řádky

Každý řádek má svou vlastní `View`. Zobrazení může být buď z předdefinovaných zobrazení definovaná v `Android.Resources`, nebo vlastní zobrazení. Každý řádek můžete použít se stejné rozvržení zobrazení nebo se můžou být různé. V tomto dokumentu pomocí předdefinovaných rozložení a dalších informací, jak definovat vlastní rozložení jsou příklady.


## <a name="adapter"></a>Adaptér

`ListView` Vyžaduje ovládací prvek `Adapter` k poskytování formátovaný `View` pro každý řádek. Android má integrovanou adaptéry a zobrazení, které lze použít, nebo můžete vytvořit vlastní třídy.


## <a name="fast-scrolling"></a>Rychlé posouvání

Když `ListView` obsahuje mnoho řádků dat posouvání fast lze povolit pomoct uživateli, přejděte na všechny části seznamu. Fast posouvání 'posuvníku, může být volitelně povoleno (a přizpůsobené na úrovni rozhraní API 11 a vyšší).


## <a name="section-index"></a>Index oddílu

Při procházení dlouhými seznamy, poskytuje index volitelné části uživatele s zpětnou vazbu na jaké části seznamu, se aktuálně prohlížíte. Jenom je vhodné na dlouhými seznamy, obvykle ve spojení s rychlé posouvání.


## <a name="classes-overview"></a>Přehled třídy

Primární třídy používané k zobrazení `ListViews` zde se zobrazují:

[![UML diagram ilustrující vztahy mezi ListView a související třídy](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

Účelem každá třída je popsán dále:

- **ListView** &ndash; element uživatelského rozhraní, obsahující posuvného kolekce řádků. Na telefony se obvykle používá přes celou obrazovku (v takovém případě `ListActivity` třídu lze použít) nebo to může být součástí větší rozložení na telefon nebo tablet zařízení.

- **Zobrazení** &ndash; zobrazení v Android může být libovolný element uživatelského rozhraní, ale v kontextu `ListView` vyžaduje `View` poskytované pro každý řádek.

- **BaseAdapter** &ndash; základní třída pro implementace adaptéru pro vazbu `ListView` ke zdroji dat.

- **ArrayAdapter** &ndash; předdefinované adaptér třídu, která sváže pole řetězců `ListView` pro zobrazení. Obecná `ArrayAdapter<T>` dělá to stejné pro ostatní typy.

- **CursorAdapter** &ndash; použití `CursorAdapter` nebo `SimpleCursorAdapter` k zobrazení dat založené na dotazu SQLite.

Tento dokument obsahuje jednoduché příklady, které používají `ArrayAdapter` i složitější příklady, které vyžadují vlastní implementace `BaseAdapter` nebo `CursorAdapter`.

