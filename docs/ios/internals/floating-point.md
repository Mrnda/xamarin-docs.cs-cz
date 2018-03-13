---
title: "Plovoucí desetinnou čárkou"
ms.topic: article
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 73681a37f15f3dd93c85bafb6bb9d71ab30af85c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="floating-point"></a>Plovoucí desetinnou čárkou

Xamarin.iOS ve výchozím nastavení provede 32bitové a 64bitové verze operace s plovoucí desetinnou pomocí 64-bit přesnost na ARM.  

Při této vyšší přesnost je blíž ke vývojáři očekávat od plovoucí bodu operací v jazyce C# na ploše na mobilních zařízeních, může být významný dopad na výkon.

Je možné zkompilovat 32-bit plovoucí bodu kódu k použití operace s plovoucí desetinnou 32-bit.  K tomuto účelu, budete muset použít alespoň Xamarin.iOS 8.10 a sada v iOS sestavení možnosti na panelu "mtouch další argumenty" záznam řádku následující hodnotu:

     --aot-options=-O=float32

To bude informovat statické kompilátory (Mono na předdefinované statické kompilátoru, nebo jeden používá technologii LLVM) k provedení operace s plovoucí desetinnou pomocí 32bitové obtékaných objektů.
