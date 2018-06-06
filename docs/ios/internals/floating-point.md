---
title: Plovoucí bodu operace v Xamarin.iOS
description: Tento dokument popisuje, jak Xamarin.iOS zpracovává 32bitové a 64bitové verze přesnost operací s pohyblivou čárkou a přidružené dopad na výkon.
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ea5d69b52cbd4c76abb236bd1a272633dde440b7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786158"
---
# <a name="floating-point-operations-in-xamarinios"></a>Plovoucí bodu operace v Xamarin.iOS

Xamarin.iOS ve výchozím nastavení provede 32bitové a 64bitové verze operace s plovoucí desetinnou pomocí 64-bit přesnost na ARM.  

Při této vyšší přesnost je blíž ke vývojáři očekávat od plovoucí bodu operací v jazyce C# na ploše na mobilních zařízeních, může být významný dopad na výkon.

Je možné zkompilovat 32-bit plovoucí bodu kódu k použití operace s plovoucí desetinnou 32-bit.  K tomuto účelu, budete muset použít alespoň Xamarin.iOS 8.10 a sada v iOS sestavení možnosti na panelu "mtouch další argumenty" záznam řádku následující hodnotu:

     --aot-options=-O=float32

To bude informovat statické kompilátory (Mono na předdefinované statické kompilátoru, nebo jeden používá technologii LLVM) k provedení operace s plovoucí desetinnou pomocí 32bitové obtékaných objektů.
