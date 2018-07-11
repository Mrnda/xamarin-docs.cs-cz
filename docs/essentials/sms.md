---
title: 'Xamarin.Essentials: SMS'
description: Třída serveru Sms v Xamarin.Essentials umožňuje aplikaci otevřete aplikaci výchozí SMS pomocí zadané zprávy k odeslání příjemci.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a93a67b83ea8f435a5e3ad5d26e1d6cbbb7092f7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815594"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Sms** třída umožňuje aplikaci otevřete aplikaci výchozí SMS pomocí zadané zprávy k odeslání příjemci.

## <a name="using-sms"></a>Pomocí služby Sms

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Funkce SMS funguje tak, že volání `ComposeAsync` metoda `SmsMessage` , který obsahuje text zprávy, které jsou volitelné a příjemce zprávy.

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string recipient)
    {
        try
        {
            var message = new SmsMessage(messageText, recipient);
            await Sms.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException ex)
        {
            // Sms is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>rozhraní API

- [Zdrojový kód SMS](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Sms)
- [Dokumentace k rozhraní API služby SMS](xref:Xamarin.Essentials.Sms)
