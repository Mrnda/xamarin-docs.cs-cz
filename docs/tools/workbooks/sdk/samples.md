---
title: "Ukázka integrace"
ms.topic: article
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 3b07e457bd54f16826f44a3b913434fea6ebe20b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="sample-integrations"></a>Ukázka integrace

## <a name="sample-integrations"></a>Ukázka integrace

Najdete v článku [kuchyně jímky] [ KitchenSink] ukázku například pracovní integrace. Jednoduše sestavení `KitchenSink.sln` v sadě Visual Studio pro Mac nebo Visual Studio a pak otevřete `KitchenSink.workbook`.

[![Snímek obrazovky integrace kuchyně jímka](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png)

Ukázka kuchyně jímky ukazuje obě sady koncepty:

* Reprezentace části ukazují, jak používat `RepresentationManager` k rozšíření vykreslování pomocí předdefinovaných reprezentace.
* `Person` Objekt a jeho přidružené renderer JavaScript demonstruje použití `ISerializableObject` bez průchodu přes zprostředkovatele reprezentace.

Informace v tématu [SkiaSharp] [ skiasharp] například reálného integrace, který používá existující [reprezentace](~/tools/workbooks/sdk/representations.md) poskytované sešity Xamarin k vykreslení jeho typů.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks