---
title: "Pomocí ContentProvider kontaktů"
ms.topic: article
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 730cc1f815641d79350784790e3b33b743d1aebe
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="using-the-contacts-contentprovider"></a>Pomocí ContentProvider kontaktů

Kód, že používá přístup k datům, které jsou zveřejněné `ContentProvider` nevyžaduje odkaz na `ContentProvider` třídy vůbec. Místo toho se identifikátoru Uri používá k vytvoření kurzoru nad daty vystavené `ContentProvider`. Android používá pro vyhledávání systému pro aplikaci, která zveřejňuje identifikátor Uri `ContentProvider` s tímto identifikátorem. Identifikátor Uri, jako je řetězec, obvykle ve formátu DNS zpětného `com.android.contacts/data`.

Místo provedení vývojáři mějte na paměti, tento řetězec Android *kontakty* zprostředkovatele zveřejňuje jeho metadata v `android.provider.ContactsContract` třídy. Tato třída slouží k určení identifikátor Uri `ContentProvider` a názvy tabulek a sloupců, které může být dotazován.

Některé typy dat také vyžadují speciální oprávnění k přístupu. Seznam předdefinovaných kontakty vyžaduje `android.permission.READ_CONTACTS` oprávnění v **AndroidManifest.xml** souboru.

Vytvoření kurzoru z identifikátoru Uri třemi způsoby:

1. **ManagedQuery()** &ndash; žádoucí v Android 2.3 (API úrovně 10) a starší, `ManagedQuery` vrátí kurzor a také automaticky spravuje aktualizace dat a zavření kurzor. Tato metoda je zastaralá ve Android 3.0 (11 úroveň rozhraní API).

1. **ContentResolver.Query()** &ndash; vrátí nespravované kurzor, což znamená, musí být aktualizována a ukončeno explicitně v kódu.

1. **CursorLoader(). LoadInBackground()** &ndash; byla zavedená v systému Android 3.0 (11 úroveň rozhraní API), `CursorLoader` je nyní upřednostňovaný způsob využití `ContentProvider` . `CursorLoader` dotazy `ContentResolver` na vlákna na pozadí, takže uživatelského rozhraní není blokován.
   Tato třída je přístupná ve starších verzích systému Android pomocí knihovně v4.


Každá z těchto metod má stejnou základní sadu vstupy:

-  **Identifikátor URI** &ndash; plně kvalifikovaný název `ContentProvider` .
-  **Projekce** &ndash; specifikaci sloupce, které můžete vybrat pro kurzor.
-  **Výběr** &ndash; . podobá se SQL `WHERE` klauzule.
-  **SelectionArgs** &ndash; parametry, které nahrazena ve výběru.
-  **SortOrder** &ndash; sloupce, které chcete seřadit.



## <a name="creating-inputs-for-a-query"></a>Vytváření vstupy pro vytvoření dotazu

`ContactsProvider` Ukázkový kód provede velmi jednoduchý dotaz na Android pro předdefinované zprostředkovatele kontakty. Není potřeba znát skutečné Uri nebo sloupec názvy – všechny informace požadované pro dotaz kontaktů `ContentProvider` je k dispozici jako konstanty vystavené `ContactsContract` třídy.

Bez ohledu na to, jakou metodu se používá k načtení kurzoru, tyto stejné objekty se používají jako parametry jak je znázorněno *ContactsProvider/ContactsAdapter.cs* souboru:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

V tomto příkladu `selection`, `selectionArgs` a `sortOrder` budou ignorovány jejich nastavením na `null`.



## <a name="creating-a-cursor-from-a-content-provider-uri"></a>Vytvoření kurzoru z identifikátoru Uri poskytovateli obsahu

Po vytvoření objektů parametrů lze použít v jednom z následujících tří způsobů:



### <a name="using-a-managed-query"></a>Použití spravovaných dotazu

Aplikace cílené na Android 2.3 (10 úroveň rozhraní API) nebo dříve by měl použít tuto metodu:

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

Tento kurzor bude spravovat Android, není potřeba ho zavřít.



### <a name="using-contentresolver"></a>Pomocí ContentResolver

Přístup k `ContentResolver` přímo k získání kurzoru proti `ContentProvider` může být podobné výjimky:

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

Tento kurzor nespravované, takže musí být uzavřen, když už se nevyžaduje.
Zkontrolujte, zda kód ukončení kurzor, který je otevřený, jinak dojde k chybě.

```csharp
cursor.Close();
```

Alternativně můžete volat `StartManagingCursor()` a `StopManagingCursor()` ke správě kurzor. Spravované kurzory jsou automaticky deaktivována a znovu dotazován při aktivity jsou zastavena a restartována.



### <a name="using-cursorloader"></a>Pomocí CursorLoader

