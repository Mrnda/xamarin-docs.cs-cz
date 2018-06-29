---
title: 'Xamarin.Essentials: OrientationSensor'
description: Třída OrientationSensor umožňuje monitorovat orientaci zařízení v trojrozměrné prostoru.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c7bbc849e5fa0b901f5b54e77d548b28bc2a72c6
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080377"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**OrientationSensor** třída umožňuje monitorovat orientaci zařízení do tří dimenzí místa.

> [!NOTE]
> Tato třída je pro určení orientaci zařízení v 3D prostoru. Pokud potřebujete zjistit, pokud je zařízení grafické zobrazení je v režimu na výšku nebo na šířku, použijte `Orientation` vlastnost `ScreenMetrics` objekt k dispozici z [ `DeviceDisplay` ](device-display.md) třídy.

## <a name="using-orientationsensor"></a>Pomocí OrientationSensor

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

`OrientationSensor` Je povoleno voláním `Start` sledovat změny orientace zařízení a zakázáno voláním metody `Stop` metoda. Změny jsou odesílány zpět pomocí `ReadingChanged` událostí. Tady je využití vzorků:

```csharp

public class OrientationSensorTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public OrientationSensorTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        OrientationSensor.ReadingChanged += OrientationSensor_ReadingChanged;
    }

    void OrientationSensor_ReadingChanged(AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Orientation.X}, Y: {data.Orientation.Y}, Z: {data.Orientation.Z}, W: {data.Orientation.W}");
        // Process Orientation quaternion (X, Y, Z, and W)
    }

    public void ToggleOrientationSensor()
    {
        try
        {
            if (OrientationSensor.IsMonitoring)
                OrientationSensor.Stop();
            else
                OrientationSensor.Start(speed);
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

`OrientationSensor` odečty nejsou hlášeny ve formě [ `Quaternion` ](xref:System.Numerics.Quaternion) orientaci zařízení založené na dva 3D systém souřadnic, který popisuje:

Zařízení (obvykle telefon nebo tablet) má 3D systém souřadnic s ose následující:

- Kladná X ukazuje osy napravo od zobrazení v režimu na výšku.
- Kladné osy Y odkazuje na horní části zařízení v režimu na výšku.
- Kladné osy Z bodů z obrazovky.

3D systém souřadnic zemském povrchu má následující osy:

- Kladnou osy X je tangens na povrch zemském povrchu a bodů – východ.
- Je také tečný na povrchu země a body severní kladné osy Y.
- Kladné osy Z je kolmé na povrchu země a body nahoru.

`Quaternion` Popisuje oběh souřadný systém zařízení je relativní vzhledem k zemském povrchu souřadnicový systém.

A `Quaternion` hodnota je velmi úzce související Otočení okolo osy. Pokud otočení osy vector normalizovaný (<sub>x</sub>,<sub>y</sub>,<sub>z</sub>), a je úhel otočení Θ, pak (X, Y, Z, W) jsou součástí quaternion:

(<sub>x</sub>·sin(Θ/2),<sub>y</sub>·sin(Θ/2),<sub>z</sub>·sin(Θ/2), cos(Θ/2))

Toto jsou pravém systém souřadnic, tak s úchytu pravém odkazoval ve směru kladné otočení osy, křivku prsty, které indikují směru pro kladné hodnoty úhlu.

Příklady:

* Pokud zařízení je nestrukturované v tabulce s jeho obrazovky čelí, s horní části zařízení (v režimu na výšku) odkazující sever, je zarovnán dvě systém souřadnic. `Quaternion` Hodnota představuje quaternion identity (0, 0, 0, 1). Všechny otočení lze analyzovat relativně k této pozice.

* Když je zařízení nestrukturované v tabulce s jeho obrazovka směřující a horní části zařízení (v režimu na výšku) – Západ, odkazující `Quaternion` hodnota je (0, 0, 0.707, 0.707). Zařízení má byla otočený o 90 stupňů kolem osy Z zemském povrchu.

* Když zařízení držena výšku tak, aby horní (v režimu na výšku) ukazuje k sky a zadní straně zařízení, kterým čelí Severní, zařízení bylo otočený o 90 stupňů okolo osy X. `Quaternion` Hodnota je (0.707, 0, 0, 0.707).

* Pokud zařízení je nastavený tak, aby se jeho levé hrany v tabulce a horní body sever, zařízení otočený &ndash;90 stupňů okolo osy Y (nebo 90 stupňů okolo záporné osy Y). `Quaternion` Hodnota je (0,-0.707, 0, 0.707).

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Snímač rychlosti](xref:Xamarin.Essentials.SensorSpeed)

- **Nejrychlejší** – získat data snímačů co nejrychleji (není zaručena k vrácení při vlákna uživatelského rozhraní).
- **Herní** – míra vhodný pro hry (není zaručena k vrácení při vlákna uživatelského rozhraní).
- **Normální** – výchozí rychlost vhodný pro změny orientace obrazovky.
- **Uživatelské rozhraní** – míra vhodný pro obecné uživatelské rozhraní.

Pokud není zaručena vaší obslužné rutiny události pro spuštění na vlákna uživatelského rozhraní a pokud obslužné rutiny události musí pro přístup k elementům uživatelského rozhraní, použijte [ `MainThread.BeginInvokeOnMainThread` ](main-thread.md) metodu pro spuštění tohoto kódu ve vlákně UI.

## <a name="api"></a>rozhraní API

- [OrientationSensor zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [Dokumentaci k rozhraní API OrientationSensor](xref:Xamarin.Essentials.OrientationSensor)
