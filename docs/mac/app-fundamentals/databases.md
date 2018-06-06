---
title: Databáze v Xamarin.Mac
description: Tento článek popisuje použití kódování a klíč hodnota sledování povolit pro datovou vazbu mezi databáze SQLite a prvky uživatelského rozhraní v Xcode na rozhraní tvůrce klíč hodnota. Také vysvětluje použití SQLite.NET ORM pro poskytnutí přístupu k datům SQLite.
ms.prod: xamarin
ms.assetid: 44FAFDA8-612A-4E0F-8BB4-5C92A3F4D552
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3bc40824396aba78b2645bf9701e8e0e659c8b0a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791989"
---
# <a name="databases-in-xamarinmac"></a>Databáze v Xamarin.Mac

_Tento článek popisuje použití kódování a klíč hodnota sledování povolit pro datovou vazbu mezi databáze SQLite a prvky uživatelského rozhraní v Xcode na rozhraní tvůrce klíč hodnota. Také vysvětluje použití SQLite.NET ORM pro poskytnutí přístupu k datům SQLite._

## <a name="overview"></a>Přehled

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup do stejné databáze SQLite, které můžete přístup k aplikaci Xamarin.iOS nebo Xamarin.Android.

V tomto článku jsme se pokrývajících dva způsoby, jak získat přístup k datům SQLite:

