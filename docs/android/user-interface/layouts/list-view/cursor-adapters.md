---
title: "Pomocí CursorAdapters"
ms.topic: article
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: 5cadaf5f41d940a0255113178d018b59b780eabc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="using-cursoradapters"></a>Pomocí CursorAdapters


## <a name="overview"></a>Přehled

Android poskytuje třídy adaptér speciálně pro zobrazení dat z dotazu databáze SQLite:

 **SimpleCursorAdapter** – podobně jako do `ArrayAdapter` protože je možné použít bez vytváření podtříd. V konstruktoru stačí zadat požadované parametry (například informace o kurzoru a rozložení) a pak přiřadit `ListView`.

 **CursorAdapter** – základní třídu, která může dědit vlastnosti z když budete potřebovat větší kontrolu nad vázání dat hodnoty k rozložení ovládacích prvků (například ovládací prvky skrytí nebo zobrazení nebo změna jejich vlastnosti).

Kurzor adaptérů umožňují vysoce výkonné nemuseli probírat dlouhými seznamy data, která jsou uložená v SQLite. Kód náročné musí definovat dotazu SQL v `Cursor` objekt a potom popisují, jak vytvořit a naplnit zobrazení pro každý řádek.


## <a name="creating-an-sqlite-database"></a>Vytváření databázi SQLite

K předvedení kurzoru adaptéry vyžaduje jednoduché implementace databáze SQLite. Kód v **SimpleCursorTableAdapter/VegetableDatabase.cs** obsahuje kód a SQL vytvořte tabulku a naplnit určitými daty.
Kompletní `VegetableDatabase` třída je znázorněno zde:

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
   public static readonly string create_table_sql =
       "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
   public static readonly string DatabaseName = "vegetables.db";
   public static readonly int DatabaseVersion = 1;
   public VegetableDatabase(Context context) : base(context, DatabaseName, null, DatabaseVersion) { }
   public override void OnCreate(SQLiteDatabase db)
   {
       db.ExecSQL(create_table_sql);
       // seed with data
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Vegetables')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Fruits')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Flower Buds')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Legumes')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Bulbs')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Tubers')");
   }
   public override void OnUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
   {   // not required until second version :)
       throw new NotImplementedException();
   }
}
```

`VegetableDatabase` Se vytvořit instanci třídy v `OnCreate` metodu `HomeScreen` aktivity. `SQLiteOpenHelper` Základní třída spravuje instalačním souboru databáze a zajišťuje, že SQL v jeho `OnCreate` metoda je spuštěn pouze jednou. Tato třída se používá v následujících dvou příkladech pro `SimpleCursorAdapter` a `CursorAdapter`.

Dotaz kurzor *musí* mít sloupec celé číslo `_id` pro `CursorAdapter` pracovat. Pokud základní tabulka neobsahuje sloupec celé číslo s názvem `_id` pak použít alias sloupce pro jiný jedinečný celé číslo v `RawQuery` , která tvoří kurzor. Odkazovat [dokumenty Android](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/) Další informace.


### <a name="creating-the-cursor"></a>Vytvoření kurzoru

Příklady používají `RawQuery` zapnout příkazu jazyka SQL do `Cursor` objektu. Seznam sloupců, vrácená kurzor definuje sloupce dat, které jsou k dispozici pro zobrazení v adaptéru kurzoru. Kód, který vytvoří databázi **SimpleCursorTableAdapter/HomeScreen.cs** `OnCreate` metoda je znázorněno zde:

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

Kód, který volá `StartManagingCursor` měli také zavolat `StopManagingCursor`. Příklady používají `OnCreate` chcete spustit, a `OnDestroy` zavřete kurzor. `OnDestroy` Metoda obsahuje tento kód:

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

Jakmile aplikace má k dispozici databáze SQLite a vytvořil objekt kurzoru, jak je znázorněno, ho můžete využít buď `SimpleCursorAdapter` nebo podtřídou třídy `CusorAdapter` zobrazíte řádků v `ListView`.


## <a name="using-simplecursoradapter"></a>Pomocí SimpleCursorAdapter

`SimpleCursorAdapter` je třeba `ArrayAdapter`, ale specializované pro použití s SQLite. Ji nevyžadují vytváření podtříd – stačí nastavit některé jednoduché parametry při vytváření objektu a potom je přiřadit `ListView`na `Adapter` vlastnost.

Parametry pro konstruktor SimpleCursorAdapter jsou:

 **Kontext** – odkaz na obsahující aktivitu.

 **Rozložení** – ID prostředku řádek zobrazení použít.

 **ICursor** – kurzoru obsahující SQLite dotaz na data k zobrazení.

 **Z** pole řetězců – pole řetězců odpovídající názvy sloupců v kurzor.

 **K** celé číslo pole – pole ID rozložení, které odpovídají k ovládacím prvkům v rozložení řádků. Hodnota sloupce zadané v `from` pole bude vázán k ControlID zadaný v toto pole ve stejném indexu.

`from` a `to` pole musí mít stejný počet položek, protože tvoří mapování ze zdroje dat k ovládacím prvkům rozložení v zobrazení.

**SimpleCursorTableAdapter/HomeScreen.cs** ukázkový kód vodičům nahoru `SimpleCursorAdapter` podobné výjimky:

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` je rychlý a jednoduchý způsob, jak zobrazit SQLite data `ListView`. Hlavní omezení je, že ho můžete vázat pouze hodnoty ve sloupcích zobrazíte ovládací prvky, neumožňuje měnit dalších aspektů rozložení řádků (například zobrazení nebo skrytí ovládacích prvků nebo změna vlastností).


