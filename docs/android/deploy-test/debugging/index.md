---
title: Ladění Xamarin.Android na zařízeních a emulátorů
description: Postup testování a ladění aplikace Xamarin.Android
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: a4ce36e28bd5b6dcf78d248b1f2ba951cad9b286
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="debugging"></a>Ladění

Tato část popisuje postup ladění aplikace Xamarin.Android na zařízeních nebo emulátorů.
## <a name="debugging-overview"></a>Přehled ladění

Vývoj aplikací pro Android vyžaduje spuštění aplikace, buď na fyzickém hardwaru nebo použití emulátoru. Použití hardwaru je nejlepší metodou, ale ne vždy nejvhodnější. V mnoha případech může být jednodušší a nákladově efektivní simulovat nebo emulovat Android hardwaru pomocí jedné z emulátorů popsané dole.


### <a name="google-android-emulatorandroiddeploy-testdebuggingandroid-sdk-emulatorindexmd"></a>[Emulátor Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

Tyto články vysvětlují použití výchozí emulátor, který je k dispozici s SDK pro Android. Tento emulátor je k dispozici pro Visual Studio pro Windows a Visual Studio for Mac.

### <a name="visual-studio-android-emulatorandroiddeploy-testdebuggingvisual-studio-android-emulatormd"></a>[Visual Studio Android Emulator](~/android/deploy-test/debugging/visual-studio-android-emulator.md)

Tento článek vysvětluje, jak ladit a testovat aplikace Xamarin.Android pomocí emulátoru systému Android, která je integrována do sady Visual Studio 2015. Tento emulátor je vhodné použít, pokud používáte Visual Studio 2015 a není nutné profily vlastní zařízení.

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[Ladění na zařízení](~/android/deploy-test/debugging/debug-on-device.md)

Tento článek ukazuje, jak nakonfigurovat fyzického zařízení Android, tak, aby aplikace pro Xamarin.Android se dá nasadit na ji přímo v sadě Visual Studio nebo Visual Studio nebo Mac.

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Protokol ladění Androidu](~/android/deploy-test/debugging/android-debug-log.md)

Jeden velmi běžné efektu vývojáři použít k ladění aplikací používá `Console.WriteLine`. Však na mobilní platformu jako Android neexistuje žádné konzoly. Zařízení se systémem Android poskytuje protokolu, který budete pravděpodobně muset využít při zápisu aplikace. To se někdy označuje jako **logcat** z důvodu příkaz zadali ho Pokud chcete zjistit. Tento článek popisuje způsob použití **logcat**.

> [!WARNING]
> Všimněte si, že **Xamarin Android Player** je zastaralá. Další informace najdete v tématu [oznámení v tomto příspěvku na blogu](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/).
