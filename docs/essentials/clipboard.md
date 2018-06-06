---
title: 'Xamarin.Essentials: schránky'
description: Tento dokument popisuje třídy schránky v Xamarin.Essentials, která vám umožní zkopírovat a vložit text do schránky systému mezi aplikacemi.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782356"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: schránky

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Schránky** třída umožňuje kopírování a vložení textu do schránky systému mezi aplikacemi.

## <a name="using-clipboard"></a>Použití schránky

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Ke kontrole, pokud **schránky** je nyní připraven k vložení textu:

```csharp
var hasText = Clipboard.HasText;
```

Nastavit text na **schránky**:

```csharp
Clipboard.SetText("Hello World");
```

Čtení textu ze **schránky**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>rozhraní API

- [Schránky zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Dokumentace rozhraní API schránky](xref:Xamarin.Essentials.Clipboard)
