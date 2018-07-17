---
title: Řešení potíží s emulátoru androidu
description: Tento článek vysvětluje, jak diagnostikovat a vyřešit problémy, které mohou nastat při použití emulátoru Androidu.
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 1d13a3dae509fea4a2e955c4ad206a81a57e75ed
ms.sourcegitcommit: 6433b424410a850f504e0f934bbb5baf8f093e49
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067331"
---
# <a name="android-emulator-troubleshooting"></a>Řešení potíží s emulátoru androidu

_V tomto článku nejběžnější zprávy upozornění a problémů, ke kterým dochází při konfiguraci a spuštění emulátoru Androidu jsou popsány, spolu s tipy a alternativní řešení._

<a name="perfwarn" />

## <a name="performance-warnings"></a>Upozornění výkonu

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Od verze Visual Studio 2017 verze 15.4, dialogové okno upozornění výkonu může zobrazit při prvním nasazení aplikace do emulátoru Androidu. Tato upozornění jsou vysvětleny níže.

### <a name="computer-does-not-contain-an-intel-procesor"></a>Počítač neobsahuje Procesor Intel

![Počítač neobsahuje procesor Intel](troubleshooting-images/01-no-intel-processor.png)

Když se zobrazí toto dialogové okno, počítač nemá procesor Intel, které se vyžaduje k akceleraci emulátoru Androidu SDK. Pokud v počítači není procesor Intel, doporučujeme používat pro vývoj fyzické zařízení s Androidem.

### <a name="hyper-v-is-installed-or-active"></a>Technologie Hyper-V je nainstalována nebo aktivní

![Technologie Hyper-V je nainstalována nebo aktivní](troubleshooting-images/02-hyper-v-active.png)