1. **Přímý přístup** – přímým přístupem k databázi SQLite, můžeme použít data z databáze pro klíč hodnota kódování a vytvoření datové vazby s prvky uživatelského rozhraní vytvořili v Xcode na rozhraní tvůrce. Pomocí kódování a datové vazby v aplikaci Xamarin.Mac techniky klíč hodnota, může výrazně snížit množství kód, který máte k zápisu a udržovat naplnit a pracovat s prvky uživatelského rozhraní. Máte také výhodou další oddělení zálohování dat (_datový Model_) z vaší přední ukončení uživatelské rozhraní (_Model-View-Controller_), tečka na začátku k snadněji provádět údržbu, flexibilnější aplikace návrh.
2. **SQLite.NET ORM** – pomocí open source [SQLite.NET](http://www.sqlite.org) vztah Manager ORM (Object) jsme může výrazně snížit množství kód potřebný k čtení a zápisu dat z databáze SQLite. Tato data pak slouží k naplnění položku uživatelského rozhraní například zobrazení tabulky.

[![Příkladem spuštěné aplikaci](databases-images/intro01.png "příklad spuštěné aplikaci")](databases-images/intro01-large.png#lightbox)

V tomto článku vám nabídneme základní informace o práci s klíč hodnota kódování a datové vazby v aplikaci Xamarin.Mac s databáze SQLite. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Vzhledem k tomu, že jsme budete používat klíč hodnota kódování a datové vazby, prosím fungovat prostřednictvím [datové vazby a klíč hodnota kódování](~/mac/app-fundamentals/databinding.md) nejprve jako základní techniky a koncepty se budeme, bude použita v této dokumentaci a jeho ukázková aplikace.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` atributy Umožňuje propojit až tříd jazyka C# a uživatelského rozhraní jazyka Objective-C objekty elementy.

## <a name="direct-sqlite-access"></a>Přímý přístup SQLite

Pro SQLite data, která má být vázána k elementům uživatelského rozhraní v Xcode na rozhraní tvůrce vysoce, doporučuje přístupu k databázi SQLite přímo (na rozdíl od pomocí technik, jako je například ORM), vzhledem k tomu, že máte celkový řídit způsob, jakým data jsou zapsána a čtení  z databáze.

Jak jsme mohli vidět v [datové vazby a klíč-hodnota kódování](~/mac/app-fundamentals/databinding.md) dokumentace, pomocí kódování a datové vazby v aplikaci Xamarin.Mac techniky klíč hodnota můžete výrazně zkrátit dobu kód, který máte k zápisu a Udržujte naplnit a pracovat s prvky uživatelského rozhraní. V kombinaci s přímým přístupem k databázi SQLite, může také výrazně snížit množství kód potřebný k čtení a zápisu dat do dané databáze.

V tomto článku jsme se úpravy ukázkovou aplikaci z datové vazby a klíč hodnota kódování dokumentu použít databáze SQLite jako zdroj zálohování pro vazbu.

### <a name="including-sqlite-database-support"></a>Podpora databáze SQLite včetně

Než budeme pokračovat, potřebujeme do naší aplikaci zahrnutím odkazy na několik přidat podporu databáze SQLite. Soubory knihoven DLL.

Postupujte takto:

1. V **řešení Pad**, klikněte pravým tlačítkem na **odkazy** složky a vyberte **upravit odkazy**.
2. Vyberte **Mono.Data.Sqlite** a **System.Data** sestavení: 

    [![Přidání odkazů požadované](databases-images/reference01.png "přidání požadované odkazy")](databases-images/reference01-large.png#lightbox)
3. Klikněte **OK** tlačítko a uložte změny a přidejte odkazy na.

### <a name="modifying-the-data-model"></a>Úprava datový model

Teď, když jsme doplnili podporu pro přímý přístup k databázi SQLite naše aplikace, je potřeba upravit objekt modelu našich dat pro čtení a zápisu dat z databáze (stejně jako zadejte klíč hodnota kódování a datová vazba). V případě naše ukázková aplikace jsme upravíte **PersonModel.cs** třídy a nastavit jej vypadat třeba takto:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _ID = "";
        private string _managerID = "";
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        private SqliteConnection _conn = null;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        [Export("ID")]
        public string ID {
            get { return _ID; }
            set {
                WillChangeValue ("ID");
                _ID = value;
                DidChangeValue ("ID");
            }
        }

        [Export("ManagerID")]
        public string ManagerID {
            get { return _managerID; }
            set {
                WillChangeValue ("ManagerID");
                _managerID = value;
                DidChangeValue ("ManagerID");
            }
        }

        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }

        public PersonModel (string id, string name, string occupation)
        {
            // Initialize
            this.ID = id;
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (SqliteConnection conn, string id)
        {
            // Load from database
            Load (conn, id);
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion

        #region SQLite Routines
        public void Create(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Create new record ID?
            if (ID == "") {
                ID = Guid.NewGuid ().ToString();
            }

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Save manager ID and create the sub record
                Person.ManagerID = ID;
                Person.Create (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Update(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Update sub record
                Person.Update (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Load(SqliteConnection conn, string id) {
            bool shouldClose = false;

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Is the database already open?
            if (conn.State != ConnectionState.Open) {
                shouldClose = true;
                conn.Open ();
            }

            // Execute query
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", id);

                using (var reader = command.ExecuteReader ()) {
                    while (reader.Read ()) {
                        // Pull values back into class
                        ID = (string)reader [0];
                        Name = (string)reader [1];
                        Occupation = (string)reader [2];
                        isManager = (bool)reader [3];
                        ManagerID = (string)reader [4];
                    }
                }
            }

            // Is this a manager?
            if (isManager) {
                // Yes, load children
                using (var command = conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

                    // Populate with data from the record
                    command.Parameters.AddWithValue ("@COL1", id);

                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Load child and add to collection
                            var childID = (string)reader [0];
                            var person = new PersonModel (conn, childID);
                            _people.Add (person);
                        }
                    }
                }
            }

            // Should we close the connection to the database
            if (shouldClose) {
                conn.Close ();
            }

            // Save last connection
            _conn = conn;
        }

        public void Delete(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Empty class
            ID = "";
            ManagerID = "";
            Name = "";
            Occupation = "";
            isManager = false;
            _people = new NSMutableArray();

            // Save last connection
            _conn = conn;
        }
        #endregion
    }
}
```

Podívejme se na změny v níže uvedené podrobnosti.

Nejprve přidali jsme některé příkazy, které jsou nutné k použití SQLite a přidali jsme proměnnou pro uložení naše poslední připojení k databázi SQLite:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection _conn = null;
```

Chcete-li automaticky uloží změny do záznam do databáze, když uživatel změní obsah v uživatelském rozhraní prostřednictvím datových vazeb použijeme toto uložené připojení:

```csharp
[Export("Name")]
public string Name {
    get { return _name; }
    set {
        WillChangeValue ("Name");
        _name = value;
        DidChangeValue ("Name");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("Occupation")]
public string Occupation {
    get { return _occupation; }
    set {
        WillChangeValue ("Occupation");
        _occupation = value;
        DidChangeValue ("Occupation");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}
```

Některé změny provedené **název**, **povolání** nebo **isManager** vlastnosti se odeslal do databáze, pokud byla data uložena existuje před (například pokud `_conn` Proměnná není `null`). V dalším kroku Podíváme se na metody, které jsme přidali do **vytvořit**, **aktualizace**, **zatížení** a **odstranit** osoby z databáze.

#### <a name="create-a-new-record"></a>Vytvořit nový záznam

Chcete-li vytvořit nový záznam v databázi SQLite byl přidán následující kód:

```csharp
public void Create(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Create new record ID?
    if (ID == "") {
        ID = Guid.NewGuid ().ToString();
    }

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Save manager ID and create the sub record
        Person.ManagerID = ID;
        Person.Create (conn);
    }

    // Save last connection
    _conn = conn;
}
```

Používáme `SQLiteCommand` vytvořit nový záznam v databázi. Nemůžeme získat nový příkaz z `SQLiteConnection` (kontext), jsme předán do metody voláním `CreateCommand`. Potom nastaví pokyn SQL ve skutečnosti zapsat nový záznam, poskytuje parametry pro skutečnými hodnotami:

```csharp
command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";
```

Později jsme nastavit hodnoty pro parametry pomocí `Parameters.AddWithValue` metodu `SQLiteCommand`. Pomocí parametrů je zajištěno, že hodnoty (například v jednoduchých uvozovkách) získat správně kódovaný před odesláním do SQLite. Příklad:

```csharp
command.Parameters.AddWithValue ("@COL1", ID);
```

Navíc vzhledem k tomu, že uživatel může být manažera a mít kolekce zaměstnanci pod nimi, Snažíme se rekurzivní volání `Create` metodu na tyto osoby o jejich uložení do databáze také:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Save manager ID and create the sub record
    Person.ManagerID = ID;
    Person.Create (conn);
}
```

#### <a name="updating-a-record"></a>Aktualizace záznamu

Chcete-li aktualizovat existující záznam v databázi SQLite byl přidán následující kód:

```csharp
public void Update(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Update sub record
        Person.Update (conn);
    }

    // Save last connection
    _conn = conn;
}
```

Jako **vytvořit** vyšších verzích se nám získat `SQLiteCommand` z předaného v `SQLiteConnection`a nastavte naše SQL k aktualizaci našemu záznamu (za předpokladu parametry):

```csharp
command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";
```

Jsme vyplňte hodnoty parametrů (Příklad: `command.Parameters.AddWithValue ("@COL1", ID);`) a znovu rekurzivní volání aktualizace na všechny podřízené záznamy:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Update sub record
    Person.Update (conn);
}
```
#### <a name="loading-a-record"></a>Načítání záznam

Načtení existující záznam z databáze SQLite byl přidán následující kód:

```csharp
public void Load(SqliteConnection conn, string id) {
    bool shouldClose = false;

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Is the database already open?
    if (conn.State != ConnectionState.Open) {
        shouldClose = true;
        conn.Open ();
    }

    // Execute query
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Pull values back into class
                ID = (string)reader [0];
                Name = (string)reader [1];
                Occupation = (string)reader [2];
                isManager = (bool)reader [3];
                ManagerID = (string)reader [4];
            }
        }
    }

    // Is this a manager?
    if (isManager) {
        // Yes, load children
        using (var command = conn.CreateCommand ()) {
            // Create new command
            command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

            // Populate with data from the record
            command.Parameters.AddWithValue ("@COL1", id);

            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Load child and add to collection
                    var childID = (string)reader [0];
                    var person = new PersonModel (conn, childID);
                    _people.Add (person);
                }
            }
        }
    }

    // Should we close the connection to the database
    if (shouldClose) {
        conn.Close ();
    }

    // Save last connection
    _conn = conn;
}
```

Protože rutiny volat rekurzivně z nadřazeného objektu (například objekt správce načítání objektu jejich zaměstnanci), přidala se speciální kód pro zpracování otvírání a zavírání připojení k databázi:

```csharp
bool shouldClose = false;
...

