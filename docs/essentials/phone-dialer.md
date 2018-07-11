---
title: 'Xamarin.Essentials: Telefon'
description: Třída telefon.dokument v Xamarin.Essentials umožňuje aplikaci otevřete webový odkaz v upřednostňovaný prohlížeč optimalizovaného systému nebo v externím prohlížeči.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6733e43ed4174d1dd78b2e8f70268eb54adadb98
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831394"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: Telefon

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Telefon.dokument** třída umožňuje aplikaci otevřete webový odkaz v upřednostňovaný prohlížeč optimalizovaného systému nebo v externím prohlížeči.

## <a name="using-phone-dialer"></a>Pomocí telefonní vytáčecí zařízení

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Funkce telefon funguje tak, že volání `Open` metoda s telefonním číslem, chcete-li spustit program Telefon s. Když `Open` je požadováno rozhraní API se automaticky pokusí o formátování čísla podle směrové číslo země, pokud zadaný.

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
- [Dokumentace ke službě Telefonní vytáčecí zařízení API](xref:Xamarin.Essentials.PhoneDialer)
