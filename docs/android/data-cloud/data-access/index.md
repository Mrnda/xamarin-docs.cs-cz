---
title: Přístup k datům Xamarin.Android
description: Většina aplikací má nějakou požadavek na uložení dat na zařízení místně. Pokud je triviálně malá množství dat, to obvykle vyžaduje databáze a datové vrstvy v aplikaci, která chcete spravovat přístup k databázi.  Android má databázový stroj SQLite "integrované" a zjednodušuje přístup k ukládání a načítání dat Xamarinu pro platformu. Tento dokument ukazuje, jak získat přístup k databázi SQLite způsobem napříč platformami.
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 0ee89728395134f26972ebefa28ca96925b98c84
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241091"
---
# <a name="xamarinandroid-data-access"></a>Přístup k datům Xamarin.Android

_Většina aplikací má nějakou požadavek na uložení dat na zařízení místně. Pokud je triviálně malá množství dat, to obvykle vyžaduje databáze a datové vrstvy v aplikaci, která chcete spravovat přístup k databázi.  Android má databázový stroj SQLite "integrované" a zjednodušuje přístup k ukládání a načítání dat Xamarinu pro platformu. Tento dokument ukazuje, jak získat přístup k databázi SQLite způsobem napříč platformami._

## <a name="data-access-overview"></a>Přehled přístupu k datům

Většina aplikací má nějakou požadavek na uložení dat na zařízení místně. Pokud je triviálně malá množství dat, to obvykle vyžaduje databáze a datové vrstvy v aplikaci, která chcete spravovat přístup k databázi. Android i má databázový stroj Sqlite "integrované" a zjednodušuje přístup k datům Xamarinu pro platformu, která se dodává s poskytovatelem dat SQLite.

Xamarin.Android podporují rozhraní API pro přístup k databázi jako je například:

-  Architektura ADO.NET.
-  3. SQLite NET knihovny strany.

Většina kódu v této části je zcela multiplatformní a poběží beze změny v Iosu nebo Androidu. Existují dvě ukázkové aplikace popsané:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; zapíše jednoduché datové operace ovládacího prvku; zobrazí výsledky do textu

-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; malý funkční aplikaci, která obsahuje seznam a upraví jednoduchou datovou strukturu integruje se s operace s daty.

Obě ukázkové řešení obsahují projekty aplikací ukázka pro Android a iOS.

U aplikací Xamarin.Forms, přečtěte si [práce s databázemi](~/xamarin-forms/app-fundamentals/databases.md) což vysvětluje, jak pracovat s SQLite v knihovně PCL s Xamarin.Forms.

Témata v této části popisují přístup k datům v Xamarin.Android pomocí jako databázový stroj SQLite. Databáze je přístupná "přímo" pomocí syntaxe ADO.NET nebo můžete zahrnout SQLite.NET ORM a provádět operace s daty v jazyce C#.

Oba vzorky jsou kontrolovány: ten, který obsahuje velmi jednoduchou datovou přístupový kód, který výstupy do textového pole a jednoduchou aplikaci, která zahrnuje vytváření, čtení, aktualizace a odstranění. Zřetězení a tom, jak naplnit vaší aplikace pomocí předem naplněných databázi SQLite jsou také popsány.

Další příklady přístup k datům napříč platformami najdete v našich [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) případovou studii.


## <a name="related-links"></a>Související odkazy

- [Basic přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Pokročilé přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recepty dat pro Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
