---
title: Pomocí Android ADO.NET
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 29e81afdf2c46cdefc68e2c2fae4e6e47999a346
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/20/2018
ms.locfileid: "31646778"
---
# <a name="using-adonet-with-android"></a>Pomocí Android ADO.NET

Xamarin má integrovanou podporu pro databázi SQLite, která je k dispozici v systému Android a mohou být zpřístupněny pomocí známé syntaxe pro technologii ADO.NET. Pomocí těchto rozhraní API vyžaduje, abyste zápisu příkazů SQL, které jsou zpracovány SQLite, jako například `CREATE TABLE`, `INSERT` a `SELECT` příkazy.

## <a name="assembly-references"></a>Odkazy na sestavení

Použití access SQLite prostřednictvím ADO.NET, je nutné přidat `System.Data` a `Mono.Data.Sqlite` odkazuje na vašeho projektu Android, jak je vidět tady:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin) 

![Android odkazy v sadě Visual Studio](using-adonet-images/image7.png "Android odkazuje v sadě Visual Studio") 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac) 

![Android odkazy v sadě Visual Studio pro Mac](using-adonet-images/image5.png "Android odkazuje v sadě Visual Studio pro Mac") 

-----


Klikněte pravým tlačítkem na **odkazy > Upravit odkazy...**  pak zaškrtněte požadovaná sestavení.

## <a name="about-monodatasqlite"></a>O Mono.Data.Sqlite

Budeme používat `Mono.Data.Sqlite.SqliteConnection` třídy za účelem vytvoření souboru prázdnou databázi a potom vytvořit instanci `SqliteCommand` objekty, že budeme moci použít pro spouštění instrukcí SQL v databázi.

**Vytváření prázdnou databázi** &ndash; volání `CreateFile` metoda platný (ie. zapisovatelné) cesta k souboru. Byste měli zkontrolovat, zda soubor již existuje před voláním této metody, jinak se vytvoří novou (prázdnou) databázi v horní části starý data v původní soubor se ztratí.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` `dbPath` Proměnná by měla určit podle pravidla popsané dříve v tomto dokumentu.

**Vytvoření připojení k databázi** &ndash; po vytvoření souboru databáze SQLite můžete vytvořit objekt připojení pro přístup k datům. Připojení je vytvořený pomocí připojovací řetězec, který má formu `Data Source=file_path`, jak je vidět tady:

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

Jak už bylo zmíněno dříve, připojení nebude pravděpodobně znovu použít v různých vláknech. Pokud máte pochybnosti, vytvořte připojení podle potřeby a zavřete ho, když jste hotovi; ale mějte na paměti to tento další často než příliš nezbytné.

**Vytváření a spouštění příkazu databáze** &ndash; Jakmile připojení můžeme spouštět libovolné příkazy SQL u ní. Kód uvedený níže ukazuje `CREATE TABLE` příkaz spouštěna.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

Při provádění příkazu SQL přímo s databází, které byste měli vzít normální opatření Nedělejte neplatných požadavků, jako je například pokusu o vytvoření tabulku, která již existuje. Udržování přehledu o struktuře databáze tak, že nemáte způsobit `SqliteException` například **SQLite chyba tabulka [položky] již existuje**.

## <a name="basic-data-access"></a>Základní přístup k datům

*DataAccess_Basic* při spuštění v systému Android se ukázkový kód pro tento dokument vypadat třeba takto:

![Ukázka Android ADO.NET](using-adonet-images/image8.png "ukázka Android ADO.NET")

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

Protože SQLite umožňuje libovolné příkazů SQL pro použit pro data, můžete provést ať `CREATE`, `INSERT`, `UPDATE`, `DELETE`, nebo `SELECT` příkazy, které se vám líbí. Další informace o příkazech jazyka SQL nepodporuje SQLite na webu Sqlite. Příkazy SQL jsou spustit pomocí jedné ze tří metod na `SqliteCommand` objektu:

-   **ExecuteNonQuery** &ndash; obvykle se používá pro vytvoření nebo data vložení tabulky. Návratovou hodnotu pro některé operace počet ovlivněných řádků, v opačném případě je -1.

-   **ExecuteReader** &ndash; používá, když bude kolekce řádků má být vrácen jako `SqlDataReader`.

-   **ExecuteScalar** &ndash; načte jednu hodnotu (například agregace).


### <a name="executenonquery"></a>EXECUTENONQUERY

`INSERT`, `UPDATE`, a `DELETE` příkazy vrátí počet ovlivněných řádků. Všechny ostatní příkazy SQL vrátí hodnotu -1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Následující metoda ukazuje `WHERE` klauzuli v `SELECT` příkaz.
Protože kód je věnujte dokončení příkazu jazyka SQL ho musí postará, abyste se vyhnuli vyhrazené znaky, jako je například uvozovky (') kolem řetězce.

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

`ExecuteReader` Metoda vrátí `SqliteDataReader` objektu. Kromě `Read` metoda ukazuje příklad, další užitečné vlastnosti zahrnují:

-   **RowsAffected** &ndash; počet řádků, vliv na dotaz.

-   **HasRows** &ndash; jestli nebyly vráceny žádné řádky.


### <a name="executescalar"></a>EXECUTESCALAR

Použít pro `SELECT` příkazy, které vrátí jednu hodnotu (například agregace).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` Návratový typ metody `object` &ndash; výsledku v závislosti na databázový dotaz by měl přetypování. Výsledkem může být celé číslo od `COUNT` dotazu nebo řetězec z jednoho sloupce `SELECT` dotazu. Tento název se liší od dalších `Execute` metody, které vrací objekt čtečky nebo počet ovlivněných řádků.



## <a name="related-links"></a>Související odkazy

- [Přístup Basic (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Rozšířené přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recepty dat v androidu](https://developer.xamarin.com/recipes/android/data/)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
