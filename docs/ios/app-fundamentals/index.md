---
title: Principy aplikací Xamarin.iOS
description: Tento dokument obsahuje odkazy na různá vodítka, které popisují koncepty základní vývoj na platformě Xamarin.iOS, jako je zabezpečení přenosu aplikací, zpracování úloh na pozadí, události a dělení na vlákna.
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/21/2017
ms.openlocfilehash: de337291554e81a2434dcc30c163f4789fc832eb
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111209"
---
# <a name="xamarinios-application-fundamentals"></a>Principy aplikací Xamarin.iOS

Tato část poskytuje návod na některé běžné úlohy věcí nebo koncepty, že vývojáři potřebujete znát při vývoji aplikace Xamarin.iOS (dříve MonoTouch).

## <a name="accessibilityiosapp-fundamentalsaccessibilitymd"></a>[Usnadnění](~/ios/app-fundamentals/accessibility.md)

Tento dokument popisuje různých rozhraní API a nástroje, které můžete použít k sestavení aplikací, které jsou přístupné pro tolik uživatelů co možná.

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[Zabezpečení přenosu aplikací](~/ios/app-fundamentals/ats.md)

Tento článek vás seznámí změny zabezpečení, které na aplikaci pro iOS 9 a co to znamená, že pro vaše projekty Xamarin.iOS vynucuje zabezpečení přenosu aplikací, se nevztahuje na možnosti konfigurace ATS a bude se vztahovat jak výslovný nesouhlas s ATS, v případě potřeby. Protože ATS ve výchozím nastavení zapnutá, budou všechny nezabezpečené připojení k Internetu vyvolat výjimku v aplikacích pro iOS 9 (Pokud jste explicitně povoleno ji).

## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Zpracovávání úloh na pozadí](~/ios/app-fundamentals/backgrounding/index.md)

Pozadí zpracování nebo zpracování úloh na pozadí je proces aplikace provádět úlohy na pozadí, zatímco jiná aplikace je spuštěná v popředí. Tato příručka slouží jako úvod ke zpracování v Iosu na pozadí.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[Vytváření aplikací pro iOS v kódu](~/ios/app-fundamentals/ios-code-only.md)

Tento článek zkoumá, jak k vytváření aplikací pro iOS kompletně v kódu s využitím sady Visual Studio a Visual Studio pro Mac. Ukazuje, jak začít od prázdná šablona projektu k sestavení můžete vytvořit hierarchii zobrazení z UIKit obrazovce aplikace v kontroleru. Pak popisuje, jak vytvořit vlastní zobrazení, které mohou být načteny v kontroleru.

## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[Události, protokoly a delegáti](~/ios/app-fundamentals/delegates-protocols-and-events.md)

Tento článek představuje iOS klíčových technologií používaných přijmout zpětná volání a k vyplnění ovládacích prvků uživatelského rozhraní s daty. Tyto technologie jsou události, protokoly a delegáti; Tento článek vysvětluje, co každý z nich je a jak jsou použity z jazyka C#. Ukazuje, jak Xamarin.iOS používá ovládací prvky s Iosem k vystavení známých .NET události, a jak Xamarin.iOS poskytuje podporu pro Objective-C konceptů, jako jsou protokoly a delegáti (Objective-C delegáti by neměly být zaměněny s C# delegáty). Tento článek také obsahuje příklady, které ukazují, jak protokolů se používají i jako základ pro delegáty, Objective-C a ve scénářích nedelegující.

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[Práce pomocí systému souborů](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS můžete použít stejné tříd System.IO pro práci se soubory a adresáře v iOS, která byste použili ve všech aplikací .NET. Bez ohledu na zkušenosti třídy a metody, ale iOS implementuje určitá omezení na soubory, které můžou vytvořit nebo získat přístup a poskytuje také speciální funkce pro určité adresáře. Tento článek obsahuje přehled těchto omezení a funkce a ukazuje, jak funguje přístup k souborům aplikace pro Xamarin.iOS.

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[Práce s obrázky](~/ios/app-fundamentals/images-icons/index.md)

Tento článek zkoumá použití obrázků v Xamarin.iOS, podporu obrázky aplikace (např. ikony, načítají se obrázky, atd.) a obrázků v rámci aplikace (například Image použít na ovládací prvky). Také popisuje, jak začlenit obrázky pomocí sady Visual Studio pro Mac, jakož i jak pracovat s obrázky z kódu.

## <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[Lokalizace](~/ios/app-fundamentals/localization/index.md)

Tento průvodce popisuje součet kódování do aplikace Xamarin.iOS pro podporu mezinárodní prostředí.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[Práce se seznamy vlastností](~/ios/app-fundamentals/index.md)

Toto téma představuje sady Visual Studio pro Mac grafické a rozšířené vlastnosti seznamu (.plist) editor pro práci s Info.plist a do souboru Entitlements.plist. Ukazuje nastavení ikon a spouštěcí Image pro aplikace pro iOS a ukazuje možnosti aplikace zadání (oprávnění) z v sadě Visual Studio pro Mac.

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[Práce s zabezpečení a ochrana osobních údajů](~/ios/app-fundamentals/security-privacy.md)

Apple bylo provedeno několik vylepšení zabezpečení a ochrana osobních údajů v iOS 10 (nebo novější), které vám pomůžou developer zlepšit zabezpečení svých aplikací a zajištění ochrany osobních údajů koncového uživatele. Tento článek se zabývá implementaci těchto funkcí v aplikaci pro Xamarin.iOS.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[Dělení na vlákna](~/ios/app-fundamentals/threading.md)

Tento článek popisuje dělení na vlákna aplikace pro Xamarin.iOS a hovoří o něco o fondu vláken .NET, responzivní aplikace a systém kolekce paměti.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[Dotykové ovládání](~/ios/app-fundamentals/touch/index.md)

Dotykové obrazovky na množství dnešních zařízení umožňují uživatelům rychle a efektivně komunikovat se zařízeními ve fyzických a intuitivní způsob. Tato interakce není omezena pouze na jednoduché touch detekce – je možné použít i gesta. Přiblížení roztažením gesta například je velmi Běžným příkladem tohoto – sevření části obrazovky se dvěma prsty, které uživatel může zvětšit nebo zmenšit. Tento průvodce zkontroluje dotyky a gesta v iOS.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[Práce s výchozí nastavení uživatele](~/ios/app-fundamentals/user-defaults.md)

`NSUserDefaults` Třída poskytuje způsob, jak pro iOS aplikace a rozšíření programově interagovat s výchozí systém celý systém. S použitím výchozí nastavení systému, může uživatel nakonfigurovat chování vaší aplikace nebo pro používání stylů pro splnění jejich předvoleb (podle návrhu aplikace). Chcete-li například prezentovat data v sadě Visual Studio metriky měření imperiální nebo vyberte danou motiv uživatelského rozhraní.
