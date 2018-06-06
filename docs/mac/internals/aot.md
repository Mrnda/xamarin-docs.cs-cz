---
title: Xamarin.Mac před doba kompilace
description: Tento dokument popisuje před doba kompilace v Xamarin.Mac. Porovná AOT kompilace JIT – kompilace, vysvětluje, jak povolit AOT a se podíváme na hybridní AOT.
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: ec8474293fbb7372529e0f850e2d16db7ebf17be
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792236"
---
# <a name="xamarinmac-ahead-of-time-compilation"></a>Xamarin.Mac před doba kompilace

## <a name="overview"></a>Přehled

Článek času (AOT) kompilace je technika výkonné optimalizace pro zlepšení výkonu při spuštění. Ale taky ovlivňuje vaše čase vytvoření buildu, velikost aplikace a spuštění programu velký způsoby. Zjistit kompromisy, které ukládá vytvoříme trochu podrobně kompilace a spuštění aplikace.

K reprezentaci zprostředkující názvem IL je zkompilovat kód napsaný v spravované jazyky, jako je C# a F #. Tato IL uložené v knihovny a program sestavení je poměrně compact a přenosné mezi architekturami procesoru. IL, je však pouze středně pokročilé nastavení pokynů a v určitém okamžiku, který bude potřeba IL přeložit na zkompilovaný kód, které jsou specifické pro procesor.

Existují dva body, ve kterých lze provést zpracování:

- **Právě v čase (JIT)** – během spuštění a provádění IL kompiluje v paměti počítače kódu aplikace.
- **Napřed času (AOT)** – během vytváření sestavení IL kompiluje a zapsané do nativní knihovny a uložit do vaší aplikace sady.

Každá možnost má několik výhod a kompromisy:

- **JIT**
  - **Čas spuštění** – JIT – kompilace je třeba provést při spuštění. Pro většinu aplikací jde v řádu 100ms, ale pro velké aplikace může být této doby výrazně Další.
  - **Provádění** – jako za běhu kódu lze optimalizovat pro specifické procesory používá, může být generována mírně lepší kódu. Ve většině aplikací maximálně jde několika bodů procento rychlejší.
- **AOT**
  - **Čas spuštění** – načítání předem kompilovaném knihovny dylibs je mnohem rychlejší než JIT sestavení.
  - **Místo na disku** – tyto knihovny dylibs může trvat ale významné množství místa na disku. V závislosti na tom, které sestavení jsou AOTed, můžete dvakrát nebo větší velikost části kódu aplikace.
  - **Čas sestavení** – kompilace AOT je výrazně pomalejší této JIT a zpomalí sestavení jeho použití. Tato zpomalení rozsahu sekund až několik minut nebo víc, v závislosti na velikosti a počtu sestavení zkompilovat.
  - **Maskováním** – jako IL, které je výrazně jednodušší pro zpětnou než zkompilovaný kód, není nezbytně nutné můžete se odstraní pomohou obfuskováním citlivé kódu. To vyžaduje "Hybridní" možnost popisují níže.

## <a name="enabling-aot"></a>Povolení AOT

Možnosti AOT přidá do podokna Mac sestavení v budoucí aktualizaci. Do té doby se povolení AOT vyžaduje předávání přes pole "Další zhr argumenty" v sestavení Mac argument příkazového řádku. Možnosti jsou následující:


      --aot[=VALUE]          Specify assemblies that should be AOT compiled
                               - none - No AOT (default)
                               - all - Every assembly in MonoBundle
                               - core - Xamarin.Mac, System, mscorlib
                               - sdk - Xamarin.Mac.dll and BCL assemblies
                               - |hybrid after option enables hybrid AOT which
                               allows IL stripping but is slower (only valid
                               for 'all')
                                - Individual files can be included for AOT via +
                               FileName.dll and excluded via -FileName.dll

                               Examples:
                                 --aot:all,-MyAssembly.dll
                                 --aot:core,+MyOtherAssembly.dll,-mscorlib.dll



## <a name="hybrid-aot"></a>Hybridní AOT

Při spuštění aplikace systému macOS modulu runtime načíst výchozí hodnota je pomocí zkompilovaný kód z nativní knihovny vyprodukované AOT kompilace. Existují však některé oblasti kódu třeba trampolines, kde JIT – kompilace může způsobit výrazně více optimalizované výsledky. To vyžaduje IL spravovaná sestavení být k dispozici. V systému iOS jsou omezené aplikace z jakékoli použití JIT – kompilace; Tyto části kódu jsou AOT také zkompilovat.

Hybridní možnost instruuje kompilátor, aby obě kompilace tyto části (jako je iOS) ale také k předpokládají, že IL nebudete mít k dispozici v době běhu. Potom můžete se odstraní tento IL post sestavení. Jak jsme uvedli výše, bude vynutit použití méně optimalizované rutin, v některých místech modulu runtime.

## <a name="further-considerations"></a>Další informace

Záporné důsledky AOT škálování se velikosti a počtu sestavení zpracovat. Kompletní [cílové rozhraní](~/mac/platform/target-framework.md) pro příklad obsahuje výrazně větší základní třídy knihovny (BCL) než moderních, a proto bude AOT trvat výrazně déle a vytvořit větší sady. To je kombinovaných pomocí úplné cílové rozhraní nekompatibilita s propojení, které odstraní nepoužívané kódu. Zvažte přesunutí vaše aplikace a moderní a povolení propojování nejlepších výsledků.

Další výhodou AOT obsahuje vylepšené interakce s nativní ladění a profilování toolchains. Vzhledem k tomu, že velká většina základu kódu, se zkompiluje dopředu, bude mít názvy funkcí a symboly, které se snadněji číst uvnitř nativní havárií sestavy, ladění a profilování. Funkce JIT generované nemají tyto názvy a často zobrazují jako nepojmenované šestnáctkových posuny, které jsou velmi obtížné vyřešit.
