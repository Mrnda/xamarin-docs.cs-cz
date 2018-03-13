---
title: "Sestavení pro různé platformy aplikace"
description: "Tato část popisuje v souhrn plus šest částí, jak vytvářet aplikace, které používají platformou vývoj Xamarin – z pochopení, jak funguje Xamarin navrhování mobilní aplikace a pak testování a nasazení do různých úložišť aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: 7934738a546a266036573b81e15ef9b2fa28d7b4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="sharing-code-options"></a>Možnosti sdílení kódu

Existují dvě možnosti pro sdílení kódu mezi různé platformy mobilních aplikací: sdílený prostředek projekty a knihovny přenosných tříd. Tyto možnosti jsou [tady popisovaných](~/cross-platform/app-fundamentals/code-sharing.md); Další informace o [přenosné knihovny tříd](~/cross-platform/app-fundamentals/pcl.md) a [sdílených projektů](~/cross-platform/app-fundamentals/shared-projects.md) je také k dispozici.

<a name="Sections" />

## <a name="building-cross-platform-mobile-apps"></a>Sestavení pro různé platformy mobilních aplikací

 [Přehled](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-0-overview.md)

 [Část 1 – Principy Xamarin mobilní platformy](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-1-understanding-the-xamarin-mobile-platform.md)

 [Část 2 – architektura](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-2-architecture.md)

 [Část 3 – nastavení řešení platformy mezi Xamarin](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-3-setting-up-a-xamarin-cross-platform-solution.md)

 [Součástí 4 – plánování práce s více platforem](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-4-platform-divergence-abstraction-divergent-implementation.md)

 [Část 5 – sdílení strategie praktické kódu](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-5-practical-code-sharing-strategies.md)

 [Část 6 – Testování a schvalování v obchodě App Store](~/cross-platform/app-fundamentals/building-cross-platform-applications/part-6-testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />


## <a name="case-studies"></a>Případové studie

Principy uvedených v tomto dokumentu jsou uvedené do praxe v ukázkové aplikaci *Tasky*, a také [předem integrované aplikace](https://xamarin.com/prebuilt) jako [Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm).

 <a name="Tasky" />


### <a name="tasky"></a>Tasky

Tasky je aplikace seznamu úkolů jednoduché pro iOS, Android a Windows Phone.
Ho ukazuje základní informace o vytvoření aplikace pro různé platformy s Xamarinem a používá místní databáze SQLite.

 [![tasky seznamu](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![tasky seznamu](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

Pro čtení [Tasky Případová studie](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md).


## <a name="summary"></a>Souhrn

Tato část nabízí nástroje pro vývoj aplikací pro Xamarin a popisuje, jak vytvářet aplikace, které více mobilní platformy.

Popisuje Vrstvená architektura tento kód struktury pro nové použití napříč různými platformami a popisuje vzory jiný software, které lze použít v rámci této architektuře.

Příklady jsou uvedeny běžné funkce aplikace (např. soubor a síťových operací) a jak může být sestaven tak napříč platformami.

Nakonec stručně popisuje testování a poskytuje odkazy na příkladovou studii, která vloží těchto zásad do akce.



## <a name="related-links"></a>Související odkazy

- [Možnosti sdílení kódu](~/cross-platform/app-fundamentals/code-sharing.md)
- [Případová studie: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky ukázkovou aplikaci (githubu)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Vývoj aplikací Xamarin Mobile: Napříč platformami C# a základy Xamarin.Forms](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/))
- [Mobilní vývoj pomocí jazyka C# pomocí Gregu články (O'Reilly)](http://shop.oreilly.com/product/0636920024002.do)
- [Profesionální mobilní vývoj pro různé platformy v jazyce C# Scott Olson, Jan Hunter, Ben Horgen, Kenny Goers (Wrox)](http://www.wiley.com/WileyCDA/WileyTitle/productCd-1118157702.html)
