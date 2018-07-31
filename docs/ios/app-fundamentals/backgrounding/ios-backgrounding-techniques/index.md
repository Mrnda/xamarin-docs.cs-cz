---
title: Techniky zpracování úloh na pozadí s Iosem
description: 'Tento dokument obsahuje odkazy na pokyny, které popisují různé backgrounding techniky v Iosu: úlohy na pozadí, služba přenosu na pozadí, načítání na pozadí a Vzdálená oznámení.'
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 33c4613e3698556b41a0d01acf2a4ac359ca5dd8
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350966"
---
# <a name="ios-backgrounding-techniques"></a>Techniky zpracování úloh na pozadí s Iosem

V následujících částech se podíváme na následující funkce iOS souběžně s existující backgrounding možnosti:

-  [Příležitostné úlohy na pozadí](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -prodlužovat výdrž baterie spuštěním úlohy na pozadí v příležitostné bloky dat, když je zařízení vzhůru pro další zpracování.
-  [Služba přenosu na pozadí](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) – spolehlivě nahrávání a stahování souborů bez ohledu na velikost stavu nebo souboru v síti.
-  [Načítání na pozadí](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) – aktualizace aplikace na pozadí v intervalech určit systém.
-  [Vzdálená oznámení](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) – používání nabízených oznámení pro aktivaci obsahu aktualizace na pozadí, než uživatel otevře aplikaci, s možností upozornit uživatele, nebo bezobslužná aktualizace.
-  **Aktualizace uživatelského rozhraní na pozadí** – Příprava aplikace uživatelského rozhraní pro uživatele a aktualizovat snímek vaší aplikace z na pozadí.
