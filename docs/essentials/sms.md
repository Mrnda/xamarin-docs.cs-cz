---
title: Xamarin.Essentials SMS
description: Sms třída umožňuje aplikaci otevřete aplikaci SMS výchozí pomocí zadané zprávy k příjemce.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1f466e01d9b1e48849e60a364cf1d49d9cbdcb37
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
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

- [SMS zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Sms)
- [Dokumentaci k rozhraní API služby SMS](xref:Xamarin.Essentials.Sms)
