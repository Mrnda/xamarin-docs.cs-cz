---
title: "Instalace a požadavky"
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: a587935e35882ed1dc68817fbbe1ae3e91200f29
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="installation-and-requirements"></a>Instalace a požadavky

<script> var inspectorOnLoad = (funkce) {var primaryTextBase = "Xamarin sešity & Inspector pro" var secondaryTextBase = "nebo stáhnout pro"; var inspectorDownloadUrlMac = "https://dl.xamarin.com/interactive/XamarinInteractive.pkg"; var inspectorDownloadUrlWin = "https://dl.xamarin.com/interactive/XamarinInteractive.msi";

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = aPrimary; var aWin = aSecondary; var macTextBase = primaryTextBase; var winTextBase = secondaryTextBase;

  Pokud (/win/i.test(navigator.platform.toLowerCase())) {aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase;}

  aMac.href = inspectorDownloadUrlMac; aMac.text = macTextBase + " Mac"; aWin.href = inspectorDownloadUrlWin; aWin.text = winTextBase + " Windows"; };

document.addEventListener ("DOMContentLoaded", inspectorOnLoad);
</script>

## <a name="download-and-installation"></a>Stažení a instalaci

<ol>
  <li>Stáhněte a nainstalujte <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Xamarin sešity & Kontrola pro Mac</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">nebo ke stažení pro Windows</a>).
  </li>
  <li><a href="~/tools/inspector/inspect.md"> Zkontrolujte vlastní aplikace!</a>
    </li>
</ol>

## <a name="requirements"></a>Požadavky

### <a name="supported-operating-systems"></a>Podporované operační systémy

- **Mac** -OS X 10.11 nebo vyšší
- **Windows** – Windows 7 nebo novější (Internet Explorer 11 nebo větší a .NET 4.6.1 nebo vyšší)

### <a name="supported-ides"></a>Podporované integrovaného vývojového prostředí

- Xamarin Studio 6.2 nebo větší
- Visual Studio pro Mac Preview 4 nebo vyšší
- Visual Studio 2015 s Xamarinem 4.3.x nebo vyšší
- Visual Studio 2017 zatížení Xamarin

Kontroly za provozu aplikace je k dispozici pro podnikové zákazníky.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Platformy podporované aplikace

<table>
<thead>
  <tr>
    <th>Aplikace platformy</th>
    <th>Podpora rozhraní IDE</th>
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
    <td>Podporované v XS a Visual Studio</td>
    <td>Kontroly aplikací pro iOS ze systému Windows vyžaduje stejnou verzi nástroje Inspector taky nainstalovat na hostiteli Mac sestavení.</td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Podporované v XS a Visual Studio</td>
    <td>
      <ul>
        <li>Android musí být > = 4.0.3</li>
        <li>Musí mít fastdev povolen</li>
        <li>Musíte použít emulátorů Google, Visual Studio a Xamarin Android. Android 7 emulátorů nemusí umožňovat kontroly v tuto chvíli.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Podporuje jenom v sadě Visual Studio v systému Windows</td>
    <td/>
  </tr>
</tbody>
</table>

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Zasílání zpráv o chybách

Chyby by měly být uvedeny přímo pomocí sady Visual Studio:

- **Pomůže → odeslat zpětnou vazbu → hlášení problému**

Uveďte všechny následující informace:

### <a name="platform-version-information"></a>Informace o verzi platformy

Tyto informace je důležité.

Visual Studio pro Mac

- **Visual Studio → o kopírování informace o sadě Visual Studio → zobrazení podrobností →**
- Vložte do sestavy chyb

Xamarin Studio

- **Xamarin Studio → o Xamarin Studio → zobrazit podrobnosti kopie →**
- Vložte do sestavy chyb

Visual Studio

- **Nápověda > o sadě Visual Studio > zkopírujte údaje**
- Dejte nám vědět verze vašeho operačního systému a zda používáte 32bitové nebo 64bitové verze systému Windows.

### <a name="log-files"></a>Soubory protokolu

Vždy připojte IDE a Inspector protokolových souborů klienta.

Kontrola klienta

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4.x funkce také umožňuje vybrat soubor protokolu v vyhledávací (macOS) nebo v Průzkumníku (Windows) přímo z hlavní nabídky:

- **Soubor protokolu zobrazení nápovědy →**

Visual Studio pro Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Obsah sady Visual Studio `Output` podokně je také možné informativní.

### <a name="project-settings"></a>Nastavení projektu

Pokud se můžete připojit `.csproj` pro projekt, který chcete zkontrolovat, je velmi užitečné. Toto je jednodušší než žádostí o jednotlivých nastavení.

Také potvrďte, že jste v konfiguraci ladění.

### <a name="selected-devices"></a>Vybraná zařízení

Pro Android a iOS je důležité, abychom věděli, jaké zařízení jsou na ladění, pokud chcete zkontrolovat. Potřebujeme vědět:

- Název zařízení, jak je znázorněno v vaší IDE
- Verze operačního systému vašeho zařízení.
- Android: Ověřte, že používáte x86 emulátoru
- Android: Platformě emulátoru používáte? Google Emulator? Visual Studio emulátoru Android? Přehrávač Xamarin Android?
- Neobsahuje aplikaci, kterou ladíte správně zobrazí a funkce v zařízení?
- Zařízení mít připojení k síti (kontrola prostřednictvím webového prohlížeče)?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Odinstalovat

### <a name="windows"></a>Windows

V závislosti na tom, jak jste získali sešity & Kontrola možná budete muset provést dva postupy odinstalace. Zkontrolujte prosím obě tyto úplně odinstalujte software.

#### <a name="visual-studio-installer"></a>Instalační program sady Visual Studio

Pokud máte Visual Studio 2017, otevřete "Instalační program Visual Studio" a vyhledejte "Xamarin sešity" v "Jednotlivých součástí". Pokud je zaškrtnuto, zrušte jeho zaškrtnutí a poté klikněte na "Upravit" odinstalovat.

#### <a name="system-uninstall"></a>Odinstalace systému

Pokud jste nainstalovali sešity & Kontrola sami s stažený instalační program, ji budou muset odinstalovat přes **aplikace a funkce** stránka nastavení systému Windows 10 nebo prostřednictvím **přidat nebo odebrat programy**v Ovládacích panelech ve starších verzích systému Windows.

> **Spustit → nastavení → systému → aplikace a funkce**

![](install-images/windows-remove.png "Sešity Xamarin a Inspector, jak je uvedeno v, aplikace a funkce.")

**Postupujte podle procesu pro instalační program Visual Studio k Ujistěte se, že sešity & není získat přeinstalovat nástroj Inspector bez vašeho vědomí.**

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

