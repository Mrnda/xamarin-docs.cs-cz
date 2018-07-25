---
title: Přístup k datům Xamarin.iOS
description: Tento dokument obsahuje odkazy na příručky, které popisují, jak pracovat s místními databázemi aplikace pro Xamarin.iOS. Obsah odkazovaný popisuje SQLite.NET, ADO.NET a další.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 017118a74528718ea99fe218f443b8df83d7c52e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241276"
---
# <a name="xamarinios-data-access"></a>Přístup k datům Xamarin.iOS

Xamarin.iOS podporuje rozhraní API pro přístup k databázi, jako:

-  Architektura ADO.NET.
-  3. SQLite NET knihovny strany.

Tato příručka obsahuje přehled databází obecně před popisující, jak nastavit ADO.NET a SQLite.NET a přistupuje k databázím SQLite aplikace Xamarin.iOS. 

Většina kódu v tomto dokumentu je zcela multiplatformní a poběží beze změny v Iosu nebo Androidu. Existují dvě ukázkové aplikace popsané:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – zapíše jednoduché datové operace ovládacího prvku; zobrazí výsledky do textu
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – malý funkční aplikaci, která obsahuje seznam a upraví jednoduchou datovou strukturu integruje se s operace s daty.

Obě ukázkové řešení obsahují projekty aplikací ukázka pro Android a iOS.

U aplikací Xamarin.Forms, přečtěte si [práce s databázemi](~/xamarin-forms/app-fundamentals/databases.md) což vysvětluje, jak pracovat s SQLite v knihovně PCL s Xamarin.Forms.

## <a name="sections"></a>Oddíly

-  [Úvod](introduction.md)
-  [Konfigurace](configuration.md)
-  [Používání SQLite.NET ORM](using-sqlite-orm.md)
-  [Používání ADO.NET](using-adonet.md)
-  [Používání dat v aplikaci](using-data-in-an-app.md)

## <a name="summary"></a>Souhrn

Tato kapitola popsány přístup k datům v Xamarin.iOS pomocí jako databázový stroj SQLite. Databáze je přístupná "přímo" pomocí syntaxe ADO.NET nebo můžete zahrnout SQLite.NET ORM a provádět operace s daty v jazyce C#.

Jsme se zaměřili na dvou vzorcích: ten, který obsahuje velmi jednoduchou datovou přístupový kód, který výstupy do textového pole a jednoduchou aplikaci, která zahrnuje vytváření, čtení, aktualizace a odstranění. Jsme probírali také práce s vlákny a jak naplnit vaší aplikace pomocí předem naplněných databáze SQLite.

Další příklady přístup k datům napříč platformami najdete v našich [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) případovou studii.

## <a name="related-links"></a>Související odkazy

- [Basic přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Pokročilé přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recepty dat](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
