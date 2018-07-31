---
title: Rozhraní API pro vyhledávání v Xamarin.iosu
description: Tento článek se věnuje nové aplikace rozhraní API služby Search poskytovaného Iosem 9 umožňuje uživatelům vyhledávat informace a funkce uvnitř vaší aplikace na platformě Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4e73e1bc34df8628790a3734e5b3b32a687fdf14
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351649"
---
# <a name="search-apis-in-xamarinios"></a>Rozhraní API pro vyhledávání v Xamarin.iosu

_Tento článek popisuje použití rozhraní API pro vyhledávání aplikací iOS 9 poskytuje umožňuje uživatelům vyhledávat informace a funkce uvnitř vaší aplikace na platformě Xamarin.iOS._

Došlo k rozbalení vyhledávání v systému iOS 9 k poskytování skvělé nové způsoby přístupu k informacím a funkce v aplikaci Xamarin.iOS. Rozhraní API služby Search novou aplikaci, je obsah aplikace jako prohledávatelný prostřednictvím Spotlight a Safari výsledky hledání, aby Handoff a Siri připomenutí a návrhy. To umožňuje uživatelům rychlý přístup k aktivity a informace hloubkové v rámci vaší aplikace.

Kromě toho nové rozhraní API pro hledání usnadňují integraci hledání v aplikaci bez předchozího provedení vyhledáváním. Z tohoto důvodu Apple deklarací, obvykle trvá několik hodin, aby se obsah aplikace systému iOS 9 univerzálně prohledávatelná pomocí hledání aplikací.

[![](images/intro01.png "Příklad obsah aplikace systému iOS 9 univerzálně prohledávatelná pomocí hledání aplikací")](images/intro01.png#lightbox)

Hledání aplikací se skládá ze tří samostatných rozhraní API:

1. [**NSUserActivity** ](nsuseractivity.md) – to je rozšířením rozhraní API předání, který Apple v iOS 8. Používá se k daly historie interakce aplikace prohledávat veřejně a soukromou) uživatelem.

2. [**Základní Spotlight** ](corespotlight.md) – umožňuje aplikacím indexovat obsah, které se budou zobrazovat ve výsledcích hledání. To funguje, jako jsou databáze rozhraní API, kde můžete přidávat a odebírat položky a je nejlepší způsob, jak index soukromý obsah v rámci aplikace.

3. [**WebMarkup** ](web-markup.md) – pro aplikace, které poskytují přístup ke svému obsahu prostřednictvím webovým rozhraním (ne pouze z v rámci aplikace). Webový obsah, mohou být označeny zvláštní odkazy, které bude možné procházet společností Apple a poskytují přímé odkazování do vaší aplikace na zařízení s Iosem 9 uživatele.

## <a name="selecting-an-app-search-approach"></a>Výběr metodiky hledání aplikací

Rozhodování o tom, které tyto metody k implementaci závisí na typech interakce poskytované vaší aplikace a typu obsahu, který představuje.

Pomocí následujících pokynů:

- [**NSUserActivity** ](nsuseractivity.md) – použití tohoto rozhraní k zajištění vyhledatelnost obsah veřejného a privátního i také vyhledatelnost body navigace v rámci vaší aplikace.

- [**Základní Spotlight** ](corespotlight.md) – použití tohoto rozhraní k zajištění vyhledatelnost soukromá data ukládaná do zařízení.

- [**Webové značky** ](web-markup.md) – použití tohoto rozhraní k zajištění vyhledatelnost aplikace, které se jejich obsah, nejen z v rámci aplikace, ale také z webu aplikace.

Každé hledání aplikací přístupy se liší a je možné použít jednotlivě, ale jejich vzájemnou spolupráci Apple návrhu. Při použití více než jedním z přístupů k indexování určitou položku, ujistěte se, že používáte stejné **ID položky** na každý přístup, proto tato osoba propojí pracovní společně.

