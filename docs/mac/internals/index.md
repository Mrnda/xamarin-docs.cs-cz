---
title: "Pod pokličkou"
description: "Funkce Náhled na vnitřního chodu Xamarin.Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 2ba3ffb421dc64bba7df1e10a40125f14365f29e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="under-the-hood"></a>Pod pokličkou

_Funkce Náhled na vnitřního chodu Xamarin.Mac_

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Můžete některé z času kompilace (AOT)](aot.md)

Článek času (AOT) kompilace je technika výkonné optimalizace pro zlepšení výkonu při spuštění. Ale taky ovlivňuje vaše čase vytvoření buildu, velikost aplikace a spuštění programu velký způsoby, proto je vhodné pochopit, jak funguje.

## <a name="mac-architecturearchitecturemd"></a>[Architektura Mac](architecture.md)

Xamarin.Mac na relaci Objective-C, včetně konceptů například kompilace, selektory, registrátorů, aplikace a generátor.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac registrar](registrar.md)

Xamarin.Mac přemosťuje mezeru mezi spravované world a modul runtime na kakao, povolení spravované třídy volání nespravované třídy jazyka Objective-C a volat zpět, pokud dojde k událostem. Práce nutná provedení akce tento "magic" se zpracovává souborem registrátora, ale vysvětlení, co se děje "pod pokličkou" může být někdy užitečné.