---
title: Velikost balíčku aplikace
description: Tento článek prozkoumá základní součástí balíčku aplikace Xamarin.Android a přidružené strategií, které lze použít pro nasazení balíčku efektivní během ladění a vydání fázích vývoje.
ms.prod: xamarin
ms.assetid: 8D70CDDD-3D3C-9949-8045-AB8F93D18E74
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: babe45c39f6a69dd9384f3bce8fe28ada31ebfdf
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="application-package-size"></a>Velikost balíčku aplikace

_Tento článek prozkoumá základní součástí balíčku aplikace Xamarin.Android a přidružené strategií, které lze použít pro nasazení balíčku efektivní během ladění a vydání fázích vývoje._


## <a name="overview"></a>Přehled

Xamarin.Android používá celou řadu mechanismy na minimální velikost balíčku je zachován, že že efektivní debug a release procesem nasazení. V tomto článku jsme podívejte se na verzi Xamarin.Android a ladění pracovní postup nasazení a jak platformě Xamarin.Android zajišťuje, že jsme sestavení a verze balíčky malé aplikací.


## <a name="release-packages"></a>Balíčky verzí

Pro odeslání plně obsažený aplikace, musí zahrnovat balíček aplikace, přidružené knihovny, obsah, Mono runtime a požadovaná sestavení základní třídy knihovny (BCL). Například pokud jsme využít výchozí šablony "Hello World", obsah balíčku dokončení sestavení by vypadat takto:

[![Velikost balíčku před linkeru](app-package-size-images/hello-world-package-size-before-linker.png)](app-package-size-images/hello-world-package-size-before-linker.png#lightbox)

Je větší velikost stažení než rádi bychom znali 15,8 MB. Problém je v knihovnách BCL, protože obsahují mscorlib, systém a Mono.Android, které poskytují spoustu komponenty potřebné ke spuštění vaší aplikace. Však také obsahují funkce, které je možné, že používáte ve vaší aplikaci, tak může být vhodnější než vyloučit těchto součástí.

Když jsme sestavení aplikace pro distribuci, jsme spuštění procesu, se říká propojení, která hledá aplikace a odebere kód, který se nepoužívá přímo. Tento proces je podobný s funkcemi, [uvolňování paměti](~/android/internals/garbage-collection.md) poskytuje pro přidělené halda paměti. Ale namísto operační přes objekty, propojení funguje přes kódu. Například celý obor názvů je v System.dll pro odesílání a příjmu e-mailu, ale pokud vaše aplikace neprovede použití této funkce kód je právě plýtvání místa. Po spuštění linkeru na aplikace Hello World, naše balíček nyní vypadat třeba takto:

[![Velikost balíčku po linkeru](app-package-size-images/hello-world-package-size-after-linker.png)](app-package-size-images/hello-world-package-size-after-linker.png#lightbox)

Jak jsme vidět, tím se odeberou významné množství BCL, který nebyl používán. Všimněte si, že konečné velikosti BCL je závislá na jaké aplikace ve skutečnosti používá. Například pokud jsme si prohlédněte mnohem vyšší ukázkové aplikaci s názvem ApiDemo, vidíme, že komponentu BCL zvýšilo velikost, protože ApiDemo používá více BCL než Hello, World nemá:

![Velikost balíčku ApiDemo po propojení](app-package-size-images/api-demo-package-size-after-linker.png)

Jak je znázorněno zde bude vaše velikost balíčku aplikace obecně o 2.9 MB větší než vaše aplikace a jeho závislé součásti.


## <a name="debug-packages"></a>Ladění balíčky

Co jsou zpracovávané trochu jinak pro ladění. Když opětovného nasazení opakovaně do zařízení, aplikace musí být tak rychlý jako možné, takže jsme optimalizovat ladění balíčky pro nasazení, nikoli velikost rychlost.

Android je relativně pomalé kopírování a instalace balíčku, takže chceme, aby velikost balíčku, být co nejmenší. Jak jsme už jsme probírali výše, jeden možný způsob, jak minimální velikost balíčku je přes linkeru. Ale propojení je pomalé a obvykle chcete nasadit pouze ty části aplikace, které se změnily od posledního nasazení. K tomu, oddělte jsme naše aplikace ze základních komponent Xamarin.Android.

Při prvním jsme ladění na zařízení, jsme zkopírujte dvě velkých balíčků názvem *sdílený modul Runtime* a *sdílené platformě*. Sdílený modul Runtime obsahuje Mono Runtime a BCL, při sdílené platformě obsahuje rozhraní API systému Android úrovně konkrétní sestavení:

[![Sdílený modul runtime velikost balíčku](app-package-size-images/shared-runtime-package-size.png)](app-package-size-images/shared-runtime-package-size.png#lightbox)

Kopírování tyto základní součásti se provádí jenom jednou jako trvá poměrně kousek čas, ale umožňuje všechny následné aplikacím spuštění v režimu ladění je využívají. Nakonec jsme zkopírujte skutečné aplikace, která je malé a rychlé:

![Skutečné aplikace je malá](app-package-size-images/hello-world-debug-application-no-link.png)

### <a name="fast-assembly-deployment"></a>Nasazení rychlé sestavení

*Rychlého nasazení sestavení* sestavení možnost slouží k další zmenšení velikosti ladění instalace balíčku není zahrnutím sestavení v balíčku aplikace, instalace sestavení přímo v zařízení jenom jednou a kopírování pouze soubory, které byly upraveny od posledního nasazení.

Chcete-li povolit *rychlého nasazení sestavení*, postupujte takto:

1.  Klikněte pravým tlačítkem na projekt pro Android v Průzkumníku řešení a vyberte **možnosti**.

2.  Dialogové okno Možnosti projektu vyberte **Android sestavení** :  

    ![Android sestavení možnosti projektu](app-package-size-images/fastdev0.png)

3.  Zkontrolujte **využít sdílené Mono runtime políčko** a **rychlé nasazení sestavení** zaškrtávací políčka:  

    ![Zaškrtávací políčka vybrané kartě balení](app-package-size-images/fastdev.png)

4.  Klikněte **OK** tlačítko Uložit změny a zavřete dialogové okno Možnosti projektu.


Další čas, při vytváření aplikace pro ladění, bude nainstalována sestavení přímo na zařízení (Pokud již nebyly) a menší balíček aplikace, (který nezahrnuje sestavení) bude nainstalována na zařízení. To se zkrátil čas potřebný k zprovoznění změny aplikace pro testování.

Trvalé na dlouhém nejprve nasazení sdílený modul runtime a sdílené platformě, pokaždé, když jsme provést změnu aplikace, můžeme nasadit novou verzi rychle a painlessly, takže máme cyklus rychlého změn a nasadit nebo spuštění.


## <a name="summary"></a>Souhrn

V tomto článku jsme se zaměřili omezující vlastnosti pro Xamarin.Android verze a balení profil ladění. Kromě toho jsme se podívali na strategie, která Mono pro platformu Android používá k zajištění efektivní balíčku nasazení během ladění a vydání fázích vývoje.
