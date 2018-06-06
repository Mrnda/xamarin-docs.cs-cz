---
title: Úvod do proaktivní návrhů v Xamarin.iOS
description: Tento článek ukazuje způsob použití proaktivní návrhy v aplikaci Xamarin.iOS engagement disku tím, že systém proaktivně automaticky nabídne užitečné informace pro uživatele.
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f736e9dda00546ddef7cf03457813c7e3d10882b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788012"
---
# <a name="introduction-to-proactive-suggestions-in-xamarinios"></a>Úvod do proaktivní návrhů v Xamarin.iOS

_Tento článek ukazuje způsob použití proaktivní návrhy v aplikaci Xamarin.iOS engagement disku tím, že systém proaktivně automaticky nabídne užitečné informace pro uživatele._

Nový iOS 10, proaktivní návrhy přítomen způsoby zprávy pro uživatele, abyste upoutali aplikace Xamarin.iOS pomocí proaktivně zobrazoval užitečné informace, které automaticky pro uživatele v příslušnou dobu.

iOS 10 uvede nové způsoby řízení zapojení do aplikace tím, že systém proaktivně poskytnout užitečné informace automaticky uživateli v příslušnou dobu. Stejně jako iOS 9 poskytuje možnost přidávat hloubkové hledání do aplikace pomocí funkce služby, aby Handoff a návrhy Siri (viz [nové rozhraní API pro vyhledávání](~/ios/platform/search/index.md)), s iOS 10 aplikace můžou zpřístupnit funkce, které lze zobrazit pro uživatele v systému v nástroji Následující umístění:

- Aplikace přepínači
- Zamykací obrazovce
- CarPlay
- Mapy
- Interakce Siri
- Návrhy QuickType

Aplikace zpřístupní tuto funkci do systému pomocí kolekce technologií, jako třeba `NSUserActivity`, webové značek, Spotlight jádra, MapKit, přehrávač médií a UIKit. Kromě toho tím, že poskytuje proaktivní návrhu podporu pro aplikace, získá hlubší integrace Siri zdarma.

## <a name="location-based-suggestions"></a>Návrhy na základě umístění

Nový iOS 10, `NSUserActivity` zahrnuje třídy `MapItem` vlastnost, která umožňuje vývojáři poskytnout informace o umístění, které je možné v jiném kontextu. Například pokud aplikace zobrazí recenze restaurace, vývojáře můžete nastavit `MapItem` vlastnost umístění restaurace, které je uživatel zobrazit v aplikaci. Pokud se uživatel do aplikace Maps, je automaticky dostupný restaurace umístění.

Pokud aplikace podporuje vyhledávání aplikace, může použít nové součásti adresu `CSSearchableItemAttributesSet` třídu k určení umístění, které uživatel může chtít navštívit. Nastavením `MapItem` vlastnost, ostatní vlastnosti jsou automaticky vyplněné.

Kromě nastavení `Latitude` a `Longitude` vlastnosti součásti adresy, je doporučeno, aby aplikace zadat `NamedLocation` a `PhoneNumbers` vlastnosti, takže Siri můžete zahájit volání do umístění.

## <a name="web-markup-based-suggestions"></a>Návrhy na základě webové značek

