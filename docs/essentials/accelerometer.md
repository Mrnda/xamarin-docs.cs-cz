---
title: 'Xamarin.Essentials: akcelerometru'
description: Třída akcelerometr v Xamarin.Essentials vám umožní monitorovat senzor akcelerometr zařízení, který označuje zrychlení zařízení do tří rozměrného prostoru.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b5a24e214eb129b4d53b94586632791c8827447b
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353838"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials: akcelerometru

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Akcelerometr** třída umožňuje monitorovat zařízení akcelerometr senzor, který označuje zrychlení zařízení do tří rozměrného prostoru.

## <a name="using-accelerometer"></a>Pomocí akcelerometru

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Funguje akcelerometru funkce voláním `Start` a `Stop` metody k naslouchání pro zrychlení. Všechny změny jsou odesílány zpět prostřednictvím `ReadingChanged` událostí. Tady je ukázkový používání:

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(object sender, AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Acceleration.X}, Y: {data.Acceleration.Y}, Z: {data.Acceleration.Z}");
        // Process Acceleration X, Y, and Z
    }

    public void ToggleAcceleromter()
    {
        try
        {
            if (Accelerometer.IsMonitoring)
              Accelerometer.Stop();
            else
              Accelerometer.Start(speed);
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

Akcelerometr odečty jsou hlášeny v G. G je jednotka gravitation vynutit rovna hodnotě vyvíjený gravitační pole země (9,81 m/s ^ 2).

Systém souřadnic je definována relativně vzhledem k obrazovce telefonu v její výchozí orientace. Při změně orientace obrazovky zařízení nejsou prohodily OS.

Je vodorovné osy X a odkazuje na pravé straně, osy Y je svislý odkazuje a odkazuje na ose Z ven přední plochy obrazovky. V tomto systému je za obrazovce mají záporné hodnoty Z.

Příklady:

* Když zařízení je plochý v tabulce a posunuto v jeho levé směrem doprava, zrychlení hodnota x je kladný.

* Když zařízení je plochý u tabulky, je hodnota akcelerace +1.00 G nebo (+ 9,81 m/s ^ 2), které odpovídají zrychlení zařízení (0 m/s ^ 2) minus platnost závažnosti (-9,81 m/s ^ 2) a normalizované jako např.

* Kdy se zařízení je plochý v tabulce a vloženy směrem k sky s zrychlení m/s ^ 2, hodnota akcelerace je rovna 9.81 A +, které odpovídají zrychlení zařízení (+ m/s ^ 2) minus platnost závažnosti (-9,81 m/s ^ 2) a normalizované v G.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>rozhraní API

- [Akcelerometr zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Dokumentace ke službě akcelerometr rozhraní API](xref:Xamarin.Essentials.Accelerometer)
