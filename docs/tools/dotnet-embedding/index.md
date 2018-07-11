---
title: Vkládání technologie .NET
description: 'Vkládání technologie .NET umožňuje váš stávající kód technologie .NET (C#, F # a dalších) Chcete-li být zaplněny kódem napsaným v jiných programovacích jazycích.'
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 16f59498a49d10a43e04989136d8835bf78bd89d
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38830397"
---
# <a name="net-embedding"></a>Vkládání technologie .NET

![Náhled](~/media/shared/preview.png)

Vkládání technologie .NET umožňuje váš stávající kód technologie .NET (C#, F # a dalších) který se má používat v jiných programovacích jazycích a v různých prostředích různé.

To znamená, že pokud máte knihovnu .NET, kterou chcete použít ze své stávající aplikace pro iOS, můžete to udělat.   Nebo pokud chcete propojit s nativní knihovnu C++, můžete také to udělat.   Nebo využívají kódu .NET z Javy.

Vkládání technologie .NET je založená na [Embeddinator 4000](https://github.com/mono/Embeddinator-4000) projekt open source.

## <a name="environments-and-languages"></a>Prostředí a jazyky

Tento nástroj je i přehled o prostředí, ve kterém se bude používat, stejně jako jazyk, který bude využívat.   Například na platformu iOS neumožňuje kompilace just-in-time (JIT), takže vkládání technologie .NET staticky zkompiluje kód .NET do nativního kódu, který lze použít v iOS.  Další prostředí povolit kompilaci JIT a v těchto prostředích jsme vyjádřit souhlas se službou kompilace JIT.

Podporuje různé jazykové spotřebitele, takže plochy kódu .NET podle kódu idiomatickou v cílovém jazyce.   Toto je seznam podporovaných jazyků v současné době:

- [**Objective-C** ](objective-c/index.md) – mapování .NET idiomatickou rozhraní API jazyka Objective-C
- [**Java** ](android/index.md) – mapování .NET idiomatickou rozhraní Java API
- [**C** ](get-started/c.md) – mapování rozhraní .NET pro objektově orientované jako rozhraní API jazyka C

Další jazyky přijde později.

## <a name="getting-started"></a>Začínáme

Abyste mohli začít, najdete jednu z našich vodítka pro každou z aktuálně podporované jazyky:

- [**Objective-C** ](get-started/objective-c/index.md) – zahrnuje macOS a iOS
- [**Java** ](get-started/java/index.md) – zahrnuje macOS a Android
- [**C** ](get-started/c.md) – zahrnuje jazyka C na desktopové platformy

## <a name="related-links"></a>Související odkazy

- [Embeddinator 4000 na Githubu](https://github.com/mono/Embeddinator-4000)