iOS 9 přidat do možnost přidání značka strukturovaných dat na webu, která vylepšuje obsah, který uživatelé uvidí ve výsledcích vyhledávání Spotlight a Safari (viz [hledání se webové značek](~/ios/platform/search/web-markup.md)). iOS 10 přidává možnost používat značky na základě umístění (například [PostalAddress služby Active Directory](http://schema.org/PostalAddress) podle definice [Schema.org](http://schema.org/)) můžete dále rozšířit uživatele. Například pokud uživatelé zobrazení umístění označen na webu, systém můžete navrhnout stejné umístění při otevření mapy.

## <a name="text-based-suggestions"></a>Textové návrhy

UIKit se rozšířila na iOS 10 zahrnout [TextContentType](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) vlastnost [UITextInputTraits](https://developer.apple.com/reference/uikit/uitextinputtraits) třídu k určení význam sémantického obsah v textová oblast. Tyto informace na místě a systému můžete obvykle automaticky vybere typ odpovídající klávesnice, zlepšení návrhy automatické opravy a proaktivně integrovat informace z jiných aplikací a webů.

Například, pokud uživatel zadá text do textového pole označené `UITextContentType.FullStreetAddress`, systém může naznačovat automaticky plnění pole s umístěním uživatel byl nedávno zobrazení.

## <a name="media-based-suggestions"></a>Médium na základě návrhy

Pokud aplikace hraje média pomocí [MPPlayableContentManager](https://developer.xamarin.com/api/type/MediaPlayer.MPPlayableContentManager/) rozhraní API, iOS 10 uživatelům umožňuje zobrazit obrázek alba a hrát médií prostřednictvím aplikace na zamykací obrazovce.

## <a name="contextual-siri-reminders"></a>Kontextová Siri připomenutí

Umožňuje uživatelům používat Siri a rychle zajistit zobrazí se připomenutí pro zobrazení obsahu, že aktuálně nalezli jsou v aplikaci později. Například pokud kontrolu restaurace jejich byly zobrazení v aplikaci, se může vyvolat Siri a vyslovte *"Připomenout o to při doručení Domů."* Siri by vygeneroval upozornění se zobrazí odkaz na kontrola v aplikaci.

## <a name="contact-based-suggestions"></a>Obraťte se na základě návrhy

Umožňuje aplikaci pro kontakty (a kontaktní údaje související), než se objeví v **obraťte se na** aplikace na zařízení s iOS společně s všechny kontakty uživatele uložených v systému.

## <a name="ride-sharing-based-suggestions"></a>Pravé sdílení na základě návrhy

Pokud aplikace se pravé sdílení využívá [MKDirectionsRequest](https://developer.xamarin.com/api/type/MapKit.MKDirectionsRequest/) rozhraní API, iOS 10 nabídne ho jako možnost v přepínači aplikace v době, kdy uživatel je pravděpodobně vhodné pravé. Aplikace musí být také zaregistrovaný jako aplikaci sdílení pravé zadáním `MKDirectionsModeRideShare` pro [MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) klíče v jeho `Info.plist` souboru.

Pokud aplikace podporuje pouze pravé sdílení, by začínat návrhu systému *"Get pravé k..."*, pokud jsou podporovány další typy směrování směru (například vycházkové nebo kolo), bude používat systém *"Pokynů k zjištění..."*

> [!IMPORTANT]
> [MKMapItem](https://developer.xamarin.com/api/type/MapKit.MKMapItem/) objekt, který přijme aplikace nesmí obsahovat informace o zeměpisné šířky a délky a bude vyžadovat geografické kódování.

## <a name="implementing-proactive-suggestions"></a>Implementace proaktivní návrhy

Přidání proaktivní návrhu je obvykle stejně snadná jako implementace několik rozhraní API nebo rozšíření na několik rozhraní API, které aplikace může být implementace již podporu, aby aplikace Xamarin.iOS.

Proaktivní návrhy pracovat s aplikací třemi hlavními způsoby:

- **`NSUserActivity` a Schema.org**  -  `NSUserActivity` pomáhá pochopit, jaké informace uživatel aktuálně pracuje s na obrazovce systému. Schema.org podobné dalo přidá do webové stránky.
- **Umístění návrhy** – Pokud je aplikace nabízí nebo využívá informace o umístění na základě, tyto nové způsoby rozhraní API rozšíření nabídky sdílet tyto informace napříč aplikací.
- **Návrhy aplikace média** – systém může podporovat aplikace a její obsah média na základě kontextu interakce uživatele s zařízení s iOS.

A je podporováno v aplikaci implementací následující:

- **Aby handoff**  -  `NSUserActivity` byl přidán v iOS 8 pro podporu, aby Handoff, který umožňuje vývojáři spusťte aktivitu na jednom zařízení, a pokračovat na jiném (najdete v části [Úvod k předání](~/ios/platform/handoff.md)).
- **Bodové vyhledávání** -iOS 9 přidání možnosti zvýšit úroveň obsah aplikace v nástroji výsledky vyhledávání Spotlight pomocí `NSUserActivity` (najdete v části [hledání se základní Spotlight](~/ios/platform/search/corespotlight.md)).
- **Kontextová připomenutí Siri** – v iOS 10, `NSUserActivity` rozšířila umožní aplikaci Siri získat rychle Ujistěte se, zobrazí se připomenutí pro zobrazení obsahu uživatel aktuálně zobrazení v aplikaci později.
- **Umístění návrhy** -iOS 10 zlepšuje `NSUserActivity` zaznamenat umístění zobrazit v aplikaci a podporovat jejich na mnoha místech v systému.
- **Kontextová požadavky Siri**  -  `NSUserActivity` poskytuje kontext, který má informace zobrazené v aplikaci Siri tak, aby uživatel můžete získat pokynů nebo místní volání být volajícím Siri z aplikace.
- **Obraťte se na interakce** – nové v iOS 10, `NSUserActivity` umožňuje aplikacím komunikace povýšit z karty kontaktu (v aplikaci kontakty) jako metodu alternativní komunikace.

Všechny tyto funkce mají jedno, že jsou všechny používané `NSUserActivity` v jednoho formuláře nebo jiné zajistit jejich funkce. 

## <a name="nsuseractivity"></a>NSUserActivity

Jak jsme uvedli výše, `NSUserActivity` pomáhá pochopit, jaké informace uživatel aktuálně pracuje s na obrazovce systému. `NSUserActivity` je stav šedé – ukládání do mezipaměti mechanismus pro zachycení činnost uživatele procházet aplikace. Například prohlížení restaurace aplikace:

[![](proactive-suggestions-images/activity02.png "Šedé – stav NSUserActivity mechanismus ukládání do mezipaměti")](proactive-suggestions-images/activity02.png#lightbox)

Pomocí následujících interakce:

1. Jako uživatel pracuje s aplikací, `NSUserActivity` vzniká, chcete-li obnovit stav aplikace později.
2. Pokud uživatel hledá restaurace, a pod ním stejný vzor vytváření aktivit.
3. A znovu, když uživatel prohlíží výsledku. V tomto případě poslední uživatel prohlíží umístění a iOS 10, systém je víc paměti některé pojmy (jako je například umístění nebo komunikace interakce).

Prohlédněte si blíže poslední obrazovky:

[![](proactive-suggestions-images/activity03.png "Podrobnosti o NSUserActivity")](proactive-suggestions-images/activity03.png#lightbox)

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

Vývojář muset zajistit toto je stejný typ identifikátor aktivity (`com.xamarin.platform`) jako aktivita vytvořili výše. Aplikace využívá informace uložené v `NSUserActivity` k obnovení stavu zpět do tam, kde uživatel přestali.

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
2. Jako uživatel přesune mimo aplikaci pomocí přepínači aplikace multitasking, systém se automaticky zobrazí zlepšení (v dolní části obrazovky) Chcete-li získat pokyny k restaurace pomocí svých oblíbených navigační aplikace.
3. Pokud uživatel přepne do aplikace zprávy a začne psát *"Můžeme splňovat v"*, klávesnice QuickType automaticky navrhne vkládání v adrese restauraci.
4. Pokud se uživatel do aplikace Maps, restaurace adres automaticky navržený jako cíl.
5. To funguje i pro aplikace, 3. stran (podporující `NSUserActivity`), takže může uživatel přepínat do aplikace pro sdílení pravé a jako cíl existuje restaurace adres automaticky navržený také.
6. Také poskytuje kontext Siri, takže uživatel může vyvolat Siri v rámci aplikace restaurace a požádejte *"Get pokynů..."* a Siri poskytuje pokyny k restaurace, uživatel je zobrazení.

Všechny výše uvedené funkce je jednou z věcí, všechny indikují kde návrhy původně pochází z. V případě výše uvedeném příkladu je fiktivní restaurace revize aplikace.

Chcete-li povolit tuto funkci pro aplikaci prostřednictvím několika malé změny a dodatky do existujících architektur se rozšířily iOS 10:

- `NSUserActivity` obsahuje další pole pro zaznamenání informace o umístění, které se zobrazí uvnitř aplikace.
- Několik přidané jste provedli MapKit a CoreSpotlight k zachycení umístění.
- Umístění vědět funkce přidaná Siri, mapy, klávesnice, Multitasking a dalších aplikací v rámci systému.

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

Pak je potřeba pro text na základě instance (například klávesnici QuickType) popis text na základě umístění:

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Zeměpisné šířky a délky jsou nepovinné, ale ujistěte se, že uživatel se směruje na přesné umístění aplikace je chtějí odesílal, takže by měl být zahrnutý:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Nastavením telefonní čísla aplikaci získat přístup k Siri, proto může uživatel vyvolat Siri z aplikace vyslovením něco podobného jako, *"Volání toto místo"*:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Nakonec aplikaci může naznačovat, pokud instance je vhodný pro navigaci a telefonních hovorů:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>Implementace interakce kontaktu

Nové v iOS 10 komunikaci aplikace jsou integrováno do aplikace kontakty z karty kontaktu. Pro aplikace, které byla implementována interakce kontaktu uživatel může přidávat informace o dané aplikaci konkrétním lidem v jejich kontakty. A Pokud například stiskněte tlačítko rychlé akce v horní části karty Odeslat zprávu, připojené aplikaci bude uvedený jako možnost odeslat zprávu.

Pokud je vybrána 3rd aplikace jiných výrobců, můžete zapamatovaných a zobrazí jako výchozí způsob zprávy příštím chce uživatel o kontaktování dané osoby.

Interakce kontaktu jsou implementované v aplikaci pomocí `NSUserActivity` a nové architektury záměry byla zavedená v iOS 10. Další informace o práci s záměry, najdete v tématu naše [Principy SiriKit koncepty](~/ios/platform/sirikit/understanding-sirikit.md) a [implementace SiriKit](~/ios/platform/sirikit/implementing-sirikit.md) příručky.

#### <a name="donating-interactions"></a>Mohou interakce

Podívejte se na tom, jak můžete aplikaci darovat interakce:

[![](proactive-suggestions-images/activity04.png "Mohou interakce – přehled")](proactive-suggestions-images/activity04.png#lightbox)

Vytvoří aplikaci `INInteraction` objekt, který obsahuje **záměr** (`INIntent`), **účastníky** a **Metadata**. **Záměr** představuje akci uživatele jako video volání nebo odeslání textové zprávy. **Účastníky** zahrnují osoby přijímání komunikace. **Metadata** definuje informace například úspěšně odeslání zprávy atd.

Vývojář nikdy přímo vytvoří instanci `INIntent` nebo `INIntentResponse`, bude používat jeden konkrétní podřízenými třídami (podle úloh aplikace je splníte jménem uživatele), dědí tyto nadřazené třídy. Například `INSendMessageIntent` a `INSendMessageIntentResponse` pro odeslání textové zprávy. 

Po úplném naplnění interakce volání `DonateInteraction` metoda informovat systém, který interakce je k dispozici pro použití.

Když uživatel pracuje s aplikace z karty kontaktu, získá dodávat interakci s `NSUserActivity`, který je pak použitý ke spuštění aplikace:

[![](proactive-suggestions-images/activity05.png "Získá dodávat interakci s NSUserActivity, který se používá ke spuštění aplikace")](proactive-suggestions-images/activity05.png#lightbox)

Podívejte se na následující příklad záměru odeslat zprávu:

```csharp
using System;
using Foundation;
using UIKit;
using Intents;

namespace MonkeyNotification
{
    public class DonateInteraction
    {
        #region Constructors
        public DonateInteraction ()
        {
        }
        #endregion

        #region Public Methods
        public void SendMessageIntent (string text, INPerson from, INPerson[] to)
        {

            // Create App Activity
            var activity = new NSUserActivity ("com.xamarin.message");

            // Define details
            var info = new NSMutableDictionary ();
            info.Add (new NSString ("message"), new NSString (text));

            // Populate Activity
            activity.Title = "Sent MonkeyChat Message";
            activity.UserInfo = info;

            // Enable capabilities
            activity.EligibleForSearch = true;
            activity.EligibleForHandoff = true;
            activity.EligibleForPublicIndexing = true;

            // Inform system of Activity
            activity.BecomeCurrent ();

            // Create message Intent
            var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);

            // Create Intent Reaction
            var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);

            // Create interaction
            var interaction = new INInteraction (intent, response);

            // Donate interaction to the system
            interaction.DonateInteraction ((err) => {
                // Handle donation error
                ...
            });
        }
        #endregion
    }
}
```

Prohlížení tento kód podrobně, vytvoří a naplní instanci `NSUserActivity` (jak je znázorněno [vytváření aktivitu](#Creating-an-Activity) část výše). V dalším kroku vytvoří instanci `INSendMessageIntent` (který dědí z `INIntent`) a naplní s Podrobnosti zprávy odesílány:

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

`INSendMessageIntentResponse` Se vytvoří a předán `NSUserActivity` vytvořili výše:

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

`INInteraction` Vychází ze záměru odeslání zprávy (`INSendMessageIntent`) a odpovědi (`INSendMessageIntentResponse`) právě vytvořili:

```csharp
var interaction = new INInteraction (intent, response);
```

Nakonec systému je oznámení o interakci:

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
    // Handle donation error
    ...
});
```

Obslužná rutina dokončení, je předaná kde aplikace může reagovat odběru buď úspěšné nebo neúspěšné.

### <a name="activities-best-practices"></a>Osvědčené postupy aktivity

Apple navrhuje následující osvědčené postupy při práci s aktivitami:

- Použití `NeedsSave` aktualizací opožděné datové části.
- Zajistěte, aby silné odkaz na aktuální aktivity.
- Převod pouze malé datových částí, které obsahují právě dostatek informací k obnovení stavu.
- Zajistěte, aby identifikátory aktivit typu jedinečný a popisný pomocí DNS zpětného zápisu zadávat. 

## <a name="schemaorg"></a>Schema.org

Jako v příkladu nahoře, `NSUserActivity` pomáhá pochopit, jaké informace uživatel aktuálně pracuje s na obrazovce systému. Schema.org podobné dalo přidá do webové stránky.

Schema.org může poskytovat stejné typy umístění na základě interakce na web. Apple určená nové návrhy umístění fungovat stejně dobře, když zobrazit v prohlížeči Safari, stejně jako v nativní aplikaci.

Základní Schema.org:

- Poskytuje standard otevřete web značek termínů.
- Funguje díky včetně strukturovaných metadata na webových stránkách.
- Existuje více než 500 schémata představující různé koncepty, které jsou k dispozici.
- Implementací ho na webu můžete získat vývojář některé z výhod použití `NSUserActivity` v nativní aplikaci.

Schémata jsou uspořádány do stromové struktury jako struktura, kde konkrétní typy, jako *restaurace*, dědí více obecné typy, jako *místní obchodní*. Další informace najdete v tématu [Schema.org](http://schema.org).

Například, pokud webová stránka zahrnuty následující data:

```xml
<script type="application/ld+json>
{
    "@context":"http://schema.org",
    "@type":"Restaurant",
    "telephone":"(415) 781-1111",
    "url":"https://www.yanksing.com",
    "address":{
        "@type":"PostalAddress",
        "streetAddress":"101 Spear St",
        "addressLocality":"San Francisco",
        "postalCode":"94105",
        "addressRegion":"CA"
    },
    "aggregateRating":{
        "@type":"AggregateRating",
        "ratingValue":"3.5",
        "reviewCount":"2022"
    }
}
</script>
```

Pokud uživatel navštívil tuto stránku v prohlížeči Safari a potom přepnout do jiné aplikace, informace o umístění ze stránky by zachytit a nabídnut jako umístění návrhu v dalších částech tohoto systému (jak je vidět v aktivitách výše).

Safari extrahuje nic na webové stránce, odpovídající některý z následujících vlastností schématu:

- **PostalAddress služby Active Directory**
- **GeoCoordinates**
- Vlastnost telefonu.

Další informace najdete v tématu naše [hledání se webové značek](~/ios/platform/search/web-markup.md) průvodce.

## <a name="consuming-location-suggestions"></a>Využívání návrhy umístění

Této další části se bude zabývat využívání návrhu umístění, které pocházejí z dalších částí systému (například aplikace Maps) nebo jiné aplikace 3. stran.

Existují dva hlavní způsoby, kterými aplikace můžou využívat umístění návrhy:

- Z QuickType klávesnice.
- Přímo ve využívání informace o poloze ve směrování aplikace.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>Umístění návrhy a QuickType klávesnice

Pokud aplikace týká adresy ve formátu textu na základě, může aplikace využít výhod návrhy umístění prostřednictvím uživatelského rozhraní QuickType. iOS 10 rozšíří rozhraní QuickType s následující funkce:

- Aplikace můžete přidat nápovědu, jak sémantického záměr u textových polí v uživatelském rozhraní.
- Aplikaci můžete získat proaktivní návrhy v aplikaci.
- Aplikace může těžit z rozšířené automatické opravy.

Nové `TextContentType` vlastnosti ovládacích prvků pole text v iOS 10 umožňuje vývojáři definovat sémantického záměr pro hodnotu, která se bude uživatel do daného pole zadejte. Příklad:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

By oznamují systému, že aplikace očekává, že uživatelům do daného pole zadejte úplné adresu. To vám umožní klávesnice QuickType automaticky poskytnout umístění návrhy na klávesnici, když uživatel zadá hodnotu v tomto poli.

Toto jsou několik běžných typů, které jsou k dispozici pro vývojáře v `UITextContentType` statická třída:

* `Name`
* `GivenName`
* `FamilyName`
* `Location`
* `FullStreetAddress`
* `AddressCityAndState`
* `TelephoneNumber`
* `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>Směrování aplikace a návrhy umístění

V této části se podívejte se na využívání umístění návrhy přímo z aplikace směrování. Pro směrování aplikaci přidat tuto funkci, se vývojáři využít existující `MKDirectionsRequest` framework následujícím způsobem:

- Podporovat aplikace v Multitasking.
- Pro registraci aplikace jako směrování aplikaci.
- Pro zpracování spuštění aplikace s MapKit `MKDirectionsRequest` objektu.
- Udělit iOS možnost Další navrhovat aplikace pro uživatele v příslušnou dobu, založené na zapojení uživatele.

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

Nové v iOS 10 aplikace lze odeslat adresu, která nemá geo souřadnice, v tomto příčina vývojář musí ke kódování adresu:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="media-app-suggestions"></a>Návrhy aplikace média

Pokud aplikace zpracovává jakýkoli typ média, jako jsou aplikace podcastu nebo streamování mediální obsah, jako třeba zvuk nebo video s iOS 10, může trvat výhod návrhy média v systému.

Pro aplikace, které zpracovávají média podporuje iOS následující chování:

- iOS podporuje aplikace, které uživatel je pravděpodobně používat na základě jejich předchozí chování.
- Návrhy týkající se aplikace zobrazí v Spotlight a zobrazení dnes.
- Pokud aplikace splňuje jednu z následujících aktivačních událostí, může být zvýšena na zlepšení zámek obrazovky:
    - Po připojení v zařízení Bluetooth nebo sluchátka vytvoří připojení.
    - Po získání v automobilu.
    - Po přicházejících doma nebo práce. 

Zahrnutím jednoduché volání rozhraní API v iOS 10 může vývojář vytvořit více nestačí, aby zámek obrazovky prostředí pro uživatele aplikace média. Pomocí `MPPlayableContentManager` třídy ke správě přehrávání médium, úplné médium ovládací prvky (například ty předložený aplikace Hudba) zobrazí na obrazovce zámku pro aplikaci.


```csharp
using System;
using MediaPlayer;
using UIKit;

namespace MonkeyPlayer
{
    public class PlayableContentDelegate : MPPlayableContentDelegate
    {
        #region Constructors
        public PlayableContentDelegate ()
        {
        }
        #endregion

        #region Override methods
        public override void InitiatePlaybackOfContentItem (MPPlayableContentManager contentManager, Foundation.NSIndexPath indexPath, Action<Foundation.NSError> completionHandler)
        {
            // Access the media item to play
            var item = LoadMediaItem (indexPath);

            // Populate the lock screen
            PopulateNowPlayingItem (item);

            // Prep item to be played
            var status = PreparePlayback (item);

            // Call completion handler
            completionHandler (null);
        }
        #endregion

        #region Public Methods
        public MPMediaItem LoadMediaItem (Foundation.NSIndexPath indexPath)
        {
            var item = new MPMediaItem ();

            // Load item from media store
            ...

            return item;
        }

        public void PopulateNowPlayingItem (MPMediaItem item)
        {
            // Get Info Center and album art
            var infoCenter = MPNowPlayingInfoCenter.DefaultCenter;
            var albumArt = (item.Artwork == null) ? new MPMediaItemArtwork (UIImage.FromFile ("MissingAlbumArt.png")) : item.Artwork;

            // Populate Info Center
            infoCenter.NowPlaying.Title = item.Title;
            infoCenter.NowPlaying.Artist = item.Artist;
            infoCenter.NowPlaying.AlbumTitle = item.AlbumTitle;
            infoCenter.NowPlaying.PlaybackDuration = item.PlaybackDuration;
            infoCenter.NowPlaying.Artwork = albumArt;
        }

        public bool PreparePlayback (MPMediaItem item)
        {
            // Prepare media item for playback
            ...

            // Return results
            return true;
        }
        #endregion
    }
}
```

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých proaktivní návrhy a vám ukázal, jak vývojář lze využít přesměrovat provoz na aplikace Xamarin.iOS. Je popsaná v kroku k implementaci proaktivní návrhy a uvedené pokyny týkající se používání.



## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
- [Průvodce programováním SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
