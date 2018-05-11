---
title: Volný setrvačník Xamarin.Essentials
description: Třída volný setrvačník umožňuje monitorovat senzor volný setrvačník zařízení, která je otočení přibližně tři primární osy v zařízení.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 0fa6ed6a0ced97c2600a24860c4f42aee1a24161
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-gyroscope"></a>Volný setrvačník Xamarin.Essentials

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

## <a name="api"></a>rozhraní API

- [Volný setrvačník zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Essentials/Gyroscope)
- [Dokumentace volný setrvačník rozhraní API](xref:Xamarin.Essentials.Gyroscope)
