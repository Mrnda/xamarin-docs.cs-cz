---
title: Kde najdu Moje informace o verzi a protokoly?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: cdbe480c45e9c0117f1437b1ee632f6ea8f142e0
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>Kde najdu Moje informace o verzi a protokoly?

## <a name="outline"></a>Obrys

- [Informace o verzi](#version-information)
    - Informace o verzi systému Windows
    - Informace o verzi Mac
    - Sestavení nástroje, nástrojů platformy Android SDK – nástroje
- [Protokoly IDE a instalační program](#ide-and-installer-logs)
    - [Protokoly systému Windows](#windows-logs)
        - Xamarin Studio
        - Xamarin pro Visual Studio
        - Instalační program Xamarin Universal
        - Jednotlivé `.msi` instalační programy, podrobné protokoly
        - Spuštění Visual Studio, podrobné protokoly
    - [Protokoly Mac](#mac-logs)
        - Sestavení hostitele
    - Visual Studio for Mac
        - Xamarin Studio
        - Instalační program Xamarin
- [Podrobné sestavení výstupu](#verbose-build-output-logs)
- [Ladění protokoly pro aplikace Xamarin.Android a Xamarin.iOS](#debug-logs-for-xamarin-apps)
    - Android `adb` logcat protokoly
    - simulátoru iOS přihlásí (Mac)
    - zařízení s iOS přihlásí (Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />Informace o verzi

Obvykle je nejvhodnější odeslat zálohovat všechny informace z **kopie informace** tlačítka. V opačném případě jsme často muset požádat o další informace. Například nainstalovat úrovně rozhraní API systému Android verze operačního systému, Xcode verze a verze rozhraní .NET mohou být důležité při pomoc při řešení problému.

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Informace o verzi systému Windows

#### <a name="xamarin-studio"></a>Xamarin Studio

**Nápověda > o > Zobrazit podrobnosti > kopírování informace [tlačítko]**

#### <a name="visual-studio"></a>Visual Studio

**Nápověda > o sadě Microsoft Visual Studio > Kopírovat informace [tlačítko]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Informace o verzi Mac

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Visual Studio > o sadě Visual Studio > Zobrazit podrobnosti > kopírování informace [tlačítko]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Sestavení nástroje, nástrojů platformy Android SDK – nástroje

Otevřete Android SDK Manager a pořídit snímek horní **nástroje** části.

![](https://kb.xamarin.com/customer/portal/attachments/337323 "Snímek obrazovky Android SDK Manager > složky nástroje")

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Nástroje > Spusťte správce sady SDK pro Android**

#### <a name="visual-studio"></a>Visual Studio

Vyberte ikonu panelu nástrojů SDK Manager:

![](https://kb.xamarin.com/customer/portal/attachments/420238 "Spustit Android SDK Manager ikonu na panelu nástrojů v sadě Visual Studio")

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />Protokoly IDE a instalační program

Pro každé umístění protokolu nezapomeňte zip a připojte složce celý protokol.

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Protokoly systému Windows

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Visual Studio Tools pro Xamarin

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[Jak získat protokoly instalace sady Visual Studio](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> Instalační program "Universal" Xamarin

`%LOCALAPPDATA%\Xamarin\Universal`

Jedná se o protokoly z `XamarinInstaller.exe` Instalační služby.

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />Jednotlivé `.msi` instalační programy, podrobné protokoly

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

Referenční dokumentace: [možnosti příkazového řádku](http://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Spuštění Visual Studio, podrobné protokoly

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Referenční dokumentace:  [ /log (devenv.exe)](http://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Protokoly Mac

Můžete vybrat **přejděte > přejděte do složky** nabídky položky v hledání a pak zkopírujte a vložte některé z těchto cest do dialogu.

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio pro Mac

`~/Library/Logs/VisualStudio/7.0` (toto číslo může změnit v závislosti na verzi, kterou používáte)

Tato složka může také otevřít pomocí "Nápověda -> otevřete adresář protokolu".

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (toto číslo může změnit v závislosti na verzi, kterou používáte)

Tato složka může také otevřít pomocí "Nápověda -> otevřete adresář protokolu".

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />Instalační program "Universal" Xamarin

`~/Library/Logs/XamarinInstaller/Universal`

Jedná se o protokoly z `XamarinInstaller.dmg` Instalační služby.

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Sestavení Xamarin hostitele

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />Podrobné sestavení výstupu

1.  Povolit [výstup diagnostiky MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).

2.  Pro aplikace iOS, také povolit **mtouch podrobný výstup** přidáním `-v -v -v -v` pod **vlastnosti projektu > iOS sestavení > Obecné [tab] > Další možnosti > argumenty další mtouch**.

    **Visual Studio (Windows)**

    [![](https://kb.xamarin.com/customer/portal/attachments/337319 "Zadejte čtyři dash v do vstupního pole Další mtouch argumenty")](https://kb.xamarin.com/customer/portal/attachments/337319)

    **Visual Studio for Mac**

    [![](https://kb.xamarin.com/customer/portal/attachments/337316 "Zadejte čtyři dash v do vstupního pole Další mtouch argumenty")](https://kb.xamarin.com/customer/portal/attachments/337316)

3.  Vyčistěte a znovu sestavte projekt.

4.  Zkopírujte a vložte jeho výstup z prostředí IDE do textového souboru.
     - Visual Studio (Windows): **zobrazení > Výstup > Zobrazit výstup z: sestavení**
     - Visual Studio pro Mac: **zobrazení > dotyková zařízení > chyby > sestavení výstupu [tab]**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Ladění protokoly pro aplikace Xamarin.Android a Xamarin.iOS

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Zobrazení > dotyková zařízení > výstup aplikace**

![](https://kb.xamarin.com/customer/portal/attachments/337290 "Ikona pad výstupní aplikace v sadě Visual Studio pro Mac")


(Všimněte si, že tuto položku nabídky se zobrazí pouze po byla aplikace spuštěna.)

### <a name="visual-studio"></a>Visual Studio

**Zobrazení > Výstup > Zobrazit výstup z: ladění**

![](https://kb.xamarin.com/customer/portal/attachments/337292 "Výstup pad zobrazující možnost ladění v sadě Visual Studio")

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Android [ `adb` ](http://developer.android.com/tools/help/adb.html) logcat protokoly

Po spuštění `adb` příkaz, připojit zpět **android_logcat.txt** souboru z plochy. Tyto pokyny předpokládají, že máte pouze jedno zařízení, které jsou připojené.

Viz také [Android protokol ladění](~/android/deploy-test/debugging/android-debug-log.md) stránky.

#### <a name="visual-studio"></a>Visual Studio

1.  **Nástroje > Android > Start Android Adb příkazového řádku**
2.  Vyčištění protokolu: `adb logcat -c`
3.  Reprodukujte problém.
4.  Výstup v protokolu: `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1.  **Nástroje > Otevřete příkazový řádek sady SDK pro Android**
2.  Vyčištění protokolu: `adb logcat -c`
3.  Reprodukujte problém.
4.  Výstup v protokolu: `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />simulátoru iOS přihlásí (Mac)

*   Chcete-li získat přístup k protokolu systému, vyberte **ladění > otevřete protokol systému...**  v aplikaci v simulátoru iOS.

    ![](https://kb.xamarin.com/customer/portal/attachments/382617 "Ladění nabídky zobrazující možnost Otevřít systémového protokolu")

*   Chcete-li zobrazit sestavy havárií z simulátoru, otevřete Console.app a přejděte do `~/Library/Logs > DiagnosticReports`.

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />zařízení s iOS přihlásí (Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Zobrazení > dotyková zařízení > iOS protokolu zařízení**

#### <a name="xcode"></a>Xcode 

**Okno > zařízení > ${DeviceName}**

Jsou k dispozici v části sestavy havárií **zobrazit protokoly zařízení** tlačítko. Protokol systému pro zařízení se zobrazí v dolní části okna pod šipku zpřístupnění <img alt="Disclosure arrow" src="https://kb.xamarin.com/customer/portal/attachments/382618" style="width: 15px; height: 12px;" />.

#### <a name="xcode-5"></a>Xcode 5

**Okno > organizátora > zařízení [tab] > ${DeviceName}**
