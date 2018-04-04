---
title: V aplikaci nákupu
description: aplikace pro iOS můžete prodeje digitální produkty a služby pomocí rozhraní API úložiště Kit. Produkty jsou vytvořit a spravovat na portálu Connect iTunes. Apple spravuje zpracování transakcí a schválí všechny produkty, než může být prodaných a účtuje poplatek za jednotlivé transakce (aktuálně 30 %). Apple vyžaduje, aby používáte v aplikaci nákupu pro všechny digitální prodej ve vaší aplikaci, ale nemůžete je využít pro prodej fyzické zboží nebo služeb bez digitální. Aplikace, které nabízí možnosti alternativní platby pro digitální produkty a služby se pravděpodobně odmítnuty. Tento dokument vysvětluje postup konfigurace aplikace k používání úložiště Kit a obsahuje příklady Xamarin.iOS většiny běžných nákupu scénáře v aplikaci.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 7a8dec6051caeba55c45df29c085ecfcddd160d2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="in-app-purchasing"></a>V aplikaci nákupu

_aplikace pro iOS můžete prodeje digitální produkty a služby pomocí rozhraní API úložiště Kit. Produkty jsou vytvořit a spravovat na portálu Connect iTunes. Apple spravuje zpracování transakcí a schválí všechny produkty, než může být prodaných a účtuje poplatek za jednotlivé transakce (aktuálně 30 %). Apple vyžaduje, aby používáte v aplikaci nákupu pro všechny digitální prodej ve vaší aplikaci, ale nemůžete je využít pro prodej fyzické zboží nebo služeb bez digitální. Aplikace, které nabízí možnosti alternativní platby pro digitální produkty a služby se pravděpodobně odmítnuty. Tento dokument vysvětluje postup konfigurace aplikace k používání úložiště Kit a obsahuje příklady Xamarin.iOS většiny běžných nákupu scénáře v aplikaci._


aplikace pro iOS můžete prodeje digitální produktech či službách pomocí StoreKit – sadu rozhraní API poskytované iOS, která se komunikovat se servery společnosti Apple k provedení finanční transakce se uživatele na základě jejich Apple ID. Rozhraní API StoreKit se týká především načítání informací o produktu a provádění transakcí – neexistuje žádná součást uživatelského rozhraní. Aplikace, které implementují nákupu v aplikaci musíte vytvořit své vlastní uživatelské rozhraní a sledování zakoupených položek s vlastní kód k poskytování požadované produkty nebo služby pro uživatele.

Poskytuje funkce nákupy v aplikaci vyžaduje několik kroků:

-  **Konfigurování aplikace** – profil pro zřizování aplikace musí být správně nainstalována.
-  **Vytváření produkty** – popisy produktu a ceny musí být vytvořený na portálu Connect iTunes.
-  **Implementace StoreKit** – rozhraní API StoreKit musí být implementována podle typy prodává produkty.
-  **Vytváření uživatelského rozhraní a produkty sami** – produkty musí být implementována, včetně mechanismy pro sledování každý nákupu a zálohování nebo obnovení je podle potřeby.
-  **Monitorování prodeje a přijetí fondů** – použijte informace poskytované iTunes připojit ke sledování prodejním trendům a příjmy a sledovat.


Tento dokument vysvětluje, jak provést tyto kroky zajistit, že pomocí Xamarin.iOS nákupy v aplikaci.


## <a name="requirements"></a>Požadavky

Pro podporu nákupu v aplikaci je nutné použít Xamarin.iOS 5.0 nebo novější s Xcode 7 a vyšší.

## <a name="contents"></a>Obsah

 * [Základní informace a konfigurace nákupů v aplikaci](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [Přehled StoreKitu a načítání informací o produktu](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [Nákup spotřebních produktů](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [Nákup nespotřebních produktů](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [Transakce a ověření](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [Předplatná a sestavy](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)


## <a name="summary"></a>Souhrn

Tento článek má zaveden koncept nákupu v aplikaci, uvedených postup konfigurace vaše aplikace a využívat jejich výhod a uvedené příklady použití Xamarin.iOS. Které:

-  **iOS Provisioning Portal** – pokyny pro povolení v aplikaci zakoupit funkce.
-  **iTunes Connect** – konfiguraci produktů, aby je prodal ve vaší aplikaci.
-  **Uložení Kit** – Vysvětlení třídy používané k vytvoření funkce nákupy v aplikaci.
-  **Kódování aplikace k nákupu** – příklady, jak sestavit nákupy v aplikaci do aplikace Xamarin.iOS.
-  **Vytváření sestav** – přehled statistiky, které jsou k dispozici prostřednictvím iTunes připojit.


## <a name="related-links"></a>Související odkazy

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [V Průvodci programováním nákupy aplikaci](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect – Příručka vývojáře](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Úložiště Kit Framework – referenční informace](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [Nákupy v aplikaci produktu identifikátory otázek a odpovědí](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [Technická poznámka nákupy v aplikaci](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [Vaše žádost první App Storu](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [Centrum prostředků App Store](https://developer.apple.com/appstore/index.html)
- [Tipy pro odeslání obchodu s aplikacemi](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Pokyny pro recenze v App Storu](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Řízení aplikace](https://developer.apple.com/appstore/resources/managing/index.html)
