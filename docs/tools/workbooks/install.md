---
title: Sešity instalace a požadavky
description: Jak chcete stáhnout, nainstalovat a používat sešity Xamarin.
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 4d3217140605be8567d70e6dcf63d60a02e6b2fb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="workbooks-installation-and-requirements"></a>Sešity instalace a požadavky

<a name="install" />

## <a name="download-and-install"></a>Stáhnout a nainstalovat

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Zkontrolujte [požadavky](#requirements) níže.
2. Stáhněte a nainstalujte [sešity Xamarin pro systém Windows](https://dl.xamarin.com/interactive/XamarinInteractive.msi).
3. Spustit [přehrávání](~/tools/workbooks/workbook.md) se sešity nebo vyzkoušet [ukázky](https://developer.xamarin.com/workbooks)

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Zkontrolujte [požadavky](#Requirements) níže.
2. Stáhněte a nainstalujte [sešity Xamarin pro Mac](https://dl.xamarin.com/interactive/XamarinInteractive.pkg).
3. Spustit [přehrávání](~/tools/workbooks/workbook.md) se sešity nebo vyzkoušet [ukázky](https://developer.xamarin.com/workbooks)

-----

## <a name="requirements"></a>Požadavky

#### <a name="supported-operating-systems"></a>Podporované operační systémy

- **Mac** -OS X 10.11 nebo vyšší
- **Windows** – Windows 7 nebo novější (Internet Explorer 11 nebo větší a .NET 4.6.1 nebo vyšší)

#### <a name="supported-app-platforms"></a>Platformy podporované aplikace

|Aplikace platformy|Podpora operačního systému|Poznámky|
|--- |--- |--- |
|Mac (Unified)|Podporováno pouze v systému Mac|
|iOS (Unified)|Podporované na Mac a Windows|Xamarin.iOS 11.0 a Xcode 9.0 nebo vyšší musí být nainstalován na macu. Sešity iOS systémem Windows vyžaduje Mac sestavení hostitele se systémem všechny výše uvedené a [používat vzdáleně simulátoru iOS](~/tools/ios-simulator.md) nainstalovaná v systému Windows.|
|Android|Podporované na Mac a Windows|Google, Visual Studio nebo Xamarin Android emulátoru, musíte použít s virtuálního zařízení > = 5.0|
|WPF|Podporováno pouze v systému Windows|
|Konzole (rozhraní .NET Framework)|Podporované na Mac a Windows|
|Konzole (.NET Core)|Podporované na Mac a Windows|


## <a name="reporting-bugs"></a>Zasílání zpráv o chybách

Prosím [hlášení problémů na Githubu][bugs]a zahrnovat všechny následující informace:

### <a name="log-files"></a>Soubory protokolu

Vždy připojte soubory protokolů klienta sešity:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

1.4.x funkce také umožňuje vybrat soubor protokolu v vyhledávací (macOS) nebo v Průzkumníku (Windows) přímo z hlavní nabídky:

- **Nápověda > odhalit souboru protokolu**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>Pro sešity 1.3 a starší protokolu cesty:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>Informace o verzi platformy

Je velmi užitečné zjistit podrobnosti o operačním systému a nainstalovaných produktů Xamarin.

Z hlavní nabídky v sešitech:

* **Nápověda > zkopírujte informace o verzi**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>Pokyny pro sešity 1.3 a starší:

Visual Studio pro Mac

- **Visual Studio > o sadě Visual Studio > Zobrazit podrobnosti > zkopírujte informace**
- Vložte do sestavy chyb

Visual Studio

- **Nápověda > o sadě Visual Studio > zkopírujte údaje**
- Dejte nám vědět verze vašeho operačního systému a zda používáte 32bitové nebo 64bitové verze systému Windows.

### <a name="samples"></a>Ukázky kódu

Pokud se můžete připojit nebo odkaz **.workbooks** máte potíže s, soubor, který vám může pomoci vyřešit vaše chyb rychleji.

### <a name="devices"></a>Zařízení

Pokud jsou potíže s připojením zařízení s iOS nebo Android sešitu a jste již zaškrtnuté [naše řešení potíží stránky](~/tools/workbooks/troubleshooting/index.md), budeme muset vědět:

- Název zařízení, které se pokoušíte připojit k
- Verze operačního systému vašeho zařízení.
- Android: Ověřte, že používáte x86 emulátoru
- Android: Platformě emulátoru používáte? Google Emulátor?
  Visual Studio emulátoru Android? Přehrávač Xamarin Android?
- iOS v systému Windows: Jaká verze simulátoru iOS Xamarin vzdálené instalaci (zkontrolujte **přidat nebo odebrat programy** v **ovládací panely**)?
- iOS v systému Windows: uveďte také informace o verzi platformy pro svého hostitele Mac sestavení
- Zařízení mít připojení k síti (kontrola prostřednictvím webového prohlížeče)?

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Odinstalovat

### <a name="windows"></a>Windows

V závislosti na tom, jak jste získali sešity & Kontrola možná budete muset provést dva postupy odinstalace. Zkontrolujte prosím obě tyto úplně odinstalujte software.

#### <a name="visual-studio-installer"></a>Instalační program pro Visual Studio

Pokud máte Visual Studio 2017, otevřete **instalační program Visual Studio**a vyhledejte v **jednotlivých součástí** pro **Xamarin sešity**. Pokud je zaškrtnuto, zrušte jeho zaškrtnutí a pak klikněte na **upravit** odinstalovat.

#### <a name="system-uninstall"></a>Odinstalace systému

Pokud jste nainstalovali sešity & Kontrola sami s stažený instalační program, ji budou muset odinstalovat přes **aplikace a funkce** stránka nastavení systému Windows 10 nebo prostřednictvím **přidat nebo odebrat programy**v Ovládacích panelech ve starších verzích systému Windows.

> **Spustit > Nastavení > Systém > aplikace a funkce**

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

Identifikátor sady pro **aplikace nebo Xamarin Workbooks.app** se změnil z `com.xamarin.Inspector` k `com.xamarin.Workbooks` ve verzi 1.4 pro usnadnění budoucí rozdělení instalační programy Xamarin sešity & Kontrola.

Z důvodu chyby v starší instalační programy není možné přejít na starší verzi 1,4 nebo novější verze pomocí 1.3.2 nebo starší instalační programy.

Přejít na starší verzi z 1,4 nebo novějšího 1.3.2 nebo starší:

1. [Odinstalujte sešity & Kontrola ručně](#macOS)
2. Spustit 1.3.2 nebo starší `.pkg` instalační program