Vytvořené aplikací pro Android 3.0 (11 úroveň rozhraní API) nebo novější, by měl použít tuto metodu:

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader` Zajistí, že všechny operace kurzoru se provádějí v vlákna na pozadí a můžete inteligentně znovu použít existující kurzoru napříč instancemi aktivity aktivitu po restartování (např. z důvodu změn konfigurace) místo, znovu načíst data znovu.

Starší verze Android můžete také použít `CursorLoader` pomocí [v4 podpory knihovny](http://developer.android.com/tools/support-library/index.html).



## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>Zobrazení dat kurzor vlastní adaptér

Zobrazit kontaktní bitové kopie použijeme vlastní adaptér tak, aby jsme vyřešit ručně `PhotoId` odkaz na cestu souboru bitové kopie.

K zobrazení dat s adaptérem vlastní, v příkladu se používá `CursorLoader` načíst všechna data obraťte se na místní kolekci v **FillContacts** metoda z **ContactsProvider/ContactsAdapter.cs**:

```csharp
void FillContacts ()
{
   var uri = ContactsContract.Contacts.ContentUri;
   string[] projection = {
       ContactsContract.Contacts.InterfaceConsts.Id,
       ContactsContract.Contacts.InterfaceConsts.DisplayName,
       ContactsContract.Contacts.InterfaceConsts.PhotoId
  };
   // CursorLoader introduced in Honeycomb (3.0, API11)
   var loader = new CursorLoader(activity, uri, projection, null, null, null);
   var cursor = (ICursor)loader.LoadInBackground();
   contactList = new List<Contact> ();
   if (cursor.MoveToFirst ()) {
      do {
          contactList.Add (new Contact{
              Id = cursor.GetLong (cursor.GetColumnIndex (projection [0])),
              DisplayName = cursor.GetString (cursor.GetColumnIndex (projection [1])),
              PhotoId = cursor.GetString (cursor.GetColumnIndex (projection [2]))
          });
       } while (cursor.MoveToNext());
   }
}
```

Potom implementovat pomocí metody BaseAdapter `contactList` kolekce. Adaptér je implementována stejně, jako je s druhá kolekce &ndash; neexistuje žádné zvláštní zpracování, protože data pochází z `ContentProvider`:

```csharp
Activity activity;
public ContactsAdapter (Activity activity)
{
   this.activity = activity;
   FillContacts ();
}
public override int Count {
   get { return contactList.Count; }
}
public override Java.Lang.Object GetItem (int position)
{
  return null; // could wrap a Contact in a Java.Lang.Object to return it here if needed
}
public override long GetItemId (int position)
{
   return contactList [position].Id;
}
public override View GetView (int position, View convertView, ViewGroup parent)
{
   var view = convertView ?? activity.LayoutInflater.Inflate (Resource.Layout.ContactListItem, parent, false);
   var contactName = view.FindViewById<TextView> (Resource.Id.ContactName);
   var contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
   contactName.Text = contactList [position].DisplayName;
   if (contactList [position].PhotoId == null) {
       contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
       contactImage.SetImageResource (Resource.Drawable.ContactImage);
   } else {
       var contactUri = ContentUris.WithAppendedId (ContactsContract.Contacts.ContentUri, contactList [position].Id);
       var contactPhotoUri = Android.Net.Uri.WithAppendedPath (contactUri, Contacts.Photos.ContentDirectory);
       contactImage.SetImageURI (contactPhotoUri);
   }
   return view;
}
```

(Pokud existuje), zobrazí se bitovou kopii pomocí identifikátoru Uri k souboru bitové kopie na zařízení. Aplikace vypadá takto:

[![Snímek obrazovky aplikace zobrazení kontaktů v prvku ListView; Obrázek se zobrazí vlevo od jednu položku](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

Používá podobný Princip kódu, vaše aplikace přístup celou řadu dat systému, včetně fotografie, videa a Hudba uživatele.
Některé typy dat vyžadují speciální oprávnění vyžadované v projektu **AndroidManifest.xml**.



## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>Zobrazení dat kurzoru s SimpleCursorAdapter

Kurzor, může se také zobrazit s `SimpleCursorAdapter` (i když se zobrazí pouze název, ne fotografie). Tento kód ukazuje, jak používat `ContentProvider` s `SimpleCursorAdapter` (Tento kód se nezobrazí v ukázce):

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName
};
var loader = new CursorLoader (this, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
var fromColumns = new string[] {ContactsContract.Contacts.InterfaceConsts.DisplayName};
var toControlIds = new int[] {Android.Resource.Id.Text1};
adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlsIds);
listView.Adapter = adapter;
```

Odkazovat [ListViews a adaptéry](~/android/user-interface/layouts/list-view/index.md) Další informace o implementaci `SimpleCursorAdapter`.


## <a name="related-links"></a>Související odkazy

- [Ukázkový ContactsAdapter (ukázka)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
