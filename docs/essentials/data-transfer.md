---
title: Přenos dat Xamarin.Essentials
description: Třída DataTransfer povolí aplikaci pro sdílení dat jako text a webové odkazy na jiné aplikace na zařízení.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: baec3bdd89cb98d7595a524b6b9c4263ca18aa41
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="xamarinessentials-data-transfer"></a>Přenos dat Xamarin.Essentials

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**DataTransfer** třída umožňuje aplikaci sdílet data, jako jsou text a webové odkazy na jiné aplikace na zařízení.

## <a name="using-data-transfer"></a>Pomocí přenosu dat

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Funkci přenosu dat funguje tak, že volání `RequestAsync` metodu se datová část požadavku data, která obsahuje informace, které sdílejí k ostatním aplikacím. Text a identifikátoru Uri můžete kombinovat a každou platformu, bude zpracovávat filtrování na základě obsahu.

```csharp

public class DataTransferTest
{
    public async Task ShareText(string text)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

Uživatelské rozhraní pro sdílenou složku do externí aplikací, které se zobrazí po požadavku:

![Přenos dat](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>Rozdíly platformy

| Platforma | Rozdíl |
| --- | --- |
| Android | Vlastnost předmětu se používá pro požadovanou předmětu zprávy. |
| iOS | Předmět nepoužívá. |
| iOS | Název nepoužívá. |
| UWP | Název bude jako výchozí název aplikace není-li nastavit. |
| UWP | Předmět nepoužívá. |

## <a name="api"></a>rozhraní API

- [Zdrojový kód přenosu dat](https://github.com/xamarin/Essentials/tree/master/Essentials/DataTransfer)
- [Dokumentace k API přenosu dat](xref:Xamarin.Essentials.DataTransfer)
