---
title: Xamarin.Essentials otevřít prohlížeč
description: Třída prohlížeče v Xamarin.Essentials umožňuje aplikaci otevřete webový odkaz v upřednostňovaný prohlížeč optimalizovaného systému nebo v externím prohlížeči.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e58d439f5a6eaafe9b1b5e7ca874a986e468cb9
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353279"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: prohlížeč

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Prohlížeče** třída umožňuje aplikaci otevřete webový odkaz v upřednostňovaný prohlížeč optimalizovaného systému nebo v externím prohlížeči.

## <a name="using-browser"></a>Pomocí prohlížeče

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Funkce Browser funguje tak, že volání `OpenAsync` metodu `Uri` a `BrowserLaunchMode`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Specifika platforem implementace

# <a name="androidtabandroid"></a>[Android](#tab/android)

Režim spuštění určuje, jak se spustí prohlížeč:

## <a name="system-preferred"></a>Upřednostňovaný systému

[Chrome vlastní karty](https://developer.chrome.com/multidevice/android/customtabs) se pokusil použít načíst identifikátor Uri a zachovat povědomí o navigaci.

## <a name="external"></a>Externí

`Intent` Se použije k vyžádání identifikátoru Uri otevřít prostřednictvím prohlížeče běžné systémy.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Upřednostňovaný systému

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) slouží k načtení identifikátoru Uri a zachovat povědomí o navigaci na.

## <a name="external"></a>Externí

Standardní `OpenUrl` v hlavní aplikaci lze spustit výchozí prohlížeč mimo aplikaci.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

Výchozí prohlížeč spustí vždy bez ohledu na to `BrowserLaunchMode`.

--------------

## <a name="api"></a>rozhraní API

- [Prohlížeč zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Dokumentace ke službě prohlížeč rozhraní API](xref:Xamarin.Essentials.Browser)
