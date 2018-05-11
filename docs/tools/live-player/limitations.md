---
title: Omezení
description: Některá omezení Xamarin Live Player
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: a3aea31f32e3bf6227cce2e3340e3d04828472a0
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="limitations"></a>Omezení

![Funkce ve verzi Preview](~/media/shared/preview.png)

## <a name="device-requirements"></a>Požadavky na zařízení
Aplikace Xamarin Live Player podporuje následující zařízení:

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Android 4.2 nebo novější.
- ARM – v7a, ARM v8a, ARM64-v8a, x 86 nebo x86_64 procesoru.

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 nebo novější.
- ARM64 procesoru.

-----

## <a name="limitations"></a>Omezení

Existují některá omezení akcí, které můžete spustit Xamarin Live Player, včetně následujících položek:

### <a name="xamarinforms"></a>Xamarin.Forms
- Vlastní nástroji pro vykreslování nejsou podporovány.
- Účinky nejsou podporovány.
- Vlastní ovládací prvky s vlastní vazbu vlastnosti nejsou podporovány.
- Vložené prostředky nejsou podporovány (ie. vložení obrázků nebo jiným prostředkům v PCL).
- Rozhraní modelem MVVM třetích stran nejsou podporovány (tj. Modulu PRISM mezi modelem Mvvm, rozhraní Mvvm lehký, atd.).
- Katalog Asset v systému iOS nejsou podporovány.

### <a name="other-project-types"></a>Typy projektu
- Za provozu Player není určen pro nativní Android nebo iOS projekty, (které používají Android XML nebo scénářů pro uživatelské rozhraní).

### <a name="misc"></a>Různé
- Omezenou podporu pro reflexi (ovlivňuje aktuálně některých oblíbených NuGets, jako je SQLite a Json.NET). Další NuGets může být stále podporovány.
- Nebylo možné přepsat některé třídy systému (například nemůžete implementovat podtřídy).
- Některé funkce platformy, které vyžadují zřizování nemůže pracovat v aplikaci Xamarin Live Player (ale byla nakonfigurována pro běžné operace, jako je přístup Galerie fotografií).
- Vlastní cíle a kroků sestavení se ignorují. Například nástroje, například Fody, Refit, AutoFac a AutoMapper nelze vložit.
- F # projekty nejsou podporované v systému Android a omezenou podporu v systému iOS
- Složitější scénáře s vlastní obecné třídy a rozhraní nemusí být podporována.

Nahlaste všechny další problémy na [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Související odkazy

- [Odstraňování potíží](~/tools/live-player/troubleshooting.md)
- [Instalace a nastavení](~/tools/live-player/install.md)
