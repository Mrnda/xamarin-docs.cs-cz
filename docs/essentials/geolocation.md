---
title: 'Xamarin.Essentials: informace o zeměpisné poloze'
description: Tento dokument popisuje informace o zeměpisné poloze třídy v Xamarin.Essentials, které poskytuje rozhraní API k načtení souřadnice aktuální informace o zeměpisné poloze zařízení.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 11749107403fc99e1d49b63ee3b50ff105abaa57
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848748"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials: informace o zeměpisné poloze

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Informace o zeměpisné poloze** třída poskytuje rozhraní API k načtení souřadnice aktuální informace o zeměpisné poloze zařízení.

## <a name="getting-started"></a>Začínáme

Pro přístup **informace o zeměpisné poloze** funkce, která je následující nastavení specifické pro platformu je nutné:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Oprávnění hrubé a bez problémů umístění jsou povinné a musí být nakonfigurovány v projektu pro Android. Navíc pokud vaše aplikace cílí na Android 5.0 (úroveň rozhraní API 21) nebo vyšší, musí deklarovat, že vaše aplikace používá funkce hardwaru v souboru manifestu. Jde přidat následujícími způsoby:

Otevřít **AssemblyInfo.cs** soubor **vlastnosti** složky a přidejte:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

Nebo aktualizovat Android manifest:

Otevřít **AndroidManifest.xml** soubor **vlastnosti** složky a přidejte následující uvnitř **manifest** uzlu:

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Nebo klikněte pravým tlačítkem na projekt pro Android a otevřete vlastnosti projektu. V části **Manifest v Androidu** najít **požadovaná oprávnění:** oblasti a kontrolu **ACCESS_COARSE_LOCATION** a **ACCESS_FINE_LOCATION**oprávnění. Tím se automaticky aktualizují **AndroidManifest.xml** souboru.

# <a name="iostabios"></a>[iOS](#tab/ios)

Vaše aplikace **Info.plist** musí obsahovat `NSLocationWhenInUseUsageDescription` klíč pro přístup k poloze zařízení.

Otevřete plist editor a přidejte **soukromí – popis umístění. když v použití využití** vlastnosti a vyplňte hodnoty pro zobrazení uživatele.

Nebo ručně upravit soubor a přidejte následující:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

Je nutné nastavit `Location` oprávnění pro aplikaci. To můžete udělat tak, že otevřete **Package.appxmanifest** a selecing **možnosti** kartu a kontrola **umístění**.

-----

## <a name="using-geolocation"></a>Pomocí informace o zeměpisné poloze

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Rozhraní API Geoloation také vyzve uživatele, oprávnění, pokud je to nezbytné.

Můžete získat poslední známý [umístění](xref:Xamarin.Essentials.Location) zařízení voláním `GetLastKnownLocationAsync` metody. To je často rychlejší, pak proveďte celý dotaz, ale může být méně přesné.

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

Výšku není vždy k dispozici. Pokud není k dispozici, `Altitude` může být vlastnost `null` nebo hodnota může být nula. Pokud je k dispozici výšku, hodnota je v metrech nahoře stopách. 

Zjistit aktuální zařízení [umístění](xref:Xamarin.Essentials.Location) souřadnice, `GetLocationAsync` lze použít. Je nejvhodnější a zajistěte tak předání úplné `GeolocationRequest` a `CancellationToken` vzhledem k tomu, že může trvat nějakou dobu získání umístění zařízení.

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

## <a name="geolocation-accuracy"></a>Informace o zeměpisné poloze přesnost

Následující tabulka popisuje přesnost jednotlivé platformy:

### <a name="lowest"></a>Nejnižší

| Platforma | Distance (v metrech) |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| UWP | 1000 – 5000 |

### <a name="low"></a>Nízká

| Platforma | Distance (v metrech) |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| UWP | 300 – 3000 |

### <a name="medium-default"></a>Střední (výchozí)

| Platforma | Distance (v metrech) |
| --- | --- |
| Android | 100 - 500 |
| iOS | 100 |
| UWP | 30-500 |

### <a name="high"></a>Vysoká

| Platforma | Distance (v metrech) |
| --- | --- |
| Android | 0 – 100 |
| iOS | 10 |
| UWP | < = 10 |

### <a name="best"></a>Nejlepší

| Platforma | Distance (v metrech) |
| --- | --- |
| Android | 0 – 100 |
| iOS | ~0 |
| UWP | < = 10 |

<a name="calculate-distance" />

## <a name="distance-between-two-locations"></a>Vzdálenost mezi dvěma umístěními

[ `Location` ](xref:Xamarin.Essentials.Location) a [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) třídy definují `CalculateDistance` metody, které umožňují vzdálenosti mezi dvěma zeměpisné umístění. To vypočítá vzdálenost nepřijímá silnicích zakázána nebo jiné cesty v úvahu a se taky říká pouze nejkratší vzdáleností mezi dvěma body podél na povrchu země, _-delšími_ nebo colloquially, vzdálenost "jako dovézt."

Tady je příklad:

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

`Location` Konstruktor obsahuje argumenty zeměpisné šířky a délky v tomto pořadí. Pozitivní severně od rovníku jsou hodnoty zeměpisné šířky a délky kladné hodnoty východ od poledníku, který. Použijte konečný argument `CalculateDistance` k určení mil nebo kilometrů. `Location` Třída také definuje `KilometersToMiles` a `MilesToKilometers` metody pro převod mezi dvěma jednotkami.

## <a name="api"></a>rozhraní API

- [Informace o zeměpisné poloze zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geolocation)
- [Dokumentace k rozhraní API zeměpisné polohy](xref:Xamarin.Essentials.Geolocation)
