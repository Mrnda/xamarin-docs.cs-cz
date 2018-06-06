---
title: v Xamarin – Internals tvOS
description: Dokumenty popisující do interní chodu tvOS na Xamarin, která je založena na Xamarin.iOS. Odkaz obsahu popisuje sestavení, cílové architektury a související s koncepty iOS.
ms.prod: xamarin
ms.assetid: 8C076FED-9C03-44DE-9723-0E20272DD16B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 0132eac4edd4ecb9f693828bd58288dfbcb1c008
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789142"
---
# <a name="tvos-in-xamarin--internals"></a>v Xamarin – Internals tvOS 

##  <a name="assembliesiostvosinternalsassembliesmd"></a>[Sestavení](~/ios/tvos/internals/assemblies.md)

Seznam sestavení podporuje Xamarin pro vaše aplikace Xamarin.tvOS.

##  <a name="target-frameworksiostvosinternalsframeworksmd"></a>[Cílové architektury](~/ios/tvos/internals/frameworks.md)

Tento článek popisuje typy cílové rozhraní (základní knihovny tříd), které jsou k dispozici v Xamarin.tvOS a výběr specifické cíle pro vaši aplikaci Xamarin.tvOS důsledky.

## <a name="related-ios-articles"></a>IOS související články

V následujících článcích jsou specifické pro iOS, ale relevantní pro tvOS, (protože tvOS 9 je podmnožinou iOS 9).

###  <a name="unified-apicross-platformmaciosunifiedindexmd"></a>[Unified API](~/cross-platform/macios/unified/index.md)

Zavádí nové jednotné rozhraní API díky pro základy i zavedení podpory pro rozhraní API 64bitová verze a 64bitová verze kompilace kódu jednodušší kód sdílení mezi Apple TV a iOS.  

###  <a name="api-designiosinternalsapi-designindexmd"></a>[Návrh rozhraní API](~/ios/internals/api-design/index.md)

Vysvětluje principy návrhu za vazby rozhraní API.

###  <a name="limitationsiosinternalslimitationsmd"></a>[Omezení](~/ios/internals/limitations.md)

Tato část ukazuje nástrahy a omezení znát s namapoval Xamarin.iOS, řada z nich se vztahují na Xamarin.tvOS.

###  <a name="linkeriosdeploy-testlinkermd"></a>[Linker](~/ios/deploy-test/linker.md)

Vysvětluje, jak funguje linkeru zajistit nejmenší možné aplikace balíčku, dobře, jak upravit nastavení jeho a využití.

###  <a name="localization-and-internationalizationiosapp-fundamentalslocalizationindexmd"></a>[Lokalizace a internacionalizace](~/ios/app-fundamentals/localization/index.md)

Tento průvodce popisuje přidání kódování pro aplikace pro Xamarin.iOS pro podporu internacionalizace.

###  <a name="mtouchiosdeploy-testmtouchmd"></a>[mtouch](~/ios/deploy-test/mtouch.md)

Informace o mtouch.exe, nástroje příkazového řádku, který sestaví projekt do použitelného aplikace v iOS a poznámky.

###  <a name="linking-native-librariesiosplatformnative-interopmd"></a>[Propojování nativní knihovny](~/ios/platform/native-interop.md)

Xamarin.iOS podporuje propojení s nativní knihovny jazyka C a knihovny jazyka Objective-C. Tento dokument popisuje, jak propojit váš nativní knihovny jazyka C s projektu Xamarin.iOS. Informace o provádění stejný pro knihovny jazyka Objective-C, najdete v článku&nbsp; [vazby typy jazyka Objective-C](~/ios/platform/binding-objective-c/index.md)&nbsp;dokumentu.

##  <a name="objective-c-selectorsiosinternalsobjective-c-selectorsmd"></a>[Selektory jazyka Objective-C](~/ios/internals/objective-c-selectors.md)

Poznámky a využití pro přímé volání jazyka Objective-C selektory (metody).

###  <a name="systemdataiosdata-cloudsystemdatamd"></a>[System.Data](~/ios/data-cloud/system.data.md)

Informace a pokyny týkající se použití System.Data pro přístup k předdefinovaného systémového databáze SQLite.

###  <a name="threadingiosapp-fundamentalsthreadingmd"></a>[Dělení na vlákna](~/ios/app-fundamentals/threading.md)

Poznámky k použití vlákna v rámci aplikace Xamarin.iOS.

###  <a name="xib-code-generationiosinternalsxib-code-generationmd"></a>[Generování kódu XIB](~/ios/internals/xib-code-generation.md)

Jak Visual Studio pro Mac se integruje s Tvůrce Xcode je rozhraní, ve kterém vám umožní používat rozhraní tvůrce návrh uživatelského rozhraní.

## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