Použití více než jedním z přístupů nejen zajišťuje, že váš obsah se nenašel koncový uživatel, ale také pomáhá zlepšit vaši položku pořadí z v rámci vyhledávání.

Proces hodnocení v většinou transparentní pro vývojáře, interakce uživatele s danou položku váží silně na toto pořadí (například uživatel zaznamenávat odkaz).
Tím, že poskytuje bohaté a informativní položky, můžete zajistit, že uživatel bude být enticed pracovat s obsahem, tedy zvýšení jeho pořadí.

## <a name="what-content-to-index"></a>Obsah do indexu

Apple nabízí následující návrhy, jaký obsah a akce k poskytování vyhledávacích indexů pro ve vaší aplikaci:

 - Žádný obsah zobrazit, vytvořit nebo spravované uživatelem z v rámci vaší aplikace.
 - Body navigace a funkcí v rámci aplikace.
 - Věci, jako jsou nové zprávy, obsah nebo jiných druhů položek zobrazených na aplikace, které jste právě stáhli do zařízení.

## <a name="app-search-enhancements"></a>Vylepšení hledání aplikací

Základní Spotlight IOS 10 nabízí několik vylepšení hledání aplikací, jako:

- **Popularita Crowdsourcingu přímým odkazem (s rozdílovou o ochraně osobních údajů)** – poskytuje způsob, jak zvýšit úroveň ve výsledcích obsah aplikace přímý odkaz.
- **Hledání v aplikaci** -použít novou `CSSearchQuery` třídě poskytnout možnost vyhledávání Spotlight v aplikaci podobný fungování aplikace e-mailu, zpráv a poznámky.
- **Hledat pokračování** – umožňuje uživateli spustit vyhledávání Spotlight nebo Safari a pak otevřete aplikaci a pokračovat v hledání.
- **Vizualizace výsledků ověření** – od společnosti Apple [nástroje pro ověření rozhraní API aplikace hledání](https://search.developer.apple.com/appsearch-validation-tool) při preforming testy se teď zobrazují vizuální reprezentaci těchto značek a hloubkové propojení webu.
- **Zpráva aplikace sdílení Imagí** – umožňuje oblíbených imagí v aplikaci pro sdílení v zprávy (přes rozšíření aplikace zpráv) se zobrazí ve vyhledávání Spotlightu k dispozici.

Další informace najdete v tématu naše [vylepšení hledání aplikací](~/ios/platform/search/app-search-enhancements.md) průvodce.

### <a name="proactive-suggestions"></a>Proaktivní návrhy

nové způsoby jízdy zapojení do aplikace pro iOS 10 uvede tím, že systém aktivně prezentovat užitečné informace automaticky uživateli ve vhodných chvílích. Stejně jako iOS 9 poskytuje možnost přidávat hloubkové prohledávání do aplikace pomocí Spotlightu, aby Handoff a návrhy Siri, se systémem iOS 10 aplikaci můžete zveřejnit funkce, které lze zobrazit uživateli systému z v následujících umístěních:

- Přepínání aplikace
- Zamykací obrazovce
- CarPlay
- Mapy
- Interakce Siri
- QuickType návrhy 

Aplikace zpřístupňuje tuto funkci do systému pomocí kolekce technologií, jako [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), webové značky, Core Spotlightu, Mapkitu, Media Player a UIKit.

Další informace najdete v tématu naše [proaktivní návrhy](~/ios/platform/search/proactive-suggestions.md) průvodce.

## <a name="summary"></a>Souhrn

Tento článek se popisuje nové funkce rozhraní API pro vyhledávání tohoto systému iOS 9 poskytuje pro aplikace Xamarin.iOS. Je zahrnutých [NSUserActivity](nsuseractivity.md), [Core Spotlight](corespotlight.md) a [webové značky](web-markup.md) metody pro indexování obsahu. Bylo dokončeno krátkou představíme kdy je vhodné používat zadanými vyhledávanými přístup a jaké typy obsahu by měly být indexovány.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [pro vývojáře v systému iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Průvodce programováním v hledání aplikací](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
