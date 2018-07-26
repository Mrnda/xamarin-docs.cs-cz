---
title: Práce s seznamů vlastností v Xamarin.iosu
description: Toto téma představuje sady Visual Studio pro Mac grafické a rozšířené vlastnosti seznamu (.plist) editor pro práci s Info.plist a do souboru Entitlements.plist. Ukazuje nastavení ikon a spouštěcí obrázky pro aplikace pro iOS v rámci sady Visual Studio for Mac
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b102f268fd457ca42f3d64b5766a2b3824e3849d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242326"
---
# <a name="working-with-property-lists-in-xamarinios"></a>Práce s seznamů vlastností v Xamarin.iosu

_Toto téma představuje sady Visual Studio pro Mac grafické a rozšířené vlastnosti seznamu (.plist) editor pro práci s Info.plist a do souboru Entitlements.plist. Ukazuje nastavení ikon a spouštěcí obrázky pro aplikace pro iOS v rámci sady Visual Studio for Mac_

Visual Studio for Mac obsahuje editor grafické .plist, který umožňuje úpravy vlastností aplikace a jednodušší možnosti. Visual Studio for Mac obsahuje dvě .plists - `Info.plist` pro úpravy vlastností aplikace a ikony, a `Entitlements.plist` pro správu aplikace funkcí. Tento průvodce přináší Info.plists a poskytuje přehled o práci s nimi v sadě Visual Studio pro Mac. Informace o do souboru Entitlements.plist, najdete v článku [práce s nároky](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

## <a name="infoplist"></a>Info.plist

Informace o seznamu vlastností ( `Info.plist`) je povinný iOS soubor, který poskytuje informace o konfiguraci vaší aplikace do systému. V sadě Visual Studio for Mac vlastní `Info.plist` funkce editoru zbývající tři panely řídí karty v dolní části okna editoru:

 [![](property-lists-images/tabs.png "Zbývající karty editoru Info.plist v dolní části okna editoru")](property-lists-images/tabs.png#lightbox)

Každý panel – ovládací prvky různé vlastnosti, jak je uvedeno níže:

-  **Panel aplikace** – grafické rozhraní pro nastavení vlastností aplikace stejně jako ikony a spouštěcí Image; zadejte integrace s mapami a zpracování úloh na pozadí režimy.
-  **Pokročilé Panel** – pokročilé panel je místem, kde můžete určit typy podporovaných dokumentů, identifikátory Uti a typy adres URL.
-  **Panel zdroje** – ovládací prvky panelu zdroj, méně běžné vlastnosti, jakož i vlastní vlastnosti aplikace.


V následujících třech částech prozkoumat funkce jednotlivé panely podrobněji.

## <a name="application-panel"></a>Panel aplikace

Visual Studio pro Mac nabízí grafické rozhraní pro úpravy common `Info.plist` záznamy aplikace:

1.  Vlastnosti aplikace
1.  Typy podporovaných zařízení
1.  Orientace podporu pro každý typ zařízení
1.  Stavový řádek styl a barvy
1.  Ikony a úvodní obrazovky
1.  Mapy a režimy běhu na pozadí


Tyto možnosti jsou popsány podrobněji v následujících částech.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>Cíl aplikace pro iOS

Tato část obsahuje důležité informace, které popisují vaše aplikace.
**Identifikátor** uložené zde musí odpovídat identifikátoru sady prostředků, která je zadána ve službě iTunes Connect (pro aplikace pro Store App) a taky na seznam iOS zřizování ID aplikace portálu a vývoje a distribuce certifikátů.

 [![](property-lists-images/image24.png "Cíl aplikace pro iOS")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>Nasazení zařízení

 [![](property-lists-images/deployment.png "Nasazení zařízení")](property-lists-images/deployment.png#lightbox)

Zařízení **nasazení** informace o oddílech se zobrazují jednotlivě, v závislosti na výběru v **zařízení** rozevírací seznam v **cíl aplikace** výše uvedené části. **Hlavní rozhraní** rozevíracího seznamu je nastavena na **MainStoryboard** aplikace řízené scénáře. Pokud uživatelské rozhraní je zcela napsané v kódu, a to může být ponecháno prázdné.

### <a name="supported-device-orientations"></a>Podporované orientace zařízení

 **Podporované orientace zařízení** řídí způsob reakce aplikace na otočení obrazovky. Je velmi běžné, že aplikace pro iPhone/iPad pro podporu pouze **na výšku**, nebo všechno, ale **vzhůru nohama**. Všechny aplikace pro iPad s výjimkou hry obecně by měl podporovat všechny orientace.

### <a name="status-bar-styles"></a>Styly stavového řádku

**Styly řádku stav** grafické rozhraní pro úpravy aplikace je oddíl `UIStatusBarStyle`:

 [![](property-lists-images/status.png "Styly stavového řádku")](property-lists-images/status.png#lightbox)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>Ikony, obrázky po spuštění a obrázky pro iTunes

Informace o použití ikony, obrázky a obrázky v souboru Info.plist najdete v [práce s obrázky](~/ios/app-fundamentals/images-icons/index.md) průvodce.




### <a name="maps-integration-and-background-modes"></a>Integrace s mapami a režimy běhu na pozadí

`Info.plist` Obsahuje speciální části k určení integrace s mapami a zpracování úloh na pozadí režimy. Výběr možnosti, které chcete podporovat přidá požadované vlastnosti pro aplikaci za vás.

 [![](property-lists-images/maps.png "Integrace s mapami")](property-lists-images/maps.png#lightbox)

Další informace o práci s mapami najdete Xamarin [iOS mapy](~/ios/user-interface/controls/ios-maps/index.md) průvodce.

 [![](property-lists-images/bging.png "Režimy pozadí")](property-lists-images/bging.png#lightbox)

Další informace o režimy běhu na pozadí, přečtěte si Xamarin [zpracování úloh na pozadí v Iosu](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) průvodce.

## <a name="advanced-panel"></a>Pokročilé Panel

Panelu pokročilé ovládací prvky, typy dokumentů a schémata URL, které aplikace podporuje.

 [![](property-lists-images/image34.png "Pokročilé Panel")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />


## <a name="document-types"></a>Typy dokumentů

Pro aplikace, které podporují otevření konkrétní typy souborů poskytuje iOS `CFBundleDocumentTypes` klíč. Pokud chceme, aby naše aplikace pro podporu určitých souborů známých typů – například soubory PDF – doporučujeme přidat hodnotu PDF ke klíči. Tato část poskytuje pohodlný způsob, jak zadat data ukládaná ve službě `CFBundleDocumentTypes` klíče v `Info.plist` souboru.

Další informace naleznete v dokumentaci v [registraci souboru typy vaše aplikace podporuje](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) podrobnosti o tom, jak nakonfigurovat tyto hodnoty.

## <a name="utis"></a>Identifikátory Uti

Někdy aplikace musí podporovat otevřením souboru vlastního typu. Například může chceme k otevírání souborů obrázků s vlastní příponou *.xam*. Chcete-li určit vlastní typ souboru, vytvoříme vlastní identifikátor UTI – univerzální identifikátor typu – použití `UIExportedTypeDeclarations` klíč. Následující snímek obrazovky ukazuje, jak vytvořit vlastní identifikátor UTI .xam rozšíření:

 [![](property-lists-images/uti.png "Editor identifikátory Uti")](property-lists-images/uti.png#lightbox)

Stejně jako exportované identifikátory Uti typů zadejte vlastní identifikátory Uti specifické pro vaši aplikaci *importovat identifikátory Uti typů* ( `UIImportedTypeDeclarations` klíče) zadejte vlastní typy podporované, ale není vlastněn vaší aplikace.

Další informace o používání vlastní identifikátory Uti najdete společnosti Apple [registraci souboru typy vaše aplikace podporuje](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) průvodce.

## <a name="custom-urls"></a>Vlastní adresy URL

Název schématu adresy URL (také nazývané protocol) je první část adresy URL. Například `http://` a `https://` jsou společných schémat URL. Máte možnost vytvořit vlastní schéma adresy URL pro vaši aplikaci. Vlastní schémata adres URL se používají ke komunikaci a odesílat data vpřed a zpět s jinými aplikacemi. Následující snímek obrazovky ukazuje vytvoření nové vlastní schéma adresy URL názvem `monkeys://`:

 [![](property-lists-images/url.png "Vlastní adresy URL")](property-lists-images/url.png#lightbox)



Další informace o implementaci vlastní schémata URL, najdete v od Applu [implementace vlastní schémata URL části této příručky](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>Panel zdroje

**Zdroj** karty `Info.plist` soubor umožňuje vlastní hodnoty a přidávat ani upravovat. Visual Studio for Mac obsahuje seznam nejběžnějších vlastnosti:

 [![](property-lists-images/image31.png "Přidání nové vlastnosti z rozevíracího seznamu")](property-lists-images/image31.png#lightbox)

Pro známé vlastnosti sady Visual Studio pro Mac bude seznam platných hodnot, jak je znázorněno v následujícím snímku obrazovky:

 [![](property-lists-images/image32.png "Vyberte hodnotu ze seznamu hodnot ví")](property-lists-images/image32.png#lightbox)

Visual Studio pro Mac také detekuje tento typ vlastnosti, jak je znázorněno:

 [![](property-lists-images/image33.png "Dostupné typy vlastností")](property-lists-images/image33.png#lightbox)

Projděte si společnosti Apple [související prostředky aplikace](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) odkazy na další informace o volitelných vlastností.

 <a name="Entitlements" />

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali, pomocí grafických pokročilé i pokročilé .plist editory upravit také obvyklé konfigurace aplikace zadejte ikony a spouštěcí obrázky. Je také představila `Entitlements.plist` pro přidávání a Správa aplikace funkcí.


## <a name="related-links"></a>Související odkazy

- [INTEGROVANÉ VÝVOJOVÉ PROSTŘEDÍ](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide)
- [Prostředky související s aplikacemi](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Vaše aplikace podporuje registraci souboru typy.](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Použití vlastní adresu URL schémat](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [Odkaz na prostředek katalogu formát](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/index.html#//apple_ref/doc/uid/TP40015170-CH18-SW1)
