---
title: Funkce watchOS
description: Tento dokument obsahuje odkazy na různé příručky, které popisují watchOS platformy funkce jako je dotykový identifikátor, oznámení, komplikace, proaktivní návrhy, cvičení aplikace a další.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 16d10dd69223f404aac7c933302992a1544461e9
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066611"
---
# <a name="watchos-platform-features"></a>Funkce watchOS

Tento dokument obsahuje odkazy na různé příručky, které popisují watchOS platformy funkce jako je dotykový identifikátor, oznámení, komplikace, proaktivní návrhy, cvičení aplikace a další.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Úvod do watchOSu 4](introduction-to-watchos4.md)

Tento dokument obsahuje přehled funkce a aktualizace v watchOS 4.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Úvod do watchOSu 3](introduction-to-watchos3/index.md)

Tento článek popisuje nové a aktualizované rozhraní API v watchOS 3.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Dotykový identifikátor vylepšení](~/ios/watchos/platform/apple-pay.md)

V watchOS 3 rozhraní PassKit rozšířila povolit podporu pro zabezpečené, v aplikaci platby (z fyzického zboží a služeb) pro aplikace běžící na Apple Watch.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Úlohy na pozadí](~/ios/watchos/platform/background-tasks.md)

watchOS 3 zavádí několik úlohy na pozadí, které aplikace můžete použít k aktualizovat informace o zajištění, že obsahuje obsah, uživatel musí před jeho otevřením.

## <a name="notificationsnotificationsmd"></a>[Oznámení](notifications.md)

Zjistěte, jak poskytnout vlastní oznámení zpracování v aplikaci sledování.

## <a name="complicationscomplicationsmd"></a>[Komplikace](complications.md)

Přidání podpory komplikace zobrazíte aktuální data na tučné sledovat.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Proaktivní návrhy](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 umožňuje aplikaci na proaktivně informace pro uživatele v rámci zadané kontexty. Chcete-li tuto funkci podporovat [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) nyní zahrnuje `MapItem` vlastnost, která umožní aplikaci poskytují informace o umístění pro pozdější použití jiné aplikace.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Techniky rychlé interakce](~/ios/watchos/platform/quick-interaction-techniques.md)

Poskytnete rychlé uživatelské interakce jsou nezbytné pro vytváření byly atraktivní aplikace Apple Watch a komplikace. Nové za účelem watchOS 3, Apple přidala podporu pro rozpoznávání gesto, přístup k digitální Crown a nové techniky oznámení pro uživatele a navigace. To, společně s přidanou podporu pro SceneKit a SpriteKit, umožňuje vývojářům snadno vytvářet bohaté, přehledné rozhraní, která se rychle a dobře reagovaly.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Vylepšení cvičení aplikace](~/ios/watchos/platform/workout-apps.md)

Nové k watchOS 3 cvičení související s aplikací mít možnost spustit na pozadí na Apple Watch. Povolit tuto funkci (a přístup k datům HealthKit), musí aplikace obsahovat `WKBackgroundModes` klíče v `Info.plist` soubor s hodnotou `workout-processing`.
