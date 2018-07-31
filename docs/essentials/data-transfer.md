---
title: 'Xamarin.Essentials: Přenos dat'
description: Třída DataTransfer v Xamarin.Essentials umožňuje aplikaci pro sdílení dat, například text a webové odkazy na jiné aplikace na zařízení.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 31e27556a6681b144084d2177cf3fde8fe8e5459
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353516"
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials: Přenos dat

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**DataTransfer** třída umožňuje aplikaci pro sdílení dat, například text a webové odkazy na jiné aplikace na zařízení.

## <a name="using-data-transfer"></a>Pomocí datové přenosy

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Funkce přenosu dat funguje tak, že volání `RequestAsync` metodu se datová část požadavku dat, který obsahuje informace k ostatním aplikacím sdílet. Text a identifikátor Uri lze kombinovat a jednotlivé platformy, bude zpracovávat filtrování na základě obsahu.

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

Uživatelské rozhraní sdílení externí aplikaci, která se zobrazí, když se požadavek:

![Přenos dat](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>Rozdíly pro tyto platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `Subject` Vlastnost se používá pro požadovanou předmět zprávy.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `Subject` Nepoužívá se.
* `Title` Nepoužívá se.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

* `Title` bude ve výchozím nastavení název aplikace není-li nastavit.
* `Subject` Nepoužívá se.

-----

## <a name="api"></a>rozhraní API

- [Zdrojový kód přenosu dat](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [Dokumentace k API přenosu dat](xref:Xamarin.Essentials.DataTransfer)
