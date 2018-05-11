---
title: Hardwarová akcelerace emulátoru systému Android
description: Postup přípravy počítače pro maximální výkon emulátor Google Android
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/07/2018
ms.openlocfilehash: 2d903df97da2e8d6ae0c5df3b1ba09dd3015e404
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Hardwarová akcelerace emulátoru systému Android

Emulátor Google Android je prohibitively pomalé bez hardwarovou akceleraci. Je možné výrazně zlepšit výkon emulátor Google Android pomocí bitové kopie hardwarového speciální emulátoru cíl x86 hardwaru a mezi dvěma virtualizačních technologií:

1. **Microsoft je technologie Hyper-V a platformou hypervisoru** &ndash; technologie Hyper-V je součástí virtualizace, která je k dispozici ve Windows 10, která umožňuje spuštěné virtualizované počítačové systémy na fyzickém hostiteli. Toto je doporučená virtualizační technologii pro Zrychlený Image emulátor Google Android. Další informace o technologii Hyper-V, najdete [Hyper-V v systému Windows 10 průvodce](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).
2. **Společnosti Intel hardwaru Accelerated spuštění správce (HAXM)** &ndash; Toto je modul virtualizace pro počítače se systémem procesory Intel. Toto je doporučená virtualizace modul pro vývojáře, které nelze použít technologie Hyper-V.

Android SDK Manager bude automaticky vytváří použití hardwarovou akceleraci v případě, že je k dispozici je spuštěná image emulátoru speciálně pro **x86**– na základě virtuální zařízení (jak je popsáno v [konfigurace a použití ](~/android/deploy-test/debugging/android-sdk-emulator/index.md)).

## <a name="hyper-v-overview"></a>Přehled technologie Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Podpora technologie Hyper-V je aktuálně ve verzi Preview.

Vývojáři, kteří používají Windows 10 (duben 2018 aktualizace) důrazně doporučujeme používat společnosti Microsoft Hyper-V. Visual Studio Tools pro Xamarin usnadnění pro vývojáře pro testování a ladění aplikací jejich Xamarin.Android v situacích, kdy zařízení se systémem Android je nedostupné nebo nepoužitelné.

Chcete-li začít s použitím technologie Hyper-V a emulátor Google Android:

