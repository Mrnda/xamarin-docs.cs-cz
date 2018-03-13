---
title: "iOS přístup k datům"
description: "Většina aplikací mít některé požadavek k uložení dat v zařízení místně. Pokud je trivially malé množství dat, většinou je potřeba databáze a datové vrstvy v aplikaci pro správu přístupu k databázi. iOS má databázový stroj SQLite \"součástí\" a přístup k uložení a načtení dat se zjednodušilo díky Xamarin pro platformu. Tento dokument ukazuje, jak získat přístup k databázi SQLite."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 6dba862055cc266b2af1eaf87418fe7ebf5830c5
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="ios-data-access"></a>iOS přístup k datům

_Většina aplikací mít některé požadavek k uložení dat v zařízení místně. Pokud je trivially malé množství dat, většinou je potřeba databáze a datové vrstvy v aplikaci pro správu přístupu k databázi. iOS má databázový stroj SQLite "součástí" a přístup k uložení a načtení dat se zjednodušilo díky Xamarin pro platformu. Tento dokument ukazuje, jak získat přístup k databázi SQLite._

Přístup k databázi rozhraní API podporuje Xamarin.iOS například:

-  Architektura technologie ADO.NET.
-  SQLite NET 3. stran knihovny.

Tato příručka obsahuje podrobný přehled databáze obecně před popisující, jak nastavit ADO.NET a SQLite.NET a přístup k databázím SQLite ve svých aplikacích Xamarin.iOS. 

Většinu kódu v tomto dokumentu je zcela platformy a bude spuštěna v iOS nebo Android beze změny. Existují dvě ukázkových aplikací, které jsou popsané:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – jednoduché datové operace zapíše výsledky do textového zobrazení ovládacího prvku;
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – integruje operace dat v malých funkční aplikaci, která obsahuje seznam a upravuje jednoduché datové struktury.

Obě řešení ukázka obsahovat iOS a Android ukázkové aplikace projekty.

Xamarin.Forms aplikace, najdete v tématu [práce s databázemi](~/xamarin-forms/app-fundamentals/databases.md) které vysvětluje, jak pracovat s SQLite knihovny PCL s Xamarin.Forms.

## <a name="sections"></a>Oddíly

-  [Úvod](introduction.md)
-  [Konfigurace](configuration.md)
-  [Používání SQLite.NET ORM](using-sqlite-orm.md)
-  [Používání ADO.NET](using-adonet.md)
-  [Používání dat v aplikaci](using-data-in-an-app.md)


## <a name="summary"></a>Souhrn

Tato kapitola popsané přístup k datům v Xamarin.iOS pomocí SQLite jako databázového stroje. Databáze je přístupná "přímo" pomocí syntaxe ADO.NET, nebo můžete zahrnout SQLite.NET ORM a provádění operací s daty v jazyce C#.

Jsme přečetli dvou vzorcích: jeden, který obsahuje velmi jednoduché datové přístupový kód, který výstupy do textového pole a jednoduchou aplikaci, která zahrnuje vytváření, čtení, aktualizace a odstranění funkce. Můžeme také popsané dělení na vlákna a postup počáteční hodnoty vaší aplikace pomocí předem vyplněná databáze SQLite.

Další příklady přístup k datům a platformy najdete v tématu naše [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) Případová studie.

## <a name="related-links"></a>Související odkazy

- [Přístup Basic (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Rozšířené přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recepty dat](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
