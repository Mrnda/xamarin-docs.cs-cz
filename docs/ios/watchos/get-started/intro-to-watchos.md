---
title: "Úvod do watchOS"
description: "Přehled watchOS řešení strukturu a omezení"
ms.topic: article
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: fbf430e16506bdcf89ea3e280d42f27b0406f153
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-watchos"></a>Úvod do watchOS

> [!NOTE]
> Podívejte se [Úvod do watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md) přehled nejnovějších funkcí.

## <a name="about-watchos"></a>O watchOS

Řešení aplikace watchOS má 3 projekty:

- **Podívejte se na rozšíření** – projekt, který obsahuje kód pro sledování aplikace.
- **Podívejte se na aplikace** – obsahuje storyboard uživatelského rozhraní a prostředky.
- **Nadřazená aplikace pro iOS** – je tato aplikace normální iPhone. Sledování aplikace a rozšíření jsou seskupeny do iPhone aplikace pro doručování sledovat uživatele.

V aplikacích watchOS 1 kód v rozšíření běží na iPhone – Apple Watch je efektivně externí zobrazení. zcela na Apple Watch spustit aplikace watchOS 2 a 3. Tento rozdíl je znázorněno na obrázku níže:

[ ![](intro-to-watchos-images/arch-sml.png "Rozdíl mezi watchOS 1 a watchOS 2 (a vyšší) se zobrazí v tomto diagramu")](intro-to-watchos-images/arch.png#lightbox)

Bez ohledu na to, kterou verzi watchOS je určeno v sadě Visual Studio pro Mac je řešení Pad kompletního řešení bude vypadat přibližně takto:

[![](intro-to-watchos-images/projectstructure-sml.png "Odsazení řešení")](intro-to-watchos-images/projectstructure.png#lightbox)

*Nadřazená aplikace* v watchOS řešení je regulární iOS aplikace. Toto je pouze projekt v řešení, které se zobrazuje **v telefonu**. Případy použití pro tuto aplikaci bude zahrnovat kurzy, správu obrazovky a střední vrstvě filtrování, cacheing atd. Je však možné pro uživatele k instalaci a spuštění aplikace nebo rozšíření sledovat bez **někdy** otevření aplikace nadřazené proto pokud potřebujete nadřazené aplikaci spustit pro jednorázové inicializace nebo správy, budete muset programu vaší sledování aplikace nebo rozšíření říct uživatelům, který.

I když aplikaci nadřazené doručí aplikaci sledovat a rozšíření, spouštějí se v různých izolovaných prostorů.

Na watchOS 1 můžou sdílet data prostřednictvím skupiny sdílené aplikaci nebo prostřednictvím statických funkce `WKInterfaceController.OpenParentApplication`, které budou aktivovat `UIApplicationDelegate.HandleWatchKitExtensionRequest` metoda ve vaší aplikaci nadřazené `AppDelegate` (najdete v části [práce s nadřazenou aplikací](~/ios/watchos/app-fundamentals/parent-app.md)).

Na watchOS 2 nebo novější, sledovat připojení framework se používá ke komunikaci s nadřazenou aplikací pomocí `WCSession` třídy.

## <a name="application-lifecycle"></a>Životní cyklus aplikace

V rozšíření sledovat, podtřídou třídy `WKInterfaceController` třída se vytvoří pro každou scény scénáře.

Tyto `WKInterfaceController` třídy jsou obdobou `UIViewController` objekty při programování pro iOS, ale nemají stejnou úroveň přístupu k zobrazení.
Nelze například dynamicky přidání ovládacích prvků na nebo změny struktury uživatelské rozhraní.
Můžete, ale skrýt a odhalit ovládací prvky a, některé ovládací prvky, změnit jejich velikost, průhlednost a možností vzhledu.

Životní cyklus `WKInterfaceController` objekt zahrnuje následující volání:

- [Vzhůru](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.Awake/) : Tato metoda by měla provést většinu vaší inicializace.
- [WillActivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.WillActivate/) : volat krátce před sledováním aplikace se zobrazí uživateli. Tuto metodu použijte, chcete-li provést inicializaci poslední chvíli, spuštění animace atd.
- V tomto okamžiku sledování aplikace se zobrazí a rozšíření zahájí zpracování uživatelského vstupu a aktualizace zobrazení sledování aplikace na aplikační logiku.
- [DidDeactivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.DidDeactivate/) po sledování aplikace byla zamítnuta uživatel, tato metoda je volána. Po návratu tato metoda ovládacích prvků uživatelského rozhraní nemůže být upravena až po příštím `WillActivate` je volána. Tato metoda bude volána taky, pokud dojde k přerušení připojení k iPhone.
- Po rozšíření bylo deaktivováno, je nedostupný pro váš program. Čekající asynchronní funkce **nikoli** volat. Podívejte se, že rozšíření Kit nesmíte používat režimy zpracování na pozadí. Pokud program se znovu aktivuje uživatelem, ale aplikace nebyla byla ukončena podle operačního systému, bude první metoda volána `WillActivate`.

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "Přehled životního cyklu aplikací")

## <a name="types-of-user-interface"></a>Typy uživatelského rozhraní

Existují tři typy interakci, kterou uživatel může mít s vaší aplikací pro sledování.
Všechny jsou naprogramovaný tak pomocí vlastní dílčí třídy `WKInterfaceController`, takže pořadí dřív popsané životního cyklu platí všeobecně (oznámení jsou naprogramovat se dílčí třídy `WKUserNotificationController`, který je podtřídou třídy `WKInterfaceController`):

### <a name="normal-interaction"></a>Normální interakce

Většina sledování aplikace nebo rozšíření interakce bude s dílčí třídy `WKInterfaceController` který zapsat tak, aby odpovídaly scény ve vaší aplikaci sledovat **Interface.storyboard**. To je podrobně v [instalace](~/ios/watchos/get-started/installation.md) a [Začínáme](~/ios/watchos/get-started/index.md) články.
Následující obrázek ukazuje část [sledovat Kit katalogu](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) na ukázkové scénáře. Pro každé scény vám ukázal, zde, je odpovídající vlastní `WKInterfaceController` (`LabelDetailController`, `ButtonDetailController`, `SwitchDetailController`atd) v projektu rozšíření.

![](intro-to-watchos-images/scenes.png "Příklady normální interakce")

### <a name="notifications"></a>Oznámení

[Oznámení](~/ios/watchos/platform/notifications.md) jsou hlavní případ použití pro Apple Watch. Jsou podporovány místních i vzdálených oznámení. Ve dvou fázích, názvem krátké a dlouho vzhled dojde k interakci s oznámení.

Krátký vypadá zobrazují stručně a zobrazovat ikona aplikace sledování, jeho název a název (jako zadaný `WKInterfaceController.SetTitle`).

Dlouhé vypadat kombinuje poskytované systémem **křídla** přeskočení tlačítko s vlastní obsah na základě scénáře a oblasti.

`WKUserNotificationInterfaceController` rozšiřuje `WKInterfaceController` pomocí metod `DidReceiveLocalNotification` a `DidReceiveRemoteNotification`.
Přepište tyto metody reagování na události oznámení.

Další informace o návrhu uživatelského rozhraní oznámení, naleznete [Apple Watch Human Interface Guidelines](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)

![](intro-to-watchos-images/notifications.png "Ukázka oznámení")

## <a name="screen-sizes"></a>Velikost obrazovky

Apple Watch má dva velikosti řez: 38mm a 42mm, jak s poměrem zobrazení 5:4 a sítnice zobrazení. Jejich funkční velikosti jsou:

- pixelů na logický 38 mm: 136 x 170 (272 x 340 fyzické pixelů)
- 42 mm: 156 x 195 logické pixelů (312 x 390 fyzické pixelech).

Použití `WKInterfaceDevice.ScreenBounds` k určení, na které zobrazení sledování aplikace běží.

Obecně je snazší vývoj návrhu text a rozložení s více omezené 38mm zobrazení a pak vertikálně navýšit kapacitu.
Pokud spustíte s větší prostředí, škálování může dojít k ugly překrývají nebo zkrácení textu.

Další informace o [práce s velikost obrazovky](~/ios/watchos/app-fundamentals/screen-sizes.md).


## <a name="limitations-of-watchos"></a>Omezení watchOS

Existují některá omezení watchOS znát při vývoji aplikace watchOS:

- Zařízení Apple Watch mají omezenou úložiště – zajímat, dostupné místo před stažením velkých souborů (např. zvuk nebo video soubory).

- Mnoho watchOS [ovládací prvky](~/ios/watchos/user-interface/index.md) podobných operací v UIKit, ale jsou různé třídy (`WKInterfaceButton` místo `UIButton`, `WKInterfaceSwitch` pro `UISwitch`atd) a mají omezenou sadu metody ve srovnání s jejich UIKit ekvivalenty. Kromě toho watchOS má některé ovládací prvky, jako `WKInterfaceDate` (pro zobrazení datum a čas), že UIKit nemá.

  - Nelze směrovat oznámení pro sledování pouze nebo iPhone pouze (jaký druh řízení má uživatel přes směrování nebyl byla oznamují Apple).

Některé známá omezení / nejčastější dotazy:

- Apple neumožní řezy vlastní sledování 3. stran.

- Rozhraní API, které umožňují sledovat k řízení iTunes na připojeného telefonu jsou privátní.


## <a name="further-reading"></a>Další čtení

Projděte si dokumentaci od společnosti Apple:

* [Vývoj pro sledování Kit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

* [Podívejte se na Průvodce programováním Kit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

* [Apple Watch lidského Interface Guidelines](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)


## <a name="related-links"></a>Související odkazy

- [watchOS 3 katalogu (ukázka)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [watchOS 1 katalogu (ukázka)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Instalační program a instalace](~/ios/watchos/get-started/installation.md)
- [První aplikaci sledovat video](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple je vývoji pro průvodce sledováním Kit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Tipy WatchKit společnosti Apple](https://developer.apple.com/watchkit/tips/)
- [Úvod do watchOSu 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
