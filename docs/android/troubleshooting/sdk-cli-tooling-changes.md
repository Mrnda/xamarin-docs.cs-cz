---
title: Změny nástrojů sady SDK pro Android
description: Jak spravuje SDK pro Android nainstalovaná úrovně rozhraní API a AVDs změny.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/02/2018
ms.openlocfilehash: b5de9d673a348ddd4b939ae387257f835b37117a
ms.sourcegitcommit: c9ebf456e1c6924956bedb13f4ea78ff09f7b1a0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/04/2018
---
# <a name="changes-to-the-android-sdk-tooling"></a>Změny nástrojů sady SDK pro Android

_Jak spravuje SDK pro Android nainstalovaná úrovně rozhraní API a AVDs změny._

## <a name="changes-to-android-sdk-tooling"></a>Změny nástrojů sady SDK pro Android

V posledních verzích nástroje SDK pro Android se odebrala Google existující správce AVD a sady SDK považuje nové nástrojů rozhraní příkazového řádku (rozhraní příkazového řádku). **Android** program byl odebrán, a správci Google grafickým uživatelským rozhraním (grafické uživatelské rozhraní) v sadě Visual Studio pro Mac a starší verze Xamarin pro Visual Studio přestane fungovat po 25.2.5 verzi nástroje pro Android SDK. Například se pokouší použít **android** program prostřednictvím příkazového řádku způsobí chybovou zprávu takto:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

Následující části popisují, jak spravovat Android SDK a pomocí sady Android SDK 25.3.0 virtuální zařízení se systémem Android a novější.

### <a name="ui-tools"></a>Nástroje uživatelského rozhraní

Visual Studio a Visual Studio pro Mac teď poskytovat Xamarin náhrady – starší formáty manažerům Google grafického uživatelského rozhraní:

-   Pokud chcete stáhnout nástroje pro Android SDK, platformy a další součásti, které potřebujete pro vývoj aplikací Xamarin.Android, použijte [Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md) místo starší verze sady SDK Manager Google.

-   Chcete-li vytvořit a nakonfigurovat virtuální zařízení se systémem Android, použijte [Správce zařízení Xamarin Android](~/android/get-started/installation/android-emulator/xamarin-device-manager.md) místo starší verze emulátoru správce Google.

Tyto nástroje jsou funkčně rovnocenné využívající grafické rozhraní Google správci nahrazují.

### <a name="cli-tools"></a>Nástrojů příkazového řádku

Alternativně můžete nástrojů příkazového řádku pro správu a aktualizovat emulátorů a sady SDK pro Android. Následující programy nyní tvoří rozhraní příkazového řádku nástroje Android SDK:

#### <a name="sdkmanager"></a>sdkmanager

**Přidáno v:** Android SDK Tools 25.2.3 (od listopadu 2016) a vyšší.

Je nový program s názvem **sdkmanager** v **nástroje/bin** složky Android SDK. Tento nástroj se používá k udržování SDK pro Android na příkazovém řádku. Další informace o použití tohoto nástroje najdete v tématu [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Přidáno v:** Android SDK Tools 25.3.0 (březen 2017) a vyšší.

Je nový program s názvem **avdmanager** v **nástroje/bin** složky Android SDK. Tento nástroj se používá k udržování AVDs pro emulátor Google Android. Další informace o použití tohoto nástroje najdete v tématu [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Přechod na starší verzi

Může ponížit vaše **nástroje pro Android SDK** verzi instalaci předchozí verze sady SDK pro Android z [webu pro vývojáře pro Android](https://developer.android.com/studio/index.html).

### <a name="using-the-old-gui"></a>Pomocí staré grafického uživatelského rozhraní

Můžete dál používat původní grafického uživatelského rozhraní tak, že spustíte **android** programu uvnitř vaší **nástroje** složky, pokud jsou na **nástroje pro Android SDK** verze **25.2.5**  nebo nižší.


## <a name="related-links"></a>Související odkazy

- [Instalace sady Android SDK](~/android/get-started/installation/android-sdk.md)
- [Správce zařízení Android](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)
- [Principy úrovní rozhraní API systému Android](~/android/app-fundamentals/android-api-levels.md)
- [Sady SDK nástroje poznámky k verzi (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
