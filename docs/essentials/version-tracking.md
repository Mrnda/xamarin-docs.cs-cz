---
title: Verze Xamarin.Essentials sledování
description: Třída VersionTracking umožňuje zkontrolujte verzi aplikace a čísla sestavení spolu se zobrazuje tyto další informace, jako by se jednalo o první čas spuštění někdy aplikace nebo pro aktuální verzi, získat předchozí informace o sestavení a další.
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f6ab63c44307fca860ccb73744b35c006f25a9ed
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-version-tracking"></a>Verze Xamarin.Essentials sledování

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**VersionTracking** třída umožňuje zkontrolujte verzi aplikace a čísla sestavení spolu se zobrazuje tyto další informace, jako by se jednalo o první čas spuštění někdy aplikace nebo pro aktuální verzi, získat předchozí informace o sestavení a další.

## <a name="using-version-tracking"></a>Pomocí sledování verze

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Při prvním použití **VersionTracking** třída zahájí sledování aktuální verze. Je třeba volat `Track` časná pouze ve vaší aplikaci pokaždé, když je načten zajistit sledován aktuální informace o verzi:

```csharp
VersionTracking.Track();
```

Po počáteční `Track` nazývá můžou číst informace o verzi:

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

## <a name="platform-implementation-specifics"></a>Podrobnosti implementace platformy

Ukládají se všechny verze informace, pomocí [Předvolby](preferences.md) rozhraní API v Xamarin.Essentials a je uložen s názvem souboru z **[YOUR-balíček-ID aplikace] .xamarinessentials**.

Odinstalace aplikace způsobí, že _LocalSettings_a všechny verze sledování informace, které mají být odebrány.

## <a name="api"></a>rozhraní API

- [Verze sledování zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/VersionTracking)
- [Verze API pro sledování dokumentace](xref:Xamarin.Essentials.VersionTracking)
