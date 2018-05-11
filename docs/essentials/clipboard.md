---
title: Xamarin.Essentials schránky
description: Třída schránky umožňuje kopírování a vložení textu do schránky systému mezi aplikacemi.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 44ba8090851b65327682b3d290d58fb23bb8aae4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
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

- [Schránky zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Essentials/Clipboard)
- [Dokumentace rozhraní API schránky](xref:Xamarin.Essentials.Clipboard)
