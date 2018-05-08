---
title: Xamarin.Essentials Magnetometer
description: Třída Magnetometer umožňuje monitorovat senzor magnetometer zařízení, označující orientace zařízení relativně k země magnetické pole.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: e43834756e6a582bd0fd30a5655da8d087a971b7
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials Magnetometer

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Magnetometer** třída umožňuje monitorovat senzor magnetometer zařízení, označující orientace zařízení relativně k země magnetické pole.

## <a name="using-magnetometer"></a>Pomocí Magnetometer

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Funkce Magnetometer funguje tak, že volání `Start` a `Stop` metody pro naslouchání magnetometer změny. Změny jsou odesílány zpět pomocí `ReadingChanged` událostí. Tady je využití vzorků:

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometerr_ReadingChanged(MagnetometerChangedEventArgs e)
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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Snímač rychlosti](xref:Xamarin.Essentials.SensorSpeed)

- **Nejrychlejší** – získat data snímačů co nejrychleji (není zaručena k vrácení při vlákna uživatelského rozhraní).
- **Herní** – míra vhodný pro hry (není zaručena k vrácení při vlákna uživatelského rozhraní).
- **Normální** – výchozí rychlost vhodný pro změny orientace obrazovky.
- **Uživatelské rozhraní** – míra vhodný pro obecné uživatelské rozhraní.

## <a name="api"></a>rozhraní API

- [Magnetometer zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Essentials/Magnetometer)
- [Dokumentace magnetometer rozhraní API](xref:Xamarin.Essentials.Magnetometer)
