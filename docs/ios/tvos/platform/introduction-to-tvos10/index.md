---
title: "Úvod do tvOS 10"
description: "Tento článek představuje všechny nové a změněné rozhraní API a funkce dostupné v tvOS 10 pro vývojáře Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8bc379e507287cd609dca8440b1210b7f1514114
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-tvos-10"></a>Úvod do tvOS 10

_Tento článek představuje všechny nové a změněné rozhraní API a funkce dostupné v tvOS 10 pro vývojáře Xamarin.tvOS._

S novou tvOS 10 SDK Apple zařadila nové rozhraní API a služby, které umožňují vývojáři k vytvoření nové kategorie aplikace a funkce. 

Další informace o tvOS 10, najdete v tématu společnosti Apple [tvOS + aplikace](https://developer.apple.com/tvos/) dokumentaci.

## <a name="whats-new-in-tvos-10"></a>Co je nového v tvOS 10

Apple přidala několik nových rozhraní API a služby v tvOS 10 společně s mnoho vylepšení stávajících funkcí, včetně:

## <a name="new-user-interface-styles"></a>Nové uživatelské rozhraní styly

tvOS 10 nyní podporuje motiv světlý i Light uživatelské rozhraní, všechna sestavení v UIKit určí, bude automaticky přizpůsobí se jí, podle preferencí uživatele.

Při vytváření a implementaci nové vlastní ovládací prvky uživatelského rozhraní, měli používat Vývojář [UITraitCollection](https://developer.apple.com/reference/uikit/uitraitcollection) třída přizpůsobit motiv vybraného uživatele.

Další informace najdete v tématu naše [nové uživatelské rozhraní styly](~/ios/tvos/platform/user-interface-styles.md) dokumentaci.

## <a name="security-and-privacy-enhancements"></a>Zabezpečení a ochrany osobních údajů vylepšení

Apple udělal několik vylepšení zabezpečení a ochrany osobních údajů v tvOS 10, který vám pomůže vývojáře zlepšit zabezpečení své aplikace a zajištění ochrany osobních údajů koncového uživatele.

V důsledku toho je třeba deklarovat aplikace běžící na watchOS 3 (nebo novější) staticky jejich pokus o přístup k určité funkce nebo informace o uživateli tak, že zadáte jednu nebo více o ochraně osobních údajů konkrétní klíče v jejich `Info.plist` soubory, které vysvětlují, uživateli, proč aplikace chce získat přístup.

Vzhledem k tomu, že tvOS 10 sdílí tyto změny s iOS 10, najdete v tématu naše iOS 10 [zabezpečení a ochrany osobních údajů vylepšení](~/ios/app-fundamentals/security-privacy.md) Průvodce pro další informace.

## <a name="video-subscriber-account"></a>Účet video odběratele

Nové pro tvOS 10 rozhraní Video odběratele účet umožňuje aplikacím dané podporu ověření streamování nebo vyžádání video-on-demand k ověření se svým poskytovatelem TV kabel nebo satelitní pomocí Single-Sign in prostředí pro koncového uživatele.

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>Široké barev

tvOS 10 rozšiřuje podporu pro rozšířené rozsah pixelů formáty a prostory celou rozsah, barvy v celém systému, včetně architektury, jako je například základní grafické prvky, základní Image, operačního systému a AVFoundation. Podpora pro zařízení s wide barev zobrazí se další opatřeny náběhem / tím, že poskytuje toto chování v rámci celého grafiky zásobníku.

Kromě toho `UIKit` byla změněna pro práci v nové rozšířené **sRGB** barevný prostor, což usnadňuje kombinovat barev v rozsahů barev široké bez ztráty významně zvýšit výkon.

Apple nabízí následující osvědčené postupy při práci s wide barev:

 - `UIColor` Teď používá sRGB barvu místa a nadále nebude CLAMP – hodnoty `0.0` k `1.0` rozsahu. Pokud aplikace závisí na předchozí chování svorka, ji budou muset upravit pro tvOS 10.
 - Pokud aplikace provede vlastní vykreslování `UIImages`, použít novou [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) třídu k určení použití formátů rozšířené rozsah nebo standard-range.
 - Při použití nízké úrovně rozhraní API, například základní grafické prvky nebo operačního systému k zajištění zpracování obrázků, aplikace by měla používat rozšířené rozsah barva místa a pixelů formátu, který podporuje 16bitové hodnot s plovoucí desetinnou. V případě potřeby bude muset ručně CLAMP – hodnoty barev součásti aplikace.
 - Základní grafické prvky, základní Image a shadery výkonu operačního systému všech poskytují nové metody pro převod mezi dvěma barevné prostory.

Další informace, najdete v tématu naše [Úvod do široké barva](~/ios/platform/wide-color.md) průvodce.

## <a name="newly-available-existing-frameworks"></a>Nově dostupné existující rozhraní

Několik rozhraní, které byly k dispozici v iOS (a ne tvOS), je k dispozici pro tvOS 10, jako:

 - ExternalAccessory
 - HomeKit
 - MultipeerConnectivity
 - Fotografie
 - ReplayKit
 - UserNotification

## <a name="additional-framework-changes"></a>Další Framework změny

Kromě hlavní framework změny a dodatky uvedených výše Apple udělal mnoho dalších menší framework změn v tvOS 10.

Další informace, najdete v tématu naše [další změny Framework](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md) průvodce.

## <a name="deprecated-apis"></a>Zastaralá rozhraní API

Žádné rozhraní API nebo rozhraní byly zastaralá tvOS 10. Najdete v článku společnosti Apple [tvOS 10 API rozdíly](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html) dokumentaci úplný seznam úpravy rozhraní API.



## <a name="related-links"></a>Související odkazy

- [Ukázky tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Co je nového v tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
