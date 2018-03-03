---
title: "Proaktivní návrhy"
description: "Tento článek ukazuje způsob použití proaktivní návrhy v aplikaci watchOS 3 engagement disku tím, že systém proaktivně automaticky nabídne užitečné informace pro uživatele."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: ca2476eef120c7d86b939934ec4b286e871d6a78
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="proactive-suggestions"></a>Proaktivní návrhy

_Tento článek ukazuje způsob použití proaktivní návrhy v aplikaci watchOS 3 engagement disku tím, že systém proaktivně automaticky nabídne užitečné informace pro uživatele._


Nový watchOS 3, proaktivní návrhy přítomen způsoby zprávy pro uživatele, abyste upoutali aplikace Xamarin.iOS pomocí proaktivně zobrazoval užitečné informace, které automaticky pro uživatele v příslušnou dobu.


## <a name="about-proactive-suggestions"></a>O proaktivní návrhy

Nový watchOS 3, `NSUserActivity` zahrnuje `MapItem` vlastnost, která umožňuje aplikaci poskytují informace o umístění, které je možné v jiném kontextu. Například, pokud aplikace zobrazí hotelů zkontroluje a poskytuje `MapItem` umístění, pokud uživatel přepnout do aplikace mapy, umístění hotelů pouze zobrazení by být k dispozici.

Aplikace zpřístupní tuto funkci do systému pomocí kolekce technologií, jako třeba `NSUserActivity`, MapKit, přehrávač médií a UIKit. Kromě toho tím, že poskytuje proaktivní návrhu podporu pro aplikace, získá hlubší integrace Siri zdarma.

## <a name="location-based-suggestions"></a>Návrhy na základě umístění

Nový watchOS 3, `NSUserActivity` zahrnuje třídy `MapItem` vlastnost, která umožňuje vývojáři poskytnout informace o umístění, které je možné v jiném kontextu. Například pokud aplikace zobrazí recenze restaurace, vývojáře můžete nastavit `MapItem` vlastnost umístění restaurace, které je uživatel zobrazit v aplikaci. Pokud se uživatel do aplikace Maps, je automaticky dostupný restaurace umístění.

Pokud aplikace podporuje vyhledávání aplikace, může použít nové součásti adresu `CSSearchableItemAttributesSet` třídu k určení umístění, které uživatel může chtít navštívit. Nastavením `MapItem` vlastnost, ostatní vlastnosti jsou automaticky vyplněné.

Kromě nastavení `Latitude` a `Longitude` vlastnosti součásti adresy, je doporučeno, aby aplikace zadat `NamedLocation` a `PhoneNumbers` vlastnosti, takže Siri můžete zahájit volání do umístění.

## <a name="contextual-siri-reminders"></a>Kontextová Siri připomenutí

Umožňuje uživatelům používat Siri a rychle zajistit zobrazí se připomenutí pro zobrazení obsahu, že aktuálně nalezli jsou v aplikaci později. Například pokud kontrolu restaurace jejich byly zobrazení v aplikaci, se může vyvolat Siri a vyslovte *"Připomenout o to při doručení Domů."* Siri by vygeneroval upozornění se zobrazí odkaz na kontrola v aplikaci.

## <a name="implementing-proactive-suggestions"></a>Implementace proaktivní návrhy

Přidání proaktivní návrhu je obvykle stejně snadná jako implementace několik rozhraní API nebo rozšíření na několik rozhraní API, které aplikace může být implementace již podporu, aby aplikace Xamarin.iOS.

Proaktivní návrhy pracovat s aplikací třemi hlavními způsoby:

- **`NSUserActivity`** -Pomáhá pochopit, jaké informace uživatel aktuálně pracuje s na obrazovce systému.
- **Umístění návrhy** – Pokud je aplikace nabízí nebo využívá informace o umístění na základě, tyto nové způsoby rozhraní API rozšíření nabídky sdílet tyto informace napříč aplikací.

A je podporováno v aplikaci implementací následující:

- **Kontextová připomenutí Siri** – v iOS 10, `NSUserActivity` rozšířila umožní aplikaci Siri získat rychle Ujistěte se, zobrazí se připomenutí pro zobrazení obsahu aktuálně nalezli jsou v aplikaci později.
- **Umístění návrhy** -iOS 10 zlepšuje `NSUserActivity` zaznamenat umístění zobrazit v aplikaci a podporovat jejich na mnoha místech v systému.
- **Kontextová požadavky Siri**  -  `NSUserActivity` poskytuje kontext, který má informace zobrazené v aplikaci Siri tak, aby uživatel můžete získat pokynů nebo místní volání být volajícím Siri z aplikace.

Všechny tyto funkce mají jedno, že jsou všechny používané `NSUserActivity` v jednoho formuláře nebo jiné zajistit jejich funkce. 

## <a name="nsuseractivity"></a>NSUserActivity

Jak jsme uvedli výše, `NSUserActivity` pomáhá pochopit, jaké informace uživatel aktuálně pracuje s na obrazovce systému. `NSUserActivity` je stav šedé – ukládání do mezipaměti mechanismus pro zachycení činnost uživatele procházet aplikace. Například prohlížení restaurace aplikace:

