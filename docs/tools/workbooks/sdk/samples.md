---
title: Ukázka integrace
description: Tento dokument obsahuje odkazy na ukázky, která ukazují integrace sešity Xamarin. Propojené ukázky pracovat s reprezentace vykreslování a SkiaSharp.
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 75d88af4c294a35d45f05724eb96ce822cddf126
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793968"
---
# <a name="sample-integrations"></a>Ukázka integrace

Najdete v článku [kuchyně jímky] [ KitchenSink] ukázku například pracovní integrace. Jednoduše sestavení `KitchenSink.sln` v sadě Visual Studio pro Mac nebo Visual Studio a pak otevřete `KitchenSink.workbook`.

[![Snímek obrazovky integrace kuchyně jímka](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

Ukázka kuchyně jímky ukazuje obě sady koncepty:

* Reprezentace části ukazují, jak používat `RepresentationManager` k rozšíření vykreslování pomocí předdefinovaných reprezentace.
* `Person` Objekt a jeho přidružené renderer JavaScript demonstruje použití `ISerializableObject` bez průchodu přes zprostředkovatele reprezentace.

Informace v tématu [SkiaSharp] [ skiasharp] například reálného integrace, který používá existující [reprezentace](~/tools/workbooks/sdk/representations.md) poskytované sešity Xamarin k vykreslení jeho typů.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks
