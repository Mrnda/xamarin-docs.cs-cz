---
title: tvOS přehled distribuce aplikace
description: Tento dokument poskytuje přehled distribuční technik, které jsou k dispozici pro aplikaci Xamarin.tvOS a slouží jako ukazatel na podrobnější dokumenty v tomto tématu.
ms.prod: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 440d980c39434165ba9a59ccf67850977c8f75c3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788742"
---
# <a name="tvos-app-distribution-overview"></a>tvOS přehled distribuce aplikace

_Tento dokument poskytuje přehled distribuční technik, které jsou k dispozici pro aplikaci Xamarin.tvOS a slouží jako ukazatel na podrobnější dokumenty v tomto tématu._


Po byla vyvinuta Xamarin.tvOS aplikace, je dalším krokem v životního cyklu softwaru distribuovat aplikace pro uživatele, jak je uvedeno v části zvýrazněných v následujícím diagramu:


[![Přehled životního cyklu vývoje softwaru](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)


Apple nabízí tyto způsoby distribuovat tvOS aplikace, které jsou podporovány Xamarin.tvOS:

1. [**Obchod s aplikacemi**](#Apple-TV-App-Store-Distribution)
2. [**Interní (Enterprise)**](#In-House-Distribution) 
2. [**Ad Hoc**](#Ad_Hoc_Distribution) 

Tyto scénáře vyžadují, aby aplikace zřídit pomocí odpovídající *profil pro zřizování*. Profily zřizování jsou soubory, které obsahují informace, jakož i identity aplikace a mechanismus určený distribuce pro podpis kódu. Pro distribuci App Store také obsahují informace o zařízeních, jaké aplikace se dá nasadit na.

<a name="Apple-TV-App-Store-Distribution" />

## <a name="apple-tv-app-store-distribution"></a>Distribuce Apple TV App Store

To je hlavním prostředkem, že aplikace tvOS distribuují k příjemce na zařízení Apple TV. Všechny aplikace, které jsou odeslána do App Storu Apple TV vyžadovat schválení společností Apple.

Aplikace se odešlou do obchodu s aplikacemi prostřednictvím portálu, nazývá *iTunes Connect*. Najdete v tématu naše [konfigurace aplikace v iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) průvodce informace v následujících tématech:

- Správa smluv, daně a bankovnictví.
- Vytváření záznamu připojit iTunes.
- Správa aplikací videa a snímky.
- Správa aplikací název, popis, co je nového, klíčová slova a adresy URL.
- Údržba aplikace obecné informace.
- Uchovávání informací herní centrum.
- Údržba aplikace zkontrolujte informace.
- Informace o cenách za údržbu.
- Uchovávání informací nákupy v aplikaci.
- Zobrazení aplikace recenze.

Když není napsané konkrétně pro tvOS, tento dokument obsahuje informace o tom, jak nastavit a používat tento portál ke vaší aplikaci připravit pro publikování na webu Apple TV App Store. Řada kroků stejná jsou požadovány, jestli jsou nastavení iOS nebo tvOS aplikace.

Po dokončení všech kroků uvedených výše v tématu naše [konfigurace vaší tvOS aplikace v iTunes Connect](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) přidat konkrétní informace tvOS aplikace, které se bude vyžadovat.

Je důležité si uvědomit, že pouze vývojáři, kteří patří do **programu pro vývojáře Apple** mít přístup k iTunes připojit. Členové **Apple Developer Enterprise Program** nemají přístup.

Pokud máte problémy s odesláním aplikace Xamarin.tvOS do App Storu Apple TV, najdete v tématu naše [Poradce při potížích s](~/ios/tvos/troubleshooting.md) průvodce. Obsahuje několik známých problémů, které se můžete setkat a jak je vyřešit v Xamarin.tvOS.

Další informace naleznete [publikování do App Storu Apple TV](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md) průvodce.

<a name="In-House-Distribution" />

## <a name="in-house-distribution"></a>Interní distribuční

Někdy označuje jako *distribuční Enterprise*, interní distribuční umožňuje členům **Apple Developer Enterprise Program** k distribuci aplikací interně na ostatní členy stejné organizace. Interní distribuční má výhod vyžadující kontrolu obchodu s aplikacemi a nutnosti žádné omezení počtu zařízení, na kterých je možné nainstalovat aplikace. Je ale důležité si uvědomit, že **Apple Developer Enterprise Program** členů **není** mají přístup k iTunes připojit, a proto je zodpovědný za distribuci aplikace držitel licence.

Další informace o získávání nastavení a jak se bude distribuovat aplikace interně, naleznete [interní distribuční průvodce](~/ios/deploy-test/app-distribution/in-house-distribution.md). Tento dokument je specifické pro iOS, ale používají se stejné techniky pro tvOS aplikace.

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>Ad Hoc distribuce

Xamarin.tvOS aplikace může být testována uživatele prostřednictvím ad hoc distribuci, která je k dispozici na obou **programu pro vývojáře Apple**a **Apple Developer Enterprise Program**a umožňuje zařízením s až 100 Apple TV má být testována. Nejlepší případ použití pro ad hoc distribuci je distribuce v rámci vaší společnosti, když iTunes připojení není možnost.

Další informace o získávání nastavení a jak se bude distribuovat aplikace interně, naleznete [průvodci distribucí Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md). Znovu tento dokument je specifické pro iOS, ale se stejné techniky, které se používají pro tvOS aplikace.

<a name="Summary" />

## <a name="summary"></a>Souhrn

V tomto článku jste dali stručný přehled distribuční mechanismy, které jsou k dispozici pro Xamarin.tvOS aplikace. Zavedla v App Storu Apple TV, Ad Hoc a interně nasazení a poskytuje odkazy na podrobnější informace.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
