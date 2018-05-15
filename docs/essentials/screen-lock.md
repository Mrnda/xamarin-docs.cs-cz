---
title: Zamykací obrazovka Xamarin.Essentials
description: Třída ScreenLock můžete žádost o zabránit návratem když je aplikace spuštěna v režimu spánku obrazovky.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f7e6a3d6933ed1fce7522fdbb8102e5100bd1589
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-screen-lock"></a>Zamykací obrazovka Xamarin.Essentials

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**ScreenLock** třídy můžete žádost o zabránit návratem když je aplikace spuštěna v režimu spánku obrazovky.

## <a name="using-screenlock"></a>Pomocí ScreenLock

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Funkce Zámek obrazovky funguje tak, že volání `RequestActive` a `RequestRelease` metody k vyžádání obrazovky z vypnutí.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (ScreenLock.IsMonitoring)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>rozhraní API

- [Zdrojový kód zámku obrazovky](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Dokumentace k API zámku obrazovky](xref:Xamarin.Essentials.ScreenLock)
