---
title: Profil uživatele
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/22/2018
ms.openlocfilehash: 1eaae86ab9eacf007eca792d96e889db6f367922
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="user-profile"></a>Profil uživatele

Android obsahoval podporované výčet kontakty s [ContactsContract](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/) zprostředkovatele od 5 úroveň rozhraní API. Například výpis kontakty je jednoduché, pomocí [ContactContracts.Contacts](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Contacts/) třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
// Get the URI for the user's contacts:
var uri = ContactsContract.Contacts.ContentUri;

// Setup the "projection" (columns we want) for only the ID and display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id, 
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the user's contacts data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();

// Print the contact data to the console if reading back succeeds:
if (cursor != null)
{
    if (cursor.MoveToFirst())
    {
        do
        {
            Console.WriteLine("Contact ID: {0}, Contact Name: {1}",
                               cursor.GetString(cursor.GetColumnIndex(projection[0])),
                               cursor.GetString(cursor.GetColumnIndex(projection[1])));
        } while (cursor.MoveToNext());
    }
}
```

Od verze Android 4 (14 úroveň rozhraní API), [ContactsContact.Profile](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Profile/) třída je k dispozici prostřednictvím `ContactsContract` zprostředkovatele. `ContactsContact.Profile` Poskytuje přístup k osobním profilem pro vlastníka zařízení, která zahrnuje kontaktní údaje, jako je vlastník zařízení název a telefonní číslo.


## <a name="required-permissions"></a>Požadovaná oprávnění

Čtení a zápis kontaktů dat, aplikace musí požádat o `READ_CONTACTS` a `WRITE_CONTACTS` oprávnění, v uvedeném pořadí.
Kromě toho pokud chcete přečíst a upravit profil uživatele, aplikace musíte požádat o `READ_PROFILE` a `WRITE_PROFILE` oprávnění.


## <a name="updating-profile-data"></a>Aktualizace dat profilu

Jakmile nastavili tato oprávnění aplikace můžete použít běžné techniky Android k interakci s daty uživatelský profil. Například k aktualizaci profilu zobrazovaný název, volání [ContentResolver.Update](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.Update) s `Uri` načteny prostřednictvím [ContactsContract.Profile.ContentRawContactsUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentRawContactsUri/) vlastnost, jak je znázorněno níže:

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>Čtení dat profilu

Zadání dotazu na [ContactsContact.Profile.ContentUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentUri/) zpět čte data profilu. Například následující kód bude číst profil uživatele zobrazovaný název:

```csharp
// Read the profile
var uri = ContactsContract.Profile.ContentUri;

// Setup the "projection" (column we want) for only the display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();
if (cursor != null)
{
    if (cursor.MoveToFirst ())
    {
        Console.WriteLine(cursor.GetString (cursor.GetColumnIndex (projection [0])));
    }
}
```

## <a name="navigating-to-the-user-profile"></a>Navigace k profilu uživatele

Nakonec přejděte na uživatelský profil, vytvořte záměrem s `ActionView` akce a `ContactsContract.Profile.ContentUri` pak předejte jej `StartActivity` metoda takto:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

Při spuštění ve výše uvedeném kódu, se zobrazí profil uživatele, jak je znázorněno na následujícím snímku obrazovky:

[![Snímek obrazovky zobrazení profilu uživatele John Doe profilu](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

Práce s profil uživatele je podobná interakci s ostatními daty v Android a poskytuje další úroveň individuální nastavení zařízení.



## <a name="related-links"></a>Související odkazy

- [ContactsProviderDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Představení Sandwichovy Zmrzlinová](http://www.android.com/about/ice-cream-sandwich/)
- [Platformu Android 4.0](http://developer.android.com/sdk/android-4.0.html)
