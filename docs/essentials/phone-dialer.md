---
title: 'Xamarin.Essentials: Telefon'
description: Třída telefon.dokument v Xamarin.Essentials umožňuje aplikaci otevřete telefonní číslo telefon
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 34a6c80836d8cb42b1f8fd95718fe248d4701c0f
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130790"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: Telefon

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Telefon.dokument** třída umožňuje aplikaci otevřete telefonní číslo telefon.

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
