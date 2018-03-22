---
title: System.Data
ms.topic: article
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 145a0692ed9761944eec4c7ece1f098a584f2d54
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="systemdata"></a>System.Data

Přidává podporu pro Xamarin.iOS 8.10 [System.Data](https://developer.xamarin.com/api/namespace/System.Data/), včetně `Mono.Data.Sqlite.dll` zprostředkovatele ADO.NET. Podpora zahrnuje přidání následující [sestavení](~/cross-platform/internals/available-assemblies.md):

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`


<a name="Example" />

## <a name="example"></a>Příklad

Následující program vytvoří databázi v `Documents/mydb.db3`, a pokud databáze neexistuje dříve ho naplněný vzorovými daty. Databáze je pak dotazován výstup zapsán do `stderr`.

### <a name="add-references"></a>Přidejte odkazy na

První, klikněte pravým tlačítkem na **odkazy** uzel a zvolte **upravit odkazy...**  vyberte `System.Data` a `Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "Přidávání nových odkazů")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>Ukázkový kód

Následující kód ukazuje jednoduchý příklad vytvoření tabulky a vložení řádků embedded příkazy SQL:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;

class Demo {
    static void Main (string [] args)
    {
        var connection = GetConnection ();
        using (var cmd = connection.CreateCommand ()) {
            connection.Open ();
            cmd.CommandText = "SELECT * FROM People";
            using (var reader = cmd.ExecuteReader ()) {
                while (reader.Read ()) {
                    Console.Error.Write ("(Row ");
                    Write (reader, 0);
                    for (nint i = 1; i < reader.FieldCount; ++i) {
                        Console.Error.Write(" ");
                        Write (reader, i);
                    }
                    Console.Error.WriteLine(")");
                }
            }
            connection.Close ();
        }
    }

    static SqliteConnection GetConnection()
    {
        var documents = Environment.GetFolderPath (
                Environment.SpecialFolder.Personal);
        string db = Path.Combine (documents, "mydb.db3");
        bool exists = File.Exists (db);
        if (!exists)
            SqliteConnection.CreateFile (db);
        var conn = new SqliteConnection("Data Source=" + db);
        if (!exists) {
            var commands = new[] {
            "CREATE TABLE People (PersonID INTEGER NOT NULL, FirstName ntext, LastName ntext)",
            // WARNING: never insert user-entered data with embedded parameter values
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (1, 'First', 'Last')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (2, 'Dewey', 'Cheatem')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (3, 'And', 'How')",
            };
            conn.Open ();
            foreach (var cmd in commands) {
                using (var c = conn.CreateCommand()) {
                    c.CommandText = cmd;
                    c.CommandType = CommandType.Text;
                    c.ExecuteNonQuery ();
                }
            }
            conn.Close ();
        }
        return conn;
    }

    static void Write(SqliteDataReader reader, int index)
    {
        Console.Error.Write("({0} '{1}')",
                reader.GetName(index),
                reader [index]);
    }
}
```

> [!IMPORTANT]
> Jak je uvedeno v ukázce kódu, výše, je chybný postup pro vložení řetězce do příkazy SQL, protože umožňuje snadno napadnutelný kódu [Injektáž SQL](http://en.wikipedia.org/wiki/SQL_injection).


### <a name="using-command-parameters"></a>Pomocí parametrů příkazu

Následující kód ukazuje, jak používat parametry příkazu pro vkládání uživatelem zadaný text bezpečně do databáze (i v případě, že text obsahuje speciální znaky SQL jako jeden apostrof):

```csharp
// user input from Textbox control
var fname = fnameTextbox.Text;
var lname = lnameTextbox.Text;
// use command parameters to safely insert into database
using (var addCmd = conn.CreateCommand ()) {
    addCmd.CommandText = "INSERT INTO [People] (PersonID, FirstName, LastName) VALUES (@COL1, @COL2, @COL3)";
    addCmd.CommandType = System.Data.CommandType.Text;
    addCmd.AddParameterWithValue ("@COL1", 1);
    addCmd.AddParameterWithValue ("@COL2", fname);
    addCmd.AddParameterWithValue ("@COL3", lname);
    addCmd.ExecuteNonQuery ();
}
```

<a name="Missing_Functionality" />

## <a name="missing-functionality"></a>Chybějící funkce

Obě **System.Data** a **Mono.Data.Sqlite** chybí některé funkce.

<a name="System.Data" />

### <a name="systemdata"></a>System.Data

Funkce chybí **System.Data.dll** se skládá z:

-  Cokoli vyžadují [System.CodeDom](https://developer.xamarin.com/api/namespace/System.CodeDom/) (např.)  [System.Data.TypedDataSetGenerator](https://developer.xamarin.com/api/type/System.Data.TypedDataSetGenerator/) )
-  Podpora souborů konfigurace XML (např.)  [System.Data.Common.DbProviderConfigurationHandler](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderConfigurationHandler/) )
-   [System.Data.Common.DbProviderFactories](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderFactories/) (závisí na podporu soubor XML konfigurace)
-   [System.Data.OleDb](https://developer.xamarin.com/api/namespace/System.Data.OleDb/)
-   [System.Data.Odbc](https://developer.xamarin.com/api/namespace/System.Data.Odbc/)
-  `System.EnterpriseServices.dll` Závislost byla *odebrat* z `System.Data.dll` , což je odebrání [SqlConnection.EnlistDistributedTransaction(ITransaction)](https://developer.xamarin.com/api/member/System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction/(System.EnterpriseServices.ITransaction)) metoda.


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

Mezitím **Mono.Data.Sqlite.dll** vyskytla žádné změny zdrojového kódu, ale místo toho může být hostitelem na počet *runtime* problémy od `Mono.Data.Sqlite.dll` váže SQLite 3.5. iOS 8, mezitím se dodává s SQLite 3.8.5. To suffice vyslovte některé věci změnily mezi dvěma verzemi.

Starší verze iOS se dodávají s následujícími verzemi SQLite:

- **iOS 7** -verze 3.7.13.
- **iOS 6** -verze 3.7.13.
- **systém iOS 5** -verze 3.7.7.
- **iOS 4** -verze 3.6.22.

Většina běžných potíží zobrazí souvisejí s dotazování schématu databáze, například určení za běhu, které sloupce existují v dané tabulce, jako například `Mono.Data.Sqlite.SqliteConnection.GetSchema` (přepsání [DbConnection.GetSchema](https://developer.xamarin.com/api/member/System.Data.Common.DbConnection.GetSchema/)) a `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` () přepsání [DbDataReader.GetSchemaTable](https://developer.xamarin.com/api/member/System.Data.Common.DbDataReader.GetSchemaTable/)). Stručně řečeno, zdá se, že cokoli pomocí [DataTable](https://developer.xamarin.com/api/type/System.Data.DataTable/) se pravděpodobně fungovat.

<a name="Data_Binding" />

## <a name="data-binding"></a>Datová vazba

Datová vazba není v tuto chvíli nepodporuje.

