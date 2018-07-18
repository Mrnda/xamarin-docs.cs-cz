---
title: Hardwarovou akceleraci emulátoru výkonu
description: Tento článek vysvětluje, jak používat funkce hardwarové akcelerace počítače pro zajištění maximálního výkonu emulátoru Androidu.
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: c2bef2c614d4cc0655deb9732ccefec223a8318a
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848464"
---
# <a name="hardware-acceleration-for-emulator-performance"></a>Hardwarovou akceleraci emulátoru výkonu

_Tento článek vysvětluje, jak používat funkce hardwarové akcelerace počítače pro zajištění maximálního výkonu emulátoru Androidu._

## <a name="overview"></a>Přehled

Visual Studio usnadňuje vývojářům otestovat a ladit své aplikace Xamarin.Android pomocí emulátoru Androidu v situacích, kdy zařízení s Androidem je nedostupné nebo nepoužitelné.
Ale emulátoru Androidu běží příliš pomalu Pokud hardwarovou akceleraci není k dispozici v počítači, na kterém běží. Pomocí imagí virtuálního zařízení speciální cíl x86 hardwaru ve spojení s jednou ze dvou technologií virtualizace může výrazně zlepšit výkon emulátoru Androidu:

1. **Společnosti Microsoft Hyper-V a platformou hypervisoru. Tento**. Technologie Hyper-V je funkce virtualizace systému Windows, která umožňuje spustit na fyzickém hostitelském počítači virtualizované počítačové systémy. Toto je doporučená virtualizační technologie pro urychlení emulátoru Androidu. Další informace o technologii Hyper-V najdete v tématu [Hyper-V ve Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).

2. **Intel Hardware Accelerated provádění Manager (HAXM)**. 
   Modul HAXM není modul virtualizace pro počítače s procesory Intel.
   Toto je modul virtualizace s doporučenou pro počítače, které se nedaří spustit technologii Hyper-V.

Emulátor Androidu automaticky provede využívají hardwarovou akceleraci, pokud se splní následující kritéria:

-   Hardwarová akcelerace je k dispozici a povoleno na vývojovém počítači.

-   Emulátor běží vytvořený specificky pro image emulátoru **x86**– na základě virtuálního zařízení.

Informace o spouštění a ladění pomocí emulátoru Androidu najdete v tématu [ladění v emulátoru Android](~/android/deploy-test/debugging/debug-on-emulator.md).

## <a name="hyper-v"></a>Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Podpora technologie Hyper-V je aktuálně ve verzi Preview.

Vývojáři, kteří používají Windows 10 (aktualizace z dubna 2018 nebo novější) důrazně doporučujeme použít společnosti Microsoft Hyper-V ke zrychlení emulátoru Androidu. Chcete-li otestovat pomocí emulátoru Androidu s technologií Hyper-V:

1. **Aktualizace na Windows 10. dubna 2018 Update (build 1803) nebo novější**.
   Pokud chcete ověřit, jakou verzi Windows, klikněte na tlačítko na panelu hledání Cortany a typ **o**. Vyberte **informace o počítači** ve výsledcích hledání. Posuňte se dolů v **o** dialogové okno **specifikace Windows** oddílu. **Verze** by měl být alespoň 1803:

    [![Specifikace Windows](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Povolit platformu hypervisoru Windows**.
   Na panelu hledání v Cortaně zadejte **Windows zapnout nebo vypnout funkce**.
   Posuňte se dolů v **funkce Windows** dialogové okno a ujistěte se, že **platformu hypervisoru Windows** zapnutá:

    [![Platforma hypervisoru Windows povolena](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

   Povolení **platformu hypervisoru Windows** automaticky povolí Hyper-V. Je vhodné k restartování Windows po provedení této změny.

3. **Nainstalujte [Visual Studio 15.8 Preview 1 nebo novější](https://visualstudio.microsoft.com/vs/preview/)**.
   Tato verze sady Visual Studio podporuje integrované vývojové prostředí pro spuštění emulátoru Androidu s technologií Hyper-V.
 
4. **Nainstalovat balíček Android Emulator 27.2.7 nebo novější**. Chcete-li nainstalovat tento balíček, přejděte na **nástroje > Android > správce sady Android SDK** v sadě Visual Studio. Vyberte **nástroje** kartu a ujistěte se, že 27.2.7 minimální verze emulátoru Androidu. Také se ujistěte, že je verze Android SDK Tools 26.1.1 nebo novější:

    [![Dialogové okno sady Android SDK a nástroje](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Pokud je verze emulátoru alespoň 27.2.7 ale menší než 27.3.1, následující alternativní řešení, je potřeba použít technologie Hyper-V:

    1.  V **C:\\uživatelé\\_uživatelské jméno_\\.android** složce vytvořte soubor s názvem **advancedFeatures.ini** (Pokud ne již existuje).

    2.  Přidejte následující řádek, který **advancedFeatures.ini**:
        ```
        WindowsHypervisorPlatform = on
        ```


### <a name="known-issues"></a>Známé problémy

-   Pokud se nemůžete aktualizovat na verzi emulátoru 27.2.7 nebo novější po aktualizaci do náhledu Visual Studio, možná budete muset přímo nainstalovat [instalační náhled](http://aka.ms/hyperv-emulator-dl) pro povolení novějších verzí emulátoru.

-   Z webu Intelu, stáhněte si nejnovější modul virtualizace s modulem HAXM instalačního programu pro Windows.

-   Výhodou stáhnout instalační program modulu HAXM přímo z webu Intelu je, že jste si jistí pomocí nejnovější verze.

-   Alternativně můžete použít správce sady SDK se stáhnout instalační program modulu HAXM (ve správci sady SDK, klikněte na tlačítko nástroje > Funkce > Intel x86 emulátor akcelerátorů (Instalační program modulu HAXM)). Sady Android SDK obvykle stáhne instalační program modulu HAXM do následujícího umístění:


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

C:Program Files (x86)Androidsady android sdkfunkceintelhardwaruAcceleratedprováděnísprávce Podrobnější informace naleznete v [požadavcích Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements).

-----

## <a name="haxm"></a>HAXM

Přijměte výchozí hodnoty v dialogových oknech instalačního programu: Intel Hardware Accelerated správce spuštění okno

Pokud se vyvíjíte na počítači s procesorem Intel, který má schopnosti VT, můžete využít výhod HAXMu, abyste velmi rychle urychlila Emulátor Android (pokud si nejste jisti, zda CPU podporuje VT, viz [Má procesor Intel Virtualization Technika?](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

> [!NOTE]
> Vzhledem k tomu, že emulátor Androidu v současné době podporuje hardwarovou akceleraci AMD pouze v Linuxu, hardwarovou akceleraci není k dispozici pro procesory AMD počítače se systémem Windows. Z webu Intelu, stáhněte si nejnovější [modul virtualizace s modulem HAXM](https://developer.android.com/studio/run/emulator-acceleration.html#extensions) instalačního programu pro macOS.

Spusťte instalační program modulu HAXM.

### <a name="verifying-haxm-installation"></a>Spuštění aplikace v emulátoru Androidu

Můžete zkontrolovat, zda modul HAXM je k dispozici zobrazením **spuštění emulátoru Androidu** okno při spuštění emulátoru. Spustit Android emulátor, postupujte takto:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknutím na spustit Správce zařízení s Androidem **nástroje > Android > Správce zařízení s Androidem**:

    [![Umístění položky nabídky Správce zařízení s androidem](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Pokud se zobrazí **upozornění ohledně výkonu** dialogové okno podobné následujícímu, potom modul HAXM je ještě není nainstalovaná nebo správně nakonfigurovaná v počítači:

    ![Dialogové okno upozornění výkonu, modul HAXM není připraveno](hardware-acceleration-images/win/11-perf-warn.png)

   Pokud **upozornění ohledně výkonu** dialogu tímto způsobem, naleznete v tématu [upozornění výkonu](~/android/get-started/installation/android-emulator/troubleshooting.md#perfwarn) určit příčinu a vyřešte příčinu problému.

3. Vyberte **x86** bitové kopie (například **VisualStudio\_android 23\_x86\_telefonní**) a klikněte na tlačítko **Start**:

    ![Spouští se Android Emulator s výchozí image virtuálního zařízení](hardware-acceleration-images/win/02-start-default-avd.png)

4. Podívejte se **spuštění emulátoru Androidu** dialogového okna při spouštění v emulátoru. Pokud je nainstalovaný modul HAXM, zobrazí se zpráva, **HAX pracuje a emulátor běží v režimu rychlé virt** jak je znázorněno na tomto snímku obrazovky:

    ![Modul HAXM je uvedené jako dostupné v dialogovém okně spuštění emulátoru Androidu](hardware-acceleration-images/win/03-haxm-detected.png)

   Pokud nevidíte tuto zprávu, modul HAXM pravděpodobně není nainstalovaný. Například tady je snímek obrazovky zpráva se může zobrazit, pokud modul HAXM není k dispozici:

    ![Modul HAXM není k dispozici na tomto počítači](hardware-acceleration-images/win/04-haxm-error.png)

   Pokud modul HAXM není k dispozici v počítači, postupujte podle kroků v další části nainstalovat modul HAXM.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknutím na spustit Správce zařízení s Androidem **nástroje > Správce zařízení**:

    [![Umístění položky nabídky Správce zařízení s androidem](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Pokud se zobrazí **upozornění ohledně výkonu** dialogové okno podobné následujícímu, potom modul HAXM je ještě není nainstalovaná nebo správně nakonfigurovaná v počítači:

    ![Dialogové okno upozornění výkonu, modul HAXM není připraveno](hardware-acceleration-images/mac/04-avd-warning.png)

   Pokud **upozornění ohledně výkonu** dialogu tímto způsobem, naleznete v tématu [upozornění výkonu](~/android/get-started/installation/android-emulator/troubleshooting.md#perfwarn) určit příčinu a vyřešte příčinu problému.

3. Vyberte **x86** bitové kopie (například **Android\_Accelerated\_x86**) a klikněte na tlačítko **Přehrát**:

    [![Spouští se Android Emulator s výchozí image virtuálního zařízení](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. Podívejte se **spuštění emulátoru Androidu** dialogového okna při spouštění v emulátoru. Pokud je nainstalovaný modul HAXM, zobrazí se zpráva, **HAX pracuje a emulátor běží v režimu rychlé virt** jak je znázorněno na tomto snímku obrazovky:

    ![Modul HAXM je uvedené jako dostupné v dialogovém okně spuštění emulátoru Androidu](hardware-acceleration-images/mac/03-haxm-detected.png)

   Pokud modul HAXM není k dispozici ve vašem počítači (např. Pokud se zobrazí chybová zpráva jako _zkontrolujte Intel HAXM je neukončily správně nainstalován a použitelný_), pomocí kroků v další části, chcete-li nainstalovat modul HAXM.

-----

<a name="install-haxm" />

### <a name="installing-haxm"></a>Instaluje se modul HAXM

Pokud není možné spustit v emulátoru, modul HAXM možná muset nainstalovat ručně. Modul HAXM instalaci balíčků pro Windows a macOS je k dispozici [Intel Hardware Accelerated spuštění správce](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) stránky. Stáhnout a nainstalovat modul HAXM ručně pomocí následujících kroků:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Z webu Intelu, stáhněte si nejnovější [modul virtualizace s modulem HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) instalačního programu pro Windows. Výhodou stáhnout instalační program modulu HAXM přímo z webu Intelu je, že jste si jistí pomocí nejnovější verze.

   Alternativně můžete použít správce sady SDK se stáhnout instalační program modulu HAXM (ve správci sady SDK, klikněte na tlačítko **nástroje > Funkce > Intel x86 emulátor akcelerátorů (Instalační program modulu HAXM)**). Sady Android SDK obvykle stáhne instalační program modulu HAXM do následujícího umístění:

   **C:\\Program Files (x86)\\Android\\sady android sdk\\funkce\\intel\\hardwaru\_Accelerated\_provádění\_správce**

   Všimněte si, že správce sady SDK není možné nainstalovat modul HAXM, pouze stáhne instalační program modulu HAXM do výše uvedené lokalitě; Máte pořád spustit ručně.

2. Spustit **intelhaxm android.exe** spustit instalační program modulu HAXM. Přijměte výchozí hodnoty v dialogových oknech instalačního programu:

   ![Intel Hardware Accelerated správce spuštění okno](hardware-acceleration-images/win/05-haxm-installer.png)

## <a name="hardware-acceleration-and-amd-cpus"></a>Hardwarová akcelerace a procesory AMD

Vzhledem k tomu, že emulátor Androidu v současné době podporuje hardwarovou akceleraci AMD [pouze v Linuxu](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies), hardwarovou akceleraci není k dispozici pro procesory AMD počítače se systémem Windows.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Z webu Intelu, stáhněte si nejnovější [modul virtualizace s modulem HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) instalačního programu pro macOS.

2. Spusťte instalační program modulu HAXM. Přijměte výchozí hodnoty v dialogových oknech instalačního programu:

   [![Intel Hardware Accelerated správce spuštění okno](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>Související odkazy

* [Spuštění aplikace v emulátoru Androidu](https://developer.android.com/studio/run/emulator)
