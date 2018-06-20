---
title: Ladění integrace
description: Tento dokument popisuje, jak ladit Xamarin sešity integrace, straně agenta a klienta v systému Windows a macu.
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: 6e37b1ac3d0fb78b5737ebe97b5a28ab40adb648
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36269054"
---
# <a name="debugging-integrations"></a>Ladění integrace

## <a name="debugging-agent-side-integrations"></a>Ladění na straně agenta integrace

Ladění na straně agenta integrace se nejlépe provádí pomocí metody protokolování poskytované `Log` třídy v `Xamarin.Interactive.Logging`. Najdete v článku [ `API docs` ](https://developer.xamarin.com/api/type/Xamarin.Interactive.Logging.Log/) pro metod k volání.

V systému macOS, protokolu zprávy se zobrazují v obou nabídce log viewer (**okno > Log Viewer**) a v protokolu klientů. V systému Windows zprávy se zobrazují pouze v protokolu klienta, jako je prohlížeč žádné protokolu.

Protokol klienta je v následujících umístěních v systému macOS nebo ve Windows:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

Jednou z věcí zajímat je, že při načítání integrace prostřednictvím obvykle `#r` mechanismus během vývoje, integrace sestavení bude být zachyceny jako _závislostí_ sešitu a spojených s ním, pokud se absolutní cesta nepoužívá se. To může způsobit změny, která se zobrazí nešířily, jako kdyby znovu sestavit integrace nebyla nic.

## <a name="debugging-client-side-integrations"></a>Ladění na straně klienta integrace

Integrace klienta jsou napsané v jazyce JavaScript a načtena do prostor naše webové prohlížeče (najdete v článku [architektura](~/tools/workbooks/sdk/architecture.md) dokumentace), je nejlepší způsob, jak je ladění používání nástrojů pro vývojáře WebKit v systému Mac, nebo pomocí F12 výběru v systému Windows .

Obě sady nástrojů umožňují zobrazit JavaScript nebo TypeScript zdroj, nastavte zarážky, zobrazit výstup konzoly a kontrolovat a upravovat modelu DOM.

### <a name="mac"></a>Mac

Pokud chcete povolit nástrojů pro vývojáře pro Xamarin sešity v systému Mac, spusťte následující příkaz v terminálu:

```shell
defaults write com.xamarin.Workbooks WebKitDeveloperExtras -bool true
```

a znovu spusťte Xamarin sešity. Až to uděláte tak, měli byste vidět **zkontrolovat Element** objeví ve vaší místní nabídce klikněte pravým tlačítkem a novou **vývojáře** podokně bude k dispozici v předvolbách sešity. Tato možnost vám umožňuje zvolit, pokud chcete otevřít při spuštění nástroje pro vývojáře:

[![Podokno vývojáře](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

Je tato předvolba jen restartování také – budete muset po restartování klienta sešity se projeví na nových sešitů. Aktivace prostřednictvím místní nabídky nebo preference nástrojů pro vývojáře zobrazí známé uživatelské rozhraní Safari:

[![Nástroje pro vývojáře Safari](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Informace o používání nástrojů pro vývojáře Safari najdete v tématu [WebKit inspector dokumentaci][webkit-docs].

### <a name="windows"></a>Windows

V systému Windows, týmem IE poskytuje nástroj označuje jako "F12 výběru" tedy vzdáleného ladicího programu pro embedded instance Internet Exploreru. Tento nástroj naleznete v následujícím umístění:

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

Spuštění výběru F12 a měli vidět embedded instance, která pohání prostor klienta sešity v seznamu. Zvolte jej a známých F12 ladicí nástroje z Internet Exploreru se zobrazí, připojené ke klientovi:

[![Nástroje pro F12](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector
