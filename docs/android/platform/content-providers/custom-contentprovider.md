---
title: "Vytváření vlastních ContentProvider"
description: "V předchozí části ukázal, jak využívat data ze předdefinované ContentProvider implementace. Tato část vysvětluje, jak vytvořit vlastní ContentProvider a pak využívat jeho data."
ms.topic: article
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: 66b956eddc48699c6fd61e9cb52a7fbc3fa70a51
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-custom-contentprovider"></a>Vytváření vlastních ContentProvider

_V předchozí části ukázal, jak využívat data ze předdefinované ContentProvider implementace. Tato část vysvětluje, jak vytvořit vlastní ContentProvider a pak využívat jeho data._

## <a name="about-contentproviders"></a>O ContentProviders

Třída poskytovateli obsahu musí dědit z `ContentProvider`. By měla obsahovat interní úložiště dat sloužící k odpovídání na dotazy a jako konstanty pomohou využívání kód zajistěte platné požadavky na data, by měli vystavit identifikátory URI a typy MIME.

### <a name="uri-authority"></a>Identifikátor URI (autorita)

`ContentProviders` jsou přístupné v Android pomocí identifikátoru Uri. Aplikace, která zveřejňuje `ContentProvider` nastaví identifikátory URI, který bude odpovídat v jeho **AndroidManifest.xml** souboru. Pokud je nainstalovaná aplikace, jsou tyto identifikátory URI registrované, aby ostatní aplikace přístup.

Mono pro Android, by měl mít třídě poskytovateli obsahu `[ContentProvider]` atributu zadejte identifikátor Uri (nebo identifikátory URI) musí být přidaní do **AndroidManifest.xml**.

<a name="Mime_Type" />

### <a name="mime-type"></a>Typ MIME

Typický formát pro typy MIME se skládá ze dvou částí. Android `ContentProviders` běžně používají tyto dva řetězce pro první část typ MIME:

1. `vnd.android.cursor.item` &ndash; Chcete-li představují jeden řádek, použijte `ContentResolver.CursorItemBaseType` konstantní v kódu.

1. `vnd.android.cursor.dir` &ndash; pro více řádků, použijte `ContentResolver.CursorDirBaseType` konstantní v kódu.

Druhá část typ MIME je specifický pro vaši aplikaci a měli používat standardní zpětného DNS s `vnd.` předponu. Ukázkový kód používá `vnd.com.xamarin.sample.Vegetables`.

<a name="Data_Model_Metadata" />

### <a name="data-model-metadata"></a>Datový Model metadat

Přijímajícím aplikacím muset vytvořit identifikátor Uri dotazy pro přístup k různé typy dat. Základní identifikátor Uri lze rozšířit odkazovat na konkrétní tabulku dat a může obsahovat parametry pro filtrování výsledků. Také musí být deklarován sloupců a klauzule použít k zobrazení dat s výsledné kurzor.

Aby se zajistilo, že jsou pouze dotazy na platný identifikátor Uri vytvořený, se obvykle poskytují platné řetězce jako konstantní hodnoty. To usnadňuje přístup `ContentProvider` protože díky hodnoty zjistitelný prostřednictvím doplňování kódu který brání překlepům v řetězcích.

V předchozím příkladu `android.provider.ContactsContract` třída vystavena metadata pro kontakty data. Pro naše vlastní `ContentProvider` zveřejňujeme bude pouze konstanty na vlastní třídy.

<a name="Implementation" />

## <a name="implementation"></a>Implementace

Existují tři kroky k vytvoření a použití vlastní `ContentProvider`:

1. **Vytvořte třídu databáze** &ndash; implementace `SQLiteOpenHelper`.

2. **Vytvoření `ContentProvider` třída** &ndash; implementace `ContentProvider` s instancí databáze, metadata zveřejněné jako konstantní hodnoty a metody k přístupu k datům.

3. **Přístup `ContentProvider` pomocí jeho identifikátoru Uri** &ndash; vyplnit `CursorAdapter` pomocí `ContentProvider`, přístupu pomocí jeho identifikátoru Uri.

