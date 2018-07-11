---
title: Aplikace Xamarin Live Player
description: Tento dokument popisuje Xamarin Live Player živé aplikace, která umožňuje zobrazit náhled změn kódu na zařízení. Popisuje nastavení, ukázky, protokoly, nastavení správy zařízení a další.
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: topgenorth
ms.author: toopge
ms.date: 05/14/2017
ms.openlocfilehash: 88f7f62650484007c221aa7baaa684f872e0a8e9
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38830410"
---
# <a name="xamarin-live-player-app"></a>Aplikace Xamarin Live Player

![Funkce ve verzi Preview](~/media/shared/preview.png)

Po instalaci aplikace na telefonu, postupujte [instalační pokyny](~/tools/live-player/install.md) pro připojení k vašemu počítači. Zkuste použít jeden z [ukázkové aplikace](~/tools/live-player/samples.md) k jeho zprovoznění.

Při spuštění aplikace Xamarin Live Player vypadá takto (v Iosu a Androidu v uvedeném pořadí):

![Snímek obrazovky aplikace iOS Live Player](player-images/app-iphone-sml.png) ![Živé snímek obrazovky aplikace Player Android](player-images/app-android-sml.png)

Když stisknete klávesu **pár do sady Visual Studio**, pomocí fotoaparátu/kamery naskenoval čárový kód ukazuje, na vašem počítači:

![Snímek obrazovky s Iosem skeneru čárových kódů](player-images/scan-iphone-sml.png) ![Snímek obrazovky s Androidem čárového](player-images/scan-android-sml.png)

Pokud je připojení úspěšné, má kód téměř okamžitě spustit na zařízení (například [Kalkulačka ukázka](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator)):

![Ukázková aplikace Kalkulačka spuštěná v zařízení](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>Možnosti

Klikněte na tlačítko informace **(i)** v dolní části aplikace zobrazíte **možnosti** nabídky:

[![Snímek obrazovky možností nabídky](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>Protokoly

Zobrazit protokoly pro diagnostiku problémů.

### <a name="settings"></a>Nastavení

- Přepnout zobrazení chyb kompilace a modulu runtime.
- Informace o verzi.
- Odešlete zpětnou vazbu.

[![Snímek obrazovky nastavení](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>Správa zařízení

Připojit zařízení poprvé, postupujte podle pokynů v [nastavení & požadavky](~/tools/live-player/install.md). Můžete spárovat více zařízení (třeba iOS a Android) a správa prostřednictvím rozhraní IDE.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

V sadě Visual Studio, zvolte **nástroje > Xamarin Live Player > Správa zařízení...**

![Správa zařízení okna](player-images/manage-tools-menu-vs.png)

Toto okno umožňuje provádět následující:

- Pair – tím, že kontroluje kód nové zařízení
- Můžete také spárujte zařízení tak, že zadáte kód zobrazený na své obrazovce
- Odeberte stávající zařízení ze seznamu

Toto okno můžete také přistupovat ze seznamu zařízení.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

V sadě Visual Studio pro Mac, zvolte **nástroje > Správa zařízení (Xamarin Live Player)...**

![Správa zařízení okna](player-images/manage-tools-menu.png)

Toto okno umožňuje provádět následující:

- Pair – tím, že kontroluje kód nové zařízení
- Můžete také spárujte zařízení tak, že zadáte kód zobrazený na své obrazovce
- Odeberte stávající zařízení ze seznamu

![Správa zařízení okna](player-images/manage.png)

Toto okno se můžete dostat taky z seznamu zařízení:

![V seznamu zařízení vyberte zařízení s Xamarin Live Player](player-images/manage-device-menu.png)

-----

Pokud zaznamenáte jakékoli problémy viz [omezení a řešení potíží s](~/tools/live-player/troubleshooting.md).

## <a name="related-links"></a>Související odkazy

- [Omezení](~/tools/live-player/limitations.md)
- [Odstraňování potíží](~/tools/live-player/troubleshooting.md)
- [Ukázky Xamarin Live Player](samples.md)
