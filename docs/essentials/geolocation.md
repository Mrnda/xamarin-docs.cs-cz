---
title: Informace o zeměpisné poloze Xamarin.Essentials
description: Informace o zeměpisné poloze třída poskytuje rozhraní API pro načtení souřadnice aktuální informace o zeměpisné poloze zařízení.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 51e63245cd6959f650a0aa078cc4632bc825de5b
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-geocoding"></a>Geografické kódování Xamarin.Essentials

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Informace o zeměpisné poloze** třída poskytuje rozhraní API pro načtení souřadnice aktuální informace o zeměpisné poloze zařízení.

## <a name="getting-started"></a>Začínáme

Abyste měli přístup **informace o zeměpisné poloze** funkce následující nastavení konkrétní platformy není třeba.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Hrubé a umístění dobře oprávnění jsou povinné a musí být konfigurované v projektu pro Android. Kromě toho pokud aplikace cílí Android 5.0 (API úrovně 21) nebo vyšší, musí deklarovat, který vaše aplikace používá funkce hardwaru v souboru manifestu. To lze přidat následujícími způsoby:

Otevřete **AssemblyInfo.cs** souboru pod **vlastnosti** složky a přidat:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

NEBO aktualizovat manifestu systému Android.:

Otevřete **AndroidManifest.xml** souboru pod **vlastnosti** složky a přidejte následující uvnitř **manifest** uzlu.

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Nebo klikněte pravým tlačítkem na projekt Anroid a otevřete vlastnosti projektu. V části **Android Manifest** najít **požadovaná oprávnění:** oblasti a kontroly **ACCESS_COARSE_LOCATION** a **ACCESS_FINE_LOCATION**oprávnění. Tím se automaticky aktualizuje **AndroidManifest.xml** souboru.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aplikace je potřeba mít klíče vaší **Info.plist** pro NSLocationWhenInUseUsageDescription, aby bylo možné získat přístup k umístění v zařízení.

Otevřete plist editor a přidat **soukromí - umístění při v použití využití popis** vlastnosti a vyplňte hodnotu zobrazíte uživatele.

Ručně upravte soubor a přidejte následující:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Je nutné nastavit `Location` oprávnění pro aplikaci. To lze provést pomocí otevřete **Package.appxmanifest** a selecing **možnosti** kartě a kontrola **umístění**.

-----

## <a name="using-geolocation"></a>Pomocí zeměpisnou polohu

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Rozhraní API Geoloation také vyzve uživatele k zadání oprávnění, pokud je to nezbytné.

Můžete získat poslední známou [umístění](xref:Xamarin.Essentials.Location) zařízení voláním `GetLastKnownLocationAsync` metoda. To je často rychlejší, pak to celý dotaz, ale může být méně přesný.

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

Zjistit aktuální zařízení [umístění](xref:Xamarin.Essentials.Location) souřadnice `GetLocationAsync` lze použít. Doporučuje se předat úplnou `GeolocationRequest` a `CancellationToken` vzhledem k tomu, že může trvat nějakou dobu umístění zařízení.

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
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

Následující tabulka popisuje přesnost na každou platformu

### <a name="lowest"></a>Nejnižší

| Platforma | Vzdálenost (v měřidla) |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| UWP | 1000 - 5000 |

### <a name="low"></a>Nízká

| Platforma | Vzdálenost (v měřidla) |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| UWP | 300 - 3000 |

### <a name="medium-default"></a>Střední (výchozí)

| Platforma | Vzdálenost (v měřidla) |
| --- | --- |
| Android | 100 - 500 |
| iOS | 100 |
| UWP | 30-500 |

### <a name="high"></a>Vysoká

| Platforma | Vzdálenost (v měřidla) |
| --- | --- |
| Android | 0 - 100 |
| iOS | 10 |
| UWP | < = 10 |

### <a name="best"></a>Nejvhodnější

| Platforma | Vzdálenost (v měřidla) |
| --- | --- |
| Android | 0 - 100 |
| iOS | ~0 |
| UWP | < = 10 |

## <a name="api"></a>rozhraní API

- [Informace o zeměpisné poloze zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Essentials/Geolocation)
- [Dokumentaci k rozhraní API zeměpisnou polohu](xref:Xamarin.Essentials.Geolocation)
