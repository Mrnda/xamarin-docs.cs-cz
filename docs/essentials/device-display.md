---
title: 'Xamarin.Essentials: Informace o zobrazení zařízení'
description: Tento dokument popisuje třídy DeviceDisplay v Xamarin.Essentials, který poskytuje metriky obrazovky zařízení, na kterém je aplikace spuštěna.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3060d56e14fb0d3801a96ec0fe6e24c9efda4dac
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080310"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: Informace o zobrazení zařízení

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

**DeviceDisplay** třída taky zpřístupňuje na událost, která může přihlásit k odběru se aktivuje vždy, když všechny obrazovky metriky změny:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
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
- [Dokumentaci k rozhraní API DeviceDisplay](xref:Xamarin.Essentials.DeviceDisplay)
