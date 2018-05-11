---
title: Xamarin.Essentials SMS
description: Sms třída umožňuje aplikaci otevřete aplikaci SMS výchozí pomocí zadané zprávy k příjemce.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 5baeee03626ba659ac7e5c06be40039476a67e08
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials SMS

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Sms** třída umožňuje aplikaci otevřete aplikaci SMS výchozí pomocí zadané zprávy k příjemce.

## <a name="using-sms"></a>Pomocí serveru Sms

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Funkci SMS funguje tak, že volání `ComposeAsync` metoda `SmsMessage` obsahující tělo zprávy, které jsou volitelné a příjemce zprávy.

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

- [SMS zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Essentials/Sms)
- [Dokumentaci k rozhraní API služby SMS](xref:Xamarin.Essentials.Sms)
