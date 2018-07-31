---
title: 'Xamarin.Essentials: e-mailu'
description: Třída e-mailu v Xamarin.Essentials umožňuje aplikaci otevřít výchozí e-mailové aplikace se zadanými informacemi, včetně předmět, text a příjemců (Komu, kopie, SKRYTÁ).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f113cebfebf4238fd4b75ad8ab248e2abf61efea
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353903"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: e-mailu

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**E-mailu** třída umožňuje aplikaci otevřít výchozí e-mailové aplikace se zadanými informacemi, včetně předmět, text a příjemců (Komu, kopie, SKRYTÁ).

## <a name="using-email"></a>E-mailem

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

E-mailové funkce funguje tak, že volání `ComposeAsync` metoda `EmailMessage` , který obsahuje informace o e-mailu:

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string body, List<string> recipients)
    {
        try
        {
            var message = new EmailMessage
            {
                Subject = subject,
                Body = body,
                To = recipients,
                //Cc = ccRecipients,
                //Bcc = bccRecipients
            };
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Email is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occurred
        }
    }
}
```

## <a name="api"></a>rozhraní API

- [E-mailu zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Dokumentace k rozhraní API e-mailu](xref:Xamarin.Essentials.Email)
