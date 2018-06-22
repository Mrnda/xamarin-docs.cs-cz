---
title: Úvod do systému iOS 9
description: Tento článek představuje všechny nové a změněné rozhraní API a funkce, které jsou k dispozici v systému iOS 9 pro vývojáře Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 10ed9154b92e6f13dd71f83cf4fed47585dc795f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30781269"
---
# <a name="introduction-to-ios-9"></a>Úvod do systému iOS 9

_Tento článek představuje všechny nové a změněné rozhraní API a funkce, které jsou k dispozici v systému iOS 9 pro vývojáře Xamarin.iOS._

![](images/ios9-sml.png "Logo iOS 9")

Apple přidal několik nových rozhraní API a služby v iOS 9 společně s mnoho vylepšení stávajících funkcí.

## <a name="3d-touch"></a>3D dotykového ovládání

Nové iOS 9 a iPhone 6s a iPhone 6s Plus, přidá 3D Touch citlivé gesta přetížení do aplikace pro iOS. Pomocí 3D dotykového ovládání, iPhone aplikace je teď možné nejen říct, že uživatel je klepnou na displeji zařízení, můžete také smysl kolik zatížení je vyvození uživatele a reagovat na úrovni různé zatížení.

3D Touch poskytuje následující funkce do vaší aplikace:

- **Zatížení velkých a malých písmen** – aplikací lze měřit teď jak pevného nebo light uživatele je klepnou na obrazovce a proveďte výhod těchto informací. Například aplikace Malování provádět řádku silnější nebo tenčí podle jak pevného je uživatel klepnou na obrazovce.
- **Prohlížení a Pop** -aplikace můžete teď umožňují uživatelům interakci s jeho data bez nutnosti přejít z jejich aktuálního kontextu. Stisknutím kombinace kláves pevný na obrazovce, mohou *prohlížet* položku mají zájem (např. zobrazení náhledu zprávy). Stisknutím kombinace kláves těžší, mohou *Pop* do položky.
- **Rychlé akce** -myslíte o rychlé akce jako kontextové nabídky, které mohou být odebrány up když uživatel klikne pravým tlačítkem myši na položku v aplikace na ploše. Pomocí rychlé akce, můžete přidat běžné, rychlé a snadné na zástupce přístup k funkcím ve vaší aplikaci z ikonu domovskou obrazovku na zařízení s iOS.

Další informace, najdete v tématu naše [Úvod do 3D Touch](~/ios/platform/3d-touch.md) průvodce.

## <a name="app-transport-security"></a>Zabezpečení přenosu aplikace

Nové do systému iOS 9, zabezpečení přenosu aplikace (ATS) vynucuje zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikací. ATS zajišťuje shodu všechny internetové komunikace s zabezpečené připojení osvědčených postupů, a zabrání náhodného zpřístupnění citlivých informací, buď přímo prostřednictvím aplikace nebo knihovny, které je využívají.

Vzhledem k tomu, že je ve výchozím nastavení v aplikacích, které jsou vytvořené pro iOS 9 a OS X 10.11 (El Capitan), všechna připojení pomocí povolené ATS [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) nebo [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) bude podléhají Požadavky na zabezpečení ATS. Pokud vaše připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.

Chcete-li získat další informace o ATS, najdete v tématu naše [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md) průvodce.

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>Multitasking pro iPad

S iOS 9 Apple přidal multitasking podporu pro spuštění dvě aplikace ve stejnou dobu na konkrétní iPad hardwaru. V důsledku toho aplikace Xamarin.iOS již nemůže předpokládat, že jsou spuštěné v každém okamžiku pouze aplikace nebo který mají přístup k celé obrazovky nebo prostředky zařízení.

Multitasking pro iPad je podporována prostřednictvím následujících funkcí:

