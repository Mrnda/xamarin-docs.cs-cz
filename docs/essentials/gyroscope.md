---
title: 'Xamarin.Essentials: volný setrvačník'
description: Třída volný setrvačník v Xamarin.Essentials umožňuje monitorovat senzor volný setrvačník zařízení, která měří otočení přibližně tři primární osy v zařízení.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6b2df3b6c5061464a06730ad09cbbbfdbceeb247
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080385"
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials: volný setrvačník

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Volný setrvačník** třída umožňuje monitorovat senzor volný setrvačník zařízení, která je otočení přibližně tři primární osy v zařízení.

## <a name="using-gyroscope"></a>Pomocí volný setrvačník

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Volný setrvačník funkce funguje tak, že volání `Start` a `Stop` metody pro naslouchání volný setrvačník změny. Změny jsou odesílány zpět pomocí `ReadingChanged` událostí. Tady je využití vzorků:

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(GyroscopeChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Angular Velocity X, Y, and Z
        Console.WriteLine($"Reading: X: {data.AngularVelocity.X}, Y: {data.AngularVelocity.Y}, Z: {data.AngularVelocity.Z}");
    }

    public void ToggleGyroscope()
    {
        try
        {
            if (Gyroscope.IsMonitoring)
              Gyroscope.Stop();
            else
              Gyroscope.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Snímač rychlosti](xref:Xamarin.Essentials.SensorSpeed)

- **Nejrychlejší** – získat data snímačů co nejrychleji (není zaručena k vrácení při vlákna uživatelského rozhraní).
- **Herní** – míra vhodný pro hry (není zaručena k vrácení při vlákna uživatelského rozhraní).
- **Normální** – výchozí rychlost vhodný pro změny orientace obrazovky.
- **Uživatelské rozhraní** – míra vhodný pro obecné uživatelské rozhraní.

Pokud není zaručena vaší obslužné rutiny události pro spuštění na vlákna uživatelského rozhraní a pokud obslužné rutiny události musí pro přístup k elementům uživatelského rozhraní, použijte [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) metodu pro spuštění tohoto kódu ve vlákně UI.

## <a name="api"></a>rozhraní API

- [Volný setrvačník zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Dokumentace volný setrvačník rozhraní API](xref:Xamarin.Essentials.Gyroscope)
