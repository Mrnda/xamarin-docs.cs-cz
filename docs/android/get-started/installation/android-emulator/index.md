---
title: "Instalační program emulátoru systému Android"
description: "Tato část popisuje postup přípravy emulátoru sady SDK pro Android pro testování vaší aplikace. Vysvětluje, jak zrychlit emulátor pro maximální výkon a ukazuje, jak pomocí Správce emulátoru vytvářet a přizpůsobovat virtuální zařízení."
ms.topic: article
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/25/2018
ms.openlocfilehash: 854ca06abc8be2f55f3e95a8ac3bd87c78af19cf
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="android-emulator-setup"></a>Instalační program emulátoru systému Android

_Tato část popisuje postup přípravy emulátoru sady SDK pro Android pro testování vaší aplikace. Vysvětluje, jak zrychlit emulátor pro maximální výkon a ukazuje, jak pomocí Správce emulátoru vytvářet a přizpůsobovat virtuální zařízení._


## <a name="overview"></a>Přehled

Emulátor Google Android SDK můžete spustit v různých konfiguracích k simulaci různých zařízení. Každé z nich tyto konfigurace je vytvořen jako _virtuální zařízení_. V tomto průvodci se dozvíte, jak zrychlit emulátoru Android pro lepší výkon a vytvořit virtuální zařízení pomocí Správce emulátoru Android Xamarin nebo starší verze správce emulátorů Google.


> [!NOTE]
> **Poznámka:** od verze nástroje pro Android SDK **26.0.1** a novější, Google má odebranou podporu pro existující správce AVD/SDK považuje jejich nových nástrojů rozhraní příkazového řádku (rozhraní příkazového řádku). Z důvodu této změny vyřazení správci Xamarin SDK nebo zařízení teď používají místo Google emulátoru sady SDK nebo správci pro nástroje pro Android 26.0.1 a novější. (Další informace o Správci Xamarin SDK najdete v tématu [Android SDK instalace](~/android/get-started/installation/android-sdk.md)).


## <a name="sections"></a>Oddíly

### <a name="hardware-accelerationandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Hardwarová akcelerace](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Postup přípravy počítače pro maximální výkon emulátoru sady SDK pro Android. Protože emulátoru Android SDK může být pomalé prohibitively bez hardwarové akcelerace, doporučujeme povolit hardwarovou akceleraci v počítači před použitím emulátoru Android SDK.

### <a name="xamarin-android-device-managerandroidget-startedinstallationandroid-emulatorxamarin-device-managermd"></a>[Správce zařízení Xamarin Android](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)

Jak používat Správce zařízení Xamarin Android vytvářet a přizpůsobovat virtuální zařízení Android emulátoru sady SDK. **Správce zařízení Xamarin Android**, aktuálně ve verzi preview, je určena k nahrazení starší verze správce emulátorů Google. Pokud cílíte na Android Oreo 8.0 nebo novější, musíte použít Správce zařízení Xamarin Android namísto správce emulátorů Google.

### <a name="google-emulator-managerandroidget-startedinstallationandroid-emulatorgoogle-emulator-managermd"></a>[Google Emulator Manager](~/android/get-started/installation/android-emulator/google-emulator-manager.md)

Jak používat starší verze správce emulátorů Google vytvářet a přizpůsobovat virtuální zařízení Android emulátoru sady SDK. Můžete spustit emulátor Google Android s správce emulátorů původní Google zbývající na verzi nástroje pro Android SDK 25.2.5 nebo nižší.

Po nakonfigurování emulátoru sady SDK pro Android, najdete v části [emulátoru Android SDK](~/android/deploy-test/debugging/android-sdk-emulator/index.md) informace o tom, jak spusťte emulátor a použít jej pro účely testování a ladění aplikace.
