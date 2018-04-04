---
title: Backgrounding
description: Pozadí zpracování nebo backgrounding je proces, když je umožněno aplikace provádět úlohy na pozadí, zatímco jiná aplikace je spuštěná v popředí. Tato příručka slouží jako úvod ke zpracování v iOS na pozadí.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ae3e732008c6503f511dc4be9cad874ecfe1272d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="backgrounding"></a>Backgrounding

_Pozadí zpracování nebo backgrounding je proces, když je umožněno aplikace provádět úlohy na pozadí, zatímco jiná aplikace je spuštěná v popředí. Tato příručka slouží jako úvod ke zpracování v iOS na pozadí._

Backgrounding v mobilních aplikacích se zásadně liší od tradiční koncept multitasking na ploše. Stolní počítače mají různé druhy prostředků, které jsou k dispozici pro aplikaci, včetně obrazovky nemovitosti, napájení a paměti. Aplikace jsou lze spustit souběžně sdílená a zůstanou původce a dá se použít. Na mobilním zařízení prostředky jsou mnohem omezenější. Je obtížné zobrazit více než jednu aplikaci na malou obrazovku a spuštěných víc aplikací při plné rychlosti by vyprazdňování baterie. Backgrounding je konstantní kompromis mezi poskytnutí prostředků ke spuštění úlohy na pozadí, které potřebují k provedení dobře aplikace a zachování přizpůsobivý foregrounded aplikace a zařízení. IOS a Android má předpisy pro backgrounding, ale jejich zpracování velmi různými způsoby.

V iOS backgrounding rozpoznán jako stav aplikace a aplikace se přesunou do/z stav pozadí v závislosti na chování aplikaci a uživatele. iOS také nabízí několik možností pro vzájemné propojení aplikace ke spuštění na pozadí, třeba požádat OS dobu pro dokončení důležité úlohy, jako typ známé pozadí nezbytné aplikace, a aktualizovat obsah aplikace na webu intervaly.

V této příručce a doplňujícími návody přidáme další úkoly aplikace na pozadí. Jsme se týkají klíčových konceptů a osvědčené postupy a poté procházet vytvořit skutečných aplikaci, která získává aktualizace umístění na pozadí.

## <a name="contents"></a>Obsah

1.  [Úvod do zpracovávání úloh na pozadí v iOSu](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [Ukázka životního cyklu aplikace](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [Techniky zpracování úloh na pozadí v iOSu](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [Návody: Zpracování úloh na pozadí v iOSu](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [Doprovodné materiály ke zpracování úloh na pozadí v iOSu](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>Souhrn

V této příručce zavedli jsme různé způsoby plnění zpracování na pozadí v iOS. Jsme popsaná iOS aplikace stavy a zkontrolován roli backgrounding plní v iOS životního cyklu aplikace. Kromě toho jsme zjistili, jak jsme by se mohl zaregistrovat jednotlivé úlohy nebo celé aplikace fungovat na pozadí v iOS. Nakonec jsme zesílené naše pochopení backgrounding v systému iOS ve vytváření aplikací, které provedení aktualizace na pozadí.



## <a name="related-links"></a>Související odkazy

- [Backgrounding v systému Android](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (ukázka)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [Umístění (ukázka)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Jednoduché přenosu na pozadí (ukázka)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS provádění pozadí](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
