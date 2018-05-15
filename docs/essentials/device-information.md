---
title: Informace o zařízení Xamarin.Essentials
description: Třída DeviceInfo poskytuje informace o zařízení, aplikace běží na.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a398f2697b03cec26f2c786b11e7a3c332cb5f63
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-device-information"></a>Informace o zařízení Xamarin.Essentials

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**DeviceInfo** třída poskytuje informace o zařízení, aplikace běží.

## <a name="using-deviceinfo"></a>Pomocí DeviceInfo

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Tyto informace jsou dostupné prostřednictvím rozhraní API:

```csharp
// Device Model (SMG-950U)
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

`DeviceInfo.Platform` koreluje s konstantní řetězec, který se mapuje na operační systém. Hodnoty mohou být kontrolována pomocí `Platforms` třídy:

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** – nepodporované

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Idioms](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` korelaci konstantní řetězec, který se mapuje na typ zařízení, aplikace běží na. Hodnoty mohou být kontrolována pomocí `Idioms` třídy:

- **DeviceInfo.Idioms.Phone** – telefon
- **DeviceInfo.Idioms.Tablet** – Tablet
- **DeviceInfo.Idioms.Desktop** – plochy
- **DeviceInfo.Idioms.TV** – TV
- **DeviceInfo.Idioms.Unsupported** – nepodporované

## <a name="device-type"></a>Typ zařízení

`DeviceInfo.DeviceType` korelaci výčet k určení, pokud je aplikace spuštěním na fyzický nebo virtuální zařízení. Virtuální zařízení je simulátoru nebo emulátor.

## <a name="api"></a>rozhraní API

- [DeviceInfo zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [Dokumentaci k rozhraní API DeviceInfo](xref:Xamarin.Essentials.DeviceInfo)