[ ![](proactive-suggestions-images/activity02.png "Restaurace aplikace")](proactive-suggestions-images/activity02.png)

Pomocí následujících interakce:

1. Jako uživatel pracuje s aplikací, `NSUserActivity` vzniká, chcete-li obnovit stav aplikace později.
2. Pokud uživatel hledá restaurace, a pod ním stejný vzor vytváření aktivit.
3. A znovu, když uživatel prohlíží výsledku. V tomto případě poslední uživatel prohlíží umístění a iOS 10, systém je víc paměti některé pojmy (jako je například umístění nebo komunikace interakce).

Prohlédněte si blíže poslední obrazovky:

[ ![](proactive-suggestions-images/activity03.png "Datová část NSUserActivity")](proactive-suggestions-images/activity03.png)

Zde je vytváření aplikace `NSUserActivity` a naplněné s informacemi o stavu znovu vytvořit později. Aplikace má také zahrnuty některé metadata, např. název a adresu umístění. K této aktivitě vytvořen umožňuje aplikaci iOS vědět, že reprezentuje aktuální stav uživatele.

Aplikace pak rozhodne, pokud bude inzerován bezdrátovou předání, uložit jako dočasnou hodnotou návrhy umístění nebo přidat do indexu Spotlight na zařízení pro zobrazení ve výsledcích hledání aktivity.

Další informace o předání a Spotlight search, najdete v tématu naše [Úvod k předání](~/ios/platform/handoff.md) a [iOS 9 nové rozhraní API pro vyhledávání](~/ios/platform/search/index.md) příručky.

### <a name="creating-an-activity"></a>Vytvoření aktivity

Před vytvořením aktivitu, identifikátor typu aktivity bude potřeba vytvořit pro identifikaci. Identifikátor typu aktivity je krátký řetězec přidán do `NSUserActivityTypes` pole aplikace `Info.plist` souboru, který slouží k jednoznačné identifikaci typu aktivity daného uživatele. V poli pro každou aktivitu, která podporuje a vystavuje aplikace vyhledávání aplikace bude jeden záznam. V tématu naše [vytváření odkaz identifikátory typ aktivity](~/ios/platform/search/nsuseractivity.md) další podrobnosti.

Podívejte se na příklad aktivity:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Inform system of Activity
activity.BecomeCurrent();
```

Nová aktivita je vytvořený pomocí typu identifikátor aktivity. Dále některá metadata definování aktivity je vytvořen, tento stav může být obnovena později. Aktivita je pak přidělen smysluplný název a připojené k informace o uživateli. Nakonec některé možnosti jsou povolené a aktivity jsou odeslána do systému.

Výše uvedený kód může být další vylepšené a zahrnují metadata, která poskytuje kontext, který má aktivita tím, že provedete následující změny:

```csharp
...

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Inform system of Activity
activity.BecomeCurrent();
```

Pokud vývojář má web, který je schopen zobrazovat stejné informace jako aplikace, aplikace může obsahovat adresu URL a obsah lze zobrazit na jiná zařízení, které nemají nainstalovanou (prostřednictvím aby Handoff):

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>Obnovení aktivitu

Reagovat na uživatel klepnutím na výsledek hledání (`NSUserActivity`) pro aplikaci a upravit **AppDelegate.cs** souborů a přepsat `ContinueUserActivity` metoda. Příklad:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

Ujistěte se, toto je stejný typ identifikátor aktivity (`com.xamarin.platform`) jako aktivita vytvořili výše. Aplikace využívá informace uložené v `NSUserActivity` k obnovení stavu zpět do tam, kde uživatel přestali.

### <a name="benefits-of-creating-an-activity"></a>Výhody vytvoření aktivity

S minimální velikostí kód uvedený výše aplikace je teď možné využívat tři nové funkce iOS 10:

- **Handoff**
- **Pomocí vyhledávání Spotlight**
- **Kontextová Siri připomenutí**

V následující části se podívejte se na povolení dva další nové funkce iOS 10:

- **Návrhy umístění**
- **Kontextová Siri požadavky**

### <a name="location-based-suggestions"></a>Návrhy na základě umístění 

V příkladu výše aplikace vyhledávání restaurace trvat. Pokud má implementovaný `NSUserActivity` a správně nastaven všechna metadata a atributy, uživatel bude moct provádět následující:

1. Najít v aplikaci, která se mají splňovat friend v restauraci.
4. Pokud se uživatel do aplikace Maps, restaurace adres automaticky navržený jako cíl.
5. To funguje i pro aplikace, 3. stran (podporující `NSUserActivity`), takže může uživatel přepínat do aplikace pro sdílení pravé a jako cíl existuje restaurace adres automaticky navržený také.
6. Také poskytuje kontext Siri, takže uživatel může vyvolat Siri v rámci aplikace restaurace a požádejte *"Get pokynů..."* a Siri poskytuje pokyny k restaurace, uživatel je zobrazení.

Všechny výše uvedené funkce je jednou z věcí, všechny indikují kde návrhy původně pochází z. V případě výše uvedeném příkladu je fiktivní restaurace revize aplikace.

Chcete-li povolit tuto funkci pro aplikaci prostřednictvím několika malé změny a dodatky do existujících architektur se rozšířily watchOS 3:

- `NSUserActivity` obsahuje další pole pro zaznamenání informace o umístění, které se zobrazí uvnitř aplikace.
- Několik přidané jste provedli MapKit a CoreSpotlight k zachycení umístění.
- Umístění vědět funkce přidaná Siri, mapy, Multitasking a dalších aplikací v rámci systému.

K implementaci umístění na základě návrhy, začněte s stejný kód aktivity, které jsou popsané výše:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");

// Inform system of Activity
activity.BecomeCurrent();
```

