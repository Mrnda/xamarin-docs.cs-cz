---
title: 'Xamarin.Essentials: Informace o aplikaci'
description: Tento dokument popisuje třídy AppInfo v Xamarin.Essentials, která poskytuje informace o vaší aplikaci. Například udává název aplikace a verze.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 25765fbbcbd2475bfcbc72ca5c4feefa8ef19d08
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782102"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: Informace o aplikaci

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**AppInfo** třída poskytuje informace o vaší aplikaci.

## <a name="using-appinfo"></a>Pomocí AppInfo

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Tyto informace jsou dostupné prostřednictvím rozhraní API:

```csharp
// Application Name
var appName = AppInfo.Name;

// Package Name/Application Identifier (com.microsoft.testapp)
var packageName = AppInfo.PackageName;

// Application Version (1.0.0)
var version = AppInfo.VersionString;

// Application Build Number (1)
var build = AppInfo.BuildString;
```

## <a name="api"></a>rozhraní API

- [AppInfo zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [Dokumentaci k rozhraní API AppInfo](xref:Xamarin.Essentials.AppInfo)