Dřív popsané, `ContentProviders` z aplikací, než kde jsou definovány, mohou být využívány. V tomto příkladu je využívat data ve stejné aplikaci, ale mějte na paměti, která jiné aplikace taky k němu přístup, dokud vědí, identifikátor Uri a informace o schématu (který je obvykle zveřejňují jako konstantní hodnoty).

<a name="Create_a_database" />

## <a name="create-a-database"></a>Vytvoření databáze

Většina `ContentProvider` implementace bude na základě `SQLite` databáze. Příklad kódu databáze v **SimpleContentProvider/VegetableDatabase.cs** vytvoří velmi jednoduché databázi dvěma sloupci, jak je znázorněno:

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
  const string create_table_sql =
    "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
  const string DatabaseName = "vegetables.db";
  const int DatabaseVersion = 1;

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
  {
    throw new NotImplementedException();
  }
}
```

Implementace databáze samotné nemusí žádná zvláštní opatření, které mají být exponovány s `ContentProvider`, ale pokud chcete vytvořit vazbu `ContentProvider's` dat `ListView` řízení pak celé číslo jedinečný sloupec s názvem `_id` musí být součástí Sada výsledků dotazu. Najdete v článku [ListViews a adaptéry](~/android/user-interface/layouts/list-view/index.md) dokumentu pro další podrobnosti o použití `ListView` ovládacího prvku.

<a name="Create_the_ContentProvider" />

## <a name="create-the-contentprovider"></a>Vytvořte ContentProvider

Zbytek Tato část obsahuje podrobné pokyny o jak **SimpleContentProvider/VegetableProvider.cs** Ukázka třídy byl sestaven.

<a name="Initialize_the_Database" />

### <a name="initialize-the-database"></a>Inicializace databáze

Prvním krokem je podtřídou `ContentProvider` a přidejte databáze, která se bude používat.

```csharp
public class VegetableProvider : ContentProvider 
{
    VegetableDatabase vegeDB;
    public override bool OnCreate()
    {
       vegeDB = new VegetableDatabase(Context);
        return true;
    }
}
```

Zbytek kód budou formovat implementace skutečné obsahu zprostředkovatele, který umožňuje data, která mají být zjištěny a dotaz.

<a name="Add_Metadata_for_Consumers" />


## <a name="add-metadata-for-consumers"></a>Přidání metadat pro příjemce

Existují čtyři různé typy metadat, které přidáme ke zveřejnění na `ContentProvider` třídy. Pouze oprávnění je vyžadováno, zbývající provádějí konvence.

- **Autorita** &ndash; `ContentProvider` atribut *musí* přidáni do třídy, takže ho není zaregistrována pro Android, když je aplikace nainstalována.

- **Identifikátor URI** &ndash; `CONTENT_URI` je k dispozici jako konstanta tak, aby se snadno používá v kódu. By měl odpovídat oprávnění, ale zahrnuje schéma a základní cesta.

- **Typy MIME** &ndash; seznam výsledků a jeden výsledky jsou považovány za různé typy obsahu, takže jsme definovali dva typy MIME je zastupovat.

- **InterfaceConsts** &ndash; poskytují konstantní hodnotu pro každý název sloupce dat, takže použití kódu můžete snadno vyhledat a na ně odkazují bez rizika typografické chyby.

Tento kód ukazuje, jak každá z těchto položek je implementována, přidání k definici databáze z předchozího kroku:

```csharp
[ContentProvider(new string[] { CursorTableAdapter.VegetableProvider.AUTHORITY })]
public class VegetableProvider : ContentProvider 
{
   public const string AUTHORITY = "com.xamarin.sample.VegetableProvider";
   static string BASE_PATH = "vegetables";
   public static readonly Android.Net.Uri CONTENT_URI = Android.Net.Uri.Parse("content://" + AUTHORITY + "/" + BASE_PATH);
   // MIME types used for getting a list, or a single vegetable
   public const string VEGETABLES_MIME_TYPE = ContentResolver.CursorDirBaseType + "/vnd.com.xamarin.sample.Vegetables";
   public const string VEGETABLE_MIME_TYPE = ContentResolver.CursorItemBaseType + "/vnd.com.xamarin.sample.Vegetables";
   // Column names
   public static class InterfaceConsts {
       public const string Id = "_id";
       public const string Name = "name";
   }
   VegetableDatabase vegeDB;
   public override bool OnCreate()
   {
       vegeDB = new VegetableDatabase(Context);
       return true;
   }
}
```

<a name="Implement_the_URI_Parsing_Helper" />

## <a name="implement-the-uri-parsing-helper"></a>Implementace identifikátor URI analýza pomocné rutiny

Protože využívání kód používá identifikátory URI pro požadavky `ContentProvider`, potřebujeme, abyste mohli analyzovat těchto požadavků do zjistit, jaká data vrátit. `UriMatcher` Třída může pomoci analýza identifikátory URI, jakmile byla inicializována pomocí identifikátoru Uri vzory `ContentProvider` podporuje.

`UriMatcher` v příkladu, budou inicializována dva identifikátory URI, které:

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; žádost o úplný seznam zeleniny vrátit.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; kde \# je zástupný symbol pro číselné parametr ( `_id` řádků v databázi). Zástupný znak hvězdičky ("\*") můžete také použít tak, aby odpovídaly parametr text.

V kódu odkazovat na metadata hodnoty jako AUTORITA a základní používáme konstanty\_cesta. Návratové kódy se použije v metod, které provádí analýzu, chcete-li zjistit, jaká data se vrátíte identifikátoru Uri.

```csharp
const int GET_ALL = 0; // return code when list of Vegetables requested
const int GET_ONE = 1; // return code when a single Vegetable is requested by ID
static UriMatcher uriMatcher = BuildUriMatcher();
static UriMatcher BuildUriMatcher()
{
  var matcher = new UriMatcher(UriMatcher.NoMatch);
  // Uris to match, and the code to return when matched
  matcher.AddURI(AUTHORITY, BASE_PATH, GET_ALL); // all vegetables
  matcher.AddURI(AUTHORITY, BASE_PATH + "/#", GET_ONE); // specific vegetable by numeric ID
  return matcher;
}
```

Tento kód je pro všechny privátní `ContentProvider` třídy. Odkazovat na [Google UriMatcher dokumentace](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/) Další informace.

<a name="Implement_the_QueryMethod" />

## <a name="implement-the-querymethod"></a>Implementace QueryMethod

Nejjednodušším `ContentProvider` je metoda implementovat `Query` metoda. Implementace níže používá `UriMatcher` analyzovat `uri` parametr a volejte metodu správné databázi. Pokud `uri` obsahuje parametr ID, potom si je analyzována na celé číslo (pomocí `LastPathSegment`) a použít v dotazu databáze.

```csharp
public override Android.Database.ICursor Query(Android.Net.Uri uri, string[] projection, string selection, string[] selectionArgs, string sortOrder)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return GetFromDatabase();
  case GET_ONE:
    var id = uri.LastPathSegment;
    return GetFromDatabase(id); // the ID is the last part of the Uri
  default:
    throw new Java.Lang.IllegalArgumentException("Unknown Uri: " + uri);
  }
}
Android.Database.ICursor GetFromDatabase()
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables", null);
}
Android.Database.ICursor GetFromDatabase(string id)
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables WHERE _id = " + id, null);
}
```

`GetType` Metoda musí být také přepsat. Tato metoda může být volána k určení typu obsahu, který bude vrácen pro daný identifikátor Uri.
To může informování spotřebitelskou aplikací jak pro zpracování dat.

```csharp
public override String GetType(Android.Net.Uri uri)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return VEGETABLES_MIME_TYPE; // list
  case GET_ONE:
    return VEGETABLE_MIME_TYPE; // single item
  default:
    throw new Java.Lang.IllegalArgumentExceptoin ("Unknown Uri: " + uri);
   }
}
```