Pokud aplikace používá MapKit, je jednoduché, přidání aktuální mapu `MKMapItem` aktivity:

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

Pokud aplikace nepoužívá MapKit, je přijmout aplikace hledání a zadejte následující nové atributy pro umístění:

```csharp
// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
...

attributes.NamedLocation = "Apple Inc.";
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

Podívejte se na výše uvedeném kódu podrobně. Nejprve název umístění je požadováno v každé instanci:

```csharp
attributes.NamedLocation = "Apple Inc.";
```

Potom text podle popisu v požadované pro text na základě instance (například klávesnici QuickType):

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Zeměpisné šířky a délky jsou nepovinné, ale ujistěte se, že uživatel se směruje na přesné umístění aplikace je chtějí pošlete je:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Nastavením telefonní čísla aplikaci získat přístup k Siri, proto může uživatel vyvolat Siri z aplikace vyslovením něco podobného jako, * "Volání toto místo":

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Nakonec aplikaci může naznačovat, pokud instance je vhodný pro navigaci a telefonních hovorů:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```
## <a name="activities-best-practices"></a>Osvědčené postupy aktivity

Apple navrhuje následující osvědčené postupy při práci s aktivitami:

- Použití `NeedsSave` aktualizací opožděné datové části.
- Zajistěte, aby silné odkaz na aktuální aktivity.
- Převod pouze malé datových částí, které obsahují právě dostatek informací k obnovení stavu.
- Zajistěte, aby identifikátory aktivit typu jedinečný a popisný pomocí DNS zpětného zápisu zadávat. 

## <a name="consuming-location-suggestions"></a>Využívání návrhy umístění

Této další části se bude zabývat využívání návrhu umístění, které pocházejí z dalších částí systému (například aplikace Maps) nebo jiné aplikace 3. stran.

## <a name="routing-apps-and-locations-suggestions"></a>Směrování aplikace a návrhy umístění

V této části se podívejte se na využívání umístění návrhy přímo z aplikace směrování. Pro směrování aplikaci přidat tuto funkci, se vývojáři využít existující `MKDirectionsRequest` framework následujícím způsobem:

- Podporovat aplikace v Multitasking.
- Pro registraci aplikace jako směrování aplikaci.
- Pro zpracování spuštění aplikace s MapKit `MKDirectionsRequest` objektu.
- Poskytují watchOS možnost Další navrhovat aplikace založené na zapojení uživatele.

Při spuštění aplikace s MapKit `MKDirectionsRequest` objektu ho by měl automaticky spustit poskytnutí pokynů uživatele zadaného umístění nebo uživatelské rozhraní, které usnadňuje uživatelům spustit získání pokynů k dispozici. Příklad:


```csharp
using System;
using Foundation;
using UIKit;
using MapKit;
using CoreLocation;

namespace MonkeyChat
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate, IUISplitViewControllerDelegate
    {
        ...
        
        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            if (MKDirectionsRequest.IsDirectionsRequestUrl (url)) {
                var request = new MKDirectionsRequest (url);
                var coordinate = request.Destination?.Placemark.Location?.Coordinate;
                var address = request.Destination.Placemark.AddressDictionary;
                if (coordinate.IsValid()) {
                    var geocoder = new CLGeocoder ();
                    geocoder.GeocodeAddress (address, (place, err) => {
                        // Handle the display of the address

                    });
                }
            }

            return true;
        }
    }       
}
```

Podívejte se na tento kód podrobně. Průvodce zjišťuje, zkontrolujte, jestli je požadavek platný cíl:

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

Pokud je, pak vytvoří `MKDirectionsRequest` z adresy URL:

```csharp
var request = new MKDirectionsRequest(url);
```

Nové v watchOS 3 aplikace lze odeslat adresu, která nemá geo souřadnice, v tomto příčina vývojář musí ke kódování adresu:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých proaktivní návrhy a vám ukázal, jak vývojář lze využít přesměrovat provoz na aplikace Xamarin.iOS pro watchOS. Je popsaná v kroku k implementaci proaktivní návrhy a uvedené pokyny týkající se používání.


## <a name="related-links"></a>Související odkazy

- [Ukázky pro watchOS](https://developer.xamarin.com/samples/watchos/all/)
- [Průvodce programováním SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
