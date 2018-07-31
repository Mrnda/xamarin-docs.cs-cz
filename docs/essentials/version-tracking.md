---
title: 'Xamarin.Essentials: Sledování verze'
description: Třída VersionTracking v Xamarin.Essentials umožňuje zkontrolovat verzi aplikace a čísla sestavení spolu se zobrazuje další informace, jako by se jednalo o první čas někdy spustili aplikaci nebo aktuální verzi, a získat předchozí sestavení informace a další.
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 81dc67fa5a4975f31d0fbf9f7219637596a827ce
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353656"
---
# <a name="xamarinessentials-version-tracking"></a>Xamarin.Essentials: Sledování verze

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**VersionTracking** třída umožňuje zkontrolovat verzi aplikace a čísla sestavení spolu se zobrazuje další informace, jako by se jednalo o první čas někdy spustili aplikaci nebo pro aktuální verzi získat předchozí informace o sestavení a další.

## <a name="using-version-tracking"></a>Použití verze sledování

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Při prvním použití **VersionTracking** třídy, které se spustí, aktuální verzi sledování. Je nutné volat `Track` předčasné pouze ve vaší aplikaci pokaždé, když je načten do zkontrolujte aktuální verzi informace jsou sledovány:

```csharp
VersionTracking.Track();
```

Po počáteční `Track` nazývá najdete informace o verzi:

```csharp

// First time ever launched application
var firstLaunch = VersionTracking.IsFirstLaunchEver;

// First time launching current version
var firstLaunchCurrent = VersionTracking.IsFirstLaunchForCurrentVersion;

// First time launching current build
var firstLaunchBuild = VersionTracking.IsFirstLaunchForCurrentBuild;

// Current app version (2.0.0)
var currentVersion = VersionTracking.CurrentVersion;

// Current build (2)
var currentBuild = VersionTracking.CurrentBuild;

// Previous app version (1.0.0)
var previousVersion = VersionTracking.PreviousVersion;

// Previous app build (1)
var previousBuild = VersionTracking.PreviousBuild;

// First version of app installed (1.0.0)
var firstVersion = VersionTracking.FirstInstalledVersion;

// First build of app installed (1)
var firstBuild = VersionTracking.FirstInstalledBuild;

// List of versions installed (1.0.0, 2.0.0)
var versionHistory = VersionTracking.VersionHistory;

// List of builds installed (1, 2)
var buildHistory = VersionTracking.BuildHistory;
```

## <a name="platform-implementation-specifics"></a>Specifika platforem implementace

Všechny informace o verzi je uložen pomocí [Předvolby](preferences.md) rozhraní API v Xamarin.Essentials a je uložen s názvem souboru z **.xamarinessentials.versiontracking [YOUR-APP-– ID balíčku]** a dodržovat stejné trvalost dat uvedených v [Předvolby](preferences.md#persistence) dokumentaci.

## <a name="api"></a>rozhraní API

- [Verze sledování zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/VersionTracking)
- [Verze rozhraní API pro sledování dokumentace](xref:Xamarin.Essentials.VersionTracking)
