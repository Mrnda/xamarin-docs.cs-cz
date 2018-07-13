---
title: System.Data v Xamarin.iosu
description: Tento dokument popisuje způsob použití System.Data a Mono.Data.Sqlite.dll pro přístup k datům SQLite aplikace pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 183079c150ad4df05424d4dbf2980a307a889352
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997195"
---
# <a name="systemdata-in-xamarinios"></a>System.Data v Xamarin.iosu

Přidává podporu pro Xamarin.iOS 8.10 [System.Data](xref:System.Data), včetně `Mono.Data.Sqlite.dll` poskytovatele ADO.NET. Podpora zahrnuje přidání následujících [sestavení](~/cross-platform/internals/available-assemblies.md):

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`

<a name="Example" />

## <a name="example"></a>Příklad

Následující program vytvoří databázi v `Documents/mydb.db3`, a pokud databáze neexistuje. dříve se vyplní ukázkovými daty. Databáze je následně dotázán pomocí výstup zapsán do `stderr`.

### <a name="add-references"></a>Přidání odkazů

Nejprve, klikněte pravým tlačítkem na **odkazy** uzlu a zvolte **upravit odkazy...**  vyberte `System.Data` a `Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "Přidání nových odkazů")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>Ukázkový kód

Následující kód ukazuje jednoduchý příklad vytvoření tabulky a vložení řádků pomocí vložených příkazů SQL:

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
> Jak je uvedeno v ukázkovém kódu výše, je špatný postup vložit řetězce v příkazy SQL, protože je zranitelný kódu [útok prostřednictvím injektáže SQL](http://en.wikipedia.org/wiki/SQL_injection).


### <a name="using-command-parameters"></a>Pomocí parametrů příkazu

Následující kód ukazuje, jak používat u parametrů příkazu k vložení uživatel zadal text bez obav do databáze (i v případě, že text obsahuje speciální znaky SQL, jako je jedním apostrof):

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

Funkce chybí **System.Data.dll** se skládá ze:

-  Cokoli vyžadující [System.CodeDom](xref:System.CodeDom) (např.)  [System.Data.TypedDataSetGenerator](xref:System.Data.TypedDataSetGenerator) )
-  Podpora souborů konfigurace XML (např.)  [System.Data.Common.DbProviderConfigurationHandler](xref:System.Data.Common.DbProviderConfigurationHandler) )
-   [System.Data.Common.DbProviderFactories](xref:System.Data.Common.DbProviderFactories) (závisí na podpoře soubor config XML)
-   [System.Data.OleDb](xref:System.Data.OleDb)
-   [System.Data.Odbc](xref:System.Data.Odbc)
-  `System.EnterpriseServices.dll` Závislost byla *odebrat* z `System.Data.dll` výsledkem odebrání [SqlConnection.EnlistDistributedTransaction(ITransaction)](xref:System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction*) metody.


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

Mezitím **Mono.Data.Sqlite.dll** utrpělo bez změny zdrojového kódu, ale místo toho může být hostitelem počtu *runtime* vydá od `Mono.Data.Sqlite.dll` váže SQLite 3.5. iOS 8, mezitím se dodává s SQLite 3.8.5. To suffice Řekněme mezi oběma verzemi změnilo několik věcí.

V následujících verzích SQLite dodávat starší verze iOS:

- **iOS 7** – verze 3.7.13.
- **iOS 6** – verze 3.7.13.
- **systém iOS 5** – verze 3.7.7.
- **iOS 4** – verze 3.6.22.

Nejběžnějších problémů zobrazí souvisí s dotazování schématu databáze, například určení za běhu, která sloupce existovat pro danou tabulku jako `Mono.Data.Sqlite.SqliteConnection.GetSchema` (přepsání [DbConnection.GetSchema](xref:System.Data.Common.DbConnection.GetSchema) a `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` (přepsání [DbDataReader.GetSchemaTable](xref:System.Data.Common.DbDataReader.GetSchemaTable). Stručně řečeno, vypadá to cokoli pomocí [DataTable](xref:System.Data.DataTable) nebude fungovat.

<a name="Data_Binding" />

## <a name="data-binding"></a>Datová vazba

Vytváření datových vazeb není v tuto chvíli nepodporuje.

