---
title: Konfigurace vaší tvOS aplikace v iTunes Connect
description: Tento článek obsahuje další Průvodce iOS konfigurace aplikace v iTunes Connect pro tvOS konkrétní konfigurace.
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d082a980572349da1b7e6155b3aa4de41512796f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>Konfigurace vaší tvOS aplikace v iTunes Connect

_Tento článek obsahuje další Průvodce iOS konfigurace aplikace v iTunes Connect pro tvOS konkrétní konfigurace._


Kromě konfigurace a nastavení, které budete muset provést pomocí následujících iOS [konfigurace aplikace v iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) průvodce, tento dokument popisuje konkrétní konfigurace, které bude vyžadovat, aby verze Xamarin.tvOS aplikace v App Storu Apple TV.

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>Přidání tvOS prodejní verzi

Ať už vytváříte nové aplikace, které budou vydány na App Storu Apple TV, nebo přidání podpory Apple TV do stávající aplikace pro iOS, budete muset mít vytvořil záznam připojit iTunes a nakonfiguruje ji pomocí následující iOS konkrétní provede:

- [Vytvoření záznamu připojit iTunes](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Správa aplikací videa a snímky](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [Správa název, popis, co je nového, klíčová slova a adresy URL](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [Zachování obecné informace](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

Volitelně můžete také potřebovat:

- [Uchovávání informací herní Centrum](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [Uchovávání informací nákupy v aplikaci](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

Se všemi výše uvedené kroky dokončení otevřete iTunes záznam připojit a vyberte, které chcete přidat podporu tvOS pomocí na bočním panelu vlevo vaší aplikace:

[![](itunes-connect-images/connect01.png "Přidání podpory tvOS pomocí na bočním panelu vlevo")](itunes-connect-images/connect01.png#lightbox)

Obrazovky konkrétní informace tvOS pak budou k dispozici pro danou iTunes záznam připojení:

[![](itunes-connect-images/connect02.png "Na obrazovce tvOS konkrétní informace")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>tvOS informace o verzi

Na bočním panelu vlevo vyberte **1.0 Příprava pro odeslání** části tvOS aplikace:

[![](itunes-connect-images/connect03.png "tvOS informace o verzi")](itunes-connect-images/connect03.png#lightbox)

Na této obrazovce zadejte následující informace:

- Požadované snímky obrazovky, popis, klíčová slova a adresy URL.
- Aplikace obecné informace, jako je číslo verze o autorských právech a stáří hodnocení.
- Volitelné nákupy v aplikaci.
- Volitelné herní Centrum podpora s tabulky a jejich výsledků.
- Požadované aplikace zkontrolujte informace, jako je kontaktu, ukázku účty a poznámky.

Po zadání požadovaných informací klikněte na **Uložit** tlačítko v pravém horním rohu obrazovky a uložit provedené změny:

[![](itunes-connect-images/connect04.png "tvOS připravené k odeslání informací o verzi")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>Příprava na odeslání ke kontrole

Až budete připravení nakonec odeslat aplikace Xamarin.tvOS do App Storu Apple TV ke kontrole, vraťte se na aplikace iTunes záznam připojení a klikněte na **odeslat ke kontrole** tlačítko v pravém horním rohu obrazovky:

[![](itunes-connect-images/connect05.png "Odeslat ke kontrole")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Souhrn

V tomto článku jste dali přehled tvOS konkrétní nastavení, které jsou potřeba v iTunes připojit k uvolnění tvOS aplikace pro Apple TV App Store.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
