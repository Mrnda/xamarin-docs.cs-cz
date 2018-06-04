---
title: Xamarin.Essentials e-mailu
description: Třída e-mailu umožňuje aplikace pro otevření výchozí e-mailové aplikace se zadanými informacemi, včetně předmět, text a příjemců (na, kopie, pole SKRYTÁ).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 4bba713fa4dbcac3c145389c4b009d6e207273b8
ms.sourcegitcommit: a4c2a63ba76b839cda99e4474e7ab46fe307cd39
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34562861"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials e-mailu

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**E-mailu** třída umožňuje aplikaci otevřete výchozí e-mailové aplikace se zadanými informacemi, včetně předmět, text a příjemců (na, kopie, pole SKRYTÁ).

## <a name="using-email"></a>E-mailem

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

E-mailové funkce funguje tak, že volání `ComposeAsync` metoda `EmailMessage` obsahující informace o e-mailu:

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
            // Some other exception occured
        }
    }
}
```

## <a name="api"></a>rozhraní API

- [E-mailu zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Dokumentace rozhraní API e-mailu](xref:Xamarin.Essentials.Email)
