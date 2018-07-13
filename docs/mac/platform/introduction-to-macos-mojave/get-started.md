---
title: Začínáme s macOS Mojave
description: Tento dokument popisuje, jak jejich nastavení až po sestavení macOS Mojave aplikací Xamarin.Mac. Popisuje, jak stáhnout Xcode 10 a aktualizace sady Visual Studio pro Mac.
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/08/2018
ms.openlocfilehash: cf2725eafa18330a07a08db4235bad1a1ecd47b6
ms.sourcegitcommit: cfb72be633e335147d156af3ef9527151b9e31d9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030617"
---
# <a name="getting-started-with-macos-mojave"></a>Začínáme s macOS Mojave

![Náhled](~/media/shared/preview.png)

> [!WARNING]
> MacOS Xamarinu pro podporu Mojave je aktuálně ve verzi preview, což znamená, že může obsahovat chyby, není plně funkční a může změnit.
> Použijte pouze experimentování.

> [!NOTE]
> Další informace najdete v článku Xamarin ve verzi preview [release blogový příspěvek](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

Tento dokument popisuje, jak jejich nastavení až po sestavení macOS Mojave aplikací Xamarin.Mac. Popisuje, jak stáhnout Xcode 10 a aktualizace sady Visual Studio pro Mac.

## <a name="download-and-install"></a>Stažení a instalace

1. **Instalace nejnovější verze beta Xcode 10** – vývojáře Apple zaregistrovaný, můžete stáhnout a nainstalovat nejnovější verzi Xcode 10 z [portálu Apple Developer](https://developer.apple.com/download/).

   > [!NOTE]
   > MacOS Mojave SDK je distribuován spolu s Xcode 10.

2. **Spuštění Xcode 10** – spustit 10 Xcode před aktualizací a spuštění sady Visual Studio pro Mac; nainstaluje několik nástrojů, které vyžaduje Xamarin.

3. **Aktualizace sady Visual Studio pro Mac** – postupujte podle pokynů [release blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-3/) nainstalovat Xamarin ve verzi preview.

4. _(volitelné)_  **Nainstalovat nejnovější beta Mojave macOS na počítači Mac** – k otestování aplikace Xamarin.Mac, používající nově zavedeném macOS Mojave rozhraní API pro registrovaný můžou vývojáři Apple [Stáhnout](https://developer.apple.com/download/) a nainstalovat nejnovější beta developer Mojave macOS.

   > [!TIP]
   > I v případě, že vaše aplikace nepoužívá žádné nové macOS Mojave rozhraní API, je potřeba vytvořit v systému macOS SDK Mojave (instalovanou se Xcode 10) a otestovat a ujistit se, že všechno funguje podle očekávání. Pokud aplikace nevolá žádná nová rozhraní API, můžete ji znovu zkompilovat v systému macOS Mojave SDK a testování bez upgradu operačního systému počítače Mac.

   > [!IMPORTANT]
   > Před upgradem na macOS Mojave pro vytváření a testování aplikací Xamarin.Mac, které volají macOS nové rozhraní API Mojave počítače Mac:
   > - Čtení [zpráva k vydání verze společnosti Apple](https://developer.apple.com/download/) aktualizaci operačního systému.
   > - Přečtěte si Xamarin ve verzi preview [release blogový příspěvek](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

## <a name="related-links"></a>Související odkazy

- [Stáhnout Xcode 10](https://developer.apple.com/download/)
- Xamarin ve verzi preview [release blogový příspěvek](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)
