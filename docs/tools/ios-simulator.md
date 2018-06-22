---
title: Remoted iOS Simulator pro Windows
description: Můžete používat vzdáleně iOS simulátor pro systém Windows k testování aplikace na simulátoru iOS zobrazí v systému Windows společně Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: topgenorth
ms.author: toopge
ms.date: 05/11/2018
ms.openlocfilehash: b07cc24e63f4aa3ce4451e3bdb5819f1df1058c6
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
ms.locfileid: "34149804"
---
# <a name="remoted-ios-simulator-for-windows"></a>Remoted iOS Simulator pro Windows

Můžete používat vzdáleně iOS simulátor pro systém Windows k testování aplikace na simulátoru iOS zobrazí v systému Windows společně Visual Studio 2017.

[![](ios-simulator-images/hero-sml.png "simulátoru iOS systémem Windows")](ios-simulator-images/hero.png#lightbox)

## <a name="getting-started"></a>Začínáme

Používat vzdáleně iOS simulátor pro systém Windows je automaticky nainstalován jako součást Xamarinu ve Visual Studio 2017. Pokud chcete použít, postupujte takto:

1. [Spárujte Visual 2017 na Mac sestavení hostitele](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. V aplikaci Visual Studio 2017 spusťte ladění projektu iOS nebo tvOS. Používat vzdáleně iOS simulátor pro systém Windows se zobrazí na počítač se systémem Windows.

## <a name="simulator-window"></a>Simulátor okna

Panelu nástrojů v horní části okna simulátoru obsahuje řadu užitečné tlačítka:

- **Domácí** – simuluje tlačítko Domů na zařízení s iOS
- **Zámek** – uzamkne simulátoru (prstem k odemknutí)
- **Snímek obrazovky** – snímek obrazovky simulátoru uloží
- [**Nastavení** ](#settings) – zobrazí klávesnice, umístění a další nastavení
- [**Další možnosti** ](#other-options) – otevře různé možnosti simulátoru, jako je například otočení a zatřesením gesta

    [![](ios-simulator-images/maps-app-sml.png "příklad mapuje simulátoru iOS")](ios-simulator-images/maps-app.png#lightbox)

## <a name="settings"></a>Nastavení

Kliknutím na ikonu panelu nástrojů ozubené kolečko otevře **nastavení** okno:

[![](ios-simulator-images/settings-sml.png "nastavení simulátoru iOS")](ios-simulator-images/settings.png#lightbox)

Tato nastavení umožňují povolit hardwaru klávesnice, vyberte umístění, která se má zařízení sestavy (statické a přesunutí umístění jsou podporované), povolit Touch ID a resetovat obsah a nastavení pro simulátoru.

## <a name="other-options"></a>Další možnosti

Tlačítko se třemi tečkami panelu nástrojů zobrazí se další možnosti, jako je například otočení, zatřesením gesta a restartování. Tyto stejné možnosti jako seznam lze zobrazit kliknutím pravým tlačítkem na libovolné místo v okně simulátoru:

[![](ios-simulator-images/more-sml.png "Další nastavení simulátoru iOS")](ios-simulator-images/more.png#lightbox)

## <a name="touchscreen-support"></a>Podpora dotykovou obrazovku

Většina moderních počítače se systémem Windows mají dotykovou obrazovku. Vzhledem k tomu, že používat vzdáleně iOS simulátor pro Windows podporuje interakce dotykového ovládání, můžete otestovat vaší aplikace pomocí stejné roztahováním, prstem a gesta více prstem touch, používané v iOS fyzického zařízení.

Podobně platí používat vzdáleně iOS simulátor pro systém Windows zpracovává vstup pera Windows jako vstup tužky Apple.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>Zakázání používat vzdáleně iOS simulátor pro Windows

Pokud chcete zakázat používat vzdáleně iOS simulátor pro systém Windows, přejděte na **nástroje > Možnosti > Xamarin > Nastavení iOS** a zrušte zaškrtnutí políčka **vzdáleného simulátoru Windows**.

[![](ios-simulator-images/options-sml.png "Chcete-li použít simulátoru")](ios-simulator-images/options.png#lightbox)

Tato možnost zakázáno, ladění otevře na připojené Mac simulátoru iOS sestavení hostitele.
