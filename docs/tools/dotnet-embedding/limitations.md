---
title: "Vložení omezení rozhraní .NET"
ms.topic: article
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 110eb4169103f99c1538a1846fd231069863530c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="net-embedding-limitations"></a>Vložení omezení rozhraní .NET


Tento dokument popisuje omezení .NET vnoření (Embeddinator-4000) a kdykoli je to možné, poskytuje řešení pro ně.

## <a name="general"></a>Obecné

### <a name="use-more-than-one-embedded-library-in-a-project"></a>Použití více než jeden embedded knihovny v projektu

Není možné, že dvě mono runtimes společně existující uvnitř stejné aplikaci. To znamená, že nemůžete použít dvě různé knihovnami embeddinator generované 4000 uvnitř stejné aplikaci.

**Alternativní řešení:** generátor můžete použít k vytvoření jedné knihovny, která zahrnuje několik sestavení (z různých projektů).

### <a name="subclassing"></a>Vytvoření podtřídy

Embeddinator usnadňuje integraci mono runtime uvnitř aplikací díky zpřístupnění sada připravené k použití rozhraní API pro cílový jazyk a platformu.

Ale není to dva způsob integrace, například nemůžete podtřídami spravovaného typu a očekávat spravovaného kódu pro zpětné volání do nativního kódu, protože spravovaný kód je vědoma této společné existance.

Podle potřeby bude pravděpodobně možné na alternativní řešení součástí toto omezení, například

* spravovaný kód můžete p/invoke do nativního kódu. To vyžaduje přizpůsobení spravovaného kódu umožňují vlastní z nativního kódu;

* použití produktů, třeba Xamarin.iOS a vystavit spravované knihovny, která by povolit ObjC (v tomto případě) podtřídami některé spravované NSObject podtřídy.


## <a name="objc-generated-code"></a>ObjC vygeneruje kód

### <a name="nullability"></a>Možnost použití hodnoty Null

V rozhraní .NET neexistuje žádná metadata, která Řekněte nám, pokud je přijatelné odkaz s hodnotou null nebo není pro rozhraní API. Většina rozhraní API vyvolá výjimku `ArgumentNullException` Pokud nelze zvládl `null` argument. To je problematické, protože ObjC zpracování výjimek je něco lépe vyhnout.

Vzhledem k tomu, že nelze generovat poznámky přesné možnost použití hodnoty Null v soubory hlaviček a chcete minimalizovat spravované výjimky jsme výchozí argumenty nesmí být nulová (`NS_ASSUME_NONNULL_BEGIN`) a přidejte některé konkrétní, když přesnost je možné, poznámky možnost použití hodnoty Null.
