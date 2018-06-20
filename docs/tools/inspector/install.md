---
title: Kontrola instalace a požadavky
description: Tento dokument popisuje, jak nainstalovat nástroj Xamarin Inspector a popisuje podporovaný operační systém, integrovaného vývojového prostředí a aplikace platformy.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: f7c5217a9c2d3881ca29094c3186e448975db6a3
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268966"
---
# <a name="inspector-installation-and-requirements"></a>Kontrola instalace a požadavky

## <a name="download-and-installation"></a>Stažení a instalaci

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Stáhněte a nainstalujte [Visual Studio Enterprise](https://www.visualstudio.com/vs/) a vyberte **pro vývoj mobilních řešení s .NET** zatížení.
1. [Přihlaste se](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio) povolit předplatné Enterprise.
1. [Zkontrolujte](~/tools/inspector/inspect.md) vlastní aplikace!

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Stáhněte a nainstalujte [Visual Studio pro Mac](https://www.visualstudio.com/vs/mac/).
1. [Přihlaste se](https://docs.microsoft.com/visualstudio/mac/activation) povolit předplatné Enterprise.
1. [Zkontrolujte](~/tools/inspector/inspect.md) vlastní aplikace!

-----

## <a name="requirements"></a>Požadavky

### <a name="supported-operating-systems"></a>Podporované operační systémy

- **Mac** -OS X 10.11 nebo vyšší
- **Windows** – Windows 7 nebo novější (Internet Explorer 11 nebo větší a .NET 4.6.1 nebo vyšší)

### <a name="supported-ides"></a>Podporované integrovaného vývojového prostředí

- Visual Studio for Mac
- Visual Studio 2017 s **pro vývoj mobilních řešení s .NET** pracovního vytížení

Kontroly za provozu aplikace je k dispozici pro podnikové zákazníky.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Platformy podporované aplikace

|Aplikace platformy|Podpora rozhraní IDE|Poznámky|
|--- |--- |--- |
|Mac|Podporuje jenom v sadě Visual Studio pro Mac|
|iOS|Podporováno v aplikaci Visual Studio 2017 a Visual Studio pro Mac| |
|Android|Podporováno v aplikaci Visual Studio 2017 a Visual Studio pro Mac|Android musí být > = 4.0.3, s **fastdev** povolena.<br />Musíte použít emulátorů Google, Visual Studio a Xamarin Android. Android 7 emulátorů nemusí umožňovat kontroly v tuto chvíli.|
|WPF|Podporováno pouze v Visual Studio 2017|

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Zasílání zpráv o chybách

Chyby by měly být uvedeny přímo pomocí sady Visual Studio:

- **Nápověda > váš názor > nahlásit problém**

Uveďte všechny následující informace:

### <a name="platform-version-information"></a>Informace o verzi platformy

Tyto informace je důležité.

Visual Studio pro Mac

- **Visual Studio > o sadě Visual Studio > Zobrazit podrobnosti > zkopírujte informace**
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

- **Nápověda > odhalit souboru protokolu**

Visual Studio pro Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Obsah sady Visual Studio **výstup** podokně je také možné informativní.

### <a name="project-settings"></a>Nastavení projektu

Pokud se můžete připojit **.csproj** pro projekt, který chcete zkontrolovat, je velmi užitečné. Toto je jednodušší než žádostí o jednotlivých nastavení.

Také potvrďte, že jste v konfiguraci ladění.

### <a name="selected-devices"></a>Vybraná zařízení

Pro Android a iOS je důležité, abychom věděli, jaké zařízení jsou na ladění, pokud chcete zkontrolovat. Potřebujeme vědět:

- Název zařízení, jak je znázorněno v vaší IDE
- Verze operačního systému vašeho zařízení.
- Android: Ověřte, že používáte x86 emulátoru
- Android: Platformě emulátoru používáte? Google Emulátor? Visual Studio emulátoru Android? Přehrávač Xamarin Android?
- Neobsahuje aplikaci, kterou ladíte správně zobrazí a funkce v zařízení?
- Zařízení mít připojení k síti (kontrola prostřednictvím webového prohlížeče)?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
