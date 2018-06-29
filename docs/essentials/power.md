---
title: 'Xamarin.Essentials: Spotřeby energie šetřič stav'
description: Napájení třída umožňuje aplikaci získat stav energie šetřič k určení, pokud zařízení pracuje v režimu úsporného režimu.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 6d8ccb5be69eb1ea7ea63d3f5c373d9284089679
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080384"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: Spotřeby energie šetřič stav

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Power** třída poskytuje informace o stavu energie šetřič zařízení, které označuje, zda zařízení je spuštěn v režimu úsporného režimu. Aplikace by se zpracování na pozadí, pokud je stav energie šetřič zařízení na.

## <a name="background"></a>Pozadí

Zařízení, které běží na baterie můžou být přepnuté do režimu snížené spotřeby energie šetřič. V některých případech zařízení jsou přepnout do tohoto režimu automaticky, například když baterie klesne pod 20 % kapacity. Operační systém odpoví na energii úsporný režim snížením aktivity, které jsou obvykle vyčerpat by baterie. Aplikace může pomoct vyhnout zpracování na pozadí nebo jiné vysokou činnosti při na energii šetřič režimu.

Pro zařízení s Androidem **Power** třída vrací důležité informace, jenom pro verzi systému Android 5.0 (typu Lupa) a vyšší.

## <a name="using-the-power-class"></a>Používání třídy napájení

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Získat aktuální stav energie šetřič zařízení pomocí statické `Power.EnergySaverStatus` vlastnost:

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Tato vlastnost vrací členem `EnergySaverStatus` výčtu, který je buď `On`, `Off`, nebo `Unknown`. Pokud vlastnost vrací `On`, aplikace by měla vyhnout zpracování na pozadí nebo jiných aktivit, které může využívat velké množství napájení.

Aplikace také nainstalovat obslužné rutiny události. **Power** třída zpřístupňuje událost, která se aktivuje, když se změní stav energie šetřič:

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanaged += OnEnergySaverStatusChanaged;
    }

    private void OnEnergySaverStatusChanaged(EnergySaverStatusChanagedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

Pokud se změní stav energie šetřič na `On`, aplikace by se měla zastavit provádění zpracování na pozadí. Pokud se stav změní na `Unknown` nebo `Off`, aplikace může pokračovat zpracování na pozadí.

## <a name="api"></a>rozhraní API

- [Napájení zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Dokumentaci k rozhraní API napájení](xref:Xamarin.Essentials.Power)
