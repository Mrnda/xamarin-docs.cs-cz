---
title: Jaké ovladače USB jsou nutné k ladění Android v systému Windows?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: 7b3a4c2f807839897a099959fe3a6ea9ec25df78
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732746"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Jaké ovladače USB jsou nutné k ladění Android v systému Windows?

## <a name="finding-usb-drivers"></a>Hledání ovladače USB

Chcete-li ladit na zařízení s Androidem při vývoji v systému Windows. musíte nainstalovat kompatibilní ovladač USB. Android SDK Manager zahrnuje "USB ovladač Google" ve výchozím nastavení, která přidává podporu pro zařízení Nexus podle postupu popsaného tady: [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

Jiná zařízení vyžadují ovladače USB konkrétně publikováno výrobce zařízení. U většiny běžných výrobců některé odkazy jsou zahrnuté v tomto průvodci: [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Alternativy

V závislosti na manfacturer může být obtížné sledovat přesný ovladač USB potřeby. Některé alternativami pro testování aplikací pro Android vyvinuté v systému Windows včetně pomocí emulátoru Androidu nebo pomocí externích služeb testování. Mezi ty patří:

- [Testovací aplikace Center](https://docs.microsoft.com/appcenter/test-cloud/) – cloudové služby, spusťte na stovky skutečných Android zařízení testování.

- [Emulátor sady Visual Studio pro Android](https://www.visualstudio.com/en-us/features/msft-android-emulator-vs.aspx)

- [Ladění pomocí emulátor Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

