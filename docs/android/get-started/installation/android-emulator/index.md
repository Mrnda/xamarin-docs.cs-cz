---
title: Instalační program emulátor Google Android
description: Emulátor Google Android se může spouštět v různých konfiguracích k simulaci různých zařízení. Tato příručka vysvětlují, jak připravit emulátoru systému Android pro testování vaší aplikace.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: e5ba2cc23ea9751ca60644d3eb5b7e3f31bbb6bb
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732528"
---
# <a name="google-android-emulator-setup"></a>Instalační program emulátor Google Android

_Tato příručka vysvětlují, jak připravit emulátor Google Android pro testování vaší aplikace._


## <a name="overview"></a>Přehled

Emulátor Google Android se může spouštět v různých konfiguracích k simulaci různých zařízení. Každá konfigurace se nazývá _virtuální zařízení_. Při nasazení a testování aplikace v emulátoru, vyberete předem nakonfigurované nebo vlastní virtuální zařízení, která simuluje fyzické zařízení Android například phone Nexus nebo pixelů.

Níže uvedené oddíly popisují, jak zrychlit emulátor Google Android pro maximální výkon, jak vytvářet a přizpůsobovat virtuální zařízení pomocí Správce zařízení Android a postup přizpůsobení vlastností profilu virtuálního zařízení. Kromě toho část s řešením potíží vysvětluje běžných problémů s instalací a alternativní řešení.

## <a name="sections"></a>Oddíly

### <a name="hardware-acceleration-for-emulator-performanceandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Hardwarovou akceleraci emulátoru výkonu](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Postup přípravy počítače pro maximální výkon emulátoru systému Android.
Protože emulátor Google Android může být pomalé prohibitively bez hardwarové akcelerace, doporučujeme povolit hardwarovou akceleraci v počítači dříve, než použijete tento emulátor.

### <a name="managing-virtual-devices-with-the-android-device-managerandroidget-startedinstallationandroid-emulatordevice-managermd"></a>[Správa virtuálního zařízení pomocí Správce zařízení Android](~/android/get-started/installation/android-emulator/device-manager.md)

Jak vytvářet a přizpůsobovat virtuální zařízení pomocí Správce zařízení Android.

### <a name="editing-android-virtual-device-propertiesandroidget-startedinstallationandroid-emulatordevice-propertiesmd"></a>[Úprava vlastností virtuálního zařízení se systémem Android](~/android/get-started/installation/android-emulator/device-properties.md)

Jak upravit vlastnosti profilu virtuálního zařízení se systémem Android pomocí Správce zařízení Android.

### <a name="troubleshooting-emulator-setup-problemsandroidget-startedinstallationandroid-emulatortroubleshootingmd"></a>[Odstraňování problémů s instalací emulátoru](~/android/get-started/installation/android-emulator/troubleshooting.md)

Jak diagnostikovat a opravit problémy Správce zařízení Android, které mohou nastat při nastavení emulátoru systému Android.


Po nakonfigurování emulátoru systému Android, najdete v části [ladění pomocí emulátor Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md) informace o tom, jak spusťte emulátor a použít jej pro účely testování a ladění aplikace.


> [!NOTE]
> Od verze nástroje pro Android SDK **26.0.1** a novější, Google má odebranou podporu pro existující správce AVD/SDK považuje jejich nových nástrojů rozhraní příkazového řádku (rozhraní příkazového řádku). Z důvodu této změny vyřazení správci Xamarin SDK nebo zařízení teď používají místo správci Google SDK nebo zařízení pro nástroje pro Android 26.0.1 a novější. Další informace o Správci Xamarin SDK najdete v tématu [Android SDK instalace](~/android/get-started/installation/android-sdk.md).

