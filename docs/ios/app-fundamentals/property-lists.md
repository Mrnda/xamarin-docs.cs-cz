---
title: "Práce s seznamů vlastností"
description: "Toto téma představuje Visual Studio pro Mac na grafickém uživatelském rozhraní nebo rozšířené vlastnosti seznamu (.plist) editor pro práci s Info.plist a Entitlements.plist. Ilustruje ikony nastavení a spuštění bitové kopie pro aplikace pro iOS z v sadě Visual Studio for Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 0d7b4c5a539470a3544d0117251f40fd6bd37f2b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-property-lists"></a>Práce s seznamů vlastností

_Toto téma představuje Visual Studio pro Mac na grafickém uživatelském rozhraní nebo rozšířené vlastnosti seznamu (.plist) editor pro práci s Info.plist a Entitlements.plist. Ilustruje ikony nastavení a spuštění bitové kopie pro aplikace pro iOS z v sadě Visual Studio for Mac._

Visual Studio pro Mac funkce editor grafické .plist, který umožňuje úpravy vlastností aplikace a možnosti jednodušší. Visual Studio pro Mac má dva .plists - `Info.plist` pro úpravy vlastností aplikace a ikony, a `Entitlements.plist` pro správu možnosti aplikace. Tento průvodce uvádí Info.plists a poskytuje přehled o práci s nimi v sadě Visual Studio for Mac. Informace o Entitlements.plist najdete v tématu [práce oprávnění](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

## <a name="infoplist"></a>Info.plist

Seznam vlastností informace ( `Info.plist`) je soubor vyžaduje iOS, která poskytuje informace o konfiguraci vaší aplikace do systému. Visual Studio pro Mac je vlastní `Info.plist` editor funkce zbývající tři panely řízené karty v dolní části okna editoru:

 [ ![](property-lists-images/tabs.png "Zbývající karty editor Info.plist v dolní části okna editoru")](property-lists-images/tabs.png)

Jednotlivé panely prvky různé vlastnosti, jak je uvedeno níže:

-  **Panel aplikace** -grafické rozhraní pro nastavení vlastnosti aplikace a také ikony a spuštění bitové kopie; zadejte integrace mapy a backgrounding režimy.
-  **Rozšířené panely** -pokročilé panel je na místě k určení typů podporovaných dokumentů, UTIs a typy adresy URL.
-  **Zdroj panely** – ovládací prvky panelu zdroj méně běžné vlastnosti, jakož i vlastní vlastnosti pro aplikaci.


V následujících třech částech prozkoumat funkce jednotlivé panely podrobněji.

## <a name="application-panel"></a>Panel aplikace

Visual Studio pro Mac funkce grafické rozhraní pro úpravy běžné `Info.plist` položek pro aplikaci:

1.  Vlastnosti aplikace
1.  Typy podporovaných zařízení
1.  Orientace podporu pro každý typ zařízení
1.  Stavový řádek stylu a barvy
1.  Ikony a spuštění obrazovky
1.  Mapy a režimy pozadí


Tyto možnosti jsou popsány podrobněji v dalších částech.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>iOS cílové aplikace

Tato část obsahuje důležité informace, které popisují vaše aplikace.
**Identifikátor** uložené v tomto poli musí odpovídat identifikátor svazku, který jste zadali v iTunes Connect (aplikace pro aplikace pro Store) a taky v seznamu zřizování ID aplikace portálu iOS a vývoj a distribuci certifikátů.

 [ ![](property-lists-images/image24.png "iOS cílové aplikace")](property-lists-images/image24.png)

### <a name="device-deployment"></a>Nasazení zařízení

 [ ![](property-lists-images/deployment.png "Nasazení zařízení")](property-lists-images/deployment.png)

Zařízení **nasazení** informace o části se zobrazí selektivně, v závislosti na výběru v **zařízení** rozevírací seznam v **cíl aplikací** část výše. **Hlavní rozhraní** rozevíracího seznamu je nastaven na **MainStoryboard** v řízené Storyboard aplikace. Pokud uživatelské rozhraní je zcela napsán v kódu, a to může být ponecháno prázdné.

### <a name="supported-device-orientations"></a>Orientace zařízení

 **Podporované orientace zařízení** řídí, jak aplikaci reagovat na otočení zařízení. Je velmi běžné pro iPhone nebo iPad aplikace pro podporu pouze **na výšku**, nebo všechno, ale **opačně**. Všechny aplikace na zařízení iPad s výjimkou hry obecně by měly podporovat všechny orientace.

### <a name="status-bar-styles"></a>Styly stavového řádku

**Stav panelu Styly** část se grafické rozhraní pro úpravy aplikace `UIStatusBarStyle`:

 [ ![](property-lists-images/status.png "Styly stavového řádku")](property-lists-images/status.png)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>Ikony, spusťte Image a iTunes kresby

Informace o používání ikony, Image a kresby v souboru Info.plist najdete v [práce s obrázky](~/ios/app-fundamentals/images-icons/index.md) průvodce.




### <a name="maps-integration-and-background-modes"></a>Integrace mapy a režimy pozadí

`Info.plist` Obsahuje speciální části k určení integrace mapy a backgrounding režimy. Výběr možností, které chcete podporovat přidá požadované vlastnosti pro aplikaci za vás.

 [ ![](property-lists-images/maps.png "Integrace mapy")](property-lists-images/maps.png)

Další informace o práci s mapy, najdete v části Xamarin [iOS mapy](~/ios/user-interface/controls/ios-maps/index.md) průvodce.

 [ ![](property-lists-images/bging.png "Režimy pozadí")](property-lists-images/bging.png)

Další informace o režimy pozadí, najdete v části Xamarin [Backgrounding v iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) průvodce.

## <a name="advanced-panel"></a>Pokročilé panely

Ovládací prvky panelu Další typů dokumentů a schémata URL, které podporuje aplikace.

 [ ![](property-lists-images/image34.png "Pokročilé panely")](property-lists-images/image34.png)

 <a name="Document_Types" />


## <a name="document-types"></a>Typů dokumentů.

Pro aplikace, které podporují otevírání konkrétní typy souborů, poskytuje iOS `CFBundleDocumentTypes` klíč. Pokud chceme, aby naše aplikace pro podporu určitých typů souborů známých – například soubory PDF - doporučujeme přidat hodnotu PDF ke klíči. Tato část nabízí pohodlný způsob, jak zadejte data, která bude uložená v `CFBundleDocumentTypes` klíče v `Info.plist` souboru.

Informace naleznete v dokumentaci na [registrace podporuje vaše aplikace soubor typy](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) podrobnosti o tom, jak nakonfigurovat tyto hodnoty.

## <a name="utis"></a>UTIs

V některých případech aplikace musí podporovat otevírání vlastní typ souboru. Například může chceme otevřít soubory obrázků s příponou vlastní *.xam*. Pokud chcete zadat vlastní typ souboru, vytvoříme vlastní UTI - univerzální typ identifikátor - pomocí `UIExportedTypeDeclarations` klíč. Následující snímek obrazovky ukazuje, jak vytvořit vlastní UTI .xam rozšíření:

 [ ![](property-lists-images/uti.png "UTIs Editor")](property-lists-images/uti.png)

Stejně jako exportovaný typ UTIs zadejte vlastní UTIs specifické pro vaši aplikaci *importovat typ UTIs* ( `UIImportedTypeDeclarations` klíč) zadejte vlastní typy podporuje, ale není vlastníkem vaší aplikace.

Další informace o používání vlastní UTIs, najdete v části společnosti Apple [podporuje aplikace registrace souboru typy vaše](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) průvodce.

## <a name="custom-urls"></a>Vlastní adresy URL

Název schématu adresy URL (také nazývané protocol) je první část adresy URL. Například `http://` a `https://` jsou společných schémat URL. Máte možnost vytvořit vlastní schéma adresy URL pro aplikaci. Vlastní schémata URL se používají ke komunikaci a odesílat data a zpět s jinými aplikacemi. Následující snímek obrazovky ukazuje vytvoření nové vlastní schéma adresy URL názvem `monkeys://`:

 [ ![](property-lists-images/url.png "Vlastní adresy URL")](property-lists-images/url.png)



Další informace o implementaci vlastní schémata URL, najdete v části společnosti Apple [implementace vlastní schémata URL části této příručky](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>Panel Zdroj

**Zdroj** kartě `Info.plist` souboru umožňuje vlastní hodnoty a přidal nebo upravil. Visual Studio pro Mac obsahuje seznam nejčastějších vlastnosti:

 [ ![](property-lists-images/image31.png "Přidání nové vlastnosti z rozevírací seznam")](property-lists-images/image31.png)

Pro známé vlastnosti Visual Studio pro Mac se seznam platných hodnot, které jsou popsány v následující snímek obrazovky:

 [ ![](property-lists-images/image32.png "Vyberte hodnotu ze seznamu hodnot přehled")](property-lists-images/image32.png)

Visual Studio pro Mac také detekuje vlastnost typu, jak je znázorněno:

 [ ![](property-lists-images/image33.png "Dostupné typy vlastností")](property-lists-images/image33.png)

Zkontrolujte společnosti Apple [související prostředky aplikace](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) odkazy na další informace o volitelných vlastností.

 <a name="Entitlements" />

## <a name="summary"></a>Souhrn

Tento článek ukázal pomocí grafického rozhraní a pokročilých .plist editory upravit také obvyklé konfigurace aplikace, zadejte ikony a spuštění bitové kopie. Je také zavedená `Entitlements.plist` pro přidávání a správa možnosti aplikace.


## <a name="related-links"></a>Související odkazy

- [IDE](https://developer.xamarin.com/recipes/cross-platform/ide)
- [Aplikace související prostředky](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Vaše aplikace podporuje registraci soubor typy.](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Použití vlastní adresu URL schémat](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [O Asset katalogů](https://developer.apple.com/library/ioshttps://developer.xamarin.com/recipes/xcode_help-image_catalog-1.0/Recipe.html)
