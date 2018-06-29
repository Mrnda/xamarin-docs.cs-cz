---
title: 'Xamarin.Essentials: Magnetometer'
description: Třída Magnetometer v Xamarin.Essentials umožňuje monitorovat senzor magnetometer zařízení, označující orientace zařízení relativně k země magnetické pole.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2c02188b5282949559e0abc5fa1b61b6b451fc8e
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080375"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials: Magnetometer

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

Pokud není zaručena vaší obslužné rutiny události pro spuštění na vlákna uživatelského rozhraní a pokud obslužné rutiny události musí pro přístup k elementům uživatelského rozhraní, použijte [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) metodu pro spuštění tohoto kódu ve vlákně UI.

## <a name="api"></a>rozhraní API

- [Magnetometer zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Magnetometer)
- [Dokumentace magnetometer rozhraní API](xref:Xamarin.Essentials.Magnetometer)
