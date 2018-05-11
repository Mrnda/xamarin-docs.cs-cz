---
title: Chybí rozšíření Visual Studia po instalaci
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: asb3993
ms.author: amburns
ms.date: 03/20/2017
ms.openlocfilehash: e47cfc4de77a6310a81867eefb07c3c1e5cc7060
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="missing-visual-studio-extensions-after-installation"></a>Chybí rozšíření Visual Studia po instalaci

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>Chybová zpráva: Tento projekt není kompatibilní s aktuální edice sady Visual Studio

Ujistěte se, že je nainstalována kompatibilní verze sady Visual Studio:

-   Visual Studio 2017 (Community, Professional a Enterprise)
-   Visual Studio 2015 (Community, Professional a Enterprise)

Viz také [požadavky Windows](~/cross-platform/get-started/requirements.md#windows).

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>Možné oprava 1: Změňte instalace a ujistěte se, jestli že jsou nainstalované rozšíření Visual Studia

V některých situacích Xamarin instalační program může automaticky zrušte výběr možnosti instalace rozšíření Visual Studia. Pokud, která je příčinou problému, nainstalujte chybějící rozšíření sady Visual Studio pomocí instalačního programu **změnu** příkaz. Chcete-li například nainstalovat rozšíření pro Visual Studio 2013:

1. Otevřete Windows **programy a funkce** ovládací panely.

2. Klikněte pravým tlačítkem **Xamarin** položku a vyberte **změnu**.

3. Klikněte na tlačítko **Další**, pak **změnu**.

4. Zajistěte, aby **Xamarin pro Visual Studio 2013** je možnost nastavena k instalaci:

    ![](missing-vs-extensions-images/installer.png "Povolit Xamarin pro možnost instalace Visual Studio 2013")

5. Pokračujte zbytek průvodce instalační služby.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>Možné oprava 2: Požádejte znovu nastavit rozšíření sady Visual Studio

1. Zkontrolujte, pokud rozšíření Xamarin byly zkopírovány do složky rozšíření sady Visual Studio:

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    Pokud rozšíření jsou správně nainstalovaný (verze 3.1.228), bude 60 položek ve složce:


    ![](missing-vs-extensions-images/folder.png "Seznam 'Xamarin\3.1.228.0' obsah složky v Průzkumníku")

2. Po potvrzení, že tato složka správná, říct sadě Visual Studio a zkuste to nastavení rozšíření znovu:

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>Možné oprava 3: Zkuste čerstvou přeinstalaci Xamarin

1.  Z ovládacích panelů Windows odinstalujte některé z následujících umístění, které jsou k dispozici:

    *   Xamarin

    *   Xamarin pro Windows

    *   Xamarin.Android

    *   Xamarin.iOS

    *   Xamarin pro Visual Studio

2.  V Průzkumníku odstranit všechny zbývající soubory ze složky rozšíření Xamarin Visual Studio (všechny verze, včetně obě **Program Files** a **Program Files (x86)**):

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3.  Také zkontrolujte v adresáři "VirtualStore", pokud může být žádné "překrytí" kopie každého rozšíření adresáře:

    `%LOCALAPPDATA%\VirtualStore`

4.  Otevřete editor registru (regedit).

5.  Podívejte se na tento klíč:

    _HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6.  Najít a odstranit všechny položky, které odpovídají tento vzor:

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7.  Podívejte se na tento klíč:

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8.  Odstraňte všechny položky, které vypadají stejně, jako mohou být s Xamarin. Například zde uvádíme jeden, který umožňuje způsobit problémy ve starších verzích Xamarin:

    _Mono.VisualStudio.Shell,1.0_

9.  Restartování počítače.

10.  Znovu nainstalujte aktuální stabilní verzi Xamarin z [visualstudio.com](https://visualstudio.com/xamarin).

## <a name="possible-fix-4-repair-visual-studio-installation"></a>Možné oprava 4: Instalace Visual Studia opravit

1.  Otevřete Windows **programy a funkce** ovládací panely.

2.  Klikněte pravým tlačítkem na příslušnou položku sady Microsoft Visual Studio a vyberte **změn**

3.  Klikněte **Repair** tlačítka v dialogovém okně Visual Studio, které se otevře.
