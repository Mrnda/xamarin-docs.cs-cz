---
title: Vylepšení hledání aplikace Xamarin.iOS
description: Tento článek se zabývá rozšířením Apple udělal hledání aplikace v iOS 10 a jejich implementaci v Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 06c405a15c26e02908d609bc27cac2c0509e5028
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787881"
---
# <a name="app-search-enhancements-in-xamarinios"></a>Vylepšení hledání aplikace Xamarin.iOS

_Tento článek se zabývá rozšířením Apple udělal hledání aplikace v iOS 10 a jejich implementaci v Xamarin.iOS._

V iOS 10 Apple udělal několik vylepšení vyhledávání aplikace například propojení přímým Crowdsourcovaná, vyhledávání v aplikaci, pokračování hledání a výsledky ověření vizualizace. Tento článek se zabývá implementace tyto funkce v aplikaci pro Xamarin.iOS.

## <a name="about-app-search-enhancements"></a>O vylepšení vyhledávání aplikací

Základní Spotlight v iOS 10 nabízí několik vylepšení vyhledávání aplikace, jako:

- **Crowdsourcovaný přímý odkaz oblíbenosti (s rozdílovou o ochraně osobních údajů)** -poskytuje způsob, jak zvýšit úroveň obsah aplikace s přímým odkazem ve výsledcích hledání.
- **Hledání v aplikaci** -použít novou `CSSearchQuery` třídy zajišťuje možnosti vyhledávání Spotlight v aplikaci podobná fungování aplikace e-mailu, zprávy a poznámky.
- **Hledání pokračování** – umožňuje uživatelům spustit vyhledávání Spotlight nebo Safari a pak otevřete aplikaci a pokračovat v hledání.
- **Vizualizace výsledků ověření** -společnosti Apple [nástroj ověření rozhraní API pro vyhledávání aplikace](https://search.developer.apple.com/appsearch-validation-tool) nyní zobrazovat vizuální reprezentace webu značek a propojování přímým preforming testy.
- **Zpráva Image aplikace pro sdílení** -umožňuje oblíbených bitové kopie v aplikaci pro sdílení ve zprávách (prostřednictvím rozšíření aplikace zpráv), než se objeví ve vyhledávání Spotlight zadat.

V následujících částech se bude zabývat tato témata podrobněji.

## <a name="crowdsourced-deep-link-popularity"></a>Přímý odkaz Crowdsourcovaná oblíbenosti

iOS 10 poskytuje mechanismus pro počet četnost, že oblíbené odkazy přímým do aplikace dodržíte uživatelem a používá tyto informace ke zlepšení hodnocení aplikace je přitom identitu uživatele pomocí obsahu ve výsledcích hledání  *Rozdílovou ochrany osobních údajů*.

Pro aplikace využívající `NSUserActivity` objekty a zadejte adresy URL přímý odkaz `EligibleForPublicIndexing` vlastnost nastavena na hodnotu `true`, iOS 10 odešle podmnožinu *rozdílové hodnoty hash ochrany osobních údajů* na servery společnosti Apple. Pak tyto informace slouží ke zvýšení úrovně oblíbených obsahu v aplikaci ve výsledcích hledání.

Další informace o implementaci přímým propojení v aplikaci Xamarin.iOS, najdete v tématu naše [hledání se NSUserActivity](~/ios/platform/search/nsuseractivity.md) dokumentaci.

## <a name="in-app-searching"></a>Hledání v aplikaci

Implementací nové [CSSearchQuery](https://developer.apple.com/reference/corespotlight/cssearchquery) třída, aplikace může poskytnout vyhledávání Spotlight na a odpovídající pravidlo technologie hledání obsahu uvnitř samostatně, aniž by uživatel museli opustit aplikace (podobným způsobem e-mailu, zprávy a poznámky pracovní).

Obvykle se aplikace podporující `CSSearchQuery` nebude muset zachovat své vlastní, index samostatné vyhledávání. 

## <a name="search-continuation"></a>Pokračování hledání

V iOS 9, Společnost Apple vydala rozhraní API pro vyhledávání (například základní Spotlight `NSUserActivity` a webové značek) zajistit přímým libosti obsahu v rámci aplikace umožňuje uživatelům vyhledávat obsah pomocí rozhraní vyhledávání Spotlight i Safari. V tématu naše [nové rozhraní API pro vyhledávání](~/ios/platform/search/index.md) dokumentace pro další podrobnosti.

V iOS 10 Apple staví na této funkce tím, že se uživateli spustit vyhledávání Spotlight nebo Safari a potom pokračujte v hledání při otevření aplikace. 

Chcete-li tuto funkci implementovat, upravte aplikace `Info.plist` soubor, přidejte `CoreSpotlightContinuation` klíče typu **Boolean** a jeho hodnotu nastavte `YES`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](app-search-enhancements-images/search01.png "Úpravy CoreSpotlightContinuation v souboru Info.plist")](app-search-enhancements-images/search01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](app-search-enhancements-images/searchw01.png "Úpravy CoreSpotlightContinuation v souboru Info.plist")](app-search-enhancements-images/search01.png#lightbox)

-----

Reagovat na uživatele pokračováním výsledků na vyhledávacím (`NSUserActivity`), upravit `AppDelegate.cs` souboru a přepíše `ContinueUserActivity` metoda. Příklad:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchQuery.ContinuationActionType) {
            var search = userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString);
            // Continue user's search here...
        }
        break;
    }

    return true;
}
```

Tento kód vyhledá typ akce pokračování dotazu (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`), potom načte aktuální dotaz uživatele z `NSUserActivity` třídy uživatelské informace slovníku (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`). Z tohoto místa musí aplikace provedete akce vedoucí k pokračování hledání uživatele.

Další informace o práci se hledání v aplikaci Xamarin.iOS, najdete v tématu naše [hledání se základní Spotlight](~/ios/platform/search/corespotlight.md) dokumentaci.

## <a name="visualization-of-validation-results"></a>Vizualizace výsledků ověření

Společnosti Apple [nástroj ověření rozhraní API pro vyhledávání aplikace](https://search.developer.apple.com/appsearch-validation-tool) teď zobrazuje vizuální reprezentace značek a přímý propojení na web (včetně značek, jak je uvedeno v [Schema.org](http://schema.org/)) při preforming testy.

Pomocí nástroje ověření můžete zobrazit vývojář podporovaném informace, že má webový prohledávací modul Applebot indexované webu, například název, popis, adresa URL a všechny další prvky.

Další informace o práci se značkami webové, najdete v tématu naše [vyhledávací s webové značek](~/ios/platform/search/web-markup.md) dokumentaci.

## <a name="message-app-image-sharing"></a>Obrázek zpráva aplikace pro sdílení

Pokud rozšíření aplikace zpráv poskytuje bitové kopie pro sdílení ve zprávách, rozšíření můžete nakonfigurovat tak, aby uživatel k vyhledávání Spotlight pro oblíbené bitové kopie z v rámci zprávy, aniž by museli opustit aplikace.

Chcete-li povolit tuto funkci, postupujte takto:

1. Vytváření rozšíření aplikace zprávu.
2. Přidat `com.apple.developer.associated-domains` na oprávnění aplikace a budou zahrnovat seznam webových domén, které hostují bitové kopie je rozšíření zpráv aplikace pro sdílení. Pro každou doménu, zadejte `spotlight-image-search` služby.
3. Přidat `apple-app-site-association` souboru na web, který je hostitelem bitové kopie. Tento soubor obsahuje slovník pro `spotlight-image-search` služby a obsahuje ID aplikace, které je předpona ID týmu nebo ID aplikace, za nímž následuje ID sady. Soubor může obsahovat maximálně 500 cesty a vzorů, které bude indexovat pomocí Spotlight a součástí bitové kopie oblíbených hledání. Další informace najdete v tématu společnosti Apple [vytvoření a nahrání souboru přidružení](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4) dokumentaci.
4. Povolit Applebot procházet weby. Najdete v tématu společnosti Apple [o Applebot](https://support.apple.com/HT204683) dokumentaci.

V tématu naše [integrace aplikace zpráv](~/ios/platform/message-app-integration/index.md) dokumentace pro další podrobnosti.

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých rozšířením Apple udělal hledání aplikace v iOS 10 a jejich implementaci v Xamarin.iOS.



## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
