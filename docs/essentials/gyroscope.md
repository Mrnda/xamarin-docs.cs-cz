---
title: 'Xamarin.Essentials: volný setrvačník'
description: Třída volný setrvačník v Xamarin.Essentials vám umožní monitorovat senzor volný setrvačník zařízení, která měří otočení kolem tři primární osy zařízení.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f1e1199ae32158889ec569eb5f7e9742f37d45d4
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353623"
---
# <a name="xamarinessentials-gyroscope"></a>Xamarin.Essentials: volný setrvačník

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Volný setrvačník** třída umožňuje monitorovat senzor volný setrvačník zařízení, což je otočení kolem tři primární osy zařízení.

## <a name="using-gyroscope"></a>Pomocí volný setrvačník

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Funkce volný setrvačník funguje tak, že volání `Start` a `Stop` metody tak, aby naslouchala volný setrvačník se změny. Všechny změny jsou odesílány zpět prostřednictvím `ReadingChanged` událostí. Tady je ukázkový používání:

```csharp

public class GyroscopeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public GyroscopeTest()
    {
        // Register for reading changes.
        Gyroscope.ReadingChanged += Gyroscope_ReadingChanged;
    }

    void Gyroscope_ReadingChanged(object sender, GyroscopeChangedEventArgs e)
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

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>rozhraní API

- [Volný setrvačník zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Gyroscope)
- [Dokumentace ke službě volný setrvačník rozhraní API](xref:Xamarin.Essentials.Gyroscope)
