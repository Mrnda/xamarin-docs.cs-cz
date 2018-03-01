---
title: "Instalace a požadavky"
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: dffea94b309b3c945a37a116668486f404f544b3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="installation-and-requirements"></a>Instalace a požadavky

<script> var inspectorOnLoad = (funkce) {var primaryTextBase = "Xamarin sešity pro;" var secondaryTextBase = "nebo stáhnout pro"; var inspectorDownloadUrlMac = "https://dl.xamarin.com/interactive/XamarinInteractive.pkg"; var inspectorDownloadUrlWin = " https://DL.xamarin.com/Interactive/XamarinInteractive.msi";

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = aPrimary; var aWin = aSecondary; var macTextBase = primaryTextBase; var winTextBase = secondaryTextBase;

  Pokud (/win/i.test(navigator.platform.toLowerCase())) {aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase;}

  aMac.href = inspectorDownloadUrlMac; aMac.text = macTextBase + " Mac"; aWin.href = inspectorDownloadUrlWin; aWin.text = winTextBase + " Windows"; };

document.addEventListener ("DOMContentLoaded", inspectorOnLoad);
</script>

<a name="install" />

## <a name="download-and-install"></a>Stáhnout a nainstalovat

<ol>
  <li>Zkontrolujte <a href="#Requirements"> požadavky</a> níže.</li>
  <li>Stáhněte a nainstalujte <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">sešity Xamarin pro Mac</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">nebo ke stažení pro Windows</a>).
  </li>
  <li>Spustit <a href="~/tools/workbooks/workbook.md"> přehrávání</a> s sešity nebo zkuste si <a href="https://developer.xamarin.com/workbooks/">ukázky</a>.
    </li>
</ol>

## <a name="requirements"></a>Požadavky

#### <a name="supported-operating-systems"></a>Podporované operační systémy

- **Mac** -OS X 10.11 nebo vyšší
- **Windows** – Windows 7 nebo novější (Internet Explorer 11 nebo větší a .NET 4.6.1 nebo vyšší)

#### <a name="supported-app-platforms"></a>Platformy podporované aplikace

<table>
<thead>
  <tr>
    <th>Aplikace platformy</th>
    <th>Podpora operačního systému</th>
    <th>Poznámky</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Mac (Unified)</td>
    <td>Podporováno pouze v systému Mac</td>
    <td/>
  </tr>
  <tr>
    <td>iOS (Unified)</td>
    <td>Podporované na Mac a Windows</td>
    <td>
      <ul>
        <li>Xamarin.iOS 11.0 a Xcode 9.0 nebo vyšší musí být nainstalován na macu.</li>
        <li>Sešity iOS systémem Windows vyžaduje Mac sestavení hostitele se systémem všechny výše uvedené a <a href="~/tools/ios-simulator.md">používat vzdáleně simulátoru iOS</a> nainstalovaná v systému Windows.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Podporované na Mac a Windows</td>
    <td>Google, Visual Studio nebo Xamarin Android emulátoru, musíte použít s virtuálního zařízení > = 5.0</td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Podporováno pouze v systému Windows</td>
    <td/>
  </tr>
  <tr>
    <td>Konzole (rozhraní .NET Framework)</td>
    <td>Podporované na Mac a Windows</td>
    <td/>
  </tr>
  <tr>
    <td>Konzole (.NET Core)</td>
    <td>Podporované na Mac a Windows</td>
    <td/>
  </tr>
</tbody>
</table>

## <a name="reporting-bugs"></a>Zasílání zpráv o chybách

Prosím [hlášení problémů na Githubu][bugs]a zahrnovat všechny následující informace:

### <a name="log-files"></a>Soubory protokolu

Vždy připojte soubory protokolů klienta sešity:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

1.4.x funkce také umožňuje vybrat soubor protokolu v vyhledávací (macOS) nebo v Průzkumníku (Windows) přímo z hlavní nabídky:

- **Soubor protokolu zobrazení nápovědy →**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>Pro sešity 1.3 a starší protokolu cesty:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>Informace o verzi platformy

Je velmi užitečné zjistit podrobnosti o operačním systému a nainstalovaných produktů Xamarin.

Z hlavní nabídky v sešitech:

* **Pomoct informace o verzi kopie →**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>Pokyny pro sešity 1.3 a starší:

Visual Studio pro Mac

