---
title: Začínáme s iOS 12 a tvOS 12
description: Tento dokument popisuje, jak jejich nastavení až po sestavení iOS 12 a tvOS 12 aplikací s využitím kódu Xamarin. Popisuje, jak stáhnout Xcode 10 a aktualizace sady Visual Studio pro Mac a Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 887a5cc72b951b2f115cdc6998a6cf6ca7a94df0
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815628"
---
# <a name="getting-started-with-ios-12-and-tvos-12"></a>Začínáme s iOS 12 a tvOS 12

![Náhled](~/media/shared/preview.png)

> [!WARNING]
> Rozhraní Xamarin 12 a tvOS 12 podpora iOS je momentálně ve verzi preview, což znamená, že může obsahovat chyb, které není plně funkční, a může změnit. Použijte pouze experimentování.

> [!NOTE]
> Další informace najdete v článku [poznámky k verzi](https://releases.xamarin.com/preview-release-xcode-10-beta/) pro Xamarin ve verzi preview verzi.

Tento dokument popisuje, jak jejich nastavení až po sestavení iOS 12 a tvOS 12 aplikací s využitím kódu Xamarin. Popisuje, jak stáhnout Xcode 10 a aktualizace sady Visual Studio pro Mac a Visual Studio 2017.

## <a name="download-and-install"></a>Stažení a instalace

1. **Instalace nejnovější verze beta Xcode 10** – vývojáře Apple zaregistrovaný, můžete stáhnout a nainstalovat nejnovější verzi Xcode 10 z [portálu Apple Developer](https://developer.apple.com/download/).

   > [!NOTE]
   > IOS 12 a tvOS 12 sady SDK jsou distribuované s Xcode 10.

2. **Spuštění Xcode 10** – spustit 10 Xcode před aktualizací a spuštění sady Visual Studio pro Mac nebo Visual Studio 2017, se nainstaluje několik nástrojů, které vyžaduje Xamarin.

3. **Aktualizace sady Visual Studio pro Mac a Visual Studio 2017** – postupujte podle pokynů [poznámky k verzi](https://releases.xamarin.com/preview-release-xcode-10-beta/) nainstalovat Xamarin ve verzi preview.

4. _(volitelné)_  **Nainstalovat nejnovější beta verze iOS na zařízeních s Iosem** – pro zařízení testování aplikací, použití nově zavedeném iOS 12 nebo tvOS 12 rozhraní API pro registrovaný vývojáře Apple můžete [Stáhnout](https://developer.apple.com/download) a nainstalovat nejnovější iOS 12 nebo beta verze developer tvOS 12 na svých zařízeních s iOS a tvOS.

   > [!TIP]
   > I v případě, že vaše aplikace nepoužívá žádné nové vstupů-výstupů 12 nebo tvOS 12 rozhraní API, je potřeba ji sestavíte pomocí iOS 12 nebo tvOS 12 SDK (instalovanou se Xcode 10) a testování a ujistit se, že všechno funguje jako očekávání. Pokud aplikace nevolá žádná nová rozhraní API, můžete znovu zkompilovat ji v iOS 12 nebo tvOS 12 sady SDK a testovat na zařízeních, která ještě nebyla upgradována na nový operační systémy.

   > [!IMPORTANT]
   > Před provedením upgradu vašeho zařízení s Iosem 12 nebo tvOS 12 k testování aplikací v Xamarinu, které volají nové vstupů-výstupů 12 nebo tvOS 12 rozhraní API:
   > - Čtení [zpráva k vydání verze společnosti Apple](https://developer.apple.com/download/) aktualizaci operačního systému.
   > - Čtení [poznámky k verzi](https://releases.xamarin.com/preview-release-xcode-10-beta/) pro Xamarin ve verzi preview verzi.

## <a name="related-links"></a>Související odkazy

- [Stáhnout Xcode 10](https://developer.apple.com/download/)
- Xamarin ve verzi preview [zpráva k vydání verze](https://releases.xamarin.com/preview-release-xcode-10-beta/)
