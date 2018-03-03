---
title: "Změny architektury další iOS 9"
description: "Tento článek se zabývá další, méně závažné změny nebo vylepšení stávajících rozhraní pro iOS 9."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 82c6c2451deafb8a4314254a8138804d927c9bbf
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="additional-ios-9-frameworks-changes"></a>Změny architektury další iOS 9

_Tento článek se zabývá další, méně závažné změny nebo vylepšení stávajících rozhraní pro iOS 9._

[ ![](additional-framework-changes-images/ios9-sml.png "iOS 9 Logo")](additional-framework-changes-images/ios9.png)

Kromě hlavních změn do systému iOS má Apple provedené změny a vylepšení několik existujících architektur v iOS 9.

## <a name="av-foundation-framework-additions"></a>Přidání Framework AV Foundation

V rozhraní framework AV Foundation [AVSpeechSynthesisVoice](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechSynthesisVoice/) třída teď umožňuje určit hlasu identifikátorem kromě jazyka.

Například následující kód získá seznam všech dostupných hlasů:

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

Pak můžete jeden z hlasů ze seznamu nastavením jako `Voice` vlastnost instance [AVSpeachUtterance](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechUtterance/) třídy.

[AVQueuePlayer](https://developer.xamarin.com/api/type/AVFoundation.AVQueuePlayer/) třída nyní podporuje směs internet streamování a na základě souborů médií ve frontě. Předchozí verze může pouze fronty média stejného typu.

Další informace najdete v tématu společnosti Apple [AVSpeechSynthesisVoice odkaz](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice).

## <a name="avkit-framework-additions"></a>Přidání AVKit Framework

Pro práci s novou funkcí obraz v obraze (PIP), obsahuje nové rozhraní AVKit `AVPictureInPictureController` a [AVPlayerViewController](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) třídy:

- **AVPictureInPictureController** -Tato třída umožňuje aplikaci pro iOS 9 reagovat na uživatele spouštění přehrávání videa v okně PIP plovoucí, s možností změny velikosti v zařízení iPad.
- **AVPlayerViewController** -spravuje `AVPlayer` řadiče, používá se k předložení video v okně PIP plovoucí, s možností změny velikosti v zařízení iPad.

Další informace najdete v tématu naše [Multitasking pro iPad](~/ios/platform/introduction-to-ios9/index.md#multitasking) dokumentace a společnosti Apple [AVPictureInPictureController odkaz](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) a [AVPlayerViewController odkaz](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>Představení CloudKit webové služby

Rozhraní framework CloudKit zjednodušuje vývoj aplikací, že přístup k serveru služby iCloud. To zahrnuje načtení data aplikací a asset práva a také schopnost bezpečně uložit informace o aplikaci. Tato sada poskytuje uživatelům vrstvu anonymity tím, že přístup k aplikacím s jejich Icloudu ID bez sdílení osobní údaje.

Nové _CloudKit webové služby_ framework poskytuje knihovna JavaScript (CloudKit JS), který může být součástí vašeho webu k poskytovat přístup ke stejným datům CloudKit na základě a obsah jako aplikace Xamarin.iOS.

> [!IMPORTANT]
> **Poznámka:** před přístup, k dispozici nebo aktualizovat obsah v databázi CloudKit pomocí CloudKit JS, musí mít již definovaných schéma této databáze.




Další informace najdete v následujících dokumentech:

- [Úvod do CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) -naše základní informace o použití CloudKit v aplikaci pro Xamarin.iOS.
- [Rychlý Start CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) -společnosti Apple Úvod do CloudKit.
- [Odkaz JS CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) -CloudKit JS dokumentaci společnosti Apple.
- [Odkaz na CloudKit webové služby](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240) -společnosti Apple odkaz, který popisuje rozhraní HTTP CloudKit.
- [Katalog CloudKit: Úvod do CloudKit (kakao a JavaScript)](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) – ukázková aplikace společnosti Apple pomocí CloudKit a CloudKit JS.

## <a name="foundation-framework-additions"></a>Přidání Framework Foundation

Apple součástí následující změny rozhraní Foundation iOS 9:

### <a name="changes-to-nsbundle"></a>Změny NSBundle

Byly provedeny následující změny [NSBundle](https://developer.xamarin.com/api/type/Foundation.NSBundle/) třídu pro iOS 9:

* `GetPreservationPriorityForTag (NSString tag)` -Získá aktuální písmen a zachovávání s prioritou o prostředky s danou značku. Platné hodnoty jsou v rozsahu `0.0` k `1.0`, prostředky s nejnižší prioritou se vyprázdní nejdřív.
* `SetPreservationPriorityForTag (double priority, NSSet tags)` -Nastaví aktuální prioritu písmen a zachovávání s prostředky s danou značky. Platné hodnoty jsou v rozsahu `0.0` k `1.0`, prostředky s nejnižší prioritou se vyprázdní nejdřív.

Další informace najdete v tématu společnosti Apple [NSBundle odkaz](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle).

### <a name="changes-to-nsprocessinfo"></a>Změny NSProcessInfo

Každý proces, který běží na zařízení s iOS je jedinou _agenta informace o procesu_ (PIA –). Použití [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) třídy poskytují informace o aktuální PIA – a řízení spotřeby a teplotní správy pro daný proces.

Například pro ovládání automatické ukončení procesu můžete použít následující kód:

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

Další informace najdete v tématu společnosti Apple [NSProcessInfo odkaz](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo).

### <a name="reacting-to-low-power-mode"></a>Reagovat na režim snížené spotřeby energie

Použití `LowPowerModeEnabled` vlastnost [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) třídu k určení, pokud byl v zařízení s iOS, která aplikace běží na povolen režim napájení nízká. Příklad:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>Změny HealthKit Framework

Apple zahrnuty následující změny [HealthKit](https://developer.xamarin.com/api/namespace/HealthKit/) framework v iOS 9:

- Podpora pro hromadné odstranění a sledování odstranění položek v databázi HealthKit. Najdete v článku společnosti Apple [HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject), [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) a [referenci třídy HKHealthStore](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) Další informace.
- Nové kategorie sledování a vlastnosti byly přidány do `HKQuantityTypeIdentifier` – třída (například `UVExposure`) a `HKCategoryTypeIdentifier` – třída (například `OvulationTestResult`). Najdete v článku společnosti Apple [HealthKit konstanty odkaz](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html#//apple_ref/doc/uid/TP40014710) Další informace.

Najdete v tématu naše [Úvod do HealthKit](~/ios/platform/healthkit.md) dokumentace pro další informace o práci s HealthKit v Xamarin.iOS.

## <a name="local-authentication-framework-changes"></a>Změny Framework místní ověřování

Apple zahrnuty následující změny [místní ověřování](https://developer.xamarin.com/api/namespace/LocalAuthentication/) framework v iOS 9:

- Pomocí `EvaluateAccessControl` a `EvaluatePolicy` metody [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) třída, teď můžete pokusy o opakované použití Touch ID odpovídá z předchozí odemknutí úspěšné.
- Umožňuje získat seznam prsty, které aktuálně registrovaný.
- Podpora pro sledování prstem je přidání nebo odebrání z ověřování.
- Možnost používat _kontext ověřování_ v řetězci klíčů volání a podpora pro vyhodnocení řízení přístup do řetězce klíčů jsou uvedené.
- Umožňuje zrušit výzvu uživatele z kódu.

Najdete v tématu naše [Úvod do Touch ID](~/ios/platform/touchid.md) dokumentace pro další informace o práci s Touch ID v Xamarin.iOS.

### <a name="lacontext-changes"></a>LAContext změny

Byly provedeny následující změny [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) třídu pro iOS 9:

- **TouchIdAuthenticationMaximumAllowableReuseDuration** -vrátí maximální množství času, který lze opětovně použít metodu ověřování touch ID.
- **EvaluatedPolicyDomainState** – získá nebo nastaví stav vyhodnotí zásady.
- **MaxBiometryFailures** -má byla odepsat v iOS 9.
- **TouchIdAuthenticationAllowableReuseDuration** získá nebo nastaví množství času, který lze opětovně použít metodu ověřování touch ID.
- **EvaluateAccessControl** – asynchronně vyhodnotí zásady ověřování.
- **Zneplatnit** -zruší platnost a daný touch ID ověřování.
- **IsCredentialSet** -vrátí `true` Pokud přihlašovací údaje jsou aktuálně nastavená.
- **SetCredentialType** nastaví zadaný typ přihlašovacích údajů.

Najdete v tématu společnosti Apple [LAContext odkaz](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) další podrobnosti.

## <a name="mapkit-framework-changes"></a>Změny MapKit Framework

Apple zahrnuty následující změny [MapKit](https://developer.xamarin.com/api/namespace/MapKit/) framework v iOS 9:

- MapKit teď poskytuje podporu pro spuštění mapy aplikace přímo do přenosu pokynů a dotazování přenosu odhad čas doručení (plánované) pomocí [MKLaunchOptions](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) a [MKDirections](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) třídy.
- Výsledky hledání vrácený MapKit a [CLGeocoder](https://developer.xamarin.com/api/type/CoreLocation.CLGeocoder/) třídy můžete zadat taky výsledek časové pásmo.
- Nyní můžete plně přizpůsobit poznámky mapy předložený vaše aplikace iOS pomocí `DetailCalloutAccessoryView` vlastnost [MKAnnotationView](https://developer.xamarin.com/api/type/MapKit.MKAnnotationView/) třídy.

Najdete v tématu naše [iOS mapy](~/ios/user-interface/controls/ios-maps/index.md) a [návod - zkoumat poznámky a překryvy v MapKit](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) dokumentace pro další informace o práci s mapy a poznámky v Xamarin.iOS a společnosti Apple [CLGeocoder odkaz](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) Další informace.

## <a name="passkit-framework-additions"></a>Přidání PassKit Framework

Apple zahrnuty následující změny [PassKit](https://developer.xamarin.com/api/namespace/PassKit/) framework v iOS 9:

- Dotykový identifikátor teď podporuje debetní úložiště a platební karta společně s Discover karty. Najdete v článku **platebních sítě** části společnosti Apple [referenci třídy PKPaymentRequest](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) Další informace.
- Z přímo v aplikaci Xamarin.iOS, můžete nyní přidat platebních sítě a karty vystavitelů Apple platit. Najdete v článku společnosti Apple [referenci třídy PKAddPaymentPassViewController](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) další podrobnosti.

Najdete v tématu naše [Úvod do PassKit](~/ios/platform/passkit.md) dokumentace pro další informace o práci s PassKit v Xamarin.iOS.

## <a name="safari-services-framework-additions"></a>Safari Services Framework doplňky

Apple zahrnuty následující změny [Safari služby](https://developer.xamarin.com/api/namespace/SafariServices/) framework v iOS 9:

- Teď můžete použít nové [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) třída při zobrazení webového obsahu v rámci aplikace Xamarin.iOS. Poskytuje možnost sdílet data webové stránky a soubory cookie v aplikaci Prohlížeč Safari a obsahuje několik funkcí na Safari (například čtečky a automatické vyplňování). [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) funkce **provádí** tlačítko, které vrátí uživatele do vaší aplikace, když jsou prohlédnete webového obsahu.

Protože [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) třída je přizpůsobené pro zobrazení jednotlivých stránek webového obsahu, měli byste zvážit použití nahradit některé [WKWebKit](https://developer.xamarin.com/api/type/WebKit.WKWebView/) nebo [UIWebView](https://developer.xamarin.com/api/type/UIKit.UIWebView/)ovládací prvky v rámci vaší stávající aplikace Xamarin.iOS.

### <a name="displaying-a-website"></a>Zobrazení webu

Následující kód je příkladem volání [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) z uvnitř jiného řadiče zobrazení:

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>Změny UIKit Framework

Apple má zahrnout mnoho vylepšení několik elementů [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) framework pro iOS 9. Následující části podrobně popisuje tyto změny.

### <a name="3d-touch-events"></a>3D Touch události

Nové iOS 9 a iPhone 6s a iPhone 6s Plus, přidá 3D Touch citlivé gesta přetížení do aplikace pro iOS. Výsledkem je, pokud vaše aplikace běží v systému iOS 9 (nebo vyšší) a zařízení s iOS je schopen podpůrné 3D dotykového ovládání, změny v přetížení způsobí, že `TouchesMoved` událost, která má být vyvolána. 

Z důvodu této změny v chování, by měly být připraveny aplikace pro iOS `TouchesMoved` událost, která má být volána častěji, i v případě X / Souřadnice Y nezměnily. 

Další informace najdete v tématu naše [Úvod do 3D Touch](~/ios/platform/3d-touch.md) průvodce.

### <a name="document-open-in-place-functionality"></a>Funkce dokument otevřít na místě

Pomocí `FinishedLaunching (application, launchOptions)` nebo `WillFinishLaunching (Application, launchOptions)` metody [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) třída, teď můžete otevřít dokument a upravit ho na místě (na rozdíl od práce na kopii).

Chcete-li podporovat nové funkce otevřete na místě, přidejte `LSSupportsOpeningDocumentsInPlace` klíče do vaší aplikace Xamarin.iOS **Info.plist** soubor s hodnotou `YES`.

Najdete v tématu společnosti Apple [UIApplicationDelegate odkaz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) další podrobnosti.

### <a name="enhanced-touch-events"></a>Rozšířené Touch události

Apple poskytl několik vylepšení Touch události v iOS 9. Mezi ně patří možnost používat Touch předpovědi a získat přístup k zprostředkující úpravy mezi aktualizacemi zobrazení.

Najdete v tématu společnosti Apple [událostí Handling Guide pro iOS](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) další podrobnosti.

### <a name="fetching-tailored-content"></a>Načítání šité na míru obsahu

Nové `NSDataAsset` třída umožňuje načíst obsah přizpůsobit paměti a grafické možnosti zařízení s iOS, která aktuálně běží na aplikace Xamarin.iOS.

### <a name="new-layout-anchors"></a>Nové kotvy rozložení

Nové `NSLayoutAnchor` a `NSLayoutDimension` rozložení ukotvení třídy pracovat nové vlastnosti ukotvení [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) – třída (například `LeadingAnchor` a `WidthAnchor`) usnadnění rozložení v iOS 9.

Najdete v tématu naše [Úvod do Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) dokumentace pro další informace o práci s automatické rozložení a velikost třídy v aplikaci Xamarin.iOS a společnosti Apple [NSLayoutAnchor odkaz](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor), [ Odkaz na NSLayoutDimension](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) a [UIView odkaz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) Další informace.

### <a name="new-readable-content-margins"></a>Nové čtení obsahu rozpětí

Nové `UILayoutGuide` třídu lze použít k poskytování čtení obsahu okraje a definovat kreslení oblasti pro obsah v rámci zobrazení. Najdete v článku společnosti Apple [UILayoutGuide odkaz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) Další informace.

### <a name="text-input-in-notifications-modifications"></a>Zadávání textu v oznámení změny

[UIUserNotificationAction](https://developer.xamarin.com/api/type/UIKit.UIUserNotificationAction/) třída má nový `Behavior` vlastnost, která lze použít pro podporu zadávání textu z oznámení.

### <a name="uiapplicationdelegate-changes"></a>UIApplicationDelegate změny

Při zastaralé není oficiálně společností Apple, indikovaly nahrazení všechna volání `FinishedLaunching (UIApplication application)` metodu [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) se buď `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` nebo `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` metody.

Najdete v tématu společnosti Apple [UIApplicationDelegate odkaz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) další podrobnosti.

### <a name="uikit-dynamics-changes"></a>UIKit Dynamics změny

Apple součástí následující změny UIKit Dynamics iOS 9:

- Dynamics teď poskytuje podporu pro obdélníkový kolizí hranice.
- Nové, přizpůsobitelné `UIFieldBehavior` třída se používá pro podporu různých typů polí.
- Další přílohy typy jsou přidané do `UIAttachmentBehavior` třídy.

Najdete v tématu společnosti Apple [UIAttachment odkaz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) další podrobnosti.

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView a UIDatePicker změny

Před iOS 9 [UIPickerView](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) a [UIDatePicker](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/) ovládací prvky byly bez umožňující změnu velikosti a bude automaticky přizpůsobit celou šířku jejich kontejneru (obvykle šířku zařízení s iOS byla aplikace systémem).

V iOS 9 Tato automatická změna velikosti již proběhne a ovládací prvky bude vykreslen v šířka 320 bodu na všech zařízeních s iOS, bez ohledu na velikost obrazovky a orientace.

Chcete-li tuto situaci, pomocí automatického rozložení a velikost třídy připnout šířku ovládacího prvku k okrajům nadřazený kontejner (zobrazení) a zadejte požadované výšku. Najdete v tématu naše [Úvod do Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) dokumentace pro další informace o práci s automatického rozložení a velikost třídy v aplikaci pro Xamarin.iOS.

### <a name="new-uitextinputassistantitem-class"></a>Nová třída UITextInputAssistantItem

Použít novou `UITextInputAssistantItem` třída skupinám tlačítko panelu rozložení v _panel zástupců_. Je novou oblast, která je k dispozici v logicky klávesnice zajistit zadáním zástupce.



## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Úvod do iOSu 9](~/ios/platform/introduction-to-ios9/index.md)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [Co je nového v iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
