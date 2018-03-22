---
title: "Používat vzdáleně iOS simulátoru (pro Windows)"
description: "Test a ladění aplikací pro iOS zcela v sadě Visual Studio v systému Windows"
ms.topic: article
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/07/2017
ms.openlocfilehash: 6d1401728c1063ce09c5848865e4c9b3fe7687d7
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="remoted-ios-simulator-for-windows"></a>Používat vzdáleně iOS simulátoru (pro Windows)

_Test a ladění aplikací pro iOS zcela v sadě Visual Studio v systému Windows_

[![](ios-simulator-images/hero-sml.png "simulátoru iOS systémem Windows")](ios-simulator-images/hero.png#lightbox)

## <a name="download-and-install"></a>Stáhnout a nainstalovat

Stažení [instalační program](https://dl.xamarin.com/xamarin-simulator/Xamarin.Simulator.Installer.msi) a nainstalovat na počítače se systémem Windows. Nástroje sady Visual Studio pro Xamarin by měl být již nainstalován.

> [!NOTE]
> Použití vzdáleného simulátoru iOS v sadě Visual Studio vyžaduje síťově připojeného počítače Mac pomocí Xamarinu nainstalována.

## <a name="getting-started"></a>Začínáme

Použití simulátoru vzdálené iOS:

1. Ujistěte se, že Visual Studio se připojil k počítači Mac alespoň jednou před zahájením vzdálené simulátoru iOS.
2. Ujistěte se, je aplikace iOS nebo tvOS **spouštěný projekt** a spuštění ladění.

Můžete zakázat simulátoru iOS vzdálené z **nástroje > Možnosti > Xamarin > Nastavení iOS** zrušením zaškrtnutí políčka **vzdáleného simulátoru Windows** znázorněno zde:

[![](ios-simulator-images/options-sml.png "Chcete-li použít simulátoru")](ios-simulator-images/options.png#lightbox)

Připojeného počítače Mac bude poté otevřete simulátoru iOS. Když zaškrtnete tuto možnost simulátoru iOS vzdálené znovu zapnout.

## <a name="features"></a>Funkce

Vzdálené simulátoru iOS vám poskytuje způsob, jak testování a ladění aplikací pro iOS v simulátoru zcela ze sady Visual Studio v systému Windows.

### <a name="simulator-window"></a>Simulátor okna

Panelu nástrojů okna zahrnuje několik tlačítek pro interakci s simulátoru:

- **Domácí** – simuluje tlačítko Domů v zařízení.
- **Zámek** – uzamkne simulátoru (můžete prstem odemkněte).
- **Snímek obrazovky** – snímek obrazovky simulátoru uloží na disk.
- [**Nastavení** ](#settings) – konfigurace klávesnici a k umístění.
- Další [ **možnosti** ](#options) – celou řadu možností simulátoru jsou k dispozici jako je například otočení, zatřesením nebo vyvolání jiných stavů v simulátoru. Pokud některé možnosti jsou skryté, lze k nim ze tří teček ikonu, která se zobrazí na panelu nástrojů nebo kliknutím pravým tlačítkem na obrazovce.

    [![](ios-simulator-images/maps-app-sml.png "příklad mapuje simulátoru iOS")](ios-simulator-images/maps-app.png#lightbox)


### <a name="settings"></a>Nastavení

Ikona "zařízení" se otevře **nastavení** okno:

[![](ios-simulator-images/settings-sml.png "nastavení simulátoru iOS")](ios-simulator-images/settings.png#lightbox)

To umožňuje povolit klávesnice hardwaru v simulátoru a vyberte, jaké umístění je hlášených zařízení (včetně statické umístění nebo jiné možnosti přesunutí umístění).



### <a name="other-options"></a>Další možnosti

Klikněte pravým tlačítkem na libovolné místo v okně simulátoru zobrazíte všechny možnosti, které jsou k dispozici v simulátoru, jako je například otočení, spouštění gesto zatřesením a restartování simulátoru:

[![](ios-simulator-images/more-sml.png "Další nastavení simulátoru iOS")](ios-simulator-images/more.png#lightbox)

### <a name="touchscreen-support"></a>Podpora dotykovou obrazovku

Většina moderních počítače se systémem Windows mají dotykovou obrazovku a simulátoru iOS vzdálené umožňuje touch okno simulátoru testování interakce uživatele v aplikaci s iOS.

To zahrnuje roztáhnout, k načtení a více prstem touch gesta - věcí, které dříve pouze snadno testovat na fyzických zařízení.

Podpora pera v systému Windows je také převedeny na vstup Apple tužky v simulátoru.

<!--
<a name="knownissues" />

# Known Issues

 - Apple Watch devices may show in the Visual Studio device list, but are not yet supported.
 - Launching in **Release** mode may also start Apple’s simulator on the networked Mac.
 - Closing the remote iOS Simulator on Windows will not immediately stop debugging in Visual Studio. Stop debugging manually from the menu or the red button.
 - Opening too many different simulators simultaneously will produce unexpected results.
 - Exception of type `Foundation.NSErrorException` may be thrown while launching Simulators. Workaround is to kill csproxy (server process) on the Mac host and re-deploy to the simulator.
 - Performance may be slower when using Xcode 8
-->