1. **Aktualizace systému Windows 10. dubna 2018 aktualizace (sestavení 1803)** &ndash; ověřte, jakou verzi systému Windows je spuštěna, klikněte v panelu vyhledávání Cortana a typ **o**. Vyberte **o vašem počítači** ve výsledcích hledání. Přejděte dolů v **o** dialogové okno Možnosti **specifikace Windows** části. **Verze** by měla být alespoň 1803:

    [![Specifikace Windows](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

1. **Povolení technologie Hyper-V a na platformu hypervisoru Windows** &ndash; panelu v vyhledávání Cortaně, typ **Windows zapnout nebo vypnout funkce**. Přejděte dolů v **funkce systému Windows** dialogové okno a ujistěte se, že **platformu hypervisoru Windows** je povoleno.

    [![Technologie Hyper-V a povolit platformu hypervisoru Windows](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

    Může být nutné restartování systému Windows po povolení Hyper-V a na platformu hypervisoru Windows.

1. **Nainstalujte [Visual Studio 15.8 Preview 1](https://aka.ms/hyperv-emulator-dl)**  &ndash; tato verze sady Visual Studio poskytuje podporu rozhraní IDE pro spuštění emulátor Google Android s podporou technologie Hyper-V.

1. **Instalovat balíček emulátor Google Android 27.2.7 nebo vyšší** &ndash; k instalaci tohoto balíčku, přejděte na **nástroje > Android > Android SDK Manager** v sadě Visual Studio. Vyberte **nástroje** kartě a zajistit komponentu emulátoru Android minimálně verze 27.2.7.

    [![Dialogové okno sady Android SDK a nástroje](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

### <a name="known-issues"></a>Známé problémy

* Při použití určitých Intel a na základě AMD procesorů se může snížit výkon.
* Aplikace pro Android, může trvat neobvyklé množství času se načíst na nasazení.
* Chyba přístupu k MMIO může zabránit občas spouštěcího emulátoru systému Android. Restartování emulátoru by měla potíže vyřešit následovně.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Podpora technologie Hyper-V vyžaduje Windows 10. Podrobnosti najdete [požadavky technologie Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements) další podrobnosti.

-----

## <a name="haxm-overview"></a>Přehled HAXM

HAXM je modul virtualizace s hardwarovým řízením (hypervisor), který používá virtualizace technologie Intel (VT) pro urychlení emulace aplikace pro Android na hostitelském počítači. V kombinaci s Androidem x86 bitové kopie emulátoru zadaný Intel a oficiální Android SDK Manager HAXM umožňuje rychlejší Android emulace v systémech VT povolena. 

Pokud vyvíjíte na počítači s Intel využití procesoru, který má VT možnosti, které můžete využít výhod HAXM výrazně urychlit emulátor Google Android (Pokud si nejste jistí, jestli vaše procesoru podporuje VT, přečtěte si téma [určit, pokud vaše procesoru podporuje Intel Virtualizační technologie](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

> [!NOTE]
> Nelze spustit emulátoru accelerated virtuálních počítačů uvnitř jiným virtuálním Počítačem, jako je například virtuální počítač hostovaný VirtualBox, VMWare nebo Docker. Je nutné spustit emulátor Google Android [přímo na váš hardware systému](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

Než poprvé použijete emulátor Google Android, je vhodné ověřit, že HAXM je nainstalovaná a k dispozici pro emulátor Google Android.

### <a name="verifying-haxm-installation"></a>Ověření instalace HAXM

Můžete zkontrolovat, zda je k dispozici zobrazením HAXM **od emulátoru Android** okno při spuštění emulátor. Spusťte emulátor Google Android, postupujte takto:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknutím na spustit Správce emulátoru Android **nástroje > Android > Správce emulátoru Android**:

    [![Umístění položky nabídky Správce emulátoru Android](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Pokud se zobrazí **upozornění výkonu** dialogové okno podobné následujícímu, pak HAXM je ještě nebyly správně nainstalovány nebo nakonfigurovány ve vašem počítači:

    ![Dialogové okno upozornění výkonu, HAXM není připraven](hardware-acceleration-images/win/11-perf-warn.png)

   Pokud **upozornění výkonu** se zobrazí dialogové okno jako to, najdete v části [upozornění výkonu](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) určit příčinu a vyřešte příčinu problému.

3. Vyberte **x86** bitové kopie (například **Visual Studio\_android 23\_x86\_phone**), klikněte na tlačítko **spustit**, pak klikněte na tlačítko  **Spusťte**:

    ![Počínaje emulátor Google Android výchozí image virtuálního zařízení](hardware-acceleration-images/win/02-start-default-avd.png)

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

    [![Počínaje emulátor Google Android výchozí image virtuálního zařízení](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. Sledovat **od emulátoru Android** dialogového okna při spouštění v emulátoru. Pokud je nainstalovaná HAXM, zobrazí se zpráva, **HAX je funkční a emulátoru běží v režimu rychlé virt.krychle** jak je vidět na tomto snímku obrazovky:

    ![HAXM je uvedené jako dostupné v dialogovém okně spouštění emulátoru systému Android](hardware-acceleration-images/mac/03-haxm-detected.png)

   Pokud HAXM není k dispozici ve vašem počítači (např. Pokud se zobrazí chybová zpráva jako _zkontrolujte Intel HAXM je propertly nainstalované a dá se použít_), postupujte podle kroků v další části k instalaci HAXM.


-----

<a name="install-haxm" />

### <a name="installing-haxm"></a>Instalace HAXM

Pokud není možné spustit v emulátoru, HAXM pravděpodobně nutné být nainstalován ručně. HAXM instalovat balíčky pro systém Windows a systému macOS jsou k dispozici [správce spuštění Accelerated hardwaru Intel](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) stránky. Použijte následující postup ke stažení a instalaci HAXM ručně:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Z webu Intel, stáhněte si nejnovější [modul virtualizace HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) Instalační služby systému Windows. Výhoda stahování HAXM instalační program přímo z webu Intel spočívá v tom, můžete si být jistí pomocí nejnovější verze.

   Alternativně můžete použít Správce SDK pro stažení instalačního programu HAXM (ve Správci SDK, klikněte na tlačítko **nástroje > Funkce > Intel x86 emulátoru akcelerátoru (Instalační program HAXM)**). Sadu Android SDK normálně stáhne instalační program HAXM do následujícího umístění:

   **C:\\Program Files (x86)\\Android\\android-sdk\\funkce\\intel\\hardwaru\_Accelerated\_provádění\_Manager**

   Všimněte si, že správce SDK nenainstaluje HAXM jenom stáhne instalační program HAXM na výše uvedeném místě; Máte pořád spustit ručně.

2. Spustit **intelhaxm android.exe** spusťte instalační program HAXM. Přijměte výchozí hodnoty v dialogových oknech instalační program:

   ![Okno Nastavení správce spuštění Accelerated hardwaru Intel](hardware-acceleration-images/win/05-haxm-installer.png)

## <a name="hardware-acceleration-and-amd-cpus"></a>Hardwarová akcelerace a AMD procesorů

Protože emulátor Google Android teď podporuje hardwarovou akceleraci AMD [pouze v systému Linux](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies), hardwarovou akceleraci není k dispozici pro procesory AMD počítačů se systémem Windows.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Z webu Intel, stáhněte si nejnovější [modul virtualizace HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) instalační program systému macOS.

2. Spusťte instalační program HAXM. Přijměte výchozí hodnoty v dialogových oknech instalační program:

   [![Okno Nastavení správce spuštění Accelerated hardwaru Intel](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>Související odkazy

* [Spuštění aplikace v emulátoru systému Android](https://developer.android.com/studio/run/emulator)