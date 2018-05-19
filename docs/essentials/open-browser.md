---
title: Otevřete Xamarin.Essentials prohlížeče
description: Prohlížeč třída umožňuje aplikaci otevřete webový odkaz v upřednostňované prohlížeč optimalizované systému nebo externího prohlížeče.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2a95d0b89b78ce8b0ddfdc94d86c284a6f8b2bbe
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/19/2018
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials prohlížeče

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Prohlížeče** třída umožňuje aplikaci otevřete webový odkaz v upřednostňované prohlížeč optimalizované systému nebo externího prohlížeče.

## <a name="using-browser"></a>Pomocí prohlížeče

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Funkce prohlížeč funguje tak, že volání `OpenAsync` metoda s `Uri` a `BrowserLaunchType`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchType.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Podrobnosti implementace platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

Typ spuštění určuje, jak se spustí v prohlížeči:

## <a name="system-preferred"></a>Upřednostňovaný systému

[Vlastní karty pro Chrome](https://developer.chrome.com/multidevice/android/customtabs) se pokusil použít načíst identifikátor Uri a udržovat sledování navigace.

## <a name="external"></a>Externí

`Intent` Se použije k žádosti o identifikátor Uri otevřít prostřednictvím prohlížeče normální systémy.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Upřednostňovaný systému

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) na slouží k načtení identifikátoru Uri a zachovat sledování navigace.

## <a name="external"></a>Externí

Standardní `OpenUrl` na hlavní aplikace, které se používá k spuštění výchozího prohlížeče mimo aplikaci.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Výchozí prohlížeč vždy se spustí bez ohledu na to `BrowserLaunchType`.

--------------

## <a name="api"></a>rozhraní API

- [Prohlížeč zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Dokumentaci k prohlížeči rozhraní API](xref:Xamarin.Essentials.Browser)
