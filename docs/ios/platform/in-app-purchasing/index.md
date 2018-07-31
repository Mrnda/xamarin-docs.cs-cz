---
title: Nákup v Xamarin.iosu v aplikaci
description: Tento dokument popisuje, jak prodávat digitální produkty a služby pomocí rozhraní API StoreKit. To obsahuje odkazy na pokyny, které popisují konfiguraci, spotřebních produktů, nespotřebních produktů, transakce, předplatných a další.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 102ff2f11cc2f3d536e3ce9dd595a881f370f764
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351418"
---
# <a name="in-app-purchasing-in-xamarinios"></a>Nákup v Xamarin.iosu v aplikaci

aplikace pro iOS můžete prodávat digitálních produktů a služeb pomocí StoreKit – sadu rozhraní API poskytovaných iOS, které komunikují se servery společnosti Apple k provádění finančních transakcí s uživateli prostřednictvím zadání Apple ID. Rozhraní API StoreKit se týká především načítání informací o produktu a provádění transakcí – neexistuje žádná komponenta uživatelského rozhraní. Aplikace, které implementují nákupy v aplikaci musíte vytvářet své vlastní uživatelské rozhraní a sledovat zakoupené položky, které vlastní kód na poskytnutí požadovaných produktů nebo služeb pro uživatele.

Poskytuje funkce nákupy v aplikaci vyžaduje několik kroků:

-  **Konfigurace aplikace** – zřizovací profil aplikace musí být nastaveny správně.
-  **Vytváření produktů** – popisů produktů a ceny musí být vytvořené na portálu iTunes Connect.
-  **Implementace StoreKit** – rozhraní API StoreKit musí být implementován v souladu se typy produktů, které se prodávají.
-  **Vytváření uživatelského rozhraní a produkty, sami** – produkty musí být implementována, včetně mechanismů, abyste mohli sledovat každému svému nákupu a zálohování a obnovení je v případě potřeby.
-  **Sledování prodejních a přijímání fondů** – použijte informace uvedené v iTunes Connectu ke sledování prodejních trendů a sledovat příjmy.

Tento dokument vysvětluje, jak dokončit tyto kroky k poskytování nákupy v aplikaci pomocí Xamarin.iOS.

## <a name="requirements"></a>Požadavky

Pro podporu nákupy v aplikaci je nutné použít Xamarin.iOS 5.0 nebo novější s Xcode 7 a novějším.

## <a name="contents"></a>Obsah

 * [Základní informace a konfigurace nákupů v aplikaci](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [Přehled StoreKit a načítání informací o produktu](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [Nákup spotřebních produktů](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [Nákup nespotřebních produktů](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [Transakce a ověření](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [Předplatná a sestavy](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## <a name="summary"></a>Souhrn

Tento článek má představil nový koncept nákupy v aplikaci, popsané postupy konfigurace aplikace pro využívat jejich výhod a uvedené příklady použití Xamarin.iOS. Zahrnují:

-  **Portál zřizování iOS** – pokyny pro povolení v aplikaci zakoupit funkce.
-  **iTunes Connect** – Konfigurujeme produkty, aby je prodal ve vaší aplikaci.
-  **Store Kit** – Vysvětlení tříd použit k vytvoření funkce nákupy v aplikaci.
-  **Psaní kódu vaší aplikace při nákupu** – příklady, jak začlenit do aplikace Xamarin.iOS nákupy v aplikaci.
-  **Vytváření sestav** – přehled statistiky dostupná přes iTunes Connect.


## <a name="related-links"></a>Související odkazy

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [V Průvodci programováním nákupy aplikaci](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect – Příručka pro vývojáře](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Odkaz na Store rozhraní Kit](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [Nákupy v aplikaci produktu identifikátory funkce Q & A](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [Technická poznámka nákupy v aplikaci](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [Váš příspěvek první App Store](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [Centrum prostředků App Store](https://developer.apple.com/appstore/index.html)
- [Tipy k odeslání App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Pokyny pro recenze v App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Správa aplikací](https://developer.apple.com/appstore/resources/managing/index.html)
