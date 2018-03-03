---
title: Funkce platformy
description: "Apple Watch funkce specifické pro zahrnutí do watchOS aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 16e779761aa164ea9890547e3baca527a3592021
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="platform-features"></a>Funkce platformy

_Apple Watch funkce specifické pro zahrnutí do watchOS aplikace._

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Vylepšení platím Apple](~/ios/watchos/platform/apple-pay.md)

V watchOS 3 rozhraní PassKit rozšířila povolit podporu pro zabezpečené, v aplikaci platby (z fyzického zboží a služeb) pro aplikace běžící na Apple Watch.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Úlohy na pozadí](~/ios/watchos/platform/background-tasks.md)

watchOS 3 zavádí několik úlohy na pozadí, které aplikace můžete použít k aktualizovat informace o zajištění, že obsahuje obsah, uživatel musí před jeho otevřením.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Úvod do watchOSu 4](introduction-to-watchos4.md)

Nové funkce v watchOS 4.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Úvod do watchOSu 3](introduction-to-watchos3/index.md)

Tento článek představuje všechny nové a změněné rozhraní API dostupná v watchOS 3 pro vývojáře pro Xamarin.

##  <a name="notificationsnotificationsmd"></a>[Oznámení](notifications.md)

Zjistěte, jak poskytnout vlastní oznámení zpracování v aplikaci sledování.

##  <a name="complicationscomplicationsmd"></a>[Komplikace](complications.md)

Přidání podpory komplikace zobrazíte aktuální data na tučné sledovat.


## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Proaktivní návrhy](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 umožňuje aplikaci na proaktivně informace pro uživatele v rámci zadané kontexty. Chcete-li tuto funkci podporovat [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) nyní zahrnuje `MapItem` vlastnost, která umožní aplikaci poskytují informace o umístění pro pozdější použití jiné aplikace.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Techniky rychlé interakce](~/ios/watchos/platform/quick-interaction-techniques.md)

Poskytnete rychlé uživatelské interakce jsou nezbytné pro vytváření byly atraktivní aplikace Apple Watch a komplikace. Nové za účelem watchOS 3, Apple přidala podporu pro rozpoznávání gesto, přístup k digitální Crown a nové techniky oznámení pro uživatele a navigace. To, společně s přidanou podporu pro SceneKit a SpriteKit, umožňuje vývojářům snadno vytvářet bohaté, přehledné rozhraní, která se rychle a dobře reagovaly.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Vylepšení aplikace Workout](~/ios/watchos/platform/workout-apps.md)

Nové k watchOS 3 cvičení související s aplikací mít možnost spustit na pozadí na Apple Watch. Povolit tuto funkci (a přístup k datům HealthKit), musí aplikace obsahovat `WKBackgroundModes` klíče v `Info.plist` soubor s hodnotou `workout-processing`.
