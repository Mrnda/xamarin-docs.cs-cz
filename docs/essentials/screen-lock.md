---
title: 'Xamarin.Essentials: Zamykací obrazovka'
description: Tento dokument popisuje třídy ScreenLock v Xamarin.Essentials, které můžou požádat o zabránit klesá režimu spánku, když aplikace běží na obrazovce.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c8110b7abc86fe1d12485579f134997718540e6
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848567"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: Zamykací obrazovka

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**ScreenLock** třídy můžete požádat o zabránit klesá režimu spánku, když aplikace běží na obrazovce.

## <a name="using-screenlock"></a>Pomocí ScreenLock

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Funkce Zámek obrazovky funguje tak, že volání `RequestActive` a `RequestRelease` metody se žádá na obrazovce na vypnutí.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (!ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>rozhraní API

- [Zdrojový kód zamykací obrazovky](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Dokumentace k API zamykací obrazovky](xref:Xamarin.Essentials.ScreenLock)
