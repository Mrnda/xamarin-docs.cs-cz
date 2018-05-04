---
title: Apple platforma (iOS a Mac)
description: V této části nabídneme strategie sdílet kód mezi Xamarin.iOS a Xamarin.Mac projekty.
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: a842e89b74419a03e4ea8f4355162960d9109f9b
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="apple-platform-ios-and-mac"></a>Apple platforma (iOS a Mac)

_V této části nabídneme strategie sdílet kód mezi Xamarin.iOS a Xamarin.Mac projekty._

## <a name="code-sharing"></a>Sdílení kódu

Pro elementy kódu, které mají žádné elementy uživatelského rozhraní nejlepší způsob, jak sdílet kód mezi iOS a Mac je stále použití [přenosné knihovny tříd](~/cross-platform/app-fundamentals/pcl.md).

Pro kód, který má nějakou práci rozhraní uživatele a ještě, kterou chcete sdílet, měli byste použít [sdílených projektů](~/cross-platform/app-fundamentals/shared-projects.md) které umožňují umístěte do sdílené složky v jednom projektu a mějte ho kompilovat s Mac a iOS, když kód.

##  <a name="unified-apiunifiedindexmd"></a>[Unified API](unified/index.md)

Jednotné rozhraní API pro iOS a Mac projekty používá stejné obory názvů pro rozhraní tak, aby stejné souboru kódu lze použít v obou platformách pro bezproblémové sdílení kódu. Umožňuje také 32 až 64 bit sestavení. Unifikované API byla výchozí šablona od časná 2015 a doporučuje se pro všechny nové projekty - *pouze* projekty jednotné rozhraní API lze odeslat do obchodu s aplikacemi.

### <a name="classic-apis"></a>Klasické rozhraní API

> [!NOTE]
> **Classic profil vyřazení:** při přidání nové platformy v Xamarin.iOS jsme spouštějí postupně přestat používat funkce z klasického profilu (monotouch.dll). Například možnost bez NRC (-ref počet nových) byla odebrána. NRC vždy byla povolená pro všechny jednotná aplikace (tj. bez NRC se nikdy možnost) a nemá žádné známé problémy. Možnost použití Boehm jako garbage collector v budoucích verzích dojde k odebrání. Je také možnost Nikdy dostupné pro jednotné aplikace. Úplné odebrání classic podpory naplánován patří 2016 s vydáním Xamarin.iOS 10.0.

Původní Xamarin.iOS (bez Unified) a rozhraní API Xamarin.Mac provedené sdílení kódu obtížnější protože nativní rozhraní měli buď `MonoTouch.` nebo `MonoMac.` předpony oboru názvů.  Jsme poskytli některé prázdný obory názvů, která umožňuje vývojářům sdílet kód přidejte `using` příkazy, které odkazují na obory názvů MonoMac i MonoTouch ve stejném souboru, ale to bylo ještě ugly. Klasické rozhraní API by měly být pouze nadále použít ve starší verzi aplikace, které jsou distribuovány interně (upgrade na unifikované API je doporučeno).


### <a name="updating-from-classic-to-the-unified-api"></a>Aktualizace z klasického jednotné rozhraní API

Existují podrobné pokyny pro aktualizaci aplikace z klasického unifikované API.

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Knihovny pro vytváření vazeb Objective-C](binding/index.md)

Xamarin umožňuje uvést nativní knihovny do svých aplikací s vazbami. Tato část vysvětluje:

- fungování vazby
- Jak ručně vytvořit vazbu projekt, který umožňuje přizpůsobit kód jazyka Objective-C Xamarin, a
- jak používat naše **Sharpie cíl** nástroj automatizovat proces.

## <a name="native-referencesnative-referencesmd"></a>[Nativní odkazy](native-references.md)



##  <a name="macios-native-typesnativetypesmd"></a>[Mac/iOS nativních typech](nativetypes.md)

Pro podporu 32 a 64bitová verze kódu transparentně z jazyka C# a F #, Představujeme nové datové typy.   Informace o nich zde.

##  <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[Vytváření aplikací 32 až 64 bitů](32-and-64/index.md)

Co potřebujete vědět o podporu 32 až 64 bitové aplikace.

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[Práce s nativní typy v multiplatformních aplikacích](native-types-cross-platform.md)

Tento článek popisuje použití nové iOS jednotné rozhraní API nativních typech (`nint`, `nuint`, `nfloat`) v aplikaci a platformy, kde je kód sdílet s jiným systémem než iOS zařízení, třeba na Android nebo operační systémy Windows Phone.
Poskytuje přehled o při by měl použít nativní typy a poskytuje několik řešení případech, kdy nový typ musí být použita s kódem napříč platformami.


## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[Selektor implementace zásobníku HttpClient a protokolu SSL/TLS](http-stack.md)

Nový výběr zobrazení HttpClient řídí, které implementace HttpClient používat v aplikaci Xamarin.iOS, Xamarin.tvOS a Xamarin.Mac. Můžete teď přepnout implementace, která používá pro iOS, na tvOS nebo nativní přenosy značky OS x (`NSUrlSession` nebo `CFNetwork` v závislosti na operačním systémem).

Protokol SSL (Secure Socket Layer) a jeho následníka TLS (Transport Layer Security) poskytuje podporu pro protokol HTTP a jiných síťových připojení prostřednictvím `System.Net.Security.SslStream`. Nová možnost sestavení implementace SSL/TLS Přepne mezi Mono na vlastní TLS zásobníku a jedno používá technologii společnosti Apple TLS zásobníku v Mac a iOS.
