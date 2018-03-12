---
title: "Změny nástrojů sady SDK pro Android"
description: "Jak spravuje SDK pro Android nainstalovaná úrovně rozhraní API a AVDs změny."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 69e9f08870a01c056951700978d07277af5edfa8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="changes-to-the-android-sdk-tooling"></a>Změny nástrojů sady SDK pro Android

_Jak spravuje SDK pro Android nainstalovaná úrovně rozhraní API a AVDs změny._

## <a name="changes-to--android-sdk-tooling"></a>Změny nástrojů sady SDK pro Android

V moderním verzích nástroje SDK pro Android, Google odebral stávající správce AVD a sady SDK pro nové _rozhraní příkazového řádku_ nástrojů (CLI). První **android** odebrala program a správci grafickým uživatelským rozhraním (grafické uživatelské rozhraní) v sadě Visual Studio pro Mac a starší verze Xamarin pro Visual Studio přestane fungovat po verze nástroje pro Android SDK.


![Android nabídky IDE v sadě Visual Studio](sdk-cli-tooling-changes-images/android-ide-menu.png)

Pokus o použití **android** program prostřednictvím příkazového řádku způsobí chybovou zprávu takto:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

Proto budete muset použít rozhraní příkazového řádku nástroje pro správu a aktualizovat emulátorů a sady SDK pro Android.

### <a name="cli-tools"></a>Nástrojů příkazového řádku

Následující programy nyní tvoří rozhraní příkazového řádku nástroje Android SDK:

#### <a name="sdkmanager"></a>sdkmanager

**Přidáno v:** Android SDK Tools 25.2.3 (od listopadu 2016) a vyšší.

Je nový program s názvem **sdkmanager** v **nástroje/bin** složky Android SDK. Tento nástroj se používá k udržování SDK pro Android na příkazovém řádku. Další informace o použití tohoto nástroje najdete v tématu [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Přidáno v:** Android SDK Tools 25.3.0 (březen 2017) a vyšší.

Je nový program s názvem **avdmanager** v **nástroje/bin** složky Android SDK. Tento nástroj se používá k udržování AVD pro emulátor Google Android. Další informace o použití tohoto nástroje najdete v tématu [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Přechod na starší verzi

Může ponížit vaše **nástroje pro Android SDK** verzi instalaci předchozí verze sady SDK pro Android z [webu pro vývojáře pro Android](https://developer.android.com/studio/index.html).

### <a name="using-the-old-gui"></a>Pomocí staré grafického uživatelského rozhraní

Můžete dál používat původní grafického uživatelského rozhraní tak, že spustíte **android** programu uvnitř vaší **nástroje** složky, pokud jsou na **nástroje pro Android SDK** verze **25.2.5**  nebo nižší.


## <a name="related-links"></a>Související odkazy

- [Instalace sady Android SDK](~/android/get-started/installation/android-sdk.md)
- [Principy úrovní rozhraní API systému Android](~/android/app-fundamentals/android-api-levels.md)
- [Sady SDK nástroje poznámky k verzi (Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/sdkmanager.html)