---
title: 'Xamarin.Essentials: schránky'
description: Tento dokument popisuje třídy schránky v Xamarin.Essentials, která umožňuje kopírování a vložení textu do schránky systému mezi aplikacemi.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38842612"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: schránky

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Schránky** třída umožňuje kopírování a vložení textu do schránky systému mezi aplikacemi.

## <a name="using-clipboard"></a>Použití schránky

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

A zkontrolujte, zda **schránky** je nyní připravena, aby ho šlo vložit text:

```csharp
var hasText = Clipboard.HasText;
```

Nastavit text **schránky**:

```csharp
Clipboard.SetText("Hello World");
```

Čtení textu z **schránky**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>rozhraní API

- [Schránka zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Dokumentace k rozhraní API schránky](xref:Xamarin.Essentials.Clipboard)
