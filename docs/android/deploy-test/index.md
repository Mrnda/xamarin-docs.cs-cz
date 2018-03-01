---
title: "Nasazení a testování"
description: "Tato část obsahuje pokyny, které popisují, jak testovat aplikaci a optimalizovat jeho výkon, příprava pro verzi, podepište ho pomocí certifikátu a publikování na obchod s aplikacemi"
ms.topic: article
ms.prod: xamarin
ms.assetid: 568C0B85-EFF3-AF6F-5605-95055193D367
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 5ddc2a258ad09de2cdd8214dceb533441812ae54
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="deployment-and-testing"></a>Nasazení a testování

Tato část obsahuje pokyny, které popisují, jak testovat aplikaci a optimalizovat jeho výkon, příprava pro verzi, podepište ho pomocí certifikátu a publikování na obchod s aplikacemi.


##  <a name="application-package-sizesapp-package-sizemd"></a>[Velikost balíčku aplikace](app-package-size.md)

Tento článek prozkoumá základní součástí balíčku aplikace Xamarin.Android a přidružené strategií, které lze použít pro nasazení balíčku efektivní během ladění a vydání fázích vývoje.

##  <a name="building-appsbuilding-appsindexmd"></a>[Vytváření aplikací](building-apps/index.md)

Tato část popisuje, jak funguje procesu sestavení a vysvětluje, jak sestavit APKs ABI specifické.

##  <a name="command-line-emulatorcommand-line-emulatormd"></a>[Emulátor příkazového řádku](command-line-emulator.md)

Tento článek stručně dotykem spouštění v emulátoru prostřednictvím příkazového řádku.

## <a name="debuggingandroiddeploy-testdebuggingindexmd"></a>[Ladění](~/android/deploy-test/debugging/index.md)

Postupy v části pomoci při ladění aplikace pomocí Android emulátorů, skutečně Android zařízení a protokol ladění.

##  <a name="setting-the-debuggable-attributeandroiddeploy-testdebuggable-attributemd"></a>[Nastavení Debuggable atribut](~/android/deploy-test/debuggable-attribute.md)

Tento článek vysvětluje, jak nastavit debuggable atribut, který nástroje jako `adb` může komunikovat s systém JVM.

##  <a name="environmentenvironmentmd"></a>[Prostředí](environment.md)

Tento článek popisuje prostředí pro spuštění Xamarin.Android a vlastnosti systému Android, které ovlivňují spuštění programu.

##  <a name="gdbgdbmd"></a>[GDB](gdb.md)

Tento článek vysvětluje, jak používat `gdb` pro ladění aplikace pro Xamarin.Android.

##  <a name="installing-a-system-appinstall-system-appmd"></a>[Instalace aplikace na systém](install-system-app.md)

Tato příručka vysvětluje, jak nainstalovat aplikace Xamarin.Android jako systémová aplikace na zařízení se systémem Android nebo jako součást vlastní ROM.

##  <a name="linking-on-androidlinkermd"></a>[Propojení v systému Android](linker.md)

Tento článek popisuje proces propojení používá ke snížení konečné velikosti aplikace Xamarin.Android. Popisuje různé úrovně propojení, které můžete provést a obsahuje některé pokyny a řešení potíží s Rady ke zmírnění chyby, které mohou být způsobeny pomocí linkeru.

## <a name="xamarinandroid-performanceandroiddeploy-testperformancemd"></a>[Výkon Xamarin.Android](~/android/deploy-test/performance.md)

Existuje mnoho postupů pro zvýšení výkonu aplikace sestavené s Xamarin.Android. Souhrnně těchto postupů může výrazně snížit objem práce využití procesoru a paměti spotřebovávají aplikace.

## <a name="preparing-an-application-for-releaseandroiddeploy-testrelease-prepindexmd"></a>[Příprava aplikace pro vydání](~/android/deploy-test/release-prep/index.md)

Po aplikaci je programového a otestovat, je nezbytné pro přípravu balíček pro distribuci. První úlohou Příprava tento balíček je vytvořit aplikaci pro verzi, která především zahrnuje nastavení některých atributů aplikací.

## <a name="signing-the-android-application-packageandroiddeploy-testsigningindexmd"></a>[Při podpisu balíčku aplikace pro Android](~/android/deploy-test/signing/index.md)

Naučte se vytvářet Android podepisování identity, vytvořte nový podpisový certifikát pro aplikace pro Android a podepsání aplikace s podpisový certifikát. Kromě toho toto téma vysvětluje, jak exportovat aplikaci na disk pro *ad-hoc* distribuce. Výsledný APK může být zkušebně načtené do zařízení s Androidem bez průchodu přes obchod s aplikacemi.

## <a name="publishing-an-applicationandroiddeploy-testpublishingindexmd"></a>[Publikování aplikace](~/android/deploy-test/publishing/index.md)

Tato řada článků vysvětluje kroky pro distribuci veřejného aplikace vytvořené pomocí Xamarin.Android. Distribuce možné provádět prostřednictvím kanálů například e-mailu, privátní webového serveru, webu Google Play nebo obchodu s aplikacemi Amazon pro Android.
