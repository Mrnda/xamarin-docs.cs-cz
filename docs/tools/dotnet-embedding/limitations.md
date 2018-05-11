---
title: Vložení omezení rozhraní .NET
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 8ea3e7c6ff2fc28700c46109a814fc5da6958500
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="net-embedding-limitations"></a>Vložení omezení rozhraní .NET

Tento dokument popisuje omezení vnoření .NET a kdykoli je to možné, poskytuje řešení pro ně.

## <a name="general"></a>Obecné

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Použití více než jeden embedded knihovny v projektu

Není možné, že dvě Mono runtimes společně existující uvnitř stejné aplikaci. To znamená, že nemůžete použít dvě různé knihovny .NET vložení generované uvnitř stejné aplikaci.

**Alternativní řešení:** generátor můžete použít k vytvoření jedné knihovny, která zahrnuje několik sestavení (z různých projektů).

### <a name="subclassing"></a>Vytvoření podtřídy

Díky zpřístupnění sada připravené k použití rozhraní API pro cílový jazyk a platformu .NET vložení usnadňuje integrace Mono runtime uvnitř aplikace.

Ale není to obousměrný integraci, například nemůžete podtřídami spravovaného typu a očekávat spravovaného kódu pro zpětné volání do nativního kódu, protože spravovaný kód je vědoma této společné existance.

Podle potřeby bude pravděpodobně možné na alternativní řešení součástí toto omezení, například

* spravovaný kód můžete p/invoke do nativního kódu. To vyžaduje přizpůsobení spravovaného kódu umožňují vlastní z nativního kódu;

* použití produktů, třeba Xamarin.iOS a vystavit spravované knihovny, který by umožnil že jazyka Objective-C (v tomto případě) k podtřídami některé spravované nástrojem NSObject podtřídy.

## <a name="objective-c-generated-code"></a>Generovaný kód jazyka Objective-C

### <a name="nullability"></a>Možnost použití hodnoty Null

Neexistuje žádná metadata v rozhraní .NET, která Řekněte nám, pokud je přijatelné odkaz s hodnotou null nebo není pro rozhraní API. Většina rozhraní API vyvolá výjimku `ArgumentNullException` Pokud nelze zvládl `null` argument. To je problematické, protože jazyka Objective-C zpracování výjimek je něco lépe vyhnout.

Vzhledem k tomu, že nelze generovat poznámky přesné možnost použití hodnoty Null v soubory hlaviček a chcete minimalizovat spravované výjimky jsme výchozí argumenty nesmí být nulová (`NS_ASSUME_NONNULL_BEGIN`) a přidejte některé konkrétní, když přesnost je možné, poznámky možnost použití hodnoty Null.

### <a name="bitcode-ios"></a>Bitcode (iOS)

Aktuálně vložení .NET nepodporuje bitcode na iOS, která je povolena pro některé šablony projektu Xcode. To bude muset být úmyslně zakázáno úspěšně odkaz generované architektury.

![Možnost Bitcode](images/ios-bitcode-option.png)
