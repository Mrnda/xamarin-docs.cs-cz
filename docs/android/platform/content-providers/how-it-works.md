---
title: Jak obsahu pracovní zprostředkovatelů
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: b4c674176be5af09d6b780d79923737a364d1591
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767122"
---
# <a name="how-content-providers-work"></a>Jak obsahu pracovní zprostředkovatelů

Se skládá ze dvou tříd `ContentProvider` interakce:

- **ContentProvider** &ndash; implementuje rozhraní API, který zveřejňuje sadu dat standardním způsobem. Hlavní metody jsou dotazu, Insert, Update a Delete.

- **ContentResolver** &ndash; statické proxy server, který komunikuje `ContentProvider` pro přístup k jeho dat, buď v rámci stejné aplikaci, nebo jiná aplikace.

Poskytovateli obsahu je obvykle zajištěna databázi SQLite, ale rozhraní API znamená, že spotřeba kód není nutné znát základní SQL. Dotazy se provádí přes identifikátoru Uri pomocí konstanty Chcete-li názvy sloupců (ke snížení závislosti na podkladová struktura dat) a `ICursor` se vrátí pro kód náročné Iterujte přes.


## <a name="consuming-a-contentprovider"></a>Využívání ContentProvider

`ContentProviders` vystavení jejich funkce prostřednictvím identifikátor Uri, který je zaregistrován v **AndroidManifest.xml** aplikace, která publikuje data. Existuje je konvence kde identifikátor Uri a sloupce dat, které jsou zveřejněné by měly být dostupné jako konstanty, aby bylo snadné vázat na data. Android je integrované `ContentProviders` všechny třídy pohodlí poskytnout konstanty, které odkazují na strukturu dat v [ `Android.Providers` ](https://developer.xamarin.com/api/namespace/Android.Provider/) oboru názvů.



### <a name="built-in-providers"></a>Předdefinované zprostředkovatele

Android nabízí přístup k široké škále systému a dat uživatele pomocí `ContentProviders`:

- *Prohlížeč* &ndash; záložky a prohlížeč historie (vyžaduje oprávnění `READ_HISTORY_BOOKMARKS` nebo `WRITE_HISTORY_BOOKMARKS`).

- *CallLog* &ndash; poslední volání provedené nebo přijímat se zařízením.

- *Kontakty* &ndash; podrobné informace ze seznamu kontaktů uživatele, včetně osoby, telefony, fotografie a skupiny.

- *MediaStore* &ndash; obsah zařízení uživatele: zvuk (alb, umělci, žánry, seznamy stop), bitové kopie (včetně miniatur) a video.

- *Nastavení* &ndash; zařízení systémového nastavení a předvolby.

- *UserDictionary* &ndash; obsah uživatelem definované slovník používaný pro zadávání prediktivní textu.

- *Hlasová pošta* &ndash; historie hlasová pošta zpráv.



## <a name="classes-overview"></a>Přehled třídy

Primární třídy používané při práci se službou `ContentProvider` zde se zobrazují:

[![Diagram aplikace poskytovateli obsahu a interakce aplikace spotřeba – třída](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

V tomto diagramu `ContentProvider` implementuje dotazy a zaregistruje identifikátor URI, na který jiné aplikace pomocí vyhledejte data. `ContentResolver` Funguje jako "proxy" k `ContentProvider` (dotazování, Insert, Update a odstranit metody). `SQLiteOpenHelper` Obsahuje data používaná `ContentProvider`, ale není zveřejněné přímo k použití aplikací.
`CursorAdapter` Předá vrácený kurzor `ContentResolver` zobrazíte v `ListView`. `UriMatcher` Je pomocná třída, která analyzuje identifikátory URI při zpracování dotazů.

Účelem každá třída je popsán dále:

- **ContentProvider** &ndash; implementace metody Tato abstraktní třída vystavit data. Rozhraní API je k dispozici pro jiné třídy a aplikace prostřednictvím atribut identifikátor Uri, který je přidán do definice třídy.

- **SQLiteOpenHelper** &ndash; pomáhá implementovat SQLite úložiště dat, který je zveřejněný prostřednictvím `ContentProvider`.

- **UriMatcher** &ndash; použití `UriMatcher` ve vaší `ContentProvider` implementace ke správě identifikátory URI, které se používají k dotazu obsah.

- **ContentResolver** &ndash; využívání kód používá `ContentResolver` přístupu `ContentProvider` instance. Dvě třídy společně postará o komunikaci mezi procesy problémů, což dat snadno sdílet mezi aplikacemi. Použití kódu nikdy vytvoří `ContentProvider` třídy výslovně; místo toho je přístupu k datům ve vytvoření kurzoru podle identifikátoru Uri vystavené `ContentProvider` aplikace.

- **CursorAdapter** &ndash; použití `CursorAdapter` nebo `SimpleCursorAdapter` k zobrazení dat přistupuje prostřednictvím `ContentProvider`.

`ContentProvider` Rozhraní API umožňuje příjemci k provádění různých operací na data, jako například:

-  Dotaz pro seznamy nebo jednotlivé záznamy vrácená data.
-  Úprava jednotlivých záznamů.
-  Přidáte nové záznamy.
-  Odstraňte záznamy.

Tento dokument obsahuje příklad, který používá zadaný systému `ContentProvider`, a také jednoduchý příklad jen pro čtení, který implementuje vlastní `ContentProvider`.

