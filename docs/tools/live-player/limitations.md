---
title: Omezení Xamarin Live Player
description: Tento dokument popisuje omezení pro Xamarin Live Player. Tento článek popisuje požadavky na zařízení, s typy projektů a dalších témat, která různé funkce.
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: ea71391382f9e1ecb80cbf5f2d5bf127e0d6d1be
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815279"
---
# <a name="limitations-of-xamarin-live-player"></a>Omezení Xamarin Live Player

![Funkce ve verzi Preview](~/media/shared/preview.png)

## <a name="device-requirements"></a>Požadavky na zařízení
Aplikace Xamarin Live Player podporuje následující zařízení:

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Android 4.2 nebo novější.
- ARM – v7a, ARM v8a, ARM64 v8a, x 86, x86_64 procesor nebo.

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 nebo novější.
- Procesor ARM64.

-----

## <a name="limitations"></a>Omezení

Existují určitá omezení na věci, které lze spustit Xamarin Live Player, včetně následujících položek:

### <a name="xamarinforms"></a>Xamarin.Forms

- Vlastní Renderery nejsou podporovány.
- Účinky nejsou podporovány.
- Vlastní ovládací prvky s vlastní umožňujících vazbu vlastnosti nejsou podporovány.
- Vložené prostředky nejsou podporovány (tj. vkládání obrázků nebo jiných prostředků v PCL).
- Architektury MVVM třetích stran nejsou podporovány (tj. PRISM Mvvm různé, Mvvm Light, atd.).
- Katalogy assetů v Iosu se nepodporují.

### <a name="other-project-types"></a>Ostatní typy projektů

- Live Player není určena pro nativní Android nebo iOS projekty (používající Android XML nebo scénáře pro uživatelské rozhraní).

### <a name="misc"></a>Různé

- Omezená podpora pro účely reflexe (aktuálně má vliv na některé oblíbené balíčky Nuget, podobně jako SQLite a Json.NET). Další balíčky Nuget může i nadále podporovaná.
- Některé systémové třídy se nedá přepsat (například je nelze implementovat podtřídu).
- Některé funkce platformy, které vyžadují zřizování nemůže pracovat v aplikaci Xamarin Live Player (ale byla nakonfigurována pro běžné operace, jako jsou fotky Galerie přístup).
- Vlastní cíle a kroky sestavení jsou ignorovány. Například nástroje, jako je Fody, Refit, AutoFac a AutoMapper nemůže být zahrnut.
- Projekty F # nepodporují v Androidu a omezenou podporu pro iOS
- Pokročilé scénáře se službou vlastní obecné třídy a rozhraní, nemusí být podporované.

Ohlaste prosím všechny další problémy ve [bugzilla](https://aka.ms/live-player-report-issue).

## <a name="related-links"></a>Související odkazy

- [Odstraňování potíží](~/tools/live-player/troubleshooting.md)
- [Instalace a nastavení](~/tools/live-player/install.md)
