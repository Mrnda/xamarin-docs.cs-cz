---
title: 'Xamarin.Essentials: Informace o zařízení'
description: Tento dokument popisuje třídy DeviceInfo v Xamarin.Essentials, který poskytuje informace o zařízení, že aplikace běží.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 18fe081372cc190e5ead2045f36d63652f8702c3
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353799"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials: Informace o zařízení

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**DeviceInfo** třída poskytuje informace o zařízení, aplikace běží.

## <a name="using-deviceinfo"></a>Pomocí DeviceInfo

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Tyto informace jsou dostupné prostřednictvím rozhraní API:

```csharp
// Device Model (SMG-950U, iPhone10,6)
var device = DeviceInfo.Model;

// Manufacturer (Samsung)
var manufacturer = DeviceInfo.Manufacturer;

// Device Name (Motz's iPhone)
var deviceName = DeviceInfo.Name;

// Operating System Version Number (7.0)
var version = DeviceInfo.VersionString;

// Platform (Android)
var platform = DeviceInfo.Platform;

// Idiom (Phone)
var idiom = DeviceInfo.Idiom;

// Device Type (Physical)
var deviceType = DeviceInfo.DeviceType;
```

## <a name="platformsxrefxamarinessentialsdeviceinfoplatforms"></a>[Platformy](xref:Xamarin.Essentials.DeviceInfo.Platforms)

`DeviceInfo.Platform` koreluje s konstantním řetězcem, který se mapuje na operační systém. Hodnoty mohou být kontrolována pomocí `Platforms` třídy:

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** – nepodporované

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Idiomy](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` koreluje konstantní řetězec, který se mapuje na typ zařízení, aplikace běží na. Hodnoty mohou být kontrolována pomocí `Idioms` třídy:

- **DeviceInfo.Idioms.Phone** – telefon
- **DeviceInfo.Idioms.Tablet** – Tablet
- **DeviceInfo.Idioms.Desktop** – klasické pracovní plochy
- **DeviceInfo.Idioms.TV** – TV
- **DeviceInfo.Idioms.Unsupported** – nepodporované

## <a name="device-type"></a>Typ zařízení

`DeviceInfo.DeviceType` koreluje výčet k určení, zda aplikace běží na fyzickém nebo virtuálním zařízení. Virtuální zařízení je simulátor nebo emulátoru.

## <a name="platform-implementation-specifics"></a>Specifika platforem implementace

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS nevystavuje rozhraní API pro vývojáře a získat tak název zařízení s Iosem konkrétní. Místo toho identifikátor hardwaru se vrátí jako _iPhone10 6_ odkazuje na iPhone X. Mapování těchto Identifiers nejsou poskytovaných společností Apple, ale můžete najít na [iPhone Wiki](https://www.theiphonewiki.com/wiki/Models) (bez oficiální zdroj zdroje).

--------------

## <a name="api"></a>rozhraní API

- [DeviceInfo zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [Dokumentace k rozhraní API DeviceInfo](xref:Xamarin.Essentials.DeviceInfo)
