---
title: Instalační program emulátoru systému Android
description: Emulátoru systému Android se může spouštět v různých konfiguracích k simulaci různých zařízení. Tato příručka vysvětlují, jak připravit emulátoru systému Android pro testování vaší aplikace.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: f281227ae6ee17548e9c4653d52c7ae6d2bfff2d
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935032"
---
# <a name="android-emulator-setup"></a>Instalační program emulátoru systému Android

_Tato příručka vysvětlují, jak připravit emulátoru systému Android pro testování vaší aplikace._


## <a name="overview"></a>Přehled

Emulátoru systému Android se může spouštět v různých konfiguracích k simulaci různých zařízení. Každá konfigurace se nazývá _virtuální zařízení_. Při nasazení a testování aplikace v emulátoru, vyberete předem nakonfigurované nebo vlastní virtuální zařízení, která simuluje fyzické zařízení Android například phone Nexus nebo pixelů.

Níže uvedené oddíly popisují, jak zrychlit emulátoru Android pro maximální výkon, jak vytvářet a přizpůsobovat virtuální zařízení pomocí Správce zařízení Android a postup přizpůsobení vlastností profilu virtuálního zařízení. Kromě toho část s řešením potíží vysvětluje běžné emulátoru problémy a řešení.

## <a name="sections"></a>Oddíly

### <a name="hardware-acceleration-for-emulator-performanceandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Hardwarovou akceleraci emulátoru výkonu](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Postup přípravy počítače pro maximální výkon emulátoru systému Android.
Protože emulátoru systému Android může být pomalé prohibitively bez hardwarové akcelerace, doporučujeme povolit hardwarovou akceleraci v počítači před použitím emulátor.

### <a name="managing-virtual-devices-with-the-android-device-managerandroidget-startedinstallationandroid-emulatordevice-managermd"></a>[Správa virtuálního zařízení pomocí Správce zařízení Android](~/android/get-started/installation/android-emulator/device-manager.md)

Jak vytvářet a přizpůsobovat virtuální zařízení pomocí Správce zařízení Android.

### <a name="editing-android-virtual-device-propertiesandroidget-startedinstallationandroid-emulatordevice-propertiesmd"></a>[Úprava vlastností virtuálního zařízení se systémem Android](~/android/get-started/installation/android-emulator/device-properties.md)

Jak upravit vlastnosti profilu virtuálního zařízení pomocí Správce zařízení Android.

### <a name="android-emulator-troubleshootingandroidget-startedinstallationandroid-emulatortroubleshootingmd"></a>[Řešení potíží s emulátoru systému Android](~/android/get-started/installation/android-emulator/troubleshooting.md)

V tomto článku nejběžnější zprávy upozornění a problémy, ke kterým dochází při spuštění emulátoru systému Android jsou popsány, společně s alternativní řešení a tipů.

Po nakonfigurování emulátoru systému Android, najdete v části [ladění na emulátoru systému Android](~/android/deploy-test/debugging/debug-on-emulator.md) informace o tom, jak spusťte emulátor a použít jej pro účely testování a ladění aplikace.


> [!NOTE]
> Od verze nástroje pro Android SDK **26.0.1** a novější, Google má odebranou podporu pro existující správce AVD/SDK považuje jejich nových nástrojů rozhraní příkazového řádku (rozhraní příkazového řádku). Z důvodu této změny vyřazení správci Xamarin SDK nebo zařízení teď používají místo správci Google SDK nebo zařízení pro nástroje pro Android 26.0.1 a novější. Další informace o Správci Xamarin SDK najdete v tématu [nastavení Android SDK pro Xamarin.Android](~/android/get-started/installation/android-sdk.md).

