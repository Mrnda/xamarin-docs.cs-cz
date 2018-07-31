---
title: 'Xamarin.Essentials: baterie'
description: Tento dokument popisuje třídy baterie v Xamarin.Essentials, který umožňuje zkontrolovat informace o baterie zařízeních a monitorujte změny.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1deafed85e9400bf7d4592fc06f71c22cc0015f0
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353451"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: baterie

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Baterie** třída umožňuje zkontrolovat informace o bateriích a monitorujte změny zařízení.

## <a name="getting-started"></a>Začínáme

Pro přístup **baterie** funkce je následující nastavení konkrétní platformy se vyžaduje.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`Battery` Oprávnění je povinné a musí být nakonfigurovány v projektu pro Android. Jde přidat následujícími způsoby:

Otevřít **AssemblyInfo.cs** soubor **vlastnosti** složky a přidejte:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.BatteryStats)]
```

NEBO aktualizovat Android Manifest:

Otevřít **AndroidManifest.xml** soubor **vlastnosti** složky a přidejte následující uvnitř **manifest** uzlu.

```xml
<uses-permission android:name="android.permission.BATTERY_STATS" />
```

Nebo klikněte pravým tlačítkem na projekt pro Android a otevřete vlastnosti projektu. V části **Manifest v Androidu** najít **požadovaná oprávnění:** oblasti a kontrolu **baterie** oprávnění. Tím se automaticky aktualizují **AndroidManifest.xml** souboru.

# <a name="iostabios"></a>[iOS](#tab/ios)

Není požadováno žádné další nastavení.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

Není požadováno žádné další nastavení.

-----

## <a name="using-battery"></a>Používání baterie

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Zkontrolujte informace o aktuálním baterie:

```csharp
var level = Battery.ChargeLevel; // returns 0.0 to 1.0 or -1.0 if unable to determine.

var state = Battery.State;

switch (state)
{
    case BatteryState.Charging:
        // Currently charging
        break;
    case BatteryState.Full:
        // Battery is full
        break;
    case BatteryState.Discharging:
    case BatteryState.NotCharging:
        // Currently discharging battery or not being charged
        break;
    case BatteryState.NotPresent:
        // Battery doesn't exist in device (desktop computer)
    case BatteryState.Unknown:
        // Unable to detect battery state
        break;
}

var source = Battery.PowerSource;

switch (source)
{
    case BatteryPowerSource.Battery:
        // Being powered by the battery
        break;
    case BatteryPowerSource.AC:
        // Being powered by A/C unit
        break;
    case BatteryPowerSource.Usb:
        // Being powered by USB cable
        break;
    case BatteryPowerSource.Wireless:
        // Powered via wireless charging
        break;
    case BatteryPowerSource.Unknown:
        // Unable to detect power source
        break;
}
```

Při každé změně vlastnosti baterie se aktivuje událost:

```csharp
public class BatteryTest
{
    public BatteryTest()
    {
        // Register for battery changes, be sure to unsubscribe when needed
        Battery.BatteryChanged += Battery_BatteryChanged;
    }

    void Battery_BatteryChanged(BatteryChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

## <a name="platform-differences"></a>Rozdíly pro tyto platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

Žádné rozdíly pro tyto platformy.

# <a name="iostabios"></a>[iOS](#tab/ios)

* Zařízení musí použít k testování rozhraní API. 
* Pouze vrátí `AC` nebo `Battery` pro `PowerSource`.
* Není možné zrušit pronikavost.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

* Pouze vrátí `AC` nebo `Battery` pro `PowerSource`.

-----

## <a name="api"></a>rozhraní API

- [Baterie zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Dokumentace k rozhraní API baterie](xref:Xamarin.Essentials.Battery)
