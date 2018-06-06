---
title: Pod pokličkou v Xamarin.Mac
description: Tento dokument obsahuje odkazy na různé příručky, které popisují vnitřního chodu Xamarin.Mac. Propojené dokumenty popisují před doba kompilace, Xamarin.Mac architektury a Xamarin.Mac registrátora.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: c940252a675c38247d2c5bb374b9c30237222bda
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792486"
---
# <a name="under-the-hood-in-xamarinmac"></a>Pod pokličkou v Xamarin.Mac

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Můžete některé z času kompilace (AOT)](aot.md)

Článek času (AOT) kompilace je technika výkonné optimalizace pro zlepšení výkonu při spuštění. Ale taky ovlivňuje vaše čase vytvoření buildu, velikost aplikace a spuštění programu velký způsoby, proto je vhodné pochopit, jak funguje.

## <a name="mac-architecturearchitecturemd"></a>[Architektura Mac](architecture.md)

Xamarin.Mac na relaci Objective-C, včetně konceptů například kompilace, selektory, registrátorů, aplikace a generátor.

## <a name="xamarinmac-registrarregistrarmd"></a>[Registrátor Xamarin.Mac](registrar.md)

Xamarin.Mac přemosťuje mezeru mezi spravované world a modul runtime na kakao, povolení spravované třídy volání nespravované třídy jazyka Objective-C a volat zpět, pokud dojde k událostem. Práce nutná provedení akce tento "magic" se zpracovává souborem registrátora, ale vysvětlení, co se děje "pod pokličkou" může být někdy užitečné.
