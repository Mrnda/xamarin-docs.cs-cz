---
title: Xamarin.Mac registrar
description: Tento dokument popisuje účel registrátora Xamarin.Mac a jeho použití různých konfigurací.
ms.prod: xamarin
ms.assetid: 7CAAA6B7-D654-4AD3-BAEC-9DD01210978A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 4b70ac2271b23b54e7942fdc870e0f49548e6154
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinmac-registrar"></a>Xamarin.Mac registrar

_Tento dokument popisuje účel registrátora Xamarin.Mac a jeho použití různých konfigurací._

## <a name="overview"></a>Přehled

Xamarin.Mac přemosťuje mezeru mezi spravované world (.NET) a modul runtime na kakao, povolení spravované třídy volání nespravované třídy jazyka Objective-C a volat zpět, pokud dojde k událostem. Práce nutná provedení akce tento "magic" se zpracovává souborem registrátora a obecně se v zobrazení skrytá.

Existují ovlivnit výkon této registrace, konkrétně na dobu, po spuštění aplikace a pochopení bit co se děje "pod pokličkou" může být někdy užitečné.

## <a name="configurations"></a>Konfigurace

Zásadně vašeho registrátora úlohy při spuštění lze rozdělit do dvou catagories:

- Prohledávání každé spravované třídy pro ty odvozování z NSObject a shromáždit seznam položek, které mají být exponovány do jazyka Objective-C runtime.
- Tuto informaci s modulem runtime jazyka Objective-C.

V čase již byly vytvořeny tři různé registrátora konfigurace pro použití v odlišných situacích. Každý má jinou sestavení a spusťte čas důsledky:

- **Dynamické registrátora** – během spouštění pomocí reflexe .NET můžete kontrolovat každý načíst typ, určit seznam relevantní položek a informovat nativní modul runtime. Tato možnost přidá přechodu do režimu do sestavení, ale je velmi náročné na výpočetní během spuštění (až několik sekund).
- **Statické registrátora** – během vytváření sestavení, výpočetní sadu položek, které mají být zaregistrován a generování kódu jazyka Objective-C pro zpracování registrace. Tento kód je volána při spuštění rychle zaregistrovat všechny položky. Přidá že významné pozastavení sestavení, ale může vyjmout dlouhou dobu od spuštění aplikace.
- **"Částečné" statická** – novější přístup "hybridní", což přináší většinu výhody obou. Od exporty z **Xamarin.Mac.dll** jsou konstantní uložit předpočítané knihovnu, kterou chcete zpracovávat jejich registrace a odkaz, který v. Pomocí reflexe zpracovávat knihovny uživatele, ale jako uživatel knihovny exportovat mnohem méně typů vazby platformy je často místo rychlé. Neglectable sestavení dopad čas a snižuje valná většina "náklady" dynamické.

Dnes částečné statické je výchozí nastavení pro konfiguraci ladění a statické je výchozí pro verze konfigurace.

Je několik scénářů:

- Moduly plug-in načtené po spuštění s třídy odvozené od NSObject
- Dynamicky vytvořit instance třídy odvozené od NSObject

kde se registrátora nemůže vědět, že je nutné zaregistrovat nějaký typ při spuštění. `ObjCRuntime.Runtime.RegisterAssembly` Metoda zajišťuje informovat registrátora, který obsahuje další typy vzít v úvahu.
