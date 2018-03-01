---
title: "Profil uživatele"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2017
ms.openlocfilehash: 53ac30abea05095583fcac5ddc315f93ce7024f2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="user-profile"></a>Profil uživatele

Android obsahoval podporované výčet kontakty s `ContactsContract` zprostředkovatele od 5 úroveň rozhraní API. Například do seznamu kontaktů je jednoduché, pomocí `ContactContracts.Contacts` třídy, jak je znázorněno v následujícím kódu:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
           
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id,
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);
           
if (cursor.MoveToFirst ()) {
    do {
        Console.WriteLine ("Contact ID: {0}, Contact Name: {1}",
            cursor.GetString (cursor.GetColumnIndex (projection [0])),
            cursor.GetString (cursor.GetColumnIndex (projection [1])));
                   
    } while (cursor.MoveToNext());
}
```

Se systémem Android 4 (14 úroveň rozhraní API) nový `ContactsContact.Profile` třída je k dispozici prostřednictvím poskytovatele ContactsContract. `ContactsContact.Profile` Poskytuje přístup k osobním profilu pro vlastníka zařízení, která zahrnuje kontaktní údaje, jako je vlastník zařízení název a telefonní číslo.

<a name="Required_Permissions" />

## <a name="required-permissions"></a>Požadovaná oprávnění

Čtení a zápis kontaktů dat, aplikace musí požádat o `Read_Contacts` a `Write_Contacts` oprávnění, v uvedeném pořadí. Kromě toho pokud chcete přečíst a upravit profil uživatele, aplikace musíte požádat o `Read_Profile` a `Write_Profile` oprávnění.

<a name="Updating_Profile_Data" />

## <a name="updating-profile-data"></a>Aktualizace dat profilu

Jakmile nastavili tato oprávnění aplikace můžete použít běžné techniky Android k interakci s daty uživatelský profil. Například aktualizovat zobrazovaný název profilu by říkáme `ContentResolver.Update` s `Uri` načteny prostřednictvím `ContactsContract.Profile.ContentRawContactsUri` vlastnost, jak je uvedeno níže:

```csharp
var values = new ContentValues ();
          
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName,
    "John Doe");
           
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri,
    values, null, null);
```

<a name="Reading_Profile_Data" />

## <a name="reading-profile-data"></a>Čtení dat profilu

Zadání dotazu na `ContactsContact.Profile.ContentUri` zpět čte data profilu. Například následující kód bude číst profil uživatele zobrazovaný název:

```csharp
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);

if (cursor.MoveToFirst ()) {
    Console.WriteLine(
        cursor.GetString (cursor.GetColumnIndex (projection [0])));
}
```

<a name="Navigating_to_the_People_App" />

## <a name="navigating-to-the-people-app"></a>Navigace na aplikaci uživatelé

Nakonec, přejděte na uživatelský profil v nové aplikaci osoby, která se dodává se systémem Android 4, stačí vytvořit záměrem s `ActionView` akce a `ContactsContract.Profile.ContentUri`, možností a předejte ji do `StartActivity` metoda takto:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

Při spuštění ve výše uvedeném kódu, osoby aplikace se načte do profilu uživatele, jak je znázorněno na následujícím snímku obrazovky:

[![Zobrazení profil uživatele John Doe aplikace – snímek obrazovky osoby](user-profile-images/15-people-app.png)](user-profile-images/15-people-app.png)

Práce s profil uživatele je nyní podobná interakci s ostatními daty v Android a poskytuje další úroveň individuální nastavení zařízení.



## <a name="related-links"></a>Související odkazy

- [ContactsProviderDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Představení Sandwichovy Zmrzlinová](http://www.android.com/about/ice-cream-sandwich/)
- [Platformu Android 4.0](http://developer.android.com/sdk/android-4.0.html)
