---
title: "Řešení potíží s emulátoru sady SDK pro Android"
description: "Jak identifikovat a vyřešit problémy emulátoru sady SDK pro Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B05C3C5-E1F6-47A9-B098-C31E630194F6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 51cc7a4700e8cb3ece556b0ada841d70d5f2bb8b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="android-sdk-emulator-troubleshooting"></a>Řešení potíží s emulátoru sady SDK pro Android

V tomto článku jsou vysvětleny nejběžnější zprávy upozornění a problémy s emulátoru Android SDK (a jejich řešení).
 
<a name="perfwarn" />

## <a name="performance-warnings"></a>Upozornění výkonu

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Od verze Visual Studio 2017 verzi 15.4, dialogové okno upozornění výkonu může zobrazit při prvním nasazení aplikace pro Android emulátoru sady SDK. Tyto dialogy upozornění jsou vysvětleny níže.

### <a name="computer-does-not-contain-an-intel-procesor"></a>Počítač neobsahuje Procesor Intel

![Počítač neobsahuje procesor Intel](troubleshooting-images/01-no-intel-processor.png)

Po toto dialogové okno se zobrazí, váš počítač nemá procesor Intel, což je vyžadováno pro akcelerace emulátoru Android SDK. Pokud váš počítač nemá procesor Intel, doporučujeme používat fyzického zařízení s Androidem pro vývoj.

### <a name="hyper-v-is-installed-or-active"></a>Technologie Hyper-V není nainstalován nebo je aktivní

![Technologie Hyper-V není nainstalován nebo active](troubleshooting-images/02-hyper-v-active.png)

Když toto dialogové okno se zobrazí, technologie Hyper-V není nainstalován nebo active a musí být zakázáno. [Zakázání technologie Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv) vysvětluje, jak tento problém vyřešit. 

### <a name="haxm-is-not-installed"></a>HAXM není nainstalován

![HAXM není nainstalována.](troubleshooting-images/03-haxm-not-installed.png)

Toto dialogové okno označuje, že váš počítač je procesor Intel, technologie Hyper-V je zakázaná, ale HAXM není nainstalována.
[Instalace HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) popisuje kroky pro instalaci HAXM.

### <a name="haxm-process-not-running"></a>Proces HAXM neběží.

![Proces HAXM není spuštěna.](troubleshooting-images/04-haxm-process-not-running.png)

Toto dialogové okno se zobrazí, pokud má počítač procesor Intel, technologie Hyper-V je zakázaná, Intel HAXM je nainstalován, ale proces HAXM neběží. Chcete-li vyřešit tento problém, otevřete okno příkazového řádku a zadejte následující příkaz:

```cmd
sc query intelhaxm
```

Pokud je spuštěn proces HAXM, byste měli vidět výstup podobný následujícímu:

```cmd
SERVICE_NAME: intelhaxm
    TYPE               : 1  KERNEL_DRIVER
    STATE              : 4  RUNNING
                            (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
    WIN32_EXIT_CODE    : 0  (0x0)
    SERVICE_EXIT_CODE  : 0  (0x0)
    CHECKPOINT         : 0x0
    WAIT_HINT          : 0x0
```


Pokud **stavu** není nastavený na **systémem**, najdete v části [jak používat Správce spuštění Accelerated Intel hardwaru](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) pro vyřešení problému.


### <a name="other-failures"></a>Jiné chyby

![Jiné chyby](troubleshooting-images/05-other-failure.png)

