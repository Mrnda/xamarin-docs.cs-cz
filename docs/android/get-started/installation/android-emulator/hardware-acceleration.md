---
title: "Hardwarová akcelerace emulátoru systému Android"
description: "Postup přípravy počítače pro maximální výkon emulátoru sady SDK pro Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/22/2017
ms.openlocfilehash: 7560900ace62a737ac765bcfe93f759f8985aca2
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Hardwarová akcelerace emulátoru systému Android

Protože je prohibitively pomalé bez hardwarové akcelerace, Intel na emulátoru Android SDK HAXM (hardwaru Accelerated správce spuštění) je doporučeným způsobem výrazně zlepšit výkon emulátoru Android SDK.


## <a name="haxm-overview"></a>Přehled HAXM

HAXM je modul virtualizace s hardwarovým řízením (hypervisor), který používá virtualizace technologie Intel (VT) pro urychlení emulace aplikace pro Android na hostitelském počítači. V kombinaci s Androidem x86 bitové kopie emulátoru zadaný Intel a oficiální Android SDK Manager HAXM umožňuje rychlejší Android emulace v systémech VT povolena. Pokud vyvíjíte na počítači s Intel využití procesoru, který má VT možnosti, můžete využít výhod z HAXM výrazně urychlit emulátoru Android SDK (Pokud si nejste jistí, jestli vaše procesoru podporuje VT, přečtěte si téma [určit, pokud vaše procesoru podporuje Intel Virtualizační technologie](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

Emulátoru Android SDK automaticky využívá HAXM případě, že je k dispozici. Vyberete-li **x86**– na základě virtuální zařízení (jak je popsáno v [konfigurace a použití](~/android/deploy-test/debugging/android-sdk-emulator/index.md)), virtuální zařízení bude používat HAXM hardwarovou akceleraci. Než poprvé použijete emulátoru Android SDK, je vhodné ověřit, že HAXM je nainstalovaná a k dispozici pro Android emulátoru sady SDK.

> [!NOTE]
> HAXM nelze spustit na virtuálním počítači.


## <a name="verifying-haxm-installation"></a>Ověření instalace HAXM

Můžete zkontrolovat, zda je k dispozici zobrazením HAXM **od emulátoru Android** okno při spuštění emulátor. Spustit Android emulátor sady SDK, postupujte takto:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknutím na spustit Správce emulátoru Android **nástroje > Android > Správce emulátoru Android**:

    [![Umístění položky nabídky Správce emulátoru Android](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Pokud se zobrazí **upozornění výkonu** dialogové okno podobné následujícímu, pak HAXM je ještě nebyly správně nainstalovány nebo nakonfigurovány ve vašem počítači:

    ![Dialogové okno upozornění výkonu, HAXM není připraven](hardware-acceleration-images/win/11-perf-warn.png)

   Pokud **upozornění výkonu** se zobrazí dialogové okno jako to, najdete v části [upozornění výkonu](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) určit příčinu a vyřešte příčinu problému.

3. Vyberte **x86** bitové kopie (například **Visual Studio\_android 23\_x86\_phone**), klikněte na tlačítko **spustit**, pak klikněte na tlačítko  **Spusťte**:

    ![Počínaje obrázku výchozí virtuální zařízení Android emulátoru sady SDK](hardware-acceleration-images/win/02-start-default-avd.png)

4. Sledovat **od emulátoru Android** dialogového okna při spouštění v emulátoru. Pokud je nainstalovaná HAXM, zobrazí se zpráva, **HAX je funkční a emulátoru běží v režimu rychlé virt.krychle** jak je vidět na tomto snímku obrazovky:

    ![HAXM je uvedené jako dostupné v dialogovém okně spouštění emulátoru systému Android](hardware-acceleration-images/win/03-haxm-detected.png)

   Pokud se tato zpráva nezobrazí, HAXM pravděpodobně nejsou nainstalovány. Například zde je snímek obrazovky zprávu, že se že můžete setkat, pokud není k dispozici HAXM:

    ![HAXM není k dispozici na tomto počítači](hardware-acceleration-images/win/04-haxm-error.png)

   Pokud HAXM není k dispozici ve vašem počítači, nainstalujte HAXM pomocí kroků v další části.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknutím na spustit Správce emulátoru Android **nástroje > Správce emulátorů Google**:

    [![Umístění položky nabídky Správce emulátoru Android](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Pokud se zobrazí **upozornění výkonu** dialogové okno podobné následujícímu, pak HAXM je ještě nebyly správně nainstalovány nebo nakonfigurovány ve vašem počítači:

    ![Dialogové okno upozornění výkonu, HAXM není připraven](hardware-acceleration-images/mac/04-avd-warning.png)

   Pokud **upozornění výkonu** se zobrazí dialogové okno jako to, najdete v části [upozornění výkonu](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) určit příčinu a vyřešte příčinu problému.

3. Vyberte **x86** bitové kopie (například **Android\_Accelerated\_x86**), klikněte na tlačítko **spustit**, pak klikněte na tlačítko **spusťte**:

    [![Počínaje obrázku výchozí virtuální zařízení Android emulátoru sady SDK](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. Sledovat **od emulátoru Android** dialogového okna při spouštění v emulátoru. Pokud je nainstalovaná HAXM, zobrazí se zpráva, **HAX je funkční a emulátoru běží v režimu rychlé virt.krychle** jak je vidět na tomto snímku obrazovky:

    ![HAXM je uvedené jako dostupné v dialogovém okně spouštění emulátoru systému Android](hardware-acceleration-images/mac/03-haxm-detected.png)

   Pokud HAXM není k dispozici ve vašem počítači (např. Pokud se zobrazí chybová zpráva jako _zkontrolujte Intel HAXM je propertly nainstalované a dá se použít_), postupujte podle kroků v další části k instalaci HAXM.


-----

<a name="install-haxm" />

## <a name="installing-haxm"></a>Instalace HAXM

Pokud není možné spustit v emulátoru, HAXM pravděpodobně nutné být nainstalován ručně. HAXM instalovat balíčky pro systém Windows a systému macOS jsou k dispozici [správce spuštění Accelerated hardwaru Intel](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) stránky. Použijte následující postup ke stažení a instalaci HAXM ručně:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Z webu Intel, stáhněte si nejnovější [modul virtualizace HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) Instalační služby systému Windows. Výhoda stahování HAXM instalační program přímo z webu Intel spočívá v tom, můžete si být jistí pomocí nejnovější verze.

   Alternativně můžete použít Správce SDK pro stažení instalačního programu HAXM (ve Správci SDK, klikněte na tlačítko **nástroje > Funkce > Intel x86 emulátoru akcelerátoru (Instalační program HAXM)**). Sadu Android SDK normálně stáhne instalační program HAXM do následujícího umístění:

   **C:\\Program Files (x86)\\Android\\android-sdk\\extras\\intel\\Hardware\_Accelerated\_Execution\_Manager**

   Všimněte si, že správce SDK nenainstaluje HAXM jenom stáhne instalační program HAXM na výše uvedeném místě; Máte pořád spustit ručně.

2. Spustit **intelhaxm android.exe** spusťte instalační program HAXM. Přijměte výchozí hodnoty v dialogových oknech instalační program:

   ![Okno Nastavení správce spuštění Accelerated hardwaru Intel](hardware-acceleration-images/win/05-haxm-installer.png)

Pokud se zobrazí následující dialogové okno chyby (_tento počítač nepodporuje technologií Intel Virtualization (VT-x) nebo jej výlučně používá technologie Hyper-V_), pak technologie Hyper-V musí být zakázáno, před instalací HAXM:

![HAXM nelze nainstalovat z důvodu konfliktu technologie Hyper-V](hardware-acceleration-images/win/06-cant-install-haxm.png)

V další části vysvětluje, jak zakázat technologie Hyper-V.

<a name="disable-hyperv" />

## <a name="disabling-hyper-v"></a>Zakázání technologie Hyper-V

Pokud používáte systém Windows s technologií Hyper-V povolena, musíte ji vypnout a restartovat počítač, aby nainstalovat a používat HAXM. Technologie Hyper-V z ovládacích panelů můžete zakázat pomocí následujících kroků:

1. Do vyhledávacího pole Windows zadejte **programy a** klikněte **programy a funkce** výsledek hledání.

2. V Ovládacích panelech **programy a funkce** dialogové okno, klikněte na tlačítko **Windows zapnout nebo vypnout funkce**:

    ![Zapnutí funkce systému Windows, nebo vypnutí](hardware-acceleration-images/win/07-turn-windows-features.png)

3. Zrušte zaškrtnutí políčka **technologie Hyper-V** a restartujte počítač:

    ![Zakázání technologie Hyper-V v dialogovém okně funkce systému Windows](hardware-acceleration-images/win/08-uncheck-hyper-v.png)

Alternativně můžete pomocí následujících rutin Powershellu zakázat Hyper-V:

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM i Microsoft Hyper-V nemůže být aktivní ve stejnou dobu. Bohužel není aktuálně žádný způsob, jak přepínat mezi mezi Hyper-V a HAXM bez restartování počítače. Pokud chcete použít [Visual Studio Emulator for Android](~/android/deploy-test/debugging/visual-studio-android-emulator.md) (která závisí na technologii Hyper-V), nebude možné pomocí emulátoru Android SDK bez nutnosti restartování. Jeden ze způsobů použití technologie Hyper-V a HAXM je vytvoření instalace s možností více systémů, jak je popsáno v [vytváření žádný spouštěcí položku hypervisoru](https://blogs.msdn.microsoft.com/virtual_pc_guy/2008/04/14/creating-a-no-hypervisor-boot-entry/).

V některých případech pomocí výše uvedené kroky nepovede v zakázání technologie Hyper-V, pokud je povolena ochrana zařízení a ochranu přihlašovacích údajů. Pokud nelze zakázat technologie Hyper-V (nebo ji zdá se, že se zakáže, ale HAXM instalace se nezdaří), postupujte podle kroků v další části zakázat ochranu zařízení a ochranu přihlašovacích údajů.

<a name="disable-devguard" />

## <a name="disabling-device-guard"></a>Zakázání ochranou zařízení

Ochrana zařízení a ochranu přihlašovacích údajů mohou zabránit technologie Hyper-V bude zakázán na počítače s Windows. Často se jedná o problém pro počítače připojené k doméně, které jsou konfigurovány a řídí vlastnící organizace.
Ve Windows 10, použijte následující postup pro případ, **Device Guard** běží:

1. V **Windows Search**, typ **informace systému** spustit **informace o systému** aplikace.

2. V **systému Souhrn**, vzhled a zjistěte, zda **ochrana virtualizace zařízení na základě zabezpečení** je k dispozici a je v **systémem** stavu:

   [![Ochrana zařízení je existovat a běžet](hardware-acceleration-images/win/09-device-guard-sml.png)](hardware-acceleration-images/win/09-device-guard.png#lightbox)

Pokud je povolena ochrana zařízení, použijte ji zakázat následující kroky:

1. Ujistěte se, že **technologie Hyper-V** vypnutá (v části **zapnout nebo vypnout funkce systému Windows**) jak je popsáno v předchozí části.

2. Do vyhledávacího pole Windows zadejte **gpedit** a vyberte **upravit zásady skupiny** výsledek hledání. Spustí se **Editor místních zásad skupiny**.

3. V **Editor místních zásad skupiny**, přejděte na **konfigurace počítače > šablony pro správu > Systém > Device Guard**:

   [![Ochrana zařízení v Editoru místních zásad skupiny](hardware-acceleration-images/win/10-group-policy-editor-sml.png)](hardware-acceleration-images/win/10-group-policy-editor.png#lightbox)

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

1. Z webu Intel, stáhněte si nejnovější [modul virtualizace HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) instalační program systému macOS.

2. Spusťte instalační program HAXM. Přijměte výchozí hodnoty v dialogových oknech instalační program:

   [![Okno Nastavení správce spuštění Accelerated hardwaru Intel](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----
