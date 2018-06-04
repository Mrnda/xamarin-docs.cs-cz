---
title: Řešení potíží s emulátor Google Android
description: Jak identifikovat a vyřešit problémy emulátor Google Android
ms.prod: xamarin
ms.assetid: 4B05C3C5-E1F6-47A9-B098-C31E630194F6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: 73d0e578a0cf8ea6c0a62d8e21809cdab4b20910
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732359"
---
# <a name="google-android-emulator-troubleshooting"></a>Řešení potíží s emulátor Google Android

_V tomto článku nejběžnější zprávy upozornění a problémy, ke kterým dochází při spuštění emulátor Google Android jsou popsány, společně s alternativní řešení a tipů. Informace o řešení potíží během instalace emulátoru najdete v tématu [odstraňování potíží s instalací emulátoru](~/android/get-started/installation/android-emulator/troubleshooting.md)._

<a name="perfwarn" />

## <a name="performance-warnings"></a>Upozornění výkonu

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Od verze Visual Studio 2017 verzi 15.4, dialogové okno upozornění výkonu může zobrazit při prvním nasazení aplikace do emulátor Google Android. Tyto dialogy upozornění jsou vysvětleny níže.

### <a name="computer-does-not-contain-an-intel-procesor"></a>Počítač neobsahuje Procesor Intel

![Počítač neobsahuje procesor Intel](troubleshooting-images/01-no-intel-processor.png)

Po toto dialogové okno se zobrazí, váš počítač nemá procesor Intel, což je vyžadováno pro akcelerace emulátoru Android SDK. Pokud váš počítač nemá procesor Intel, doporučujeme používat fyzického zařízení s Androidem pro vývoj.

### <a name="hyper-v-is-installed-or-active"></a>Technologie Hyper-V není nainstalován nebo je aktivní

![Technologie Hyper-V není nainstalován nebo active](troubleshooting-images/02-hyper-v-active.png)

