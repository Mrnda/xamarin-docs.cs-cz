---
title: Je možné se připojit k Android emulátorů spuštěn na Macu z virtuálního počítače Windows?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 4afe2b9fab8eeebb3f0451379b8e3937cc8d8f55
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/05/2018
ms.locfileid: "30823061"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Je možné se připojit k Android emulátorů spuštěn na Macu z virtuálního počítače Windows?

Pro připojení k Google Android emulátor na Macu spuštění z virtuálního počítače s Windows, použijte následující kroky:

1.  Spusťte emulátor na Mac.

2.  Příkaz kill `adb` server na Mac:

    ```bash
    adb kill-server
    ```

3.  Všimněte si, že je emulátor naslouchá na 2 porty protokolu TCP v síťovém rozhraní zpětné smyčky:

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    Port lichých je používaný pro připojení k `adb`. Viz také [ http://developer.android.com/tools/devices/emulator.html#emulatornetworking ](http://developer.android.com/tools/devices/emulator.html#emulatornetworking).

4.  _Možnost 1_: použití [ `nc` ](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html) příchozí forward TCP paketů přijatých externě na portu 5555 (nebo jiný port chcete) na port lichými čísly v rozhraní zpětné smyčky (**127.0.0.1 5555** v tomto příkladu), a předávat odchozí pakety zpět jiným způsobem:

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    Tak dlouho, dokud `nc` příkazy zůstat spuštěná v okno terminálu, pakety se předají podle očekávání. Můžete zadat CTRL-C v okno terminálu a ukončete operaci `nc` příkazy po dokončení pomocí emulátoru.

    (Možnost 1 je většinou jednodušší než možnost 2, zvlášť pokud **předvolbách systému > zabezpečení a ochrana osobních údajů > brány Firewall** je zapnutá.) 

    _Možnost 2_: použití [ `pfctl` ](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html) přesměrování protokolu TCP pakety z portu `5555` (nebo jiný port chcete) na [sdílené sítě](http://kb.parallels.com/en/4948) rozhraní k portu lichých na rozhraní zpětné smyčky (`127.0.0.1:5555` v tomto příkladu):

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    Tento příkaz nastaví port předávání pomocí `pf packet filter` systémové služby. Zalomení řádků důležité. Ujistěte se, že zachovat jejich beze změn při kopírování vložení. Budete také muset upravit název rozhraní z *vmnet8* při použití Parallels. `vmnet8` je název speciální *zařízení NAT* pro *sdílené sítě* režimu v VMWare Fusion. Následuje příslušné síťové rozhraní v Parallels [vnic0](http://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm).

5.  Připojení k emulátoru počítače Windows:

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    Nahraďte "ip adresa z –-mac" s IP adresou MAC, například jako uvedené podle `ifconfig vmnet8 | grep 'inet '`. V případě potřeby nahradit `5555` s portu, které chcete z kroku 4\. (Poznámka: jeden ze způsobů, jak získat přístup přes příkazový řádek k `adb` přes [ **nástroje > Android > Android Adb příkazového řádku** ](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) v sadě Visual Studio.)

### <a name="alternate-technique-using-ssh"></a>Alternativní pomocí technik `ssh`

Pokud jste povolili _vzdálené přihlášení_ v systému Mac, pak můžete použít `ssh` předávání pro připojení k emulátoru portů.

1.  Instalace klienta SSH v systému Windows. Jednou z možností je instalace [Git pro Windows](https://git-for-windows.github.io/). `ssh` Příkaz bude k dispozici v **Git Bash** příkazového řádku.

2.  Postupujte podle kroků 1 – 3 z výše spusťte emulátor, ukončit `adb` serveru v systému Mac a identifikovat emulátoru porty.

3.  Spustit `ssh` v systému Windows k nastavení přesměrování obousměrný port mezi místních portů v systému Windows (`localhost:15555` v tomto příkladu) a portu lichých emulátoru na rozhraní zpětné smyčky Mac (`127.0.0.1:5555` v tomto příkladu):

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    Nahraďte `mac-username` s vaším jménem Mac, jak je uvedeno ve `whoami`. Nahraďte `ip-address-of-the-mac` s IP adresou Mac.

4.  Připojte k emulátoru pomocí místních portů v systému Windows:

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (Poznámka: jednoduchý způsob, jak získat přístup přes příkazový řádek pro jednu `adb` přes [ **nástroje > Android > Android Adb příkazového řádku** v sadě Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

Malé upozornění: Pokud používáte port `5555` pro místní port `adb` si bude myslet emulátoru, ke které běží místně v systému Windows. To nezpůsobí žádné potíže v sadě Visual Studio, ale v sadě Visual Studio pro Mac způsobuje, že ukončíte okamžitě po spuštění aplikace.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>Pomocí alternativní postup `adb -H` zatím není podporován

Teoreticky může být použití další přístup `adb`na integrované funkce pro připojení k `adb` server spuštěný ve vzdáleném počítači (viz například [ http://stackoverflow.com/a/18551325 ](http://stackoverflow.com/a/18551325)).
Ale Xamarin.Android IDE rozšíření není aktuálně poskytují způsob, jak nakonfigurovat tuto možnost.

## <a name="contact-information"></a>Kontaktní informace

Tento dokument popisuje aktuální chování od. března 2016. Podle postupu popsaného v tomto dokumentu není součástí stabilní testování suite pro Xamarin, takže může v budoucnu rozdělit.

Pokud si všimnete, že už funguje techniku, nebo pokud si všimnete dalších chyby v dokumentu, libosti přidat do diskuse ve fóru vlákně, v následující: [ http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm ](http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm).
Dík!