- **Visual Studio → o kopírování informace o sadě Visual Studio → zobrazení podrobností →**
- Vložte do sestavy chyb

Visual Studio

- **Nápověda o sadě Visual Studio → kopírovat informace →**
- Dejte nám vědět verze vašeho operačního systému a zda používáte 32bitové nebo 64bitové verze systému Windows.

### <a name="samples"></a>Ukázky kódu

Pokud se můžete připojit nebo odkaz `.workbooks` máte potíže s, soubor, který vám může pomoci vyřešit vaše chyb rychleji.

### <a name="devices"></a>Zařízení

Pokud jsou potíže s připojením zařízení s iOS nebo Android sešitu a jste již zaškrtnuté [naše řešení potíží stránky](~/tools/workbooks/troubleshooting/index.md), budeme muset vědět:

- Název zařízení, které se pokoušíte připojit k
- Verze operačního systému vašeho zařízení.
- Android: Ověřte, že používáte x86 emulátoru
- Android: Platformě emulátoru používáte? Google Emulator?
  Visual Studio emulátoru Android? Přehrávač Xamarin Android?
- iOS v systému Windows: Jaká verze simulátoru iOS Xamarin vzdálené instalaci (zkontrolujte `Add/Remove Programs` v `Control Panel`)?
- iOS v systému Windows: uveďte také informace o verzi platformy pro svého hostitele Mac sestavení
- Zařízení mít připojení k síti (kontrola prostřednictvím webového prohlížeče)?

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Odinstalovat

### <a name="windows"></a>Windows

V závislosti na tom, jak jste získali sešity & Kontrola možná budete muset provést dva postupy odinstalace. Zkontrolujte prosím obě tyto úplně odinstalujte software.

#### <a name="visual-studio-installer"></a>Instalační program sady Visual Studio

Pokud máte Visual Studio 2017, otevřete **instalační program Visual Studio**a vyhledejte v **jednotlivých součástí** pro **Xamarin sešity**. Pokud je zaškrtnuto, zrušte jeho zaškrtnutí a pak klikněte na **upravit** odinstalovat.

#### <a name="system-uninstall"></a>Odinstalace systému

Pokud jste nainstalovali sešity & Kontrola sami s stažený instalační program, ji budou muset odinstalovat přes **aplikace a funkce** stránka nastavení systému Windows 10 nebo prostřednictvím **přidat nebo odebrat programy**v Ovládacích panelech ve starších verzích systému Windows.

> **Spustit → nastavení → systému → aplikace a funkce**

![](install-images/windows-remove.png "Sešity Xamarin a Inspector, jak je uvedeno v &quot;aplikace &amp; funkce&quot;")

**Postupujte podle procesu pro instalační program Visual Studio k Ujistěte se, že sešity & není získat přeinstalovat nástroj Inspector bez vašeho vědomí.**

<a name="uninstall-macos" />

### <a name="macos"></a>macOS

Počínaje [1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/), Xamarin sešity & Kontrola můžou se odinstalovat z terminálu spuštěním:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Odinstalační program, který podrobně popisuje soubory a adresáře se odstraní a požádat o potvrzení, než budete pokračovat.

Předat `-help` argument `uninstall` skript pro pokročilejší scénáře.

Pro starší verze musíte ručně odebrat následující:

1. Odstranit aplikaci sešity v `"/Applications/Xamarin Workbooks.app"`
2. Odstranit aplikaci Inspector ve `"Applications/Xamarin Inspector.app"`
2. Odstranit doplňky: `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` a `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"`
3. Odstraňte Inspector a podpůrné soubory. zde: `/Library/Frameworks/Xamarin.Interactive.framework` a `/Library/Frameworks/Xamarin.Inspector.framework`

## <a name="downgrading"></a>Přechod na starší verzi

Identifikátor sady pro `/Applications/Xamarin Workbooks.app` se změnil z `com.xamarin.Inspector` k `com.xamarin.Workbooks` ve verzi 1.4 pro usnadnění budoucí rozdělení instalační programy Xamarin sešity & Kontrola.

Z důvodu chyby v starší instalační programy není možné přejít na starší verzi 1,4 nebo novější verze pomocí 1.3.2 nebo starší instalační programy.

Přejít na starší verzi z 1,4 nebo novějšího 1.3.2 nebo starší:

1. [Odinstalujte sešity & Kontrola ručně](#macOS)
2. Spustit 1.3.2 nebo starší `.pkg` instalační program