---
title: Xamarin.Essentials telefon
description: Třída telefon.dokument povolí aplikaci otevřete webový odkaz v prohlížeči upřednostňované optimalizované systému nebo externího prohlížeče.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6bea1b26e43459be12272f9dd2ecc6fafba2b8b4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials telefon

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Telefon.dokument** třída umožňuje aplikaci otevřete webový odkaz v upřednostňované prohlížeč optimalizované systému nebo externího prohlížeče.

## <a name="using-phone-dialer"></a>Pomocí programu Telefon

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Funkce telefon funguje tak, že volání `Open` metoda s telefonním číslem, chcete-li spustit program Telefon s. Když `Open` je požadováno rozhraní API se automaticky pokusí formátu podle kód země, pokud zadané číslo.

```csharp
public class PhoneDialerTest
{
    public async Task PlacePhoneCall(string number)
    {
        try
        {
            PhoneDialer.Open(number);
        }
        catch (ArgumentNullException anEx)
        {
            // Number was null or white space
        }
        catch (FeatureNotSupportedException ex)
        {
            // Phone Dialer is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>rozhraní API

- [Telefon zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Essentials/PhoneDialer)
- [Dokumentaci k rozhraní API programu Telefon Phone](xref:Xamarin.Essentials.PhoneDialer)
