---
title: 'Xamarin.Essentials: stav baterie.'
description: Tento dokument popisuje třídy baterie v Xamarin.Essentials, který vám umožňuje zkontrolovat informace o baterie zařízení a monitorujte změny.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 35764b4c2270359a7c010e1186f882e236e17fd5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782083"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: stav baterie.

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Baterie** třída umožňuje zkontrolovat stav baterie informace a monitorujte změny zařízení.

## <a name="getting-started"></a>Začínáme

Abyste měli přístup **baterie** funkce následující nastavení konkrétní platformy není třeba.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`Battery` Oprávnění je povinná a musí být konfigurované v projektu pro Android. To lze přidat následujícími způsoby:

Otevřete **AssemblyInfo.cs** souboru pod **vlastnosti** složky a přidat:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Battery)]
```

NEBO aktualizovat manifestu systému Android.:

Otevřete **AndroidManifest.xml** souboru pod **vlastnosti** složky a přidejte následující uvnitř **manifest** uzlu.

```xml
<uses-permission android:name="android.permission.BATTERY" />
```

Nebo klikněte pravým tlačítkem na projekt Anroid a otevřete vlastnosti projektu. V části **Android Manifest** najít **požadovaná oprávnění:** oblasti a kontroly **baterie** oprávnění. Tím se automaticky aktualizuje **AndroidManifest.xml** souboru.

# <a name="iostabios"></a>[iOS](#tab/ios)

Nevyžaduje žádné další nastavení.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Nevyžaduje žádné další nastavení.

-----

## <a name="using-battery"></a>Pomocí stav baterie.

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Zkontrolujte aktuální stav baterie informace:

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
    case BatteryPowerSource.Ac:
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

Při každé změně jakékoli vlastnosti baterie se aktivuje událost:

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

## <a name="platform-differences"></a>Rozdíly platformy

| Platforma | Rozdíl |
| --- | --- |
| iOS | Zařízení musí být použít k testování rozhraní API. |
| iOS | Pouze vrátí Ac nebo baterie pro PowerSource. |
| UWP | Pouze vrátí Ac nebo baterie pro PowerSource. |

## <a name="api"></a>rozhraní API

- [Baterie zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Dokumentaci k rozhraní API stav baterie.](xref:Xamarin.Essentials.Battery)
