---
title: Hardwarovou akceleraci emulátoru výkonu
description: Tento článek vysvětluje, jak používat funkce hardwarové akcelerace počítače k maximalizovat výkon emulátor Google Android.
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/05/2018
ms.openlocfilehash: 9db44d9f120f1ede5060f4680faefc49c09fffae
ms.sourcegitcommit: 5db075bdd0b62d5d1d1567c267303a6a1888c8f2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/06/2018
ms.locfileid: "34806813"
---
# <a name="hardware-acceleration-for-emulator-performance"></a>Hardwarovou akceleraci emulátoru výkonu

_Tento článek vysvětluje, jak používat funkce hardwarové akcelerace počítače k maximalizovat výkon emulátor Google Android._

## <a name="overview"></a>Přehled

Visual Studio usnadňuje vývojářům testování a ladění aplikací Xamarin.Android pomocí emulátor Google Android v situacích, kdy zařízení se systémem Android je nedostupné nebo nepoužitelné.
Ale emulátoru systému Android spouští příliš pomalu Pokud hardwarovou akceleraci není k dispozici v počítači, který ho spouští. Může výrazně zlepšit výkon emulátoru systému Android pomocí bitové kopie speciální virtuální zařízení cíl x86 hardwaru ve spojení s jedním ze dvou virtualizace technologií:

1. **Společnosti Microsoft Hyper-V a platformou hypervisoru**. Technologie Hyper-V je virtualizace funkce systému Windows, která umožňuje spustit virtualizované počítačových systémů ve fyzickém hostitelském počítači. Toto je doporučená virtualizační technologie, které chcete použít pro urychlení emulátor Google Android. Další informace o technologii Hyper-V najdete v tématu [technologie Hyper-V ve Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).

2. **Hardware společnosti Intel Accelerated správce spuštění (HAXM)**. 
   HAXM je modul virtualizace pro počítače se systémem procesory Intel.
   Toto je doporučená virtualizace modul pro počítače, které se nepodařilo spustit technologie Hyper-V.

Ke službě Google, které budou automaticky emulátoru Android použít hardwarové akcelerace, pokud se splní následující kritéria:

-   Hardwarová akcelerace je k dispozici a povolená na vývojovém počítači.

-   Emulátor běží image emulátoru vytvořený specificky pro **x86**– na základě virtuálního zařízení.

Informace o spouštění a ladění pomocí emulátoru systému Android, najdete v článku [ladění pomocí emulátor Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md).

## <a name="hyper-v"></a>Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Podpora technologie Hyper-V je aktuálně ve verzi Preview.

Vývojáři, kteří používají Windows 10 (duben 2018 aktualizovat nebo novější) důrazně doporučujeme používat společnosti Microsoft Hyper-V pro urychlení emulátor Google Android. Chcete-li používat emulátor Google Android s Hyper-V:

