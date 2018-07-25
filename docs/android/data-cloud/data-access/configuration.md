---
title: Konfigurace
ms.prod: xamarin
ms.assetid: 44526226-4E4E-4FFF-9A16-CA7B1E01BB8F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/11/2016
ms.openlocfilehash: b3a7858361d25f26807ea328e8bfdd30ca8d483b
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241874"
---
# <a name="configuration"></a>Konfigurace

Pro použití v aplikaci Xamarin.Android, budete muset určit správný soubor umístění souboru databáze SQLite.

## <a name="database-file-path"></a>Cesta k souboru databáze

Bez ohledu na to, jakou metodu přístupu dat použijete musíte vytvořit soubor databáze předtím, než se data dají uložit s SQLite. Umístění souboru se liší v závislosti na tom, jakou platformu cílíte. Pro Android můžete použít třídu prostředí vytvořit platnou cestu, jak je znázorněno v následujícím fragmentu kódu:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Existují jiné co je potřeba vzít v úvahu při rozhodování, kam chcete soubor uložte do databáze. Například v systému Android je možné, jestli se má použít interní nebo externí úložiště.

Pokud si přejete použít jiné umístění na jednotlivých platformách ve vaší aplikace pro různé platformy můžete použít direktivy kompilátoru jak je znázorněno ke generování jinou cestu pro každou platformu:

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1 to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

Pomocné parametry pomocí systému souborů v Androidu, najdete [Procházet soubory](https://github.com/xamarin/recipes/tree/master/Recipes/android/data/files/browse_files) předpisu. Najdete v článku [sestavování pro různé platformy aplikací](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) dokument pro další informace o použití direktivy kompilátoru psaní kódu, které jsou specifické pro každou platformu.

## <a name="threading"></a>Dělení na vlákna

Neměli byste používat stejné připojení k databázi SQLite napříč více vlákny. Buďte opatrní při otevření, používat a pak zavřete všechna připojení, které vytvoříte ve stejném vlákně.

Aby bylo zajištěno, že se váš kód pokusu o přístup k databázi SQLite z více vláken ve stejnou dobu, ručně zámek pokaždé, když chcete získat přístup k databázi, například takto:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Všechny přístup k databázi (čtení, zápisů, aktualizace atd.) by měl být uzavřen stejné zámku. Aby se zabránilo situaci zablokování tím, že zajišťuje, že práce v klauzuli zámek je jednoduché a není volání dalších metodách, které může také použít zámek musí věnovat pozornost.


## <a name="related-links"></a>Související odkazy

- [Basic přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Pokročilé přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recepty dat pro Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
