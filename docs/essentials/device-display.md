---
title: 'Xamarin.Essentials: Informace o zobrazení zařízení'
description: Tento dokument popisuje třídy DeviceDisplay v Xamarin.Essentials, který poskytuje metriky obrazovky pro zařízení, na kterém je aplikace spuštěna.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cb42da4c8c2d0e381a5b00f7e60da6f427d19c66
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353825"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: Informace o zobrazení zařízení

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**DeviceDisplay** třída poskytuje informace o metrikách obrazovky zařízení, aplikace běží.

## <a name="using-devicedisplay"></a>Pomocí DeviceDisplay

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Metriky obrazovky

Kromě informací o základní zařízení **DeviceDisplay** třída obsahuje informace o orientace a obrazovky zařízení.

```csharp
// Get Metrics
var metrics = DeviceDisplay.ScreenMetrics;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = metrics.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = metrics.Rotation;

// Width (in pixels)
var width = metrics.Width;

// Height (in pixels)
var height = metrics.Height;

// Screen density
var density = metrics.Density;
```

**DeviceDisplay** třída také zveřejňuje událost, která se můžou přihlásit k odběru, který se aktivuje vždy, když všechny obrazovky metriky změny:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChangedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>rozhraní API

- [DeviceDisplay zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [Dokumentace k rozhraní API DeviceDisplay](xref:Xamarin.Essentials.DeviceDisplay)
