---
title: Jaké ovladače USB jsou nutné k ladění Android v systému Windows?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/19/2017
ms.openlocfilehash: 77a7f50ab9d8f351dcefcbbdd50e88e18a13645d
ms.sourcegitcommit: c9ebf456e1c6924956bedb13f4ea78ff09f7b1a0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/04/2018
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Jaké ovladače USB jsou nutné k ladění Android v systému Windows?

## <a name="finding-usb-drivers"></a>Hledání ovladače USB

Chcete-li ladit na zařízení s Androidem při vývoji v systému Windows. musíte nainstalovat kompatibilní ovladač USB. Android SDK Manager zahrnuje "USB ovladač Google" ve výchozím nastavení, která přidává podporu pro zařízení Nexus podle postupu popsaného tady: [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

Jiná zařízení vyžadují ovladače USB konkrétně publikováno výrobce zařízení. U většiny běžných výrobců některé odkazy jsou zahrnuté v tomto průvodci: [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Alternativy

V závislosti na manfacturer může být obtížné sledovat přesný ovladač USB potřeby. Některé alternativami pro testování aplikací pro Android vyvinuté v systému Windows včetně pomocí emulátoru Androidu nebo pomocí externích služeb testování. Mezi ty patří:

- [Testovací aplikace Center](https://docs.microsoft.com/appcenter/test-cloud/) – cloudové služby, spusťte na stovky skutečných Android zařízení testování.

- [Emulátor sady Visual Studio pro Android](https://www.visualstudio.com/en-us/features/msft-android-emulator-vs.aspx)

- [Emulátor Google Android SDK](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

