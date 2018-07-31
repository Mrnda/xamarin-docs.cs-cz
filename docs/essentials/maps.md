---
title: Xamarin.Essentials mapy
description: Třída map v Xamarin.Essentials umožňuje aplikaci otevřete aplikaci nainstalovaných map na konkrétní umístění nebo placemark.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 445e2da84e9a9aaf1ce4d836af11cfba963b8cbb
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353914"
---
# <a name="xamarinessentials-maps"></a>Xamarin.Essentials: mapy

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Mapuje** třída umožňuje aplikaci otevřete aplikaci nainstalovaných map na konkrétní umístění nebo placemark.

## <a name="using-maps"></a>Pomocí mapy

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Mapování funkce funguje tak, že volání `OpenAsync` metodu `Location` nebo `Placemark` otevřete s volitelným `MapsLaunchOptions`.

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(location, options);
    }
}
```

Při otevírání s `Placemark` se vyžaduje následující informace:

* `CountryName`
* `AdminArea`
* `Thoroughfare`
* `Locality`

```csharp
public class MapsTest
{
    public async Task NavigateToBuilding25()
    {
        var placemark = new Placemark
            {
                CountryName = "United States",
                AdminArea = "WA",
                Thoroughfare = "Microsoft Building 25",
                Locality = "Redmond"
            };
        var options =  new MapsLaunchOptions { Name = "Microsoft Building 25" };

        await Maps.OpenAsync(placemark, options);
    }
}
```

## <a name="extension-methods"></a>Metody rozšíření

Pokud už máte odkaz na `Location` nebo `Placemark` můžete použít předdefinované rozšiřující metoda `OpenMapsAsync` s volitelným `MapsLaunchOptions`:

```csharp
public class MapsTest
{
    public async Task OpenPlacemarkOnMaps(Placemark placemark)
    {
        await placemark.OpenMapsAsync();
    }
}
```

## <a name="platform-differences"></a>Rozdíly pro tyto platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `MapDirectionsMode` není podporováno a nemá žádný vliv.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `MapDirectionsMode` je možné nastavit výchozí směr režim při otevření aplikace mapy.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

* `MapDirectionsMode` není podporováno a nemá žádný vliv.

--------------

## <a name="platform-implementation-specifics"></a>Specifika platforem implementace

# <a name="androidtabandroid"></a>[Android](#tab/android)

Používá Android `geo:` schéma identifikátoru Uri ke spuštění aplikace mapy v zařízení. To může vyzvat uživatele k výběru z existující aplikace, která podporuje tento schéma identifikátoru Uri.  Xamarin.Essentials je testovat pomocí mapy Google, která podporuje toto schéma.

# <a name="iostabios"></a>[iOS](#tab/ios)

Žádné podrobnosti konkrétní implementace platformy.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

Žádné podrobnosti konkrétní implementace platformy.

--------------

## <a name="api"></a>rozhraní API

- [Mapy zdrojový kód](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Maps)
- [Dokumentace ke službě mapy rozhraní API](xref:Xamarin.Essentials.Maps)