Když toto dialogové okno se zobrazí, technologie Hyper-V není nainstalován nebo active a musí být zakázáno. [Zakázání technologie Hyper-V](#disable-hyperv) vysvětluje, jak tento problém vyřešit.

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


Pokud `STATE` není nastavený na `RUNNING`, najdete v části [jak používat Správce spuštění Accelerated Intel hardwaru](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) pro vyřešení problému.


### <a name="other-failures"></a>Jiné chyby

![Jiné chyby](troubleshooting-images/05-other-failure.png)

Toto dialogové okno se zobrazí, pokud váš počítač je procesor Intel, technologie Hyper-V je zakázaná, Intel HAXM je nainstalovaná, je spuštěn proces HAXM, ale emulátoru nepodaří spustit z neznámých důvodů.
Chcete-li tuto chybu vyřešit, přečtěte si téma [jak používat Správce spuštění Accelerated Intel hardwaru](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="disabling-performance-warnings"></a>Zakázání upozornění výkonu

Pokud nechcete zobrazovat upozornění výkonu, můžete je zakázat. V sadě Visual Studio, klikněte na tlačítko **nástroje > Možnosti > Xamarin > Nastavení Androidu** a zakázat **varování, pokud AVD akcelerace není podporované (HAXM)** možnost:

[![Zakázání AVD akcelerace upozornění](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png#lightbox)



# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Od verze sady Visual Studio pro Mac sestavení 7.2 (sestavení 559), dialogové okno upozornění výkonu může zobrazit při prvním nasazení aplikace do emulátor Google Android. Tyto dialogy upozornění jsou vysvětleny níže.

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


## <a name="deployment-issues"></a>Problémy při nasazení

Pokud dojde k chybě o selhání instalace APK na emulátoru nebo selhání při spouštění most ladění Android (**adb**), ověřte, zda SDK pro Android můžete připojit k vaší emulátor. Chcete-li to provést, použijte následující kroky:

1. Spusťte emulátor ze **Správce zařízení Android** (vyberte virtuální zařízení a klikněte na **spustit**).

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


## <a name="haxm-issues"></a>HAXM problémy

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pokud emulátor Google Android nespustí správně, je často příčinou problémů s HAXM. HAXM problémy jsou často výsledek je v konfliktu s jiných virtualizačních technologií, nesprávná nastavení nebo aktuální ovladač HAXM.

<a name="virt-conflicts" />

### <a name="haxm-virtualization-conflicts"></a>Konflikty HAXM virtualizace

HAXM může dojít ke konfliktu s jinými technologiemi, které používají virtualizaci, jako je například technologie Hyper-V, Windows Device Guard a některé antivirový software:

- **Technologie Hyper-V** &ndash; Pokud používáte verzi systému Windows, než **Windows 10. dubna 2018 aktualizovat (sestavení 1803)** a je povolená technologie Hyper-V, postupujte podle kroků v [zakázání technologie Hyper-V](#disable-hyperv).

- **Ochrana zařízení** &ndash; ochranou zařízení a ochranu přihlašovacích údajů mohou zabránit technologie Hyper-V bude zakázán na počítače s Windows. Pokud chcete zakázat ochranu zařízení a ochranu přihlašovacích údajů, přečtěte si téma [zakázání Device Guard](#disable-devguard).

- **Antivirový Software** &ndash; Pokud máte spuštěný antivirový software, který používá virtualizace s hardwarovým řízením (například Avast), zakažte nebo odinstalujte tento software, restartování a opakovat emulátoru Android SDK.


### <a name="incorrect-bios-settings"></a>Nastavení nesprávný systému BIOS

Pokud používáte HAXM v počítačích s Windows, HAXM nebude fungovat, pokud není v systému BIOS povolena virtualizace technology (Intel VT-x). Pokud VT-x je zakázaná, obdržíte chybu podobný následujícímu při pokusu o spuštění emulátor Google Android:

**Tento počítač splňuje požadavky pro HAXM, ale není zapnutý technologií Intel Virtualization (VT-x).**

Chcete-li opravit tuto chybu, spustit počítač v systému BIOS, povolit VT-x a SLAT (překlad adres druhé úrovně) a pak restartujte počítač zpět do systému Windows.

<a name="disable-hyperv" />

### <a name="disabling-hyper-v"></a>Zakázání technologie Hyper-V

Pokud používáte verzi systému Windows, než **Windows 10. dubna 2018 aktualizace (sestavení 1803)** a je povolená technologie Hyper-V, je nutné zakázat technologie Hyper-V a restartujte počítač k instalaci a používání HAXM. Pokud používáte **Windows 10. dubna 2018 aktualizace (sestavení 1803)** nebo novější, emulátor Google Android verze 27.2.7 nebo novější můžete použít technologie Hyper-V (ne HAXM) pro hardwarové akcelerace, takže není nutné zakázat technologie Hyper-V.

Technologie Hyper-V z ovládacích panelů můžete zakázat pomocí následujících kroků:

1. Do vyhledávacího pole Windows zadejte **programy a** klikněte **programy a funkce** výsledek hledání.

2. V Ovládacích panelech **programy a funkce** dialogové okno, klikněte na tlačítko **Windows zapnout nebo vypnout funkce**:

    ![Zapnutí funkce systému Windows, nebo vypnutí](troubleshooting-images/win/07-turn-windows-features.png)

3. Zrušte zaškrtnutí políčka **technologie Hyper-V** a restartujte počítač:

    ![Zakázání technologie Hyper-V v dialogovém okně funkce systému Windows](troubleshooting-images/win/08-uncheck-hyper-v.png)

Alternativně můžete pomocí následujících rutin Powershellu zakázat Hyper-V:

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM i Microsoft Hyper-V nemůže být aktivní ve stejnou dobu. Bohužel neexistuje žádný způsob, jak přepínat mezi mezi Hyper-V a HAXM bez restartování počítače. Pokud chcete použít Visual Studio 2015 [Visual Studio Emulator for Android](~/android/deploy-test/debugging/visual-studio-android-emulator.md) (která závisí na technologii Hyper-V), nebude možné používat emulátor Google Android bez restartování. Jedním ze způsobů, chcete-li vyřešit tento problém se k upgradu systému Windows **Windows 10. dubna 2018 aktualizace (sestavení 1803)** nebo novější a používat Hyper-V pro obě emulátorů (najdete v části [hardwarovou akceleraci emulátoru výkonu](~/android/get-started/installation/android-emulator/hardware-acceleration.md)).
Dalším způsobem je použít technologie Hyper-V a HAXM vytvořením instalace s možností více systémů, jak je popsáno v [vytváření žádný spouštěcí položku hypervisoru](https://blogs.msdn.microsoft.com/virtual_pc_guy/2008/04/14/creating-a-no-hypervisor-boot-entry/).

V některých případech pomocí výše uvedené kroky nepovede v zakázání technologie Hyper-V, pokud je povolena ochrana zařízení a ochranu přihlašovacích údajů. Pokud nelze zakázat technologie Hyper-V (nebo ji zdá se, že se zakáže, ale HAXM instalace se nezdaří), postupujte podle kroků v další části zakázat ochranu zařízení a ochranu přihlašovacích údajů.

<a name="disable-devguard" />

### <a name="disabling-device-guard"></a>Zakázání ochranou zařízení

Ochrana zařízení a ochranu přihlašovacích údajů mohou zabránit technologie Hyper-V bude zakázán na počítače s Windows. Často se jedná o problém pro počítače připojené k doméně, které jsou konfigurovány a řídí vlastnící organizace.
Ve Windows 10, použijte následující postup pro případ, **Device Guard** běží:

1. V **Windows Search**, typ **informace systému** spustit **informace o systému** aplikace.

2. V **systému Souhrn**, vzhled a zjistěte, zda **ochrana virtualizace zařízení na základě zabezpečení** je k dispozici a je v **systémem** stavu:

   [![Ochrana zařízení je existovat a běžet](troubleshooting-images/win/09-device-guard-sml.png)](troubleshooting-images/win/09-device-guard.png#lightbox)

Pokud je povolena ochrana zařízení, použijte ji zakázat následující kroky:

1. Ujistěte se, že **technologie Hyper-V** vypnutá (v části **zapnout nebo vypnout funkce systému Windows**) jak je popsáno v předchozí části.

2. Do vyhledávacího pole Windows zadejte **gpedit** a vyberte **upravit zásady skupiny** výsledek hledání. Spustí se **Editor místních zásad skupiny**.

3. V **Editor místních zásad skupiny**, přejděte na **konfigurace počítače > šablony pro správu > Systém > Device Guard**:

   [![Ochrana zařízení v Editoru místních zásad skupiny](troubleshooting-images/win/10-group-policy-editor-sml.png)](troubleshooting-images/win/10-group-policy-editor.png#lightbox)

4. Změna **zapnout na virtualizace zabezpečení na základě** k **zakázané** (jak je uvedeno výše) a ukončete **Editor místních zásad skupiny**.

5. Do vyhledávacího pole Windows zadejte **cmd**. Když **příkazového řádku** se zobrazí ve výsledcích hledání klikněte pravým tlačítkem na **příkazového řádku** a vyberte **spustit jako správce**.

6. Zkopírujte a vložte následující příkazy do okna příkazového řádku (Pokud jednotky **Z:** je v použít, vyberte k nepoužívanému písmenu jednotky místo toho použít):

        mountvol Z: /s
        copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
        bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
        bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
        mountvol Z: /d

7. Restartujte počítač. Na obrazovce spouštěcí byste měli vidět řádku takto:

   **Opravdu chcete zakázat ochranu přihlašovacích údajů?**

   Stisknutím klávesy uvedené zakázat ochranu přihlašovacích údajů po zobrazení výzvy.

8. Po restartování počítače, zkontrolujte znovu zkontrolujte, zda technologie Hyper-V je vypnutá (jak je popsáno v předchozím kroku).

Pokud ještě není zakázán technologie Hyper-V, zabránit vám v zakázání Guard zařízení a ochranu přihlašovacích údajů mohou zásady počítače připojené k doméně. V takovém případě může požádat o výjimku z vašeho správce domény a umožní vám pro vyjádření výslovného nesouhlasu ochranu přihlašovacích údajů. Alternativně můžete použít počítač, který není připojený k doméně používat HAXM.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud emulátor Google Android nespustí správně, je často příčinou problémů s HAXM. HAXM problémy jsou často výsledek je v konfliktu s jiných virtualizačních technologií, nesprávná nastavení nebo aktuální ovladač HAXM. Zkuste znovu nainstalovat ovladač HAXM, pomocí kroky popsané v [instalace HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm).

-----

