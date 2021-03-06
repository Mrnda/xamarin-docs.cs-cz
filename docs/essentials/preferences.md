---
title: 'Xamarin.Essentials: předvolby'
description: Tento dokument popisuje třídy předvolby v Xamarin.Essentials, který ukládá předvolby aplikace do úložiště dvojic klíč/hodnota. Popisuje, jak použít třídy a typy dat, které mohou být uloženy.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 4a45587c79cfbbcd1198f100915e698289f74950
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353747"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: předvolby

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Předvolby** třída umožňuje uložit předvolby aplikace do úložiště dvojic klíč/hodnota.

## <a name="using-preferences"></a>Použití předvoleb

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Uložte hodnotu daného _klíč_ v předvolbách:

```csharp
Preferences.Set("my_key", "my_value");
```

K načtení hodnoty z předvolby nebo výchozí hodnotu, pokud není nastavena:

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

Kromě těchto metod každá vzít v volitelně `sharedName` , který slouží k vytvoření dalších kontejnerů pro předvoleb. Přečtěte si níže podrobnosti implementace platformy.

## <a name="supported-data-types"></a>Podporované datové typy

Jsou podporovány následující typy dat v **Předvolby**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **Datum a čas**

## <a name="implementation-details"></a>Podrobnosti implementace

Hodnoty `DateTime` jsou uložené ve formátu binárního souboru 64-bit (long integer) pomocí dvou metod, které jsou definované `DateTime` třída: [ `ToBinary` ](xref:System.DateTime.ToBinary) metoda se používá ke kódování `DateTime` hodnotu a [ `FromBinary` ](xref:System.DateTime.FromBinary(System.Int64)) metoda dekóduje hodnotu. Viz dokumentace ke službě z těchto metod pro úpravy, které by mohly být provedeny na dekódovaný hodnot, když `DateTime` je uložená, který je není hodnota koordinovaný univerzální čas (UTC).

## <a name="platform-implementation-specifics"></a>Specifika platforem implementace

# <a name="androidtabandroid"></a>[Android](#tab/android)

Všechna data jsou uložena do [sdílené Předvolby](https://developer.android.com/training/data-storage/shared-preferences.html). Pokud ne `sharedName` určena výchozí sdílené předvolby se používají, jinak název slouží k získání **privátní** sdílené předvolby se zadaným názvem.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) se používá k ukládání hodnot na zařízeních s Iosem. Pokud ne `sharedName` je zadán `StandardUserDefaults` se používá, jinak název slouží k vytvoření nového `NSUserDefaults` se zadaným názvem používá pro `NSUserDefaultsType.SuiteName`.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) slouží k uložení hodnoty v zařízení. Pokud ne `sharedName` je zadán `LocalSettings` se používá, jinak název slouží k vytvoření nového kontejneru uvnitř `LocalSettings`.

--------------

## <a name="persistence"></a>Trvalost

Odinstalace aplikace způsobí, že všechny _Předvolby_ odeberou. Existuje jedna výjimka tohoto, které je pro aplikace, které cílí a spustit na Android 6.0 (úroveň rozhraní API 23) nebo novější, který používáte [ __automatické zálohování__](https://developer.android.com/guide/topics/data/autobackup). Tato funkce je ve výchozím a chrání aplikace data včetně __sdílené Předvolby__, to znamená, co **Předvolby** využívá rozhraní API. Tuto funkci můžete vypnout podle následujících Google [dokumentaci](https://developer.android.com/guide/topics/data/autobackup).

## <a name="limitations"></a>Omezení

Při ukládání řetězce, je toto rozhraní API určené k ukládání malé množství textu.  Výkon může být subpar, pokud se pokusíte použít k ukládání velkého množství textu.

## <a name="api"></a>rozhraní API

- [Předvolby zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Dokumentace ke službě předvolby rozhraní API](xref:Xamarin.Essentials.Preferences)
