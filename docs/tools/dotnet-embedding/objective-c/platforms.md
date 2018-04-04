---
title: Platformy jazyka Objective-C
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: a783dc554f54922f4fa4f99a43d83c4c864ef681
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="objective-c-platforms"></a>Platformy jazyka Objective-C


Vložení .NET, můžete vybrat různé platformy při generování kódu jazyka Objective-C:

* macOS
* iOS
* tvOS
* watchOS [není dosud implementována]

Platforma je vybrána předáním `--platform=<platform>` argument příkazového řádku k embeddinator.

Při sestavování pro platformy iOS, tvOS a watchOS, embeddinator vždy vytvoří rozhraní, které se vloží Xamarin.iOS, protože Xamarin.iOS obsahuje velké množství runtime podporu kód, který je vyžadován na těchto platformách.

Ale při vytváření pro platformu systému macOS, je možné zvolit, zda by měl rozhraní vygenerovaný vložení Xamarin.Mac nebo ne. Je možné, pokud vázané sestavení neodkazuje Xamarin.Mac.dll (přímo ani nepřímo) a tato možnost je vybrána předáním není vložit Xamarin.Mac `--platform=macOS` k embeddinator.

Pokud vázané sestavení obsahuje odkaz na Xamarin.Mac.dll, je nutné vložit Xamarin.Mac a kromě embeddinator musí vědět, které cílové rozhraní používat.

Existují tři možné cílové rozhraní Xamarin.Mac: `modern` (označovaly jako `mobile`), `full` a `system` (rozdíl mezi jednotlivými je popsaná v Xamarin.Mac na [cílové rozhraní] [ 1] dokumentace), a každou je vybraná předáním `--platform=macOS-modern`, `--platform=macOS-full` nebo `--platform=macOS-system` k embeddinator.

[1]: ~/mac/platform/target-framework.md
