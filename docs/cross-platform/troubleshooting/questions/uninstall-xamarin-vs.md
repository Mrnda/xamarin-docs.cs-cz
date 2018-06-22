---
title: Jak provádět důkladná odinstalovat pro Xamarin pro Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 99fde9330498ee62d3cf6b5910c2cbfae39cfdeb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
ms.locfileid: "33917665"
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Jak provádět důkladná odinstalovat pro Xamarin pro Visual Studio?


1.  Z ovládacích panelů Windows odinstalujte některé z následujících umístění, které jsou k dispozici:

    -   Xamarin
    -   Xamarin pro Windows
    -   Xamarin.Android
    -   Xamarin.iOS
    -   Xamarin pro Visual Studio

2.  V Průzkumníku odstranit všechny zbývající soubory ze složky rozšíření Xamarin Visual Studio (všechny verze, včetně obě _Program Files_ a _Program Files (x86)_):

    _C:\\soubory programu\*\\sady Microsoft Visual Studio 1\*.0\\Common7\\IDE\\rozšíření\\Xamarin_

3.  Odstraňte adresář mezipaměti součást Visual Studio MEF také:

    _%LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*.0\\ComponentModelCache_

    Tento krok sám o sobě je ve skutečnosti často dostatečná k řešení chyb, jako například:

    -   "Balíček 'XamarinShellPackage' nebyl správně načten"

    -   "Soubor projektu... nelze otevřít. Je chybějící podtypem projektu"

    -   "Odkaz na objekt není nastavený na instanci objektu.  v Xamarin.VisualStudio.IOS.XamarinIOSPackage.Initialize()"

    -   "Setsite – se nezdařilo pro balíček" (v sadě Visual Studio _ActivityLog.xml_)

    -   "LegacySitePackage se nezdařilo pro balíček" (v sadě Visual Studio _ActivityLog.xml_)

    (Viz také [vymazat mezipaměť MEF součást](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) rozšíření sady Visual Studio.  A v tématu [chyb 40781, komentář 19](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19) pro kontext o něco víc o nadřazeného problému v sadě Visual Studio, který může způsobit, že tyto chyby.)

4.  Také se změnami _VirtualStore_ překrytí adresáři, pokud Windows může mít uložené všechny soubory _rozšíření\\Xamarin_ nebo _ComponentModelCache_adresáře existuje:

    _%LOCALAPPDATA%\\VirtualStore_

5.  Otevřete editor registru (`regedit`).

6.  Vyhledejte následující klíč:

    _HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7.  Najít a odstranit všechny položky, které odpovídají tento vzor:

    _C:\\soubory programu\*\\sady Microsoft Visual Studio 1\*.0\\Common7\\IDE\\rozšíření\\Xamarin_

8.  Podívejte se na tento klíč:

    _Nastavení HKEY\_aktuální\_uživatele\\softwaru\\Microsoft\\Visual Studio\\1\*.0\\ExtensionManager\\PendingDeletions_

9.  Odstraňte všechny položky, které vypadají stejně, jako mohou být s Xamarin.  Například zde uvádíme jeden, který umožňuje způsobit problémy ve starších verzích Xamarin:

    _Mono.VisualStudio.Shell,1.0_

10. Otevřete správce `cmd.exe` příkazový řádek a spusťte `devenv /setup` a `devenv /updateconfiguration` příkazy pro každou nainstalovanou verzi sady Visual Studio.  Například pro Visual Studio 2015:

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. Restartování počítače.

12. Znovu nainstalujte aktuální stabilní verzi pomocí Xamarin [visualstudio.com](https://visualstudio.com/xamarin/).

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>Další kroky řešení potíží pro "balíček nebyl správně načten"

V případech, kdy výše uvedené kroky nebylo možné vyřešit chyby "balíček nebyl správně načten" Zde jsou několika další kroky opakujte.

1.  Vytvořte nový uživatelský účet systému Windows.

2.  Zkontrolujte, pokud rozšíření Visual Studia Xamarin načíst bez chyby pro nového uživatele.

3.  Pokud rozšíření správně načíst, pak problému je pravděpodobně způsobena některých uložené nastavení pro původního uživatele:

    -   **V Průzkumníku** – _LOCALAPPDATA %\\Microsoft\\Visual Studio\\1\*.0_
    -   **V editoru registru** – _HKEY\_aktuální\_uživatele\\softwaru\\Microsoft\\Visual Studio\\1\*.0_
    -   **V editoru registru** – _HKEY\_aktuální\_uživatele\\softwaru\\Microsoft\\Visual Studio\\1\*.0\_konfigurace_

4.  Pokud tyto uložené nastavení skutečně zdají být problém, můžete zkusit je zálohování a jejich odstranění.
