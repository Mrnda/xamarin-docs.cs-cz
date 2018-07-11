---
title: 'Xamarin.Essentials: Compass'
description: Tento dokument popisuje třídy Compass v Xamarin.Essentials, což vám umožní monitorovat zařízení magnetickém severu záhlaví.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cf41948c55c742140896bfb48d9bb4abf25c8d68
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947410"
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials: Compass

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Compass** třída umožňuje monitorovat zařízení magnetickém severu záhlaví.

## <a name="using-compass"></a>Pomocí Compass

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Funkce Compass funguje tak, že volání `Start` a `Stop` metody pro naslouchání změn kompasu. Všechny změny jsou odesílány zpět prostřednictvím `ReadingChanged` událostí. Tady je příklad:

```csharp
public class CompassTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(CompassChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: {data.HeadingMagneticNorth} degrees");
        // Process Heading Magnetic North
    }

    public void ToggleCompass()
    {
        try
        {
            if (Compass.IsMonitoring)
              Compass.Stop();
            else
              Compass.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Some other exception has occured
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>Specifika platforem implementace

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android neposkytuje o rozhraní API pro načítání daný kompasem. Můžeme využívat akcelerometr a magnetometer k výpočtu záhlaví magnetickém severu, které se doporučuje Google. 

Ve výjimečných případech možná uvidíte nekonzistentní výsledky protože snímačům muset být kalibrován, což zahrnuje přesunutí zařízení přenášená data obrázku 8. Nejlepší způsob, jak to jde k otevření map Google, klepněte na tečku pro vaši polohu a vybrat **kalibrovat compass**.

Mějte na paměti systémem řadu senzorů z vaší aplikace ve stejnou dobu může upravit snímač rychlosti.

--------------

## <a name="api"></a>rozhraní API

- [Compass zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Kompasu dokumentace k rozhraní API](xref:Xamarin.Essentials.Compass)
