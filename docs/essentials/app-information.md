---
title: 'Xamarin.Essentials: Informace o aplikaci'
description: Tento dokument popisuje třídy AppInfo v Xamarin.Essentials, který poskytuje informace o vaší aplikaci. Například udává název aplikace a verze.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e79b3003f41b8de22950624e44e8c9e0e7e7e31
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831504"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: Informace o aplikaci

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**AppInfo** třída poskytuje informace o vaší aplikaci.

## <a name="using-appinfo"></a>Pomocí AppInfo

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>Získání informací o aplikaci:

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

## <a name="displaying-application-settings"></a>Zobrazení nastavení aplikace

**AppInfo** třídy můžete také zobrazit stránku nastavení udržovány v operačním systému pro aplikaci:

```csharp
// Display settings page
AppInfo.OpenSettings();
```

Tato stránka nastavení umožní uživateli změnit oprávnění aplikace a provádět další úlohy specifické pro platformu.

## <a name="api"></a>rozhraní API

- [AppInfo zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [Dokumentace k rozhraní API AppInfo](xref:Xamarin.Essentials.AppInfo)
