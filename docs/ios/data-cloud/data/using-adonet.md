---
title: Pomocí technologie ADO.NET s Xamarin.iOS
description: Tento dokument popisuje, jak používat technologie ADO.NET jako metoda pro přístup k SQLite aplikace pro Xamarin.iOS. Popisuje odkazy na sestavení, Mono.Data.Sqlite a BasicDataAccess vzorku.
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 83f6059c405b2156270f4359cbba33177861af02
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241235"
---
# <a name="using-adonet-with-xamarinios"></a>Pomocí technologie ADO.NET s Xamarin.iOS

Xamarin nabízí integrovanou podporu pro databázi SQLite, která je k dispozici v systémech iOS, která je vystavena pomocí známé syntaxe pro ADO.NET. Pomocí těchto rozhraní API vyžaduje, abyste zápisu příkazů SQL, které jsou zpracovány SQLite, jako například `CREATE TABLE`, `INSERT` a `SELECT` příkazy.

## <a name="assembly-references"></a>Odkazy na sestavení

Použití přístupu pomocí ADO.NET, je nutné přidat SQLite `System.Data` a `Mono.Data.Sqlite` odkazy na váš projekt pro iOS, jak je znázorněno zde (pro ukázky v sadě Visual Studio pro Mac a Visual Studio):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](using-adonet-images/image4.png "Odkazy na sestavení v sadě Visual Studio pro Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Odkazy na sestavení v sadě Visual Studio")

-----

Klikněte pravým tlačítkem na **odkazy > Upravit odkazy...**  pak kliknutím vyberte požadovaná sestavení.

## <a name="about-monodatasqlite"></a>O Mono.Data.Sqlite

Budeme používat `Mono.Data.Sqlite.SqliteConnection` třídy za účelem vytvoření souboru prázdnou databázi a pak vytvořit instanci `SqliteCommand` objekty, můžeme použít pro spouštění instrukcí SQL na databázi.


1. **Vytvoření prázdné databáze** – volání `CreateFile` metoda s platným (ie. zapisovatelná) cesta k souboru. Zkontrolujte, zda soubor před voláním této metody již existuje, jinak se vytvoří novou (prázdnou) databázi v horní části starý a data v původní soubor se ztratí:

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > `dbPath` Proměnné byste měli určit podle pravidel popsaných výše v tomto dokumentu.

2. **Vytváří se připojení k databázi** – po vytvoření souboru databáze SQLite můžete vytvořit objekt připojení pro přístup k datům. Připojení je vytvořený pomocí připojovací řetězec, který má formu `Data Source=file_path`, jak je znázorněno zde:

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    Jak už bylo zmíněno dříve, připojení by nikdy neměly být znovu použít v různých vláknech. Pokud máte pochybnosti, vytvořte připojení podle potřeby a zavřete ho, až to budete mít; ale mějte na paměti dělat toto více často než příliš požadované.
    
3. **Vytvoření a spuštění příkazu databáze** – Jakmile budeme mít připojení můžeme spouštět libovolné příkazy SQL před ním. Následující kód ukazuje při provádění příkazu CREATE TABLE.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

Při provádění SQL přímo na databázi byste měli podniknout normální opatření Nedělejte neplatných požadavků, jako například pokusu vytvořit tabulku, která již existuje. Sledovat, struktura databáze tak, aby nezpůsobí SqliteException jako "tabulky chyba SQLite [položky] již existuje".

## <a name="basic-data-access"></a>Přístup k základním datům

*DataAccess_Basic* při spuštění v Iosu se ukázkový kód pro tento dokument vypadá například takto:

 ![](using-adonet-images/image9.png "Ukázka technologie ADO.NET pro iOS")

Následující kód ukazuje, jak provádět jednoduché operace SQLite a zobrazí výsledky v jako text v hlavním okně aplikace.

Je potřeba zahrnout tyto obory názvů:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

Následující příklad kódu ukazuje interakci celou databázi:

1.  Vytvoření databázového souboru
2.  Vkládání některá data
3.  Dotazování na data

Tyto operace by obvykle zobrazují na několika místech kódu, například můžete vytvořit soubor databáze a tabulky při prvním spuštění aplikace a provádět zápisy a čtení dat na jednotlivých obrazovkách v aplikaci. V následujícím příkladu, byly seskupeny do jedné metody v tomto příkladu:

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}
```

## <a name="more-complex-queries"></a>Složitější dotazy

Protože SQLite umožňuje libovolné příkazy SQL, spouštějte data, můžete provést cokoli, co vytvořit, vložení, aktualizace, odstranění nebo vyberte příkazy, které vám vyhovuje. Další informace o SQL příkazy podporované SQLite na webu Sqlite. Příkazy SQL se spouštějí pomocí jedné ze tří metod objektu SqliteCommand:

-  **Metodu ExecuteNonQuery** – obvykle používá pro vložení vytváření nebo dat tabulky. Počet ovlivněných řádků je návratová hodnota pro některé operace, v opačném případě je hodnota -1.
-  **ExecuteReader** – při kolekce řádků má být vrácen jako `SqlDataReader` .
-  **ExecuteScalar** – načte hodnotu single (například agregace).


### <a name="executenonquery"></a>METODU EXECUTENONQUERY

Příkazy INSERT, UPDATE a DELETE vrátí počet ovlivněných řádků. Všechny ostatní příkazy SQL vrátí hodnotu -1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Následující metoda ukazuje klauzule WHERE v příkazu SELECT. Protože kód je vytváření úplný příkaz jazyka SQL ho musíte pečlivě řídicí vyhrazených znaků uvozovky (') řetězce.

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

Vrátí metodu ExecuteReader SqliteDataReader objektu. Kromě metodu pro čtení je znázorněno v příkladu další užitečné vlastnosti patří:

-  **RowsAffected** – počet řádků, které jsou ovlivněny dotazu.
-  **HasRows** – Určuje, zda nebyly vráceny žádné řádky.


### <a name="executescalar"></a>EXECUTESCALAR

Používá se pro příkazy SELECT, které vrátí jednu hodnotu (jako jsou agregace).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` Je návratový typ metody `object` – měli přetypovat výsledku v závislosti na databázového dotazu. Výsledkem může být celé číslo od AGREGAČNÍ dotaz nebo řetězec dotazu vyberte jeden sloupec. Všimněte si, že se jedná o různé jiné spouštět metody, které vracejí objekt čtečky nebo počet ovlivněných řádků.


## <a name="related-links"></a>Související odkazy

- [Basic přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Pokročilé přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recepty dat](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
