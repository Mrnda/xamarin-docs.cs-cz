---
title: "Nové rozhraní API pro vyhledávání"
description: "Tento článek popisuje použití nových aplikace vyhledávání rozhraní API poskytované iOS 9 umožníte uživatelům hledat informace a funkce uvnitř vaší aplikace na platformě Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 6ec8cb9b6fdb391afcb8f9baaa641da5aec38f6d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="new-search-apis"></a>Nové rozhraní API pro vyhledávání

_Tento článek popisuje použití nových aplikace vyhledávání rozhraní API poskytované iOS 9 umožníte uživatelům hledat informace a funkce uvnitř vaší aplikace na platformě Xamarin.iOS._

Hledání se rozšířila na iOS 9 zajistit skvělé nové způsoby pro přístup k informacím a funkcím uvnitř aplikace Xamarin.iOS. Pomocí nových rozhraní API vyhledávání aplikace, je obsah aplikace jako prohledávatelný prostřednictvím Spotlight a Safari výsledky hledání, aby Handoff a Siri připomínky a návrhy. To umožňuje uživatelům rychle se dostat k aktivity a informace o hloubkové v rámci vaší aplikace.

Kromě toho nové rozhraní API pro vyhledávání usnadňují integraci vyhledávání ve vaší aplikaci bez před vyhledáváním implementace. Z toho důvodu Apple deklarací, že obvykle trvá několik hodin, aby se obsah aplikace iOS 9 všeobecně vyhledávat pomocí vyhledávací aplikaci.

[![](images/intro01.png "Příklad všeobecně vyhledávat pomocí vyhledávací aplikaci obsahu aplikace iOS 9")](images/intro01.png#lightbox)

Aplikace vyhledávání se skládá ze tří samostatných rozhraní API:

1. [**NSUserActivity** ](nsuseractivity.md) – to je rozšířením aby Handoff API, které Apple vydala v iOS 8. Slouží k zkontrolujte historii interakce aplikace vyhledávat i) uživatelem.

2. [**Základní Spotlight** ](corespotlight.md) -umožňuje aplikacím indexu jeho obsah předkládané ve výsledcích hledání. Funguje jako databáze rozhraní API, které položky můžete přidat nebo odebrat a je nejlepší způsob, jak index soukromý obsah v rámci aplikace.

3. [**WebMarkup** ](web-markup.md) – pro aplikace, které poskytují přístup k jejich obsahu prostřednictvím webové rozhraní (ne jenom z aplikace). Webový obsah může být označena speciální odkazy, které bude procházen společností Apple a poskytují přímé propojení do vaší aplikace na zařízení s iOS 9 uživatele.

## <a name="selecting-an-app-search-approach"></a>Výběr aplikace vyhledávání přístup

Rozhodování, který tyto metody k implementaci, závisí na typech interakce poskytované vaší aplikace a typ obsahu, který představuje.

Pomocí následujících pokynů:

- [**NSUserActivity** ](nsuseractivity.md) – pomocí této framework pro zajištění vyhledatelnost obsah veřejného a privátního i taky vyhledatelnost bodů navigace v rámci vaší aplikace.

- [**Základní Spotlight** ](corespotlight.md) – použijte toto rozhraní k zajištění vyhledatelnost privátní dat uložených na zařízení.

- [**Webové značek** ](web-markup.md) – použijte toto rozhraní zajistit vyhledatelnost pro aplikace, která představují obsah nejen z aplikace, ale také z webu aplikace.

Všechny aplikace hledání přístupy se liší a může být použít jednotlivě, nicméně navržené Apple je používat společně. Při použití více než jeden z přístupů k indexu konkrétní položku, zkontrolujte, jestli jsou používají stejné **ID položky** na každý přístup, že jednotlivé odkazy pracovní společně.

Použití více než jeden ze způsobů nejen zajišťuje, obsah bude nalezen koncovým uživatelem, ale také pomáhá zlepšit hodnocení vaší položky z v rámci hledání.

