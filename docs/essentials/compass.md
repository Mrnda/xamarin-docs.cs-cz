---
title: 'Xamarin.Essentials: Compass'
description: Tento dokument popisuje třídy Compass v Xamarin.Essentials, což vám umožní monitorovat zařízení magnetickém severu záhlaví.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c3fe98c384a87bdc08ce94e7537d1a6343767561
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353880"
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
    SensorSpeed speed = SensorSpeed.UI;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(object sender, CompassChangedEventArgs e)
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
            // Some other exception has occurred
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

## <a name="low-pass-filter"></a>Filtr nízká

Z důvodu jak Android compass hodnoty jsou aktualizovány a počítá, může být potřeba vyhlazení hodnoty. A _předat filtr s nízkou_ lze použít, který zobrazí průměr hodnot sinus a kosinus úhlů a je možné zapnout tak, že nastavíte `ApplyLowPassFilter` vlastnost `Compass` třídy:

```csharp
Compass.ApplyLowPassFilter = true;
```

Použije se pouze pro platformu Android. Další informace lze číst [tady](https://github.com/xamarin/Essentials/pull/354#issuecomment-405316860).

--------------

## <a name="api"></a>rozhraní API

- [Compass zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Kompasu dokumentace k rozhraní API](xref:Xamarin.Essentials.Compass)
