---
title: Začínáme s iOS 12, 12 pro tvOS a watchOS 5
description: Tento dokument popisuje, jak jejich nastavení až po sestavení aplikace pro iOS 12, 12 pro tvOS a watchOS 5 s využitím kódu Xamarin. Popisuje, jak stáhnout Xcode 10 a aktualizace sady Visual Studio pro Mac a Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/07/2018
ms.openlocfilehash: cb84ddc646933d253ca72fe8f9f581364aab698b
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615171"
---
# <a name="getting-started-with-ios-12-tvos-12-and-watchos-5"></a>Začínáme s iOS 12, 12 pro tvOS a watchOS 5

![Náhled](~/media/shared/preview.png)

> [!WARNING]
> Podpora rozhraní Xamarin pro iOS 12, 12 pro tvOS a watchOS 5 SDK distribuovat s Xcode 10 je aktuálně ve verzi preview, což znamená, že může obsahovat chyby, není funkce dokončení a může změnit. Použijte pouze experimentování.

Tento dokument popisuje, jak nastavit k vytváření aplikací Xamarin, které volají rozhraní API vydané s Xcode 10. Popisuje, jak stáhnout Xcode 10 a aktualizace sady Visual Studio pro Mac a Visual Studio 2017.

## <a name="download-and-install"></a>Stažení a instalace

1. **Instalace nejnovější verze beta Xcode 10** – vývojáře Apple zaregistrovaný, můžete stáhnout a nainstalovat nejnovější verzi Xcode 10 z [portálu Apple Developer](https://developer.apple.com/download/).

2. **Spuštění Xcode 10** – vyžaduje Xcode 10 spustit před aktualizací a spuštění sady Visual Studio pro Mac nebo Visual Studio 2017, když se instaluje některé nástroje této Xamarin.

3. **Aktualizace sady Visual Studio pro Mac a Visual Studio 2017** – postupujte podle pokynů [release blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-5/) nainstalovat Xamarin ve verzi preview.

4. _(volitelné)_  **Nainstalovat nejnovější beta verze iOS na zařízeních s Iosem** – pro testování zařízení z aplikace, které používají rozhraní API zavedená s 10 Xcode, můžou vývojáři registrované Apple [Stáhnout](https://developer.apple.com/download) a nainstalujte nejnovější verzi Beta verze pro vývojáře na svých zařízeních.

   > [!TIP]
   > I v případě, že vaše aplikace nepoužívá žádná nová rozhraní API, je nutné sestavit spolu se sadami SDK nejnovější Xcode 10 a otestovat a ujistit se, že všechno funguje podle očekávání. Pokud aplikace nevolá žádná nová rozhraní API, můžete ji znovu zkompilovat s tyto nové sady SDK a testovat na zařízeních, která ještě nebyla upgradována na nový operační systém.
   >
   > Předtím, než upgrade na nejnovější operační systém zařízení uvolní od společnosti Apple k testování aplikace Xamarin, nezapomeňte na následující:
   >
   > - Čtení [zpráva k vydání verze společnosti Apple](https://developer.apple.com/download/) aktualizací operačního systému.
   > - Přečtěte si Xamarin ve verzi preview [release blogový příspěvek](https://releases.xamarin.com/preview-release-xcode-10-beta-5/).

## <a name="related-links"></a>Související odkazy

- [Stáhnout Xcode 10](https://developer.apple.com/download/)
- Xamarin ve verzi preview [release blogový příspěvek](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
