---
title: "Poskytnutí zpětné kompatibility s balíčkem podpora pro Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/12/2017
ms.openlocfilehash: 670ec465843bbe819b41a53fff71b01ab78b0059
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Poskytnutí zpětné kompatibility s balíčkem podpora pro Android

Užitečnost fragmenty by omezené bez zpětné kompatibility předem 3.0 (11 úroveň rozhraní API) se zařízeními s Androidem. Poskytovat této funkce, zavedená Google [knihovna podpory](http://developer.android.com/sdk/compatibility-library.html) (původně volat *knihovna pro Android kompatibility* kdy byl vydán) které backports některé z rozhraní API z novější verze Android starší verze systému Android. Je balíček Android podpory, která umožňuje zařízení se systémem Android 1.6 (rozhraní API úroveň 4) pro Android 2.3.3. (API úrovně 10).

> [!NOTE]
> Pouze `ListFragment` a `DialogFragment` jsou k dispozici prostřednictvím podpory balíček Android. Žádná z dalších fragmentovat podtřídy, například `PreferenceFragment,` jsou podporovány v systému Android podporu balíčku. V aplikacích předem Android 3.0 nebudou správně fungovat. 


## <a name="adding-the-support-package"></a>Probíhá přidávání balíčku podpory

Balíček Android podporu není automaticky přidat do aplikace pro Xamarin.Android. Poskytuje Xamarin [balíček NuGet v4 knihovna pro Android podporují](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) zjednodušit přidání podpory knihovny do aplikace pro Xamarin.Android. K přidání podpory balíčky do vašeho Xamarin.Android aplikace obsahovat [knihovna pro Android podporují v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) komponentu do projektu Xamarin.Android, jak je znázorněno na následujícím snímku obrazovky: 

[![Snímek obrazovky z podpory knihovna pro Android v4 balíčku se přidává do projektu](providing-backwards-compatibility-images/02.png)](providing-backwards-compatibility-images/02.png#lightbox)

Po provedení těchto kroků, bude možné použít fragmenty v dřívějších verzích systému Android. Rozhraní API Fragment bude fungovat stejným nyní ve tyto starší verze, s následujícími výjimkami: 

-   **Změnit minimální verze Android** &ndash; aplikace se už nepotřebuje pro Android 3.0 nebo vyšší, jak je uvedeno níže: 

    [![Snímek obrazovky z aplikaci minimální Android cíl se nastavuje v části Vlastnosti aplikace](providing-backwards-compatibility-images/03.png)](providing-backwards-compatibility-images/03.png#lightbox)

-   **Rozšíření FragmentActivity** &ndash; aktivit, které jsou hostiteli fragmenty teď musí dědit z `Android.Support.V4.App.FragmentActivity` a nikoli z `Android.App.Activity` . 

-   **Aktualizovat obory názvů** &ndash; třídy, které dědí od `Android.App.Fragment` teď musí dědit z `Android.Support.V4.App.Fragment` . Odebrat na pomocí příkazu " `using Android.App;` " v horní části ve zdrojovém kódu souboru a nahraďte ho " `using Android.Support.V4.App` ". 

-   **Použít SupportFragmentManager** &ndash; `Android.Support.V4.App.FragmentActivity` zpřístupní `SupportingFragmentManager` vlastnost, která bude použita k získání odkazu na `FragmentManager` . Příklad: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

Tyto změny v místě bude možné spustit aplikaci na základě Fragment na Android 1.6 nebo 2.x, a také na voštinový a Sandwichovy Zmrzlinová. 


## <a name="related-links"></a>Související odkazy

- [Podpora pro Android knihovny v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