Toto dialogové okno se zobrazí, pokud váš počítač je procesor Intel, technologie Hyper-V je zakázaná, Intel HAXM je nainstalovaná, je spuštěn proces HAXM, ale emulátoru nepodaří spustit z neznámých důvodů.
Chcete-li tuto chybu vyřešit, přečtěte si téma [jak používat Správce spuštění Accelerated Intel hardwaru](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="disabling-performance-warnings"></a>Zakázání upozornění výkonu

Pokud nechcete zobrazovat upozornění výkonu, můžete je zakázat. V sadě Visual Studio, klikněte na tlačítko **nástroje > Možnosti > Xamarin > Nastavení Androidu** a zakázat **varování, pokud AVD akcelerace není podporované (HAXM)** možnost:

[![Zakázání AVD akcelerace upozornění](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Od verze sady Visual Studio pro Mac sestavení 7.2 (sestavení 559), dialogové okno upozornění výkonu může zobrazit při prvním nasazení aplikace pro Android emulátoru sady SDK. Tyto dialogy upozornění jsou vysvětleny níže.

### <a name="haxm-is-not-installed"></a>HAXM není nainstalován

![HAXM není nainstalována.](troubleshooting-images/03-haxm-not-installed.png)

Toto dialogové okno označuje, že HAXM není nainstalována.
[Instalace HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) popisuje kroky pro instalaci HAXM.

### <a name="haxm-process-not-running"></a>Proces HAXM neběží.

![Proces HAXM není spuštěna.](troubleshooting-images/04-haxm-process-not-running.png)

Toto dialogové okno se zobrazí, pokud proces HAXM neběží. Podrobné informace o pomoc při řešení tohoto problému najdete v tématu [jak používat Správce spuštění Accelerated Intel hardwaru](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) pro vyřešení problému.

### <a name="other-failures"></a>Jiné chyby

![Jiné chyby](troubleshooting-images/05-other-failure.png)

Toto dialogové okno se zobrazí, pokud emulátoru nepodaří spustit z neznámých důvodů. Chcete-li tuto chybu vyřešit, přečtěte si téma [jak používat Správce spuštění Accelerated Intel hardwaru](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) pro vyřešení problému.

-----

<a name="solutions" />

## <a name="solutions-to-common-problems"></a>Řešení běžných potíží

Mnoho běžných problémů emulátoru Android SDK lze vyřešit tak, že změny konfigurace v počítači nebo po instalaci další software. Následující části popisují tyto problémy a poskytují řešení.

<a name="deployment" />

### <a name="deployment-issues"></a>Problémy při nasazení

Pokud dojde k chybě o selhání instalace APK na emulátoru nebo selhání při spouštění most ladění Android (**adb**), ověřte, zda SDK pro Android můžete připojit k vaší emulátor. Chcete-li to provést, použijte následující kroky:

1. Spusťte emulátor ze **Manager virtuální zařízení Android (AVD)** (vyberte virtuální zařízení a klikněte na **spustit**).

2. Otevřete příkazový řádek a přejděte do složky, kde **adb** je nainstalovaná. Například v systému Windows, může se jednat v: **C:\\Program Files (x86)\\Android\\android-sdk\\nástrojů platformy\\adb.exe**.

3. Zadejte následující příkaz:

   ```shell
   adb devices
   ```

4. Pokud je přístupná ze sady SDK pro Android emulátoru, emulátoru by se zobrazit v seznamu připojená zařízení. Příklad:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Pokud v tomto seznamu nezobrazí emulátoru, spusťte **Android SDK Manager**, všechny aktualizace a pak se pokuste spustit v emulátoru znovu.


<a name="haxm-issues" />

### <a name="haxm-issues"></a>HAXM problémy

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pokud emulátoru Android SDK se nespustí správně, to je obvykle způsobeno problémy s HAXM. HAXM problémy jsou často výsledek je v konfliktu s jiných virtualizačních technologií, nesprávná nastavení nebo aktuální ovladač HAXM.

<a name="virt-conflicts" />

#### <a name="haxm-virtualization-conflicts"></a>Konflikty HAXM virtualizace

HAXM může dojít ke konfliktu s jinými technologiemi, které používají virtualizaci, jako je například technologie Hyper-V, Windows Device Guard a některé antivirový software:

- **Technologie Hyper-V** &ndash; Pokud používáte systém Windows s technologií Hyper-V povolena, postupujte podle kroků v [zakázání technologie Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv).

- **Ochrana zařízení** &ndash; ochranou zařízení a ochranu přihlašovacích údajů mohou zabránit technologie Hyper-V bude zakázán na počítače s Windows. Pokud chcete zakázat ochranu zařízení a ochranu přihlašovacích údajů, přečtěte si téma [zakázání Device Guard](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-devguard).

- **Antivirový Software** &ndash; Pokud máte spuštěný antivirový software, který používá virtualizace s hardwarovým řízením (například Avast), zakažte nebo odinstalujte tento software, restartování a opakovat emulátoru Android SDK.

<a name="bios" />

#### <a name="incorrect-bios-settings"></a>Nastavení nesprávný systému BIOS

Pokud používáte HAXM v počítačích s Windows, HAXM nebude fungovat, pokud není v systému BIOS povolena virtualizace technology (Intel VT-x). Pokud VT-x je zakázaná, obdržíte chybu podobný následujícímu při pokusu o spuštění emulátoru Android SDK:

**Tento počítač splňuje požadavky pro HAXM, ale není zapnutý technologií Intel Virtualization (VT-x).**

Chcete-li opravit tuto chybu, spustit počítač v systému BIOS, povolit VT-x a SLAT (překlad adres druhé úrovně) a pak restartujte počítač zpět do systému Windows.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud emulátoru Android SDK se nespustí správně, to je obvykle způsobeno problémy s HAXM. HAXM problémy jsou často výsledek je v konfliktu s jiných virtualizačních technologií, nesprávná nastavení nebo aktuální ovladač HAXM. Zkuste znovu nainstalovat ovladač HAXM, pomocí kroky popsané v [instalace HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm).

-----
