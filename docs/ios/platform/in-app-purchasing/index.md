---
title: Zakoupení v Xamarin.iOS v aplikaci
description: Tento dokument popisuje, jak prodávat digitální produkty a služby pomocí rozhraní API StoreKit. Odkazuje příručky, které popisují konfigurace, použití produktů, -nespotřebitelné produktů, transakce, odběry a další.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 8a41ed44a331c91a333b95c1d62136244a6945dd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787338"
---
# <a name="in-app-purchasing-in-xamarinios"></a>Zakoupení v Xamarin.iOS v aplikaci

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

 * [Přehled StoreKit a načítání informací o produktu](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

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
