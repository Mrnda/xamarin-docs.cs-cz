---
title: "Pomocí SQLite.NET"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/18/2018
ms.openlocfilehash: d18fe5960a44153626fbf0bda30e3485faf5b9fe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="using-sqlitenet"></a>Pomocí SQLite.NET

SQLite.NET knihovny, která doporučuje Xamarin je základní ORM, který umožňuje ukládání a načítání objektů v místní databázi SQLite na zařízení s iOS.
ORM je zkratka pro objekt relační mapování – rozhraní API, které vám umožní uložit a načíst "objekty" z databáze bez nutnosti psaní příkazů SQL.

## <a name="using-sqlitenet"></a>Pomocí SQLite.NET

Přidat [balíček SQLite.net PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/), do projektu - podporuje mnoha různých platforem včetně iOS, Android a Windows.

  [ ![](using-sqlite-orm-images/image1a-sml.png "Balíček SQLite.NET NuGet")](using-sqlite-orm-images/image1a.png)

Až budete mít k dispozici SQLite.NET knihovny, postupujte podle tyto tři kroky ji používat pro přístup k databázi:


1. **Přidat pomocí příkazu** -do kdy je potřeba přístup k datům soubory C# přidejte následující příkaz:

        using SQLite;

1. **Vytvořit prázdnou databázi** – odkaz na databázi lze vytvořit pomocí předání konstruktoru třídy SQLiteConnection cesta k souboru. Nemusíte zaškrtněte, pokud soubor již existuje – se automaticky vytvoří Pokud požadováno, v opačném případě stávající soubor databáze se otevřít.

        var db = new SQLiteConnection (dbPath);

    Je třeba určit proměnnou dbPath podle pravidla popsané dříve v tomto dokumentu.

1. **Uložení dat** – po vytvoření objektu SQLiteConnection, databáze, které příkazy budou provedeny voláním její metody, jako je například CreateTable a vložení takto:

        db.CreateTable<Stock> ();
        db.Insert (newStock); // after creating the newStock object

1. **Načtení dat** – Pokud chcete načíst objekt (nebo seznam objektů) použijte následující syntaxi:

        var stock = db.Get<Stock>(5); // primary key id of 5
        var stockList = db.Table<Stock>();

## <a name="basic-data-access-sample"></a>Základní Data Access – ukázka

*DataAccess_Basic* ukázkový kód pro tento dokument vypadat třeba takto při spuštění v systému iOS. Kód ukazuje, jak provádět jednoduché operace SQLite.NET a zobrazí výsledky v jako text v hlavním okně aplikace.

**iOS**

 ![](using-sqlite-orm-images/image2.png "Ukázka SQLite.NET iOS")

Následující příklad kódu ukazuje interakce celou databázi pomocí knihovny SQLite.NET k zapouzdření základní přístup k databázi. Zobrazuje:

1.  Vytvoření souboru databáze
1.  Vytváření objektů a jejich uložením vkládání některá data
1.  Dotazování na data


Budete muset zahrnují tyto obory názvů:

```csharp
using SQLite; // from the github SQLite.cs class
```

To vyžaduje, že jste přidali do projektu, jak je znázorněno SQLite [zde](#Using_SQLite.NET). Všimněte si, že v tabulce databáze SQLite je definována přidáním atributy třídy ( `Stock` třída) místo příkazu CREATE TABLE.

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

-  **[PrimaryKey]**  – Tento atribut lze použít k ve vlastnosti integer. Chcete-li vynutit, aby se primární klíč základní tabulky. Složené primární klíče nejsou podporovány.
-  **[AutoIncrement]**  – Způsobí, že tento atribut hodnotu ve vlastnosti integer na automatického přírůstku pro každý nový objekt vložit do databáze
-  **[Column(name)]**  – Poskytuje nepovinný `name` parametr přepíše výchozí hodnotu názvu základní sloupce databáze, (což je stejný jako vlastnost).
-  **[Table(name)]**  – Označí třída jako mohou být uloženy v podkladové tabulce SQLite. Zadáním parametru nepovinný název přepíše výchozí hodnoty základní tabulky databáze název (což je stejný jako název třídy).
-  **[MaxLength(value)]**  – Omezit délku textu vlastnosti, když dojde k pokusu o vložení databáze. Použití kódu by to ověřit před vložení objektu, protože tento atribut je pouze 'zaškrtnout' při vložení databáze nebo dojde k pokusu o operaci aktualizace.
-  **[Ignorovat]**  – Způsobí, že SQLite.NET ignorovat tuto vlastnost. To je obzvláště užitečná pro vlastnosti, které mají typ, který nelze uložit do databáze a vlastnosti, zda je model kolekce, které nelze vyřešit automaticky SQLite.
-  **[Jedinečné]**  – Zajišťuje, aby byly jedinečné hodnoty ve sloupci základní databáze.


Většina těchto atributů je volitelná, SQLite použije výchozí hodnoty pro názvy tabulek a sloupců. Primární klíč celé číslo musíte vždycky zadat tak, že výběr a mazání dotazů můžete provedeny efektivně na vaše data.

## <a name="more-complex-queries"></a>Složitější dotazy

Následující metody na `SQLiteConnection` slouží k provádění jiných operací s daty:

-  **Vložit** – přidá nový objekt do databáze.
-  **Získat<T>**  – se pokusí načíst objekt, který používá primární klíč.
-  **Tabulka<T>**  – vrátí všechny objekty v tabulce.
-  **Odstranit** – odstraní objekt, který používá jeho primární klíč.
-  **Dotaz<T>**  -provést dotaz SQL, která vrátí počet řádků (jako objekty).
-  **Provést** – pomocí této metody (a ne `Query` ) Pokud neočekáváte řádky zpět z SQL (jako je například vložení, aktualizaci a odstranění pokyny).


### <a name="getting-an-object-by-the-primary-key"></a>Získávání objektu podle primárního klíče

SQLite.Net poskytuje metodu Get pro načtení jednoho objektu na základě jeho primárního klíče.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Vyberte objekt, který používá Linq

Metody, které vrací kolekce podporují rozhraní IEnumerable<T> tak můžete Linq dotazů nebo řazení obsahu tabulky. Následující kód ukazuje příklad použití Linq filtrovat všechny položky, začínající písmenem "A":

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

> [!IMPORTANT]
> **Poznámka:**: při psaní příkazů SQL přímo vytvoření závislosti na názvy tabulek a sloupců v databázi, která byla vygenerována z vaší třídy a jejich atributy. Pokud změníte tyto názvy ve vašem kódu nezapomeňte aktualizovat všechny ručně psaného příkazů SQL.

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


## <a name="related-links"></a>Související odkazy

- [Přístup Basic (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Rozšířené přístup (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recepty dat](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Přístup k datům Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
