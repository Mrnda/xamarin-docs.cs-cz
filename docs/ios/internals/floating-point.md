---
title: "Plovoucí desetinnou čárkou"
ms.topic: article
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ea29132ad4ac55f6fb151ac2125ab1add82c8518
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="floating-point"></a>Plovoucí desetinnou čárkou

Xamarin.iOS ve výchozím nastavení provede 32bitové a 64bitové verze operace s plovoucí desetinnou pomocí 64-bit přesnost na ARM.  

Při této vyšší přesnost je blíž ke vývojáři očekávat od plovoucí bodu operací v jazyce C# na ploše na mobilních zařízeních, může být významný dopad na výkon.

Je možné zkompilovat 32-bit plovoucí bodu kódu k použití operace s plovoucí desetinnou 32-bit.  K tomuto účelu, budete muset použít alespoň Xamarin.iOS 8.10 a sada v iOS sestavení možnosti na panelu "mtouch další argumenty" záznam řádku následující hodnotu:

     --aot-options=-O=float32

To bude informovat statické kompilátory (Mono na předdefinované statické kompilátoru, nebo jeden používá technologii LLVM) k provedení operace s plovoucí desetinnou pomocí 32bitové obtékaných objektů.
