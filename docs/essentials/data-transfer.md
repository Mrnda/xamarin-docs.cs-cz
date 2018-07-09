---
title: 'Xamarin.Essentials: Přenos dat'
description: Třída DataTransfer v Xamarin.Essentials umožňuje aplikaci pro sdílení dat, například text a webové odkazy na jiné aplikace na zařízení.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c1ed298e1317d0a3f78f4dbd9fc89a2b01c6958c
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855107"
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

* `Subject` nepoužívá se.
* `Title` nepoužívá se. 

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

* `Title` bude ve výchozím nastavení název aplikace není-li nastavit.
* `Subject` nepoužívá se.

-----

## <a name="api"></a>rozhraní API

- [Zdrojový kód přenosu dat](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [Dokumentace k API přenosu dat](xref:Xamarin.Essentials.DataTransfer)
