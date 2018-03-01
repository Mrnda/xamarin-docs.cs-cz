---
title: "Nastavení projektu, které jsou požadovány pro ladicí program?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 6097dc8dfdff8807137ef68be86a08e4c9e23988
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Nastavení projektu, které jsou požadovány pro ladicí program?

Aby ladicí program na fungovat podle očekávání (přístupů zarážky, protokoly zobrazení ladění atd.) zobrazení informací instrumentace a ladění vývojáře obě povoleny.

Postupujte podle těchto kroků zkontrolujte nastavení prostředí:

## <a name="visual-studio"></a>Visual Studio
1. Otevřete dialogové okno Možnosti projektu
2. Přejděte na **sestavení > Rozšířené...** Nastavte informace o ladění **úplné**
3. Nastavení pro každou platformu:
   - Přejděte na **Android možnosti > Možnosti ladění**. Značky **povolit instrumentace vývojáře** pole.
   - Přejděte na **iOS sestavení > Možnosti ladění**. Značky **povolit ladění** pole.

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. Otevřete dialogové okno Možnosti projektu
2. Přejděte na **sestavení > kompilátoru > Obecné možnosti**. Nastavte informace o ladění **úplné**
3. Nastavení pro každou platformu:
  - Přejděte na **sestavení > Android sestavení > Možnosti ladění**. Značky **povolit instrumentace vývojáře** pole.
  - Přejděte na **sestavení > iOS ladění**. Značky **povolit ladění** pole.

