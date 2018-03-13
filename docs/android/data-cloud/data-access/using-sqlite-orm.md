---
title: "Pomocí SQLite.NET"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: b9523d76c04dae97b74744fbe2bd6bc7022c3194
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="using-sqlitenet"></a>Pomocí SQLite.NET

SQLite.NET knihovny, která doporučuje Xamarin je velmi základní ORM, který vám umožní snadno uložení a načtení objektů v místní databázi SQLite na zařízení s Androidem. ORM znamená relační mapování objektu &ndash; rozhraní API, které vám umožní uložit a načíst "objekty" z databáze bez nutnosti psaní příkazů SQL.

## <a name="using-sqlitenet"></a>Pomocí SQLite.NET

Zahrnout knihovně SQLite.NET aplikace Xamarin, přidejte [balíček SQLite.net PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) pomocí projektu **SQLite net PCL** balíček NuGet:

[![Balíček SQLite.NET NuGet](using-sqlite-orm-images/image1a-sml.png "balíček SQLite.NET NuGet")](using-sqlite-orm-images/image1a.png#lightbox)

Až budete mít k dispozici SQLite.NET knihovny, postupujte podle tyto tři kroky ji používat pro přístup k databázi:


1.  **Přidat pomocí příkazu** &ndash; do kdy je potřeba přístup k datům soubory C# přidejte následující příkaz: 

    ```csharp
    using SQLite;
    ```

2.  **Vytvořit prázdnou databázi** &ndash; odkaz na databázi lze vytvořit pomocí předání konstruktoru třídy SQLiteConnection cesta k souboru. Není potřeba zkontrolovat, zda soubor již existuje &ndash; bude automaticky vytvořena podle potřeby, v opačném případě se otevře existující soubor databáze. `dbPath` Proměnná by měla určit podle pravidla popsané dříve v tomto dokumentu:

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3.  **Uložení dat** &ndash; po vytvoření objektu SQLiteConnection příkazy databáze jsou provedeny voláním její metody, jako je například CreateTable a vložení takto:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4.  **Načtení dat** &ndash; k načtení objektu (nebo seznam objektů), použijte následující syntaxi:

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>Základní Data Access – ukázka

*DataAccess_Basic* ukázkový kód pro tento dokument vypadat třeba takto při spuštění v systému Android. Kód ukazuje, jak provádět jednoduché operace SQLite.NET a zobrazí výsledky v jako text v hlavním okně aplikace.


**Android**

![Ukázka Android SQLite.NET](using-sqlite-orm-images/image3.png "Android SQLite.NET vzorku")

Následující příklad kódu ukazuje interakce celou databázi pomocí knihovny SQLite.NET k zapouzdření základní přístup k databázi.
Zobrazuje:

1.  Vytvoření souboru databáze

2.  Vytváření objektů a jejich uložením vkládání některá data

3.  Dotazování na data

Budete muset zahrnují tyto obory názvů:

```csharp
using SQLite; // from the github SQLite.cs class
```

Poslední vyžaduje, že jste přidali do projektu SQLite. Všimněte si, že v tabulce databáze SQLite je definována přidáním atributy třídy ( `Stock` třída) místo příkazu CREATE TABLE.

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

Pomocí `[Table]` atribut bez zadání způsobí, že parametr název tabulky základní tabulky databáze do mají stejný název jako třída (v tomto případě "Stock"). Skutečný název tabulky je důležité, pokud budete psát dotazy SQL přímo s databází místo použití metody ORM datového přístupu. Podobně jako `[Column("_id")]` atribut je volitelný a pokud chybějící sloupec bude přidán do tabulky se stejným názvem jako vlastnost v třídě.

## <a name="sqlite-attributes"></a>Atributy SQLite

Běžné atributy, které lze použít k řízení, jak jsou uloženy v databázi základní třídy patří:

-   **[PrimaryKey]**  &ndash; Tento atribut lze použít k ve vlastnosti integer. Chcete-li vynutit, aby se primární klíč základní tabulky. Složené primární klíče nejsou podporovány.

-   **[AutoIncrement]**  &ndash; Způsobí, že tento atribut hodnotu ve vlastnosti integer na automatického přírůstku pro každý nový objekt vložit do databáze

-   **[Column(name)]**  &ndash; Zadávání nepovinný `name` parametr přepíše výchozí hodnotu názvu základní sloupce databáze, (což je stejný jako vlastnost).

-   **[Table(name)]**  &ndash; Označí třída jako mohou být uloženy v podkladové tabulce SQLite. Zadáním parametru nepovinný název přepíše výchozí hodnoty základní tabulky databáze název (což je stejný jako název třídy).

-   **[MaxLength(value)]**  &ndash; Omezit délku textu vlastnosti, když dojde k pokusu o vložení databáze. Použití kódu by to ověřit před vložení objektu, protože tento atribut je pouze 'zaškrtnout' při vložení databáze nebo dojde k pokusu o operaci aktualizace.

-   **[Ignorovat]**  &ndash; SQLite.NET způsobí, že tato vlastnost ignorovat.
    To je obzvláště užitečná pro vlastnosti, které mají typ, který nelze uložit do databáze a vlastnosti, zda je model kolekce, které nelze vyřešit automaticky SQLite.

-   **[Jedinečné]**  &ndash; Zajišťuje, aby byly jedinečné hodnoty ve sloupci základní databáze.


Většina těchto atributů je volitelná, SQLite použije výchozí hodnoty pro názvy tabulek a sloupců. Primární klíč celé číslo musíte vždycky zadat tak, že výběr a mazání dotazů můžete provedeny efektivně na vaše data.

## <a name="more-complex-queries"></a>Složitější dotazy

Následující metody na `SQLiteConnection` slouží k provádění jiných operací s daty:

-   **Vložit** &ndash; přidá nový objekt do databáze.

-   **Získat&lt;T&gt;**  &ndash; se pokusí načíst objekt, který používá primární klíč.

-   **Tabulka&lt;T&gt;**  &ndash; vrátí všechny objekty v tabulce.

-   **Odstranit** &ndash; odstraní objekt, který používá jeho primární klíč.

-   **Dotaz&lt;T&gt;**  &ndash; provést dotaz SQL, která vrátí počet řádků (jako objekty).

-   **Spuštění** &ndash; tuto metodu použijte, (a ne `Query`) Pokud neočekáváte řádky zpět z SQL (jako je například vložení, aktualizaci a odstranění pokyny).


### <a name="getting-an-object-by-the-primary-key"></a>Získávání objektu podle primárního klíče

SQLite.Net poskytuje metodu Get pro načtení jednoho objektu na základě jeho primárního klíče.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Vyberte objekt, který používá Linq

Metody, které vrací kolekce podporují `IEnumerable<T>` tak můžete Linq dotazů nebo řazení obsahu tabulky. Následující kód ukazuje příklad použití Linq filtrovat všechny položky, začínající písmenem "A":

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>Vyberte objekt, který používá SQL

I když SQLite.Net můžete zadat na základě objektů přístup k datům, v některých případech může musíte udělat komplexnější dotaz, než umožňuje Linq (nebo může být nutné vyšší výkon). U metody dotazu, můžete použít příkazy SQL, jak je vidět tady:

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!NOTE]
> Při psaní příkazů SQL přímo vytvoření závislosti na názvy tabulek a sloupců v databázi, která byla vygenerována z vaší třídy a jejich atributy. Pokud změníte tyto názvy ve vašem kódu nezapomeňte aktualizovat všechny ručně psaného příkazů SQL.

