---
title: Platformy jazyka Objective-C
description: Tento dokument popisuje různé platformy, které vložení .NET, můžete vybrat při práci s kódem jazyka Objective-C. Popisuje systému macOS, iOS, tvOS a watchOS.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 6eeb776959d1a2a37d67bfae6603971d0e5a22b7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793888"
---
# <a name="objective-c-platforms"></a>Platformy jazyka Objective-C

Vložení .NET, můžete vybrat různé platformy při generování kódu jazyka Objective-C:

* macOS
* iOS
* tvOS
* watchOS [není dosud implementována]

Platforma je vybrána předáním `--platform=<platform>` argument příkazového řádku k vložení .NET.

Při sestavování pro iOS, tvOS a watchOS platformy .NET vložení vždy vytvoří rozhraní, které se vloží Xamarin.iOS, protože Xamarin.iOS obsahuje velké množství runtime podporu kód, který je vyžadován na těchto platformách.

Ale při vytváření pro platformu systému macOS, je možné zvolit, zda by měl rozhraní vygenerovaný vložení Xamarin.Mac nebo ne. Je možné, pokud vázané sestavení neodkazuje Xamarin.Mac.dll (přímo ani nepřímo) a tato možnost je vybrána předáním není vložit Xamarin.Mac `--platform=macOS` k nástroji vložení .NET.

Pokud vázané sestavení obsahuje odkaz na Xamarin.Mac.dll, je nutné vložit Xamarin.Mac a kromě embeddinator musí vědět, které cílové rozhraní používat.

Existují tři možné cílové rozhraní Xamarin.Mac: `modern` (označovaly jako `mobile`), `full` a `system` (rozdíl mezi jednotlivými je popsaná v Xamarin.Mac na [cílové rozhraní] [ 1] dokumentace), a každou je vybraná předáním `--platform=macOS-modern`, `--platform=macOS-full` nebo `--platform=macOS-system` k nástroji vložení .NET.

[1]: ~/mac/platform/target-framework.md
