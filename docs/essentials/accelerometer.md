---
title: Xamarin.Essentials zrychlení
description: Třída zrychlení umožňuje monitorovat senzor zrychlení v zařízení, označující akcelerace zařízení za tři dimenzí místa na disku.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: bb62ad438c2db906af112322174656bc62740cbc
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials zrychlení

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Zrychlení** třída umožňuje monitorovat senzor zrychlení v zařízení, označující akcelerace zařízení za tři dimenzí místa na disku.

## <a name="using-accelerometer"></a>Pomocí zrychlení

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Funkce zrychlení funguje tak, že volání `Start` a `Stop` metody pro naslouchání změny zrychlení. Změny jsou odesílány zpět pomocí `ReadingChanged` událostí. Tady je využití vzorků:

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(AccelerometerChangedEventArgs e)
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

Zrychlení odečty nejsou hlášeny v G. G je jednotka gravitation vynutit rovna hodnoty, kterou zemském povrchu gravitační pole (9.81 m/s ^ 2).

Systém souřadnic je definována relativně k obrazovce telefonu v jeho výchozí orientace. Když se změní orientace obrazovky zařízení, nejsou vzájemně zaměněny osy.

Je vodorovné osy X a body vpravo osy Y je svislý a bodů a ukazuje osy Z směrem od čelní obrazovky. V tomto systému souřadnice za obrazovky mít záporné hodnoty Z.

Příklady:

* Když je zařízení je nestrukturované v tabulce a se instaluje na jeho levé straně směrem doprava, je kladné hodnoty zrychlení x.

* Pokud zařízení je nestrukturované v tabulce, hodnoty zrychlení je +1.00 G nebo (+ 9.81 m/s ^ 2), které odpovídají akcelerace zařízení (0 m/s ^ 2) minus platnost gravitace (-9.81 m/s ^ 2) a normalizovaný jako G.

* Pokud zařízení je nestrukturované v tabulce a je přesunuty směrem k sky s zrychlení m/s ^ 2, akcelerace hodnota se rovná + 9.81, které odpovídají akcelerace zařízení (+ m/s ^ 2) minus platnost gravitace (-9.81 m/s ^ 2) a normalizovaný v G. 

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Snímač rychlosti](xref:Xamarin.Essentials.SensorSpeed)

- **Nejrychlejší** – získat data snímačů co nejrychleji (není zaručena k vrácení při vlákna uživatelského rozhraní).
- **Herní** – míra vhodný pro hry (není zaručena k vrácení při vlákna uživatelského rozhraní).
- **Normální** – výchozí rychlost vhodný pro změny orientace obrazovky.
- **Uživatelské rozhraní** – míra vhodný pro obecné uživatelské rozhraní.

## <a name="api"></a>rozhraní API

- [Zrychlení zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Dokumentace zrychlení rozhraní API](xref:Xamarin.Essentials.Accelerometer)
