---
title: "Úvod do watchOS 3"
description: "Tento článek představuje všechny nové a změněné rozhraní API a funkce dostupné v watchOS 3 pro vývojáře pro Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/07/2017
ms.openlocfilehash: f11db7496d132b742cb57b86ddea240712b09e34
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-watchos-3"></a>Úvod do watchOS 3

_Tento článek představuje všechny nové a změněné rozhraní API a funkce dostupné v watchOS 3 pro vývojáře pro Xamarin._

Tento dokument popisuje v následujících tématech:

- [Co je nového v watchOS 3](#Whats-New-in-watchOS-3)
    - [Vylepšení platit Apple](#Apple-Pay-Enhancements) přidává podporu pro plateb v aplikaci v Apple Watch.
    - [Úlohy na pozadí](#Background-Tasks) poskytují aplikace možnost aktualizovat informace o na pozadí tak, aby byl připraven, když uživatel potřebuje ho.
    - [Vylepšení komplikace](#Complications-Enhancements) byla pro watchOS 3, které poskytují nové funkce pro aplikace.
    - [Nově k dispozici rozhraní](#Newly-Available-Frameworks) mít vystavený pro watchOS aplikace.
    - [Proaktivní návrhy](#Proactive-Suggestions) umožňuje aplikaci proaktivně zobrazit informace o uživateli.
    * Několik [zabezpečení a ochrany osobních údajů vylepšení](#Security-and-Privacy-Enhancements) byly provedeny watchOS 3.
    - [Snímky a ukotvení](#Snapshots-and-Dock) uživateli poskytnout rychlý přístup k watchOS aplikace.
    - [Oznámení uživateli](#User-Notifications) poskytuje místních i vzdálených oznámení pro uživatele.
    * Několik [sledovat připojení Framework vylepšení](#Watch-Connectivity-Framework-Enhancements) byly provedeny v watchOS 3.
    * Několik [WatchKit Framework vylepšení](#WatchKit-Framework-Enhancements) byly provedeny v watchOS 3.
    - [Vylepšení aplikace cvičení](#Workout-App-Enhancements) poskytuje nové možnosti cvičení související Apple Watch aplikace.
- [Další změny Framework](#Additional-Framework-Changes) byly provedeny v rámci watchOS 3.
- [Zastaralá rozhraní API](#Deprecated-APIs) v watchOS 3.

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>Co je nového v watchOS 3

Apple přidala několik nových rozhraní API a služby v watchOS 3 společně s mnoho vylepšení stávajících funkcí, včetně:

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>Vylepšení platím Apple

V watchOS 3 rozhraní PassKit rozšířila povolit podporu pro zabezpečené, v aplikaci platby (z fyzického zboží a služeb) pro aplikace běžící na Apple Watch.

Použít novou [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) a [PKPaymentAuthorizationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) třídy k dispozici a reagovat na rozhraní, kde uživatel může autorizovat žádostí o platbu.

Další informace, najdete v tématu naše [vylepšení platit Apple](~/ios/watchos/platform/apple-pay.md) průvodce.

<a name="Background-Tasks" />

## <a name="background-tasks"></a>Úlohy na pozadí

watchOS 3 zavádí několik úlohy na pozadí, které aplikace můžete použít k aktualizovat informace o zajištění, že obsahuje obsah, uživatel musí před jeho otevřením.

Následující nové úlohy na pozadí jsou k dispozici:

- **Aplikace aktualizace na pozadí** – [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) úloha umožňuje aplikaci aktualizovat její stav na pozadí. Obvykle to bude zahrnovat jiné úlohy, jako je například stahování nový obsah z Internetu pomocí [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Na pozadí aktualizovat snímek** – [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) úloha umožňuje aplikaci aktualizovat jeho obsah a uživatelského rozhraní, než systém pořídí snímek, který se použije k naplnění ukotvení.
- **Sledování připojení na pozadí** – [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) úloha je spuštěna aplikace, když obdrží data na pozadí z spárované zařízení iPhone.
- **Adresa URL relace na pozadí** – [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) úloha je spuštěna aplikace při přenosu na pozadí vyžaduje ověření nebo dokončení (úspěšně nebo chyba).

Další informace, najdete v tématu naše [úlohy na pozadí](~/ios/watchos/platform/background-tasks.md) průvodce.

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>Vylepšení komplikace

Komplikace jsou malé vizuální prvky, které poskytují užitečné informace na první pohled. V závislosti na tučné sledovat vybrána uživatel má schopnost přizpůsobit vzhled sledovat s komplikace jeden nebo více.

watchOS 3 dává možnost vytvořit jeden nebo více komplikace pro sledování aplikace tak, aby má uživatel přístup z řez sledovat její informace na přehled aplikace.

Kromě toho komplikace poskytovat následující výhody:

- Uživatele můžete rychle spustit aplikaci klepnutím na komplikace přímo z sledovat řez.
- Mají jedno z aplikace komplikace příčiny vzhled sledovat systému zachovat aplikace ve stavu připraveno spuštění, kterých se pokusí spustit aplikaci na pozadí, uložte ho paměti a poskytuje velmi je čas aktualizace.
- Komplikace jsou zaručit alespoň 50 nabízené aktualizace za den.
- Pokud aplikace zahrnuje komplikace, bude být umístěná v galerii Apple Watch vzhled (najdete v článku společnosti Apple [přidání komplikace do Galerie](https://developer.apple.com/documentation/clockkit/adding_complications_to_the_gallery) Další informace naleznete v dokumentaci).

V watchOS 3, rozhraní ClockKit nyní zahrnuje několik nových šablon pro velmi velké komplikace, jako [CLKComplicationTemplateExtraLargeColumnsText](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) a [CLKComplicationTemplateExtraLargeRingImage ](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). Kromě toho pokud chcete vytvořit lokalizovatelný textu, použijte nové metody [CLKTextProvider](https://developer.apple.com/reference/clockkit/clktextprovider) třídy.

Další informace, najdete v tématu naše [rychlé interakce techniky pro watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) průvodce.

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>Nově dostupné architektury

watchOS 3 obsahuje několik existující Apple rozhraní, které byly dříve k dispozici jako:

- **SceneKit** -použití SceneKit zahrnout 3D modelů do uživatelského rozhraní aplikace sledovat včetně většinu funkcí, které jsou k dispozici na jiných platformách, jako jsou fyzice osvětlení, stínování animace, a částice systémy. 3D prostorových zvuk, vlastní operačního systému nebo OpenGL shadery, základní Image filtry a fyzicky na základě materiály nejsou podporovány.
- **SpriteKit** -SpriteKit použít k vykreslení a použije animaci Sprite v uživatelském rozhraní aplikace sledování aplikace včetně většinu funkcí, které jsou k dispozici na jiných platformách, jako akce, fyziky, osvětlení a částice systémy. 3D prostorových zvuk, přehrávání videa a základní Image filtry nejsou podporovány.
- **AVFoundation** – ke správě a přehrajte zvuk.
- **CloudKit** – pro přesun dat mezi kontejnery sledovat aplikace a na Icloudu.
- **Základní zvuk** – Pokud chcete spravovat typy dat sloužící k zastoupení zvukové datové proudy, komplexní vyrovnávací paměti a hodnot času.
- **GameKit** – Pokud chcete vytvořit sociálních hry.

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>Proaktivní návrhy

watchOS 3 umožňuje aplikaci na proaktivně informace pro uživatele v rámci zadané kontexty. Chcete-li tuto funkci podporovat [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) nyní zahrnuje `MapItem` vlastnost, která umožní aplikaci poskytují informace o umístění pro pozdější použití jiné aplikace.

Další informace, najdete v tématu naše [Úvod do proaktivní návrhy](~/ios/watchos/platform/proactive-suggestions.md) průvodce.

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>Zabezpečení a ochrany osobních údajů vylepšení

Apple udělal několik vylepšení zabezpečení a ochrany osobních údajů v watchOS 3, který vám pomůže vývojáře zlepšit zabezpečení své aplikace a zajištění ochrany osobních údajů koncového uživatele.

V důsledku toho je třeba deklarovat aplikace běžící na watchOS 3 (nebo novější) staticky jejich pokus o přístup k určité funkce nebo informace o uživateli tak, že zadáte jednu nebo více o ochraně osobních údajů konkrétní klíče v jejich `Info.plist` soubory, které vysvětlují, uživateli, proč aplikace chce získat přístup.

Vzhledem k tomu, že watchOS 3 sdílí tyto změny s iOS 10, najdete v tématu naše iOS 10 [zabezpečení a ochrany osobních údajů vylepšení](~/ios/app-fundamentals/security-privacy.md) Průvodce pro další informace.

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>Snímky a ukotvení

V watchOS 3 byl přidán Apple ukotvení, kde mohou uživatelé připnout své oblíbené aplikace a rychle přistupovat k nim. Když uživatel stiskne tlačítko straně na Apple Watch, zobrazí se Galerie definovaného aplikace snímků. Uživatel může prstem doleva nebo doprava najít požadovanou aplikaci a pak klepněte na aplikaci spustíte nahraďte snímek spuštěné aplikaci rozhraní.

Systém pravidelně trvá snímky uživatelském rozhraní aplikace a používá tyto snímky k vyplnění dokumentaci. watchOS poskytuje aplikaci příležitost k aktualizaci uživatelského rozhraní a obsahu předtím, než se provede tento snímek.

Další informace najdete v tématu naše [úlohy na pozadí](~/ios/watchos/platform/background-tasks.md) průvodce a společnosti Apple [WKSnapshotRefreshBackgroundTask odkaz](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) .

<a name="User-Notifications" />

## <a name="user-notifications"></a>Oznámení uživateli

Rozhraní oznámení pro uživatele byla zavedená v watchOS 3 podporuje doručování místních i vzdálených oznámení Apple Watch. Použijte toto rozhraní k plánu oznámení na základě určitých podmínek například čas, den nebo umístění a k přijímat a zpracovávat oznámení.

Další informace, najdete v tématu naše [rychlé interakce techniky pro watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) průvodce.

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>Podívejte se na vylepšení Framework připojení

Nové `HasContentPending` vlastnost [WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession) – třída označuje, že relace přijme data na pozadí, které je potřeba zpracovat. A `RemainingComplicationUserInfoTransfers` vlastnost vrací zbývající dobu, aplikaci pro iOS můžete aktualizovat jeho watchOS komplikace.

Další informace, najdete v tématu naše [úlohy na pozadí](~/ios/watchos/platform/background-tasks.md) průvodce.

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>Vylepšení WatchKit Framework

watchOS 3 obsahuje několik vylepšení WatchKit framework, včetně následujících:

- Aplikaci můžete získat stav digitální Crown pomocí nové [WKCrownSequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer) třída a přijímat aktualizace, když uživatel otočí crown pomocí [WKCrownDelegate](https://developer.apple.com/reference/watchkit/wkcrowndelegate) třídy.
- [WKExtension](https://developer.apple.com/reference/watchkit/wkextension) třída nyní zahrnuje `ApplicationState` metoda a [WKApplicationState](https://developer.apple.com/reference/watchkit/wkapplicationstate) konstanta, která aplikace můžete použít ke sledování stav modulu runtime aplikace. `WKExtension` také poskytuje dvě nové metody, které lze použít k naplánování úlohy na pozadí.
- [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) nyní zahrnuje nové `ApplicationWillEnterForeground`, `ApplicationDidEnterBackground` a `HandleBackgroundTasks` metody pro sledování změn ve stavu aplikace a zpracování aktualizace úloh na pozadí.
- Nový [WKGestureRecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) třída byla přidána zajistit následující typy gesto rozpoznávání do aplikace pro sledování: [WKLongPressGestureRecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [WKPanGestureRecognizer ](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer), [WKSwipeGestureRecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) a [WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer).
- Nové [WKinterfaceHMCamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) třída poskytuje rozhraní pro všechny HomeKit připojené IP fotoaparát.
- Nové [WKInterfaceInlineMovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) třída umožňuje aplikaci zobrazovat film "plakát", který se nahrazuje spuštěné film, když uživatel klepnutím ji.
- Nové [WKInterfacePaymentButton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) třída umožňuje aplikaci dotykový identifikátor tlačítko k dispozici ve svém uživatelském rozhraní, která zahájí žádost o platbu po klepnutí.
- Nové [WKInterfaceSCNScene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) třída představuje rozhraní pro zobrazení scény SceneKit v Apple Watch.
- Nové [WKInterfaceSKScene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) třída představuje rozhraní pro zobrazení scény SpriteKit v Apple Watch.

Další informace, najdete v tématu naše [rychlé interakce techniky pro watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) průvodce.

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>Vylepšení cvičení aplikace

Nové k watchOS 3 cvičení související s aplikací mít možnost spustit na pozadí na Apple Watch. Povolit tuto funkci (a přístup k datům HealthKit), musí aplikace obsahovat `WKBackgroundModes` klíče v `Info.plist` soubor s hodnotou `workout-processing`.

Kromě toho vývojář má teď možnost spustit aplikaci cvičení watchOS od verze iOS aplikace na spárované zařízení iPhone.

Další informace, najdete v tématu naše [cvičení aplikace vylepšení](~/ios/watchos/platform/workout-apps.md) průvodce.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Další Framework změny

Kromě hlavní framework změny a dodatky uvedených výše Apple udělal mnoho dalších menší framework změn v watchOS 3.

Další informace, najdete v tématu naše [další změny Framework](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) průvodce.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>Zastaralá rozhraní API

Následující rozhraní API jsou zastaralé v watchOS 3:

- `UILocalNotification` Třídy UIKit se už nepoužívá a měl by být nahrazen rozhraní oznámení pro uživatele.

Najdete v článku společnosti Apple [watchOS 2.2 k rozhraní API rozdíly watchOS 3.0](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) dokumentace úplný seznam vyřazení a změn.


## <a name="related-links"></a>Související odkazy

- [Ukázky pro watchOS](https://developer.xamarin.com/samples/watchos/all/)
- [Co je nového v watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