## <a name="subclassing-cursoradapter"></a>Vytvoření podtřídy CursorAdapter

A `CursorAdapter` podtřídami má stejné výkonnostních výhod, jako `SimpleCursorAdapter` pro zobrazení dat z SQLite, ale také poskytuje úplnou kontrolu nad vytváření a rozložení jednotlivých řádků zobrazení. `CursorAdapter` Implementace je příliš neliší od vytváření podtříd `BaseAdapter` protože nepřepisuje `GetView`, `GetItemId`, `Count` nebo `this[]` indexer.

Zadané funkční databáze SQLite, potřebujete jenom k přepsání dvě metody vytvoření `CursorAdapter` podtřídami:

- **Společnosti BindView** – zadané zobrazení, aktualizujte zobrazení dat v zadané kurzor.

- **NewView** – voláno, když `ListView` vyžaduje nové zobrazení pro zobrazení. `CursorAdapter` Se postará o recyklace zobrazení (na rozdíl od `GetView` metoda regulární síťové adaptéry).

Podtřídy adaptér v předchozích příkladech mají metody, které se chcete vrátit počet řádků a k získání aktuální položky – `CursorAdapter` nevyžaduje těchto metod, protože tyto informace můžete shromažďovat informace ze samotné kurzor. Rozdělením vytvoření a naplnění jednotlivých zobrazení v těchto dvou metod `CursorAdapter` vynucuje zobrazení znovu použít. Jde na rozdíl od regulární adaptéru kde je možné ignorovat `convertView` parametr `BaseAdapter.GetView` metoda.


### <a name="implementing-the-cursoradapter"></a>Implementace CursorAdapter

Kód v **CursorTableAdapter/HomeScreenCursorAdapter.cs** obsahuje `CursorAdapter` podtřídy. Ukládají se zde odkaz kontextu předaný do konstruktoru, aby měl přístup `LayoutInflater` v `NewView` metoda. Dokončení třídy vypadá takto:

```csharp
public class HomeScreenCursorAdapter : CursorAdapter {
   Activity context;
   public HomeScreenCursorAdapter(Activity context, ICursor c)
       : base(context, c)
   {
       this.context = context;
   }
   public override void BindView(View view, Context context, ICursor cursor)
   {
       var textView = view.FindViewById<TextView>(Android.Resource.Id.Text1);
       textView.Text = cursor.GetString(1); // 'name' is column 1 in the cursor query
   }
   public override View NewView(Context context, ICursor cursor, ViewGroup parent)
   {
       return this.context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, parent, false);
   }
}
```


### <a name="assigning-the-cursoradapter"></a>Přiřazení CursorAdapter

V `Activity` , se zobrazí `ListView`, vytvořit kurzor a `CursorAdapter` přiřaďte ho k zobrazení seznamu.

Kód, který provádí tuto akci v **CursorTableAdapter/HomeScreen.cs** `OnCreate` metoda je znázorněno zde:

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

`OnDestroy` Metoda obsahuje `StopManagingCursor` volání metody popsané.



## <a name="related-links"></a>Související odkazy

- [SimpleCursorTableAdapter (ukázka)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (ukázka)](https://developer.xamarin.com/samples/CursorTableAdapter/)
