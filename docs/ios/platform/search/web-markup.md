---
title: "Hledání se webové značek"
description: "Přidání webové vyhledávání výsledky, které můžete propojit zpět do vaší aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 8812c6a234e05e4d651effbeb83a7bcad38dc683
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="search-with-web-markup"></a>Hledání se webové značek

Pro aplikace, které poskytují přístup k obsahu prostřednictvím webového serveru (ne jenom z aplikace), webový obsah může být označena speciální odkazy, které bude procházen společností Apple a poskytují přímé propojení aplikace na zařízení uživatele iOS 9.

Pokud vaše aplikace iOS již podporuje mobilní přímé propojení a webu uvedené přímé odkazy na obsah v rámci vaší aplikace, Apple _Applebot_ webový prohledávací modul indexu tento obsah, který se automaticky přidají do svého indexu cloudu:

[![](web-markup-images/webmarkup01.png "Přehled Index cloudu")](web-markup-images/webmarkup01.png#lightbox)

Apple bude surface těchto výsledků ve výsledcích vyhledávání Spotlight a Safari vyhledávání.
Pokud uživatel klepnutím na jednu z těchto výsledků (a mají nainstalované aplikace) pak budete přesměrováni na obsah ve vaší aplikaci:

[![](web-markup-images/webmarkup02.png "Přímý odkaz z webu ve výsledcích hledání")](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>Povolení webového obsahu indexování

Existují čtyři kroky, aby vás obsah aplikace vyhledávat pomocí značek Web:

1. Zajistěte, aby Apple lze zjistit a indexu webu vaší aplikace tak, že definujete jej jako **podporu** nebo **Marketing** webu v iTunes připojit.
2. Zajistěte, aby vaše aplikace Web obsahoval požadované značky k implementaci mobilní přímé propojení. Najdete v částech níže další podrobnosti.
3. Povolte přímý odkaz na zpracování v aplikaci s iOS.
4. Přidáte kód pro strukturovaná data prezentované web vaší aplikace k poskytování výsledku bohatý a nestačí, aby koncový uživatel. Přestože tento krok není nezbytně nutné, důrazně doporučujeme společností Apple.

V následujících částech se tyto kroky si podrobně projít.

## <a name="make-your-apps-website-discoverable"></a>Zjistitelnost web vaší aplikace

Nejjednodušší způsob, jak mají Apple najít web vaší aplikace je můžete použít jako buď **podporu** nebo **Marketing** webu při odesílání prostřednictvím iTunes připojit aplikace společnosti Apple.

## <a name="using-smart-app-banners"></a>Používání hlaviček inteligentní aplikace

Zadejte inteligentní informační zpráva aplikaci na vašem webu nabídne zrušte odkaz do vaší aplikace. Pokud aplikace ještě není nainstalovaný, Safari automaticky vyzve uživatele k instalaci vaší aplikace. V opačném případě může klepnout použití **zobrazení** odkaz na spusťte aplikaci z webu. Například pokud chcete vytvořit inteligentní informační zpráva aplikaci, můžete použít následující kód:

```xml
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

Další informace najdete v tématu společnosti Apple [povýšení aplikace s inteligentní aplikace hlaviček](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html) dokumentaci.

## <a name="using-universal-links"></a>Univerzální odkazy

Nové univerzální odkazy na iOS 9, poskytují lepší alternativou inteligentní hlaviček aplikace nebo existující vlastní schémata URL tím, že poskytuje následující:

- **Jedinečné** – stejnou adresu URL nelze získat více než jeden web.
- **Zabezpečení** – pro web, který zajišťuje vlastní web vy a řádně propojen se vyžaduje certifikát podepsaný držitelem vaši aplikaci.
- **Flexibilní** – koncový uživatel může řídit, jestli spustí adresu URL webu nebo aplikace.
- **Univerzální** – stejnou adresu URL lze použít k definování obsah vašeho webu a vaší aplikace.

## <a name="using-twitter-cards"></a>Pomocí karty služby Twitter.

Můžete zadat přímé odkazy na obsah vaší aplikace pomocí karty služby Twitter. Příklad:

```xml
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

Další informace najdete v tématu na Twitteru [Twitter protokol karty](http://dev.twitter.com/cards/mobile) dokumentaci.

## <a name="using-facebook-app-links"></a>Pomocí odkazů aplikace Facebook

Můžete zadat přímé odkazy na obsah vaší aplikace pomocí odkazu aplikace Facebook. Příklad:

```xml
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

Další informace najdete v tématu na Facebooku [aplikace odkazy](http://applinks.org) dokumentaci.

## <a name="opening-deep-links"></a>Otevírání přímých odkazů

Je nutné přidat podporu pro otevření a zobrazení přímé odkazy v aplikaci Xamarin.iOS. Upravit **AppDelegate.cs** souborů a přepsat `OpenURL` metodu ke zpracování vlastní formát adresy URL. Příklad:

```csharp
public override bool OpenUrl (UIApplication application, NSUrl url, string sourceApplication, NSObject annotation)
{

    // Handling a URL in the form http://company.com/appname/?123
    try {
        var components = new NSUrlComponents(url,true);
        var path = components.Path;
        var query = components.Query;

        // Is this a known format?
        if (path == "/appname") {
            // Display the view controller for the content
            // specified in query (123)
            return ContentViewController.LoadContent(query);
        }
    } catch {
        // Ignore issue for now
    }

    return false;
}
```

Ve výše uvedeném kódu Těšíme se na adresu URL obsahující `/appname` a předání hodnotu `query` (`123` v tomto příkladu) do vlastního zobrazení řadiče v naší aplikaci zobrazíte požadovaný obsah pro uživatele.

## <a name="providing-rich-results-with-structured-data"></a>Poskytuje bohaté výsledků s strukturovaných dat

Zahrnutím strukturovaných dat značek můžete poskytovat výsledky bohaté vyhledávání pro koncového uživatele, která jdou nad rámec jednoduše název a popis. Zahrnují bitové kopie, data konkrétní aplikace (například hodnocení) a akcí pro výsledky pomocí strukturovaných dat značek.

Bohaté výsledky jsou další zapojení a může pomoct zlepšit vaše hodnocení v cloudu na základě indexu vyhledávání podle roste více uživatelů s nimi pracovat.

Jednou z možností pro zajištění strukturovaných dat značek je pomocí Open Graph. Příklad:

```xml
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

Další informace najdete v tématu [Open Graph](http://ogp.me) webu.

Jiné běžný formát pro strukturovaných dat je formát mikrodata schema.org společnosti. Příklad:

```xml
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
    <span itemprop="ratingValue">4** stars -
    <span itemprop="reviewCount">255** reviews


```

Stejné informace může být reprezentován ve formátu JSON-LD schema.org na:

```xml
<script type="application/ld+json">
    "@content":"http://schema.org",
    "@type":"AggregateRating",
    "ratingValue":"4",
    "reviewCount":"255"
</script>
```

Na obrázku je příkladem metadata z vašeho webu, který poskytuje bohaté výsledků pro koncového uživatele:

[![](web-markup-images/deeplink01.png "Rozšířené možnosti hledání výsledků prostřednictvím strukturovaných dat značek")](web-markup-images/deeplink01.png#lightbox)

Apple aktuálně podporuje následující typy schémat ze schema.org:

 - AggregateRating
 - ImageObject
 - InteractionCount
 - Nabízí
 - Organizace
 - PriceRange
 - Recepturách
 - SearchAction

Další informace o těchto typech schéma, najdete v tématu [schema.org](http://schema.org).

## <a name="providing-actions-with-structured-data"></a>Zajištění akce s strukturovaných dat

Konkrétní typy strukturovaná Data vám umožní výsledků na vyhledávacím být řešitelné koncovým uživatelem. Aktuálně jsou podporovány následující akce:

 - Vytáčení telefonní číslo.
 - Získávání směr mapy na danou adresu.
 - Přehrávání soubor zvuku a videa.

Můžete například definovat akci, která má vytočte telefonní číslo může vypadat následovně:

```xml
<div itemscope itemtype="http://schema.org/Organization">
    <span itemprop="telephone">(408) 555-1212**


```

Když se tento výsledek hledání pro koncového uživatele, zobrazí se ikona malé phone ve výsledku. Pokud uživatel klepnutím na ikonu, bude mít název zadanému počtu.

Následující kód HTML by přidat akci k přehrání zvukového souboru z výsledků vyhledávání:

```xml
<div itemscope itemtype="http://schema.org/AudioObject">
    <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**


```

Nakonec následující HTML přidat akci, kterou chcete získat pokyny z výsledek hledání:

```xml
<div itemscope itemtype="http://schema.org/PostalAddress">
    <span itemprop="streetAddress">1 Infinite Loop**
    <span itemprop="addressLocality">Cupertino**
    <span itemprop="addressRegion">CA**
    <span itemprop="postalCode">95014**


```

Další informace najdete v tématu společnosti Apple [Web Developer vyhledávání aplikace](http://developer.apple.com/ios/search/).



## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Průvodce programováním v aplikaci vyhledávání](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
