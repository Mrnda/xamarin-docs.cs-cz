---
title: Úvod do systému macOS Sierra
description: Tento článek představuje všechny nové a změněné rozhraní API a funkce, které jsou k dispozici v systému macOS Sierra pro vývojáře Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3b8211e4c38fd37040fab5b35be4709d4b926c91
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
---
# <a name="introduction-to-macos-sierra"></a>Úvod do systému macOS Sierra

S novou macOS Sierra vývojáři mohou využít výhod nových rozhraní API, které koncovému uživateli interakci s jejich aplikace a weby způsoby dříve k dispozici. Například Apple teď umožňuje weby zákazníkům poskytovat možnost platícího bezpečně prostřednictvím dotykový identifikátor a vylepšení systému framework nárůst grafiky aplikace a výpočetních potenciální. 

Další informace o systému macOS Sierra, najdete v tématu společnosti Apple [systému macOS + aplikace](https://developer.apple.com/macos/) dokumentaci.

<a name="Whats-New-in-macOS-Sierra" />

## <a name="whats-new-in-macos-sierra"></a>Co je nového v systému macOS Sierra

Apple přidala několik nových rozhraní API a služby v systému macOS Sierra společně s mnoho vylepšení stávajících funkcí, včetně:

<a name="Apple-File-System" />

### <a name="apple-file-system"></a>Systém souborů Apple

Pomocí systému macOS Sierra Apple vydala jako systém souborů moderní pro iOS, systému macOS, tvOS a watchOS nový systém souborů Apple. Systém souborů Apple bylo optimalizováno pro Flash a SSD úložiště a poskytuje následující funkce: silné šifrování, metadata kopírování při zápisu, místo sdílení, klonování pro soubory a adresáře, snímky, rychlé directory velikosti a atomic primitiv bezpečné uložení.

Další informace najdete v tématu společnosti Apple [příručce k systému souborů Apple](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999).

<a name="Apple-Pay-Enhancements" />

### <a name="apple-pay-enhancements"></a>Vylepšení platím Apple

Apple udělal několik vylepšení Apple platit v systému macOS Sierra povolit uživatele, aby zabezpečené platby od weby.

Pomocí systému macOS Sierra několik nových rozhraní API přidané které pracují s systému macOS Sierra, iOS a watchOS pro podporu dynamické platebních sítí a nové testovacím prostředí izolovaného prostoru.

systému macOS Sierra zahrnuje nové rozhraní ApplePay Javascript, které umožňuje vývojáři začlenit dotykový identifikátor přímo do iOS a systému macOS Safari webových stránek. Weby, které podporují dotykový identifikátor může ověřit uživatele pomocí jejich zařízení iPhone nebo Apple Watch platba.

Další informace najdete v tématu společnosti Apple [ApplePay JS Framework](https://developer.apple.com/reference/applepayjs) odkaz.

<a name="Building-Modern-macOS-Apps" />

### <a name="building-modern-macos-apps"></a>Sestavení systému macOS moderní aplikace

Moderní systému macOS aplikace například společnosti Apple Safari webový prohlížeč, stránky textového procesoru a čísla šíření pomocí list mnoho nové technologie, které představují jednotné a závislé na kontextu uživatelské rozhraní, která provádí rychle s tradiční prvky uživatelského rozhraní, jako je například plovoucí panely a více open Windows.

[![Příkladem okno s kartami Mac](images/content08.png)](images/content08.png#lightbox)

Naše [systému macOS vytváření moderní aplikace](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md) Průvodce obsahuje několik tipy, funkce a techniky vývojář může použít k vytvoření aplikace moderní systému macOS v Xamarin.Mac.

<a name="CloudKit-Data-Sharing" />

### <a name="cloudkit-data-sharing"></a>Sdílení dat CloudKit

Rozhraní framework CloudKit rozšířila v systému macOS Sierra chcete umožnit uživatelům rychle a snadno sdílet záznamy nebo sady záznamů z jejich databází privátní serveru služby iCloud.

CloudKit poskytuje kompletní uživatelského rozhraní pro odesílání a přijímání sdílených záznamů pozvánek a uživatel má dokončení pro čtení a zápis kontrolu nad osoby, které mají přístup k záznamům.

Další informace najdete v tématu společnosti Apple [CloudKit Framework – referenční informace](https://developer.apple.com/reference/clockkit) a [CloudKit JS Framework – referenční informace](https://developer.apple.com/reference/cloudkitjs).

> [!IMPORTANT]
> Apple [poskytuje nástroje](https://developer.apple.com/support/allowing-users-to-manage-data/) , což vývojářům správně zpracovat Evropské unie obecné Data Protection nařízení (GDPR).

<a name="Safari-App-Extensions-Support" />

### <a name="safari-app-extensions-support"></a>Podpora rozšíření Safari aplikací

Rozšíření aplikace Safari povolit aplikaci k rozšíření chování webový prohlížeč Safari při je těsně integrovaná se službami systému macOS Sierra. Vzhledem k tomu, že rozšíření systému macOS Safari aplikace fungovat podobná iOS přípony aplikace Safari, jsou snadno port v jednom systému do jiného.

Další informace najdete v tématu společnosti Apple [Průvodce programováním v prohlížeči Safari aplikace rozšíření](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319).

<a name="Security-and-Privacy-Enhancements" />

### <a name="security-and-privacy-enhancements"></a>Zabezpečení a ochrany osobních údajů vylepšení

Apple má provedli několik vylepšení zabezpečení a ochrany osobních údajů v systému macOS Sierra, který vám pomůže aplikaci zlepšit zabezpečení aplikace a zajištění ochrany osobních údajů koncového uživatele včetně následujících:

- Nové `NSAllowsArbitraryLoadsInWebContent` klíč můžete přidat do aplikace `Info.plist` souboru a umožní načíst správně, je stále povolena ochrana zabezpečení přenosu Apple (ATS) pro zbytek aplikace při webové stránky.
- Běžné Data zabezpečení architektura (CDSA) rozhraní API se už nepoužívá a měl by být nahrazen SecKey rozhraní API pro generování asymetrické klíče.
- Pro všechna připojení protokolem SSL/TLS symetrický šifrovací algoritmus RC4 nyní ve výchozím nastavení vypnutá. Kromě toho rozhraní API zabezpečit přenos již nepodporuje SSLv3 a se doporučuje, aby aplikace přestat používat šifrování SHA-1 a 3DES co nejdříve.
- Protože nové schránky v iOS 10 a systému macOS Sierra umožňuje uživatelům kopírovat a vkládat mezi zařízeními, rozšířila rozhraní API umožňující schránky do být omezeno na konkrétní zařízení a být označen časovým razítkem automaticky vymazat k danému bodu. Kromě toho pojmenované pracovních plochách už jsou trvalé a měl by být nahrazen sdíleným kontejnerům pracovní ploše.
- Pokud aplikace přístup k chráněným datům (například uživatele kalendář), jeho _musí_ deklarovat tohoto záměru hodnota klíčem účel správný řetězec v jeho `Info.plist` souboru (`NSCalendarUsageDescription` v případě kalendáře).
- Podepsaná aplikace vývojáře, které nejsou doručeny prostřednictvím Mac App Storu můžete nyní využít výhod CloudKit, Icloudu řetězce klíčů, Icloudu jednotky, vzdálené nabízená oznámení, oprávnění MapKit a síť VPN.
- systému macOS Sierra již nepodporuje, jako cesta runtime není známá před runtime doručování externí kód nebo dat společně s podepisující kód aplikace v jeho archivu zip nebo image disku bez znaménka.

Kromě toho aplikace běžící v systému macOS Sierra (nebo novější) je třeba deklarovat staticky jejich pokus o přístup k určité funkce nebo informace o uživateli tak, že zadáte jednu nebo více o ochraně osobních údajů konkrétní klíče v jejich `Info.plist` soubory, které vysvětlují, proč chce získat aplikaci uživateli přístup.

Vzhledem k tomu, že systému macOS Sierra sdílí tyto změny s iOS 10, najdete v tématu naše iOS 10 [zabezpečení a ochrany osobních údajů vylepšení](~/ios/app-fundamentals/security-privacy.md) Průvodce pro další informace.

<a name="Smart-Card-Driver-Extension-Support" />

### <a name="smart-card-driver-extension-support"></a>Podpora rozšíření ovladače čipových karet

Pomocí systému macOS Sierra, můžete vytvořit aplikaci `NSExtension` na základě ovladače čipových karet, které umožňuje přístup jen pro čtení k obsahu z určitých typů čipové karty. Tyto informace jsou pak uvedeny v řetězci klíčů systému (Nahraďte metodu nepoužívané běžné Architektura zabezpečení dat).

Další informace, stránku najdete Apple na [CryptoTokenKit Framework – referenční informace](https://developer.apple.com/reference/cryptotokenkit).

<a name="Unified-Logging" />

### <a name="unified-logging"></a>Jednotná protokolování

Jednotná protokolování poskytuje aplikaci pomocí jediného rozhraní API pro efektivní služby zasílání zpráv na všech úrovních systému. Unified protokolování má aplikace jemně odstupňovanou kontrolu nad více úrovní protokolování, které zahrnují ochrany osobních údajů a aktivity sledování pro snazší ladění. 

Protokolování poskytuje korelace automatické zprávy v případě aktivity sledování a protokolování používají společně.

systému macOS Sierra zahrnuje novou aplikaci konzoly (v aplikace/nástroje) aby bylo možné zobrazit data protokolu z více zdrojů včetně připojená zařízení. Také podporuje tokenizovaná a uložené hledání a zobrazí připojení mezi související zprávy více procesy.

Kromě toho zprávy protokolu lze zobrazit a udržuje pomocí nástroje příkazového řádku.

Další informace najdete v tématu společnosti Apple [protokolování odkaz](https://developer.apple.com/reference/os/1891852-logging).

<a name="Wide-Color" />

### <a name="wide-color"></a>Široké barev

systému macOS Sierra rozšiřuje podporu pro rozšířené rozsah pixelů formáty a prostory celou rozsah, barvy v celém systému, včetně architektury, jako je například základní grafické prvky, základní Image, operačního systému a AVFoundation. Podpora pro zařízení s wide barev zobrazí se další opatřeny náběhem / tím, že poskytuje toto chování v rámci celého grafiky zásobníku.

Kromě toho `AppKit` byla změněna pro práci v nové rozšířené **sRGB** barevný prostor, což usnadňuje kombinovat barev v rozsahů barev široké bez ztráty významně zvýšit výkon.

Apple nabízí následující osvědčené postupy při práci s wide barev:

- `NSColor` Teď používá sRGB barvu místa a nadále nebude CLAMP – hodnoty `0.0` k `1.0` rozsahu. Pokud aplikace závisí na předchozí chování svorka, ji budou muset u systému macOS Sierra změnit.
- Při použití nízké úrovně rozhraní API, například základní grafické prvky nebo operačního systému k zajištění zpracování obrázků, aplikace by měla používat rozšířené rozsah barva místa a pixelů formátu, který podporuje 16bitové hodnot s plovoucí desetinnou. V případě potřeby bude muset ručně CLAMP – hodnoty barev součásti aplikace.
- Základní grafické prvky, základní Image a shadery výkonu operačního systému všech poskytují nové metody pro převod mezi dvěma barevné prostory.

Další informace, najdete v tématu naše [Úvod do široké barva](~/ios/platform/wide-color.md) průvodce.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Další Framework změny

Kromě hlavní framework změny a dodatky uvedených výše Apple udělal mnoho dalších menší framework změn v systému macOS Sierra.

Další informace, najdete v tématu naše [další změny Framework](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md) průvodce.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>Zastaralá rozhraní API

V systému macOS Sierra jsou zastaralé následující rozhraní API:

- HFS standardní systém souborů není podporován.

Najdete v článku společnosti Apple [v10.12 OS X API Diffs](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/APIDiffsMacOS10_12/index.html) dokumentace úplný seznam vyřazení a změn.

## <a name="related-links"></a>Související odkazy

- [Ukázky pro Mac](https://developer.xamarin.com/samples/mac/)
- [Co je nového v OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
