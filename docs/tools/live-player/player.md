---
title: Aplikace Xamarin za provozu Player
description: "Upravit a testování aplikací v reálném čase na systému iOS nebo zařízení se systémem Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/10/2017
ms.openlocfilehash: 6ae0439566387e22d939ede83e6b4cb5c1a5a8fe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-live-player-app"></a>Aplikace Xamarin za provozu Player

![Funkce ve verzi Preview](~/media/shared/preview.png)

## <a name="get-the-app"></a>Získat aplikaci

Přehrávač Xamarin za provozu je dostupná z iOS App Store a Google Play:

[ ![Stáhnout z iOS App Storu](player-images/app-store-badge.svg)](https://itunes.apple.com/us/app/xamarin-live-player/id1228841832?ls=1&mt=8) [ ![k dispozici na webu Google Play](player-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)



## <a name="using-the-app"></a>Pomocí aplikace

Po instalaci aplikace na váš telefon, postupujte podle [pokyny](~/tools/live-player/install.md) pro připojení k vašemu počítači. Použijte jeden z [ukázkové aplikace](~/tools/live-player/samples.md) k jeho zprovoznění.

Při spuštění aplikace Xamarin Live Player bude vypadat takto (na iOS a Android v uvedeném pořadí):

![Snímek obrazovky aplikace iOS Player za provozu](player-images/app-iphone-sml.png) ![Za provozu Player Android aplikace – snímek obrazovky](player-images/app-android-sml.png)

Po stisknutí klávesy **pár k sadě Visual Studio**, použít fotoaparát ke skenování čárového kódu zobrazuje ve vašem počítači:

![Snímek obrazovky čárových iOS](player-images/scan-iphone-sml.png) ![Snímek obrazovky Android čárových](player-images/scan-android-sml.png)

Pokud je připojení úspěšné, by měl kód spouštět v zařízení téměř okamžitě (například ukázka kalkulačku):

![Ukázka kalkulačku aplikaci spuštěnou na zařízení](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>Možnosti

Klikněte na tlačítko informace **(i)** v dolní části aplikaci odhalit **možnosti** nabídky:

![Snímek obrazovky možností nabídky](player-images/options.png)

### <a name="logs"></a>Protokoly

Zobrazit protokoly k diagnostikování problémů.

### <a name="settings"></a>Nastavení

* Přepnutí zobrazení chyby při kompilaci a modulu runtime.
* Informace o verzi.
* Odešlete zpětnou vazbu.

![Snímek obrazovky nastavení](player-images/settings.png)

## <a name="managing-devices"></a>Správa zařízení

Chcete-li připojit zařízení poprvé, postupujte podle pokynů v [nastavení & požadavky](~/tools/live-player/install.md). Můžete spárujte více zařízení (třeba iOS a Android) a spravovat je přes rozhraní IDE.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V sadě Visual Studio, vyberte **nástroje > Xamarin Live Player > Správa zařízení...**

![Správa zařízení okna](player-images/manage-tools-menu-vs.png)

Toto okno umožňuje postupujte takto:

- Pár nové zařízení naskenováním kódu
- Můžete také zadáním kódu zobrazí na jeho obrazovce spárujte zařízení
- Odeberte existující zařízení ze seznamu

Toto okno se dostanete ze seznamu zařízení.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V sadě Visual Studio pro Mac, zvolte **nástroje > Správa zařízení (Xamarin Live přehrávač)...**

![Správa zařízení okna](player-images/manage-tools-menu.png)

Toto okno umožňuje postupujte takto:

- Pár nové zařízení naskenováním kódu
- Můžete také zadáním kódu zobrazí na jeho obrazovce spárujte zařízení
- Odeberte existující zařízení ze seznamu

![Správa zařízení okna](player-images/manage.png)

Toto okno se dostanete ze seznamu zařízení:

![Zvolte Xamarin Live Player zařízení ze seznamu zařízení](player-images/manage-device-menu.png)

-----

Pokud máte jakékoli problémy najdete [omezení a řešení potíží s](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>Související odkazy

- [Omezení](~/tools/live-player/limitations.md)
- [Odstraňování potíží](~/tools/live-player/troubleshooting.md)
- [Ukázky za provozu Player Xamarin](~/tools/livehttps://developer.xamarin.com/samples.md)