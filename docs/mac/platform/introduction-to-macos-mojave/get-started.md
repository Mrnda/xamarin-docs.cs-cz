---
title: Začínáme s macOS Mojave
description: Tento dokument popisuje, jak jejich nastavení až po sestavení macOS Mojave aplikací Xamarin.Mac. Popisuje, jak stáhnout Xcode 10 a aktualizace sady Visual Studio pro Mac.
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/08/2018
ms.openlocfilehash: 213f1feb53abf4a4eb00ae0d65c312eaaec95614
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615363"
---
# <a name="getting-started-with-macos-mojave"></a>Začínáme s macOS Mojave

![Náhled](~/media/shared/preview.png)

> [!WARNING]
> MacOS Xamarinu pro podporu Mojave je aktuálně ve verzi preview, což znamená, že může obsahovat chyby, není plně funkční a může změnit.
> Použijte pouze experimentování. 

Tento dokument popisuje, jak jejich nastavení až po sestavení macOS Mojave aplikací Xamarin.Mac. Popisuje, jak stáhnout Xcode 10 a aktualizace sady Visual Studio pro Mac.

## <a name="download-and-install"></a>Stažení a instalace

1. **Instalace nejnovější verze beta Xcode 10** – vývojáře Apple zaregistrovaný, můžete stáhnout a nainstalovat nejnovější verzi Xcode 10 z [portálu Apple Developer](https://developer.apple.com/download/).

2. **Spuštění Xcode 10** – spustit 10 Xcode před aktualizací a spuštění sady Visual Studio pro Mac; nainstaluje několik nástrojů, které vyžaduje Xamarin.

3. **Aktualizace sady Visual Studio pro Mac** – postupujte podle pokynů [release blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-5/) nainstalovat Xamarin ve verzi preview.

4. _(volitelné)_  **Nainstalovat nejnovější beta Mojave macOS na počítači Mac** – k otestování aplikace Xamarin.Mac, používající nově zavedeném macOS Mojave rozhraní API pro registrovaný můžou vývojáři Apple [Stáhnout](https://developer.apple.com/download/) a nainstalovat nejnovější beta developer Mojave macOS.

   > [!TIP]
   > I v případě, že vaše aplikace nepoužívá žádné nové macOS Mojave rozhraní API, je potřeba vytvořit v systému macOS Mojave SDK a otestovat a ujistit se, že všechno funguje podle očekávání. Pokud aplikace nevolá žádná nová rozhraní API, můžete ji znovu zkompilovat v systému macOS Mojave SDK a testování bez upgradu operačního systému počítače Mac.
   >
   > Před upgradem na macOS Mojave pro vytváření a testování aplikací Xamarin.Mac, které volají macOS nové rozhraní API Mojave počítače Mac:
   >
   > - Čtení [zpráva k vydání verze společnosti Apple](https://developer.apple.com/download/) aktualizaci operačního systému.
   > - Přečtěte si Xamarin ve verzi preview [release blogový příspěvek](https://releases.xamarin.com/preview-release-xcode-10-beta-5/).

## <a name="related-links"></a>Související odkazy

- [Stáhnout Xcode 10](https://developer.apple.com/download/)
- Xamarin ve verzi preview [release blogový příspěvek](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
