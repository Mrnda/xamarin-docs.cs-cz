---
title: Úvod do úložiště dat do aplikace Xamarin.iOS
description: Tento dokument popisuje různé prostředky úložiště dat do aplikace Xamarin.iOS a obsahuje konkrétní informace o výhodách SQLite.
ms.prod: xamarin
ms.assetid: B1994468-FD06-4FD9-96B3-FCEBB13A972A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: c5324d7e7daa8fbefde1776743dbdc595463fe33
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241146"
---
# <a name="introduction-to-data-storage-in-xamarinios-apps"></a>Úvod do úložiště dat do aplikace Xamarin.iOS

## <a name="when-to-use-a-database"></a>Kdy použít databázi

Zatímco roste úložnou a výpočetní kapacitu mobilních zařízení telefonů a tabletů i nadále bude tak hrozit plochy &amp; protějšky přenosném počítači. Z tohoto důvodu je vhodné místo pouze za předpokladu, že databáze je správná odpověď neustále trvá nějakou dobu plánování architektura úložiště dat pro vaši aplikaci. Existuje mnoho různých možností, které vyhovují jiné požadavky, jako například:

-  **Předvolby** – iOS nabízí předdefinovaný mechanismus pro ukládání dat jednoduché páry klíč hodnota. Pokud ukládáte jednoduché uživatelské nastavení nebo malé kousky dat (jako jsou informace o individuálním nastavení), použijte nativní funkce platformy pro ukládání tohoto typu informací. Pro iOS můžete také využít výhod Icloudu synchronizace pro tato data pro zálohování a synchronizaci pro uživatelům mají několik zařízení.
-  **Textové soubory** – vstup uživatele nebo z mezipamětí stažený obsah (např.) HTML) mohou být uloženy přímo v systému souborů. Pomocí odpovídající konvencí pojmenování souboru můžete uspořádat soubory a najít data.
-  **Serializovat datových souborů** – objekty lze nastavit jako trvalý, XML nebo JSON v systému souborů. Rozhraní .NET framework obsahuje knihovny pro serializaci a deserializaci objektů snadno. Použijte odpovídající názvy pro uspořádání datových souborů.
-  **Databáze** – The SQLite databázový stroj je dostupných sad iOS a je užitečné pro ukládání strukturovaných dat, které potřebujete k dotazování, řazení nebo jinak pracovat. Úložiště databáze je vhodné k seznamům dat s více vlastnostmi.
-  **Soubory obrázků** – i když je možné k ukládání binárních dat v databázi na mobilním zařízení, se doporučuje uložit je přímo v systému souborů. V případě potřeby můžete ukládat názvy souborů v databázi a přidružit bitovou kopii s jinými daty. Při zpracování komplexnějších velké obrázky nebo velké množství bitové kopie, je vhodné k plánování strategie ukládání do mezipaměti, který odstraňuje soubory, které už nepotřebujete, aby využívání prostoru úložiště pro všechny uživatele.


Pokud je databáze úložiště mechanismus pro vaši aplikaci, zbývající část tohoto dokumentu popisuje, jak použít SQLite na platformě Xamarin.

## <a name="advantages-of-using-a-database"></a>Mezi výhody používání databáze

Existuje mnoho výhod použití databáze SQL v mobilní aplikaci:

-  SQL Database umožňují efektivní úložiště strukturovaných dat.
-  Konkrétního data může být extrahována složitých dotazech.
-  Může být řazeny výsledky dotazu.
-  Výsledky dotazu se dají agregovat.
-  Vývojářům stávající dovednosti: pište databáze se můžou využívat své znalosti na návrhu databáze a dat přístupový kód.
-  Datový model z komponenty serveru připojenou aplikací může znovu použít (ať už zcela nebo zčásti) v mobilní aplikaci.


## <a name="sqlite-database-engine"></a>Modul databáze SQLite

SQLite je modul open source databáze, která byla přijata společností Apple pro jejich mobilní platformu. Proto neexistuje žádný další práci vývojářům využívat jejich výhod, jsou integrované do zařízení iOS databázový stroj SQLite. SQLite se skvěle hodí pro vývoj mobilních řešení napříč platformami, protože:

-  Databázový stroj je malé, rychlé a snadné přenosné.
-  Databázi je uložena v jednom souboru, který má jednoduchou správu mobilních zařízení.
-  Formát souboru je snadno použitelné napříč platformami: zda 32 - a 64-bit a systémy big - nebo little endian.
-  Implementuje většinu standardní SQL92.


Protože SQLite byla navržena jako malé a rychlé, existují některé upozornění na jeho použití:

-  Některé syntaxe vnější spojení není podporována.
-  Pouze tabulky přejmenovat a ADDCOLUMN jsou podporovány. Nelze provést jiné úpravy schématu.
-  Zobrazení jsou jen pro čtení.


Další informace o SQLite na webu – [SQLite.org](http://SQLite.org) – ale všechny informace, je třeba použít SQLite s využitím kódu Xamarin je obsažen v tomto dokumentu a související ukázky. Databázový stroj SQLite, jsou integrované do všech verzí systému iOS.
I když nejsou zahrnuta v této kapitole, je také k dispozici pro použití v systémech Windows Phone a aplikace Windows SQLite.

## <a name="windows-and-windows-phone"></a>Windows a Windows Phone

SQLite lze také na platformách Windows, i když tyto platformy, které nejsou zahrnuté v tomto dokumentu.
Další informace najdete v [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) a [Tasky Pro](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/building_cross_platform_applications/case_study%3A_tasky) malá a velká studie a zkontrolujte [Tim Heuer blogu](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).



## <a name="related-links"></a>Související odkazy

- [Basic přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Pokročilé přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recepty dat](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
