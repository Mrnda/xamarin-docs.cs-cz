---
title: Xamarin.Essentials kompas
description: Třída kompas umožňuje monitorovat zařízení – magnetická sever záhlaví.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 39f141424ddd247458b9c8b35ae02ab29e2c2206
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-compass"></a>Xamarin.Essentials kompas

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Kompas** třída umožňuje monitorovat zařízení – magnetická sever záhlaví.

## <a name="using-compass"></a>Pomocí kompas

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Funkce kompas funguje tak, že volání `Start` a `Stop` metody pro naslouchání změny kompas. Změny jsou odesílány zpět pomocí `ReadingChanged` událostí. Tady je příklad:

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

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Snímač rychlosti](xref:Xamarin.Essentials.SensorSpeed)

- **Nejrychlejší** – získat data snímačů co nejrychleji (není zaručena k vrácení při vlákna uživatelského rozhraní).
- **Herní** – míra vhodný pro hry (není zaručena k vrácení při vlákna uživatelského rozhraní).
- **Normální** – výchozí rychlost vhodný pro změny orientace obrazovky.
- **Uživatelské rozhraní** – míra vhodný pro obecné uživatelské rozhraní.

## <a name="platform-implementation-specifics"></a>Podrobnosti implementace platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android neposkytuje rozhraní API pro načítání kompasu záhlaví. Můžeme využívat zrychlení a magnetometer k výpočtu záhlaví magnetická – sever, což je doporučeno Google. 

Ve výjimečných případech může být zobrazí nekonzistentním výsledkům protože snímače muset zkalibrovat, což zahrnuje přesun zařízení pohybem obrázek 8. Nejlepší způsob, jak to jde otevřete službu mapy Google, klepněte na tečky pro vaše umístění a vyberte **kalibrovat kompas**.

Mějte na paměti systémem více senzorů z vaší aplikace ve stejnou dobu může upravit snímač rychlosti.

--------------

## <a name="api"></a>rozhraní API

- [Kompas zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Kompasu dokumentaci k rozhraní API](xref:Xamarin.Essentials.Compass)
