---
title: Geografické kódování Xamarin.Essentials
description: Geografické kódování třída poskytuje rozhraní API pro geocode placemark pro poziční souřadnice a nechat provést zpětnou geocode coordincates placemark.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 95301dad847887e867b220997ea9c34dba827982
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="xamarinessentials-geocoding"></a>Geografické kódování Xamarin.Essentials

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Geografické kódování** třída poskytuje rozhraní API pro geocode placemark pro poziční souřadnice a nechat provést zpětnou geocode coordincates placemark.

## <a name="getting-started"></a>Začínáme

Abyste měli přístup **geografické kódování** následující nastavení konkrétní platformy funkce jsou nezbytné.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Nevyžaduje žádné další nastavení.

# <a name="iostabios"></a>[iOS](#tab/ios)

Nevyžaduje žádné další nastavení.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Klíč rozhraní API map Bing je potřeba použít funcationality geografické kódování. Zaregistrujte si bezplatný [mapy Bing](https://www.bingmapsportal.com/) účtu. V části **Můj účet > mé klíče** vytvořte nový klíč a vyplňte informace podle vašeho typu aplikace.

Časná na v životnosti vaší aplikace před voláním žádné **geografické kódování** nastavit klíč rozhraní API:

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Pomocí geografické kódování

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Získávání [umístění](xref:Xamarin.Essentials.Location) souřadnice adresu:

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occured in geocoding
}
```

Získávání [placemarks](xref:Xamarin.Essentials.Placemark) pro existující sady souřadnice:

```csharp
try
{
    var lat = 47.673988;
    var lon = -122.121513;

    var placemarks = await Geocoding.GetPlacemarksAsync(lat, lon);

    var placemark = placemarks?.FirstOrDefault();
    if (placemark != null)
    {
        var geocodeAddress =
            $"AdminArea:       {placemark.AdminArea}\n" +
            $"CountryCode:     {placemark.CountryCode}\n" +
            $"CountryName:     {placemark.CountryName}\n" +
            $"FeatureName:     {placemark.FeatureName}\n" +
            $"Locality:        {placemark.Locality}\n" +
            $"PostalCode:      {placemark.PostalCode}\n" +
            $"SubAdminArea:    {placemark.SubAdminArea}\n" +
            $"SubLocality:     {placemark.SubLocality}\n" +
            $"SubThoroughfare: {placemark.SubThoroughfare}\n" +
            $"Thoroughfare:    {placemark.Thoroughfare}\n";

        Console.WriteLine(geocodeAddress);
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

## <a name="api"></a>rozhraní API

- [Geografické kódování zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Dokumentace k určování zeměpisných souřadnic rozhraní API](xref:Xamarin.Essentials.Geocoding)