---
title: Informace o zobrazení Xamarin.Essentials zařízení
description: Třída DeviceDisplay poskytuje informace o zařízení obrazovky metriky aplikace běží na.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 05701ff2bc9fceac8a0a490989e52d0327079d46
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-device-display-information"></a>Informace o zobrazení Xamarin.Essentials zařízení

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**DeviceDisplay** třída poskytuje informace o zařízení metriky obrazovky je aplikace spuštěna.

## <a name="using-devicedisplay"></a>Pomocí DeviceDisplay

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Metriky obrazovky

Kromě informací o základní zařízení **DeviceDisplay** třída obsahuje informace o obrazovky a orientace zařízení.

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

**DeviceDisplay** třídy taky zpřístupňuje a událost, která může přihlásit k odběru, která spustí událost vždy, když všechny obrazovky metriky změny:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChanagedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>rozhraní API

- [DeviceDisplay zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [Dokumentaci k rozhraní API DeviceDisplay](xref:Xamarin.Essentials.DeviceDisplay)