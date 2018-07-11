---
title: 'Xamarin.Essentials: Geokódování'
description: Třída Geokódování v Xamarin.Essentials poskytuje rozhraní API do obou geokód placemark poziční souřadnice a obrátit geokód souřadnice placemark.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 063adba82d96e7fcc64d7ec49a0c0133e1cef8ef
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831445"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials: Geokódování

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Geokódování** třída poskytuje rozhraní API pro geokód placemark poziční souřadnice a obrátit geokód coordincates placemark.

## <a name="getting-started"></a>Začínáme

Pro přístup **Geokódování** funkce je následující nastavení konkrétní platformy se vyžaduje.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Není požadováno žádné další nastavení.

# <a name="iostabios"></a>[iOS](#tab/ios)

Není požadováno žádné další nastavení.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

Klíč rozhraní API map Bing je potřeba použít funcationality geokódování. Zaregistrujte si bezplatnou [mapy Bing](https://www.bingmapsportal.com/) účtu. V části **Můj účet > Moje klíče** vytvořte nový klíč a vyplňte informace podle typu aplikací (který by měl být **veřejné aplikace Windows (UPW, 8.x a starší)** pro aplikace pro UPW).

Hned zpočátku v životě vaší aplikace před voláním některé **Geokódování** nastavit klíč rozhraní API:

```csharp
Geocoding.MapKey = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Pomocí Geokódování

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Získávání [umístění](xref:Xamarin.Essentials.Location) souřadnice pro adresu:

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
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

Výšku není vždy k dispozici. Pokud není k dispozici, `Altitude` může být vlastnost `null` nebo hodnota může být nula. Pokud je k dispozici výšku, hodnota je v metrech nahoře stopách. 

Získávání [placemarks](xref:Xamarin.Essentials.Placemark) pro stávající sadu souřadnice:

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

## <a name="distance-between-two-locations"></a>Vzdálenost mezi dvěma umístěními

[ `Location` ](xref:Xamarin.Essentials.Location) a [ `LocationExtensions` ](xref:Xamarin.Essentials.LocationExtensions) třídy definuje metody k výpočtu vzdálenost mezi dvěma umístěními. Přečtěte si článek [ **Xamarin.Essentials: informace o zeměpisné poloze** ](geolocation.md#calculate-distance) příklad.

## <a name="api"></a>rozhraní API

- [Geokódování zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Geocoding)
- [Dokumentace ke službě geokódování rozhraní API](xref:Xamarin.Essentials.Geocoding)
