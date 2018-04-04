---
title: Úvod
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/28/2017
ms.openlocfilehash: a2d92b6f7bfa1e2a40e30d0b832dd8a8f737cee2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction"></a>Úvod

## <a name="when-to-use-a-database"></a>Kdy použít databázi

Když jsou zvýšení možnosti úložiště a zpracování mobilních zařízení, funkce lag za jejich stolní a přenosné protějšky telefonů a tabletů, stále. Z tohoto důvodu je vhodné místo pouze za předpokladu, že databáze je pravé odpovědi vždy trvá delší dobu plánování architektura úložiště dat pro vaši aplikaci. Existuje několik různých možností, která vyhovuje jiné požadavky, jako například:

-  **Předvolby** – Android nabízí integrovanou mechanismus pro ukládání jednoduché páry klíč hodnota dat. Pokud ukládáte uživatelská jednoduchého nastavení nebo malé data (například informace o přizpůsobení) použijte pro ukládání tento typ informací nativní funkce platformu.
-  **Textové soubory** – vstup uživatele nebo mezipamětí objektu stažen obsah (např. HTML) mohou být uloženy přímo v systému souborů. Pomocí příslušné konvence pojmenování souboru můžete uspořádat soubory a vyhledávat data.
-  **Serializovat datové soubory** – objekty můžete zachovat jako soubor XML nebo JSON v systému souborů. Rozhraní .NET framework zahrnuje knihovny, které serializaci a zrušte serializaci objektů snadné. Použijte vhodná jména k uspořádání datových souborů.
-  **Databáze** – SQLite databázový stroj je k dispozici na platformě Android a je užitečná pro ukládání strukturovaných dat, které potřebujete k dotazování, řazení nebo jinak manipulaci. Úložiště databáze je vhodnější seznam dat pomocí mnoho vlastností.
-  **Soubory obrázků** – Přestože je možné uložit binární data v databázi v mobilním zařízení, doporučujeme uložit je přímo v systému souborů. V případě potřeby můžete ukládat názvy souborů v databázi pro přidružení bitovou kopii s ostatními daty. Při plánování práce s velkých obrázků nebo velký počet obrázků, je dobrým zvykem plánování strategie ukládání do mezipaměti, který odstraní soubory, aby se zabránilo využívání prostoru úložiště pro všechny uživatele již nepotřebujete.

Pokud je databáze správné úložiště mechanismus pro vaši aplikaci, zbývající část tohoto dokumentu popisuje jak používat SQLite na platformě Xamarin.

## <a name="advantages-of-using-a-database"></a>Výhody používání databáze

Existuje několik výhod používání databáze SQL v mobilní aplikaci:

-  Databáze SQL umožňuje efektivní uložení strukturovaná data.
-  Konkrétní data mohou být extrahuje složitých dotazů.
-  Výsledky dotazu může být seřazeny.
-  Výsledky dotazu se dají agregovat.
-  Vývojářům existujících dovedností databáze můžete využít svými znalostmi k návrhu kód přístup k databázi a data.
-  Datový model z komponenty serveru k připojené aplikaci znovu lze (celé nebo částečně) v mobilní aplikaci.


## <a name="sqlite-database-engine"></a>SQLite databázový stroj

SQLite je open-source databázový stroj, který přijal Google pro své mobilní platformu. Databázový stroj SQLite je integrované do obou operačních systémů, takže není žádné další kroky pro vývojáře k jej využít. SQLite je skvěle hodí pro mobilní vývoj pro různé platformy, protože:

-  Databázový stroj je malý, rychlý a snadno přenosné.
-  Databáze je uložené v jednom souboru, který se snadno spravovat na mobilních zařízeních.
-  Formát souboru je snadno použitelný napříč platformami: zda 32 - nebo 64bitová verze a systémy big - nebo little endian.
-  Implementuje většinu standardní SQL92.


Protože SQLite je navržen pro malé a rychlé, existují některé upozornění na jeho použití:

-  Některé syntaxe vnější spojení není podporována.
-  Pouze tabulky přejmenovat a ADDCOLUMN jsou podporovány. Nelze provést další změny schématu.
-  Zobrazení jsou jen pro čtení.


Další informace o SQLite na webu – [SQLite.org](http://SQLite.org) – ale všechny informace, které budete muset použít SQLite s Xamarinem je obsažené v tomto dokumentu a související ukázky. Databázový stroj SQLite byl podporován Android od verze Android 2.
I když nejsou zahrnuté v této kapitoly, SQLite je také k dispozici pro použití na Windows Phone a aplikace systému Windows.

## <a name="windows-and-windows-phone"></a>Systém Windows a Windows Phone

SQLite lze také na platformách systému Windows, i když tyto platformy nejsou zahrnuté v tomto dokumentu.
Další informace najdete v [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) a [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) studie případu a zkontrolujte [Tim Heuer blog](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).


## <a name="related-links"></a>Související odkazy

- [Přístup Basic (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Rozšířené přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recepty dat v androidu](https://developer.xamarin.com/recipes/android/data/)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
