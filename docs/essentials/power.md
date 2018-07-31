---
title: 'Xamarin.Essentials: Stav úspora energie napájení'
description: Třída Power umožňuje aplikaci získat stav úspora energie k určení, pokud zařízení pracuje v režimu nízkým výkonem.
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 760a305280269734034a817182a8c2a07894ca2b
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353487"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: Stav úspora energie napájení

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Power** třída poskytuje informace o stavu zařízení úspora energie, který označuje, zda zařízení běží v režimu nízkým výkonem. Pokud je stav tohoto zařízení úspora energie na aplikace se měli vyhnout zpracování na pozadí.

## <a name="background"></a>Pozadí

Zařízení, které běží na baterie můžou být přepnuté do režimu snížené spotřeby energie spořič. V některých případech zařízení jsou přepnuté do tohoto režimu automaticky, například když baterie klesne pod 20 % kapacity. Operační systém jsou reaguje na používání režimu spořiče energie snížením aktivity, které mají tendenci poškozují baterie. Aplikace může pomoci při používání režimu spořiče energie je na se vyhnout zpracování na pozadí nebo jiné vysokou aktivity.

Pro zařízení s Androidem **Power** třída vrací smysluplné informace pouze pro Android verze 5.0 (Lollipop) a vyšší.

## <a name="using-the-power-class"></a>Pomocí třídy napájení

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Získat aktuální stav úspora energie zařízení pomocí statické `Power.EnergySaverStatus` vlastnost:

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

Tato vlastnost vrátí členem `EnergySaverStatus` výčet, který je buď `On`, `Off`, nebo `Unknown`. Pokud vlastnost vrátí `On`, aplikace byste se vyhnout zpracování na pozadí nebo jiných aktivit, které může spotřebovat velké množství energie.

Aplikace by měl také nainstalovat obslužnou rutinu události. **Power** třída zveřejňuje událost, která se aktivuje, když se změní stav úspora energie:

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanged += OnEnergySaverStatusChanged;
    }

    private void OnEnergySaverStatusChanged(EnergySaverStatusChangedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

Pokud se změní stav úspora energie na `On`, aplikace by se měla zastavit provádění zpracování na pozadí. Pokud se stav změní na `Unknown` nebo `Off`, aplikace může pokračovat zpracování na pozadí.

## <a name="api"></a>rozhraní API

- [Napájení zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Dokumentace k rozhraní API napájení](xref:Xamarin.Essentials.Power)
