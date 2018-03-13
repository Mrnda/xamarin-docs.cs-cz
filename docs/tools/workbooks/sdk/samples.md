---
title: "Ukázka integrace"
ms.topic: article
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: f71da0f522c6c028981637a9797c3836063516f0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="sample-integrations"></a>Ukázka integrace

## <a name="sample-integrations"></a>Ukázka integrace

Najdete v článku [kuchyně jímky] [ KitchenSink] ukázku například pracovní integrace. Jednoduše sestavení `KitchenSink.sln` v sadě Visual Studio pro Mac nebo Visual Studio a pak otevřete `KitchenSink.workbook`.

[![Snímek obrazovky integrace kuchyně jímka](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

Ukázka kuchyně jímky ukazuje obě sady koncepty:

* Reprezentace části ukazují, jak používat `RepresentationManager` k rozšíření vykreslování pomocí předdefinovaných reprezentace.
* `Person` Objekt a jeho přidružené renderer JavaScript demonstruje použití `ISerializableObject` bez průchodu přes zprostředkovatele reprezentace.

Informace v tématu [SkiaSharp] [ skiasharp] například reálného integrace, který používá existující [reprezentace](~/tools/workbooks/sdk/representations.md) poskytované sešity Xamarin k vykreslení jeho typů.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks