---
title: Vkládání technologie .NET
description: 'Vložení rozhraní .NET umožňuje váš stávající kód rozhraní .NET (C#, F # a jiné) využijí z jinými programovací jazyky'
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 505c2902f2b8d112597b4b9b9b07282a7810db68
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="net-embedding"></a>Vkládání technologie .NET

![Náhled](~/media/shared/preview.png)

Vložení rozhraní .NET umožňuje váš stávající kód rozhraní .NET (C#, F # a jiné) využívat jinými programovací jazyky a v různých různých prostředích.

To znamená, že pokud máte knihovny .NET, který chcete použít z vaší stávající aplikace pro iOS, můžete to udělat.   Nebo pokud chcete propojit s nativní knihovny C++, můžete také to udělat.   Nebo využívat kód .NET z prostředí Java.

Vložení .NET je založena na [Embeddinator 4000](https://github.com/mono/Embeddinator-4000) open source projektu.

## <a name="environments-and-languages"></a>Prostředí a jazyky

Tento nástroj je i informace o prostředí, ve kterém se bude používat, stejně jako jazyk, který bude využívat.   Platforma iOS například neumožňuje kompilace za běhu (JIT), tak vložení .NET bude staticky kompilaci rozhraní .NET kódu do nativního kódu, který lze použít v iOS.  Jiných prostředích povolit JIT – kompilace a v těchto enviroments jsme zapojit do JIT kompilace.

Podporuje různé spotřebiteli jazyk, se zobrazí kód .NET jako idiomatickou kód v jazyce cíl.   Toto je seznam podporovaných jazyků v současné době:

- [**Jazyka Objective-C** ](objective-c/index.md) – mapování .NET idiomatickou rozhraní API jazyka Objective-C
- [**Java** ](android/index.md) – mapování .NET idiomatickou rozhraní API Java
- [**C** ](get-started/c.md) – mapování .NET na objektově orientované jako rozhraní API jazyka C

Další jazyky budou pocházet později.

## <a name="getting-started"></a>Začínáme

Abyste mohli začít, zkontrolujte jeden z naší příručky pro každou z aktuálně podporované jazyky:

- [**Jazyka Objective-C** ](get-started/objective-c/index.md) – platí systému macOS a iOS
- [**Java** ](get-started/java/index.md) – obsahuje systému macOS a Android
- [**C** ](get-started/c.md) – obsahuje jazyka C na ploše platformy

## <a name="related-links"></a>Související odkazy

- [4000 Embeddinator na Githubu](https://github.com/mono/Embeddinator-4000)
