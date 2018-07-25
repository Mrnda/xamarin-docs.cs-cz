---
title: Používání dat v aplikaci pro Android
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 563c04ef1c8eec00108844894c5f9bdc0e9950e3
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241887"
---
# <a name="using-data-in-an-app"></a>Používání dat v aplikaci

**DataAccess_Adv** příklad ukazuje funkční aplikaci, který umožňuje vstup uživatele a funkce databáze CRUD (vytvoření, čtení, aktualizace a odstranění). Aplikace se skládá ze dvou obrazovkách: seznam a formuláře datových záznamů. Všechny kód přístupu k datům je opakovaně použitelné v iOS a Android beze změny.

Po přidání některých dat na obrazovce aplikace vypadat nějak takto v Androidu:

![Seznam ukázek Android](using-data-in-an-app-images/image11.png "seznamu ukázka pro Android")

![Ukázka pro Android podrobností](using-data-in-an-app-images/image12.png "podrobností ukázka pro Android")

Projekt pro Android je uveden níže &ndash; kód uvedené v této části je obsažen v rámci **Orm** adresáře:

![Projekt pro Android stromu](using-data-in-an-app-images/image14.png "stromu projektu pro Android")

Nativní kód uživatelského rozhraní pro aktivity v Androidu sahá nad rámec tohoto dokumentu. Odkazovat [Android zobrazení a adaptéry](~/android/user-interface/layouts/list-view/index.md) Průvodce pro další informace o ovládacích prvků uživatelského rozhraní.

## <a name="read"></a>Číst

Existuje několik operací čtení v ukázce:

-  Čtení seznamu
-  Čtení jednotlivých záznamů

Tyto dvě metody v `StockDatabase` třídy jsou:

```csharp
public IEnumerable<Stock> GetStocks ()
{
    lock (locker) {
        return (from i in Table<Stock> () select i).ToList ();
    }
}
public Stock GetStock (int id)
{
    lock (locker) {
        return Table<Stock>().FirstOrDefault(x => x.Id == id);
    }
}
```

Android vykreslí data jako `ListView`.

## <a name="create-and-update"></a>Vytvoření a aktualizaci

Pro zjednodušení kódu aplikace, je jediného uložit metoda za předpokladu, který nepodporuje Insert nebo Update v závislosti na tom, zda byla nastavena PrimaryKey. Protože `Id` je vlastnost označena `[PrimaryKey]` atribut nesmí ji nastavíte v kódu. Tato metoda zjistí, zda hodnota byla předchozí uložit (že zkontrolujete vlastnost primárního klíče) a vložit nebo aktualizovat objekt odpovídajícím způsobem:

```csharp
public int SaveStock (Stock item)
{
    lock (locker) {
        if (item.Id != 0) {
            Update (item);
            return item.Id;
    } else {
            return Insert (item);
        }
    }
}
```

Reálného světa aplikací bude obvykle vyžadovat nějaké ověření (jako je povinná, minimální délky nebo jiné obchodní pravidla). Funkční aplikace napříč platformami implementovat největší část ověření logické nejrychleji sdíleného kódu, předávání chyb při ověřování zálohování do uživatelského rozhraní pro zobrazení podle možnosti platformy.

## <a name="delete"></a>Odstranit

Na rozdíl od `Insert` a `Update` metody, `Delete<T>` metoda může přijímat pouze hodnotu primárního klíče místo kompletní `Stock` objektu. V tomto příkladu `Stock` objekt je předán do metody, ale pouze vlastnost Id předána `Delete<T>` metody.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Použití předem naplněných databázového souboru SQLite

Některé aplikace se dodávají s databází již naplněný daty. Snadno to lze provádět při odeslání existující soubor databáze SQLite s vaší aplikací a jejím zkopírováním na zapisovatelný adresář před přístupem k jeho v mobilní aplikaci. Protože SQLite je standardní formát, který se používá na spoustě platforem, existuje mnoho nástrojů dostupných k vytvoření souboru databáze SQLite:

-   **Rozšíření správce SQLite Firefox** &ndash; funguje na Mac a Windows a vytvoří soubory, které jsou kompatibilní s iOS a Android.

-   **Příkazový řádek** &ndash; naleznete v tématu [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .

Při vytváření souboru databáze pro distribuci s vaší aplikací, aby se postaral s názvy tabulek a sloupců zajistit, aby odpovídaly co váš kód očekává, zejména v případě, že používáte SQLite.NET, který bude očekávat názvů tak, aby odpovídaly vaší třídy jazyka C# a vlastnosti (nebo přidružené vlastní atributy).

Aby bylo zajištěno, že nějaký kód spuštěn dříve, než nic jiného v aplikaci pro Android, je možné je umístit v první aktivita pro načtení nebo můžete vytvořit `Application` podtřídy, který je načten před žádné aktivity. Níže uvedený kód ukazuje `Application` podtřídu, která zkopíruje existující soubor databáze **data.sqlite** z celkového počtu **/Resources/Raw/** adresáře.

```csharp
[Application]
public class YourAndroidApp : Application {
    public override void OnCreate ()
    {
        base.OnCreate ();
        var docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
        Console.WriteLine ("Data path:" + Database.DatabaseFilePath);
        var dbFile = Path.Combine(docFolder, "data.sqlite"); // FILE NAME TO USE WHEN COPIED
        if (!System.IO.File.Exists(dbFile)) {
            var s = Resources.OpenRawResource(Resource.Raw.data);  // DATA FILE RESOURCE ID
            FileStream writeStream = new FileStream(dbFile, FileMode.OpenOrCreate, FileAccess.Write);
            ReadWriteStream(s, writeStream);
        }
    }
    // readStream is the stream you need to read
    // writeStream is the stream you want to write to
    private void ReadWriteStream(Stream readStream, Stream writeStream)
    {
        int Length = 256;
        Byte[] buffer = new Byte[Length];
        int bytesRead = readStream.Read(buffer, 0, Length);
        // write the required bytes
        while (bytesRead > 0)
        {
            writeStream.Write(buffer, 0, bytesRead);
            bytesRead = readStream.Read(buffer, 0, Length);
        }
        readStream.Close();
        writeStream.Close();
    }
}
```


## <a name="related-links"></a>Související odkazy

- [Basic přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Pokročilé přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recepty dat pro Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
