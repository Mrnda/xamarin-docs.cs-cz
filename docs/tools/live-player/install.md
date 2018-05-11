---
title: Instalační program za provozu Player Xamarin
description: Upravit a testování aplikací v reálném čase na systému iOS nebo zařízení se systémem Android
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: topgenorth
ms.author: toopge
ms.date: 11/22/2017
ms.openlocfilehash: 0f343e253a57de33d7e19e648862e6d11fa5af5f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="xamarin-live-player-setup"></a>Instalační program za provozu Player Xamarin

Xamarin Live Player umožňuje provádět úpravy za provozu do vaší aplikace a mají tyto změny projeví za provozu na zařízení. Váš kód běží v rámci aplikace Xamarin Live Player – není nutné nastavit emulátorů nebo nasazení pomocí kabelů! Tento článek popisuje, jak nastavit Player Xamarin za provozu.

![Funkce ve verzi Preview](~/media/shared/preview.png)

## <a name="1-get-the-app"></a>1. Získat aplikaci

# <a name="androidtabandroid"></a>[Android](#tab/android)

Přehrávač Xamarin za provozu je k dispozici pro Android z Google Play:

[ ![K dispozici na webu Google Play](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Pro zařízení s Androidem bez Google Play je k dispozici prostřednictvím Xamarin Live Player [HockeyApp](https://aka.ms/xlp-hockeyapp) distribuce. Kromě toho časné sestavení pro Android může nainstalovat přímo na webu Google Play vyjádření výslovného souhlasu s [otevřete beta programu](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="iostabios"></a>[iOS](#tab/ios)

Doporučujeme, aby uživatelé pro připojení [aplikace Xamarin Live Player _iOS Preview_ ](https://aka.ms/liveplayeralpha) rychlý přístup k nejnovější vylepšení prostřednictvím TestFlight.

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2. Získat Visual Studio 2017

Přehrávač Xamarin za provozu vyžaduje:

- Visual Studio 2017 15.4 nebo novější.
- Visual Studio počítače a zařízení ve stejné síti Wi-Fi.

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Pomocí Xamarin Player za provozu poprvé

1. Otevřete **Visual Studio 2017**.
2. Přejděte na **nástroje > Možnosti...**  a vyberte **Xamarin > jiných** kartě.
3. Značky **povolit za provozu Player Xamarin**:

  ![Zaškrtněte políčko Povolit Player Xamarin za provozu v okně Možnosti](install-images/vs2017-options.png)

2. Vytvoření nebo otevření projektu Xamarin (nebo [ukázka](~/tools/live-player/samples.md)).
3. Zvolte **Live Player** v seznamu zařízení:

  ![Seznam zařízení nabízí možnost Xamarin Live Player](install-images/devices-empty-windows.png)

  * Pokud jste již spárován zařízení, bude k dispozici možnost.
  * V opačném případě budete vyzváni k spárovat zařízení v případě potřeby.
4. Stiskněte **spustit** tlačítko nebo vyberte jednu z následujících možností z **spustit** nebo místní nabídce:

  - **Spustit bez ladění** – můžete upravit aplikaci a změnách v zařízení najdete v tématu (aplikace je restartují podle změn a soubor uložit).
  - **Spuštění ladění** – můžete nastavit zarážky a zkontrolovat proměnné, ale kód nelze upravit.

  Případně vyberte možnost **nástroje > Xamarin Live Player > Live spustit aktuální zobrazení**, které lze upravit aplikaci a změnách v zařízení najdete v tématu. (Místo hlavní obrazovky aplikace) se zobrazuje aktuální zobrazení.

5. Pokud je již spárován zařízení a aplikace Xamarin Live Player běží na zařízení, kód spustí okamžitou!

  Pokud není žádné zařízení je spárován, zobrazí se s pokyny, jak kód QR spárovat zařízení:

  ![Pár okno zařízení](install-images/manage-empty-windows.png)

  Pokud se pro párování nelze kontaktovat zařízení, může zobrazovat chybu.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2. Získání sady Visual Studio pro Mac

Přehrávač Xamarin za provozu vyžaduje:

- OS X 10.11, systému macOS 10.12 nebo vyšší
- Visual Studio for Mac
- Mac a zařízení ve stejné síti Wi-Fi

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Pomocí Xamarin Player za provozu poprvé

1. Otevřete **Visual Studio pro Mac**.
2. Přejděte na **sady Visual Studio > Předvolby...**  a vyberte **projekty > Xamarin Live Player (Preview)** kartě.
3. Značky **povolit za provozu Player Xamarin**:

  [![Zaškrtněte políčko Povolit Player Xamarin za provozu v okně Možnosti](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

2. Vytvoření nebo otevření projektu Xamarin (nebo [ukázka](~/tools/live-player/samples.md)).
3. Zvolte **Live Player** v seznamu zařízení.

  ![Seznam zařízení nabízí možnost Xamarin Live Player](install-images/devices.png)

  * Pokud jste již spárován zařízení, bude k dispozici možnost.
  * V opačném případě budete vyzváni k spárovat zařízení v případě potřeby.
  * Zvolte **Xamarin za provozu Player zařízení...**  ke správě zařízení, které chcete používat s Xamarin za provozu.

4. Stiskněte **spustit** tlačítko nebo vyberte jednu z následujících možností z **spustit** nebo místní nabídce:

  ![Spustit možnosti nabídky](install-images/run-menu.png)

  - **Spustit bez ladění** – můžete upravit aplikaci a změnách v zařízení najdete v tématu (aplikace je restartují podle změn a soubor uložit).
  - **Spuštění ladění** – můžete nastavit zarážky a zkontrolovat proměnné, ale kód nelze upravit.
  - **Live spustit aktuální zobrazení** – můžete upravit aplikaci a změnách v zařízení najdete v tématu. (Místo hlavní obrazovky aplikace) se zobrazuje aktuální zobrazení.

5. Pokud je již spárován zařízení a aplikace Xamarin Live Player běží na zařízení, kód spustí okamžitou!

  Pokud je spárován žádné zařízení, se zobrazí kód QR s pokyny, jak spárovat zařízení:

  ![Pár okno zařízení](install-images/manage-empty.png)

  Pokud se pro párování nelze kontaktovat zařízení, zobrazí se chyba:

  ![Nelze se připojit k zařízení chybová zpráva](install-images/error-cannot-connect.png)


-----

Pokud dojde k potížím, nebo se nemůže připojit, najdete v části [omezení a řešení potíží s](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>Související odkazy

- [Omezení](~/tools/live-player/limitations.md)
- [Odstraňování potíží](~/tools/live-player/troubleshooting.md)
- [Ukázky za provozu Player Xamarin](~/tools/live-player/samples.md)
