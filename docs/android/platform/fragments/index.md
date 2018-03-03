---
title: Fragmenty
description: "Android 3.0 zavedla fragmenty, znázorňující způsob pro podporu flexibilnější návrhy pro mnoho různých obrazovek velikosti nalezen z telefonů a tabletů. Tento článek se zabývá jak vyvíjet aplikace Xamarin.Android pomocí fragmentů a také jak podporovat fragmenty na zařízeních s předem Androidem 3.0 (11 úroveň rozhraní API)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 0486b9e4371a1bcab02921da42bcb929f00a782f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="fragments"></a>Fragmenty

_Android 3.0 zavedla fragmenty, znázorňující způsob pro podporu flexibilnější návrhy pro mnoho různých obrazovek velikosti nalezen z telefonů a tabletů. Tento článek se zabývá jak vyvíjet aplikace Xamarin.Android pomocí fragmentů a také jak podporovat fragmenty na zařízeních s předem Androidem 3.0 (11 úroveň rozhraní API)._

## <a name="fragments-overview"></a>Přehled fragmenty

Větší velikost obrazovky na většině tablety nalezen přidat další vrstvu složitost na vývoj pro Android – rozložení určená pro malou obrazovku nefunguje nutně také pro větší obrazovky a naopak. Abyste snížili počet komplikací, ke které tato zavedené, Android 3.0 přidali dvě nové funkce, *fragmenty* a *podporu balíčky*.

Fragmenty lze považovat za moduly uživatelského rozhraní. Umožňují vývojáře zdola nahoru uživatelského rozhraní na izolované, opakovaně použitelný části, které lze spustit v samostatné aktivity. V době běhu aktivity sami rozhodne které fragmenty používat.

Podpora balíčky byly původně názvem *kompatibility knihovny* a povoleno fragmenty k použití v zařízení se systémem verze Android před Android 3.0 (11 úroveň rozhraní API).

Například následující obrázek znázorňuje, jak jednu aplikaci používá fragmenty napříč různými velikostem zařízení.

[![Diagram použití fragmenty v tabletů a telefonů](images/00.png)](images/00.png)

*Fragment A* obsahuje seznam, zatímco *Fragment B* obsahuje podrobnosti o položce zvolené v tomto seznamu. Když aplikace běží na tablet, zobrazí každém z obou fragmentů na stejné aktivity. Pokud stejná aplikace běží v telefonu (s menší obrazovky velikost), fragmenty hostovaným ve dvou samostatných aktivit. Fragment A a Fragment B jsou stejné u obou velikostem, ale liší se aktivity, které budou jejich hostiteli.

Chcete-li aktivita koordinaci a spravovat všechny tyto fragmenty, Android zavedly novou třídu s názvem *FragmentManager*. Každá aktivita má svou vlastní instanci `FragmentManager` pro přidání, odstranění a hledání hostované fragmenty. Následující diagram znázorňuje vztah mezi fragmenty a aktivity:

[![Diagram ilustrující vztahy mezi aktivity, Fragment správce a fragmenty](images/01.png)](images/01.png)

V některých namapoval fragmenty můžete představit jako složené ovládací prvky nebo jako malé aktivity. Jejich sady až částí uživatelského rozhraní do opakovatelně použitelných moduly, které lze nezávisle vývojáři v aktivitách. Fragment mít hierarchii zobrazení – stejně jako aktivita – ale na rozdíl od aktivity, ho můžete sdílet mezi obrazovky. Zobrazení se liší od fragmentů, že fragmenty mají své vlastní životního cyklu; zobrazení nepodporují.

Sice aktivitu hostitele na jeden nebo více fragmentů, není přímo vědět fragmenty sami. Podobně fragmenty nemají přímo informace z dalších fragmentů v hostitelských aktivity. Fragmenty a aktivity jsou však vědět `FragmentManager` jejich aktivity. Pomocí `FragmentManager`, je možné pro aktivitu nebo Fragment získat odkaz na konkrétní instanci Fragment, a pak volat metody pro tuto instanci. Tímto způsobem můžete aktivity nebo fragmenty komunikovat a pracovat s dalších fragmentů.

Tato příručka obsahuje komplexní přehled o tom, jak používat fragmenty, včetně:

-   **Vytváření fragmenty** – jak vytvořit základní Fragment a klíče metody, které musí být implementována.
-   **Fragmentovat správy a transakce** – postupy k manipulaci s fragmenty za běhu.
-   **Balíček Android podporu** – jak používat knihovny, které umožňují fragmenty má být použit ke starším verzím systému Android.


## <a name="requirements"></a>Požadavky

Fragmenty jsou k dispozici v systému Android SDK počínaje úroveň rozhraní API 11 (Android 3.0), jak je znázorněno na následujícím snímku obrazovky:

[![Vyberte úroveň rozhraní API v Android SDK Manager](images/02.png)](images/02.png)

Fragmenty jsou k dispozici v Xamarin.Android 4.0 a vyšší. Aplikace pro Xamarin.Android musí být alespoň úroveň rozhraní API 11 (Android 3.0) nebo vyšší, aby bylo možné používat fragmenty. Cílovém Frameworku, který může být nastavena v možnostech projektu, jak je uvedeno níže:

[![Nastavení úrovně rozhraní Target Framework API v možnosti projektu](images/03.png)](images/03.png)

Je možné použít fragmenty ve starších verzích systému Android pomocí balíček Android podporu a Xamarin.Android 4.2 nebo novější. Jak na to je podrobněji popsány v dokumentech v této části.


## <a name="related-links"></a>Související odkazy

- [Galerie voštinový (ukázka)](https://developer.xamarin.com/samples/monodroid/HoneycombGallery)
- [Fragmenty](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Balíček pro podporu](http://developer.android.com/sdk/compatibility-library.html)
- [MOTODEV Webinář: Představení fragmenty](http://motodev.adobeconnect.com/p9h1aqk3ttn/)
