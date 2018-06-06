---
title: Konfigurace Visual Studio 2017
description: Tento článek popisuje postup konfigurace Visual Studio 2017 pro vývoj na platformě Xamarin.iOS. Konkrétně popisuje, jak nakonfigurovat nainstalovaná verze Xamarin.iOS, panelu nástrojů iOS a rozevírací nabídky platformy řešení.
ms.prod: xamarin
ms.assetid: 22D82244-890D-4325-B3CC-C0AC49130BCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: 70633877fb07f52ce4e7a399668be6268942b137
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785951"
---
# <a name="configuring-visual-studio-2017"></a>Konfigurace Visual Studio 2017

_Tento článek popisuje různé možnosti konfigurace Xamarin.iOS pro sadu Visual Studio._

## <a name="using-matching-xamarinios-versions"></a>Pomocí odpovídající verze Xamarin.iOS

Visual Studio 2017 musí používat stejnou verzi nástroje Xamarin.iOS, který je nainstalován na hostiteli sestavení Mac. Abyste měli jistotu, to platí:

 - Pokud používáte Visual Studio 2017, vyberte **stabilní** kanálu aktualizace v sadě Visual Studio for Mac.

 - Pokud používáte Visual Studio 2017 Preview, vyberte **Alpha** kanálu aktualizace v sadě Visual Studio for Mac.

> [!NOTE]
> Počínaje [Visual Studio 2017 verze 15,6 operací](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning), Visual Studio 2017 automaticky rozpozná, pokud hostitel sestavení Mac používá stejnou verzi nástroje Xamarin.iOS jako Windows. Pokud došlo k neshodě verzí, Visual Studio 2017 nabízí vzdáleně nainstalovat správnou verzi na hostiteli Mac sestavení. Další informace, podívejte se na [Mac automatické zřizování](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning) části [pár na Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) průvodce.

## <a name="ios-toolbar"></a>iOS panelu nástrojů

Když projektu iOS otevřít v aplikaci Visual Studio 2017, by měly jít vidět panelu nástrojů iOS.  Ve výchozím nastavení obsahuje čtyři tlačítka, které jsou užitečné pro vývoj na platformě Xamarin.iOS:

![Panel nástrojů Visual Studio 2017 iOS](config-options-images/ios-toolbar.png "nástrojů Visual Studio 2017 iOS")

- **Pár na Mac** – otevře pár do dialogového okna Mac. Povolit, když je otevřený v aplikaci Visual Studio 2017 projektu iOS.
- **Zobrazit simulátoru iOS** – na Mac sestavení hostitele, zobrazí se vpředu simulátoru iOS. Povolit, když je otevřený v aplikaci Visual Studio 2017 projektu iOS.
- **Zařízení protokolu** – otevře okno, které umožňuje kontrolovat protokolů zařízení. Povolit, když je otevřený v aplikaci Visual Studio 2017 projektu iOS.
- **Zobrazit soubor IPA na sestavení serveru** – otevře okno na hostiteli sestavení Mac zobrazující umístění souboru .ipa pro aplikaci. Povolit po dokončení sestavení, pro který byl vytvořen .ipa.

Pokud tento panel nástrojů nezobrazí, otevřete **zobrazení** nabídky v Visual Studio 2017 a zvolte **panely nástrojů > iOS**:

![Povolení nástrojů iOS](config-options-images/ios-toolbar-enable.png "povolení nástrojů iOS")

## <a name="solution-platforms-drop-down-menu"></a>Řešení platformy rozevírací nabídky

**Platformy řešení** rozevírací nabídky umožňuje zvolit, zda další buildu se zaměří na fyzické zařízení nebo simulátoru.

Abyste měli jistotu, se zobrazí na standardním panelu nástrojů této rozevírací nabídce:

- V 2017 Visual Studio klikněte na šipku dolů na pravé straně standardním panelu nástrojů.
- Zvolte **přidat nebo odebrat tlačítka** 
- Zajistěte, aby **platformy řešení** je zaškrtnuta možnost položky:

![Povolení rozevírací nabídky platformy řešení](config-options-images/solution-platforms-enable.png "povolení rozevírací nabídky platformy řešení")

S projektem iOS, která je otevřená **standardní** a **iOS** panely nástrojů by měl nyní vypadat následující snímek obrazovky:

![Panely nástrojů Standard a iOS](config-options-images/toolbars.png "Standard a iOS panely nástrojů")


