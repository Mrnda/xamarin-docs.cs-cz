---
title: Vkládání technologie .NET
description: 'Vložení rozhraní .NET umožňuje váš stávající kód rozhraní .NET (C#, F # a jiné) využijí z jinými programovací jazyky'
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: f5e778ef9ba31c1a9e880b9fc66c2e48ddb2420c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="net-embedding"></a>Vkládání technologie .NET

![Náhled](~/media/shared/preview.png)

Vložení rozhraní .NET umožňuje váš stávající kód rozhraní .NET (C#, F # a jiné) využívat jinými programovací jazyky a v různých různých prostředích.

To znamená, že pokud máte knihovny .NET, který chcete použít z vaší stávající aplikace pro iOS, můžete to udělat.   Nebo pokud chcete propojit s nativní knihovny C++, můžete také to udělat.   Nebo využívat kód .NET z prostředí Java.

## <a name="environments-and-languages"></a>Prostředí a jazyky

Tento nástroj je i informace o prostředí, ve kterém se bude používat, stejně jako jazyk, který bude využívat.   Platforma iOS například neumožňuje kompilace za běhu (JIT), tak embeddinator staticky kompilaci kódu .NET do nativního kódu, který lze použít v iOS.  Jiných prostředích povolit JIT – kompilace a v těchto enviroments jsme zapojit do JIT kompilace.

Podporuje různé spotřebiteli jazyk, se zobrazí kód .NET jako idiomatickou kód v jazyce cíl.   Toto je seznam podporovaných jazyků v současné době:

- [**Jazyka Objective-C** ](objective-c/index.md) – mapování .NET idiomatickou rozhraní API jazyka Objective-C.
- [**Java** ](android/index.md) – mapování .NET idiomatickou rozhraní API Java.
- **C**: mapování .NET na objektově orientovaný jako C rozhraní API.

Další jazyky budou pocházet později.

## <a name="getting-started"></a>Začínáme

Abyste mohli začít, zkontrolujte jeden z naší příručky pro každou z aktuálně podporované jazyky:

- [**Jazyka Objective-C** ](get-started/objective-c/index.md) – platí systému macOS a iOS.
- [**Java** ](get-started/java/index.md) – obsahuje systému macOS a Android.
- [**C** ](get-started/c.md) – obsahuje jazyka C na ploše platformách.


## <a name="related-links"></a>Související odkazy

- [4000 Embeddinator na Githubu](https://github.com/mono/Embeddinator-4000)