Když se zobrazí toto dialogové okno, Hyper-V je nainstalována nebo aktivní a musí se zakázat. [Zakázání Hyper-V](#disable-hyperv) vysvětluje, jak tento problém vyřešit.

### <a name="haxm-is-not-installed"></a>Je modul HAXM není nainstalovaný

![Modul HAXM není nainstalovaný.](troubleshooting-images/03-haxm-not-installed.png)

Toto dialogové okno znamená, že počítač má procesor Intel, technologie Hyper-V je zakázaná, ale modul HAXM není nainstalovaný.
[Instaluje se modul HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) popisuje kroky pro instalaci modulu HAXM.

### <a name="haxm-process-not-running"></a>Modul HAXM proces neběží.

![Modul HAXM proces není spuštěn](troubleshooting-images/04-haxm-process-not-running.png)

Toto dialogové okno se zobrazí, pokud váš počítač má procesor Intel, technologie Hyper-V je zakázaná, je nainstalován modul Intel HAXM, ale proces modul HAXM není spuštěn. Chcete-li tento problém vyřešit, otevřete okno příkazového řádku a zadejte následující příkaz:

```cmd
sc query intelhaxm
```

Pokud je spuštěn proces modul HAXM, byste měli vidět výstup podobný následujícímu:

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

Pokud `STATE` není nastavená na `RUNNING`, naleznete v tématu [Intel Hardware Accelerated spouštění Správce použití](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) aby problém pomohl vyřešit.


### <a name="other-failures"></a>Jiné chyby

![Jiné chyby](troubleshooting-images/05-other-failure.png)

Toto dialogové okno se zobrazí, pokud počítač má procesor Intel, Hyper-V je zakázaná, Intel HAXM je nainstalován, je spuštěn proces modul HAXM, ale emulátor se nepodaří spustit pro některé z neznámého důvodu.
Chcete-li tuto chybu vyřešit, přečtěte si téma [Intel Hardware Accelerated spouštění Správce použití](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="disabling-performance-warnings"></a>Zakázání upozornění výkonu

Pokud nechcete zobrazovat upozornění výkonu, můžete je zakázat. V sadě Visual Studio, klikněte na tlačítko **nástroje > Možnosti > Xamarin > Nastavení Androidu** a zakažte **podporované upozornit, pokud není akcelerace AVD (modul HAXM)** možnost:

[![Zakázání upozornění akcelerace AVD](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Od verze Visual Studio pro Mac verze 7.2 (sestavení 559), dialogové okno upozornění výkonu může zobrazit při prvním nasazení aplikace do emulátoru Androidu. Tato upozornění jsou vysvětleny níže.

### <a name="haxm-is-not-installed"></a>Je modul HAXM není nainstalovaný

![Modul HAXM není nainstalovaný.](troubleshooting-images/03-haxm-not-installed.png)

Toto dialogové okno znamená, že modul HAXM není nainstalovaný.
[Instaluje se modul HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) popisuje kroky pro instalaci modulu HAXM.

### <a name="haxm-process-not-running"></a>Modul HAXM proces neběží.

![Modul HAXM proces není spuštěn](troubleshooting-images/04-haxm-process-not-running.png)

Toto dialogové okno se zobrazí, pokud proces modul HAXM není spuštěný. Podrobné informace pomáhající při řešení tohoto problému, najdete v části [Intel Hardware Accelerated spouštění Správce použití](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) aby problém pomohl vyřešit.

### <a name="other-failures"></a>Jiné chyby

![Jiné chyby](troubleshooting-images/05-other-failure.png)

Emulátor se nepodaří spustit pro některé z neznámého důvodu, zobrazí se toto dialogové okno. Chcete-li tuto chybu vyřešit, přečtěte si téma [Intel Hardware Accelerated spouštění Správce použití](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) aby problém pomohl vyřešit.

-----

## <a name="deployment-issues"></a>Problémy s nasazením

Pokud se zobrazí chyba týkající se selhání instalace APK na emulátoru nebo selhání při spouštění přemostění pro ladění Androidu (**adb**), ověřte, že sady Android SDK můžete připojit k vaší emulátoru. Chcete-li to provést, postupujte následovně:

1. Spusťte emulátor ze **Správce zařízení s Androidem** (vyberte virtuální zařízení a klikněte na tlačítko **Start**).

2. Otevřete příkazový řádek a přejděte do složky, kde **adb** je nainstalována. Například na Windows, může se jednat na: **C:\\Program Files (x86)\\Android\\sady android sdk\\nástrojů platformy\\adb.exe**.

3. Zadejte následující příkaz:

   ```shell
   adb devices
   ```

4. Pokud emulátor je přístupná ze sady Android SDK, emulátor se zobrazí v seznamu připojených zařízení. Příklad:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Pokud v tomto seznamu nezobrazí emulátor, začněte **správce sady Android SDK**, platí všechny aktualizace, a zkuste v emulátoru znovu spustit.


## <a name="haxm-issues"></a>Problémy s modulem HAXM

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pokud emulátoru Androidu nespustí správně, to je často způsobeno problémy s modulem HAXM. Modul HAXM problémy jsou často výsledkem je v konfliktu s jiných virtualizačních technologií, nesprávné nastavení nebo ovladač zastaralý modul HAXM.

<a name="virt-conflicts" />

### <a name="haxm-virtualization-conflicts"></a>Modul HAXM virtualizace je v konfliktu

Modul HAXM může dojít ke konfliktu s jinými technologiemi, které používají virtualizaci, jako je například technologie Hyper-V, Windows Device Guard a některé antivirový software:

- **Technologie Hyper-V** &ndash; Pokud používáte verzi Windows před **Windows 10. dubna 2018 update (build 1803)** a povolenou technologii Hyper-V, postupujte podle kroků v [zakázání Hyper-V](#disable-hyperv).

- **Device Guard** &ndash; Device Guard a Credential Guard můžete zabránit v Hyper-V deaktivaci počítačích s Windows. Pokud chcete zakázat funkce Device Guard a Credential Guard, přečtěte si téma [zakázání funkce Device Guard](#disable-devguard).

- **Antivirový Software** &ndash; Pokud máte spuštěný antivirový software, který používá hardwarově řízenou virtualizaci (například Avast), zakázání nebo odinstalaci tohoto softwaru, restartování a zkuste to znovu emulátor Android SDK.


### <a name="incorrect-bios-settings"></a>Nastavení systému BIOS nesprávný

Pokud používáte modul HAXM v počítači s Windows, modul HAXM nebude fungovat, pokud je povolená technologie virtualizace (Intel VT-x) v systému BIOS. Pokud VT-x je zakázané, se zobrazí chybu podobná následující při pokusu o spuštění emulátoru Androidu:

**Tento počítač splňuje požadavky pro modul HAXM, ale není zapnuté technologií Intel Virtualization (VT-x).**

Chcete-li opravit tuto chybu, spustí počítači v systému BIOS, povolte VT-x a SLAT (překlad adres druhé úrovně) a pak restartujte počítač zpátky do Windows.

<a name="disable-hyperv" />

### <a name="disabling-hyper-v"></a>Zakázání Hyper-V

Pokud používáte verzi Windows před **Windows 10. dubna 2018 Update (build 1803)** a povolenou technologii Hyper-V, je nutné zakázat Hyper-V a restartujte počítač nainstalovat a používat modul HAXM. Pokud používáte **Windows 10. dubna 2018 Update (build 1803)** nebo novější verze emulátoru Androidu 27.2.7 nebo novější můžete použít technologie Hyper-V (ne modul HAXM) pro hardwarové akcelerace, takže není nutné zakázat Hyper-V.

Můžete zakázat Hyper-V pomocí ovládacích panelů pomocí následujících kroků:

1. Do vyhledávacího pole Windows zadejte **programy a** klikněte **programy a funkce** výsledek hledání.

2. V Ovládacích panelech **programy a funkce** dialogového okna, klikněte na tlačítko **Windows zapnout nebo vypnout funkce**:

    ![Zapnout nebo vypnout funkce Windows](troubleshooting-images/win/07-turn-windows-features.png)

3. Zrušte zaškrtnutí políčka **Hyper-V** a restartujte počítač:

    ![V dialogovém okně funkcí Windows zakážete Hyper-V](troubleshooting-images/win/08-uncheck-hyper-v.png)

Alternativně můžete použít následující rutinu Powershellu zakázat Hyper-V:

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM a Microsoft Hyper-V nemohou být současně aktivní ve stejnou dobu. Bohužel neexistuje žádný způsob, jak přepínat mezi Hyper-V a modul HAXM bez restartování počítače. 

V některých případech pomocí výše uvedených kroků nebude úspěšné, v zakážete Hyper-V, pokud je povolena funkce Device Guard a Credential Guard. Pokud nemůžete zakázat Hyper-V (nebo zdá se deaktivuje, ale modul HAXM instalace se nezdaří), postupujte podle kroků v další části zakázání funkce Device Guard a Credential Guard.

<a name="disable-devguard" />

### <a name="disabling-device-guard"></a>Zakázání funkce Device Guard

Device Guard a Credential Guard můžete zabránit v Hyper-V deaktivaci počítačích s Windows. Často se jedná o problém pro připojené k doméně počítače, které jsou konfigurovány a řídí vlastnící organizace.
Ve Windows 10, pomocí následujících kroků a zjistěte, jestli **Device Guard** běží:

1. V **Windows Search**, typ **systémové informace** spustit **systémové informace** aplikace.

2. V **systému Souhrn**, vzhled a zjistěte, jestli **virtualizace Guard zařízení na základě zabezpečení** je k dispozici a je v **systémem** stavu:

   [![Device Guard je instalována a spuštěna](troubleshooting-images/win/09-device-guard-sml.png)](troubleshooting-images/win/09-device-guard.png#lightbox)

Pokud je povolená funkce Device Guard, použijte následující kroky pro jeho zakázání:

1. Ujistěte se, že **Hyper-V** je zakázaný (v části **zapnout nebo vypnout funkce Windows**) jak je popsáno v předchozí části.

2. V poli Windows Search zadejte **gpedit** a vyberte **upravit zásady skupiny** výsledek hledání. Tím se spustí **Editor místních zásad skupiny**.

3. V **Editor místních zásad skupiny**, přejděte na **konfigurace počítače > šablony pro správu > Systém > Device Guard**:

   [![Device Guard v Editoru místních zásad skupiny](troubleshooting-images/win/10-group-policy-editor-sml.png)](troubleshooting-images/win/10-group-policy-editor.png#lightbox)

4. Změna **zapnout na zabezpečení na základě virtualizace** k **zakázané** (jak je uvedeno výše) a ukončení **Editor místních zásad skupiny**.

5. V poli Windows Search zadejte **cmd**. Když **příkazového řádku** se zobrazí ve výsledcích hledání klikněte pravým tlačítkem na **příkazového řádku** a vyberte **spustit jako správce**.

6. Zkopírujte a vložte následující příkazy do okna příkazového řádku (Pokud jednotky **Z:** je v použít, vyberte nepoužívanému písmenu jednotky místo toho použít):

        mountvol Z: /s
        copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
        bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
        bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
        bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
        mountvol Z: /d

7. Restartujte počítač. Na obrazovce spuštění byste měli vidět řádek podobný tomuto:

   **Opravdu chcete zakázat Credential Guard?**

   Stisknutím klávesy označený zakázat Credential Guard po zobrazení výzvy.

8. Po restartování počítače, zkontrolujte znovu, technologie Hyper-V bylo zakázané (jak je popsáno v předchozích krocích).

Pokud ještě není zakázáno Hyper-V, zásady pro počítače připojené k doméně je možná nebudete plynoucí ze zakázání funkce Device Guard nebo Credential Guard. V takovém případě můžete požádat o výjimku ze správce domény, aby bylo možné vyjádřit výslovný nesouhlas Credential Guard. Alternativně můžete použít počítač, který není připojený k doméně používat modul HAXM.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud emulátoru Androidu nespustí správně, to je často způsobeno problémy s modulem HAXM. Modul HAXM problémy jsou často výsledkem je v konfliktu s jiných virtualizačních technologií, nesprávné nastavení nebo ovladač zastaralý modul HAXM. Zkuste znovu nainstalovat modul HAXM ovladač, pomocí kroků uvedených v [nainstalovat modul HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm).

-----

