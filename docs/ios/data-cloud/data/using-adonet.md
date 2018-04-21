---
title: Použitím technologie ADO.NET s iOS
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7d8478c363da1e4362a8a837dafba7f9cf85872e
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/20/2018
---
# <a name="using-adonet-with-ios"></a>Použitím technologie ADO.NET s iOS

Xamarin má integrovanou podporu pro databázi SQLite, která je k dispozici v systému iOS, které jsou vystavené pomocí známé syntaxe pro technologii ADO.NET. Pomocí těchto rozhraní API vyžaduje, abyste zápisu příkazů SQL, které jsou zpracovány SQLite, jako například `CREATE TABLE`, `INSERT` a `SELECT` příkazy.

## <a name="assembly-references"></a>Odkazy na sestavení

Použití access SQLite prostřednictvím ADO.NET, je nutné přidat `System.Data` a `Mono.Data.Sqlite` odkazuje do projektu iOS, jak je vidět tady (pro ukázky v sadě Visual Studio pro Mac a Visual Studio):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](using-adonet-images/image4.png "Odkazy na sestavení v sadě Visual Studio pro Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Odkazy na sestavení v sadě Visual Studio")

-----

Klikněte pravým tlačítkem na **odkazy > Upravit odkazy...**  pak zaškrtněte požadovaná sestavení.

## <a name="about-monodatasqlite"></a>O Mono.Data.Sqlite

Budeme používat `Mono.Data.Sqlite.SqliteConnection` třídy za účelem vytvoření souboru prázdnou databázi a potom vytvořit instanci `SqliteCommand` objekty, že budeme moci použít pro spouštění instrukcí SQL v databázi.


1. **Vytváření prázdnou databázi** -volání `CreateFile` metoda platný (ie. zapisovatelné) cesta k souboru. Byste měli zkontrolovat, zda soubor již existuje před voláním této metody, jinak se vytvoří novou (prázdnou) databázi v horní části starý data v původní soubor se ztratí:

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > `dbPath` Proměnná by měla určit podle pravidla popsané dříve v tomto dokumentu.

2. **Vytvoření připojení k databázi** – po vytvoření souboru databáze SQLite můžete vytvořit objekt připojení pro přístup k datům. Připojení je vytvořený pomocí připojovací řetězec, který má formu `Data Source=file_path`, jak je vidět tady:

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    Jak už bylo zmíněno dříve, připojení nebude pravděpodobně znovu použít v různých vláknech. Pokud máte pochybnosti, vytvořte připojení podle potřeby a zavřete ho, když jste hotovi; ale mějte na paměti to tento další často než příliš nezbytné.
    
3. **Vytváření a spouštění příkazu databáze** – když máme připojení můžeme spouštět libovolné příkazy SQL u ní. Následující kód ukazuje příkazu CREATE TABLE spouštěna.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

Při provádění příkazu SQL přímo s databází, které byste měli vzít normální opatření Nedělejte neplatných požadavků, jako je například pokusu o vytvoření tabulku, která již existuje. Zachovat informace o struktuře vaší databáze, tak, aby vám nezpůsobí SqliteException, například "již existuje tabulka chyba SQLite [položky]".

## <a name="basic-data-access"></a>Základní přístup k datům

*DataAccess_Basic* při spuštění v systému iOS se ukázkový kód pro tento dokument vypadat třeba takto:

 ![](using-adonet-images/image9.png "Ukázka iOS ADO.NET")

Následující kód ukazuje, jak provádět jednoduché operace SQLite a zobrazí výsledky v jako text v hlavním okně aplikace.

Budete muset zahrnují tyto obory názvů:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

Následující příklad kódu ukazuje interakce celé databáze:

1.  Vytvoření souboru databáze
2.  Vkládání některá data
3.  Dotazování na data

Tyto operace by obvykle zobrazují na několika místech kódu, například můžete vytvořit soubor databáze a tabulky při prvním spuštění aplikace a provádět data čtení a zápisu v jednotlivých obrazovek v aplikaci. V následujícím příkladu, byly seskupeny do jedné metody v tomto příkladu:

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

Protože SQLite umožňuje libovolné příkazů SQL pro použit pro data, můžete provést ať vytvořte, vložit, aktualizovat, odstranit nebo vyberte příkazy, které se vám líbí. Další informace o příkazech jazyka SQL nepodporuje SQLite na webu Sqlite. Příkazy SQL jsou spustit pomocí jedné ze tří metod SqliteCommand objektu:

-  **ExecuteNonQuery** – obvykle používá k vytvoření nebo data vložení tabulky. Návratovou hodnotu pro některé operace počet ovlivněných řádků, v opačném případě je -1.
-  **ExecuteReader** – použít, když bude kolekce řádků má být vrácen jako `SqlDataReader` .
-  **ExecuteScalar** – načte jednu hodnotu (například agregace).


### <a name="executenonquery"></a>EXECUTENONQUERY

Příkazy INSERT, UPDATE a DELETE vrátí počet ovlivněných řádků. Všechny ostatní příkazy SQL vrátí hodnotu -1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Následující metodu ukazuje klauzule WHERE v příkazu SELECT. Protože kód je věnujte dokončení příkazu jazyka SQL ho musí postará, abyste se vyhnuli vyhrazené znaky, jako je například uvozovky (') kolem řetězce.

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

ExecuteReader metoda vrátí objekt SqliteDataReader. Kromě metodu pro čtení v příkladu další užitečné vlastnosti zahrnují:

-  **RowsAffected** – počet řádků, vliv na dotaz.
-  **HasRows** – jestli nebyly vráceny žádné řádky.


### <a name="executescalar"></a>EXECUTESCALAR

Používejte pro příkazy SELECT, které vrátí jednu hodnotu (například agregace).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` Návratový typ metody `object` – výsledku v závislosti na databázový dotaz by měl přetypování. Výsledkem může být celé číslo od dotazu COUNT nebo řetězec z dotazu vyberte jeden sloupec. Všimněte si, že se jedná o liší od jiných metod Execute, které vrací objekt čtečky nebo počet ovlivněných řádků.


## <a name="related-links"></a>Související odkazy

- [Přístup Basic (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Rozšířené přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recepty dat](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
