---
title: Xamarin.Essentials schránky
description: Třída schránky umožňuje kopírování a vložení textu do schránky systému mezi aplikacemi.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 67a0218325918b57e5ed2618b57d52d3fe3ee820
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials schránky

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
ClipBoard.SetText("Hello World");
```

Čtení textu ze **schránky**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>rozhraní API

- [Schránky zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Dokumentace rozhraní API schránky](xref:Xamarin.Essentials.Clipboard)
