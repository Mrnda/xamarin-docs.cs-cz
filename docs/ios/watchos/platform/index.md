---
title: funkce platformy pro watchOS
description: Tento dokument obsahuje odkazy na různá vodítka, které popisují funkce platformy watchOS jako oprávnění Apple Pay, oznámení, komplikace, proaktivní návrhy, aplikace cvičení a další.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: e915ec38ed29b6ef2a0710c9dad10cf339c3a3ab
ms.sourcegitcommit: cfb72be633e335147d156af3ef9527151b9e31d9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030675"
---
# <a name="watchos-platform-features"></a>funkce platformy pro watchOS

## <a name="introduction-to-watchos-5introduction-to-watchos5indexmd"></a>[Úvod do Watchosu 5](introduction-to-watchos5/index.md)

> [!WARNING]
> Podpora Xamarinu pro watchOS 5 je aktuálně ve verzi preview, což znamená, že může obsahovat chyb, které není plně funkční, a může změnit.
> Použijte pouze experimentování.

Tento dokument poskytuje základní přehled o nových a aktualizovaných funkcích watchOS 5, které jsou k dispozici pro použití při sestavování aplikace pro watchOS s využitím kódu Xamarin.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Úvod do watchOSu 4](introduction-to-watchos4.md)

Tento dokument obsahuje podrobný přehled funkcí a aktualizace v watchOS 4.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Úvod do watchOSu 3](introduction-to-watchos3/index.md)

Tento článek popisuje nové a aktualizované rozhraní API ve Watchosu 3.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Vylepšení oprávnění Apple Pay](~/ios/watchos/platform/apple-pay.md)

Ve Watchosu 3 rozhraní PassKit zvětšil povolit podporu pro zabezpečené, v aplikaci platby (z fyzického zboží a služeb) pro aplikace běžící na Apple Watch.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Úlohy na pozadí](~/ios/watchos/platform/background-tasks.md)

watchOS 3 zavádí několik úloh na pozadí, které aplikace můžete použít k aktualizaci jeho informace o zajištění, že na něm obsah uživatel musí před jeho otevřením.

## <a name="notificationsnotificationsmd"></a>[Oznámení](notifications.md)

Zjistěte, jak poskytnout vlastní oznámení v aplikace na hodinkách.

## <a name="complicationscomplicationsmd"></a>[Komplikace](complications.md)

Přidání podpory komplikací zobrazíte aktuální data na ciferníku.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Proaktivní návrhy](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 umožňuje aplikaci aktivně zobrazíte informace o uživateli v rámci zadané kontexty. Chcete-li tuto funkci podporovat [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) teď zahrnuje `MapItem` vlastnost, která umožní aplikaci poskytují informace o umístění pro pozdější použití v jiných aplikacích.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Techniky rychlé interakce](~/ios/watchos/platform/quick-interaction-techniques.md)

Poskytuje rychlé uživatelské interakce jsou nezbytné pro vytváření poutavých aplikací Apple Watch a komplikací. Nové do Watchosu 3 Apple má přidanou podporu pro rozpoznávání gest, přístup k digitální koruny a nové techniky oznámení pro uživatele a navigace. To, spolu s přidání podpory pro SceneKit a spritekit – umožňuje vývojářům snadno vytvářet bohaté a glanceable rozhraní, které jsou rychlé a responzivní.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Vylepšení aplikace workout](~/ios/watchos/platform/workout-apps.md)

Nové do Watchosu 3 cvičení související s aplikací se budou moct běžet na pozadí na Apple Watch. Pokud chcete povolit tuto funkci (a získat přístup k datům HealthKit), musí obsahovat aplikace `WKBackgroundModes` klíče v `Info.plist` soubor s hodnotou `workout-processing`.
