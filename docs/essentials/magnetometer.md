---
title: 'Xamarin.Essentials: Magnetometer'
description: Třída Magnetometer v Xamarin.Essentials vám umožní monitorovat senzor magnetometer zařízení označující orientace zařízení vzhledem k země na východozápadní ose magnetické pole.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3827b9a57ec2667a8716f5b56bfb4631b979d43a
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353786"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials: Magnetometer

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Magnetometer** třída umožňuje monitorovat zařízení magnetometer senzor, který označuje orientace zařízení vzhledem k země na východozápadní ose magnetické pole.

## <a name="using-magnetometer"></a>Pomocí Magnetometer

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Funkce Magnetometer funguje tak, že volání `Start` a `Stop` metody tak, aby naslouchala magnetometer se změny. Všechny změny jsou odesílány zpět prostřednictvím `ReadingChanged` událostí. Tady je ukázkový používání:

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometer_ReadingChanged(object sender, MagnetometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process MagneticField X, Y, and Z
        Console.WriteLine($"Reading: X: {data.MagneticField.X}, Y: {data.MagneticField.Y}, Z: {data.MagneticField.Z}");
    }

    public void ToggleMagnetometer()
    {
        try
        {
            if (Magnetometer.IsMonitoring)
              Magnetometer.Stop();
            else
              Magnetometer.Start(speed);
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

Vrátí se všechna data v microteslas.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>rozhraní API

- [Magnetometer zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Magnetometer)
- [Dokumentace ke službě magnetometer rozhraní API](xref:Xamarin.Essentials.Magnetometer)
