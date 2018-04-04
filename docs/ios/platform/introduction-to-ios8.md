---
title: Úvod do systému iOS 8
description: S iOS 8 Apple poskytl nadbytku nové architektury a rozhraní API pro buzení a štěstím vývojáři. V této příručce jsme zavést tato nová rozhraní API a v tématu jak využívat iOS 8 vývojářům a uživatelům.
ms.prod: xamarin
ms.assetid: 33AD66C0-3743-49FE-9DCE-88ED3A16BA63
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 2f57547356adcbafd01851bc54e42a14454ccd6a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-ios-8"></a>Úvod do systému iOS 8

_S iOS 8 Apple poskytl nadbytku nové architektury a rozhraní API pro buzení a štěstím vývojáři. V této příručce jsme zavést tato nová rozhraní API a v tématu jak využívat iOS 8 vývojářům a uživatelům._

iOS 7 vizuálně změnit celý iOS uživatelské rozhraní z jaké uživatelů a vývojářů měl pocházejí očekávat přímo na první iPhone operačního systému. IOS 8 pokračuje to tím, že poskytuje mnoho rozhraní pro vývojáře, která umožňuje uživatelům řídit skoro každý aspekt své životnosti přímých z jejich zařízení iPhone. Například stav a vhodnosti mohou být analyzovány s *HealthKit*, jsou obsolescent s biometrické ověřování pomocí hesla *LocalAuthentication*, *rozšíření aplikace*otevřete komunikační kanál mezi aplikacemi 3. stran, a *HomeKit* uživatelům umožňuje zapnout domu do domovské budoucí. 

Pokud iOS 7 se o delighting uživatele, se zaměřuje na delighting vývojářům celou řadu tyto tasty nové nástroje iOS 8. 

Tento průvodce představuje nové rozhraní API pro vývojáře Xamarin.iOS.  

Existují také několik rozhraní API, které jsou zastaralé v iOS 8, které jsou podrobně popsané na konci tohoto dokumentu.

## <a name="requirements"></a>Požadavky

Toto jsou nezbytné pro vytváření aplikací pro iOS 8 v sadě Visual Studio pro Mac:

- **Xcode 7 a iOS 8 nebo novější** – nejnovější Xcode společnosti Apple a iOS rozhraní API musí být nainstalovaná a nakonfigurovaná na počítači pro vývojáře.
- **Visual Studio pro Mac** – nejnovější verze sady Visual Studio pro Mac musí být nainstalováno a nakonfigurováno na zařízení uživatele.
- **iOS 8 zařízení ani simulátor** – zařízení s iOS s nejnovější verzí systému iOS 8 pro testování.

## <a name="home-and-leisure"></a>Home a volném čase

iOS 8 pomohlo pevně přímo do vysílat domácích prostřednictvím HomeKit a HealthKit plánu Apple a zařízení s iOS. V této části se podíváme na tom, jak oba tyto nové architektury fungují a jak může být integrovaná do aplikace Xamarin.iOS.

## <a name="homekit"></a>HomeKit

Řízení svoje zařízení z vašeho Iphonu není novou aplikaci technologie; Řada produktů připojené domovské lze řídit prostřednictvím aplikace pro iOS. Ale HomeKit nyní trvá tento krok další povýšení společný protokol pro domácí automatizaci zařízení a zpřístupnění veřejné rozhraní API určité výrobců, jako je například iHome, Philips a Honeywell. Pro uživatele. to znamená, že se můžete řídit skoro každý aspekt jejich domovské bezproblémově z uvnitř jednu aplikaci. Je důležité pro ně vědět, že používají Philips Hue žárovek nebo alarmů vnoření. Uživatelé mohou také zřetězené množství inteligentní domácí procesů společně do "Scény".

S HomeKit aplikace třetích stran a Siri můžete zjistit příslušenství a přidejte je do jejich osobní domácí konfigurační databáze, upravit a provedení akce tato data a komunikovat s příslušenství a jejich služby k provedení akce.

### <a name="configuration"></a>Konfigurace

Následující diagram ukazuje základní hierarchie konfigurace HomeKit příslušenství:

![](introduction-to-ios8-images/image1.png "Tento diagram zobrazuje základní hierarchie konfigurace HomeKit příslušenství")
 
