---
title: "Ladění Android opotřebení v emulátoru"
description: "Tyto články vysvětlují postup ladění aplikace Xamarin.Android opotřebení na emulátor."
ms.topic: article
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1ad3c193261bf22b7ee344aa1ccabb226533b907
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="debug-android-wear-on-an-emulator"></a>Ladění Android opotřebení v emulátoru

_Tyto články vysvětlují postup ladění aplikace Xamarin.Android opotřebení na emulátor._

## <a name="debug-wear-on-emulator-overview"></a>Ladění opotřebení emulátoru – přehled

Vývoj aplikací pro Android nosit vyžaduje spuštění aplikace, buď na fyzickém hardwaru nebo použití emulátor ani simulátor. Použití hardwaru je nejlepší metodou, ale ne vždy nejvhodnější. V mnoha případech může být jednodušší a nákladově efektivní simulovat nebo emulovat Android nosit hardwaru pomocí emulátoru, jak je popsáno níže. Pokud si nejste ještě v tématu obeznámeni s procesem nasazení a spuštění aplikace Android nosit [nosit Hello,](~/android/wear/get-started/hello-wear.md).

## <a name="configure-the-android-sdk-emulator"></a>Konfigurace emulátoru sady SDK pro Android

Ke spouštění vaší aplikace a opotřebením motoru na emulátoru, musíte nainstalovat Android emulátoru systému Android SDK a nakonfigurovat ji pro Android nosit. Celkové emulátoru Android SDK instalace a konfigurace informace najdete v tématu [emulátoru Android SDK](~/android/deploy-test/debugging/android-sdk-emulator/index.md).

Když vytvoříte virtuální zařízení a opotřebením motoru, vyberte profil zařízení Android nosit (například **Android opotřebení hranaté**). Pro lepší výkon použijte opotřebení **x86** CPU/ABI, jak je vidět v tomto příkladu:

[![Příklad opotřebení virtuální zařízení konfigurace](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png)


## <a name="launch-the-wear-virtual-device"></a>Spusťte opotřebení virtuálního zařízení 

Po vytvoření virtuálního zařízení s Androidem nosit, můžete zvolit z rozevírací nabídky zařízení v prostředí IDE před zahájením ladění. Pokud virtuální zařízení není k dispozici v zařízení v rozevíracím seznamu, ověřte, zda je váš projekt Android *nosit* projekt aplikace (ne projekt aplikace pro Android) a že jejím cílem úroveň rozhraní API je nastavená na rozhraní API stejné úrovni jako virtuální zařízení. Příklad:

[ ![Výběr AVD se nosit v nabídce zařízení sady Visual Studio](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png)

Po spuštění emulátoru systému Android, Xamarin.Android nasadí aplikaci opotřebení na emulátoru. Na emulátoru běží aplikace s bitovou kopií nakonfigurované virtuální zařízení.

Nemáte překvapeni, pokud se zobrazí tato (nebo jiné vkládaná obrazovky) na první. Emulátor sledovat může trvat dobu spuštění: 

![Podívejte se na emulátoru zobrazí, jenom několik minut...](debug-on-emulator-images/please-wait.png)

Emulátor, může být ponecháno systémem; není potřeba ho vypnout a restartovat při každém spuštění aplikace.

 
## <a name="summary"></a>Souhrn
 
Tato příručka vysvětluje postup konfigurace emulátoru Android SDK pro vývoj a opotřebením motoru a spuštění virtuálního zařízení opotřebení pro ladění.
