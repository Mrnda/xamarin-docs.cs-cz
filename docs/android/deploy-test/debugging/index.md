---
title: "Ladění"
description: "Postup testování a ladění aplikace Xamarin.Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: feb35c041349f3ce78490c8a2fc6a829f9d84a6d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="debugging"></a>Ladění

## <a name="debuggin-overview"></a>Přehled Debuggin

Vývoj aplikací pro Android vyžaduje spuštění aplikace, buď na fyzickém hardwaru nebo použití emulátor ani simulátor. Použití hardwaru je nejlepší metodou, ale ne vždy nejvhodnější. V mnoha případech může být jednodušší a nákladově efektivní simulovat nebo emulovat Android hardwaru pomocí jedné z emulátorů popsané dole.


### <a name="android-sdk-emulatorandroiddeploy-testdebuggingandroid-sdk-emulatorindexmd"></a>[Emulátor sady SDK pro Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

Tyto články vysvětlují použití výchozí emulátor, který je k dispozici s SDK pro Android. Tento emulátor je k dispozici pro Visual Studio pro Windows a Visual Studio for Mac.

### <a name="visual-studio-android-emulatorandroiddeploy-testdebuggingvisual-studio-android-emulatormd"></a>[Visual Studio Android Emulator](~/android/deploy-test/debugging/visual-studio-android-emulator.md)

Tento článek vysvětluje, jak ladit a testovat aplikace Xamarin.Android pomocí emulátoru systému Android, která je integrována do sady Visual Studio 2015. Tento emulátor je vhodné použít, pokud používáte Visual Studio 2015 a není nutné profily vlastní zařízení.

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[Ladění na zařízení](~/android/deploy-test/debugging/debug-on-device.md)

Tento článek ukazuje, jak nakonfigurovat fyzického zařízení Android, tak, aby aplikace pro Xamarin.Android se dá nasadit na ji přímo v sadě Visual Studio nebo Visual Studio nebo Mac.

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Protokol pro Android ladění](~/android/deploy-test/debugging/android-debug-log.md)

Jeden velmi běžné efektu vývojáři použít k ladění aplikací používá `Console.WriteLine`. Však na mobilní platformu jako Android neexistuje žádné konzoly. Zařízení se systémem Android poskytuje protokolu, který budete pravděpodobně muset využít při zápisu aplikace. To se někdy označuje jako **logcat** z důvodu příkaz zadali ho Pokud chcete zjistit. Tento článek popisuje způsob použití **logcat**.

> [!WARNING]
> Všimněte si, že **Xamarin Android Player** je zastaralá. Další informace najdete v tématu [oznámení v tomto příspěvku na blogu](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/).
