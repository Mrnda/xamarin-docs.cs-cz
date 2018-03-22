---
title: Kontakty a ContactsUI
description: "Tento článek se zabývá práce s nové kontakty a uživatelského rozhraní kontakty rozhraní v aplikaci pro Xamarin.iOS. Tyto architektury nahradit existující adresář a uživatelského rozhraní kniha adresy, které používají v předchozích verzích systému iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 0a9b9651a735ef4300e19f5ccb231a616850d970
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="contacts-and-contactsui"></a>Kontakty a ContactsUI

_Tento článek se zabývá práce s nové kontakty a uživatelského rozhraní kontakty rozhraní v aplikaci pro Xamarin.iOS. Tyto architektury nahradit existující adresář a uživatelského rozhraní kniha adresy, které používají v předchozích verzích systému iOS._

Se zavedením systému iOS 9, Apple vydala dvě nové rozhraní, `Contacts` a `ContactsUI`, které nahrazují existující adresář a uživatelského rozhraní kniha adres rozhraní používané v iOS 8 a starší.

Dva nové architektury obsahují následující funkce:

- [**Kontakty** ](#contacts) – poskytuje přístup k datům seznamu kontaktů uživatele.
    Protože většinu aplikací vyžadují jenom oprávnění jen pro čtení, toto rozhraní je optimalizován pro přístup z více vláken bezpečné, jen pro čtení přístup.

- [**ContactsUI** ](#contactsui) -prvky poskytuje uživatelského rozhraní Xamarin.iOS chcete zobrazit, upravit, vyberte a vytvořte kontakty na zařízeních s iOS.

[![](contacts-images/add01.png "Příklad listu obraťte se na zařízení s iOS")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> Existující `AddressBook` a `AddressBookUI` rozhraní používá iOS 8 (a před) jsou zastaralé v iOS 9 a by měla být nahrazená s novým `Contacts` a `ContactsUI` architektury co nejdříve pro jakékoli existující aplikaci Xamarin.iOS. Nové aplikace by měla být napsaných pro nové architektury.




V následujících částech provedeme podívejte se na tyto nové architektury a jejich implementaci v aplikaci pro Xamarin.iOS.

<a name="contacts" />

## <a name="the-contacts-framework"></a>Rozhraní Framework kontaktů

Rozhraní Framework kontakty poskytuje Xamarin.iOS přístup ke kontaktní údaje uživatele. Protože většinu aplikací vyžadují jenom oprávnění jen pro čtení, toto rozhraní je optimalizován pro přístup z více vláken bezpečné, jen pro čtení přístup.

<a name="Contact_Objects" />

### <a name="contact-objects"></a>Objekty kontaktů

`CNContact` Třída poskytuje přístup z více vláken bezpečné, jen pro čtení přístup k vlastnostem a kontaktovat, jako je jméno, adresu nebo telefonní čísla. `CNContact` funguje jako `NSDictionary` a obsahuje více, jen pro čtení kolekcí vlastností (například adres nebo telefonních čísel):

[![](contacts-images/contactobjects.png "Obraťte se na přehled objektů")](contacts-images/contactobjects.png#lightbox)

Jakákoli vlastnost, která může mít více hodnot (například e-mailovou adresu nebo telefonní čísla), bude být reprezentován jako pole `NSLabeledValue` objekty. `NSLabeledValue` je vlákno bezpečné řazené kolekce členů skládající se z sadu popisky jen pro čtení a hodnoty, kde popisek definuje hodnotu pro uživatele (například domácí nebo pracovní e-mailu). Rozhraní framework kontakty poskytuje řadu předdefinovaných popisky (prostřednictvím `CNLabelKey` a `CNLabelPhoneNumberKey` statické třídy), můžete použít v aplikaci nebo máte možnost definovat vlastní popisky pro vaše potřeby.

Pro jakékoli aplikaci Xamarin.iOS, která je potřeba upravit hodnoty existující kontakt (nebo vytvořit nové), použijte `NSMutableContact` verzi jeho dílčí třídy a třídy (například `CNMutablePostalAddress`).

Kód postupujte podle kroků bude například vytvořit nový kontakt a přidat do kolekce uživatele kontaktů:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Add email addresses
var homeEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@mac.com"));
var workEmail = new CNLabeledValue<NSString>(CNLabelKey.Work, new NSString("john.appleseed@apple.com"));
contact.EmailAddresses = new CNLabeledValue<NSString>[] { homeEmail, workEmail };

// Add phone numbers
var cellPhone = new CNLabeledValue<CNPhoneNumber>(CNLabelPhoneNumberKey.iPhone, new CNPhoneNumber("713-555-1212"));
var workPhone = new CNLabeledValue<CNPhoneNumber>("Work", new CNPhoneNumber("408-555-1212"));
contact.PhoneNumbers = new CNLabeledValue<CNPhoneNumber>[] { cellPhone, workPhone };

// Add work address
var workAddress = new CNMutablePostalAddress()
{
    Street = "1 Infinite Loop",
    City = "Cupertino",
    State = "CA",
    PostalCode = "95014"
};
contact.PostalAddresses = new CNLabeledValue<CNPostalAddress>[] { new CNLabeledValue<CNPostalAddress>(CNLabelKey.Work, workAddress) };

// Add birthday
var birthday = new NSDateComponents()
{
    Day = 1,
    Month = 4,
    Year = 1984
};
contact.Birthday = birthday;

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

// Attempt to save changes
NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error))
{
    Console.WriteLine("New contact saved");
}
else
{
    Console.WriteLine("Save error: {0}", error);
}
```

Pokud tento kód spustí na zařízení se systémem iOS 9, nový kontakt se zařadí do kolekce uživatele. Příklad:

[![](contacts-images/add01.png "Nový kontakt přidat do kolekce uživatele")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>Kontaktujte formátování a lokalizace

Rozhraní framework kontakty obsahuje několik objekty a metody, které vám mohou pomoci formátu a lokalizaci obsahu pro zobrazení pro uživatele. Následující kód například správně by formátování název kontakty a e-mailová adresa pro zobrazení:

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

Vlastnost štítků, které jste se zobrazení v uživatelském rozhraní aplikace kontaktujte framework má metody pro lokalizace také tyto řetězce. Znovu to je založené na aktuální národní prostředí zařízení s iOS, které se spouští aplikace. Příklad:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOption.Nickname));
Console.WriteLine(CNLabeledValue.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>Načítání existující kontakty

Pomocí instance `CNContactStore` třídy, může načíst kontaktní informace z databáze Kontakty uživatele. `CNContactStore` Obsahuje všechny metody požadované pro načítání nebo aktualizaci kontakty a skupiny z databáze. Vzhledem k tomu, že tyto metody jsou synchronní, doporučujeme vám spustit na vlákna na pozadí udržovat blokovat uživatelského rozhraní.

Pomocí predikáty (vybudována `CNContact` třída), můžete filtrovat výsledky vrácené při načítání kontakty z databáze. Načíst pouze kontakty, které obsahují řetězec `Appleseed`, použijte následující kód:

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> Obecné a složené predikáty nepodporuje rozhraní kontakty.

Například pro limit načtení jenom **GivenName** a **FamilyName** vlastnosti kontaktu, použijte následující kód:

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

Nakonec k vyhledání databáze a vrátí výsledky, použijte následující kód:

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

Pokud po ukázku, kterou jsme vytvořili v po spuštění tohoto kódu **kontakty objekt** část výše, měla by vrátit kontakt "Jan Appleseed", který jsme právě vytvořili.

### <a name="contact-access-privacy"></a>Ochrana osobních údajů kontaktní přístup

Protože koncovým uživatelům můžete udělit nebo odepřít přístup k jejich kontaktní informace na základě jednotlivých aplikací, první provedete volání `CNContactStore`, zobrazí se dialogové okno zobrazí s požadavkem pro povolení přístupu pro vaši aplikaci.

Žádost o oprávnění se zobrazí pouze jednou, při prvním spuštění a následné je aplikace spustí nebo volá, aby se `CNContactStore` použije oprávnění, který uživatel vybral v daném čase.

Měli byste navrhnout aplikace tak, aby řádně zpracovává uživatele odepření přístupu k jejich kontaktní databázi.

#### <a name="fetching-partial-contacts"></a>Načítání částečné kontaktů

A _částečné obraťte se na_ je kontaktu, který jenom některé z dostupných vlastností byl načteny z úložiště kontaktů pro. Při pokusu o přístup k vlastnosti, která nebyla dříve načtených způsobí výjimku.

Můžete snadno zkontrolujte, zda daný kontakt má požadovanou vlastnost pomocí `IsKeyAvailable` nebo `AreKeysAvailable` metody `CNContact` instance. Příklad:

```csharp
// Does the contact contain the requested key?
if (!contact.IsKeyAvailable(CNContactOption.PostalAddresses)) {
    // No, re-request to pull required info
    var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName, CNContactKey.PostalAddresses};
    var store = new CNContactStore();
    NSError error;
    contact = store.GetUnifiedContact(contact.Identifier, fetchKeys, out error);
}
```

> [!IMPORTANT]
> `GetUnifiedContact` a `GetUnifiedContacts` metody `CNContactStore` třída _pouze_ vrátit a částečné obraťte se na jiné vlastnosti vyžádaný z načítání klíče zadané.

### <a name="unified-contacts"></a>Jednotná kontaktů

Uživatel může mít různé zdroje kontaktní informace pro jedna osoba v jejich kontaktní databázi (jako je iCloud, Facebook nebo Google e-mailu). V iOS a OS X aplikace, tyto kontaktní informace bude automaticky propojit dohromady a zobrazí uživateli, jako jedinou _Unified kontaktovat_:

[![](contacts-images/unified01.png "Jednotná Přehled kontaktů")](contacts-images/unified01.png#lightbox)

Kontaktujte Unified je dočasný, v paměti zobrazení odkaz kontaktní informace, které budou mít svůj vlastní jedinečný identifikátor (která se má použít na znovu načíst kontakt v případě potřeby). Ve výchozím nastavení vrátí framework kontakty a Unified kontaktovat, pokud je to možné.

### <a name="creating-and-updating-contacts"></a>Vytvářet a aktualizovat kontakty

Jak jsme viděli v [obraťte se na objekty](#Contact_Objects) část výše, můžete použít `CNContactStore` a instance `CNMutableContact` k vytvoření nových kontaktů, pak zapsaných do uživatele obraťte se na databázi pomocí `CNSaveRequest`:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("New contact saved");
} else {
    Console.WriteLine("Save error: {0}", error);
}
```

A `CNSaveRequest` slouží také k více změn kontaktní a skupiny do jedné operace do mezipaměti a tyto změny služby batch `CNContactStore`.

Pokud chcete aktualizovat neměnitelnou kontakt získané z operace fetch, musíte nejdřív požádat o měnitelný kopie, kterou pak upravit a uložit zpět do úložiště kontaktů. Příklad:

```csharp
// Get mutable copy of contact
var mutable = contact.MutableCopy() as CNMutableContact;
var newEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@xamarin.com"));

// Append new email
var emails = new NSObject[mutable.EmailAddresses.Length+1];
mutable.EmailAddresses.CopyTo(emails,0);
emails[mutable.EmailAddresses.Length+1] = newEmail;
mutable.EmailAddresses = emails;

// Update contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.UpdateContact(mutable);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("Contact updated.");
} else {
    Console.WriteLine("Update error: {0}", error);
}
```

### <a name="contact-change-notifications"></a>Oznámení o změnách kontaktů

Vždy, když se mění a kontaktovat, obraťte se na úložiště odešle `CNContactStoreDidChangeNotification` k centru oznámení výchozí. Pokud jste do mezipaměti nebo aktuálně zobrazují všechny kontakty, budete muset aktualizovat tyto objekty z obchodu kontakt (`CNContactStore`).

### <a name="containers-and-groups"></a>Kontejnery a skupiny

Kontaktů uživatele může existovat buď místně na zařízení uživatele, nebo jako Kontakty synchronizované do zařízení z jednoho nebo více účtů serveru (jako je Facebook nebo Google). Každý fond kontaktů má svou vlastní _kontejneru_ a a obraťte se na danou může existovat pouze v jednom kontejneru.

[![](contacts-images/containers01.png "Kontejnery a skupiny – přehled")](contacts-images/containers01.png#lightbox)

Některé kontejnery umožňují kontakty, které mají být uspořádány do jedné nebo více _skupiny_ nebo _podskupin_. Toto chování je závislá na úložiště zálohování pro daný kontejner. Například Icloudu má jen jeden kontejner, ale může mít mnoho skupin (ale žádné dílčí skupiny). Microsoft Exchange na druhé straně nepodporuje skupiny, ale může mít více kontejnerů (jeden pro každou složku Exchange).

[![](contacts-images/containers02.png "Nepřekrývá kontejnery a skupiny")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>Rozhraní ContactsUI

V situacích, kde vaše aplikace nemusí k dispozici vlastní uživatelské rozhraní můžete použít rozhraní ContactsUI nabídne prvky uživatelského rozhraní, které chcete zobrazit, upravit, vyberte a vytvořit kontakty ve vaší aplikaci Xamarin.iOS.

Pomocí integrované ovládací prvky společnosti Apple nejen snížit objem kód, který je nutné vytvořit na kontaktní údaje podpory ve vaší aplikaci Xamarin.iOS, ale můžete uživatelům aplikace k dispozici jednotné rozhraní.

### <a name="the-contact-picker-view-controller"></a>Řadiče zobrazení kontaktní výběr.

Obraťte se na výběr View Controller (`CNContactPickerViewController`) spravuje standardní zobrazení výběr kontaktu, které umožňuje uživateli vybrat a obraťte se na nebo obraťte se na vlastnosti z databáze obraťte se na uživatele. Uživatele můžete vybrat jeden nebo více kontakt (na základě jeho využití) a obraťte se na výběr View Controller nezobrazí výzvu pro oprávnění, než se zobrazí nástroje pro výběr.

Před voláním `CNContactPickerViewController` třídu, můžete definovat vlastnosti, které můžete vybrat a definovat predikáty k řízení zobrazení a výběr vlastností, obraťte se na uživatele.

Použití instance třídy, která dědí z `CNContactPickerDelegate` reagovat na interakce uživatele pomocí nástroje pro výběr. Příklad:

```csharp
using System;
using System.Linq;
using UIKit;
using Foundation;
using Contacts;
using ContactsUI;

namespace iOS9Contacts
{
    public class ContactPickerDelegate: CNContactPickerDelegate
    {
        #region Constructors
        public ContactPickerDelegate ()
        {
        }

        public ContactPickerDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ContactPickerDidCancel (CNContactPickerViewController picker)
        {
            Console.WriteLine ("User canceled picker");

        }

        public override void DidSelectContact (CNContactPickerViewController picker, CNContact contact)
        {
            Console.WriteLine ("Selected: {0}", contact);
        }

        public override void DidSelectContactProperty (CNContactPickerViewController picker, CNContactProperty contactProperty)
        {
            Console.WriteLine ("Selected Property: {0}", contactProperty);
        }
        #endregion
    }
}
```

Povolit uživateli vybrat e-mailovou adresu z kontakty ve své databázi, můžete použít následující kód:

```csharp
// Create a new picker
var picker = new CNContactPickerViewController();

// Select property to pick
picker.DisplayedPropertyKeys = new NSString[] {CNContactKey.EmailAddresses};
picker.PredicateForEnablingContact = NSPredicate.FromFormat("emailAddresses.@count > 0");
picker.PredicateForSelectionOfContact = NSPredicate.FromFormat("emailAddresses.@count == 1");

// Respond to selection
picker.Delegate = new ContactPickerDelegate();

// Display picker
PresentViewController(picker,true,null);
```

### <a name="the-contact-view-controller"></a>Řadič kontaktní zobrazení

Řadiče zobrazení obraťte se na (`CNContactViewController`) třída poskytuje řadič na standardní zobrazení kontaktu pro koncového uživatele. Obraťte se na zobrazení můžete zobrazit nové nový, neznámý nebo existující kontakty a typ musí být zadána před zobrazení se zobrazí při volání správné statického konstruktoru (`FromNewContact`, `FromUnknownContact`, `FromContact`). Příklad:

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>Souhrn

Tento článek trvá podrobný pohled na práci s rozhraními kontaktu a obraťte se na uživatelské rozhraní aplikace pro Xamarin.iOS. Nejprve popsané různé typy objektů, které poskytuje rozhraní kontaktu a jak je použít k vytváření nových nebo přístup k existující kontaktům. Rovněž zkoumány obraťte se na uživatelské rozhraní framework vyberte existující kontakty a zobrazit kontaktní informace.


## <a name="related-links"></a>Související odkazy

- [Ukázka QuickContacts](https://developer.xamarin.com/samples/monotouch/ios9/QuickContacts/)
- [Co je nového v iOS 9](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html#//apple_ref/doc/uid/TP40016198-DontLinkElementID_14))
- [Kontakty Framework – referenční informace](https://developer.apple.com/library/prerelease/ios/documentation/Contacts/Reference/Contacts_Framework/index.html#//apple_ref/doc/uid/TP40015328)
- [ContactsUI Framework – referenční informace](https://developer.apple.com/library/prerelease/ios/documentation/ContactsUI/Reference/ContactsUI_Framework/index.html#//apple_ref/doc/uid/TP40016207)