Začít s HomeKit vývojáři potřebovat a ujistěte se, že jejich profilu pro zřizování má službu HomeKit vybrané. Apple má také poskytuje vývojářům HomeKit simulátoru doplňku pro Xcode. To lze nalézt v [Apple Developer Center](https://developer.apple.com/downloads/index.action)v části `Hardware IO Tools for Xcode`. 

Další informace najdete v tématu naše [HomeKit](~/ios/platform/homekit.md) průvodce.

## <a name="healthkit"></a>HealthKit

HealthKit je zavedená v iOS 8, která poskytuje centralizovaný, koordinované a zabezpečené úložiště pro informace týkající se stavu rozhraní. Operační systém zajišťuje ochranu osobních údajů a zabezpečení s informacemi o stavu a stavu aplikace, řídicí panel pro uživatele. Aplikace s oprávněními uživatele lze číst a zapisovat širokou škálu informací o stavu.

Další informace o použití to ve vaší aplikaci Xamarin.iOS, najdete v části [Úvod do HealthKit](~/ios/platform/healthkit.md) průvodce.

## <a name="extending-iphone-functionality"></a>Rozšíření iPhone funkce
S iOS8 vývojáři se mají mnohem větší kontrolu nad kdo používat své aplikace a vyšší schopností pro více otevřete komunikaci mezi aplikací třetí strany. Funkce, jako je výběr dokumentu a rozšíření aplikace otevřete široké možnosti použití aplikací v ekosystému společnosti Apple.

### <a name="app-extensions"></a>Rozšíření aplikace
Rozšíření aplikace oversimplify, představují způsob a vzájemnou komunikaci aplikace třetích stran. Udržování vysoké zabezpečení standardů a bude udržovat integritu v izolovaném prostoru aplikace, tato komunikace nedojde přímo mezi aplikacemi. Místo toho je prováděna pomocí *rozšíření* uprostřed.

Prvním krokem při vytváření rozšíření aplikace je k definování bodem správná přípona – to je důležité při zajišťování dostupnosti správné rozhraní API a chování. Pokud chcete vytvořit aplikaci rozšíření v sadě Visual Studio pro Mac, přidáte ji do existující aplikaci tak, že přidáte nový projekt pro vaše řešení.

V **nový projekt** dialogové okno přejděte na **C#** > **iOS** > **unifikované API**  >  ** Rozšíření**, jak ukazuje následující snímek obrazovky:

![](introduction-to-ios8-images/image2.png "Vytvoření nové rozšíření")
 
Dialogové okno Nový projekt poskytuje sedm nové šablony projektu pro vytváření rozšíření pro aplikace a jsou popsané dále. Všimněte si, že mnoho rozšíření souvisí jiná nové rozhraní API v iOS, jako je například dokument výběr:

- **Akce** – to umožňuje vývojářům vytvářet tlačítka jedinečný vlastní akce umožňuje uživatelům provádí určité úlohy
- **Vlastní klávesové** – to umožňuje vývojáři pro přidání do rozsahu vytvořené v Apple klávesnice přidáním vlastních vlastní. Oblíbené klávesnice, Swype, který to používá k poskytování jejich klávesnice na iOS.
- **Zdokumentujte výběr** – obsahuje řadič dokumentu výběr zobrazení, která umožňuje uživatelům přístup k souborům mimo izolovaného prostoru aplikace.
- **Zdokumentujte poskytovatele pro výběr souborů** – to poskytuje zabezpečené úložiště pro soubory pomocí nástroje pro výběr dokumentu.
- **Úpravy fotografií** – tato rozšíří na filtry a již poskytovaných společností Apple v aplikaci fotografie uživatelům větší kontrolu a další možnosti při úpravě jejich fotografie nástroje pro úpravy.
- **Dnes** – díky tomu aplikace umožňuje zobrazovat pomůcky v části dnes centra oznámení.

Další informace o použití rozšíření aplikace v Xamarinu, najdete v části [Úvod do rozšíření aplikace](~/ios/platform/extensions.md) průvodce.

### <a name="touch-id"></a>Touch ID

Touch ID byla zavedena v iOS 7 jako prostředek k ověřování uživatele – podobně jako heslo. Bylo ale omezený na odemknutí zařízení, pomocí obchodu s aplikacemi, pomocí iTunes a serveru služby iCloud řetězce klíčů pouze ověřování 

Nyní existují dva způsoby, jak použít Touch ID jako ověřovací mechanismus v aplikacích iOS 8 pomocí rozhraní API pro místní ověřování. Aktuálně není možné použít místní ověřování k ověření vzdáleně.

Za prvé pomůcky stávající služby řetězce klíčů pomocí nové řetězce klíčů seznamy řízení přístupu (ACL). Data řetězce klíčů může být odemknout s úspěšné ověření uživatelů otisk prstu.

Za druhé LocalAuthentication nabízí dvě metody k ověření vaší aplikaci můžete lokálně spustit. Vývojáři využít `CanEvaluatePolicy` lze zjistit zařízení je schopný přijímat Touch ID a potom `EvaluatePolicy` spustit operaci ověřování.

Další informace o Touch ID a další informace o jeho integraci do aplikace pro Xamarin.iOS, najdete v části [Úvod k touch ID](~/ios/platform/touchid.md) příručky.

### <a name="document-picker"></a>Výběr dokumentu.

Obnovení znovu dokumentu výběr funguje s na jednotku Icloudu uživatelů umožňuje uživatelům otevírat soubory, které byly vytvořeny v jiné aplikaci, importovat a s nimi manipulovat a exportovat je. Tím se vytvoří intuitivní pracovního postupu a proto mnohem lepší, pro uživatele. synchronizuje se Icloudu trvá ještě o krok další – veškeré změny provedené v jedné aplikace se projeví také konzistentně ve všech zařízeních.

Další informace o výběru dokumentu do větší hloubky a zjistěte, jak integrovat do aplikace pro Xamarin.iOS, se vztahují [Úvod do pro výběr dokumentu](~/ios/platform/document-picker.md) průvodce.

### <a name="handoff"></a>Aby handoff

Aby handoff, který je součástí větší funkce kontinuity, trvá krokem další k integraci OS X a iOS. To zahrnuje AirDrop napříč platformami, schopnost pořizovat iPhone volání, SMS na iPad a Mac. a vylepšení sdílení internetového připojení přes z vašeho Iphonu.

Aby handoff funguje s iOS 8 a Yosemite a vyžaduje Icloudu účet pro přihlášení na různých zařízeních, že které chcete použít. Ho by měla spolupracovat s předem nainstalovaná aplikace Apple, včetně Safari, iWork, map, kalendáře a kontaktů.

Další informace najdete v tématu naše [aby Handoff](~/ios/platform/handoff.md) průvodce.

## <a name="unified-storyboards"></a>Jednotná scénářů
iOS 8 obsahuje nový jednodušší použít mechanismus pro vytvoření uživatelského rozhraní – jednotná scénáře. Pomocí jednoho storyboard nepokrývají všechny velikosti obrazovky jiný hardware lze vytvořit rychlé a dobře reagovaly zobrazení ve stylu true "návrh jednou, použití mnoha".

Před iOS8, vývojáři použít `UIInterfaceOrientation` k rozlišení mezi režimem na výšku a šířku a `UIInterfaceIdiom` k rozlišení mezi zařízeními iOS. Ve iOS8 není nadále potřebné k vytvoření samostatné scénářů pro zařízení iPhone a iPad – orientaci a zařízení, které jsou určeny pomocí *velikost třídy*.

Každé zařízení je definované třídou velikost, svislé i vodorovné osy a existují dva typy tříd velikost v iOS 8:

- **Regulární** – to je pro velikost promítat obrazovku (jako je iPad) nebo miniaplikaci, která vyvolává dojem velké (například UIScrollView
- **Compact** – to je pro menší zařízení (například iPhone). Tato velikost bere v úvahu orientaci zařízení.

Pokud se dvěma konceptů použijí společně, výsledkem je mřížky 2 x 2, který definuje různé možné velikosti, které lze použít v obou různé orientace, jak je vidět v následujícím diagramu:

![](introduction-to-ios8-images/image3.png "Diagram představující definující různé možné velikosti, které lze použít v odlišné orientaci mřížky 2 x 2")
 
Další informace o třídách velikost, najdete v části [Úvod do Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md).

## <a name="photo-kit"></a>Fotografie Kit
Fotografie Kit je nové rozhraní, které umožňuje aplikacím pro dotazování knihovna obrázků systému a vytvořit vlastní uživatelská rozhraní zobrazovat a měnit jeho obsah. Obsahuje několik tříd, které představují bitové kopie a video prostředky, jakož i kolekcí prostředků, jako je například alb a složky.

Další informace najdete v tématu naše [PhotoKit](~/ios/platform/photokit.md) průvodce.

## <a name="games"></a>hry

<a name="scenekit" />

### <a name="scene-kit"></a>Scény Kit

Scény Kit je rozhraní API, které usnadňuje práci s 3D grafický graf 3D scény. Bylo poprvé dostupné ve OS X 10.8 a teď zase na iOS 8. S scény Kit vytváření dokonalé 3D vizualizace a běžné 3D hry nevyžaduje odborných znalostí v OpenGL. Sestavování na běžné koncepty grafu scény, scény Kit abstrahuje rychle složitosti OpenGL a ES OpenGL, což velmi usnadňuje přidat 3D obsahu k aplikaci. Pokud jste odborník OpenGL, má scény Kit však podpory pro příkazů přímo s OpenGL také. Také zahrnuje množství funkcí, které doplňují 3D grafiky, jako je například fyziky a velmi dobře se integruje se službou několik dalších rozhraní Apple, například základní animace, základní Image a pohyblivý symbol Kit.

Další informace najdete v tématu naše [SceneKit](~/ios/platform/gaming/scenekit.md) dokumentaci.

<a name="spritekit" />

### <a name="sprite-kit"></a>Pohyblivý symbol Kit

Pohyblivý symbol Kit, 2D herní framework od společnosti Apple, obsahuje některé zajímavé nové funkce v iOS 8 a OS X Yosemite. Mezi ně patří integrace s Kit scény, podporu shaderu, osvětlení, stínů, omezení, normální mapa generování a fyziky vylepšení. Konkrétně nové funkce fyziky usnadňují velmi přidat realistické efekty do hru.

Další informace najdete v tématu naše [SpriteKit](~/ios/platform/gaming/spritekit.md) dokumentaci.

## <a name="other-changes"></a>Další změny
A také hlavní změny v iOS 8, které jsou popsané výše Apple aktualizoval kromě mnoho existující rozhraní. Podrobnosti jsou níže:

- **[Základní Image](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171) ** – Apple se rozšířila při jeho zpracování framework image přidáním lepší podporu pro zjišťování oblastí obdélníkový a kódy QR uvnitř bitové kopie. Jan Bluestein jsou zde popsány to v tomto blogu post nárok [detekce bitové kopie v iOS 8](http://blog.xamarin.com/image-detection-in-ios-8/)

## <a name="deprecated-apis"></a>Zastaralá rozhraní API
Všechna vylepšení provedené v iOS 8 máte několik rozhraní API zastaralé. Některé z nich jsou podrobně popsány níže.

- **[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication) ** – mít zastaralé metody a vlastnosti používaná pro registraci vzdáleného oznámení. Jsou to registerForRemoteNotificationTypes a enabledRemoteNotificationTypes.
- **[UIViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController) ** – velikost třídy a vlastnosti nahradit metody a vlastnosti, které používají k popisu rozhraní orientace. Odkazovat [Úvod do Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) Další informace o tom, jak použít.

- **[UISearchDisplayController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController) ** – to v iOS8 nahradila UISearchController.

## <a name="summary"></a>Souhrn
V tomto článku jsme se podívali na některé nové funkce, zavedená v iOS 8 společností Apple.



## <a name="related-links"></a>Související odkazy

- [UIKitEnhancements (ukázka)](https://developer.xamarin.com/samples/monotouch/ios8/UIKitEnhancements)
- [Úvod do rozšíření aplikace](~/ios/platform/extensions.md)
- [Úvod do CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)
- [Úvod do nástroje pro výběr dokumentu](~/ios/platform/document-picker.md)
- [Úvod do HealthKit](~/ios/platform/healthkit.md)
- [Úvod k ovládacím prvkům ruční fotoaparát](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [Úvod do touch ID](~/ios/platform/touchid.md)
- [Úvod do jednotná scénářů](~/ios/user-interface/storyboards/unified-storyboards.md)