- **Snímek přes** – umožňuje uživatelům dočasně spuštění druhého aplikace iOS na snímku na panelu (buď na pravé a levé straně obrazovky založené na jazyce směr), které pokrývá přibližně 25 % hlavní aplikace aktuálně spuštěna. Vysuňte přes je k dispozici pouze na iPad Pro, iPad letecké, iPad letecké 2, Ipadu 2 malé, iPad malé 3 nebo iPad malé 4.
- **Rozdělení zobrazení** -na hardwaru podporované iPad (iPad letecké 2, 4 malé iPad a profesionál pouze iPad), můžete uživatele vyberte druhý aplikace a spustit s aktuálně spuštěné aplikaci v režimu rozdělení obrazovky vedle sebe. Uživatel může řídit procento hlavní obrazovky, který bude zabírat každou aplikaci.
- **Obrázek obrázku** – pro aplikace, které přehrávání obsahu videa, teď video může být přehráván v okně přenosné a s možností změny velikosti, které překrývat dalších aplikací aktuálně spuštěné na zařízení iOS. Uživatel má plnou kontrolu nad velikost a umístění toto okno. Obrázek na obrázku je k dispozici pouze na iPad Pro, iPad letecké, iPad letecké 2, Ipadu 2 malé, iPad malé 3 nebo iPad malé 4.

Chcete-li získat další informace o nové schopnosti multitasking systému iOS 9, najdete v tématu naše [Multitasking pro iPad](~/ios/platform/multitasking.md) průvodce.

## <a name="new-contacts-and-contacts-ui-frameworks"></a>Nové kontakty a architektur kontakty uživatelského rozhraní