<a name="Implement_the_Other_Overrides" />

## <a name="implement-the-other-overrides"></a>Implementace dalších přepsání

Naše jednoduchý příklad neumožňuje úpravy nebo odstranění dat, ale musí být implementována Insert, Update a Delete metody je proto přidejte bez implementace:

```csharp
public override int Delete(Android.Net.Uri uri, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override Android.Net.Uri Insert(Android.Net.Uri uri, ContentValues values)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override int Update(Android.Net.Uri uri, ContentValues values, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
```

Která se dokončí základní `ContentProvider` implementace. Po instalaci aplikace, budou k dispozici data zpřístupňuje jak uvnitř aplikace, ale také na všechny aplikace, které zná identifikátor Uri na něj odkazovat.

<a name="Access_the_ContentProvider" />

## <a name="access-the-contentprovider"></a>Přístup ContentProvider

Jednou `VegetableProvider` byla implementována, k ní přistupují se provádí stejným způsobem jako zprostředkovatel kontakty na začátku tohoto dokumentu: získat kurzoru pomocí zadaný identifikátor Uri a potom pomocí adaptéru pro přístup k datům.

<a name="Bind_a_ListView_to_a_ContentProvider" />

## <a name="bind-a-listview-to-a-contentprovider"></a>Vytvořit vazbu ContentProvider prvku ListView

K naplnění `ListView` s daty používáme identifikátor Uri, která odpovídá seznamu nefiltrované zeleniny. V kódu používáme konstantní hodnota `VegetableProvider.CONTENT_URI`, které jsme si vědomi přeloží na `com.xamarin.sample.vegetableprovider/vegetables`. Naše `VegetableProvider.Query` implementace vrátí kurzor, který pak může být vázána na `ListView`.

Kód v `SimpleContentProvider/HomeScreen.cs` ukáže, jak jednoduché zobrazení dat z `ContentProvider`:

```csharp
listView = FindViewById<ListView>(Resource.Id.List);
string[] projection = new string[] { VegetableProvider.InterfaceConsts.Id, VegetableProvider.InterfaceConsts.Name} ;
string[] fromColumns = new string[] { VegetableProvider.InterfaceConsts.Name };
int[] toControlIds = new int[] { Android.Resource.Id.Text1 };

// CursorLoader introduced in Honeycomb (3.0, API_11)
var loader = new CursorLoader(this,
   VegetableProvider.CONTENT_URI, projection, null, null, null);
cursor = (ICursor)loader.LoadInBackground();

// Create a SimpleCursorAdapter
adapter = new SimpleCursorAdapter(this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlIds);
listView.Adapter = adapter;
```

Výsledné aplikace vypadá takto:

[![Snímek obrazovky aplikace seznam zelenina, ovoce, poupat, luskovin, žárovky, hlízy](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png)


<a name="Retrieve_a_Single_Item_from_a_ContentProvider" />

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>Načíst jednu položku z ContentProvider

Spotřebitelskou aplikací také chtít přístup k jedné řádky dat, což lze provést vytvořením jiný identifikátor Uri, který odkazuje na konkrétní řádek (například).

Použití `ContentResolver` přímo k jednu položku, sestavením identifikátor Uri s požadovaným `Id`.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

Kompletní metoda vypadá takto:

```csharp
protected void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
  var id = e.Id;
  string[] projection = new string[] { "name" };
  var uri = Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
  ICursor vegeCursor = ContentResolver.Query(uri, projection, null, new string[] { id.ToString() }, null);
  string text = "";
  if (vegeCursor.MoveToFirst()) {
    text = vegeCursor.GetInt(0) + " " + vegeCursor.GetString(1);
    Android.Widget.Toast.MakeText(this, text, Android.Widget.ToastLength.Short).Show();
  }
  vegeCursor.Close();
}
```


## <a name="related-links"></a>Související odkazy

- [SimpleContentProvider (ukázka)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
