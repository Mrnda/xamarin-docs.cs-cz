---
title: Nastavení projektu, které jsou požadovány pro ladicí program?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 646ef7f708be2de6a851ace25d69a7c2f0b18a83
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350804"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Nastavení projektu, které jsou požadovány pro ladicí program?

Aby ladicí program fungovat podle očekávání (průchodů zarážky, zobrazit protokoly ladění atd.) zobrazené informace pro vývojáře pro instrumentaci a ladění musí být povoleny.

Postupujte podle těchto kroků ke kontrole nastavení prostředí:

## <a name="visual-studio"></a>Visual Studio
1. Otevřete dialogové okno Možnosti projektu
2. Přejděte na **sestavení > Upřesnit...** Nastavení informací o ladění **úplné**
3. Nastavení pro každou platformu:
   - Přejděte na **Android možnosti > ladění možnosti**. Značek **povolit vývojářskou instrumentaci** pole.
   - Přejděte na **iOS Build > Možnosti ladění**. Značek **povolit ladění** pole.

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. Otevřete dialogové okno Možnosti projektu
2. Přejděte na **sestavení > kompilátoru > Obecné možnosti**. Nastavení informací o ladění **úplné**
3. Nastavení pro každou platformu:
  - Přejděte na **sestavení > sestavení pro Android > možnostech ladění**. Značek **povolit vývojářskou instrumentaci** pole.
  - Přejděte na **sestavení > iOS – ladění**. Značek **povolit ladění** pole.

