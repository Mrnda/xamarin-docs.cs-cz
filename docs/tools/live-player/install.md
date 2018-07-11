---
title: Instalační program Xamarin Live Player
description: Tento dokument popisuje, jak nastavit Xamarin Live Player a používejte ho, aby živé úpravy k běžící aplikaci.
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: topgenorth
ms.author: toopge
ms.date: 05/14/2018
ms.openlocfilehash: 40c03e978cd9ce4666089f1b2a1e2ee8f47dbd81
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831670"
---
# <a name="xamarin-live-player-setup"></a>Instalační program Xamarin Live Player

Xamarin Live Player umožňuje provádět živé úpravy do vaší aplikace a mít tyto změny projeví za provozu na zařízení. Váš kód běží v rámci aplikace Xamarin Live Player – není nutné nastavit emulátory nebo použití kabely k nasazení. Tento článek popisuje, jak nastavit Xamarin Live Player.

![Funkce ve verzi Preview](~/media/shared/preview.png)

## <a name="1-get-the-app"></a>1. Stáhněte si aplikaci

# <a name="androidtabandroid"></a>[Android](#tab/android)

Xamarin Live Player je k dispozici pro Android z Google Play:

[ ![K dispozici na webu Google Play](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Pro zařízení s Androidem bez Google Play je k dispozici prostřednictvím Xamarin Live Player [HockeyApp](https://aka.ms/xlp-hockeyapp) distribuce. Kromě toho předběžnou ukázku sestavení pro Android můžete nainstalovat přímo na webu Google Play pomocí vyjádření výslovného souhlasu s [programu open beta](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="iostabios"></a>[iOS](#tab/ios)

Doporučujeme uživatelům připojit se k iOS aplikace Xamarin Live Player ve verzi Preview a využijte rychlý přístup k nejnovější vylepšení prostřednictvím testovacího prostředí. Díky přístupu do Xamarin Live Player, můžete odsouhlaste Microsoftu [Terms of Use](https://www.microsoft.com/en-us/legal/intellectualproperty/copyright/default.aspx) & [prohlášení o zásadách](https://privacy.microsoft.com/en-us/privacystatement). Microsoft může použít vaše kontaktní informace k poskytování aktualizací a speciálních nabídek k produktu Xamarin a dalších produktů a služeb Microsoftu. Kdykoli můžete kdykoli.

Pro přístup k iOS Xamarin Live Player ve verzi Preview, dokončete prosím [informace o registraci testovacího prostředí](https://fastring.xamarinliveplayer.com/), po který se zobrazí e-mailu z testovacího prostředí na tom, jak nainstalovat Xamarin Live Player iOS ve verzi Preview.

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2. Získat Visual Studio 2017

Vyžaduje se Xamarin Live Player:

- Visual Studio 2017 15.4 nebo novější.
- Visual Studio počítače a zařízení ve stejné síti Wi-Fi.

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. První přihlášení pomocí Xamarin Live Player

1. Otevřít **Visual Studio 2017**.
2. Přejděte na **nástroje > Možnosti...**  a vyberte **Xamarin > ostatní** kartu.
3. Značky **povolit Xamarin Live Player**:

  ![Zaškrtněte políčko Povolit Xamarin Live Player v okně Možnosti](install-images/vs2017-options.png)

2. Vytvořte nebo otevřete projekt Xamarin (nebo [ukázka](~/tools/live-player/samples.md)).
3. Zvolte **Live Player** v seznamu zařízení:

  ![Seznam zařízení nabízí možnost Xamarin Live Player](install-images/devices-empty-windows.png)

  * Pokud jste už spárovaná zařízení, bude k dispozici jako možnost.
  * Jinak budete vyzváni k spárování zařízení v případě potřeby.
4. Stisknutím klávesy **spustit** tlačítko, nebo vyberte jednu z následujících možností z **spustit** nebo místní nabídky:

  - **Spustit bez ladění** – můžete upravit aplikaci a změnách na zařízení najdete v tématu (aplikace se restartuje a dojde ke změně se soubor uloží).
  - **Spustit ladění** – můžete nastavit zarážky a kontrolovat proměnné, ale nemůžou je upravovat kód.

  Můžete také vybrat **nástroje > Xamarin Live Player > živě spustit aktuální zobrazení**, které lze upravit aplikaci a změnách na zařízení najdete v tématu. Aktuální zobrazení se zobrazí (a nikoli hlavní obrazovky aplikace).

5. Pokud je už spárovaná zařízení a aplikace Xamarin Live Player je spuštěného v příslušném zařízení, kód se spustí okamžitou!

  Pokud není žádné zařízení s oblastí, s pokyny, jak se zobrazí kód QR spárovat se zařízením:

  ![Pair – okno zařízení](install-images/manage-empty-windows.png)

  Pokud zařízení nemůže být kontaktován kvůli párování, mohou se zobrazit chyba.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2. Získat Visual Studio pro Mac

Vyžaduje se Xamarin Live Player:

- OS X 10.11 macOS 10.12 nebo vyšší
- Visual Studio for Mac
- Počítač Mac a zařízení ve stejné síti Wi-Fi

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. První přihlášení pomocí Xamarin Live Player

1. Otevřít **Visual Studio for Mac**.
2. Přejděte na **sady Visual Studio > Předvolby...**  a vyberte **projektů > Xamarin Live Player (Preview)** kartu.
3. Značky **povolit Xamarin Live Player**:

  [![Zaškrtněte políčko Povolit Xamarin Live Player v okně Možnosti](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

2. Vytvořte nebo otevřete projekt Xamarin (nebo [ukázka](~/tools/live-player/samples.md)).
3. Zvolte **Live Player** v seznamu zařízení.

  ![Seznam zařízení nabízí možnost Xamarin Live Player](install-images/devices.png)

  * Pokud jste už spárovaná zařízení, bude k dispozici jako možnost.
  * Jinak budete vyzváni k spárování zařízení v případě potřeby.
  * Zvolte **zařízení s aplikací Xamarin Live Player...**  ke správě zařízení, které chcete použít s Xamarin Live Player.

4. Stisknutím klávesy **spustit** tlačítko, nebo vyberte jednu z následujících možností z **spustit** nebo místní nabídky:

  ![Nabídka Možnosti spuštění](install-images/run-menu.png)

  - **Spustit bez ladění** – můžete upravit aplikaci a změnách na zařízení najdete v tématu (aplikace se restartuje a dojde ke změně se soubor uloží).
  - **Spustit ladění** – můžete nastavit zarážky a kontrolovat proměnné, ale nemůžou je upravovat kód.
  - **Živě spustit aktuální zobrazení** – můžete upravit aplikaci a změnách na zařízení najdete v tématu. Aktuální zobrazení se zobrazí (a nikoli hlavní obrazovky aplikace).

5. Pokud je už spárovaná zařízení a aplikace Xamarin Live Player je spuštěného v příslušném zařízení, kód se spustí okamžitou!

  Pokud je párována žádná zařízení, zobrazí se kód QR s pokyny, jak spárovat se zařízením:

  ![Pair – okno zařízení](install-images/manage-empty.png)

  Pokud zařízení nemůže být kontaktován kvůli párování, se zobrazí chyba:

  ![Nelze se připojit k zařízení chybová zpráva](install-images/error-cannot-connect.png)


-----

Pokud dojde k potížím, nebo se nemůže připojit, přečtěte si téma [omezení a řešení potíží s](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>Související odkazy

- [Omezení](~/tools/live-player/limitations.md)
- [Odstraňování potíží](~/tools/live-player/troubleshooting.md)
- [Ukázky Xamarin Live Player](~/tools/live-player/samples.md)