// Is the database already open?
if (conn.State != ConnectionState.Open) {
    shouldClose = true;
    conn.Open ();
}
...

// Should we close the connection to the database
if (shouldClose) {
    conn.Close ();
}

```

Jako vždy nastaví naše SQL načtení záznamu a použití parametrů:

```csharp
// Create new command
command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", id);
```

Nakonec používáme čtecí modul dat provedení dotazu a vrátíte se pole záznamu (které jsme zkopírujte do instance `PersonModel` třída):

```csharp
using (var reader = command.ExecuteReader ()) {
    while (reader.Read ()) {
        // Pull values back into class
        ID = (string)reader [0];
        Name = (string)reader [1];
        Occupation = (string)reader [2];
        isManager = (bool)reader [3];
        ManagerID = (string)reader [4];
    }
}
```

Pokud tato osoba správce, je potřeba také načíst všechny své zaměstnance (znovu pomocí volání rekurzivní jejich `Load` metoda):

```csharp
// Is this a manager?
if (isManager) {
    // Yes, load children
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Load child and add to collection
                var childID = (string)reader [0];
                var person = new PersonModel (conn, childID);
                _people.Add (person);
            }
        }
    }
}
```

#### <a name="deleting-a-record"></a>Odstranění záznamu

Pokud chcete odstranit existující záznam z databáze SQLite byl přidán následující kód:

```csharp
public void Delete(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Empty class
    ID = "";
    ManagerID = "";
    Name = "";
    Occupation = "";
    isManager = false;
    _people = new NSMutableArray();

    // Save last connection
    _conn = conn;
}
```

Zde poskytujeme SQL k odstranění správci záznamů a záznamy všechny zaměstnance v rámci tohoto správce (pomocí parametrů):

```csharp
// Create new command
command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", ID);
```

Po odebrání záznamu, jsme vyčistit aktuální instancí třídy `PersonModel` třídy:

```csharp
// Empty class
ID = "";
ManagerID = "";
Name = "";
Occupation = "";
isManager = false;
_people = new NSMutableArray();
```

### <a name="initializing-the-database"></a>Inicializace databáze

Vzhledem k změnám našich datový Model na místě k čtení a zápis do databáze je potřeba otevřít připojení k databázi a provést jeho inicializaci při prvním spuštění. Umožňuje přidat následující kód do našich **MainWindow.cs** souboru:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection DatabaseConnection = null;
...

private SqliteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "People.db3");

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);
    if (!exists)
        SqliteConnection.CreateFile (db);

    // Create connection to the database
    var conn = new SqliteConnection("Data Source=" + db);

    // Set the structure of the database
    if (!exists) {
        var commands = new[] {
            "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
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

        // Build list of employees
        var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
        Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
        Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
        Craig.Create (conn);

        var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
        Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
        Larry.Create (conn);
    }

    // Return new connection
    return conn;
}
```