Se zavedením systému iOS 9, Apple vydala dvě nové rozhraní, [kontakty](https://developer.xamarin.com/api/namespace/Contacts/) a [ContactsUI](https://developer.xamarin.com/api/namespace/ContactsUI/), který nahradí existující adresář a uživatelského rozhraní kniha adres rozhraní používané v iOS 8 a před.

Tyto nové, objektově orientovaný architektury přináší tyto výhody:

- **Kontakty** – Xamarin.iOS poskytuje přístup ke kontaktní údaje uživatele. Protože většinu aplikací vyžadují jenom oprávnění jen pro čtení, toto rozhraní je optimalizován pro přístup z více vláken bezpečné, jen pro čtení přístup.
- **ContactsUI** – elementy poskytuje uživatelské rozhraní Xamarin.iOS chcete zobrazit, upravit, vyberte a vytvořte kontakty na zařízeních s iOS.

Další informace najdete v tématu naše [kontakty a uživatelského rozhraní kontakty](~/ios/platform/contacts.md) dokumentaci.


## <a name="new-search-apis"></a>Nové rozhraní API pro vyhledávání

Hledání se rozšířila na iOS 9 zajistit skvělé nové způsoby pro přístup k informacím v rámci aplikace Xamarin.iOS. Pomocí nové rozhraní API pro vyhledávání, můžete provést obsah vaší aplikace s možností vyhledávání prostřednictvím Spotlight a Safari výsledky hledání, aby Handoff a Siri připomínky a návrhy. To umožňuje uživatelům rychlý přístup k aktivity a informace o hloubkové v rámci vaší aplikace.

Kromě toho nové rozhraní API pro vyhledávání usnadňují integraci vyhledávání ve vaší aplikaci bez před vyhledáváním implementace. Z toho důvodu Apple deklarací, že obvykle trvá několik hodin, aby se obsah aplikace iOS 9 všeobecně vyhledávat pomocí vyhledávací aplikaci.

Další informace najdete v tématu naše [vylepšení vyhledávání](~/ios/platform/search/index.md) dokumentaci.

## <a name="new-stack-view"></a>Nové zobrazení zásobníku

Ovládací prvek zobrazení zásobníku ([UIStackView](https://developer.xamarin.com/api/type/UIKit.UIStackView/)) využívá power automatického rozložení a velikost tříd, které slouží ke správě více dílčích zobrazení (vodorovně nebo svisle), která dynamicky reaguje na velikost orientaci a obrazovky zařízení s iOS.

Pomocí zobrazení zásobníku řízení množství práce, vyžaduje rozložení uživatelské rozhraní je výrazně snižují. Rozložení všechny dílčích zobrazení připojené k zobrazení protokolů jsou spravovány automaticky podle vývojáře definované vlastnosti například osy, distribuci, zarovnání a mezery.

Další informace najdete v tématu naše [Úvod do zásobníku zobrazení](~/ios/user-interface/controls/uistackview.md) dokumentaci.


## <a name="collection-view-changes"></a>Zobrazení změn v kolekci

V systému iOS 9, zobrazení kolekce ([UICollectionView](https://developer.xamarin.com/api/type/UIKit.UICollectionView/)) teď podporuje přetáhněte změny pořadí položek mimo pole přidáním nové rozpoznávání gesto výchozí a několik nových podpůrné metod.

Pomocí těchto nových metod, můžete snadno implementovat změnit přetáhněte pořadí v zobrazení kolekce a mít možnost přizpůsobení vzhledu položky během jakékoli fázi způsob procesu.

Další informace o změnách zobrazení kolekce pro iOS 9, najdete v tématu naše [zobrazení změn v kolekci](~/ios/user-interface/controls/uicollectionview.md) průvodce.

## <a name="game-enhancements"></a>Herní vylepšení

S iOS 9 Apple udělal několik vylepšení technologické herní rozhraní API, která usnadňují implementaci herní grafiky a zvuku ve vaší aplikaci Xamarin.iOS. Mezi ně patří i usnadnění vývoje prostřednictvím základní architektury a využití možností GPU zařízení s iOS pro vylepšené rychlost a grafické dalo nízké úrovně vylepšení.

To společně s novou, rozšířené funkce operačního systému, SceneKit a SpriteKit zahrnuje GameplayKit, ReplayKit, Model vstupně-výstupních operací, MetalKit a shadery výkonu operačního systému.

Další informace najdete v tématu naše [herní vylepšení](~/ios/platform/gaming/index.md) dokumentaci.

## <a name="homekit-framework-changes"></a>Změny HomeKit Framework

[HomeKit](https://developer.xamarin.com/api/namespace/HomeKit/) framework, zavedená v iOS 8, poskytuje možnost nastavit a řídit různé příslušenství HomeKit povolené (například automatizované indikátory zámků dveří a Garážový dveře otevírající) z aplikace Xamarin.iOS. Kromě toho lze snadno nainstalovat a nakonfigurovat, příslušenství HomeKit lze řídit prostřednictvím mluvené příkazy Siri.

V systému iOS 9 má Apple snazší instalační program, rozšířit typy příslušenství podporována a poskytuje další příslušenství interakce (jako je třeba řízení určité příslušenství vzdáleně prostřednictvím serveru služby iCloud).

Další informace najdete v tématu naše [Úvod do HomeKit](~/ios/platform/homekit.md), [ukázkové aplikace pro iOS HomeKitIntro](https://developer.xamarin.com/samples/monotouch/HomeKit/HomeKitIntro/) a společnosti Apple [HomeKit](https://developer.apple.com/homekit/) dokumentaci.

## <a name="handoff-framework-changes"></a>Aby handoff Framework změny

Aby handoff (také označované jako kontinuity) zavedl Apple v iOS 8 a OS X Yosemite (10.10) jako způsob, jak uživatelům aktivitu na jednom z jejich zařízení (iOS nebo Mac) a dál stejné aktivity na jiném zařízení (jako identifikovaný iClou uživatele d účtu).

Aby handoff byl v iOS 9 podporuje nový, taky rozšířit rozšířené možnosti hledání. Další informace najdete v tématu naše [vylepšení vyhledávání](~/ios/platform/search/index.md) dokumentaci. Další informace o používání aby Handoff, najdete v tématu naše [Úvod k předání](~/ios/platform/handoff.md) dokumentaci.

## <a name="new-extension-points"></a>Nové rozšiřovací body

V iOS 8, Společnost Apple vydala rozšíření – knihovny, které jsou prezentované podle operačního systému v standardní kontextech, například v centru oznámení, když si uživatel vyžádá klávesnici nebo při úpravách fotografie.

S iOS 9, Apple rozšiřuje podporu rozšíření tím, že poskytuje několik nových _rozšíření body_ , definujte zásady používání a zadejte rozhraní API pro práci v dané oblasti následujícím způsobem:

- **Nový bod rozšíření jednotky zvuk** – pomocí tohoto bodu rozšíření můžete zadat zvuk důsledky, hudební nástroje, zvukové generátory, atd. pro použití v rámci jiné aplikace zvuk jednotka hostitele (například GarageBand). Tento bod rozšíření umožňuje také prodeje _zvuk jednotky_ (audio moduly plug-in) na webu App Store.
- **Nový bod rozšíření údržby Index** – používat tento bod rozšíření pro podporu novou indexaci dat aplikace bez nutnosti obnova aplikaci.
- **Nové body rozšíření sítě** (vyžadují speciální oprávnění od společnosti Apple):
    - **Rozšíření zprostředkovatele Proxy aplikace** – pomocí tohoto bodu rozšíření implementace proxy vlastní transparentní klientské sítě.
    - **Filtrovat zprostředkovatele dat nebo poskytovatele rozšíření pro ovládací prvek filtru** -těchto rozšíření bodů použít k implementaci obsah dynamické sítě filtrování na zařízení.
    - **Paket tunelového propojení poskytovatele rozšíření** – pomocí tohoto bodu rozšíření implementovat vlastní tunelového propojení protokolu na straně klienta VPN.
- **Nové body rozšíření Safari**:
    - **Obsahu blokování rozšíření** – používat tento bod rozšíření můžete definovat seznam blokovaných obsah, který se nezobrazí, když uživatel prohlíží web.
    - **Sdílené odkazy rozšíření** – pomocí tohoto bodu rozšíření můžete povolit zobrazení obsahu vaší aplikace v prohlížeči Safari na sdílených odkazuje.

Další informace najdete v tématu naše [Úvod do rozšíření](~/ios/platform/extensions.md) a společnosti Apple [Průvodce programováním v aplikaci rozšíření](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) dokumentaci.

## <a name="keychain-enhancements"></a>Vylepšení řetězce klíčů

V systému iOS 9 je Apple vylepšené řetězce klíčů zajistit nový typ šifrování klíče pro Enclave zabezpečení a další možnosti ochrany položky:

- Nové omezení Touch ID, která by způsobila neplatnost položky řetězce klíčů, když se mění databázi otisků prstů.
- Nové omezení, které umožňují pouze vytváření položek seznamu řízení přístupu pomocí Touch ID nebo hesla.
- Nový kontext ověřování, která umožňuje vyvolání ověřování oddělené od `SecItem` volání.
- Přístup k seznamu řízení entropie (pomocí možnosti heslo aplikace) pro šifrování položky zadaný aplikace řetězce klíčů.
- Podpora pro vytváření a použití klíčů uvnitř zabezpečené enclave (prostřednictvím `kSecAttrTokenIDSecureEnclave` atributu).

Další informace najdete v tématu naše [Úvod do Touch ID](~/ios/platform/touchid.md) dokumentaci.


## <a name="right-to-left-language-support"></a>Podpora jazyků zprava doleva

V systému iOS 9 Apple udělal prezentací přetočený uživatelské rozhraní jednodušší než kdy tím, že poskytuje plnou podporu pro jazyky zprava doleva. To zahrnuje následující:

- Standardní [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) ovládací prvky bude automaticky překlopit doleva na základě nastavení národního prostředí a jazyk zařízení iOS.
- [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) třída poskytuje atributy, které umožňují definovat, jak daný zobrazení se zobrazit při překlopení zprava doleva.
- Možnost překlopení obrázku programově pomocí [FlipsForRightToLeftLayoutDirection](https://developer.xamarin.com/api/property/UIKit.UIImage.FlipsForRightToLeftLayoutDirection/) vlastnost [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/) třídy.

Další informace najdete v tématu společnosti Apple [Supporting zprava doleva](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) dokumentaci.



## <a name="additional-framework-changes"></a>Další Framework změny

Kromě hlavní změny, které jsme mít popsané výše Apple udělal změny a vylepšení několik existující rozhraní pro iOS 9 včetně následujících:

- AV Foundation Framework
- AVKit Framework
- CloudKit Framework
- Foundation Framework
- Aby handoff Framework
- HealthKit Framework
- HomeKit Framework
- Místní ověřování Framework
- MapKit Framework
- PassKit Framework
- Safari Services Framework
- UIKit Framework

Další informace najdete v tématu naše [další iOS 9 Framework změny](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) dokumentaci.

## <a name="deprecated-apis-and-functions"></a>Zastaralá rozhraní API a funkce

Apple má zastaralé následující rozhraní API a funkce v systému iOS 9:

- **Adresa kniha & adresa kniha uživatelského rozhraní** – rozhraní API pro tyto byly nahrazeny architektury kontaktu a obraťte se na uživatelské rozhraní. Další informace najdete v tématu naše [kontakty a uživatelského rozhraní kontakty](~/ios/platform/contacts.md) dokumentaci.
- **CBCentralManager** – `RetrievePeripherals` a `RetrieveConnectedPeripherals` metody `CBCentralManager` třídy, že byla odebrána v iOS 9. Volání těchto metod způsobí, že aplikace do aplikace PowerPoint při párování určité příslušenství nebo při spuštění aplikace.
- **FetchAllChanges** – `FetchAllChanges` z `CKFetchRecordChangesOperation` třída byla odepsat a odeberou v iOS 9.
- **Přehrávač médií** -Media Player framework se již nepoužívá v iOS 9. Místo toho použijte AVKit nebo AV Foundation rozhraní API.

Úplný seznam konkrétní vyřazení rozhraní API, najdete v článku společnosti Apple [iOS 9.0 rozhraní API Diffs](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) dokumentaci.

## <a name="ios-9-sample-apps"></a>Ukázkové aplikace pro iOS 9

Některé máme [iOS 9 konkrétní ukázky](https://developer.xamarin.com/samples/ios/iOS9/) začít:

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://developer.xamarin.com/samples/monotouch/ios9/MetalPerformanceShadersHelloWorld/)
- [MusicMotion](https://developer.xamarin.com/samples/monotouch/ios9/MusicMotion/)
- [PhotoProgress](https://developer.xamarin.com/samples/monotouch/ios9/PhotoProgress/)
- [SegueCatalog](https://developer.xamarin.com/samples/monotouch/ios9/SegueCatalog/)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

Podívejte se taky na části iOS tyto ukázky (doprovodné Mac OS X verze přicházející!):

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Úvod do 3D dotykového ovládání](~/ios/platform/3d-touch.md)
- [Zabezpečení přenosu aplikací](~/ios/app-fundamentals/ats.md)
- [Multitasking pro iPad](~/ios/platform/multitasking.md)
- [Kontakty a kontakty uživatelského rozhraní](~/ios/platform/contacts.md)
- [Nové rozhraní API pro vyhledávání](~/ios/platform/search/index.md)
- [Úvod do zásobníku zobrazení](~/ios/user-interface/controls/uistackview.md)
- [Zobrazení změn v kolekci](~/ios/user-interface/controls/uicollectionview.md)
- [Herní vylepšení](~/ios/platform/gaming/index.md)
- [Úvod do HomeKit](~/ios/platform/homekit.md)
- [Úvod k odložení](~/ios/platform/handoff.md)
- [Další změny v architektuře iOSu 9](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [Odstraňování potíží](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [Co je nového v iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Aktualizace aplikace Xamarin.iOS na iOS9 (video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
