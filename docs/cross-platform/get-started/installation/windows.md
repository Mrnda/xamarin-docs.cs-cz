---
title: "Nainstalovat Xamarin v sadě Visual Studio v systému Windows"
ms.topic: article
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: b68e03251b83192bdc5836af6ea54446ddaad24a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="installing-xamarin-in-visual-studio-on-windows"></a>Nainstalovat Xamarin v sadě Visual Studio v systému Windows

Vzhledem k tomu, že je nyní součástí Xamarin všechny edice sady Visual Studio bez jakýchkoli nákladů a nevyžaduje samostatné licence, instalační program sady Visual Studio můžete použít ke stažení a instalaci nástrojů pro Xamarin.

-   [Požadavky](#requirements)
-   [Instalace](#installation)
-   [Přidání Xamarin pro Visual Studio 2017](#vs2017)
-   [Přidání Xamarin pro Visual Studio 2015](#vs2015)
-   [Ověření instalace](#verifying)
-   [Další kroky](#nextsteps)


<a name="requirements" />

# <a name="requirements"></a>Požadavky

Tady jsou požadované pro instalaci nástrojů Visual Studio tools pro Xamarin:

1. Windows 7 nebo vyšší.

2. Visual Studio 2015 nebo 2017 (Community, Professional a Enterprise).

3. Xamarin for Visual Studio.

Všimněte si, že Xamarin nelze použít s edice Express sady Visual Studio z důvodu nedostatku podpory modulů plug-in.

Další informace o požadavcích pro instalaci a použití Xamarin najdete v tématu [požadavky na systém](~/cross-platform/get-started/requirements.md).


<a name="installation" />

# <a name="installation"></a>Instalace

Xamarin lze nainstalovat jako součást nové instalace sady Visual Studio.
Chcete-li dosáhnout, použijte následující kroky:

1. Stáhněte si Visual Studio Community, Visual Studio Professional nebo Visual Studio Enterprise z [Visual Studio](https://www.visualstudio.com/vs/) stránky (ke stažení jsou k dispozici odkazy v dolní části).

2. Poklikejte na stažený balíček, který chcete spustit instalaci.

3. Vyberte **pro vývoj mobilních řešení s .NET** zatížení na obrazovce instalace: 

    [![Mobilní vývoj s .NET výběrem na obrazovce úlohy](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png)

4. Při **pro vývoj mobilních řešení s .NET** je vybrána, podívejte se na **Souhrn** panely na pravé straně. Zde zrušte výběr možnosti pro vývoj mobilních řešení, které nechcete instalovat. Ve výchozím nastavení, jsou nainstalované všechny možnosti uvedené v následující snímek obrazovky (**Xamarin sešity**, **Xamarin profileru**, **používat vzdáleně simulátoru Xamarin**,  **Android NDK**, **sady SDK pro Android**, **Java SE Development Kit**, **emulátor Google Android**, **F # podpora**, a **Intel HAXM**):

    ![Panel Přehled výpis Xamarin možností instalace](windows-images/02-summary.png)

5. Až budete připravení zahájit instalaci sady Visual Studio, klikněte **nainstalovat** tlačítko v pravém dolním:

    ![Umístění instalace tlačítka](windows-images/03-click-install.png)

   V závislosti na tom, která edice sady Visual Studio instalujete proces instalace může trvat dlouhou dobu pro dokončení. Indikátory průběhu můžete použít ke sledování instalace:

    ![Příklad snímek obrazovky Indikátory průběhu během instalace](windows-images/04-progress-bars.png)

6. Po dokončení instalace sady Visual Studio, klikněte na tlačítko **spusťte** tlačítko ke spuštění sady Visual Studio:

    ![Umístění tlačítko Spustit](windows-images/05-launch.png)


<a name="vs2017" />

## <a name="adding-xamarin-to-visual-studio-2017"></a>Přidání Xamarin pro Visual Studio 2017

Pokud Visual Studio 2017 je již nainstalován, můžete přidat Xamarin znovu opětovným spuštěním instalačního programu sady Visual Studio k úpravě úlohy (viz [upravit Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) podrobnosti). Potom postupujte podle kroků uvedených výše, chcete-li nainstalovat Xamarin.

Další informace o stažení a instalaci Visual Studio 2017 najdete v tématu [nainstalovat Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


<a name="vs2015" />

## <a name="adding-xamarin-to-visual-studio-2015"></a>Přidání Xamarin pro Visual Studio 2015

Chcete-li Xamarin.Android přidat do existující instalace sady Visual Studio 2015, použijte následující kroky:

1. Klikněte pravým tlačítkem na Windows **spustit** tlačítko a vyberte **programy a funkce**.

2. Klikněte pravým tlačítkem na **Microsoft Visual Studio** a klikněte na tlačítko **změnu**.

3. Když se zobrazí dialogové okno Instalační program Visual Studio, klikněte **upravit** tlačítko.

4. V **funkce** kartě, přejděte dolů k položce **vývoj pro různé platformy Mobile**. Klikněte na zaškrtávací políčko vedle **C# nebo rozhraní .NET (Xamarin)**:

    ![Přidání C# nebo Xamarin .NET pro Visual Studio 2015](windows-images/06-add-xamarin.png)

5. Klikněte **aktualizace** tlačítko Přidat Xamarin pro Visual Studio.


<a name="verifying" />

## <a name="verifying-installation"></a>Ověření instalace

Visual Studio 2017, můžete ověřit, nainstalovaný Xamarin kliknutím **pomoci** nabídky. Pokud je nainstalovaná Xamarin, měli byste vidět **Xamarin** položky nabídky, jak je vidět na tomto snímku obrazovky:

![Položky nabídky Xamarin zobrazí v nabídce Nápověda](windows-images/12-xamarin-menu-item.png)

Pokud používáte starší verze sady Visual Studio, můžete kliknout na **pomoci > o sadě Microsoft Visual Studio** a procházet seznam nainstalované produkty, které chcete zobrazit, pokud je nainstalován Xamarin:

![Visual Studio nainstalované produkty, které obrazovky](windows-images/13-xamarin-is-installed.png)

Další informace o vyhledání informací o verzi najdete v tématu [kde najdu Moje informace o verzi a protokoly?](~/cross-platform/troubleshooting/questions/version-logs.md)

<a name="nextsteps" />

# <a name="next-steps"></a>Další kroky

Instalace Visual Studio Tools pro Xamarin umožňuje zahájit zápis kódu pro aplikace, ale nevyžaduje další nastavení pro vytváření a nasazování aplikací simulátoru, emulátoru a zařízení. Navštivte následující příručky k dokončení instalace a začít vytvářet aplikace pro kombinované platformy.

## <a name="ios"></a>iOS

Podrobnější informace najdete v článku [Xamarin.iOS instalace v systému Windows](~/ios/get-started/installation/windows/index.md) průvodce. 

1. [Instalace nástrojů pro Xamarin.iOS na počítači Mac](~/ios/get-started/installation/windows/index.md#installation)
2. [Konfigurace počítače Mac](~/ios/get-started/installation/windows/index.md#configuration)
3. [iOS Developer instalace](~/ios/get-started/installation/windows/index.md#developersetup) (ke spuštění aplikace na zařízení).
4. [Připojení k hostiteli Mac sestavení sady Visual Studio](~/ios/get-started/installation/windows/index.md#connectingtomac)
5. [Používat vzdáleně simulátoru iOS](~/tools/ios-simulator.md)
6. [Úvod do Xamarin.iOS pro sadu Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

## <a name="android"></a>Android

Podrobnější informace najdete v článku [Xamarin.Android instalace v systému Windows](~/android/get-started/installation/windows.md) průvodce.

1. [Konfigurace Xamarin.Android](~/android/get-started/installation/windows.md#configuration)
2. [Pomocí Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Emulátor sady SDK pro Android](~/android/get-started/installation/android-emulator/index.md)
4. [Nastavit zařízení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md)