Při hodnocení procesu pro vývojáře, většinou transparentní interakce uživatele s danou položku váží výraznou při toto pořadí (například uživatel zaznamenávat odkaz).
Tím, že poskytuje bohatou a informativní položky, můžete zajistit, že uživatel bude být enticed pracovat s obsahem, proto vyvolání jeho hodnocení.

## <a name="what-content-to-index"></a>Co obsahu do indexu

Apple poskytuje následující návrhy, jak jsou obsah a akce zajistit indexy vyhledávání pro ve vaší aplikaci:

 - Veškerý obsah, zobrazit, vytvořit nebo kurátorované uživatelem z vaší aplikace.
 - Navigace body a funkce v rámci aplikace.
 - Nové zprávy, obsah nebo jiných typů položek zobrazí aplikace, které jste právě stáhli do zařízení, jako například věcí.

## <a name="app-search-enhancements"></a>Vylepšení hledání aplikace

Základní Spotlight v iOS 10 nabízí několik vylepšení vyhledávání aplikace, jako:

- **Crowdsourcovaný přímý odkaz oblíbenosti (s rozdílovou o ochraně osobních údajů)** -poskytuje způsob, jak zvýšit úroveň obsah aplikace s přímým odkazem ve výsledcích hledání.
- **Hledání v aplikaci** -použít novou `CSSearchQuery` třídy zajišťuje možnosti vyhledávání Spotlight v aplikaci podobná fungování aplikace e-mailu, zprávy a poznámky.
- **Hledání pokračování** – umožňuje uživatelům spustit vyhledávání Spotlight nebo Safari a pak otevřete aplikaci a pokračovat v hledání.
- **Vizualizace výsledků ověření** -společnosti Apple [nástroj ověření rozhraní API pro vyhledávání aplikace](https://search.developer.apple.com/appsearch-validation-tool) nyní zobrazovat vizuální reprezentace webu značek a propojování přímým preforming testy.
- **Zpráva Image aplikace pro sdílení** -umožňuje oblíbených bitové kopie v aplikaci pro sdílení ve zprávách (prostřednictvím rozšíření aplikace zpráv), než se objeví ve vyhledávání Spotlight zadat.

Další informace, najdete v tématu naše [vylepšení vyhledávání aplikace](~/ios/platform/search/app-search-enhancements.md) průvodce.

### <a name="proactive-suggestions"></a>Proaktivní návrhy

iOS 10 uvede nové způsoby řízení zapojení do aplikace tím, že systém proaktivně poskytnout užitečné informace automaticky uživateli v příslušnou dobu. Stejně jako iOS 9 poskytuje možnost přidávat hloubkové hledání do aplikace pomocí funkce služby, aby Handoff a návrhy Siri s iOS 10 aplikace můžou zpřístupnit funkce, které lze zobrazit pro uživatele v systému z v následujících umístěních:

- Aplikace přepínači
- Zamykací obrazovce
- CarPlay
- Mapy
- Interakce Siri
- Návrhy QuickType 

Aplikace zpřístupní tuto funkci do systému pomocí kolekce technologií, jako třeba [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), webové značek, Spotlight jádra, MapKit, přehrávač médií a UIKit.

Další informace, najdete v tématu naše [proaktivní návrhy](~/ios/platform/search/proactive-suggestions.md) průvodce.

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých nové funkce rozhraní API pro vyhledávání tohoto iOS 9 poskytuje pro aplikace pro Xamarin.iOS. Ho zahrnutých [NSUserActivity](nsuseractivity.md), [základní Spotlight](corespotlight.md) a [webové značek](web-markup.md) metody pro indexování obsahu. Je dokončeno s krátkou diskuzi o při zadaný hledaný přístup, a případně co typy obsahu by měly být indexované.



## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Průvodce programováním v aplikaci vyhledávání](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
