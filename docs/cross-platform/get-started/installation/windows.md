---
title: Instalaci Xamarinu ve Visual Studio 2017
description: Tento dokument popisuje, jak nainstalovat Xamarin ve Visual Studio 2017. Popisuje požadavky, proces instalace a ověření instalace.
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: 419addde14d5be99833b4611a4af2a1be8756b9d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781475"
---
# <a name="installing-xamarin-in-visual-studio-2017"></a>Instalaci Xamarinu ve Visual Studio 2017

<a name="requirements" />

## <a name="requirements"></a>Požadavky

Tady jsou požadované pro instalaci Xamarinu ve Visual Studio 2017:

1. Windows 7 nebo vyšší.

2. Visual Studio 2017 (Community, Professional a Enterprise).

3. Xamarin pro Visual Studio.

Další informace o požadavcích pro instalaci a použití Xamarin najdete v tématu [požadavky na systém](~/cross-platform/get-started/requirements.md).

<a name="installation" />

## <a name="installation"></a>Instalace

Xamarin lze nainstalovat jako součást nové instalace Visual Studio 2017.
Chcete-li dosáhnout, použijte následující kroky:

1. Stáhněte si Visual Studio 2017 Community, Visual Studio Professional nebo Visual Studio Enterprise z [Visual Studio](https://www.visualstudio.com/vs/) stránky (ke stažení jsou k dispozici odkazy v dolní části).

2. Poklikejte na stažený balíček, který chcete spustit instalaci.

3. Vyberte **pro vývoj mobilních řešení s .NET** zatížení na obrazovce instalace: 

    [![Mobilní vývoj s .NET výběrem na obrazovce úlohy](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png#lightbox)

4. Při **pro vývoj mobilních řešení s .NET** je vybrána, podívejte se na **Souhrn** panely na pravé straně. Zde zrušte výběr možnosti pro vývoj mobilních řešení, které nechcete instalovat. Ve výchozím nastavení, jsou nainstalované všechny možnosti uvedené v následující snímek obrazovky (**Xamarin sešity**, **Xamarin profileru**, **používat vzdáleně simulátoru Xamarin**,  **Android NDK**, **sady SDK pro Android**, **Java SE Development Kit**, **emulátor Google Android**, **F # podpora**, a **Intel HAXM**):

    ![Panel Přehled výpis Xamarin možností instalace](windows-images/02-summary.png)

5. Až budete připravení zahájit instalaci Visual Studio 2017, klikněte **nainstalovat** tlačítko v pravém dolním:

    ![Umístění instalace tlačítka](windows-images/03-click-install.png)

   V závislosti na tom, která edice Visual Studio 2017 instalujete proces instalace může trvat dlouhou dobu pro dokončení. Indikátory průběhu můžete použít ke sledování instalace:

    ![Příklad snímek obrazovky Indikátory průběhu během instalace](windows-images/04-progress-bars.png)

6. Po dokončení instalace Visual Studio 2017, klikněte na tlačítko **spusťte** tlačítko ke spuštění sady Visual Studio:

    ![Umístění tlačítko Spustit](windows-images/05-launch.png)

<a name="vs2017" />

### <a name="adding-xamarin-to-visual-studio-2017"></a>Přidání Xamarin pro Visual Studio 2017

Pokud Visual Studio 2017 je již nainstalován, můžete přidat Xamarin znovu opětovným spuštěním instalačního programu Visual Studio 2017 upravit úlohy (najdete v části [upravit Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) podrobnosti). Potom postupujte podle kroků uvedených výše, chcete-li nainstalovat Xamarin.

Další informace o stažení a instalaci Visual Studio 2017 najdete v tématu [nainstalovat Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


### <a name="verifying-installation"></a>Ověření instalace

Visual Studio 2017, můžete ověřit, nainstalovaný Xamarin kliknutím **pomoci** nabídky. Pokud je nainstalovaná Xamarin, měli byste vidět **Xamarin** položky nabídky, jak je vidět na tomto snímku obrazovky:

![Položky nabídky Xamarin zobrazí v nabídce Nápověda](windows-images/12-xamarin-menu-item.png)

Pokud používáte starší verze sady Visual Studio, můžete kliknout na **pomoci > o sadě Microsoft Visual Studio** a procházet seznam nainstalované produkty, které chcete zobrazit, pokud je nainstalován Xamarin:

![Visual Studio nainstalované produkty, které obrazovky](windows-images/13-xamarin-is-installed.png)

Další informace o vyhledání informací o verzi najdete v tématu [kde najdu Moje informace o verzi a protokoly?](~/cross-platform/troubleshooting/questions/version-logs.md)

<a name="nextsteps" />

## <a name="next-steps"></a>Další kroky

Instalaci Xamarinu ve Visual Studio 2017 umožňuje zahájit zápis kódu pro aplikace, ale nevyžaduje další nastavení pro vytváření a nasazování aplikací simulátoru, emulátoru a zařízení. Navštivte následující příručky k dokončení instalace a začít vytvářet aplikace pro kombinované platformy.

### <a name="ios"></a>iOS

Podrobnější informace najdete v článku [Xamarin.iOS instalace v systému Windows](~/ios/get-started/installation/windows/index.md) průvodce. 

1. [Instalaci sady Visual Studio pro Mac](https://docs.microsoft.com/visualstudio/mac/installation)
2. [Připojení k hostiteli Mac sestavení sady Visual Studio](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
3. [iOS Developer instalace](~/ios/get-started/installation/device-provisioning/index.md) – musí se spustit aplikaci na zařízení
5. [Používat vzdáleně simulátoru iOS](~/tools/ios-simulator.md)
6. [Úvod k Xamarin.iOSu pro Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

### <a name="android"></a>Android

Podrobnější informace najdete v článku [Xamarin.Android instalace v systému Windows](~/android/get-started/installation/windows.md) průvodce.

1. [Konfigurace Xamarin.Android](~/android/get-started/installation/windows.md#configuration)
2. [Pomocí Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Emulátor sady Android SDK](~/android/get-started/installation/android-emulator/index.md)
4. [Nastavení zařízení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md)
