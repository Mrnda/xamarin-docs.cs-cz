---
title: Přehled distribuce aplikace Xamarin.iOS
description: Tento dokument poskytuje přehled distribuční technik, které jsou k dispozici pro aplikace Xamarin.iOS a slouží jako ukazatel na podrobnější dokumenty v tomto tématu.
ms.prod: xamarin
ms.assetid: 341D36DB-BB07-FA94-BCC9-5F8C0B18C179
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 815277e9a4f9384d92bf17376f426cacd40dbc9f
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209437"
---
# <a name="xamarinios-app-distribution-overview"></a>Přehled distribuce aplikace Xamarin.iOS

_Tento dokument poskytuje přehled distribuční technik, které jsou k dispozici pro aplikace Xamarin.iOS a slouží jako ukazatel na podrobnější dokumenty v tomto tématu._

Po byla vyvinuta aplikaci Xamarin.iOS, je dalším krokem v životního cyklu softwaru distribuovat aplikace pro uživatele, jak je uvedeno v části zvýrazněných v následujícím diagramu:


[![](images/publishingdiagram.png "Poté, co byla vyvinuta aplikaci pro iOS, dalším krokem je distribuovat aplikace pro uživatele, jak je uvedeno v části zvýrazněná tohoto diagramu")](images/publishingdiagram.png#lightbox)


Apple nabízí tyto způsoby distribuovat aplikace pro iOS, která podporuje Xamarin.iOS:

1. [**Obchod s aplikacemi**](#App_Store_Distribution)
2. [**Interní (Enterprise)**](#In-House_Distribution)
2. [**Ad Hoc**](#Ad_Hoc_Distribution)

Tyto scénáře vyžadují, aby aplikace zřídit pomocí odpovídající *profil pro zřizování*. Profily zřizování jsou soubory, které obsahují informace, jakož i identity aplikace a mechanismus určený distribuce pro podpis kódu. Pro distribuci App Store také obsahují informace o zařízeních, jaké aplikace se dá nasadit na.

<a name="App_Store_Distribution"/>

## <a name="app-store-distribution"></a>Distribuce obchodu s aplikacemi

> [!IMPORTANT]
> Apple [oznámilo](https://developer.apple.com/news/?id=05072018a) , od července 2018 se všechny aplikace a aktualizace odeslána na obchod s aplikacemi a musí mít byla vytvořena pomocí iOS 11 SDK a [podporují zobrazení iPhone X](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md).

To je hlavním prostředkem, že jsou k příjemce na zařízeních s iOS distribuovat aplikace iOS. Všechny aplikace, které jsou odeslána do obchodu s aplikacemi vyžadovat schválení společností Apple.

Aplikace se odešlou do obchodu s aplikacemi prostřednictvím portálu, nazývá *iTunes Connect*. [Konfigurace aplikace v iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) příručka obsahuje informace o tom, jak nastavit a používat tento portál Příprava aplikace Xamarin.iOS pro publikování na webu App Store.

Je důležité si uvědomit, že pouze vývojáři, kteří patří do **programu pro vývojáře Apple** mít přístup k iTunes připojit. Členové **Apple Developer Enterprise Program** nemají přístup.

Další informace naleznete [úložiště distribuce aplikací](~/ios/deploy-test/app-distribution/app-store-distribution/index.md) průvodce.

<a name="In-House_Distribution"/>

## <a name="in-house-distribution"></a>Interní distribuční

Někdy označuje jako *distribuční Enterprise*, interní distribuční umožňuje členům **Apple Developer Enterprise Program** k distribuci aplikací interně na ostatní členy stejné organizace. Interní distribuční má výhod vyžadující kontrolu obchodu s aplikacemi a nutnosti žádné omezení počtu zařízení, na kterých je možné nainstalovat aplikace. Je ale důležité si uvědomit, že **Apple Developer Enterprise Program** členů **není** mají přístup k iTunes připojit, a proto je zodpovědný za distribuci aplikace držitel licence.

Další informace o získávání nastavení a jak se bude distribuovat aplikace interně, naleznete [interní distribuční průvodce](~/ios/deploy-test/app-distribution/in-house-distribution.md).

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>Ad Hoc distribuce

Aplikace Xamarin.iOS může být testována uživatele prostřednictvím ad hoc distribuci, která je k dispozici na obou **programu pro vývojáře Apple**a **Apple Developer Enterprise Program**a umožňuje až 100 iOS zařízení, která má být testována. Nejlepší případ použití pro ad hoc distribuci je distribuce v rámci společnosti při iTunes připojení není možné.

Další informace o získávání nastavení a jak se bude distribuovat aplikace interně, naleznete [průvodci distribucí Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md).

## <a name="summary"></a>Souhrn

V tomto článku jste dali stručný přehled distribuční mechanismy, které jsou k dispozici pro aplikace Xamarin.iOS. Zavedla iTunes App Storu, Ad Hoc a interně nasazení a poskytuje odkazy na podrobnější informace.

## <a name="related-links"></a>Související odkazy

- [Distribuce v obchodě App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Konfigurace aplikace v iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publikování do obchodu App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Interní distribuční](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Jednorázová distribuce](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Soubor iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Podpora IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Odstraňování potíží](~/ios/deploy-test/troubleshooting.md)
