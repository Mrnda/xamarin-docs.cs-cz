---
title: Implementace fragmenty
description: "Android 3.0 zavedla fragmenty. Fragmenty jsou samostatné modulární komponenty, které pomáhají řešit obtíže při psaní aplikací, které můžou běžet na obrazovkách různých velikostí. Tento článek vás provede jak vyvíjet aplikace Xamarin.Android pomocí fragmentů a jak podporovat fragmenty na zařízeních s předem Androidem 3.0."
ms.topic: article
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2ed67eac51f6edcfda16caf73e4667c49124082c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="implementing-with-fragments"></a>Implementace fragmenty

_Android 3.0 zavedla fragmenty. Fragmenty jsou samostatné modulární komponenty, které pomáhají řešit obtíže při psaní aplikací, které můžou běžet na obrazovkách různých velikostí. Tento článek vás provede jak vyvíjet aplikace Xamarin.Android pomocí fragmentů a jak podporovat fragmenty na zařízeních s předem Androidem 3.0._


## <a name="overview"></a>Přehled

V této části se budeme zabývat jak vytvořit aplikaci, která se zobrazí seznam plní Shakespeare na a z každé vybrané play v uvozovkách. Naše aplikace bude využívat fragmenty tak, aby jsme definovat naše součásti uživatelského rozhraní na jednom místě, ale používat na různých faktorech. Například následující snímek obrazovky zobrazit aplikace běžící na tablet 10", a také na phone:

[![Snímky obrazovky příklad aplikaci spuštěnou na tablet a phone](images/intro-screenshot-sml.png)](images/intro-screenshot.png#lightbox)

Tato část popisuje v následujících tématech:

- **Vytváření fragmenty** &ndash; ukazuje, jak vytvořit fragment zobrazíte seznam na Shakespeare plní a jiné fragment k zobrazení v uvozovkách z každé play.

- **Podpora různých obrazovek velikosti** &ndash; ukazuje, jak rozložení aplikace využívat větší velikost obrazovky.

- **Pomocí Android balíčku pro podporu** &ndash; implementuje balíček Android podporu a potom provede menšími změnami aktivity v aplikaci, díky kterému jej spustit na starší verze systému Android.


## <a name="requirements"></a>Požadavky

Tento postup vyžaduje Xamarin.Android 4.0 nebo vyšší. Bude také nutné instalovat balíček Android podporu, jak je uvedeno v dokumentaci k fragmenty.


## <a name="introduction"></a>Úvod

V příkladu, které jsme budete sestavení v této části neobsahují aktivity logiku pro načítání seznamu, reakce na výběr uživatele nebo zobrazení nabídky pro vybrané play. Tato logika v jednotlivé fragmenty existuje.
Tím, že tato logika fragmenty sami, jsme rozdělit pracovní postup aplikaci, aby podporovala velké obrazovky s jedním aktivity nebo malé obrazovky s více aktivit bez nutnosti psaní různých logiku pro každou aktivitu. Na tablet bude každém z obou fragmentů v jedné aktivity. V telefonu fragmenty hostované ve různé aktivity.

Tato aplikace obsahuje následující části:

 **MainActivity** – zobrazí alespoň jedna z obou fragmentů, v závislosti na velikosti obrazovky. Toto je spuštění aktivity.

 **TitlesFragment** – zobrazuje seznam na Shakespeare plní, ze kterých může uživatel vybírat.

 **DetailsFragment** – zobrazí nabídku od vybrané play.

 **DetailsActivity** – hostitelem a zobrazuje DetailsFragment.
Tato aktivita používá zařízení s malé obrazovky, jako jsou telefony.



## <a name="related-links"></a>Související odkazy

- [FragmentsWalkthrough (ukázka)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Návrhář – přehled](~/android/user-interface/android-designer/index.md)
- [Ukázky Xamarin.Android: voštinový Galerie](https://developer.xamarin.com/samples/HoneycombGallery/)
- [Implementace fragmenty](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Balíček pro podporu](http://developer.android.com/sdk/compatibility-library.html)
