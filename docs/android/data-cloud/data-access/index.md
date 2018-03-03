---
title: "Přístup k datům Xamarin.Android"
description: "Většina aplikací mít některé požadavek k uložení dat v zařízení místně. Pokud je trivially malé množství dat, většinou je potřeba databáze a datové vrstvy v aplikaci pro správu přístupu k databázi.  Android má SQLite databázový stroj, součástí, a přístup k uložení a načtení dat se zjednodušilo díky Xamarin pro platformu. Tento dokument ukazuje, jak pro přístup k databázi SQLite způsobem napříč platformami."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 301c4330afe6ff7808ca7b09f6cc5260517aae43
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-data-access"></a>Přístup k datům Xamarin.Android

_Většina aplikací mít některé požadavek k uložení dat v zařízení místně. Pokud je trivially malé množství dat, většinou je potřeba databáze a datové vrstvy v aplikaci pro správu přístupu k databázi.  Android má SQLite databázový stroj, součástí, a přístup k uložení a načtení dat se zjednodušilo díky Xamarin pro platformu. Tento dokument ukazuje, jak pro přístup k databázi SQLite způsobem napříč platformami._

## <a name="data-access-overview"></a>Přehled přístupu k datům

Většina aplikací mít některé požadavek k uložení dat v zařízení místně. Pokud je trivially malé množství dat, většinou je potřeba databáze a datové vrstvy v aplikaci pro správu přístupu k databázi. Android obou má databázový stroj Sqlite "součástí" a přístup k datům se zjednodušilo díky Xamarin na platformě, která se dodává s zprostředkovatele dat SQLite.

Přístup k databázi rozhraní API podporují Xamarin.Android, jako:

-  Architektura technologie ADO.NET.
-  SQLite NET 3. stran knihovny.

Většinu kódu v této části je zcela platformy a bude spuštěna v iOS nebo Android beze změny. Existují dvě ukázkových aplikací, které jsou popsané:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; jednoduché datové operace zapíše výsledky do textového zobrazení ovládacího prvku;

-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; integruje operace dat v malých funkční aplikaci, která obsahuje seznam a upravuje jednoduché datové struktury.

Obě řešení ukázka obsahovat iOS a Android ukázkové aplikace projekty.

Xamarin.Forms aplikace, najdete v tématu [práce s databázemi](~/xamarin-forms/app-fundamentals/databases.md) které vysvětluje, jak pracovat s SQLite knihovny PCL s Xamarin.Forms.

Témata v této části popisují přístup k datům v Xamarin.Android pomocí SQLite jako databázového stroje. Databáze je přístupná "přímo" pomocí syntaxe ADO.NET, nebo můžete zahrnout SQLite.NET ORM a provádění operací s daty v jazyce C#.

Jsou kontrolovány dvou vzorcích: jeden, který obsahuje velmi jednoduché datové přístupový kód, který výstupy do textového pole a jednoduchou aplikaci, která zahrnuje vytváření, čtení, aktualizace a odstranění funkce. Také je popsána zřetězení a postup počáteční hodnoty vaší aplikace pomocí předem vyplněná databáze SQLite.

Další příklady přístup k datům a platformy najdete v tématu naše [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) Případová studie.


## <a name="related-links"></a>Související odkazy

- [Přístup Basic (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Rozšířené přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recepty dat v androidu](https://developer.xamarin.com/recipes/android/data/)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
