---
title: Začínáme s iOS 12, 12 pro tvOS a watchOS 5
description: Tento dokument popisuje, jak jejich nastavení až po sestavení iOS 12 a tvOS 12 aplikací s využitím kódu Xamarin. Popisuje, jak stáhnout Xcode 10 a aktualizace sady Visual Studio pro Mac a Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 70f67f934c2503e6f6fa0d3bae1f37bcc1f6f0a4
ms.sourcegitcommit: cfb72be633e335147d156af3ef9527151b9e31d9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030649"
---
# <a name="getting-started-with-ios-12-tvos-12-and-watchos-5"></a>Začínáme s iOS 12, 12 pro tvOS a watchOS 5

![Náhled](~/media/shared/preview.png)

> [!WARNING]
> Xamarin pro iOS tvOS 12, 12, a podporu watchOS 5 je aktuálně ve verzi preview, což znamená, že může obsahovat chyb, které není plně funkční, a může se změnit. Použijte pouze experimentování.

> [!NOTE]
> Další informace najdete v článku Xamarin ve verzi preview [release blogový příspěvek](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

Tento dokument popisuje, jak jejich nastavení až po sestavení aplikace pro iOS 12, 12 pro tvOS a watchOS 5 s využitím kódu Xamarin. Popisuje, jak stáhnout Xcode 10 a aktualizace sady Visual Studio pro Mac a Visual Studio 2017.

## <a name="download-and-install"></a>Stažení a instalace

1. **Instalace nejnovější verze beta Xcode 10** – vývojáře Apple zaregistrovaný, můžete stáhnout a nainstalovat nejnovější verzi Xcode 10 z [portálu Apple Developer](https://developer.apple.com/download/).

   > [!NOTE]
   > IOS 12, 12 pro tvOS a watchOS 5 sady SDK jsou distribuované s Xcode 10.

2. **Spuštění Xcode 10** – spustit 10 Xcode před aktualizací a spuštění sady Visual Studio pro Mac nebo Visual Studio 2017, se nainstaluje několik nástrojů, které vyžaduje Xamarin.

3. **Aktualizace sady Visual Studio pro Mac a Visual Studio 2017** – postupujte podle pokynů [release blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-3/) nainstalovat Xamarin ve verzi preview.

4. _(volitelné)_  **Nainstalovat nejnovější beta verze iOS na zařízeních s Iosem** – pro zařízení testování aplikací, použití nově zavedeném iOS 12, tvOS 12 nebo watchOS 5 rozhraní API pro registrovaný vývojáře Apple můžete [Stáhnout](https://developer.apple.com/download) a Nainstalujte nejnovější iOS 12, 12 pro tvOS nebo watchOS 5 Beta pro vývojáře na svých zařízeních.

   > [!TIP]
   > I v případě, že vaše aplikace nepoužívá žádné nové iOS 12, 12 pro tvOS nebo watchOS 5 rozhraní API, je potřeba ji sestavíte pomocí iOS 12, tvOS 12 nebo watchOS 5 SDK (instalovanou se Xcode 10) a testovací a ujistit se, že všechno funguje dle očekávání. Pokud aplikace nevolá žádná nová rozhraní API, můžete znovu zkompilovat ji v iOS 12, tvOS 12 nebo watchOS 5 SDK a testovat na zařízeních, která ještě nebyla upgradována na nový operační systémy.

   > [!IMPORTANT]
   > Před provedením upgradu vašeho zařízení iOS 12, tvOS 12 nebo watchOS 5 k testování aplikací Xamarin, které volají nové vstupů-výstupů 12, tvOS 12 nebo watchOS 5 rozhraní API:
   > - Čtení [zpráva k vydání verze společnosti Apple](https://developer.apple.com/download/) aktualizaci operačního systému.
   > - Přečtěte si Xamarin ve verzi preview [release blogový příspěvek](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

## <a name="related-links"></a>Související odkazy

- [Stáhnout Xcode 10](https://developer.apple.com/download/)
- Xamarin ve verzi preview [release blogový příspěvek](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)
