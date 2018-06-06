---
title: Pomocí dat v aplikaci pro iOS
description: Tento dokument popisuje DataAccess_Adv vzorku, který ukazuje, jak shromažďovat vstup uživatele a provádět vytvořit, číst, aktualizovat a odstranit databázi operace v aplikaci pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 5c9eab9316539ecf5988c8768bef9ef2cd61513e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784537"
---
# <a name="using-data-in-an-ios-app"></a>Pomocí dat v aplikaci pro iOS

**DataAccess_Adv** příklad ukazuje funkční aplikaci, která umožňuje vstup uživatele a *CRUD* funkce databáze (vytvoření, čtení, aktualizace a odstranění). Aplikace se skládá ze dvou obrazovkách: seznam a dat. Všechna data kód přístup je opakovaně použitelné v iOS a Android beze změny.

Po přidání některá data obrazovky aplikace vypadat například takto v systému iOS:

 ![](using-data-in-an-app-images/image9.png "seznam ukázek iOS")

 ![](using-data-in-an-app-images/image10.png "Detaily vzorku iOS")

IOS projektu je uveden níže – uvedeném v této části kódu je součástí **Orm** directory:

 ![](using-data-in-an-app-images/image13.png "strom projektu iOS")

Nativní kód uživatelského rozhraní pro ViewControllers v iOS je mimo rozsah tohoto dokumentu.
Odkazovat [iOS práce s tabulkami a buněk](~/ios/user-interface/controls/tables/index.md) průvodce Další informace o ovládacích prvků uživatelského rozhraní.

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

iOS vykreslí data jinak jako `UITableView`.

## <a name="create-and-update"></a>Vytváření a aktualizaci

Můžete zjednodušit kód aplikace, je jediný uložit metoda zadaný, který provádí typu vložení nebo aktualizace v závislosti na tom, jestli je nastavená PrimaryKey. Protože `Id` je vlastnost označena `[PrimaryKey]` atributu nesmí ho nastavit v kódu.
Tato metoda zjistí, zda hodnota byl předchozí uložit (kontrolou vlastnost primárního klíče) a vložit nebo aktualizovat objekt odpovídajícím způsobem:

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



Skutečných aplikace bude obvykle vyžadují některé ověření (například povinná pole, minimální délky nebo jiných obchodních pravidel).
Dobrý aplikací platformě implementovat co nejvíc ověření logické nejblíže v sdíleného kódu předávání chyb ověření zálohování na uživatelské rozhraní pro zobrazení podle funkce platformy.

## <a name="delete"></a>Odstranit

Na rozdíl od `Insert` a `Update` metody, `Delete<T>` metoda může přijímat pouze hodnotu primárního klíče místo úplná `Stock` objektu.
V tomto příkladu `Stock` objekt je předán do metody, ale pouze vlastnost Id předaný `Delete<T>` metoda.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Pomocí předem vyplněná souboru databáze SQLite

Některé aplikace jsou dodávané s databází již naplněný daty.
Snadno toho lze dosáhnout při odeslání stávající soubor databáze SQLite s vaší aplikací a kopírování do zapisovatelné adresáře než k ní přistupují v mobilní aplikaci. Protože SQLite je standardní formát, který se používá na mnoha platformách, existuje několik nástrojů, které jsou k dispozici pro vytvoření souboru databáze SQLite:

-  **Rozšíření správce SQLite Firefox** – funguje na Mac a Windows a vytváří soubory, které jsou kompatibilní s iOS a Android.
-  **Příkazový řádek** – viz [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .


Při vytváření souboru databáze pro distribuci s vaší aplikací, postará s názvy tabulek a sloupců zajistit budou odpovídat co očekává kódu, zvlášť pokud používáte SQLite.NET, který bude očekávat názvy tak, aby odpovídaly C# třídy a vlastnosti (nebo přidružené vlastní atributy).

Pro iOS, zahrnout soubor sqlite ve vaší aplikaci a ujistěte se, je označené **akce sestavení: obsahu**. Umístěte kód v `FinishedLaunching` při kopírování souboru do adresáře s možností zápisu *před* volání žádné metody data. Následující kód zkopíruje existující databázi názvem **data.sqlite**pouze v případě, že ještě neexistuje.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

Žádný kód přístup dat (jestli ADO.NET nebo pomocí SQLite.NET), která se spouští po to má dokončené bude mít přístup k datům předem vyplněné.


## <a name="related-links"></a>Související odkazy

- [Přístup Basic (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Rozšířené přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recepty dat](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
