---
title: Ladění Xamarin.Android na zařízeních a emulátorů
description: Postup testování a ladění aplikace Xamarin.Android
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 1ed6ec57365c5d3a861dd3fd947a2ad195ce5357
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935383"
---
# <a name="debugging"></a>Ladění

Tato část popisuje postup ladění aplikace Xamarin.Android na zařízeních nebo emulátorů.

## <a name="debugging-overview"></a>Přehled ladění

Vývoj aplikací pro Android vyžaduje spuštění aplikace, buď na fyzickém hardwaru nebo použití emulátoru. Použití hardwaru je nejlepší metodou, ale ne vždy nejvhodnější. V mnoha případech může být jednodušší a nákladově efektivní simulovat nebo emulovat Android hardwaru pomocí jedné z emulátorů popsané dole.

### <a name="debugging-on-the-android-emulatorandroiddeploy-testdebuggingdebug-on-emulatormd"></a>[Ladění na emulátoru systému Android](~/android/deploy-test/debugging/debug-on-emulator.md)

Tento článek vysvětluje, jak spustit Android emulátor ze sady Visual Studio a spusťte aplikaci v virtuálního zařízení.

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[Ladění na zařízení](~/android/deploy-test/debugging/debug-on-device.md)

Tento článek ukazuje, jak nakonfigurovat fyzického zařízení Android, tak, aby aplikace pro Xamarin.Android se dá nasadit na ji přímo v sadě Visual Studio nebo Visual Studio nebo Mac.

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Protokol ladění Androidu](~/android/deploy-test/debugging/android-debug-log.md)

Jeden velmi běžné efektu vývojáři použít k ladění aplikací používá `Console.WriteLine`. Však na mobilní platformu jako Android neexistuje žádné konzoly. Zařízení se systémem Android poskytuje protokolu, který budete pravděpodobně muset využít při zápisu aplikace. To se někdy označuje jako **logcat** z důvodu příkaz zadali ho Pokud chcete zjistit. Tento článek popisuje způsob použití **logcat**.

> [!WARNING]
> Všimněte si, že **Xamarin Android Player** je zastaralá. Další informace najdete v tématu [oznámení v tomto příspěvku na blogu](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/). Kromě toho **Visual Studio Android Emulator** je zastaralá od Visual Studio 2017.