1. **Aktualizace systému Windows 10. dubna 2018 aktualizace (sestavení 1803) nebo novější**.
   Chcete-li ověřit, jakou verzi systému Windows, klikněte na tlačítko v panelu vyhledávání Cortana a typ **o**. Vyberte **o vašem počítači** ve výsledcích hledání. Přejděte dolů v **o** dialogovém okně můžete **Windows specifikace** části. **Verze** by měla být alespoň 1803:

    [![Specifikace Windows](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Povolit platformu hypervisoru Windows**.
   V panelu vyhledávání Cortana zadejte **Windows zapnout nebo vypnout funkce**.
   Přejděte dolů v **funkce systému Windows** dialogové okno a ujistěte se, že **platformu hypervisoru Windows** zapnutá:

    [![Povolit platformu hypervisoru Windows](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

   Povolení **platformu hypervisoru Windows** automaticky povolí technologie Hyper-V. Je vhodné k restartování Windows po provedení této změny.

3. **Nainstalujte [Visual Studio 15.8 Preview 1 nebo novější](https://www.visualstudio.com/vs/preview/)**.
   Tato verze sady Visual Studio poskytuje podporu rozhraní IDE pro spuštění emulátor Google Android s technologií Hyper-V.
 
4. **Instalovat balíček emulátor Google Android 27.2.7 nebo novější**. Chcete-li nainstalovat tento balíček, přejděte na **nástroje > Android > Android SDK Manager** v sadě Visual Studio. Vyberte **nástroje** kartě a ujistěte se, zda je verze emulátoru systému Android alespoň 27.2.7. Ujistěte se také, zda je verze nástroje pro Android SDK 26.1.1 nebo novější:

    [![Dialogové okno sady Android SDK a nástroje](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Pokud je verze emulátoru alespoň 27.2.7 ale menší než 27.3.1, následující alternativní řešení, je potřeba použít technologie Hyper-V:

    1.  V **C:\\uživatelé\\_uživatelské jméno_\\.android** složky, vytvořte soubor s názvem **advancedFeatures.ini** (Pokud tomu tak není již existuje).

    2.  Přidejte následující řádek na **advancedFeatures.ini**:
        ```
        WindowsHypervisorPlatform = on
        ```


### <a name="known-issues"></a>Známé problémy

-   Pokud není možné aktualizovat verzi emulátoru 27.2.7 nebo později po aktualizaci sady Visual Studio preview, možná budete muset nainstalovat přímo [náhled instalační program](http://aka.ms/hyperv-emulator-dl) povolit novější verze emulátoru.

-   Při použití určitých Intel a na základě AMD procesorů se může snížit výkon.

-   Aplikace pro Android, může trvat neobvyklé množství času se načíst na nasazení.

-   Chyba přístupu k MMIO může zabránit občas spouštěcího emulátoru systému Android. Restartování emulátoru by měla potíže vyřešit následovně.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Podpora technologie Hyper-V vyžaduje Windows 10. Podrobnosti najdete [požadavky technologie Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements) další podrobnosti.

-----

## <a name="haxm"></a>HAXM

HAXM je modul virtualizace s hardwarovým řízením (hypervisor), který používá virtualizace technologie Intel (VT) pro urychlení emulace aplikace pro Android na hostitelském počítači. Používání HAXM ve spojení s Androidem x86 emulátoru Image poskytované Intel umožňuje rychlejší Android emulace v systémech VT povolena.

Pokud vyvíjíte na počítači s Intel využití procesoru, který má VT možnosti, které můžete využít výhod HAXM výrazně urychlit emulátor Google Android (Pokud si nejste jistí, jestli vaše procesoru podporuje VT, přečtěte si téma [nemá Moje procesoru podporuje Intel Virtualizační technologie? ](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

> [!NOTE]
> Nelze spustit emulátoru accelerated virtuálních počítačů uvnitř jiným virtuálním Počítačem, jako je například virtuální počítač hostovaný VirtualBox, VMWare nebo Docker. Je nutné spustit emulátor Google Android [přímo na váš hardware systému](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

Před použitím emulátor Google Android poprvé, je vhodné ověřit, že HAXM je nainstalovaná a k dispozici pro emulátor Google Android používat.

### <a name="verifying-haxm-installation"></a>Ověření instalace HAXM

Můžete zkontrolovat, zda je k dispozici zobrazením HAXM **od emulátoru Android** okno při spuštění emulátor. Spusťte emulátor Google Android, postupujte takto:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Spusťte Správce zařízení Android kliknutím **nástroje > Android > Správce zařízení Android**:

    [![Umístění položky nabídky Správce zařízení Android](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Pokud se zobrazí **upozornění výkonu** dialogové okno podobné následujícímu, pak HAXM je ještě nebyly správně nainstalovány nebo nakonfigurovány ve vašem počítači:

    ![Dialogové okno upozornění výkonu, HAXM není připraven](hardware-acceleration-images/win/11-perf-warn.png)

   Pokud **upozornění výkonu** se zobrazí dialogové okno jako to, najdete v části [upozornění výkonu](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) určit příčinu a vyřešte příčinu problému.

3. Vyberte **x86** bitové kopie (například **Visual Studio\_android 23\_x86\_phone**) a klikněte na tlačítko **spustit**:

    ![Počínaje emulátor Google Android výchozí image virtuálního zařízení](hardware-acceleration-images/win/02-start-default-avd.png)

4. Sledovat **od emulátoru Android** dialogového okna při spouštění v emulátoru. Pokud je nainstalovaná HAXM, zobrazí se zpráva, **HAX je funkční a emulátoru běží v režimu rychlé virt.krychle** jak je vidět na tomto snímku obrazovky:

    ![HAXM je uvedené jako dostupné v dialogovém okně spouštění emulátoru systému Android](hardware-acceleration-images/win/03-haxm-detected.png)

   Pokud se tato zpráva nezobrazí, HAXM pravděpodobně nejsou nainstalovány. Například zde je snímek obrazovky zprávu, že se že můžete setkat, pokud není k dispozici HAXM:

    ![HAXM není k dispozici na tomto počítači](hardware-acceleration-images/win/04-haxm-error.png)

   Pokud HAXM není k dispozici ve vašem počítači, nainstalujte HAXM pomocí kroků v další části.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Spusťte Správce zařízení Android kliknutím **nástroje > Správce zařízení**:

    [![Umístění položky nabídky Správce zařízení Android](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Pokud se zobrazí **upozornění výkonu** dialogové okno podobné následujícímu, pak HAXM je ještě nebyly správně nainstalovány nebo nakonfigurovány ve vašem počítači:

    ![Dialogové okno upozornění výkonu, HAXM není připraven](hardware-acceleration-images/mac/04-avd-warning.png)

   Pokud **upozornění výkonu** se zobrazí dialogové okno jako to, najdete v části [upozornění výkonu](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn) určit příčinu a vyřešte příčinu problému.

3. Vyberte **x86** bitové kopie (například **Android\_Accelerated\_x86**) a klikněte na tlačítko **přehrání**:

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
