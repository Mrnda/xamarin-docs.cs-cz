---
title: "Omezení"
description: "Některá omezení Xamarin Live Player"
ms.topic: article
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 08/04/2017
ms.openlocfilehash: d551c0a82fb8f970ce5a00ec9e64ac7f49c81a44
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="limitations"></a>Omezení

![Funkce ve verzi Preview](~/media/shared/preview.png)

## <a name="device-requirements"></a>Požadavky na zařízení
Aplikace Xamarin Live Player podporuje následující zařízení:

### <a name="ios"></a>iOS
- iOS 9.0 nebo novější.
- ARM64 procesoru.
- Zkontrolujte [obchod](https://itunes.apple.com/us/app/xamarin-live-player/id1228841832?mt=8) seznam podporovaných zařízení.

### <a name="android"></a>Android
- Android 4.2 nebo novější.
- ARM – v7a, ARM v8a, ARM64-v8a, x 86 nebo x86_64 procesoru.


## <a name="limitations"></a>Omezení

Existují některá omezení akcí, které můžete spustit Xamarin Live Player, včetně následujících položek:

### <a name="android"></a>Android
- Android uživatelská rozhraní navrhovat s AXML soubory nejsou aktuálně podporovány.

### <a name="ios"></a>iOS
- Některé funkce iOS scénáře nejsou podporovány.
- iOS XIB soubory nejsou podporovány.

### <a name="xamarinforms"></a>Xamarin.Forms
- Vlastní nástroji pro vykreslování nejsou podporovány.
- Účinky nejsou podporovány.
- Vlastní ovládací prvky s vlastní vazbu vlastnosti nejsou podporovány.
- Vložené prostředky nejsou podporovány (ie. vložení obrázků nebo jiným prostředkům v PCL).

### <a name="misc"></a>Různé
- Omezenou podporu pro reflexi (ovlivňuje aktuálně některých oblíbených NuGets, jako je SQLite a Json.NET). Ostatní NuGets jsou stále podporovány.
- Nebylo možné přepsat některé třídy systému (například nemůžete implementovat podtřídy).
- Některé funkce platformy, které vyžadují zřizování nemůže pracovat v aplikaci Xamarin Live Player (ale byla nakonfigurována pro běžné operace, jako je přístup Galerie fotografií).
- Vlastní cíle a kroků sestavení se ignorují. Například nástroje, například Fody nelze vložit.

> [!WARNING]
> **Poznámka:** Xamarin Live Player nefunguje s Xamarin Studio

Nahlaste všechny další problémy na [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Související odkazy

- [Odstraňování potíží](~/tools/live-player/troubleshooting.md)
- [Instalace a nastavení](~/tools/live-player/install.md)
