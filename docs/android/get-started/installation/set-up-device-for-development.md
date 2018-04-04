---
title: Nastavit zařízení pro vývoj
description: Tento článek popisuje postup nastavení zařízení se systémem Android a připojte k počítači tak, aby zařízení může sloužit ke spuštění a ladění aplikací Xamarin.Android.
ms.prod: xamarin
ms.assetid: 9116A3AA-EA00-56AF-AE70-BAEEC045EF11
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: 16716db67067f07166fa35df7e539cdf3ed1de5e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="set-up-device-for-development"></a>Nastavit zařízení pro vývoj

_Tento článek popisuje postup nastavení zařízení se systémem Android a připojte k počítači tak, aby zařízení může sloužit ke spuštění a ladění aplikací Xamarin.Android._

Nyní pravděpodobně jste viděli skvělé nové aplikace spuštěné v emulátoru systému Android a požadovaným systémem lesklý zařízení se systémem Android. Tady jsou kroky zabývajících se připojování zařízení k počítači pro ladění:

1.  **Povolit ladění na zařízení** – ve výchozím nastavení, nebude možné k ladění aplikací na zařízení se systémem Android.

2.  **Instalace ovladačů USB** – tento krok není nutný pro počítače, OS X. Počítače se systémem Windows mohou vyžadovat instalaci ovladačů USB.

3.  **Připojení zařízení k počítači** -poslední krok zahrnuje připojení zařízení k počítači pomocí USB nebo Wi-Fi.

Každý z těchto kroků se budeme podrobněji v následujících částech.


## <a name="enable-debugging-on-the-device"></a>Povolit ladění na zařízení

Je možné použít k testování aplikace platformy Android žádné zařízení se systémem Android. Ale zařízení musí být správně nakonfigurovaná před ladění situace může nastat. Potřebný postup se mírně liší v závislosti na verzi Android spuštěné na zařízení.


### <a name="android-40-to-android-41"></a>Android 4.0 Android 4.1

Pomocí následujících kroků je pro Android 4.0.x pro Android 4.1.x povoleno ladění:

1.  Přejděte na **nastavení** obrazovky.
2.  Vyberte **možnosti pro vývojáře** .
3.  Zkontrolujte **ladění USB** možnost.

Tento snímek obrazovky ukazuje **možnosti pro vývojáře** obrazovku na zařízení se systémem Android 4.0.3:

