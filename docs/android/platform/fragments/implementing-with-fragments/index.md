---
title: Implementace fragmenty - návod
description: Tento článek vás provede použití fragmenty k vývoji aplikací Xamarin.Android.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 92c68298d7abd2570efd89e12d7cfb6364e90972
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798353"
---
# <a name="implementing-fragments---walkthrough"></a>Implementace fragmenty - návod

_Fragmenty jsou samostatný, modulární součásti, které vám mohou pomoci vyřešit složitost aplikací pro Android, které cílí na zařízení s různými velikost obrazovky. Tento článek vás provede postup vytvoření a používání fragmenty při vývoji aplikace Xamarin.Android._

## <a name="overview"></a>Přehled

V této části budete provede procesem vytvoření a používání fragmentů v aplikaci Xamarin.Android. Tato aplikace se zobrazí názvy několik plní ve William Shakespeare v seznamu. Když uživatel klepnutím na název play, zobrazí aplikace v samostatnou aktivitě v uvozovkách z této play:

[![Aplikaci spuštěnou s telefonem s Androidem v režimu na výšku](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

Když telefonu otočen na šířku režimu, se změní vzhled aplikace: Zobrazí se ve stejné aktivitě seznamu plní a uvozovky. Pokud je vybraná play, bude nabídka zobrazení do stejné aktivity:

[![Aplikaci spuštěnou s telefonem s Androidem v režimu na šířku](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

Nakonec pokud aplikace běží na tablet:

[![Aplikaci spuštěnou na Android tablet](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

Tato ukázková aplikace můžete snadno upravit pro různá zařízení a orientace s minimálními změnami kódu s použitím fragmenty a [alternativní rozložení](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources).

Data pro aplikace, budou existovat v dvou polích řetězce, které jsou pevně kódovaný v aplikaci jako pole řetězců C#. Každé pole bude sloužit jako zdroj dat pro jeden fragment.  Jedno pole bude obsahovat název některé plní ve Shakespeare a další pole bude obsahovat v uvozovkách z této play. Po spuštění aplikace zobrazí názvy play v `ListFragment`. Když uživatel klikne na play v `ListFragment`, bude další aktivitu, která se zobrazí nabídku spuštění aplikace.

Uživatelské rozhraní pro aplikaci bude obsahovat dvě rozložení, jednu pro na výšku a jeden pro režim na šířku. V době běhu Android určí, jaké rozložení byste měli zatížení na základě orientace zařízení a bude poskytovat tohoto rozložení aktivity k vykreslení. Veškerou logiku pro neodpovídá na požadavky uživatel klikne na a zobrazování dat bude obsažená v fragmenty. Aktivity v aplikaci existují pouze jako kontejnery, které budou hostiteli fragmenty.

Tento názorný postup bude rozdělit do dvou průvodců. [První část](./walkthrough.md) se soustředí na základní částí aplikace. Jednu sadu rozložení (optimalizované pro režim na výšku) vytvoří, spolu s dva fragmenty a dvě aktivity:

1. `MainActivity` &nbsp; Toto je spuštění aktivity pro aplikaci.
1. `TitlesFragment` &nbsp; Tento fragment zobrazí seznam názvů plní, které byly napsané pomocí William Shakespeare. Bude hostitelem `MainActivity`.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` Spustí `PlayQuoteActivity` v reakci na uživatele vyberte play v `TitlesFragment`.
1. `PlayQuoteFragment` &nbsp; Tento fragment zobrazí v uvozovkách z play pomocí William Shakespeare. Bude hostitelem `PlayQuoteActivity`.

[Druhé části tohoto návodu](./walkthrough-landscape.md) zabývat přidání alternativní rozložení (optimalizované pro šířku), který se zobrazí každém z obou fragmentů na obrazovce. Navíc některé dílčí kód bude být provedeny změny kód tak, aby aplikace bude přizpůsobit své chování pro počet fragmentů, které jsou současně zobrazené na obrazovce.

## <a name="related-links"></a>Související odkazy

- [FragmentsWalkthrough (ukázka)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Návrhář – přehled](~/android/user-interface/android-designer/index.md)
- [Implementace fragmenty](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Balíček pro podporu](http://developer.android.com/sdk/compatibility-library.html)
