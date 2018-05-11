---
title: Informace o aplikaci Xamarin.Essentials
description: Třída AppInfo poskytuje informace o vaší aplikaci.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 32e3eb8fab719540e4c9ffec4e57f5510c10e3f5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="xamarinessentials-app-information"></a>Informace o aplikaci Xamarin.Essentials

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

- [AppInfo zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Essentials/AppInfo)
- [Dokumentaci k rozhraní API AppInfo](xref:Xamarin.Essentials.AppInfo)
