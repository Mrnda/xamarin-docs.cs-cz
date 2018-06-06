---
title: Úvod do úložiště dat v aplikacích pro Xamarin.iOS
description: Tento dokument popisuje různé způsoby uložení dat v aplikaci Xamarin.iOS a poskytuje konkrétní informace o výhodách SQLite.
ms.prod: xamarin
ms.assetid: B1994468-FD06-4FD9-96B3-FCEBB13A972A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 3fc82e2a1a2cf8d25d934ca2194ecd7de1b8b7df
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784700"
---
# <a name="introduction-to-data-storage-in-xamarinios-apps"></a>Úvod do úložiště dat v aplikacích pro Xamarin.iOS

## <a name="when-to-use-a-database"></a>Kdy použít databázi

Když jsou zvýšení možnosti úložiště a zpracování mobilních zařízení, telefonů a tabletů, stále funkce lag za plochy &amp; svými protějšky přenosný počítač. Z tohoto důvodu je vhodné místo pouze za předpokladu, že databáze je pravé odpovědi vždy trvá delší dobu plánování architektura úložiště dat pro vaši aplikaci. Existuje několik různých možností, která vyhovuje jiné požadavky, jako například:

-  **Předvolby** – iOS nabízí integrovanou mechanismus pro ukládání jednoduché páry klíč hodnota dat. Pokud ukládáte uživatelská jednoduchého nastavení nebo malé data (například informace o přizpůsobení) použijte pro ukládání tento typ informací nativní funkce platformu. Pro iOS můžete také využít Icloudu synchronizace pro tato data pro zálohování a synchronizace pro uživatele s více zařízení.
-  **Textové soubory** – vstup uživatele nebo mezipamětí objektu stažen obsah (např. HTML) mohou být uloženy přímo v systému souborů. Pomocí příslušné konvence pojmenování souboru můžete uspořádat soubory a vyhledávat data.
-  **Serializovat datové soubory** – objekty můžete zachovat jako soubor XML nebo JSON v systému souborů. Rozhraní .NET framework zahrnuje knihovny, které serializaci a zrušte serializaci objektů snadné. Použijte vhodná jména k uspořádání datových souborů.
-  **Databáze** – SQLite databázový stroj je k dispozici iOS a jsou užitečné pro ukládání strukturovaných dat, které potřebujete k dotazování, řazení nebo jinak manipulaci. Úložiště databáze je vhodnější seznam dat pomocí mnoho vlastností.
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

SQLite je open-source databázový stroj, který přijal Apple pro své mobilní platformu. Databázový stroj SQLite je integrovaný do systému iOS, proto není k dispozici žádné další kroky pro vývojáře k jej využít. SQLite je skvěle hodí pro mobilní vývoj pro různé platformy, protože:

-  Databázový stroj je malý, rychlý a snadno přenosné.
-  Databáze je uložené v jednom souboru, který se snadno spravovat na mobilních zařízeních.
-  Formát souboru je snadno použitelný napříč platformami: zda 32 - nebo 64bitová verze a systémy big - nebo little endian.
-  Implementuje většinu standardní SQL92.


Protože SQLite je navržen pro malé a rychlé, existují některé upozornění na jeho použití:

-  Některé syntaxe vnější spojení není podporována.
-  Pouze tabulky přejmenovat a ADDCOLUMN jsou podporovány. Nelze provést další změny schématu.
-  Zobrazení jsou jen pro čtení.


Další informace o SQLite na webu – [SQLite.org](http://SQLite.org) – ale všechny informace, které budete muset použít SQLite s Xamarinem je obsažené v tomto dokumentu a související ukázky. Databázový stroj SQLite je integrované do všech verzí systému iOS.
I když nejsou zahrnuté v této kapitoly, SQLite je také k dispozici pro použití na Windows Phone a aplikace systému Windows.

## <a name="windows-and-windows-phone"></a>Systém Windows a Windows Phone

SQLite lze také na platformách systému Windows, i když tyto platformy nejsou zahrnuté v tomto dokumentu.
Další informace najdete v [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) a [Tasky Pro](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/building_cross_platform_applications/case_study%3A_tasky) studie případu a zkontrolujte [Tim Heuer blog](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).



## <a name="related-links"></a>Související odkazy

- [Přístup Basic (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Rozšířené přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recepty dat](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