Podívejme bližší pohled na výše uvedeném kódu. Nejdřív jsme vyberte umístění pro novou databázi (v tomto příkladu plocha uživatele), podívejte se, zda databáze existuje a pokud tomu tak není, vytvořte ji:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "People.db3");

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
if (!exists)
    SqliteConnection.CreateFile (db);
```

Potom naváže připojení k databázi pomocí cestu, kterou jsme vytvořili výše:

```csharp
var conn = new SqliteConnection("Data Source=" + db);
```

Poté vytvoříme všechny tabulky SQL v databázi, která je nutné:

```csharp
var commands = new[] {
    "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
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
```

Nakonec jsme pomocí našeho datového modelu (`PersonModel`) k vytvoření výchozí sadu záznamů pro databázi první čas spuštění aplikace nebo pokud chybí databáze:

```csharp
// Build list of employees
var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
Craig.Create (conn);

var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
Larry.Create (conn);
``` 

Když se aplikace spustí a otevře se okno Main, zajišťujeme, připojení k databázi pomocí kódu, který jsme přidali výše:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get access to database
    DatabaseConnection = GetDatabaseConnection ();
}
```

### <a name="loading-bound-data"></a>Načítání vázaných dat

Se všemi součástmi pro přímý přístup k vázaných dat z databáze SQLite na místě nemůžeme načíst data v jiné zobrazení, která poskytuje naší aplikaci a automaticky se bude zobrazovat v našem uživatelského rozhraní.

#### <a name="loading-a-single-record"></a>Načítání jedním záznamem

Načíst jeden záznam, kde je ID vědět, můžeme použít následující kód:

```csharp
Person = new PersonModel (Conn, "0");
```

#### <a name="loading-all-records"></a>Načtení všech záznamů

Pokud chcete načíst všechny uživatele, bez ohledu na to, pokud se správce nebo není, použijte následující kód:

```csharp
// Load all employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People]";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();

```

Tady používáme přetížení v konstruktoru pro `PersonModel` třídy k načtení do paměti osoby:

```csharp
var person = new PersonModel (_conn, childID);
```

Také voláme třídu Data vázána k přidá uživatele do našich kolekce uživatelů `AddPerson (person)`, zajistíte tak, že naše uživatelské rozhraní rozpozná změnu a zobrazí se:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}
```

#### <a name="loading-top-level-records-only"></a>Načítání jenom záznamy nejvyšší úrovně

Chcete-li načíst pouze správce (například k zobrazení dat v zobrazení osnovy), můžeme použít následující kód:

```csharp
// Load only managers employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();
```

Pouze skutečné rozdíl v v příkazu SQL (což způsobí načtení pouze správci `command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1"`), ale funguje stejně jako část výše jinak.

<a name="Databases-and-ComboBoxes" />

### <a name="databases-and-comboboxes"></a>Databáze a polích se seznamem

