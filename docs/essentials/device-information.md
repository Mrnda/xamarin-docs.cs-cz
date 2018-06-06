---
title: 'Xamarin.Essentials: Informace o zařízení'
description: Tento dokument popisuje třídy DeviceInfo v Xamarin.Essentials, který poskytuje informace o zařízení, že je aplikace spuštěna.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b7246afca19607ef2f70288d4643696f4ac35d52
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782395"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials: Informace o zařízení

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
