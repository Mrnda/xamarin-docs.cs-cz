---
title: Aby handoff
description: "Tento článek obsahuje práce aby Handoff v aplikaci Xamarin.iOS přenos aktivit uživatelů mezi aplikace běžící na uživatele, je jiná zařízení."
ms.topic: article
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 25220f37433037b55f13c4de5a07c0c09173a269
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="handoff"></a>Aby handoff

_Tento článek obsahuje práce aby Handoff v aplikaci Xamarin.iOS přenos aktivit uživatelů mezi aplikace běžící na uživatele, je jiná zařízení._

Společnost Apple vydala aby Handoff v iOS 8 a OS X Yosemite (10.10) k poskytování běžné mechanismus pro uživatele k přenosu aktivity spustit na jednom ze svých zařízení k jinému zařízení systémem stejné aplikaci nebo jinou aplikaci, která podporuje stejné aktivity.

[![](handoff-images/handoff02.png "Příklad provádění operace aby Handoff")](handoff-images/handoff02.png#lightbox)

Tento článek se rychle zobrazit v povolení sdílení v aplikaci Xamarin.iOS aktivity a zahrnují rozhraní aby Handoff podrobně:

## <a name="about-handoff"></a>O odložení

Aby handoff (také označované jako kontinuity) zavedl Apple v iOS 8 a OS X Yosemite (10.10) jako způsob, jak uživatelům aktivitu na jednom z jejich zařízení (iOS nebo Mac) a dál stejné aktivity na jiném zařízení (jako identifikovaný iClou uživatele d účtu).

Aby handoff byl v iOS 9 podporuje nový, taky rozšířit rozšířené možnosti hledání. Další informace najdete v tématu naše [vylepšení vyhledávání](~/ios/platform/search/index.md) dokumentaci.

Uživatele můžete například spustit e-mailu na jejich zařízení iPhone a bezproblémově pokračovat e-mailu v jejich Mac se všemi stejné informace zprávy vyplněno a kurzor ve stejném umístění, které budou ponechány v iOS.

Všechny aplikace, které sdílejí stejné _ID týmu_ jsou vhodné pro použití aby Handoff pokračoval v činnosti uživatele mezi aplikací, pokud jsou tyto aplikace buď doručit přes iTunes App Store nebo podepsaný registrované vývojáři (pro počítače Mac, Enterprise nebo Ad Hoc aplikace).

Všechny `NSDocument` nebo `UIDocument` aplikací mít automaticky aby Handoff integrovanou podporu a vyžadují k podpoře aby Handoff minimální změny.

### <a name="continuing-user-activities"></a>Trvalého aktivit uživatelů

`NSUserActivity` – Třída (spolu s některé malé změny `UIKit` a `AppKit`) poskytuje podporu pro definování aktivity uživatele, která lze potenciálně pokračovat na jiném zařízení uživatele.

Pro aktivity mají být předány jiného zařízení uživatele, musí být zapouzdřené v instanci `NSUserActivity`, označena jako _aktuální aktivita_, mají jeho datové sady (data použít k provedení pokračování) a Aktivita je třeba přenést potom na toto zařízení.

Aby handoff předá úplné minimum informace, které definujte aktivitu, chcete-li pokračovat, větší datových paketů, které jsou synchronizované prostřednictvím serveru služby iCloud.

Na přijímací zařízení uživatel dostane upozornění, že aktivita je k dispozici pro pokračování. Pokud uživatel vybere možnost pokračovat aktivity na nové zařízení, je zadaná aplikace spuštěná (pokud ještě není spuštěna) a datovou část z `NSUserActivity` se používá k restartu aktivity.

[![](handoff-images/handoffinteractions.png "Přehled trvalého aktivit uživatelů")](handoff-images/handoffinteractions.png#lightbox)

Jenom aplikace, které sdílejí stejné vývojáře ID týmu a reagovat na danou _typ aktivity_ jsou způsobilé pro pokračování. Aplikace definuje typy aktivit, které podporuje v části `NSUserActivityTypes` klíče z jeho **Info.plist** souboru. To zadána trvalého zařízení vybere aplikaci k provedení pokračování na základě ID týmu, typ aktivity a volitelně _název aktivity_.

Přijímající aplikace používá informace z `NSUserActivity`na `UserInfo` slovníku konfigurace svoje uživatelské rozhraní a obnovení stavu dané aktivity tak, aby zobrazila přechodu bezproblémové pro koncového uživatele.

Pokud pokračování vyžaduje více informací, než mohou být odesílány efektivně pomocí `NSUserActivity`, obnovení aplikace může odesílat volání výchozí aplikace a vytvořit jeden nebo více datových proudů přenést požadovaná data. Například pokud aktivita byla úpravy velké textový dokument s více bitových kopií, streamování by byly zapotřebí k přenosu informací potřebných k pokračovat aktivity na přijímací zařízení. Další informace najdete v tématu [datové proudy pokračování Supporting](#Supporting-Continuation-Streams) části níže.

Jak jsme uvedli výše, `NSDocument` nebo `UIDocument` aplikací mít automaticky aby Handoff integrovanou podporu. Další informace najdete v tématu [podpora aby Handoff v aplikace založené na dokument](#Supporting-Handoff-in-Document-Based-Apps) části níže.

### <a name="the-nsuseractivity-class"></a>NSUserActivity – třída

`NSUserActivity` Třída je objekt primární v systému exchange aby Handoff a se používá k zapouzdření stav aktivity uživatelů, který je k dispozici pro pokračování. Aplikace vytvoří instanci kopii `NSUserActivity` všechny aktivity se podporuje a chce pokračovat na jiném zařízení. Například dokument editoru by vytvořit aktivitu pro každý dokument aktuálně otevřené. Je však pouze přední krajní dokumentu (zobrazený v přední krajní okně nebo záložce) _aktuální aktivita_ a pro ně k dispozici pro pokračování.

Instance `NSUserActivity` je identifikována i jeho `ActivityType` a `Title` vlastnosti. `UserInfo` Slovníku vlastnost se používá k přenosu informací o stavu aktivity. Nastavit `NeedsSave` vlastnost `true` Pokud chcete na opožděné načíst informace o stavu prostřednictvím `NSUserActivity`je delegovat. Použití `AddUserInfoEntries` metoda sloučit nová data z jiných klientů do `UserInfo` slovníku požadovaný k zachování stavu aktivity.

### <a name="the-nsuseractivitydelegate-class"></a>NSUserActivityDelegate – třída

`NSUserActivityDelegate` Se používá pro zachování informací o `NSUserActivity`na `UserInfo` slovníku aktuální a synchronizována s aktuálním stavu aktivity. Když systém potřebuje informace v aktivitě aktualizovat (například před pokračování na jiném zařízení), zavolá `UserActivityWillSave` metoda delegáta.

Je nutné implementovat `UserActivityWillSave` metoda a zkontrolujte všechny změny `NSUserActivity` (například `UserInfo`, `Title`atd) zajistit, že stále zobrazuje stav aktuální aktivita. Při volání systému `UserActivityWillSave` metody `NeedsSave` příznak budou smazány. Pokud změníte data vlastnosti aktivity, budete muset nastavit `NeedsSave` k `true` znovu.

Místo použití `UserActivityWillSave` metoda popsané výše, můžete volitelně máte `UIKit` nebo `AppKit` spravovat aktivity uživatelů automaticky. K tomuto účelu nastavit objekt respondér `UserActivity` vlastnost a implementujte `UpdateUserActivityState` metoda. Najdete v článku [podpora aby Handoff v respondéry](#Supporting-Handoff-in-Responders) části níže Další informace.

### <a name="app-framework-support"></a>Podporu architektury aplikace

Obě `UIKit` (iOS) a `AppKit` (OS X) zadejte integrovanou podporu pro aby Handoff v `NSDocument`, respondér (`UIResponder`/`NSResponder`), a `AppDelegate` třídy. Při každém OS implementuje aby Handoff trochu jinak, základní mechanismus a rozhraní API jsou stejné.

#### <a name="user-activities-in-document-based-apps"></a>Aktivity uživatele v aplikacích na základě dokumentu

Na základě dokumentu iOS a OS X aplikace mají automaticky aby Handoff podporu předdefinované. K aktivaci této podpory, budete muset přidat `NSUbiquitousDocumentUserActivityType` klíč a hodnotu pro každou `CFBundleDocumentTypes` položku v aplikace **Info.plist** souboru.

Pokud tento klíč je k dispozici, i `NSDocument` a `UIDocument` automaticky vytvářet `NSUserActivity` instancí na základě Icloudu dokumenty zadaného typu. Budete muset zadat typ aktivity pro každý typ dokumentu, který podporuje aplikace a více typů dokumentů, můžete použít stejný typ aktivity. Obě `NSDocument` a `UIDocument` automaticky vyplnit `UserInfo` vlastnost `NSUserActivity` s jejich `FileURL` hodnotu vlastnosti.

V systému OS X `NSUserActivity` spravuje `AppKit` a související s respondéry automaticky aktuální aktivita, budou až okna dokumentu začne hlavní okno. V systému iOS pro `NSUserActivity` objekty, které spravuje `UIKit`, musíte buď volání `BecomeCurrent` metoda explicitně nebo dokumentu `UserActivity` vlastnost nastavte u `UIViewController` při aplikace pochází na popředí.

`AppKit` Všechny automatické obnovení `UserActivity` vlastnost vytvořené v tomto případě na OS X. K tomu dojde, pokud `ContinueUserActivity` metoda vrátí `false` nebo pokud neimplementované. V takovém případě otevření dokumentu se `OpenDocument` metodu `NSDocumentController` a poté se zobrazí `RestoreUserActivityState` volání metody.

Najdete v článku [podpora aby Handoff v aplikace založené na dokument](#Supporting-Handoff-in-Document-Based-Apps) části níže Další informace.

#### <a name="user-activities-and-responders"></a>Aktivity uživatelů a odpovídajících zařízení

Obě `UIKit` a `AppKit` můžete automaticky spravovat aktivity uživatelů, pokud je nastavena jako objekt respondér `UserActivity` vlastnost. Pokud stav byl upraven, budete muset nastavit `NeedsSave` vlastnost s metodou v `UserActivity` k `true`. Systém automaticky uloží `UserActivity` případě potřeby po poskytnutí času respondér aktualizovat stav voláním jeho `UpdateUserActivityState` metoda.

Pokud více respondéry sdílet jeden `NSUserActivity` instance, dostanou `UpdateUserActivityState` zpětného volání, když systém aktualizuje objekt aktivity uživatele. Odpovídající partner potřebuje volat `AddUserInfoEntries` metoda aktualizace `NSUserActivity`na `UserInfo` adresář k odráží aktuální stav aktivity v tomto okamžiku. `UserInfo` Slovník je zrušeno před každým `UpdateUserActivityState` volání.

Pokud chcete zrušit přidružení sám sebe z aktivity, respondér nastavit jeho `UserActivity` vlastnost `null`. Když představuje rozhraní aplikace spravované `NSUserActivity` instance nemá žádné další související respondéry nebo dokumenty, je automaticky zneplatněné.

Najdete v článku [podpora aby Handoff v respondéry](#Supporting-Handoff-in-Responders) části níže Další informace.

#### <a name="user-activities-and-the-appdelegate"></a>Aktivity uživatele a AppDelegate

Vaše aplikace `AppDelegate` při zpracování, aby Handoff pokračování je jeho primárním vstupním bodem. Když uživatel odpoví na oznámení, aby Handoff spuštění příslušné aplikace (pokud ještě není spuštěná) a `WillContinueUserActivityWithType` metodu `AppDelegate` bude volána. Aplikace v tomto okamžiku by měl informovat uživatele, který spouští pokračování.

`NSUserActivity` Instance doručuje, kdy `AppDelegate`na `ContinueUserActivity` metoda je volána. V tomto okamžiku by měl nakonfigurovat uživatelské rozhraní aplikace a pokračovat danou aktivitu.

Najdete v článku [implementace aby Handoff](#Implementing-Handoff) části níže Další informace.

## <a name="enabling-handoff-in-a-xamarin-app"></a>Povolení, aby Handoff v aplikaci Xamarin

Z důvodu požadavky na zabezpečení, aby Handoff způsobené aplikaci Xamarin.iOS, která používá rozhraní aby Handoff musí být správně nakonfigurované i na portálu pro vývojáře Apple a v souboru projektu Xamarin.iOS.

Postupujte takto:

1. Přihlaste se [portál pro vývojáře Apple](http://developer.apple.com).
2. Klikněte na **certifikáty, identifikátory a profily**.
3. Pokud jste tak již neučinili, klikněte na **identifikátory** a vytvoření ID pro vaši aplikaci (například `com.company.appname`), jinak upravit své existující ID.
4. Ujistěte se, že **Icloudu** služby bude zkontrolován pro dané ID: 

    [![](handoff-images/provision01.png "Povolení služby iCloud pro zadané ID.")](handoff-images/provision01.png#lightbox)
5. Uložte provedené změny.
4. Klikněte na **profily zřizování** > **vývoj** a vytvořte nový vývoj profil pro zřizování pro vás aplikace: 

    [![](handoff-images/provision02.png "Vytvořit nový vývoj zřizování profilu pro aplikaci")](handoff-images/provision02.png#lightbox)
5. Stáhnout a nainstalovat nový profil pro zřizování nebo pomocí Xcode stáhněte a nainstalujte profil.
6. Upravit možnosti projektu Xamarin.iOS a ujistěte se, že používáte profil zřizování, který jste právě vytvořili: 

    [![](handoff-images/provision03.png "Vyberte právě vytvořený profilu pro zřizování")](handoff-images/provision03.png#lightbox)
7. Potom upravte vaše **Info.plist** souboru a ujistěte se, že používáte ID aplikace, která byla použita k vytvoření profilu pro zřizování: 

    [![](handoff-images/provision04.png "ID sady aplikace")](handoff-images/provision04.png#lightbox)
8. Přejděte k položce **režimy pozadí** části a zkontrolujte následující položky: 

    [![](handoff-images/provision05.png "Povolit režimy požadované pozadí")](handoff-images/provision05.png#lightbox)
9. Uložte změny do všechny soubory.

S těmito nastaveními zavedené aplikace je nyní připraven k přístupu k rozhraním API Framework aby Handoff. Podrobné informace o zřizování, najdete v tématu naše [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) a [zřizování aplikace](~/ios/get-started/installation/device-provisioning/index.md) příručky.

## <a name="implementing-handoff"></a>Implementace aby Handoff

Aktivity uživatele lze pokračovat mezi aplikací, které jsou podepsané musí tentýž vývojář ID týmu a podporují stejný typ aktivity. Implementace aby Handoff v aplikaci Xamarin.iOS vyžaduje, abyste k vytvoření objektu aktivity uživatele (buď v `UIKit` nebo `AppKit`), aktualizujte stav objektu sledovat aktivity a pokračováním aktivity na přijímací zařízení.

### <a name="identifying-user-activities"></a>Identifikace aktivity uživatele

Prvním krokem v jeho implementaci, aby Handoff je identifikovat typy aktivit uživatelů, které vaše aplikace podporuje, a zobrazuje, který tyto aktivity jsou vhodnými kandidáty pro pokračování na jiném zařízení. Příklad: aplikace seznamu úkolů může podporovat úpravy položky jako jeden _typ aktivity uživatele_a podporují procházení seznam položky k dispozici jako jiné.

Aplikace můžete vytvořit tolik uživatele typy aktivit jsou povinné, jeden pro všechny funkce, která poskytuje aplikace. Pro každý typ aktivity uživatele aplikace bude nutné sledovat, kdy aktivita typu začne a končí, třeba spravovat informace o aktuální stavu pokračujte tuto úlohu na jiném zařízení.

Aktivity uživatele můžete pokračovat na všechny aplikace podepsané se stejným ID Team bez žádné mapování 1: 1 mezi přijímající a odesílající aplikací. Dané aplikace můžete například vytvořit čtyři různé typy aktivit, které se spotřebovávají jiný, jednotlivé aplikace na jiném zařízení. To je běžné v situaci mezi Mac verzí aplikace (který může mít mnoho funkcí a) a aplikací pro iOS, kde každá aplikace je menší a zaměřují se na konkrétní úkol.

### <a name="creating-activity-type-identifiers"></a>Vytváření identifikátory typu aktivity

_Identifikátor typu aktivity_ krátký řetězec přidán do `NSUserActivityTypes` pole aplikace **Info.plist** souboru, který slouží k jednoznačné identifikaci typu aktivity daného uživatele. V poli pro každou aktivitu, která podporuje aplikace bude jeden záznam. Apple navrhuje pomocí stylu DNS zpětného zápisu pro identifikátor typu aktivity, aby se zabránilo kolizí. Příklad: `com.company-name.appname.activity` pro konkrétní aplikaci na základě aktivity nebo `com.company-name.activity` pro aktivity, které můžete spustit mezi více aplikacemi.

Identifikátor typu aktivity se používá při vytváření `NSUserActivity` instance k identifikaci typu aktivity. Když aktivita je dál na jiném zařízení, typ aktivity (spolu s ID aplikace Team) určuje, které aplikace spustit pokračujte aktivity.

Jako příklad přidáme k vytvoření ukázkové aplikace volá **MonkeyBrowser** ([stáhnout zde](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)). Tato aplikace bude k dispozici čtyři karty, každý s jinou otevřete adresu URL ve webovém prohlížeči. Uživatel bude moci pokračovat v konkrétní kartě na zařízení iOS různých spuštění aplikace.

Chcete-li vytvořit požadovaný typ identifikátory aktivit pro podporu toto chování, upravte **Info.plist** souboru a přepněte do **zdroj** zobrazení. Přidat `NSUserActivityTypes` klíče a vytváření identifikátorů následující:

[![](handoff-images/type01.png "Klíč NSUserActivityTypes a požadované identifikátory v editoru plist.")](handoff-images/type01.png#lightbox)

Jsme vytvořili čtyři nový typ identifikátory aktivit, jeden pro každou z karet v příkladu **MonkeyBrowser** aplikace. Při vytváření vlastních aplikací, nahraďte obsah `NSUserActivityTypes` pole s typem identifikátory aktivit, které jsou specifické pro aktivity aplikace podporuje.

### <a name="tracking-user-activity-changes"></a>Sledování změn aktivity uživatele

Když vytvoříme novou instanci třídy `NSUserActivity` třída, budeme zadávat `NSUserActivityDelegate` instance sledovat změny stavu aktivity. Například kód postupujte podle lze sledovat změny stavu:

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;

namespace MonkeyBrowse
{
    public class UserActivityDelegate : NSUserActivityDelegate
    {
        #region Constructors
        public UserActivityDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void UserActivityReceivedData (NSUserActivity userActivity, NSInputStream inputStream, NSOutputStream outputStream)
        {
            // Log
            Console.WriteLine ("User Activity Received Data: {0}", userActivity.Title);
        }

        public override void UserActivityWasContinued (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity Was Continued: {0}", userActivity.Title);
        }

        public override void UserActivityWillSave (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity will be Saved: {0}", userActivity.Title);
        }
        #endregion
    }
}
```

`UserActivityReceivedData` Metoda je volána, když datový proud s pokračování přijme data ze zařízení odesílání. Další informace najdete v tématu [datové proudy pokračování Supporting](#Supporting-Continuation-Streams) části níže.

`UserActivityWasContinued` Metoda je volána, když jiné zařízení provedlo v návaznosti přes aktivitu z aktuální zařízení. V závislosti na typu aktivity, jako je přidání nové položky v seznamu ToDo může potřebovat aplikace abort aktivity na odesílání zařízení.

`UserActivityWillSave` Metoda je volána před všechny změny do aktivity jsou uložena a synchronizovat mezi zařízeními se místně k dispozici. Tuto metodu můžete použít k provádění jakýchkoli změn poslední minutu `UserInfo` vlastnost `NSUserActivity` instance před odesláním.

### <a name="creating-a-nsuseractivity-instance"></a>Vytvoření NSUserActivity Instance

Každá aktivita, která vaše aplikace, které chcete poskytnout možnost pokračovat na jiném zařízení musí být zapouzdřené v `NSUserActivity` instance. Aplikace můžete vytvořit tolik aktivity podle potřeby a povaze těchto aktivit je závislá na funkce a funkce daná aplikace. E-mailovou aplikaci může například vytvořit jednu aktivitu pro vytvoření nové zprávy a druhý pro čtení zprávy.

Pro náš příklad aplikaci novou `NSUserActivity` vytvoří pokaždé, když uživatel zadá novou adresu URL v jednom zobrazení na kartách webového prohlížeče. Následující kód ukládá stav daného karty:

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSUserActivity UserActivity { get; set; }
...

UserActivity = new NSUserActivity (UserActivityTab1);
UserActivity.Title = "Weather Tab";
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Vytvoří nový `NSUserActivity` pomocí jednoho typu aktivity uživatele vytvořili výše a poskytuje čitelný název aktivity. Připojí k instanci `NSUserActivityDelegate` vytvořený výše můžete sledovat změny stavu a iOS informuje, že tato aktivita uživatele je aktuální aktivita.

### <a name="populating-the-userinfo-dictionary"></a>Naplnění slovníku informací o uživateli

Jak jsme mohli vidět výše, `UserInfo` vlastnost `NSUserActivity` třída je `NSDictionary` párů klíč hodnota používá k definování stav danou aktivitu. Hodnotami uloženými v `UserInfo` musí mít jednu z následujících typů: `NSArray`, `NSData`, `NSDate`, `NSDictionary`, `NSNull`, `NSNumber`, `NSSet`, `NSString`, nebo `NSURL`. `NSURL` hodnoty dat, které odkazují na serveru služby iCloud dokumenty budou automaticky upravována tak, aby ukazovaly na stejné dokumenty na přijímací zařízení.

V předchozím příkladu jsme vytvořili `NSMutableDictionary` objektu a naplněný poskytnutí adresy URL, která uživatel byl aktuálně zobrazení na kartě daný jeden klíč. `AddUserInfoEntries` Metoda aktivity uživatelů byla použita k aktualizaci aktivity s daty, která se použije k obnovení aktivity na přijímací zařízení:

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple navrhnout udržuje informace odesílané do barest minimální zajistit, že se neodesílají aktivity včas přijímající zařízení. Pokud větší informace jsou nezbytné, jako jsou upravit bitovou kopii připojené k dokumentu musí být odeslána byste měli používat datové proudy pokračování. Najdete v článku [datové proudy pokračování Supporting](#Supporting-Continuation-Streams) části níže další podrobnosti.

### <a name="continuing-an-activity"></a>Pokračováním aktivitu

Aby handoff bude automaticky informovat místní iOS a OS X zařízení, která jsou v fyzické blízkosti původní zařízení a přihlášení k stejným účtem Icloudu dostupnosti může pokračovat aktivit uživatelů. Pokud uživatel vybere možnost pokračovat aktivitu na nové zařízení, příslušné aplikace (podle ID týmu a typ aktivity) a informace, spustí se systém jeho `AppDelegate` , musí dojít k pokračování.

Nejdřív `WillContinueUserActivityWithType` metoda je volána, takže aplikace může informovat uživatele, který o pokračování je začít. Můžeme použít následující kód v **AppDelegate.cs** souboru náš příklad aplikace pro zpracování pokračování spouštění:

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSString UserActivityTab2 = new NSString ("com.xamarin.monkeybrowser.tab2");
public NSString UserActivityTab3 = new NSString ("com.xamarin.monkeybrowser.tab3");
public NSString UserActivityTab4 = new NSString ("com.xamarin.monkeybrowser.tab4");
...

public FirstViewController Tab1 { get; set; }
public SecondViewController Tab2 { get; set;}
public ThirdViewController Tab3 { get; set; }
public FourthViewController Tab4 { get; set; }
...

public override bool WillContinueUserActivity (UIApplication application, string userActivityType)
{
    // Report Activity
    Console.WriteLine ("Will Continue Activity: {0}", userActivityType);

    // Take action based on the user activity type
    switch (userActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Inform view that it's going to be modified
        Tab1.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Inform view that it's going to be modified
        Tab2.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Inform view that it's going to be modified
        Tab3.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Inform view that it's going to be modified
        Tab4.PreparingToHandoff ();
        break;
    }

    // Inform system we handled this
    return true;
}
```

V předchozím příkladu každého řadiče zobrazení zaregistruje se `AppDelegate` a má veřejné `PreparingToHandoff` metoda, která zobrazuje indikátor aktivity a zprávy, takže uživatel vědět, že aktivita má být předáno do aktuálního zařízení. Příklad:

```csharp
private void ShowBusy(string reason) {

    // Display reason
    BusyText.Text = reason;

    //Define Animation
    UIView.BeginAnimations("Show");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0.5f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PreparingToHandoff() {
    // Inform caller
    ShowBusy ("Continuing Activity...");
}
```

`ContinueUserActivity` z `AppDelegate` bude volána k ve skutečnosti pokračovat danou aktivitu. Znovu z našich příklad aplikace:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

Veřejnosti `PerformHandoff` metoda každého řadiče zobrazení ve skutečnosti preforms předání a obnoví aktivity na aktuální zařízení. V případě příkladu zobrazuje stejnou adresu URL v dané kartu, která uživatel byl procházení na jiném zařízení. Příklad:

```csharp
private void HideBusy() {

    //Define Animation
    UIView.BeginAnimations("Hide");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PerformHandoff(NSUserActivity activity) {

    // Hide busy indicator
    HideBusy ();

    // Extract URL from dictionary
    var url = activity.UserInfo ["Url"].ToString ();

    // Display value
    URL.Text = url;

    // Display the give webpage
    WebView.LoadRequest(new NSUrlRequest(NSUrl.FromString(url)));

    // Save activity
    UserActivity = activity;
    UserActivity.BecomeCurrent ();

}
```

`ContinueUserActivity` Metoda zahrnuje `UIApplicationRestorationHandler` lze volat pro dokument nebo respondér na základě aktivity obnovení. Budete potřebovat k předávání `NSArray` nebo obnovitelné objekty do obslužné rutiny obnovení při volání. Příklad:

```csharp
completionHandler (new NSObject[]{Tab4});
```

Pro každý objekt předaný jeho `RestoreUserActivityState` volání metody. Každý objekt pak může použít data v `UserInfo` slovníku obnovit svůj stav. Příklad:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

Pro aplikace, které mezi sebou na základě dokumentu, pokud jste neimplementují `ContinueUserActivity` metoda nebo vrátí `false`, `UIKit` nebo `AppKit` můžete automaticky obnoví aktivity. Najdete v článku [podpora aby Handoff v aplikace založené na dokument](#Supporting-Handoff-in-Document-Based-Apps) části níže Další informace.

### <a name="failing-handoff-gracefully"></a>Aby Handoff řádně selhání

Vzhledem k tomu, aby Handoff využívá k přenosu informací mezi kolekce volně připojené iOS a OS X zařízení, proces přenosu může někdy selhat. Měli byste navrhnout vaše aplikace řádně zpracovávat tyto chyby a informovat uživatele všechny situace, které jsou vyvolány.

V případě selhání `DidFailToContinueUserActivitiy` metodu `AppDelegate` bude volána. Příklad:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

Měli byste použít zadaných `NSError` poskytnout informace o uživateli o selhání.

## <a name="native-app-to-web-browser-handoff"></a>Nativní aplikaci aby Handoff webové prohlížeče

Uživatel může chcete pokračovat aktivitu bez nutnosti odpovídající nativní aplikaci nainstalovat požadované zařízení. V některých situacích může webové rozhraní zadejte požadované funkce a aktivity lze i nadále pokračovat. Například e-mailový účet uživatele může poskytnout základní webového uživatelského rozhraní pro psaní a čtení zpráv.

Pokud původní, nativní aplikace zná adresu URL pro webové rozhraní (a požadované syntaxi pro identifikaci daná položka se dál), ho můžete kódovat tyto informace `WebpageURL` vlastnost `NSUserActivity` instance. Pokud přijímající zařízení nemá odpovídající nativní aplikaci, která je nainstalovaná pro zpracování pokračování, je možné volat zadané webové rozhraní.

## <a name="web-browser-to-native-app-handoff"></a>Webový prohlížeč, aby aby Handoff nativní aplikace

Pokud uživatel byl pomocí rozhraní založené na webu na původní zařízení a nativní aplikace na přijímací zařízení deklarací část domény `WebpageURL` vlastnost a potom systému budou používat tuto aplikaci popisovač pokračování. Nové zařízení obdrží `NSUserActivity` instance, který označuje typ aktivity jako `BrowsingWeb` a `WebpageURL` bude obsahovat adresu URL, které uživatel navštívil, `UserInfo` slovník bude prázdný.

Aplikace k účasti v tomto typu aby Handoff, se musí deklarace identity domény v `com.apple.developer.associated-domains` nárocích ve formátu `<service>:<fully qualified domain name>` (například: `activity continuation:company.com`).

Pokud zadaná doména odpovídá `WebpageURL` hodnotu vlastnosti, aby Handoff seznam schválených app ID soubory ke stažení z webu v této doméně. Web, musíte zadat seznam schválených ID v podepsaný soubor JSON s názvem **apple app lokality přidružení** (například `https://company.com/apple-app-site-association`).

Tento soubor JSON obsahuje slovník, který určuje seznam aplikací ID ve formátu `<team identifier>.<bundle identifier>`. Příklad:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

K podepsání souboru JSON (tak, aby měly správné `Content-Type` z `application/pkcs7-mime`), použijte **Terminálové** aplikace a `openssl` s certifikát a klíč vystavený certifikační autoritě důvěřují iOS (najdete v části [ http://support.Apple.com/kb/ht5012](http://support.apple.com/kb/ht5012) seznam). Příklad:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

`openssl` Příkaz výstupy podepsaného souboru JSON, můžete umístit na váš web v **apple app lokality přidružení** adresy URL. Příklad:

```csharp
https://example.com/apple-app-site-association.
```

Aplikace bude přijímat žádné aktivity jejichž `WebpageURL` doména je v jeho `com.apple.developer.associated-domains` nárok. Pouze `http` a `https` protokoly podpory, bude vyvolána výjimka, žádný jiný protokol.

## <a name="supporting-handoff-in-document-based-apps"></a>Podpora aby Handoff v aplikace založené na dokumentu

Jak jsme uvedli výše, na iOS a OS X na základě dokumentu aplikace bude automaticky podporují aby Handoff dokumentů na základě serveru služby iCloud Pokud aplikace **Info.plist** soubor obsahuje `CFBundleDocumentTypes` klíče z `NSUbiquitousDocumentUserActivityType`. Příklad:

```xml
<key>CFBundleDocumentTypes</key>
<array>
    <dict>
        <key>CFBundleTypeName</key>
        <string>NSRTFDPboardType</string>
        . . .
        <key>LSItemContentTypes</key>
        <array>
        <string>com.myCompany.rtfd</string>
        </array>
        . . .
        <key>NSUbiquitousDocumentUserActivityType</key>
        <string>com.myCompany.myEditor.editing</string>
    </dict>
</array>
```

V tomto příkladu je řetězec označení DNS zpětného aplikace s názvem aktivity připojí. Pokud zadali tímto způsobem, aktivity typ položky není potřeba v opakovat `NSUserActivityTypes` pole **Info.plist** souboru.

Objekt automaticky vytvoří aktivity uživatelů (k dispozici prostřednictvím dokumentu `UserActivity` vlastnost) můžete odkazují jiné objekty v aplikaci a použít k obnovení stavu v pokračování. Například ke sledování položek výběr a dokumentu umístit. Je nutné nastavit této aktivity `NeedsSave` vlastnost `true` vždy, když se změní stav a aktualizovat `UserInfo` slovníku v `UpdateUserActivityState` metoda.

`UserActivity` Vlastnost lze použít z libovolného vlákna a vyhovuje protokol klíč hodnota observing (KVO), takže jej může použít pro synchronizaci dokumentu při přesunu a odhlášení ze serveru služby iCloud. `UserActivity` Vlastnost budou neplatné, při zavření v dokumentu.

Další informace najdete v tématu společnosti Apple [podpora aktivity uživatelů v aplikacích na základě dokumentu](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) dokumentaci.

## <a name="supporting-handoff-in-responders"></a>Podpora aby Handoff v respondéry

Můžete přidružit respondéry (zděděno z buď `UIResponder` v systému iOS nebo `NSResponder` na OS X) pro aktivity nastavením jejich `UserActivity` vlastnosti. Systém automaticky uloží `UserActivity` vlastnost v příslušnou dobu volání s metodou v `UpdateUserActivityState` metody přidat aktuální data pomocí objektu aktivity uživatelů `AddUserInfoEntriesFromDictionary` metoda.

## <a name="supporting-continuation-streams"></a>Podpora pokračování datové proudy

Situacích, kdy nelze množství informací o pokračování aktivita vyžaduje přenést efektivně pomocí datové části počáteční aby Handoff může být. V těchto situacích přijímající aplikace může vytvořit jeden nebo více datového proudu mezi samostatně a původní aplikace k přenosu dat.

Nastaví výchozí aplikace `SupportsContinuationStreams` vlastnost `NSUserActivity` instance k `true`. Příklad:

```csharp
// Create a new user Activity to support this tab
UserActivity = new NSUserActivity (ThisApp.UserActivityTab1){
    Title = "Weather Tab",
    SupportsContinuationStreams = true
};
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Potom můžete volat přijímající aplikace `GetContinuationStreams` metodu `NSUserActivity` v jeho `AppDelegate` k vytvoření datového proudu. Příklad:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

Delegát aktivity uživatele v původním zařízení obdrží datové proudy voláním jeho `DidReceiveInputStream` metody můžete zajistit data požaduje, aby pokračoval v činnosti uživatelů na opětovné zařízení.

Budete používat `NSInputStream` zajistit přístup jen pro čtení k datům datového proudu a `NSOutputStream` poskytovat přístup jen pro zápis. Datové proudy je třeba použít způsobem žádosti a odpovědi, kde přijímající aplikace vyžaduje další data a aplikace původní poskytuje ji. Tak, aby data zapsaná do výstupního datového proudu na původní zařízení je číst ze vstupního proudu na trvalého zařízení a naopak.

I v situacích, kde jsou vyžadovány pokračování datového proudu, měla by existovat minimální zpět a stanovilo komunikaci mezi dvěma aplikacemi.

Další informace najdete v tématu společnosti Apple [datové proudy pokračování používání](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) dokumentaci.

## <a name="handoff-best-practices"></a>Aby handoff osvědčené postupy

Úspěšné dokončení implementace bezproblémové pokračování aktivity uživatelů prostřednictvím aby Handoff vyžaduje kvůli všechny různé součásti související se situací pečlivě návrhu. Apple naznačuje, postupujte podle kroků doporučených postupů pro vaše aplikace, aby Handoff povoleno přijetí:

- Návrh vaše aktivity uživatele tak, aby vyžadovala nejmenší možné stavu aktivity pokračovat se týkají datové části. Větší datové části, tím déle trvá pokračování spustit.
- Pokud musíte přenést velké objemy dat pro úspěšné pokračování, vezměte v úvahu náklady na součástí režijní náklady na konfiguraci a sítě.
- Je běžné pro velké aplikace Mac k vytvoření aktivity uživatelů, které jsou zpracovávány několik, menší, úloha konkrétní aplikace na zařízení s iOS. Jiné aplikace a verze operačního systému by se měly navrhovat dobře fungují nebo řádně selže.
- Při zadávání vaší typy aktivit, použijte zápis DNS zpětného předejdete kolizí. Pokud aktivita je specifická pro danou aplikaci, její název by měl být součástí definice typu (například `com.myCompany.myEditor.editing`). Pokud aktivita může fungovat mezi více aplikacemi, vyřadit z definice název aplikace (například `com.myCompany.editing`).
- Pokud vaše aplikace, musí se aktualizovat stav aktivity uživatelů (`NSUserActivity`) nastaven `NeedsSave` vlastnost `true`. V příslušnou dobu, aby Handoff zavolá delegáta `UserActivityWillSave` metoda, abyste mohli aktualizovat `UserInfo` slovníku podle potřeby.
- Vzhledem k tomu, aby Handoff proces nemusí okamžitě inicializovat na přijímací zařízení, měli byste implementovat `AppDelegate`na `WillContinueUserActivity` a informovat uživatele, který pokračování je spustit.

## <a name="example-handoff-app"></a>Příklad aby Handoff aplikace

Jako příklad použití aby Handoff v aplikaci Xamarin.iOS, jsme zahrnuli [ **MonkeyBrowser** ](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/) ukázkovou aplikaci v této příručce. Aplikace obsahuje čtyři karty, které může uživatel použít k procházení webu, každý s typem danou aktivitu: počasí, Oblíbené položky, kávy přerušení a práce.

Na kartě, když uživatel zadá nové adresy URL a odposlouchávání **přejděte** tlačítko Nový `NSUserActivity` se vytvoří pro tuto kartu, která obsahuje adresu URL, která je aktuálně procházení uživatele:

[![](handoff-images/handoff01.png "Příklad aby Handoff aplikace")](handoff-images/handoff01.png#lightbox)

Pokud má jiné zařízení uživatele **MonkeyBrowser** aplikace nainstalovaná, je přihlášení k serveru služby iCloud pomocí stejného uživatelského účtu, je na stejné sítě a v těsné blízkosti výše uvedených zařízení, aby Handoff aktivity se zobrazí na domovské stránce obrazovka (v levém dolním):

[![](handoff-images/handoff02.png "Aby Handoff aktivity zobrazené na domovské obrazovce, v levém dolním")](handoff-images/handoff02.png#lightbox)

Pokud uživatel nastavuje tažením směrem nahoru na ikonu aby Handoff, spustí se aplikace a aktivita uživatele zadané v `NSUserActivity` bude pokračovat na nové zařízení:

[![](handoff-images/handoff03.png "Aktivity uživatelů, pokračoval na nové zařízení")](handoff-images/handoff03.png#lightbox)

Když aktivita uživatele byla úspěšně odeslána do jiného Apple zařízení, odesílání zařízení `NSUserActivity` obdrží volání `UserActivityWasContinued` metoda na jeho `NSUserActivityDelegate` umožníte ho vědět, že aktivita uživatele úspěšně přenést do jiného zařízení.

## <a name="summary"></a>Souhrn

Tento článek udělil Úvod do rozhraní aby Handoff používá pokračujte aktivity uživatelů mezi několika zařízení Apple uživatele. V dalším kroku se vám ukázal, jak povolit a implementovat aby Handoff v aplikaci pro Xamarin.iOS. Nakonec popsané různé typy aby Handoff pokračování k dispozici a aby Handoff osvědčené postupy.



## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Ukázka MonkeyBrowser](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [Co je nového v iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Průvodce HomeKitDeveloper](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [Pokyny HomeKit uživatelské rozhraní](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit Framework – referenční informace](https://developer.apple.com/library/ios/home_kit_framework_ref)
