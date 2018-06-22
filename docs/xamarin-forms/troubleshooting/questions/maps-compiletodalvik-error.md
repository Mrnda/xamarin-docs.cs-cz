---
title: Proč můj projektu Xamarin.Forms.Maps Android selhat s COMPILETODALVIK NEOČEKÁVANÁ chyba nejvyšší úrovně?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 9df9e348440b9dd4b18b3859d64cbe47bd05b24c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30790662"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Proč můj projektu Xamarin.Forms.Maps Android selhat s COMPILETODALVIK NEOČEKÁVANÁ chyba nejvyšší úrovně?

Tato chyba může se zobrazí v panelu pro chybu sady Visual Studio pro Mac, nebo v okně výstupu sestavení sady Visual Studio; v systému Android projektů pomocí Xamarin.Forms.Maps.

Nejčastěji se vyřeší zvětšením velikosti haldy Java pro projekt Xamarin.Android. Postupujte podle těchto kroků a zvyšte velikost haldy:

## <a name="visual-studio"></a>Visual Studio

1. Klikněte pravým tlačítkem na projekt pro Android a otevřete dialogové okno Možnosti projektu.
2. Přejděte na **Android možnosti -> Upřesnit**
3. V jazyce Java haldy velikost textového pole zadejte 1G.
4. Znovu sestavte projekt.

![Snímek obrazovky možnosti projektu sady Visual Studio](maps-compiletodalvik-error-images/vsjavaheap.png "Android sestavení možnosti v sadě Visual Studio")

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1.  Klikněte pravým tlačítkem na projekt pro Android a otevřete dialogové okno Možnosti projektu.
2.  Přejděte na **sestavení -> Android sestavení -> Upřesnit**
3.  V jazyce Java haldy velikost textového pole zadejte 1G.
4.  Znovu sestavte projekt.  

![Snímek obrazovky sady Visual Studio pro Mac možnosti projektu](maps-compiletodalvik-error-images/xsjavaheap.png "Android možnosti v sadě Visual Studio pro Mac sestavení")