Ovládací prvky nabídky, která je k dispozici v systému macOS (například pole se seznamem) můžete nastavit k naplnění rozevíracího seznamu, buď z interní seznamu (která se dá předem definované v Tvůrci rozhraní nebo naplněno prostřednictvím kódu) nebo tím, že poskytuje zdroj dat vlastní, externí. V tématu [poskytuje Data ovládacího prvku nabídka](~/mac/user-interface/standard-controls.md#Providing-Menu-Control-Data) další podrobnosti.

Jako příklad upravit jednoduchá vazba výše uvedených příkladech v Tvůrci rozhraní přidat pole se seznamem a vystavit pomocí výstupu s názvem `EmployeeSelector`:

[![Vystavení výstupu pole se seznamem](databases-images/combo01.png "vystavení výstupu pole se seznamem")](databases-images/combo01-large.png#lightbox)

V **atributy Inspector**, zkontrolujte **Autocompletes** a **zdroj dat používá** vlastnosti:

![Nakonfigurovat atributy pole se seznamem](databases-images/combo02.png "nakonfigurovat atributy pole se seznamem")

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci.

#### <a name="providing-combobox-data"></a>Poskytuje data pole se seznamem

V dalším kroku přidejte novou třídu do projektu názvem `ComboBoxDataSource` vypadá takto:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public class ComboBoxDataSource : NSComboBoxDataSource
    {
        #region Private Variables
        private SqliteConnection _conn = null;
        private string _tableName = "";
        private string _IDField = "ID";
        private string _displayField = "";
        private nint _recordCount = 0;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        public string TableName {
            get { return _tableName; }
            set { 
                _tableName = value;
                _recordCount = GetRecordCount ();
            }
        }

        public string IDField {
            get { return _IDField; }
            set {
                _IDField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public string DisplayField {
            get { return _displayField; }
            set { 
                _displayField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public nint RecordCount {
            get { return _recordCount; }
        }
        #endregion

        #region Constructors
        public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.DisplayField = displayField;
        }

        public ComboBoxDataSource (SqliteConnection conn, string tableName, string idField, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.IDField = idField;
            this.DisplayField = displayField;
        }
        #endregion

        #region Private Methods
        private nint GetRecordCount ()
        {
            bool shouldClose = false;
            nint count = 0;

            // Has a Table, ID and display field been specified?
            if (TableName !="" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read count from query
                            var result = (long)reader [0];
                            count = (nint)result;
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return the number of records
            return count;
        }
        #endregion

        #region Public Methods
        public string IDForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string ValueForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string IDForValue (string value)
        {
            NSString result = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", value);

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            result = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return result;
        }
        #endregion 

        #region Override Methods
        public override nint ItemCount (NSComboBox comboBox)
        {
            return RecordCount;
        }

        public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public override nint IndexOfItem (NSComboBox comboBox, string value)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";
            nint index = NSRange.NotFound;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read () && !found) {
                            // Read the display field from the query
                            field = (string)reader [0];
                            ++index;

                            // Is this the value we are searching for?
                            if (value == field) {
                                // Yes, exit loop
                                found = true;
                            }
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return index;
        }

        public override string CompletedString (NSComboBox comboBox, string uncompletedString)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Escape search string
                uncompletedString = uncompletedString.Replace ("'", "");

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            field = (string)reader [0];
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return field;
        }
        #endregion
    }
}
```

V tomto příkladu se vytváří nové `NSComboBoxDataSource` , může být položky pole se seznamem z jakéhokoli zdroje dat. SQLite. Nejprve definujeme následující vlastnosti:

- `Conn` -Získá nebo nastaví připojení k databázi SQLite.
- `TableName` -Získá nebo nastaví název tabulky.
- `IDField` -Získá nebo nastaví pole, která poskytuje jedinečné ID pro danou tabulku. Výchozí hodnota je `ID`.
- `DisplayField` -Získá nebo nastaví pole, které se zobrazí v rozevíracím seznamu.
- `RecordCount` -Získá počet záznamů v dané tabulce.

Při vytváření nové instance objektu, jsme předat připojení, název tabulky, volitelně pole ID a pole zobrazení:

```csharp
public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
{
    // Initialize
    this.Conn = conn;
    this.TableName = tableName;
    this.DisplayField = displayField;
}
```

`GetRecordCount` Metoda vrátí počet záznamů v dané tabulce:

```csharp
private nint GetRecordCount ()
{
    bool shouldClose = false;
    nint count = 0;

    // Has a Table, ID and display field been specified?
    if (TableName !="" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read count from query
                    var result = (long)reader [0];
                    count = (nint)result;
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return the number of records
    return count;
}
```

Vždy, když je volána `TableName`, `IDField` nebo `DisplayField` změně hodnoty vlastnosti.

`IDForIndex` Metoda vrátí jedinečné ID (`IDField`) pro záznam v indexu daného rozevírací seznam položky: 

```csharp
public string IDForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

`ValueForIndex` Vrátí metoda hodnotu (`DisplayField`) pro položku indexem dané rozevíracího seznamu:

```csharp
public string ValueForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

`IDForValue` Metoda vrátí jedinečné ID (`IDField`) pro dané hodnoty hodnotu (`DisplayField`):

```csharp
public string IDForValue (string value)
{
    NSString result = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", value);

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    result = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return result;
}
```

`ItemCount` Vrací předpočítané počet položek v seznamu jako při počítaného `TableName`, `IDField` nebo `DisplayField` došlo ke změně vlastností:

```csharp
public override nint ItemCount (NSComboBox comboBox)
{
    return RecordCount;
}
```

`ObjectValueForItem` Metoda obsahuje hodnotu (`DisplayField`) pro index daného rozevírací seznam položky:

```csharp
public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

Všimněte si, že se používá `LIMIT` a `OFFSET` příkazy v našem SQLite příkazu omezit na jeden záznam jsme potřeby.

`IndexOfItem` Metoda vrátí index položky rozevírací hodnoty (`DisplayField`) daného:

```csharp
public override nint IndexOfItem (NSComboBox comboBox, string value)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";
    nint index = NSRange.NotFound;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read () && !found) {
                    // Read the display field from the query
                    field = (string)reader [0];
                    ++index;

                    // Is this the value we are searching for?
                    if (value == field) {
                        // Yes, exit loop
                        found = true;
                    }
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return index;
}
```

Pokud hodnota nebyl nalezen, `NSRange.NotFound` hodnota je návratový a vypnete všechny položky v rozevíracím seznamu.

`CompletedString` Metoda vrátí první hodnotu odpovídající (`DisplayField`) pro položku částečně typu:

```csharp
public override string CompletedString (NSComboBox comboBox, string uncompletedString)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Escape search string
        uncompletedString = uncompletedString.Replace ("'", "");

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    field = (string)reader [0];
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return field;
}
```

#### <a name="displaying-data-and-responding-to-events"></a>Zobrazení dat a reagování na události

Všechny vytvořené společně nabídnou, upravte `SubviewSimpleBindingController` vypadá takto:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public partial class SubviewSimpleBindingController : AppKit.NSViewController
    {
        #region Private Variables
        private PersonModel _person = new PersonModel();
        private SqliteConnection Conn;
        #endregion

        #region Computed Properties
        //strongly typed view accessor
        public new SubviewSimpleBinding View {
            get {
                return (SubviewSimpleBinding)base.View;
            }
        }

        [Export("Person")]
        public PersonModel Person {
            get {return _person; }
            set {
                WillChangeValue ("Person");
                _person = value;
                DidChangeValue ("Person");
            }
        }

        public ComboBoxDataSource DataSource {
            get { return EmployeeSelector.DataSource as ComboBoxDataSource; }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public SubviewSimpleBindingController (IntPtr handle) : base (handle)
        {
            Initialize ();
        }

        // Called when created directly from a XIB file
        [Export ("initWithCoder:")]
        public SubviewSimpleBindingController (NSCoder coder) : base (coder)
        {
            Initialize ();
        }

        // Call to load from the XIB/NIB file
        public SubviewSimpleBindingController (SqliteConnection conn) : base ("SubviewSimpleBinding", NSBundle.MainBundle)
        {
            // Initialize
            this.Conn = conn;
            Initialize ();
        }

        // Shared initialization code
        void Initialize ()
        {
        }
        #endregion

        #region Private Methods
        private void LoadSelectedPerson (string id)
        {

            // Found?
            if (id != "") {
                // Yes, load requested record
                Person = new PersonModel (Conn, id);
            }
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Configure Employee selector dropdown
            EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");

            // Wireup events
            EmployeeSelector.Changed += (sender, e) => {
                // Get ID
                var id = DataSource.IDForValue (EmployeeSelector.StringValue);
                LoadSelectedPerson (id);
            };

            EmployeeSelector.SelectionChanged += (sender, e) => {
                // Get ID
                var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
                LoadSelectedPerson (id);
            };

            // Auto select the first person
            EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
            Person = new PersonModel (Conn, DataSource.IDForIndex(0));
    
        }
        #endregion
    }
}
```

`DataSource` Vlastnost poskytuje zástupce `ComboBoxDataSource` (vytvořili výše) připojené k pole se seznamem.

`LoadSelectedPerson` Metoda osoba, která načte z databáze pro danou jedinečné ID:

```csharp
private void LoadSelectedPerson (string id)
{

    // Found?
    if (id != "") {
        // Yes, load requested record
        Person = new PersonModel (Conn, id);
    }
}
```

V `AwakeFromNib` přepsání metody nejdřív jsme připojte instanci naše vlastní zdroj dat pole se seznamem:

```csharp
EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");
```

V dalším kroku odpovíte uživatelské úpravy textovou hodnotu tohoto pole se seznamem hledání přidružené jedinečné ID (`IDField`) dat najít prezentací a načítání dané osoby, pokud:

```csharp
EmployeeSelector.Changed += (sender, e) => {
    // Get ID
    var id = DataSource.IDForValue (EmployeeSelector.StringValue);
    LoadSelectedPerson (id);
};
```

Jsme také načíst nové osobě, pokud uživatel vybere novou položku z rozevíracího seznamu:

```csharp
EmployeeSelector.SelectionChanged += (sender, e) => {
    // Get ID
    var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
    LoadSelectedPerson (id);
};
```

Nakonec jsme automatické vyplnění pole se seznamem a zobrazit osoba s první položky v seznamu:

```csharp
// Auto select the first person
EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
Person = new PersonModel (Conn, DataSource.IDForIndex(0));
```

## <a name="sqlitenet-orm"></a>SQLite.NET ORM

Jak jsme uvedli výše, a to pomocí open source [SQLite.NET](http://www.sqlite.org) vztah Manager ORM (Object) jsme může výrazně snížit množství kód potřebný k čtení a zápisu dat z databáze SQLite. Toto nemusí být nejlepší směrování se má provést při vytvoření vazby dat z důvodu několik požadavků, které klíč hodnota kódování a datová vazba umístit na objekt.

Podle webu SQLite.Net _"SQLite je softwarové knihovny, která implementuje samostatná, bez serveru, bez nutnosti konfigurace, transakční databázový stroj SQL. SQLite je nejčastěji nasazené databázový stroj na světě. Zdrojový kód pro SQLite je ve veřejné doméně."_

V následující části ukážeme, jak používat SQLite.Net poskytující data pro zobrazení tabulky.

### <a name="including-the-sqlitenet-nuget"></a>Včetně SQLite.net NuGet

SQLite.NET je zobrazen jako balíčku NuGet, který jste do aplikace zahrnout. Před přidáním podpory databáze pomocí SQLite.NET, musíme zahrnout tento balíček.

Udělejte k přidání balíčku:

1. V **řešení Pad**, klikněte pravým tlačítkem myši **balíčky** složky a vyberte **přidat balíčky...**
2. Zadejte `SQLite.net` v **vyhledávacího pole** a vyberte **sqlite net** položku:

    [![Probíhá přidávání balíčku SQLite NuGet](databases-images/nuget01.png "přidat balíček SQLite NuGet")](databases-images/nuget01-large.png#lightbox)
3. Klikněte **přidat balíček** tlačítko Dokončit.

### <a name="creating-the-data-model"></a>Vytvoření datového modelu

Umožňuje přidat novou třídu do projektu a volání v `OccupationModel`. V dalším kroku tady upravit **OccupationModel.cs** souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using SQLite;

namespace MacDatabase
{
    public class OccupationModel
    {
        #region Computed Properties
        [PrimaryKey, AutoIncrement]
        public int ID { get; set; }

        public string Name { get; set;}
        public string Description { get; set;}
        #endregion

        #region Constructors
        public OccupationModel ()
        {
        }

        public OccupationModel (string name, string description)
        {

            // Initialize
            this.Name = name;
            this.Description = description;

        }
        #endregion
    }
}
```

Nejprve zahrnuta SQLite.NET (`using Sqlite`), pak zveřejňujeme několik vlastností, z nichž každý se zapíšou do databáze při uložení tento záznam. První vlastností jsme zkontrolujte jako primární klíč a nastavte ji na automatické zvýšení následujícím způsobem:

```csharp
[PrimaryKey, AutoIncrement]
public int ID { get; set; }
```
### <a name="initializing-the-database"></a>Inicializace databáze

Vzhledem k změnám našich datový Model na místě k čtení a zápis do databáze je potřeba otevřít připojení k databázi a provést jeho inicializaci při prvním spuštění. Pojďme přidejte následující kód:

```csharp
using SQLite;
...

public SQLiteConnection Conn { get; set; }
...

private SQLiteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "Occupation.db3");
    OccupationModel Occupation;

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);

    // Create connection to database
    var conn = new SQLiteConnection (db);

    // Initially populate table?
    if (!exists) {
        // Yes, build table
        conn.CreateTable<OccupationModel> ();

        // Add occupations
        Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
        conn.Insert (Occupation);
    }

    return conn;
}
```

Nejdřív získat cestu k databázi (plocha uživatele v tomto případě) a zda databáze již existuje:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "Occupation.db3");
OccupationModel Occupation;

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
```

Potom naváže připojení k databázi na cestu, kterou jsme vytvořili výše:

```csharp
var conn = new SQLiteConnection (db);
```

Nakonec vytvořit tabulku a přidat některé výchozí záznamy:

```csharp
// Yes, build table
conn.CreateTable<OccupationModel> ();

// Add occupations
Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
conn.Insert (Occupation);
```

### <a name="adding-a-table-view"></a>Přidání zobrazení tabulky

Jako příklad použití přidáme naše uživatelské rozhraní v Xcode na rozhraní tvůrce zobrazení tabulky. Budete zveřejňujeme Tato tabulka zobrazení prostřednictvím výstupu (`OccupationTable`), můžeme k němu přístup prostřednictvím kódu C#:

[![Vystavení tabulka zobrazení výstupu](databases-images/table01.png "vystavení tabulka zobrazení výstupu")](databases-images/table01-large.png#lightbox)

Potom přidáme vlastní třídy naplnila Tato tabulka s daty z databáze SQLite.NET.

### <a name="creating-the-table-data-source"></a>Vytváření zdroje dat tabulky

Umožňuje vytvořit vlastní zdroj dat poskytující data pro naše tabulky. Nejprve přidejte novou třídu s názvem `TableORMDatasource` vypadá takto:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDatasource : NSTableViewDataSource
    {
        #region Computed Properties
        public List<OccupationModel> Occupations { get; set;} = new List<OccupationModel>();
        public SQLiteConnection Conn { get; set; }
        #endregion

        #region Constructors
        public TableORMDatasource (SQLiteConnection conn)
        {
            // Initialize
            this.Conn = conn;
            LoadOccupations ();
        }
        #endregion

        #region Public Methods
        public void LoadOccupations() {

            // Get occupations from database
            var query = Conn.Table<OccupationModel> ();

            // Copy into table collection
            Occupations.Clear ();
            foreach (OccupationModel occupation in query) {
                Occupations.Add (occupation);
            }

        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Occupations.Count;
        }
        #endregion
    }
}
```

Při vytváření instance této třídy později, jsme budete předávat naše otevřít připojení k databázi SQLite.NET. `LoadOccupations` Metoda dotazuje databázi a zkopíruje nalezen záznamy do paměti (pomocí našich `OccupationModel` datového modelu).

### <a name="creating-the-table-delegate"></a>Vytvoření tabulky delegát

Finální třídy, na kterou je třeba je vlastní tabulky delegátovi, aby se zobrazují informace, které mají byl načten z SQLite.NET databáze. Umožňuje přidat nový `TableORMDelegate` do našich projektu a nastavit jej vypadat třeba takto:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDelegate : NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "OccCell";
        #endregion

        #region Private Variables
        private TableORMDatasource DataSource;
        #endregion

        #region Constructors
        public TableORMDelegate (TableORMDatasource dataSource)
        {
            // Initialize
            this.DataSource = dataSource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Occupation":
                view.StringValue = DataSource.Occupations [(int)row].Name;
                break;
            case "Description":
                view.StringValue = DataSource.Occupations [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Tady používáme zdroj dat `Occupations` kolekce (který byl načten z databáze SQLite.NET) k vyplnění sloupce naše tabulky prostřednictvím `GetViewForItem` přepsání metody.

### <a name="populating-the-table"></a>Naplnění tabulky

Se všemi částí na místě můžeme vyplnění naše tabulky při jeho je zvětšený ze souboru .xib přepsáním `AwakeFromNib` metoda, která by vypadat podobně jako následující:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get database connection
    Conn = GetDatabaseConnection ();

    // Create the Occupation Table Data Source and populate it
    var DataSource = new TableORMDatasource (Conn);

    // Populate the Product Table
    OccupationTable.DataSource = DataSource;
    OccupationTable.Delegate = new TableORMDelegate (DataSource);
}
```

Nejdřív jsme získat přístup do databáze hotový SQLite.NET vytvoření a naplnění ji, pokud ještě neexistuje. V dalším kroku vytvoříme novou instanci naše vlastní zdroje dat tabulky, předejte naše připojení k databázi a nemůžeme připojit k tabulce. Nakonec jsme vytvořit novou instanci třídy Naše vlastní tabulky delegáta, předejte naše zdroj dat a připojte ji k tabulce.

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce datové vazby a klíč hodnota psaní kódu s použitím databáze SQLite v aplikaci Xamarin.Mac trvá. Nejprve hledá v vystavení třída C# pro Objective-C pomocí klíč hodnota kódování (KVC) a klíč hodnota sledování (KVO). V dalším kroku se vám ukázal, jak používat třídu kompatibilní KVO a navázat jej Data prvky uživatelského rozhraní v Tvůrci rozhraní pro Xcode. V článku také zahrnutých práci s daty SQLite prostřednictvím SQLite.NET ORM a zobrazení dat v tabulkovém zobrazení.



## <a name="related-links"></a>Související odkazy

- [MacDatabase (ukázka)](https://developer.xamarin.com/samples/mac/MacDatabase/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Datové vazby a klíč hodnota kódování](~/mac/app-fundamentals/databinding.md)
- [Standardní ovládací prvky](~/mac/user-interface/standard-controls.md)
- [Zobrazení tabulek](~/mac/user-interface/table-view.md)
- [Zobrazení osnovy](~/mac/user-interface/outline-view.md)
- [Zobrazení kolekce](~/mac/user-interface/collection-view.md)
- [Klíč hodnota kódování Průvodce programováním](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Úvod do kakao vazby programování témata](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Úvod do kakao vazby odkaz](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [systému macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
