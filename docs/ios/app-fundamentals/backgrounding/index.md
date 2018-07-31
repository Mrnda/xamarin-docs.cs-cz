---
title: Zpracování úloh na pozadí v Xamarin.iosu
description: Pozadí zpracování nebo zpracování úloh na pozadí je proces aplikace provádět úlohy na pozadí, zatímco jiná aplikace je spuštěná v popředí. Tato příručka slouží jako úvod ke zpracování v Iosu na pozadí.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2018
ms.openlocfilehash: 73344b790bf6d4719d9a92cfa9146578dffe04e9
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350765"
---
# <a name="backgrounding-in-xamarinios"></a>Zpracování úloh na pozadí v Xamarin.iosu

_Pozadí zpracování nebo zpracování úloh na pozadí je proces aplikace provádět úlohy na pozadí, zatímco jiná aplikace je spuštěná v popředí. Tato příručka slouží jako úvod ke zpracování v Iosu na pozadí._

Zpracování úloh na pozadí v mobilních aplikacích se zásadně liší od tradiční konceptu multitaskingu v klientských počítačích. Stolní počítače mají různé prostředky dostupné pro aplikace, včetně plochy obrazovky, výkon a paměť. Aplikace jsou schopni spustit vedle sebe a zůstat výkonné a dá se použít. Na mobilním zařízení jsou mnohem méně prostředků. Je obtížné zobrazit více než jednu aplikaci na malé obrazovce a spuštěných víc aplikací při plné rychlosti by vybíjení baterie. Zpracování úloh na pozadí je konstantní kompromis mezi poskytuje prostředky ke spuštění úlohy na pozadí, které potřebují k provedení dobře aplikací a aby responzivní foregrounded aplikace a zařízení. IOS a Android mají opatření pro zpracování úloh na pozadí, ale jejich zpracování zcela novými způsoby.

V Iosu zpracování úloh na pozadí je považován za stav aplikace a aplikace se přesunou do stavu na pozadí v závislosti na chování uživatele a aplikace. iOS také nabízí několik možností pro vzájemné propojení aplikace běžet na pozadí, včetně s dotazem, operační systém pro čas k dokončení důležité úlohy, jako typ aplikace na pozadí potřeby známé a aktualizovat obsah aplikace v určených intervaly.

V této příručce a doprovodných návody budeme se naučíte, jak provádět aplikace úlohy na pozadí. Budeme se věnují klíčové koncepty a osvědčené postupy a poté krokovat s vytvářením reálné aplikace, která bude přijímat aktualizace polohy na pozadí.

## <a name="contents"></a>Obsah

1.  [Úvod do zpracovávání úloh na pozadí v iOSu](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [Ukázka životního cyklu aplikace](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [Techniky zpracování úloh na pozadí v iOSu](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [Návody: Zpracování úloh na pozadí v iOSu](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [Doprovodné materiály ke zpracování úloh na pozadí v iOSu](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>Souhrn

V této příručce zavedli jsme různé způsoby provedení zpracování na pozadí v Iosu. Zahrnuté stavy aplikace iOS jsme prozkoumat roli zpracování úloh na pozadí jde skvěle dohromady v Iosu životního cyklu aplikací. Kromě toho jsme zjistili, jak jsme mohli zaregistrovat jednotlivých úloh nebo celé aplikace pracovat na pozadí v Iosu. Nakonec jsme posílit naše porozumění zpracování úloh na pozadí v Iosu vytvořením aplikace, které provádějí aktualizace na pozadí.



## <a name="related-links"></a>Související odkazy

- [Zpracování úloh na pozadí v Androidu](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (ukázka)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [Umístění (ukázka)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Jednoduché přenos na pozadí (ukázka)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS zpracování na pozadí](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
