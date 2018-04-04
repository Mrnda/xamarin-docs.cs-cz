---
title: Pomocí dat v aplikaci
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: dd2a0a58a3a8c10671609aa385629d4754ca5378
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="using-data-in-an-app"></a>Pomocí dat v aplikaci

**DataAccess_Adv** příklad ukazuje funkční aplikaci, která umožňuje vstup uživatele a funkce databáze CRUD (vytvoření, čtení, aktualizace a odstranění). Aplikace se skládá ze dvou obrazovkách: seznam a dat. Všechna data kód přístup je opakovaně použitelné v iOS a Android beze změny.

Po přidání některá data obrazovky aplikace vypadat například takto v systému Android:

![Seznam ukázek Android](using-data-in-an-app-images/image11.png "seznam Android ukázek")

![Detaily Android vzorku](using-data-in-an-app-images/image12.png "detaily Android vzorku")

Projektu pro Android je zobrazena níže &ndash; uvedeném v této části kódu je součástí **Orm** directory:

![Projekt pro Android stromu](using-data-in-an-app-images/image14.png "stromu projekt pro Android")

Nativní kód uživatelského rozhraní pro aktivity v Android je mimo rozsah tohoto dokumentu. Odkazovat [Android ListViews a adaptéry](~/android/user-interface/layouts/list-view/index.md) průvodce Další informace o ovládacích prvků uživatelského rozhraní.

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

## <a name="create-and-update"></a>Vytváření a aktualizaci

Můžete zjednodušit kód aplikace, je jediný uložit metoda zadaný, který provádí typu vložení nebo aktualizace v závislosti na tom, jestli je nastavená PrimaryKey. Protože `Id` je vlastnost označena `[PrimaryKey]` atributu nesmí ho nastavit v kódu. Tato metoda zjistí, zda hodnota byl předchozí uložit (kontrolou vlastnost primárního klíče) a vložit nebo aktualizovat objekt odpovídajícím způsobem:

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

Skutečných aplikace bude obvykle vyžadují některé ověření (například povinná pole, minimální délky nebo jiných obchodních pravidel). Dobrý aplikací platformě implementovat co nejvíc ověření logické nejblíže v sdíleného kódu předávání chyb ověření zálohování na uživatelské rozhraní pro zobrazení podle funkce platformy.

## <a name="delete"></a>Odstranit

Na rozdíl od `Insert` a `Update` metody, `Delete<T>` metoda může přijímat pouze hodnotu primárního klíče místo úplná `Stock` objektu. V tomto příkladu `Stock` objekt je předán do metody, ale pouze vlastnost Id předaný `Delete<T>` metoda.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Pomocí předem vyplněná souboru databáze SQLite

Některé aplikace jsou dodávané s databází již naplněný daty. Snadno toho lze dosáhnout při odeslání stávající soubor databáze SQLite s vaší aplikací a kopírování do zapisovatelné adresáře než k ní přistupují v mobilní aplikaci. Protože SQLite je standardní formát, který se používá na mnoha platformách, existuje několik nástrojů, které jsou k dispozici pro vytvoření souboru databáze SQLite:

-   **Rozšíření správce SQLite Firefox** &ndash; funguje na Mac a Windows a vytváří soubory, které jsou kompatibilní s iOS a Android.

-   **Příkazový řádek** &ndash; najdete v části [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .

Při vytváření souboru databáze pro distribuci s vaší aplikací, postará s názvy tabulek a sloupců zajistit budou odpovídat co očekává kódu, zvlášť pokud používáte SQLite.NET, který bude očekávat názvy tak, aby odpovídaly C# třídy a vlastnosti (nebo přidružené vlastní atributy).

Aby se zajistilo, že některé kód běží před nic jiného v aplikacích pro Android, můžete ji umístit první aktivitu načíst nebo můžete vytvořit `Application` podtřídami, který je načten před žádné aktivity. Kód uvedený níže ukazuje `Application` podtřídami, který kopíruje stávající soubor databáze **data.sqlite** mimo **/Resources/Raw/** adresáře.

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

- [Přístup Basic (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Rozšířené přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recepty dat v androidu](https://developer.xamarin.com/recipes/android/data/)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
