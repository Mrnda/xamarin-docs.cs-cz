---
title: "Nasazení a testování"
description: "Ustálení a příruček nasazení"
ms.topic: article
ms.prod: xamarin
ms.assetid: 92B35AB1-7AB7-3D3B-DB31-CC971E0B43AE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 0c152c389a6aa62512882863cd2830b436587475
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="deployment-and-testing"></a>Nasazení a testování

Tato část obsahuje témata týkající se používá k testování aplikace a její distribuce. Témata v tomto poli mezi patří například nástroje používané pro ladění, nasazení a testery, jak publikovat aplikaci do obchodu s aplikacemi.


##  <a name="app-distributioniosdeploy-testapp-distributionindexmd"></a>[Distribuce aplikací](~/ios/deploy-test/app-distribution/index.md)

Tento článek ukazuje, jak nakonfigurovat, vytvářet a publikovat aplikace pro Xamarin.iOS pro distribuci přes různé jiné prostředky, včetně:

- [Distribuce obchodu s aplikacemi](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Distribuce interní (Enterprise)](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Ad Hoc distribuce](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)

##  <a name="ipa-deploymentiosdeploy-testapp-distributionipa-supportmd"></a>[Soubor IPA nasazení](~/ios/deploy-test/app-distribution/ipa-support.md)

Ad Hoc a nasazení v podnicích umožňují vývojářům vytvořit balíčky, které mohou být distribuovány pro testování nebo pro uživatele interní společnosti. Tento dokument vysvětluje, jak vytvořit IPA, které mohou být synchronizovány do zařízení s iOS pomocí iTunes.

## <a name="provisioningprovisioningindexmd"></a>[Zřizování](provisioning/index.md)

Tato sada příručky obsahuje podepisování kódu a zřizování essentials, například práce s seznamů vlastností a jak zřídit vaší aplikace pro služby aplikací. 

## <a name="wireless-deploymentwireless-deploymentmd"></a>[Bezdrátová nasazení](wireless-deployment.md)

 Xcode 9 zavedla možnost nasazení na zařízení s iOS nebo Apple TV prostřednictvím sítě, místo nutnosti pevně připojené zařízení pokaždé, když chcete nasazení a ladění aplikace. Tato funkce je aktuálně ve verzi preview.

##  <a name="testflightiosdeploy-testtestflightmd"></a>[TestFlight](~/ios/deploy-test/testflight.md)

TestFlight je nyní vlastněná společností Apple a je primární způsob, jak beta testování aplikace Xamarin.iOS. Tento článek vás provede všechny kroky procesu TestFlight – z vaší aplikace se nahrávají na práci s iTunes připojit.

##  <a name="debugging-in-xamariniosiosdeploy-testdebugging-in-xamarin-iosmd"></a>[Ladění aplikace Xamarin.iOS](~/ios/deploy-test/debugging-in-xamarin-ios.md)

Visual Studio a Visual Studio pro Mac integrovaného vývojového prostředí zahrnují podporu pro ladění aplikace Xamarin.iOS v simulátoru iOS i na zařízeních s iOS. Tento článek ukazuje, jak pomocí ladicího programu a také jak nakonfigurovat různé možnosti, kterou podporuje.


##  <a name="touchunitiosdeploy-testtouchunitmd"></a>[Touch.Unit](~/ios/deploy-test/touch.unit.md)

Tento dokument popisuje, jak vytvářet testy částí pro Xamarin.iOS projekty.
Testování částí pomocí Xamarin.iOS se provádí pomocí rozhraní Touch.Unit, která obsahuje oba iOS test runner, jakož i upravenou verzi [NUnitLite](http://www.nunitlite.com/) rozhraní, které poskytují obeznámeni sadu rozhraní API pro zápis testů částí.



##  <a name="using-instruments-to-detect-native-leaks-using-markheapiosdeploy-testusing-instruments-to-detect-native-leaks-using-markheapmd"></a>[Použití nástrojů pro zjištění nativní nevracení pomocí MarkHeap](~/ios/deploy-test/using-instruments-to-detect-native-leaks-using-markheap.md)

V tomto článku popisují způsob použití nástroje na jakékoli zařízení s iOS a všechny aplikace pro Xamarin.iOS. Vypadá to také o tom, jak na profil aplikace v simulátoru.



##  <a name="walkthrough---using-apples-instrument-tooliosdeploy-testwalkthrough-apples-instrumentmd"></a>[Návod – pomocí nástroje nástrojích společnosti Apple](~/ios/deploy-test/walkthrough-apples-instrument.md)

Tento článek vás provede použití společnosti Apple nástrojů nástroje pro diagnostiku problémů paměti v aplikaci pro systém iOS vytvořených pomocí Xamarinu. Ukazuje, jak spustit nástroje, pořizovat snímky haldy a analyzovat růstu paměti. Také ukazuje, jak používat nástroje k zobrazení a přesně určit přesnou řádky kódu, které způsobí problém paměti.

##  <a name="linking-on-ioslinkermd"></a>[Propojení v systému iOS](linker.md)

Vysvětluje, jak funguje linkeru zajistit nejmenší možné aplikace balíček, jak dobře upravit jeho nastavení a využití.

##  <a name="xamarinios-performanceperformancemd"></a>[Výkon Xamarin.iOS](performance.md)

Existuje mnoho postupů pro zvýšení výkonu aplikace sestavené s Xamarin.iOS. Tyto postupy souhrnně může výrazně snížit objem práce využití procesoru a paměti spotřebovávají aplikace.

##  <a name="mtouchmtouchmd"></a>[mtouch](mtouch.md)

Informace o mtouch.exe, nástroje příkazového řádku, který sestaví projekt do použitelného aplikace v iOS a poznámky.

## <a name="ios-build-mechanicsios-build-mechanicsmd"></a>[iOS mechanismy sestavení](ios-build-mechanics.md)

Tato příručka popisuje postup čas aplikace a jak používat metody, které mohou být použity pro rychlejší sestavení pro všechny konfigurace sestavení.