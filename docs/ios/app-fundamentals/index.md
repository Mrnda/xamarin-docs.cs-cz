---
title: Základy aplikace Xamarin.iOS
description: Tento dokument obsahuje odkazy na různé příručky, které popisují pro vývoj na platformě Xamarin.iOS, jako je například aplikace přenosu zabezpečení, základní koncepty backgrounding, události a dělení na vlákna.
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/21/2017
ms.openlocfilehash: cdace50d851b2c99f9241b869f248e58d5b93377
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784494"
---
# <a name="xamarinios-application-fundamentals"></a>Základy aplikace Xamarin.iOS

Tato část poskytuje návod na některé z běžnějších věcí úlohy nebo koncepty, že vývojáři, který je potřeba vzít na vědomí při vývoji aplikace Xamarin.iOS (dříve MonoTouch).

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[Zabezpečení přenosu aplikací](~/ios/app-fundamentals/ats.md)

Tento článek vás seznámí změny zabezpečení, které vynucuje zabezpečení přenosu aplikaci v aplikaci pro iOS 9 a co to znamená pro Xamarin.iOS projekty, bude se vztahovat možnosti konfigurace ATS a bude se vztahovat postup výslovný nesouhlas ATS, v případě potřeby. Protože ATS je ve výchozím nastavení povolené, jakékoli nezabezpečené připojení k Internetu vyvolá výjimku v aplikacích pro iOS 9 (Pokud jste explicitně povoleno ji).


## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Zpracovávání úloh na pozadí](~/ios/app-fundamentals/backgrounding/index.md)

Pozadí zpracování nebo backgrounding je proces, když je umožněno aplikace provádět úlohy na pozadí, zatímco jiná aplikace je spuštěná v popředí. Tato příručka slouží jako úvod ke zpracování v iOS na pozadí.


## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[Události, protokoly a delegáti](~/ios/app-fundamentals/delegates-protocols-and-events.md)

Tento článek představuje klíče iOS technologií používaných pro příjem zpětná volání a k vyplnění ovládacích prvků uživatelského rozhraní s daty. Tyto technologie jsou události, protokoly a delegáti; Tento článek vysvětluje, co každý z nich je a každý použití z jazyka C#. Ukazuje, jak Xamarin.iOS používá ovládací prvky iOS ke zveřejnění známé .NET události, a jak Xamarin.iOS poskytuje podporu pro koncepty jazyka Objective-C, jako jsou protokoly a delegáti (Objective-C delegáti by neměl být zaměňovat s delegáti C#). Tento článek také obsahuje příklady, které ukazují, jak protokoly se používají jak jako základ pro delegáti jazyka Objective-C a ve scénářích bez delegáta.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[Dělení na vlákna](~/ios/app-fundamentals/threading.md)

Tento článek popisuje dělení na vlákna aplikace pro Xamarin.iOS a trochu zmíněn rozhraní .NET fondu vláken, reaguje aplikace a uvolňování paměti.&nbsp;

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[Práce s obrázky](~/ios/app-fundamentals/images-icons/index.md)

Tento článek prozkoumá použití obrázků v Xamarin.iOS Image podporu aplikací (např. ikony, načítání bitové kopie, atd.) a bitové kopie v rámci aplikace (například obrázky aplikovat na ovládací prvky). Také popisuje, jak začlenit bitové kopie pomocí sady Visual Studio pro Mac, jakož i návod, jak pracovat s obrázky z kódu.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[Práce s seznamů vlastností](~/ios/app-fundamentals/index.md)

Toto téma představuje Visual Studio pro Mac na grafickém uživatelském rozhraní nebo rozšířené vlastnosti seznamu (.plist) editor pro práci s Info.plist a Entitlements.plist. Ilustruje nastavení ikon a spouštěcí bitové kopie pro aplikace pro iOS a ukazuje zadání možnosti aplikace (oprávnění) z v sadě Visual Studio for Mac.

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[Práce s systému souborů](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS můžete použít stejné třídy System.IO pro práci s souborů a adresářů v iOS, který byste použili v jakékoli aplikaci .NET. Navzdory známé třídy a metody, ale iOS implementuje určitá omezení na soubory, které můžou vytvořit nebo získat přístup a také poskytuje zvláštní funkce pro určité adresáře. Tento článek popisuje těchto omezení a funkce a vysvětluje, jak funguje přístup k souborům aplikace pro Xamarin.iOS.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[Vytváření iOS aplikace v kódu](~/ios/app-fundamentals/ios-code-only.md)

Tento článek prozkoumá postup vytvoření aplikace pro iOS zcela v kódu pomocí sady Visual Studio a Visual Studio for Mac. Ukazuje, jak spustit z prázdná šablona projektu stavět obrazovce aplikace v řadiči tím, vytváření hierarchie zobrazení z UIKit. Potom popisuje, jak vytvořit vlastní zobrazení, které je možné načíst v kontroleru.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[Práce s výchozí nastavení uživatele](~/ios/app-fundamentals/user-defaults.md)

`NSUserDefaults` Třída poskytuje způsob, jak pro iOS aplikace a rozšíření prostřednictvím kódu programu interakci se systémem výchozí systémové. Když použijete výchozí nastavení systému, můžete nakonfigurovat uživatele chování aplikace nebo práce se styly ke splnění jejich předvoleb (podle návrhu aplikace). Chcete-li například prezentují data v sadě vs metriky měření britské nebo vyberte daný motiv uživatelského rozhraní.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[Dotykové ovládání](~/ios/app-fundamentals/touch/index.md)

Dotykové obrazovky na množství dnešních zařízení umožňují uživatelům rychle a efektivně využívat zařízení přirozené a intuitivní způsobem. Tato interakce se neomezuje jenom na jednoduchý touch detekce – je možné pomocí gest také. Například gesto roztahováním přiblížení, je velmi běžné příklad tohoto – roztáhnout součástí obrazovky se dvěma prsty, které uživatel může zvětšit nebo zmenšit. Tato příručka prověří touch a gest v iOS.

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[Práce s zabezpečení a ochrana osobních údajů](~/ios/app-fundamentals/security-privacy.md)

Apple udělal několik vylepšení zabezpečení a ochrany osobních údajů v iOS 10 (a vyšší), které vám pomůžou vývojáře zlepšit zabezpečení své aplikace a zajištění ochrany osobních údajů koncového uživatele. Tento článek se zabývá implementace tyto funkce v aplikaci pro Xamarin.iOS.

##  <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[Lokalizace](~/ios/app-fundamentals/localization/index.md)

Tento průvodce popisuje přidání kódování pro aplikace pro Xamarin.iOS pro podporu internacionalizace.
