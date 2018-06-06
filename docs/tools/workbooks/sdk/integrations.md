---
title: Témata pokročilou integraci
description: Tento dokument popisuje Pokročilá témata související s Xamarin sešity integrace. Popisuje balíček Xamarin.Workbook.Integrations NuGet a rozhraní API ohrožení v sešitu Xamarin.
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 1aa6b5d0ca574345e1d349ea53df96f554c06bc4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793833"
---
# <a name="advanced-integration-topics"></a>Témata pokročilou integraci

Integrace sestavení by měla odkazovat [ `Xamarin.Workbooks.Integrations` NuGet][nuget]. Podívejte se na naše [úvodní dokumentaci](~/tools/workbooks/sdk/index.md) Další informace o začátcích se balíček NuGet.

Integrace klienta jsou podporovány také a zahájí umístěním JavaScript nebo šablon stylů CSS soubory se stejným názvem jako sestavení integraci agenta ve stejném adresáři. Například pokud je název sestavení integraci agenta (který NuGet odkazuje) `SampleExternalIntegration.dll`, pak `SampleExternalIntegration.js` a `SampleExternalIntegration.css` budete integrovat do klienta a pokud existují. Integrace klienta jsou volitelné.

Externí integrace samotné můžete zabalené jako NuGet, poskytuje a odkazuje přímo v aplikaci, která je hostitelem agenta nebo jednoduše umístěny společně `.workbook` soubor, který chce využívat ho.

Externí integrace (agenta a klient) do balíčků NuGet se má načíst automaticky, pokud balíček se odkazuje, jako pro dokumentaci úvodní při integraci sestavení dodaný spolu s sešit nutné na něj odkazovat jako proto:

```csharp
#r "SampleExternalIntegration.dll"
```

Při odkazování na integrační tímto způsobem, ta nebude načtena klient hned&mdash;budete muset volat kód z integrace jej načíst. Jsme budete mít řešení této chyby v budoucnu.

`Xamarin.Interactive` PCL poskytuje několik důležitých integrace rozhraní API. Každý integrační musíte zadat alespoň integrace vstupní bod:

```csharp
using Xamarin.Interactive;

[assembly: AgentIntegration (typeof (AgentIntegration))]

class AgentIntegration : IAgentIntegration
{
    const string TAG = nameof (AgentIntegration);

    public void IntegrateWith (IAgent agent)
    {
        // hook into IAgent APIs
    }
}
```

V tomto okamžiku po sestavení integrace se odkazuje, klient se implicitně načte integrace souborů JavaScript a CSS.

## <a name="apis"></a>API

Žádné z jejích veřejné rozhraní API, že se všechny sestavení, které se odkazuje sešitu nebo za provozu prověřit relace, jsou přístupné relace. Proto je důležité mít bezpečné a rozumný prostor rozhraní API pro uživatele, které chcete prozkoumat.

Integrace sestavení je efektivně most mezi aplikace nebo sady SDK, které vás zajímají a relace. Můžete získat nové rozhraní API, která smysl konkrétně v kontextu sešitu nebo za provozu zkontrolujte relace, nebo zadejte žádné veřejná rozhraní API a jednoduše provádět "pozadí" úlohy, jako je objekt [reprezentace](~/tools/workbooks/sdk/representations.md).

> [!NOTE]
> Rozhraní API, které musí být veřejné, ale nesmí být prezentované prostřednictvím IntelliSense může být označen obvykle `[EditorBrowsable (EditorBrowsableState.Never)]` atribut.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
