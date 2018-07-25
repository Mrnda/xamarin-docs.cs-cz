---
title: Používání dat v aplikaci pro iOS
description: Tento dokument popisuje DataAccess_Adv vzorku, který ukazuje, jak shromažďování vstupu uživatele a provádět vytvoření, čtení, aktualizace a odstranění (CRUD) operací databáze v aplikaci pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 35caae657700e321a7560d1e95c8551b7b10a5ca
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242102"
---
# <a name="using-data-in-an-ios-app"></a>Používání dat v aplikaci pro iOS

**DataAccess_Adv** příklad ukazuje funkční aplikaci, která umožňuje zadávání uživatelem a *CRUD* funkce databáze (vytvoření, čtení, aktualizace a odstranění). Aplikace se skládá ze dvou obrazovkách: seznam a formuláře datových záznamů. Všechny kód přístupu k datům je opakovaně použitelné v iOS a Android beze změny.

Po přidání některých dat na obrazovce aplikace vypadat nějak takto v systému iOS:

 ![](using-data-in-an-app-images/image9.png "seznam ukázek pro iOS")

 ![](using-data-in-an-app-images/image10.png "Podrobnosti o ukázky iOS")

Projekt pro iOS je uveden níže – kód zobrazený v této části je obsažena v **Orm** adresáře:

 ![](using-data-in-an-app-images/image13.png "strom projektu iOS")

Nativní kód uživatelského rozhraní pro ViewControllers v Iosu sahá nad rámec tohoto dokumentu.
Odkazovat [iOS práce s tabulkami a buňky](~/ios/user-interface/controls/tables/index.md) Průvodce pro další informace o ovládacích prvků uživatelského rozhraní.

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

iOS vykreslí data jiným způsobem jako `UITableView`.

## <a name="create-and-update"></a>Vytvoření a aktualizaci

Pro zjednodušení kódu aplikace, je jediného uložit metoda za předpokladu, který nepodporuje Insert nebo Update v závislosti na tom, zda byla nastavena PrimaryKey. Protože `Id` je vlastnost označena `[PrimaryKey]` atribut nesmí ji nastavíte v kódu.
Tato metoda zjistí, zda hodnota byla předchozí uložit (že zkontrolujete vlastnost primárního klíče) a vložit nebo aktualizovat objekt odpovídajícím způsobem:

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



Reálného světa aplikací bude obvykle vyžadovat nějaké ověření (jako je povinná, minimální délky nebo jiné obchodní pravidla).
Funkční aplikace napříč platformami implementovat největší část ověření logické nejrychleji sdíleného kódu, předávání chyb při ověřování zálohování do uživatelského rozhraní pro zobrazení podle možnosti platformy.

## <a name="delete"></a>Odstranit

Na rozdíl od `Insert` a `Update` metody, `Delete<T>` metoda může přijímat pouze hodnotu primárního klíče místo kompletní `Stock` objektu.
V tomto příkladu `Stock` objekt je předán do metody, ale pouze vlastnost Id předána `Delete<T>` metody.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Použití předem naplněných databázového souboru SQLite

Některé aplikace se dodávají s databází již naplněný daty.
Snadno to lze provádět při odeslání existující soubor databáze SQLite s vaší aplikací a jejím zkopírováním na zapisovatelný adresář před přístupem k jeho v mobilní aplikaci. Protože SQLite je standardní formát, který se používá na spoustě platforem, existuje mnoho nástrojů dostupných k vytvoření souboru databáze SQLite:

-  **Rozšíření správce SQLite Firefox** – funguje na Mac a Windows a vytvoří soubory, které jsou kompatibilní s iOS a Android.
-  **Příkazový řádek** – viz [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .


Při vytváření souboru databáze pro distribuci s vaší aplikací, aby se postaral s názvy tabulek a sloupců zajistit, aby odpovídaly co váš kód očekává, zejména v případě, že používáte SQLite.NET, který bude očekávat názvů tak, aby odpovídaly vaší třídy jazyka C# a vlastnosti (nebo přidružené vlastní atributy).

Pro iOS, do aplikace zahrnout soubor sqlite a zajistit, je označené atributem **Build Action: obsahu**. Do kódu v `FinishedLaunching` zkopírovat soubor do adresáře, s možností zápisu *před* volat jakékoli metody data. Následující kód zkopíruje existující databázi s názvem **data.sqlite**pouze v případě, že ještě neexistuje.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

Libovolný kód přístupu k datům (ať už ADO.NET nebo používání SQLite.NET), který se spustí to po dokončení bude mít přístup k předem naplněných daty.


## <a name="related-links"></a>Související odkazy

- [Basic přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Pokročilé přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recepty dat](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
