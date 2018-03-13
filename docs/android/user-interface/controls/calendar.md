---
title: "Kalendář"
ms.topic: article
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 9b744f4c8582aa9295645b2bdc22e6fddf2bedc3
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="calendar"></a>Kalendář


## <a name="calendar-api"></a>Kalendář rozhraní API

Novou sadu kalendáře rozhraní API byla zavedená v systému Android 4 podporuje aplikace, které jsou určeny pro čtení nebo zápisu dat do kalendáře zprostředkovatele. Tato rozhraní API podporují širokou řadu možností interakce s kalendářní data, včetně možnost číst a zapisovat události, účastníci a připomenutí. Pomocí zprostředkovatele kalendáře v aplikaci se zobrazí data, která přidáte prostřednictvím rozhraní API v předdefinované kalendáře aplikaci, která se dodává se systémem Android 4.


## <a name="adding-permissions"></a>Přidání oprávnění

Při práci s kalendáři nové rozhraní API v aplikaci, je první věcí, kterou je potřeba udělat, přidejte příslušná oprávnění k manifestu systému Android. Je nutné přidat oprávnění jsou `android.permisson.READ_CALENDAR` a `android.permission.WRITE_CALENDAR`, v závislosti na tom, jestli jsou čtení a zápis dat kalendáře.


## <a name="using-the-calendar-contract"></a>Použití kontraktů kalendáře

Jakmile jednou nastavíte oprávnění, můžete pracovat s kalendářních dat pomocí `CalendarContract` třídy. Tato třída poskytuje datový model, který aplikace můžete použít, když komunikují s poskytovateli kalendáře. `CalendarContract` Umožňuje aplikacím přeložit identifikátory URI na kalendáře entitami, jako je například kalendáře a událostí. Také poskytuje způsob, jak pracovat s různých polí v každé entity, jako je například název a ID, nebo události počáteční a koncové datum v kalendáři.

Podívejme se na příklad, který používá rozhraní API kalendáře. V tomto příkladu se podíváme, jak vytvořit výčet kalendáře a jejich událostí, jakož i postup přidání nové události do kalendáře.


## <a name="listing-calendars"></a>Výpis kalendáře

Nejprve Podívejme se na tom, jak vytvořit výčet kalendářích, které byly zapsány do kalendáře aplikace. K tomuto účelu jsme doložit `CursorLoader`. Byla zavedená v systému Android 3.0 (11 rozhraní API), `CursorLoader` je upřednostňovaný způsob, jak využívat `ContentProvider`. Minimálně budeme muset zadat obsah identifikátoru Uri pro kalendáře a sloupce, které chcete vrátit; Tento sloupec specifikace se označuje jako _projekce_.

Volání `CursorLoader.LoadInBackground` metoda nám umožní dotazovat poskytovateli obsahu pro data, jako je například poskytovatel kalendáře.
`LoadInBackground` provede operaci aktuální zatížení a vrátí `Cursor` s výsledky dotazu.

`CalendarContract` Nám pomáhá zadání obou obsah `Uri` a projekce. K získání obsahu `Uri` pro dotazování kalendáře, můžeme jednoduše použít `CalendarContract.Calendars.ContentUri` vlastnost takto:

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

Pomocí `CalendarContract` k určení kalendář, který je stejně jednoduché chceme sloupce. Právě přidáme pole v `CalendarContract.Calendars.InterfaceConsts` třídy na pole. Například následující kód obsahuje ID kalendáře, zobrazované jméno a název účtu:

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

`Id` Je důležité zahrnout, pokud používáte `SimpleCursorAdapter` k vytvoření vazby dat uživatelského rozhraní, protože jsme se krátce zobrazí. Obsah identifikátoru Uri a projekce na místě, můžeme vytvořit instanci `CursorLoader` a volání `CursorLoader.LoadInBackground` metoda vrátí kurzor s kalendářní data, jak je uvedeno níže:

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

Uživatelské rozhraní pro tento příklad obsahuje `ListView`, s každou položku v seznamu představující jednu kalendáře. Následující kód XML ukazuje kód, který obsahuje `ListView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">
  <ListView
    android:id="@android:id/android:list"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
</LinearLayout>
```

Také je potřeba zadat uživatelského rozhraní pro každou položku seznamu, který jsme umístit do samostatného souboru XML následujícím způsobem:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="wrap_content">
  <TextView android:id="@+id/calDisplayName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="16dip" />
  <TextView android:id="@+id/calAccountName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="12dip" />
</LinearLayout>
```

Od tohoto okamžiku je právě normální kódu pro systém Android k vytvoření vazby dat z kurzor uživatelského rozhraní. Použijeme `SimpleCursorAdapter` následujícím způsobem:

```csharp
string[] sourceColumns = {
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName };

int[] targetResources = {
    Resource.Id.calDisplayName, Resource.Id.calAccountName };      

SimpleCursorAdapter adapter = new SimpleCursorAdapter (this,
    Resource.Layout.CalListItem, cursor, sourceColumns, targetResources);

ListAdapter = adapter;
```

Ve výše uvedeném kódu adaptér trvá sloupce zadané ve `sourceColumns` pole a zapisuje je do prvky uživatelského rozhraní v `targetResources` pole pro každou položku kalendáře v kurzor. Aktivity použít zde je podtřídou třídy `ListActivity`; obsahuje `ListAdapter` vlastnost, ke které jsme nastavte adaptér.

Zde je snímek obrazovky zobrazující konečný výsledek s informace o kalendáři zobrazí v `ListView`:

[![CalendarDemo spuštěna v emulátoru, zobrazení dvě položky kalendáře](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)



## <a name="listing-calendar-events"></a>Výpis kalendář události

Další podíváme, jak vytvořit výčet události pro danou kalendáře.
Sestavování po výše uvedeném příkladu, jsme budete k dispozici seznam událostí, když uživatel vybere jeden z kalendáře. Proto budeme potřebovat zpracování výběr položek v předchozí kód:

```csharp
ListView.ItemClick += (sender, e) => {
    int i = (e as ItemEventArgs).Position;

    cursor.MoveToPosition(i);
    int calId =
        cursor.GetInt (cursor.GetColumnIndex (calendarsProjection [0]));

    var showEvents = new Intent(this, typeof(EventListActivity));
    showEvents.PutExtra("calId", calId);
    StartActivity(showEvents);
};
```

V tomto kódu vytváříme záměrem otevření aktivitu typu `EventListActivity`, předávání ID v kalendáři v záměr. Potřebujeme ID znáte kalendář, který k dotazu pro události. V `EventListActivity`na `OnCreate` metoda, můžeme načíst ID z `Intent` jak je uvedeno níže:

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

Nyní Pojďme událostí dotazu pro tento kalendář ID. Proces dotazu pro události je podobná způsobu, jakým jsme dotaz seznam kalendáře dříve, pouze v tomto případě jsme budete pracovat `CalendarContract.Events` třídy. Následující kód vytvoří dotaz pro načtení události:

```csharp
var eventsUri = CalendarContract.Events.ContentUri;

string[] eventsProjection = {
    CalendarContract.Events.InterfaceConsts.Id,
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart
};

var loader = new CursorLoader(this, eventsUri, eventsProjection,
                   String.Format ("calendar_id={0}", _calId), null, "dtstart ASC");
var cursor = (ICursor)loader.LoadInBackground();
```

V tomto kódu nám nejdřív získat obsah `Uri` pro události z `CalendarContract.Events.ContentUri` vlastnost. Potom jsme určit sloupce události, které chcete načíst v poli eventsProjection.
Nakonec jsme doložit `CursorLoader` s tímto informace a volání zavaděč na `LoadInBackground` metoda vrátí `Cursor` se data události.

Pokud chcete zobrazit data události v uživatelském rozhraní, jsme pomocí značek a kódu stejně to jsme předtím zobrazíte seznam kalendáře. Znovu používáme `SimpleCursorAdapter` k vytvoření vazby dat `ListView` jak je znázorněno v následujícím kódu:

```csharp
string[] sourceColumns = {
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart };

int[] targetResources = {
    Resource.Id.eventTitle,
    Resource.Id.eventStartDate };

var adapter = new SimpleCursorAdapter (this, Resource.Layout.EventListItem,
    cursor, sourceColumns, targetResources);

adapter.ViewBinder = new ViewBinder ();       
ListAdapter = adapter;
```

Hlavní rozdíl mezi tento kód a kód, který jsme použili dříve k zobrazení seznamu kalendář je použití `ViewBinder`, který je nastaven na řádku:

```csharp
adapter.ViewBinder = new ViewBinder ();
```

`ViewBinder` Třída umožňuje další řídit, jak jsme svázat hodnoty pro zobrazení. V tomto případě používáme ji převést čas zahájení událostí z milisekund na řetězec data, jak je znázorněno v následujícím implementace:

```csharp
class ViewBinder : Java.Lang.Object, SimpleCursorAdapter.IViewBinder
{    
    public bool SetViewValue (View view, Android.Database.ICursor cursor,
        int columnIndex)
    {
        if (columnIndex == 2) {
            long ms = cursor.GetLong (columnIndex);

            DateTime date = new DateTime (1970, 1, 1, 0, 0, 0,
                DateTimeKind.Utc).AddMilliseconds (ms).ToLocalTime ();

            TextView textView = (TextView)view;
            textView.Text = date.ToLongDateString ();

            return true;
        }
        return false;
    }    
}
```

Zobrazí seznam událostí, jak je uvedeno níže:

[![Snímek obrazovky aplikace příklad zobrazení tři události kalendáře](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)



## <a name="adding-a-calendar-event"></a>Přidání události kalendáře

Jsme viděli informace o načtení dat kalendáře. Nyní Podíváme se, jak přidat události do kalendáře. Tento postup, nezapomeňte zahrnout `android.permission.WRITE_CALENDAR` oprávnění jsme už zmínili dřív. Postup přidání události do kalendáře, provedeme následující:

1.  Vytvoření `ContentValues` instance.
1.  Použití klíče ze `CalendarContract.Events.InterfaceConsts` třída k naplnění `ContentValues` instance.
1.  Nastavení časových pásem pro události spuštění a ukončení.
1.  Použití `ContentResolver` vložení data události do kalendáře.


Následující kód ukazuje tyto kroky:

```csharp
ContentValues eventValues = new ContentValues ();

eventValues.Put (CalendarContract.Events.InterfaceConsts.CalendarId,
    _calId);
eventValues.Put (CalendarContract.Events.InterfaceConsts.Title,
    "Test Event from M4A");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Description,
    "This is an event created from Xamarin.Android");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtstart,
    GetDateTimeMS (2011, 12, 15, 10, 0));
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtend,
    GetDateTimeMS (2011, 12, 15, 11, 0));

eventValues.Put(CalendarContract.Events.InterfaceConsts.EventTimezone,
    "UTC");
eventValues.Put(CalendarContract.Events.InterfaceConsts.EventEndTimezone,
    "UTC");

var uri = ContentResolver.Insert (CalendarContract.Events.ContentUri,
    eventValues);
```

Všimněte si, že pokud jsme nenastavujte časové pásmo, k výjimce typu `Java.Lang.IllegalArgumentException` bude vyvolána. Protože hodnoty čas události musí být vyjádřena v milisekundách od epoch, vytvoříme `GetDateTimeMS` – metoda (v `EventListActivity`) naše specifikace datum převést do formátu milisekund:

```csharp
long GetDateTimeMS (int yr, int month, int day, int hr, int min)
{
    Calendar c = Calendar.GetInstance (Java.Util.TimeZone.Default);

    c.Set (Java.Util.CalendarField.DayOfMonth, 15);
    c.Set (Java.Util.CalendarField.HourOfDay, hr);
    c.Set (Java.Util.CalendarField.Minute, min);
    c.Set (Java.Util.CalendarField.Month, Calendar.December);
    c.Set (Java.Util.CalendarField.Year, 2011);

    return c.TimeInMillis;
}
```

Pokud jsme přidání tlačítka do seznamu těchto událostí uživatelského rozhraní a spustit ve výše uvedeném kódu na tlačítku klikněte na obslužnou rutinu události, události je přidat do kalendáře a aktualizovat na našem seznamu, jak je uvedeno níže:

[![Snímek obrazovky aplikace příklad s událostmi kalendáře následuje událost vzorku pro přidání tlačítka](calendar-images/13.png)](calendar-images/13.png#lightbox)

Pokud jsme otevřete aplikaci kalendář, pak zjistíme, zda je událost zapsána existuje také:

[![Snímek obrazovky aplikace kalendáře zobrazení událostí vybraného kalendáře](calendar-images/14.png)](calendar-images/14.png#lightbox)

Jak vidíte, Android umožní efektivní a snadný přístup k načtení a zachovat kalendářní data, aplikace bez problémů integrují schopnosti kalendáře.


## <a name="related-links"></a>Související odkazy

- [Ukázkový kalendáře (ukázka)](https://developer.xamarin.com/samples/CalendarDemo/)
- [Představení Sandwichovy Zmrzlinová](http://www.android.com/about/ice-cream-sandwich/)
- [Platformu Android 4.0](http://developer.android.com/sdk/android-4.0.html)
