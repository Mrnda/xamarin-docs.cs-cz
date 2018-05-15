---
title: Xamarin.Essentials telefon
description: Třída telefon.dokument povolí aplikaci otevřete webový odkaz v prohlížeči upřednostňované optimalizované systému nebo externího prohlížeče.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 112cc305457413ad057e390d46c5a765ea29514f
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
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

- [Telefon zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [Dokumentaci k rozhraní API programu Telefon Phone](xref:Xamarin.Essentials.PhoneDialer)
