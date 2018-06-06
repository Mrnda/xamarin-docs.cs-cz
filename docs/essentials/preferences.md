---
title: 'Xamarin.Essentials: předvolby'
description: Tento dokument popisuje třídy předvolby v Xamarin.Essentials, který uloží předvolby aplikace do úložiště dvojic klíč/hodnota. Popisuje, jak používat třídu a typy dat, které lze uložit.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: e453c04a953e60be2508670723d175bde3dc7c42
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782840"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: předvolby

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Předvolby** třída pomáhá k ukládání předvoleb aplikace v úložiště dvojic klíč/hodnota.

## <a name="using-secure-storage"></a>Použití zabezpečeného úložiště

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Uložit hodnotu danou _klíč_ v předvolbách:

```csharp
Preferences.Set("my_key", "my_value");
```

Není-li načíst hodnotu z předvolby nebo výchozí nastavení:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

Chcete-li odebrat _klíč_ předvolby:

```csharp
Preferences.Remove("my_key");
```

Chcete-li odebrat všechny předvolby:

```csharp
Preferences.Clear();
```

Kromě těchto metod každý trvat v volitelný `sharedName` který lze použít k vytvoření další kontejnery pro předvoleb. Přečtěte si následující specifika implementace platformy.

## <a name="supported-data-types"></a>Podporované datové typy

Jsou podporovány následující typy dat v **Předvolby**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**

## <a name="platform-implementation-specifics"></a>Podrobnosti implementace platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

Všechna data uložena do [sdílet Předvolby](https://developer.android.com/training/data-storage/shared-preferences.html). Pokud žádné `sharedName` je zadána výchozí sdílet předvolby se používají, jinak název se použije k získání **privátní** sdílet předvolby se zadaným názvem.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) se používá k ukládání hodnot na zařízeních s iOS. Pokud žádné `sharedName` je zadán `StandardUserDefaults` se používá, jinak název slouží k vytvoření nové `NSUserDefaults` se zadaným názvem používá pro `NSUserDefaultsType.SuiteName`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) se používá k ukládání hodnot v zařízení. Pokud žádné `sharedName` je zadána `LocalSettings` se používá, jinak název slouží k vytvoření nového kontejneru uvnitř `LocalSettings`.

--------------

## <a name="limitations"></a>Omezení

Při ukládání řetězec, toto rozhraní API slouží k uložení malé množství textu.  Výkon může být subpar, pokud se pokusíte použít k ukládání velkých objemů textu.

## <a name="api"></a>rozhraní API

- [Předvolby zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Dokumentace předvolby rozhraní API](xref:Xamarin.Essentials.Preferences)
