---
title: 'Xamarin.Essentials: OrientationSensor'
description: Třída OrientationSensor vám umožní monitorovat orientace zařízení v trojrozměrném prostoru.
ms.assetid: F3091D93-E779-41BA-8696-23D296F2F6F5
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c01fa28e495eb3eceec62885060dce8f096c4086
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947387"
---
# <a name="xamarinessentials-orientationsensor"></a>Xamarin.Essentials: OrientationSensor

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**OrientationSensor** třída umožňuje monitorovat orientace zařízení do tří rozměrného prostoru.

> [!NOTE]
> Tato třída slouží k určení orientace zařízení v 3D prostoru. Pokud je potřeba určit, pokud zařízení uživatele videa zobrazení je v režimu na výšku nebo šířku, použijte `Orientation` vlastnost `ScreenMetrics` objekt k dispozici [ `DeviceDisplay` ](device-display.md) třídy.

## <a name="using-orientationsensor"></a>Pomocí OrientationSensor

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

`OrientationSensor` Je povoleno voláním `Start` metoda monitorovat změny orientace zařízení a zakázáno voláním `Stop` metody. Všechny změny jsou odesílány zpět prostřednictvím `ReadingChanged` událostí. Tady je příklad použití:

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

`OrientationSensor` měření jsou hlášeny ve formě [ `Quaternion` ](xref:System.Numerics.Quaternion) orientace zařízení založené na dvou 3D souřadnicových systémů, který popisuje:

V zařízení (obecně telefon nebo tablet) je 3D systém souřadnic s následující osy:

- Kladná X body osy napravo od zobrazení v režimu na výšku.
- Osu Y pozitivní odkazuje na začátku zařízení v režimu na výšku.
- Pozitivní osy Z bodů z obrazovky.

3D souřadnicový systém všech koutech světa má následující osy:

- Pozitivní osy X je tangens na povrchu země a bodů – východ.
- Na povrchu země a sever bodů tangenty je také pozitivní osy Y.
- Pozitivní osy je kolmé na povrchu země a body nahoru.

`Quaternion` Popisuje otočení souřadnicovém systému zařízení vzhledem k země souřadnicový systém.

A `Quaternion` hodnotu velmi úzce souvisí s otočení kolem osy. Pokud osu otáčení normalizovaný vektor (<sub>x</sub>,<sub>y</sub>,<sub>z</sub>), a je úhel otočení Θ, pak (X, Y, Z, W) jsou součástí quaternion:

(<sub>x</sub>·sin(Θ/2),<sub>y</sub>·sin(Θ/2),<sub>z</sub>·sin(Θ/2), cos(Θ/2))

Toto jsou pravém souřadnicových systémů, takže s thumb pravé straně, na kterou ukazuje ve směru kladné otočení osy, křivky prsty označují směr pro kladné hodnoty úhlu otočení.

Příklady:

* Pokud zařízení je plochý u tabulky s jeho obrazovky směřuje, horní zařízení (v režimu na výšku) – sever, odkazující je zarovnán dvěma systémy souřadnic. `Quaternion` Hodnota představuje quaternion identity (0, 0, 0, 1). Všechny rotace mohou být analyzovány vzhledem k této pozici.

* Pokud zařízení je plochý u tabulky s jeho obrazovka lícem nahoru a horní zařízení (v režimu na výšku) – Západ, odkazující `Quaternion` hodnotu (0, 0, 0.707, 0.707). Zařízení má byla otočenou o 90 stupňů kolem osy Z všech koutech světa.

* Pokud zařízení se nachází ve tak, že nahoře (v režimu na výšku) odkazuje na oblohy a sever čelí zpět zařízení, zařízení bylo otočenou o 90 stupňů kolem osy X. `Quaternion` Hodnotu (0.707, 0, 0, 0.707).

* Pokud zařízení je umístěn v tabulce, je jeho levé hrany, takže – sever, odkazuje na začátek zařízení otočený &ndash;90 stupňů okolo osy Y (neboli 90 stupňů kolem záporný osy Y). `Quaternion` Hodnotu (0,-0.707, 0, 0.707).

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>rozhraní API

- [OrientationSensor zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/OrientationSensor)
- [Dokumentace k rozhraní API OrientationSensor](xref:Xamarin.Essentials.OrientationSensor)