[![Možnosti pro vývojáře](set-up-device-for-development-images/developer-options-sml.png)](set-up-device-for-development-images/developer-options.png#lightbox)


### <a name="android-42-and-higher"></a>Android 4.2 a vyšší

Spouštění v systému Android 4.2 a vyšší, **možnosti pro vývojáře** ve výchozím nastavení je skrytý. Chcete-li k dispozici, přejděte na **Nastavení > o telefonu**a klepněte na **číslo sestavení** položky sedm časy a odhalit **možnosti pro vývojáře** karty:

[![Počet položek sestavení](set-up-device-for-development-images/about-phone-sml.png)](set-up-device-for-development-images/about-phone.png#lightbox)

Jednou **možnosti pro vývojáře** karta je k dispozici v části **Nastavení > systému**, otevřete ho na nich nastaveními vývojáře:

[![Obrazovka nastavení vývojáře](set-up-device-for-development-images/developer3.png)](set-up-device-for-development-images/developer3.png#lightbox)

Toto je místo, kde zůstat zapnutý a povolit možnosti pro vývojáře například ladění USB.


## <a name="install-usb-drivers"></a>Instalace ovladačů USB

Tento krok není nutný pro OS X. Jednoduše připojte zařízení Mac pomocí kabelu USB.

Může být nutné k instalaci některé další ovladače, než počítač se systémem Windows rozpozná zařízení se systémem Android pomocí USB.

> [!NOTE]
> Tyto kroky nastavení zařízení Google Nexus a jsou uvedeny jako odkaz. Kroky pro konkrétní zařízení se může lišit, ale bude postupovat podle podobný Princip. Pokud máte potíže při, vyhledávání v Internetu pro vaše zařízení.

Spustit **android.bat** aplikace v **[cesta instalace sady SDK pro Android] \tools** adresáře. Ve výchozím nastavení bude instalační program Xamarin.Android put SDK pro Android v následujícím umístění na počítači se systémem Windows:

    C:\Users\[username]\AppData\Local\Android\android-sdk


### <a name="download-the-usb-drivers"></a>Stáhněte si ovladače zařízení USB

Ovladač USB Google vyžadují zařízení se Google Nexus (s výjimkou Galaxy Nexus). Ovladač pro Galaxy Nexus se [distribuovat Samsung](http://www.samsung.com/us/support/downloads/).
Všechna ostatní zařízení s Androidem by měl používat [ovladač USB z jejich odpovídajících výrobce](http://developer.android.com/tools/extras/oem-usb.html#Drivers).

Nainstalujte **ovladač USB Google** balíček spuštěním Android SDK Manager a rozšíření **funkce** složky, jak je vidět na snímku obrazovky postupujte podle kroků:

[![Balíček ovladačů USB Google vybrané](set-up-device-for-development-images/usbdriverpackage.png)](set-up-device-for-development-images/usbdriverpackage.png#lightbox)

Zkontrolujte **ovladač USB Google** pole a klikněte na tlačítko **nainstalovat** tlačítko.
Soubory ovladače se stáhnou do následujícího umístění:

    [Android SDK install path]\extras\google\usb\_driver

Výchozí cesta pro Xamarin.Android instalace je:

    C:\Users\[username]\AppData\Local\Android\android-sdk\extras\google\usb_driver



### <a name="installing-the-usb-driver"></a>Instalace ovladače USB

Po stažení jsou ovladače zařízení USB, je nezbytné k instalaci.
K instalaci ovladačů na systému Windows 7:

1.  Připojte zařízení k počítači pomocí kabelu USB.

2.  Klikněte pravým tlačítkem na počítač z plochy nebo v Průzkumníku Windows a vyberte **spravovat** .

3.  Vyberte **zařízení** v levém podokně.

4.  Vyhledejte a rozbalte **další zařízení** v pravém podokně.

5.  Klikněte pravým tlačítkem na název zařízení a vyberte **aktualizace ovladače** .
    Tím se spustí Průvodce aktualizací hardwaru.

6.  Vyberte **vyhledat v počítači software ovladače** a klikněte na tlačítko **Další** .

7.  Klikněte na tlačítko **Procházet** a vyhledejte složku ovladač USB (ovladač Google USB je umístěn ve **[cesta instalace sady SDK pro Android] \extras\google\usb_driver**.

8.  Klikněte na tlačítko **Další** instalace ovladače.


### <a name="installing-unverified-drivers-in-windows-8"></a>Instalace neověřené ovladače v systému Windows 8

Mohou být vyžadovány další kroky instalace neověřené ovladače v systému Windows 8. Následující kroky popisují, jak nainstalovat ovladače pro Galaxy Nexus:

1.  **Přístup k rozšířené možnosti spuštění aplikace systému Windows 8** – tento krok zahrnuje restartování počítače pro přístup k rozšířené možnosti spuštění. Spuštění příkazového řádku a restartujte počítač pomocí následujícího příkazu:

        shutdown.exe /r /o

2.  **Připojte zařízení** – připojení zařízení k počítači

3.  **Spusťte nástroj Správce zařízení** – spuštění **devmgmt.msc**; byste měli vidět svoje zařízení nevidíte s žlutý trojúhelník nad ním.

4.  **Instalace ovladačů zařízení** -nainstalovat ovladače zařízení, jak je popsáno výše.



## <a name="connect-the-device-to-the-computer"></a>Připojení zařízení k počítači

Posledním krokem je připojení zařízení k počítači. Chcete-li to provést dvěma způsoby:

-   **Kabel USB** -Toto je nejjednodušší a nejběžnější způsob. Právě připojte kabel USB do zařízení a pak do počítače.

-   **Wi-Fi** -je možné se připojit zařízení se systémem Android do počítače bez použití kabelu USB přes Wi-Fi. Tento postup vyžaduje trochu další úsilí, ale může být užitečné, pokud neexistuje žádné kabelu USB nebo zařízení je daleko pro kabelu USB. Připojení přes Wi-Fi se budeme v další části.


### <a name="connecting-over-wifi"></a>Připojení přes Wi-Fi

Ve výchozím nastavení [Android ladění most](http://developer.android.com/tools/help/adb.html) (*ADB*) je nakonfigurován pro komunikaci se zařízení s Androidem přes USB. Je možné k překonfigurování pro použití protokolu TCP/IP místo USB. Chcete-li to provést, musí být zařízení a počítače ve stejné síti Wi-Fi. Nastavení prostředí pro ladění přes WiF problém tyto kroky z příkazového řádku:

1.  Určete IP adresu zařízení se systémem Android. Způsob nalezení se na IP adresu se podívejte se do části **Nastavení > Wi-Fi** a potom klepněte na zařízení je připojené k síti Wi-Fi. Tím zobrazíte nastavení obrazovky zobrazuje informace o připojení k síti, podobná co je vidět na tomto snímku obrazovky:

    ![IP adresa](set-up-device-for-development-images/ip-settings.png)

    Na některé verze systému Android nebude v seznamu existuje uveden IP adresu, ale můžete místo toho najít v části **Nastavení > o telefonu > Stav**.

2.  Připojte k počítači prostřednictvím USB zařízení se systémem Android.

3.  Poté restartujte ADB proto to pomocí protokolu TCP na portu 5555. Z příkazového řádku zadejte následující příkaz:

        adb tcpip 5555

    Po vydání tohoto příkazu se váš počítač nebude možné pro naslouchání na zařízení, která jsou připojena přes USB.

4.  Odpojte kabel USB připojení zařízení k vašemu počítači.

5.  Nakonfigurujte ADB tak, aby se připojí k zařízení s Androidem na portu, který byl zadán v kroku 1 výše:

        adb connect 192.168.1.28:5555

    Po dokončení tohoto příkazu zařízení s Androidem je připojená k počítači přes Wi-Fi.

Po dokončení ladění přes Wi-Fi, je možné resetování ADB zpět do režimu USB pomocí následujícího příkazu:

    adb usb

Je možné požádat ADB seznam zařízení, která jsou připojená k počítači. Bez ohledu na to, jak jsou připojené zařízení můžete použít následující příkaz na příkazovém řádku zobrazíte, co je připojen:

    adb devices


## <a name="summary"></a>Souhrn

Tento článek popsané postupy konfigurace zařízení s Androidem pro vývoj pomocí povolení ladění na zařízení. Zahrnuté taky jak se připojit zařízení k počítači pomocí USB nebo Wi-Fi.


## <a name="related-links"></a>Související odkazy

- [Most ladění Android](http://developer.android.com/tools/help/adb.html)
- [Pomocí hardwarových zařízení](http://developer.android.com/tools/device.html)
- [Stahování ovladačů Samsung](http://www.samsung.com/us/support/downloads/)
- [Ovladače OEM USB](http://developer.android.com/tools/extras/oem-usb.html#Drivers)
- [Ovladač Google USB](http://developer.android.com/sdk/win-usb.html)
- [Vývojáři XDA: Windows 8 - ADB/splněny ovladač problém vyřešit](http://forum.xda-developers.com/showthread.php?t=1583801)
