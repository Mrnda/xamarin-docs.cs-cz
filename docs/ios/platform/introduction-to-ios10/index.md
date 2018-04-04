---
title: Úvod do systému iOS 10
description: Tento článek představuje všechny nové a změněné rozhraní API a funkce dostupné v iOS 10 pro vývojáře Xamarin.iOS.
ms.prod: xamarin
ms.assetid: FB91DFFE-CF5E-4253-92CB-78A6371259D9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: c3bee0f15016394005a67e98cd8435e6d63b3ac6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-ios-10"></a>Úvod do systému iOS 10

_Tento článek představuje všechny nové a změněné rozhraní API a funkce dostupné v iOS 10 pro vývojáře Xamarin.iOS._

## <a name="introducing-ios-10"></a>Představení iOS 10

S novou iOS 10 SDK, Apple je k dispozici nové rozhraní API a služby, které umožňují vývojáři k vytvoření nové kategorie aplikace a funkce. Aplikace pro iOS teď můžete rozšířit zprávy, Siri, Telefon a mapy aplikace nabízí bohatou a nestačí, aby funkce pro koncového uživatele, který byl dříve k dispozici.

Další informace o iOS 10, najdete v tématu společnosti Apple [iOS + aplikace](https://developer.apple.com/ios/) dokumentaci.

## <a name="whats-new-in-ios-10"></a>Co je nového v iOS 10

Apple přidala několik nových rozhraní API a služby v iOS 10 společně s mnoho vylepšení stávajících funkcí, včetně:


## <a name="adapting-to-the-true-tone-display"></a>Přizpůsobení zobrazení True styl podání

Hodnota True, zobrazit styl podání technologie společnosti Apple používá senzoru okolního světla v zařízení se systémem iOS dynamicky upravit barvy a intenzitou displej tak, aby odpovídal aktuální osvětlení. poskytuje nové iOS 10 [UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31) klíč, který lze přidat do aplikace `Info.plist` souboru a řídí, jak True styl podání platí posun standardní barev. 

K dispozici jsou následující hodnoty:

- `UIWhitePointAdaptivityStyleStandard` **Výchozí** -použít standardní adaptivity white bodu.
- `UIWhitePointAdaptivityStyleReading` -Použít pro aplikace zaměřené na čtení.
- `UIWhitePointAdaptivityStyleGame` -Použít pro zaměřené na herní aplikace.
- `UIWhitePointAdaptivityStyleVideo` -Použít pro aplikace zaměřené na video.
- `UIWhitePointAdaptivityStylePhoto` -Použít pro aplikace zaměřené na fotografie kde je důležitější než prostředí white bod úpravy věrnosti barvu.

<a name="app-extensions" />

## <a name="app-extensions"></a>Rozšíření aplikace

Apple má k dispozici několik nových bodů aplikace rozšíření v iOS 10:

- Adresář volání
- Záměry a záměry uživatelského rozhraní
- Zprávy
- Obsah oznámení
- Oznámení služby
- Pack štítku

Kromě toho 3. stran klávesnice aplikace rozšíření má následující vylepšení:

- Nové `DocumentInputMode` vlastnost `UITextDocumentProxy` třídy můžete určit vstupní jazyk dokumentu a povolit rozšíření klávesnice vyrovnání v daném jazyce.
- Nové `HandleInputModeList` metoda umožňuje zobrazit systémové klávesnice výběr nabídky v reakci na celém světě klíč se stisknuté rozšíření klávesnice.

Další informace najdete v tématu naše [Úvod do rozšíření](~/ios/platform/extensions.md), [integrace aplikace zpráv](~/ios/platform/message-app-integration/index.md), [Úvod do proaktivní návrhy](~/ios/platform/search/proactive-suggestions.md), [ Úvod do SiriKit](~/ios/platform/sirikit/index.md), [Úvod do oznámení uživateli](~/ios/platform/user-notifications/index.md) a společnosti Apple [Průvodce programováním v rozšíření aplikace](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

## <a name="app-search-enhancements"></a>Vylepšení hledání aplikace

Základní Spotlight v iOS 10 nabízí několik vylepšení vyhledávání aplikace, jako:

- **Crowdsourcovaný přímý odkaz oblíbenosti (s rozdílovou o ochraně osobních údajů)** -poskytuje způsob, jak zvýšit úroveň obsah aplikace s přímým odkazem ve výsledcích hledání.
- **Hledání v aplikaci** -použít novou `CSSearchQuery` třídy zajišťuje možnosti vyhledávání Spotlight v aplikaci podobná fungování aplikace e-mailu, zprávy a poznámky.
- **Hledání pokračování** – umožňuje uživatelům spustit vyhledávání Spotlight nebo Safari a pak otevřete aplikaci a pokračovat v hledání.
- **Vizualizace výsledků ověření** -společnosti Apple [nástroj ověření rozhraní API pro vyhledávání aplikace](https://search.developer.apple.com/appsearch-validation-tool) nyní zobrazovat vizuální reprezentace webu značek a propojování přímým preforming testy.
- **Zpráva Image aplikace pro sdílení** -umožňuje oblíbených bitové kopie v aplikaci pro sdílení ve zprávách (prostřednictvím rozšíření aplikace zpráv), než se objeví ve vyhledávání Spotlight zadat.

Další informace, najdete v tématu naše [vylepšení vyhledávání aplikace](~/ios/platform/search/app-search-enhancements.md) průvodce.

## <a name="apple-pay-enhancements"></a>Vylepšení platím Apple

Apple udělal několik vylepšení Apple platit v iOS 10, které umožní uživatele, aby zabezpečené platby z webů a prostřednictvím interakci s Siri a mapy.

S iOS 10 několik nových rozhraní API přidané které pracují s iOS a watchOS pro podporu dynamické platebních sítí a nové testovacím prostředí izolovaného prostoru.

Kromě toho rozhraní PassKit se rozšířila na podporu dotykový identifikátor mimo `UIKit` a umožnit karty vystavitelů jejich karet z v rámci své aplikace k dispozici.

Další informace, najdete v tématu naše [vylepšení platit Apple](~/ios/platform/apple-pay.md) průvodce.

## <a name="alternate-app-icons"></a>Ikony alternativní aplikace

Apple přidala do systému iOS 10.3, které umožní aplikaci ke správě jeho ikonu několik vylepšení:

 - `ApplicationIconBadgeNumber` -Získá nebo nastaví oznámení "BADGE" ikony aplikace v můstek.
 - `SupportsAlternateIcons` -Pokud `true` má alternativní sadu ikony aplikace.
 - `AlternateIconName` -Vrátí název ikonu alternativní aktuálně vybrané nebo `null` při použití ikonu primární.
 - `SetAlternameIconName` -Použijte tuto metodu přepnout na ikonu dané alternativní ikona aplikace.

Další informace, najdete v tématu naše [alternativní ikony aplikace](~/ios/app-fundamentals/images-icons/alternate-app-icons.md) průvodce.


## <a name="introduction-to-callkit"></a>Úvod do CallKit

Nové rozhraní API CallKit v iOS 10 poskytuje způsob, jak aplikace VOIP integrovat iPhone uživatelského rozhraní a poskytují obeznámeni rozhraní a prostředí pro koncového uživatele. S tímto rozhraním API, uživatelé mohou zobrazit a pracovat s VOIP volání z zamykací obrazovce zařízení s iOS a spravovat kontakty pomocí aplikace Phone **Oblíbené** a **nedávno** zobrazení.

Rozhraní API CallKit navíc poskytuje možnost vytvářet přípony aplikace, která se dá přidružit telefonní číslo s názvem (ID volajícího) nebo zjistit, že v systému, pokud by měla být číslo blokované (volání blokování).

Další informace, najdete v tématu naše [Úvod do Callkit](~/ios/platform/callkit.md) průvodce.

## <a name="message-app-integration"></a>Integrace aplikace zpráv

iOS 10 umožňuje zahrnutí rozšíření aplikace zpráv v řešení Xamarin.iOS, který se integruje s **zprávy** aplikaci a uvede nové funkce pro uživatele. Rozšíření můžete odeslat text, štítky, souborů médií a interaktivní zprávy. K dispozici jsou dva typy rozšíření aplikace zpráva:

- **Balíčky štítku** – obsahuje kolekci štítky, které uživatel může přidávat do zprávy. Štítku balíčky lze vytvořit bez psaní jakéhokoli kódu.
- **iMessage aplikace** -může být vlastní uživatelské rozhraní v rámci aplikace zprávy pro výběr štítky, zadáním textu, včetně mediální soubory (s převody volitelné typu) a vytváření, úpravy a odesílání zpráv interakce.

Další informace, najdete v tématu naše [integrace aplikací zpráva](~/ios/platform/message-app-integration/index.md) průvodce.

## <a name="news-publisher-enhancements"></a>Vylepšení vydavatele zprávy

S iOS 10 bude Apple kdokoli z hlavní zásobníky a nových organizací bloggers a nezávislé vydavatelů se zaregistrujte a produktu a doručování obsahu do aplikace Apple zprávy. Další informace najdete v tématu společnosti Apple [zprávy prostředky](https://newsresources.apple.com/) dokumentaci.

## <a name="providing-haptic-feedback"></a>Poskytnutí zpětné vazby hmatová

Na zařízení iPhone 7 a iPhone 7 Plus Apple je k dispozici nové haptics odpovědi, které poskytují další způsoby, jak fyzicky zaujmout uživatele. Použijte nové možnosti taktilní zpětnou vazbu k získání pozornost uživatele a posilovat jejich akce.

Několik předdefinovaných prvky uživatelského rozhraní již svůj názor hmatová například ovládacích prvků výběr, přepínače a posuvníky. iOS 10 teď navíc umožňuje prostřednictvím kódu programu aktivovat haptics pomocí konkrétní podtřídou třídy `UIFeedbackGenerator` třídy.

Další informace, najdete v tématu naše [poskytuje hmatová zpětné vazby](~/ios/user-interface/ios-ui/haptic-feedback.md) průvodce.

## <a name="proactive-suggestions"></a>Proaktivní návrhy

iOS 10 uvede nové způsoby řízení zapojení do aplikace tím, že systém proaktivně poskytnout užitečné informace automaticky uživateli v příslušnou dobu. Stejně jako iOS 9 poskytuje možnost přidávat hloubkové hledání do aplikace pomocí funkce služby, aby Handoff a návrhy Siri s iOS 10 aplikace můžou zpřístupnit funkce, které lze zobrazit pro uživatele v systému z v následujících umístěních:

- Aplikace přepínači
- Zamykací obrazovce
- CarPlay
- Mapy
- Interakce Siri
- Návrhy QuickType 

Aplikace zpřístupní tuto funkci do systému pomocí kolekce technologií, jako třeba [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), webové značek, Spotlight jádra, MapKit, přehrávač médií a UIKit.

Další informace, najdete v tématu naše [Úvod do proaktivní návrhy](~/ios/platform/search/proactive-suggestions.md) průvodce.

## <a name="request-app-review"></a>Zkontrolujte žádost o aplikaci

Nový iOS 10.3, `RequestReview()` metoda umožní aplikaci pro iOS požádat uživatele o hodnocení nebo zkontrolujte ho. Když tuto metodu lze volat v libovolném bodě, kde má smysl v činnost koncového uživatele, je proces kontroly řídí a řešit zásady obchodu s aplikacemi. V důsledku toho tato metoda může nebo nemusí být zobrazeny výstrahy a nikdy by měla být volána v reakci na akci uživatele, jako je například klepnutím na tlačítko.

Další informace, najdete v tématu naše [žádosti o aplikace zkontrolujte](~/ios/platform/request-app-review.md) průvodce.

## <a name="security-and-privacy-enhancements"></a>Zabezpečení a ochrany osobních údajů vylepšení

Apple udělal několik vylepšení zabezpečení a ochrany osobních údajů v iOS 10, který vám pomůže vývojáře zlepšit zabezpečení své aplikace a zajištění ochrany osobních údajů koncového uživatele.

V důsledku toho je třeba deklarovat aplikace běžící v systému iOS 10 (nebo novější) staticky jejich pokus o přístup k určité funkce nebo informace o uživateli tak, že zadáte jednu nebo více o ochraně osobních údajů konkrétní klíče v jejich `Info.plist` soubory, které vysvětlují, uživateli, proč aplikace chce získat přístup.

Další informace, najdete v tématu naše [zabezpečení a ochrany osobních údajů vylepšení](~/ios/app-fundamentals/security-privacy.md) průvodce.

## <a name="sirikit"></a>SiriKit

Nové aplikace Xamarin.iOS k poskytování služeb, které jsou dostupné pro uživatele používání Siri na zařízení s iOS povoluje SiriKit iOS 10. Tato funkce je uvedený v jedné nebo více rozšíření aplikace pomocí nového **záměry** a **uživatelského rozhraní záměry** architektury.

SiriKit podporuje následující domény služby:

- Zvuk nebo video volání.
- Rezervace pravé.
- Správa cvičení nebo.
- Zasílání zpráv.
- Hledání fotografie.
- Odesílání nebo přijímání platby.

Když uživatel odešle žádost siri jeden rozšíření aplikace služby, odešle SiriKit rozšíření **záměr** objekt, který popisuje žádost uživatele spolu s podpůrné daty. Rozšíření aplikace vygeneruje odpovídající **odpovědi** objekt pro v dané **záměr**, s podrobnostmi o tom, jak můžete rozšíření žádost zpracovat.

Zatímco Siri obvykle zpracovává všechny zásahu uživatele, můžete použít rozšíření aplikace **uživatelského rozhraní záměr** framework nabídne bohatou a vlastní uživatelské rozhraní s obchod, kde je branding aplikace a další informace.

Další informace, najdete v tématu naše [Úvod do SiriKit](~/ios/platform/sirikit/index.md) průvodce.

## <a name="speech-recognition"></a>Rozpoznávání řeči

iOS 10 obsahuje nové rozhraní API rozpoznávání řeči, které umožňuje aplikaci rozpoznávání řeči průběžné podporovat a transcribe rozpoznávání řeči (z provozu nebo záznam zvuku datových proudů) do textu.

Protože rozpoznávání řeči vyžaduje přenos a dočasné úložiště dat na servery společnosti Apple, aplikace _musí_ žádala o oprávnění uživatele k provedení rozpoznávání zahrnutím `NSSpeechRecognitionUsageDescription` klíče v jeho `Info.plist` Souborová služba a volání `SFSpeechRecognizer.RequestAutorization` metoda.

Další informace, najdete v tématu naše [Úvod k rozpoznávání řeči](~/ios/platform/speech.md) průvodce.

## <a name="user-notifications"></a>Oznámení uživateli

Nový iOS 10, framework umožňuje doručení a zpracování místní a vzdálené oznámení oznámení pro uživatele. Pomocí toto rozhraní, aplikace nebo rozšíření aplikace můžete naplánovat doručování oznámení místní zadáním sadu podmínek, jako je například umístění nebo denní dobu.

Kromě toho aplikace nebo rozšíření může přijímat (a potenciálně upravit) místních i vzdálených oznámení dodaným do zařízení iOS uživatele.

Nové architektury uživatelského rozhraní oznámení uživatele umožňuje aplikaci nebo rozšíření aplikace k přizpůsobení vzhledu místních i vzdálených oznámení poté, co se zobrazí uživateli.

Další informace, najdete v tématu naše [Framework oznámení uživatele](~/ios/platform/user-notifications/index.md) průvodce.

## <a name="video-subscriber-account"></a>Účet video odběratele

Nové pro iOS 10 rozhraní Video odběratele účet umožňuje aplikacím dané podporu ověření streamování nebo vyžádání video-on-demand k ověření se svým poskytovatelem TV kabel nebo satelitní pomocí Single-Sign in prostředí pro koncového uživatele.

## <a name="wide-color"></a>Široké barev

iOS 10 rozšiřuje podporu pro rozšířené rozsah pixelů formáty a prostory celou rozsah, barvy v celém systému, včetně architektury, jako je například základní grafické prvky, základní Image, operačního systému a AVFoundation. Podpora pro zařízení s wide barev zobrazí se další opatřeny náběhem / tím, že poskytuje toto chování v rámci celého grafiky zásobníku.

Kromě toho [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) byla změněna pro práci v nové rozšířené **sRGB** barevný prostor, což usnadňuje kombinovat barev v rozsahů barev široké bez ztráty významně zvýšit výkon.

Apple nabízí následující osvědčené postupy při práci s wide barev:

- [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/) teď používá sRGB barvu místa a nadále nebude CLAMP – hodnoty `0.0` k `1.0` rozsahu. Pokud aplikace závisí na předchozí chování svorka, ji budou muset upravit pro iOS 10.
- Kreslení prostředí bude konfigurovat barevný prostor sRGB při provádění vlastní `UIView` kreslení na Ipadu sady Pro.
- Pokud aplikace provede vlastní vykreslování `UIImages`, použít novou [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) třídu k určení použití formátů rozšířené rozsah nebo standard-range.
- Při použití nízké úrovně rozhraní API, například základní grafické prvky nebo operačního systému k zajištění zpracování obrázků, vývojáři měli použít delší rozsah barva místa a pixelů formátu, který podporuje 16bitové hodnot s plovoucí desetinnou. V případě potřeby vývojář bude muset ručně CLAMP – hodnoty barev součásti.
- Základní grafické prvky, základní Image a shadery výkonu operačního systému všech poskytují nové metody pro převod mezi dvěma barevné prostory.

Další informace, najdete v tématu naše [Úvod do široké barva](~/ios/platform/wide-color.md) průvodce.

## <a name="widget-enhancements"></a>Vylepšení pomůcky

Společnost Apple má několik vylepšení systému pomůcky a ujistěte se, že widgetů hledat skvělé na všechny pozadí, která existuje v nové iOS 10 zamykací obrazovka vydala. [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) vlastnost je zastaralá a nahradila s novým [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) nebo [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect) vlastnosti. Kromě toho nyní obsahují pomůcky [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) vlastnost, která umožňuje vývojáři popisují, kolik obsah je dostupný a umožňuje uživateli rozbalit nebo sbalit obsah.

Další informace, najdete v tématu naše [vyhledávání a domů vylepšení pomůcky obrazovky](~/ios/platform/search/widgets.md) průvodce.

## <a name="additional-framework-changes"></a>Další Framework změny

Kromě hlavní framework změny a dodatky uvedených výše Apple udělal mnoho dalších menší framework změn v iOS 10.

Další informace, najdete v tématu naše [další změny Framework](~/ios/platform/introduction-to-ios10/additional-framework-changes.md) průvodce.

## <a name="deprecated-apis"></a>Zastaralá rozhraní API

Následující rozhraní API jsou zastaralé v iOS 10:

- `CKDiscoverAllContactsOperation`, `CKDiscoveredUserInfo`, `CKDiscoverUserInfosOperation` a `CKFetchRecordChangesOperation` třídy jsou zastaralé v CloudKit pro iOS 10. Použití [CKDiscoverAllUserIdentitiesOperation](https://developer.xamarin.com/api/type/CloudKit.CKDiscoverUserIdentitiesOperation/), [CKUserIdentity](https://developer.xamarin.com/api/type/CloudKit.CKUserIdentity/) a [CKFetchRecordZoneChangesOperation](https://developer.xamarin.com/api/type/CloudKit.CKFetchRecordZoneChangesOperation/) třídy (které podporují sdílení záznamu) místo.
- Několik [CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) rozhraní API (např. na základě zóny a na základě dotazů odběry) jsou zastaralé. Použití [CKRecordZoneSubscription](https://developer.xamarin.com/api/type/CloudKit.CKRecordZoneSubscription/) a [CKQuerySubscription](https://developer.xamarin.com/api/type/CloudKit.CKQuerySubscription/) rozhraní API místo.
- [NSPersistentStoreCoordnator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) symboly pro všudypřítomný obsahu jsou zastaralé.
- `ADBannerView`, `ADInterstitialAd` a související symboly v [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) třídy jsou zastaralé.
- [SKUniform](https://developer.apple.com/reference/spritekit/skuniform) symboly pro plovoucí hodnoty bodu jsou zastaralé.
- `UILocalNotification`, `UIMutableUserNotificationAction`, `UIMutableUserNotificationCategory`, `UIUserNotificationAction`, `UIUserNotificationCategory` a `UIUserNotificationSettings` třídy UIKit jsou zastaralé. Použití [oznámení uživateli](#User-Notifications) framework místo.
- `HandleActionForLocalNotification`, `HandleActionForRemoteNotification`, `DidReceiveLocalNotification` a `DidReceiveRemoteNotification` WatchKit metody jsou zastaralé. Použití `HandleActionForNotification` a `DidReceiveNotification` metody místo.
- `DidReceiveLocalNotification` a `DidReceiveRemoteNotification` metody [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) jsou zastaralé. Vytvoření instance [UNUserNotificationCenterDelegate](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate) , implementuje příslušné metody a přiřadit ji ke `Delegate` vlastnost [UNUserNotificationCenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter) objektu.
- **Aplikace herní Centrum** je zastaralý a odebrat z iOS. Pokud aplikace používá GameKit, ho _musí_ k dispozici vlastní rozhraní pro zobrazení GameKit funkcí, jako jsou tabulky, atd.

Najdete v článku společnosti Apple [iOS 9.3 iOS 10.0 API rozdíly](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html) dokumentaci úplný seznam vyřazení.



## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
- [Co je nového v iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
