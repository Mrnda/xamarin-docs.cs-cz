---
title: Začínáme s systému macOS Mojave
description: Tento dokument popisuje, jak získat nastavena tak, aby systému macOS sestavení aplikace Mojave s Xamarin.Mac. Popisuje, jak stáhnout Xcode 10 a aktualizace sady Visual Studio for Mac.
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: ee5269bf314401328d0184631e817b37ce091479
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067167"
---
# <a name="getting-started-with-macos-mojave"></a>Začínáme s systému macOS Mojave

![Náhled](~/media/shared/preview.png)

> [!WARNING]
> Xamarin pro macOS Mojave podporu je aktuálně ve verzi preview, což znamená, že může obsahovat chyby, není funkce dokončení a může změnit.
> Použijte pouze experimenty.

> [!NOTE]
> Další informace najdete v tématu [poznámky k verzi](https://releases.xamarin.com/preview-release-xcode-10-beta/) pro Xamarin náhled verze.

Tento dokument popisuje, jak získat nastavena tak, aby systému macOS sestavení aplikace Mojave s Xamarin.Mac. Popisuje, jak stáhnout Xcode 10 a aktualizace sady Visual Studio for Mac.

## <a name="download-and-install"></a>Stáhněte a nainstalujte

1. **Nainstalujte nejnovější beta Xcode 10** – zaregistrován Apple vývojáři můžete stáhnout a nainstalovat nejnovější verzi Xcode 10 z [portál pro vývojáře Apple](https://developer.apple.com/download/).

   > [!NOTE]
   > Systému macOS Mojave SDK je distribuován s Xcode 10.

2. **Spustit Xcode 10** – 10 Xcode spustit před aktualizací a spuštění sady Visual Studio pro Mac; nainstaluje několik nástrojů, které vyžaduje Xamarin.

3. **Aktualizace sady Visual Studio pro Mac** – postupujte podle pokynů [poznámky k verzi](https://releases.xamarin.com/preview-release-xcode-10-beta/) nainstalovat ve verzi preview Xamarin.

4. _(volitelné)_  **Nainstalovat nejnovější Mojave beta systému macOS na počítači Mac** – k testování Xamarin.Mac aplikace, které používají nově zavedené systému macOS Mojave rozhraní API, mohou vývojáři registrované Apple [Stáhnout](https://developer.apple.com/download/) a nainstalovat nejnovější systému macOS Mojave vývojáře beta.

   > [!TIP]
   > I když se aplikace nepoužívá žádné nové systému macOS Mojave rozhraní API, nezapomeňte sestavení s systému macOS Mojave SDK (instalovanou se Xcode 10) a otestovat ji a ujistěte se, že vše funguje podle očekávání. Pokud aplikace nemá volání žádné nové rozhraní API, můžete ji znovu zkompilovat s systému macOS Mojave SDK a otestovat ji bez upgradovat operační systém počítače Mac.

   > [!IMPORTANT]
   > Před upgradem na verzi systému macOS Mojave pro sestavení a otestování Xamarin.Mac aplikace, které volají nového systému macOS Mojave rozhraní API pro počítače Mac:
   > - Čtení [poznámky k verzi společnosti Apple](https://developer.apple.com/download/) pro aktualizaci operačního systému.
   > - Pro čtení [poznámky k verzi](https://releases.xamarin.com/preview-release-xcode-10-beta/) pro Xamarin náhled verze. Všimněte si, že tento první preview neobsahuje vazby pro nové systému macOS Mojave AppKit rozhraní API (například tmavý režim).

## <a name="related-links"></a>Související odkazy

- [Stáhněte si Xcode 10](https://developer.apple.com/download/)
- Xamarin preview [poznámky k verzi](https://releases.xamarin.com/preview-release-xcode-10-beta/)
