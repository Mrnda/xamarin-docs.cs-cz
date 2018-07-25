---
title: Pomocí technologie ADO.NET v Androidu
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 9e0c1be2e37355242db2fb70857d90127c3b5259
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242209"
---
# <a name="using-adonet-with-android"></a>Pomocí technologie ADO.NET v Androidu

Xamarin nabízí integrovanou podporu pro databázi SQLite, který je k dispozici v Androidu a dá se vystavit pomocí známé syntaxe pro ADO.NET. Pomocí těchto rozhraní API vyžaduje, abyste zápisu příkazů SQL, které jsou zpracovány SQLite, jako například `CREATE TABLE`, `INSERT` a `SELECT` příkazy.

## <a name="assembly-references"></a>Odkazy na sestavení

Použití přístupu pomocí ADO.NET, je nutné přidat SQLite `System.Data` a `Mono.Data.Sqlite` odkazy na váš projekt pro Android, jak je znázorněno zde:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin) 

![Android – reference v sadě Visual Studio](using-adonet-images/image7.png "Android odkazuje v sadě Visual Studio") 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac) 

![Android – reference v sadě Visual Studio pro Mac](using-adonet-images/image5.png "Android odkazuje v sadě Visual Studio pro Mac") 

-----


Klikněte pravým tlačítkem na **odkazy > Upravit odkazy...**  pak kliknutím vyberte požadovaná sestavení.

## <a name="about-monodatasqlite"></a>O Mono.Data.Sqlite

Budeme používat `Mono.Data.Sqlite.SqliteConnection` třídy za účelem vytvoření souboru prázdnou databázi a pak vytvořit instanci `SqliteCommand` objekty, můžeme použít pro spouštění instrukcí SQL na databázi.

**Vytvoření prázdné databáze** &ndash; volání `CreateFile` metoda s platným (tj. zapisovatelné) cesta k souboru. Měli byste zkontrolovat, zda soubor před voláním této metody již existuje, jinak se vytvoří novou (prázdnou) databázi v horní části starý a data v původní soubor se ztratí.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` `dbPath` Proměnné byste měli určit podle pravidel popsaných výše v tomto dokumentu.

**Vytváří se připojení k databázi** &ndash; po vytvoření souboru databáze SQLite můžete vytvořit objekt připojení pro přístup k datům. Připojení je vytvořený pomocí připojovací řetězec, který má formu `Data Source=file_path`, jak je znázorněno zde:

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

Jak už bylo zmíněno dříve, připojení by nikdy neměly být znovu použít v různých vláknech. Pokud máte pochybnosti, vytvořte připojení podle potřeby a zavřete ho, až to budete mít; ale mějte na paměti dělat toto více často než příliš požadované.

**Vytvoření a spuštění příkazu databáze** &ndash; Jakmile budeme mít připojení můžeme spouštět libovolné příkazy SQL před ním. Níže uvedený kód ukazuje `CREATE TABLE` při provádění příkazu.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

Při provádění SQL přímo na databázi byste měli podniknout normální opatření Nedělejte neplatných požadavků, jako například pokusu vytvořit tabulku, která již existuje. Mějte přehled o struktuře databáze tak, aby vám nezpůsobí `SqliteException` jako **tabulka chyba SQLite [položky] již existuje**.

## <a name="basic-data-access"></a>Přístup k základním datům

*DataAccess_Basic* při spuštění v Androidu se ukázkový kód pro tento dokument vypadá například takto:

![Ukázka pro Android ADO.NET](using-adonet-images/image8.png "ukázka Android ADO.NET")

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

Protože SQLite umožňuje libovolné příkazy SQL, spouštějte data, můžete provést cokoli, co `CREATE`, `INSERT`, `UPDATE`, `DELETE`, nebo `SELECT` příkazy, které vám vyhovuje. Další informace o SQL příkazy podporované SQLite na webu Sqlite. Příkazy SQL se spouštějí pomocí jedné ze tří metod na `SqliteCommand` objektu:

-   **Metodu ExecuteNonQuery** &ndash; obvykle používá pro vložení vytváření nebo dat tabulky. Počet ovlivněných řádků je návratová hodnota pro některé operace, v opačném případě je hodnota -1.

-   **ExecuteReader** &ndash; při kolekce řádků má být vrácen jako `SqlDataReader`.

-   **ExecuteScalar** &ndash; načte hodnotu single (například agregace).


### <a name="executenonquery"></a>METODU EXECUTENONQUERY

`INSERT`, `UPDATE`, a `DELETE` příkazy vrátí počet ovlivněných řádků. Všechny ostatní příkazy SQL vrátí hodnotu -1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Následující metody `WHERE` klauzule `SELECT` příkaz.
Protože kód je vytváření úplný příkaz jazyka SQL ho musíte pečlivě řídicí vyhrazených znaků uvozovky (') řetězce.

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

`ExecuteReader` Metoda vrátí hodnotu `SqliteDataReader` objektu. Kromě `Read` metoda je znázorněno v příkladu, další užitečné vlastnosti zahrnují:

-   **RowsAffected** &ndash; počet řádků, které jsou ovlivněny dotazu.

-   **HasRows** &ndash; Určuje, zda nebyly vráceny žádné řádky.


### <a name="executescalar"></a>EXECUTESCALAR

Použít pro `SELECT` příkazy, které vrátí jednu hodnotu (jako jsou agregace).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` Je návratový typ metody `object` &ndash; měli přetypovat výsledku v závislosti na databázového dotazu. Výsledkem může být celé číslo od `COUNT` dotaz nebo řetězec z jednoho sloupce `SELECT` dotazu. Všimněte si, že to se liší u jiného `Execute` metody, které vracejí objekt čtečky nebo počet ovlivněných řádků.



## <a name="related-links"></a>Související odkazy

- [Basic přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Pokročilé přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recepty dat pro Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