### <a name="deleting-an-object"></a>Odstranění objektu

Primární klíč se používá k odstranění řádku, jak je vidět tady:

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

Můžete zkontrolovat `rowcount` k potvrzení, kolik řádků situace měla vliv na (v tomto případě odstraněny).

## <a name="using-sqlitenet-with-multiple-threads"></a>Pomocí SQLite.NET s více vlákny

SQLite podporuje tři různé režimy vláken: *jedním vláknem*, *více vláken*, a *serializovaných*. Pokud chcete pro přístup k databázi z více vláken bez jakýchkoli omezení, můžete nakonfigurovat SQLite používat **serializovaných** dělení na vlákna režimu. Je důležité nastavit tento režim již v rané fázi v aplikaci (například na začátku `OnCreate` metoda).

Chcete-li změnit režim vláken, volejte `SqliteConnection.SetConfig`. Například tento řádek kódu nakonfiguruje SQLite pro **serializovaných** režimu: 

```csharp
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

Android verze SQLite má omezení, která vyžaduje několik další kroky. Pokud volání `SqliteConnection.SetConfig` například vytvoří výjimka SQLite `library used incorrectly`, pak musíte použít následující alternativní řešení: 

1.  Odkaz na nativního **libsqlite.so** knihovny tak, aby `sqlite3_shutdown` a `sqlite3_initialize` rozhraní API jsou k dispozici pro aplikaci:

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```


2.  Od samého začátku z `OnCreate` metoda, tento kód vložte do vypnutí SQLite, nakonfigurovat ji pro **serializovaných** režim a znovu inicializovat SQLite:

    ```csharp
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

Toto řešení funguje i pro `Mono.Data.Sqlite` knihovny. Další informace o SQLite a více vláken, najdete v části [SQLite a více vláken](https://www.sqlite.org/threadsafe.html). 



## <a name="related-links"></a>Související odkazy

- [Přístup Basic (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Rozšířené přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recepty dat v androidu](https://developer.xamarin.com/recipes/android/data/)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
