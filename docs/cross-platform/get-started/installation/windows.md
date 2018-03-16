---
title: "Nainstalovat Xamarin v sadě Visual Studio v systému Windows"
ms.topic: article
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: b15c9b05a4e476353322c6d29e94267313460bfe
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/16/2018
---
# <a name="installing-xamarin-in-visual-studio-on-windows"></a>Nainstalovat Xamarin v sadě Visual Studio v systému Windows

Xamarin je zdarma a zahrnuta ve všech edicích sady Visual Studio.

<a name="requirements" />

## <a name="requirements"></a>Požadavky

Tady jsou požadované pro instalaci nástrojů Visual Studio tools pro Xamarin:

1. Windows 7 nebo vyšší.

2. Visual Studio 2017 (Community, Professional a Enterprise).

3. Xamarin for Visual Studio.

Všimněte si, že Xamarin nelze použít s edice Express sady Visual Studio z důvodu nedostatku podpory modulů plug-in.

Další informace o požadavcích pro instalaci a použití Xamarin najdete v tématu [požadavky na systém](~/cross-platform/get-started/requirements.md).


<a name="installation" />

## <a name="installation"></a>Instalace

Xamarin lze nainstalovat jako součást nové instalace sady Visual Studio.
Chcete-li dosáhnout, použijte následující kroky:

1. Stáhněte si Visual Studio Community, Visual Studio Professional nebo Visual Studio Enterprise z [Visual Studio](https://www.visualstudio.com/vs/) stránky (ke stažení jsou k dispozici odkazy v dolní části).

2. Poklikejte na stažený balíček, který chcete spustit instalaci.

3. Vyberte **pro vývoj mobilních řešení s .NET** zatížení na obrazovce instalace: 

    [![Mobilní vývoj s .NET výběrem na obrazovce úlohy](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png#lightbox)

4. Při **pro vývoj mobilních řešení s .NET** je vybrána, podívejte se na **Souhrn** panely na pravé straně. Zde zrušte výběr možnosti pro vývoj mobilních řešení, které nechcete instalovat. Ve výchozím nastavení, jsou nainstalované všechny možnosti uvedené v následující snímek obrazovky (**Xamarin sešity**, **Xamarin profileru**, **používat vzdáleně simulátoru Xamarin**,  **Android NDK**, **sady SDK pro Android**, **Java SE Development Kit**, **emulátor Google Android**, **F # podpora**, a **Intel HAXM**):

    ![Panel Přehled výpis Xamarin možností instalace](windows-images/02-summary.png)

5. Až budete připravení zahájit instalaci sady Visual Studio, klikněte **nainstalovat** tlačítko v pravém dolním:

    ![Umístění instalace tlačítka](windows-images/03-click-install.png)

   V závislosti na tom, která edice sady Visual Studio instalujete proces instalace může trvat dlouhou dobu pro dokončení. Indikátory průběhu můžete použít ke sledování instalace:

    ![Příklad snímek obrazovky Indikátory průběhu během instalace](windows-images/04-progress-bars.png)

6. Po dokončení instalace sady Visual Studio, klikněte na tlačítko **spusťte** tlačítko ke spuštění sady Visual Studio:

    ![Umístění tlačítko Spustit](windows-images/05-launch.png)


<a name="vs2017" />

### <a name="adding-xamarin-to-visual-studio-2017"></a>Přidání Xamarin pro Visual Studio 2017

Pokud Visual Studio 2017 je již nainstalován, můžete přidat Xamarin znovu opětovným spuštěním instalačního programu sady Visual Studio k úpravě úlohy (viz [upravit Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) podrobnosti). Potom postupujte podle kroků uvedených výše, chcete-li nainstalovat Xamarin.

Další informace o stažení a instalaci Visual Studio 2017 najdete v tématu [nainstalovat Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


### <a name="verifying-installation"></a>Ověření instalace

Visual Studio 2017, můžete ověřit, nainstalovaný Xamarin kliknutím **pomoci** nabídky. Pokud je nainstalovaná Xamarin, měli byste vidět **Xamarin** položky nabídky, jak je vidět na tomto snímku obrazovky:

![Položky nabídky Xamarin zobrazí v nabídce Nápověda](windows-images/12-xamarin-menu-item.png)

Pokud používáte starší verze sady Visual Studio, můžete kliknout na **pomoci > o sadě Microsoft Visual Studio** a procházet seznam nainstalované produkty, které chcete zobrazit, pokud je nainstalován Xamarin:

![Visual Studio nainstalované produkty, které obrazovky](windows-images/13-xamarin-is-installed.png)

Další informace o vyhledání informací o verzi najdete v tématu [kde najdu Moje informace o verzi a protokoly?](~/cross-platform/troubleshooting/questions/version-logs.md)

<a name="nextsteps" />

## <a name="next-steps"></a>Další kroky

Instalace Visual Studio Tools pro Xamarin umožňuje zahájit zápis kódu pro aplikace, ale nevyžaduje další nastavení pro vytváření a nasazování aplikací simulátoru, emulátoru a zařízení. Navštivte následující příručky k dokončení instalace a začít vytvářet aplikace pro kombinované platformy.

### <a name="ios"></a>iOS

Podrobnější informace najdete v článku [Xamarin.iOS instalace v systému Windows](~/ios/get-started/installation/windows/index.md) průvodce. 

1. [Instalace nástrojů pro Xamarin.iOS na počítači Mac](~/ios/get-started/installation/windows/index.md#installation)
2. [Konfigurace počítače Mac](~/ios/get-started/installation/windows/index.md#configuration)
3. [iOS Developer instalace](~/ios/get-started/installation/windows/index.md#developersetup) (ke spuštění aplikace na zařízení).
4. [Připojení k hostiteli Mac sestavení sady Visual Studio](~/ios/get-started/installation/windows/index.md#connectingtomac)
5. [Používat vzdáleně simulátoru iOS](~/tools/ios-simulator.md)
6. [Úvod k Xamarin.iOSu pro Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

### <a name="android"></a>Android

Podrobnější informace najdete v článku [Xamarin.Android instalace v systému Windows](~/android/get-started/installation/windows.md) průvodce.

1. [Konfigurace Xamarin.Android](~/android/get-started/installation/windows.md#configuration)
2. [Pomocí Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Emulátor sady Android SDK](~/android/get-started/installation/android-emulator/index.md)
4. [Nastavení zařízení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md)
