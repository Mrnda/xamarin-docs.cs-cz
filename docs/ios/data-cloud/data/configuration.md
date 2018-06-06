---
title: Konfigurace SQLite v Xamarin.iOS
description: Tento dokument popisuje, jak k určení umístění souboru databáze SQLite aplikace pro Xamarin.iOS. Tyto koncepty jsou relevantní bez ohledu na to mechanismus přístupu vybraná data.
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 57645c0bb947a60a3d74436b752210d1bac18124
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784378"
---
# <a name="configuring-sqlite-in-xamarinios"></a>Konfigurace SQLite v Xamarin.iOS

Pro použití ve vaší aplikaci Xamarin.iOS, budete muset určit správný soubor umístění souboru databáze SQLite.

## <a name="database-file-path"></a>Cestu k souboru databáze

Bez ohledu na to, jakou metodu přístupu dat můžete použít musíte vytvořit soubor databáze před data se uloží s SQLite. Umístění souboru se liší v závislosti na platformě cílení. Pro iOS třídu můžete použít prostředí vytvořit platnou cestu, jak je znázorněno v následující fragment kódu:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Existují další co je potřeba vzít v úvahu při rozhodování, kam uložit soubor databáze. V systému iOS můžete databázi na zálohované automaticky (nebo ne).

Pokud chcete použít jiné umístění na jednotlivých platformách ve vaší aplikaci křížové platformy můžete direktivu kompilátoru znázorněné ke generování jinou cestu pro každou platformu:

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

Odkazovat [práce pomocí systému souborů](~/ios/app-fundamentals/file-system.md) článku Další informace o umístění, ve kterých soubor používat v systému iOS. Najdete v článku [vytváření křížové platformy aplikací](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) dokumentu pro další informace o použití direktivy kompilátoru napsat kód, které jsou specifické pro každou platformu.

## <a name="threading"></a>Dělení na vlákna

Neměli byste používat stejné připojení databáze SQLite napříč více vláken. Dávejte pozor, otevřít, použití a pak ukončete všechna připojení, které vytvoříte ve stejném vlákně.

Aby se zajistilo, že kód není pokouší o přístup k databázi SQLite z více vláken ve stejnou dobu, ručně proveďte zámek vždy, když chcete přístup k databázi, například takto:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Všechny přístup k databázi (čtení, zápisu, aktualizace atd.) by měl být uzavřen s stejné zámek. Musí dát pozor na vyhněte situaci zablokování zajištěním, že práce v klauzuli zámku se ukládají jednoduché a není volat jiné metody, které může trvat i zámek!


## <a name="related-links"></a>Související odkazy

- [Přístup Basic (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Rozšířené přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recepty dat](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
