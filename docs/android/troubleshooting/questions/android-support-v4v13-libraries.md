---
title: Podpora chytřejší Xamarin Android v4 nebo v13 balíčků NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 47ae63fbd7062be97be04bfc1f1244ec63a1c5bd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>Podpora chytřejší Xamarin Android v4 nebo v13 balíčků NuGet

## <a name="about-the-android-support-libraries"></a>O knihovnách podpora pro Android

Podpora knihovny pro zpřístupnění nových funkcí pro starší verze systému Android se vytvořil Google. Obecně platí, jsou podpory knihovny uvedené v jejich název, který je nejnižší Android úroveň rozhraní API jsou kompatibilní s číslem verze (např: podpora v4 lze použít pouze na rozhraní API úroveň 4 a vyšší. Další informace v tomto [Stack Overflow diskusní](http://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

Dva z podpory knihovny: `Support-v4` a `Support-v13` nelze použít společně ve stejné aplikaci, to znamená, že se vzájemně vylučují. Důvodem je, že `Support-v13` ve skutečnosti obsahuje všechny typy a provádění `Support-v4`. Pokud zkuste a odkazovat oba ve stejném projektu, se setkají duplicitní typ chyby.

## <a name="problems-with-referencing"></a>Problémy s odkazování na

Vzhledem k tomu `Support-v4` stane tak oblíbenou, velké množství 3. stran knihovny nyní na ní závisí. Může rozhodli závislý na podporu v13 místo, ale je další běžné závisí na _v4_ vzhledem k tomu, že všechny aplikace, pomocí těchto 3. stran knihovny udávající možnost podpory úrovně rozhraní API úplně dolů 4.

Pokud knihovnu strany Xamarin 3rd odkazuje `Xamarin.Android.Support.v4.dll` vytvoření vazby na `Support-v4`, jakékoli aplikaci, která tuto knihovnu používá, musí také odkazovat `Xamarin.Android.Support.v4.dll`. To se změní na problém, pokud stejná aplikace chce taky použití některých funkcí z `Xamarin.Android.Support.v13.dll` vytvoření vazby na `Support-v13`. Pokud odkazujete obou vazby, se setkají duplicitní typ chyby.

## <a name="type-forwarded-v4-binding-assembly"></a>Typ předané v4 vazby sestavení

Pokud chcete tento problém vyřešit, jsme vytvořili speciální `Xamarin.Android.Support.v4.dll` sestavení, který nemá žádnou implementaci, ale jednoduše `[assembly: TypeForwardedTo (..)]` atributy, které předávání všech těchto `Support-v4` typy pro implementaci v rámci `Xamarin.Android.Support.v13.dll` sestavení.

To znamená, že vývojář může odkazovat to _směrovaných na typ_ sestavení v své aplikace, které budou splňovat odkaz na `Xamarin.Android.Support.v4.dll` ve všech 3. stran knihovny zároveň umožňuje `Xamarin.Android.Support.v13.dll` má být použit v aplikaci.

## <a name="nuget-assistance"></a>Pomoc NuGet

Zatímco vývojář může ručně přidat nezbytné správné odkazy, jsme jsou schopné používat NuGet pomoc při výběru správné sestavení (buď normální _v4_ vazba nebo typ předané _v4_ sestavení) při balíček NuGet je nainstalovaný.

Ano `Xamarin.Android.Support.v4` balíček NuGet teď obsahuje následující logice:

Pokud aplikace cílí 13 úroveň rozhraní API (Perník 3.2) nebo vyšší:

*   `Xamarin.Android.Support.v13` NuGet se automaticky přidají jako závislost
*   _Směrovaných na typ_ `Xamarin.Android.Support.v4.dll` se bude odkazovat v projektu

Pokud vaše aplikace je cílem nic nižší než 13 úroveň rozhraní API, zobrazí se normální `Xamarin.Android.Support.v4.dll` vazby, kterou se odkazuje v projektu.

## <a name="do-i-have-to-use-support-v13"></a>Je nutné použít v13 podporu?

Pokud vaše aplikace je cílení na úrovni rozhraní API 13 nebo vyšší a chcete použít `Xamarin Android Support-v4` balíček NuGet, pak se `Xamarin Android Support v13` požadovaná závislost je balíček NuGet.

Jsme pocit, že velmi malé zvýšení velikosti aplikace (dva soubory .jar lišit 17 kb) je také vhodné kompatibility a méně hlavu, který je výsledkem.

Pokud jste adamant o používání `Support-v4` v aplikaci, která se zaměřuje 13 úroveň rozhraní API nebo vyšší, můžete vždy ručně stáhnout `.nupkg`, rozbalte ho a referenční sestavení.
