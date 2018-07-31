---
title: Spouštěč Xamarin.Essentials
description: Třída Spouštěč v Xamarin.Essentials umožňuje aplikaci identifikátoru URI otevřete v systému.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: 3ab820ece127558c1a5ca8b13c05fc4d6f20646e
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353913"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: Spouštěče

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Spouštěč** třída umožňuje aplikaci identifikátoru URI otevřete v systému. To se často používá při podrobné propojení do jiné aplikace vlastní schémata identifikátoru URI. Pokud chcete otevřít prohlížeč na web, pak byste měli použít **[prohlížeče](open-browser.md)** rozhraní API.

## <a name="using-launcher"></a>Pomocí Spouštěče

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Použití volání funkce Spouštěč `OpenAsync` metoda a předejte jí `string` nebo `Uri` otevřete. Volitelně můžete `CanOpenAsync` metodu lze použít ke kontrole, pokud schéma identifikátoru URI může být zpracována aplikace na zařízení.

```csharp
public class LauncherTest
{
    public async Task OpenRideShare()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## <a name="api"></a>rozhraní API

- [Spouštěč zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Dokumentace ke službě Spouštěč rozhraní API](xref:Xamarin.Essentials.Launcher)
