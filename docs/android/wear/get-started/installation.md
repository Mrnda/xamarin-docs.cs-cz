---
title: 'Instalace a nastavení nosit onXamarin.Android operačního systému '
description: Tento článek vás provede kroky instalace a podrobnosti o konfiguraci, které jsou potřebné při přípravě počítače a zařízení pro vývoj pro Android nosit. Na konci tohoto článku budete mít funkční Xamarin.Android opotřebení instalace integrovaná do sady Visual Studio pro Mac nebo Microsoft Visual Studio a budete připravení začít vytvářet svoji první aplikaci Xamarin.Android a opotřebením motoru.
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 14162663c518fdd1324f2b0340592fbae491d112
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436502"
---
# <a name="setup-and-installation"></a>Instalační program a instalace

_Tento článek vás provede kroky instalace a podrobnosti o konfiguraci, které jsou potřebné při přípravě počítače a zařízení pro vývoj pro Android nosit. Na konci tohoto článku budete mít funkční Xamarin.Android opotřebení instalace integrovaná do sady Visual Studio pro Mac nebo Microsoft Visual Studio a budete připravení začít vytvářet svoji první aplikaci Xamarin.Android a opotřebením motoru._

## <a name="requirements"></a>Požadavky

K vytvoření aplikace založené na Xamarinu Android nosit je potřeba následující text:

-   **Visual Studio nebo Visual Studio pro Mac** &ndash; jste Pokud používáte Visual Studio, Visual Studio 2015 Professional nebo novější je požadovaná.

-   **Xamarin.Android** &ndash; Xamarin.Android 4.17 nebo novější musí být nainstalovaná a nakonfigurovaná s Visual Studio nebo Visual Studio for Mac.

-   **Sady SDK pro Android** -Android SDK – 5.0.1 (21 rozhraní API) nebo novější musí být nainstalován prostřednictvím Android SDK Manager.

-   **Sady pro vývojáře Java** &ndash; Xamarin Android vývoj vyžaduje [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) Pokud jste pro úroveň rozhraní API 24 vývoj nebo větší (JDK 1.8 také podporuje úrovně rozhraní API starší než 24).

Můžete dál používat [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Pokud vývoj speciálně pro úroveň rozhraní API 23 nebo starším.

> [!IMPORTANT]
> Xamarin.Android nepodporuje JDK 9.

## <a name="installation"></a>Instalace

Po instalaci Xamarin.Android, proveďte následující kroky, takže budete připraveni k sestavení a testů Android nosit aplikace: 

1.  Nainstalujte požadované Android SDK a nástroje.
2.  Konfigurace testovací zařízení.
3.  Vytvoření první aplikace Android nosit.

Tyto kroky jsou popsané v následujících částech.


### <a name="install-android-sdk-and-tools"></a>Instalace nástrojů a SDK pro Android 

Spusťte **Android SDK Manager**: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Postup spuštění Android SDK Manager v sadě Visual Studio](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Postup spuštění Android SDK Manager v sadě Visual Studio pro Mac](installation-images/xs/sdk-menu.png)

-----


Ujistěte se, že máte následující sady SDK pro Android a nainstalované nástroje:

* Android SDK nástroje v 24.0.0 nebo vyšší, a
* Android 4.4W (API20), nebo
* Android – 5.0.1 (API21) nebo vyšší.

Pokud nemáte nejnovější SDK a nástroje nainstalované, stáhnout požadované sady SDK nástroje *a* rozhraní API služby bits (budete muset posunout bit, kde je najít &ndash; výběr rozhraní API je uveden níže): 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Snímek obrazovky příklad SDK Manager povolení Android – 5.0.1 součásti](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Snímek obrazovky příklad SDK Manager povolení Android 4.4 a – 5.0.1 součásti](installation-images/xs/sdk-select.png)

-----


## <a name="configuration"></a>Konfigurace

Než budete moci použít testování vaší aplikace, je nutné nakonfigurovat emulátoru Android nosit nebo skutečné zařízení Android nosit. 


### <a name="android-wear-emulator"></a>Emulátoru Android opotřebení

Než budete moct použít nosit Android emulátoru, musíte nakonfigurovat k Android nosit virtuální zařízení Android (AVD) pomocí **správce emulátorů Google**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Postup spuštění Správce emulátoru Android ze sady Visual Studio](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Postup spuštění Správce emulátoru Android ze sady Visual Studio pro Mac](installation-images/xs/emulator-menu.png)

-----

Další informace o nastavení emulátoru Android nosit najdete v tématu [ladění Android nosit na emulátoru](~/android/wear/deploy-test/debug-on-emulator.md).


### <a name="android-wear-device"></a>Android opotřebení ze zařízení

Pokud máte zařízení s Androidem nosit například Android Smartwatch nosit, můžete ladit aplikace na tomto zařízení místo použití emulátoru. Informace o vývoji s opotřebení ze zařízení najdete v tématu [ladění na zařízení nosit](~/android/wear/deploy-test/debug-on-device.md).


## <a name="create-your-first-android-wear-app"></a>Vytvoření první aplikace Android opotřebení

Postupujte podle [nosit Hello,](~/android/wear/get-started/hello-wear.md) pokyny k vytvoření první aplikace sledovat.


## <a name="packaging-your-app"></a>Zabalení aplikace

Aplikace Android opotřebení jsou vždy distribuován a doprovodné telefon se systémem Android aplikaci. 

Když přidáte aplikace Android nosit jako odkaz na hlavní aplikace pro Android je automaticky předpokládá, že se projektu Android nosit a vygeneruje všechny potřebné XML a metadat pro vás. Kromě toho bude ověřte, jestli balíčku a verzi čísla odpovídají tak můžete snadno zaslat aplikace na web Google Play. 

Další informace o balení aplikací a opotřebením motoru, najdete v části [práce s balení](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>Související odkazy

- [SkeletonWear (ukázka)](https://developer.xamarin.com/samples/SkeletonWear/)
