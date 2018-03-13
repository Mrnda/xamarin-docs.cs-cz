---
title: "Hledání s NSUserActivity"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 8376ce2ccff6732fa0c89d6030b9af36d29c5085
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="search-with-nsuseractivity"></a>Hledání s NSUserActivity

`NSUserActivity` byla zavedena v iOS 8 a slouží k poskytování dat pro aby Handoff.
Umožňuje vytvořit aktivity v konkrétní části aplikace, který je pak možné předat do jiné instance aplikace spuštěné na zařízení iOS jiný. Přijímající zařízení poté můžete dál aktivity spustit na předchozích zařízení, výběr vpravo tam, kde uživatel přestali. Další informace o používání aby Handoff, najdete v tématu naše [Úvod k předání](~/ios/platform/handoff.md) dokumentaci.

Pro iOS 9, nové `NSUserActivity` můžete (i) a indexovat pomocí vyhledávání Spotlight a Safari. Označením `NSUserActivity` jako prohledávatelný a přidání indexovanou metadata, může být uvedený aktivity ve výsledcích hledání na zařízení s iOS.

[![](nsuseractivity-images/apphistory01.png "Přehled aplikace historie")](nsuseractivity-images/apphistory01.png#lightbox)

Pokud uživatel vybere výsledek vyhledávání, který patří k aktivitě z vaší aplikace, spustí se aplikace a aktivity popsaného `NSUserActivity` bude restartován a uživateli.

Následující vlastnosti `NSUserActivity` slouží k podpoře aplikace vyhledávání:

 - `EligibleForHandoff` – Pokud `true`, tato aktivita je možné v rámci operace aby Handoff.
 - `EligibleForSearch` – Pokud `true`, tato aktivita bude přidán do indexu na zařízení a zobrazí ve výsledcích hledání.
 - `EligibleForPublicIndexing` – Pokud `true`, tato aktivita bude přidán do indexu cloudové společnosti Apple a zobrazí uživatelům (prostřednictvím vyhledávání), které ještě nenainstalovali vaší aplikace na svém zařízení s iOS. Najdete v článku [veřejné indexování pro hledání](#Public-Search-Indexing) části níže další podrobnosti.
 - `Title` – Poskytuje název pro vaši aktivitu a se zobrazí ve výsledcích hledání. Uživatelé mohou také prohledávat text nadpisu sám sebe.
 - `Keywords` – Je pole řetězců, které používají k popisu vaše aktivity, která bude indexované a jako prohledávatelný koncovým uživatelem.
 - `ContentAttributeSet` – Je `CSSearchableItemAttributeSet` používá k další aktivitě podrobně popisují a poskytování bohaté obsahu ve výsledcích hledání.
 - `ExpirationDate` – Pokud chcete aktivitu pouze zobrazený do konkrétního data, můžete zadat datum sem.
 - `WebpageURL` – Pokud aktivity lze zobrazit na webu nebo pokud vaše aplikace podporuje přímé odkazy na Safari, můžete nastavit odkaz přejděte sem.

## <a name="nsuseractivity-quickstart"></a>NSUserActivity rychlý start

Postupujte podle těchto pokynů k implementaci vyhledávat `NSUserActivity` ve vaší aplikaci:

- [Vytváření identifikátory typu aktivity](#creatingtypeid)
- [Vytvoření aktivity](#createactivity)
- [Odpovídá na aktivitu](#respondactivity)
- [Indexování pro veřejné hledání](#indexing)

Některé [další výhody](#benefits) pomocí `NSUserActivity` Chcete-li vyhledávat obsah.

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>Vytváření identifikátory typu aktivity

Před vytvořením aktivity hledání, budete muset vytvořit _identifikátor typu aktivity_ pro identifikaci. Identifikátor typu aktivity je krátký řetězec přidán do `NSUserActivityTypes` pole aplikace **Info.plist** souboru, který slouží k jednoznačné identifikaci typu aktivity daného uživatele. V poli pro každou aktivitu, která podporuje a vystavuje aplikace vyhledávání aplikace bude jeden záznam. 

Apple navrhuje pomocí stylu DNS zpětného zápisu pro identifikátor typu aktivity, aby se zabránilo kolizí. Příklad: `com.company-name.appname.activity` pro konkrétní aplikaci na základě aktivity nebo `com.company-name.activity` pro aktivity, které můžete spustit mezi více aplikacemi.

Identifikátor typu aktivity se používá při vytváření `NSUserActivity` instance k identifikaci typu aktivity. Když aktivita pokračuje v důsledku uživatele klepnutím výsledek hledání, typ aktivity (spolu s ID aplikace Team) určuje, které aplikace spustit pokračujte aktivity.

Chcete-li vytvořit požadovaný typ identifikátory aktivit pro podporu toto chování, upravte **Info.plist** souboru a přepněte do **zdroj** zobrazení. Přidat `NSUserActivityTypes` klíče a vytváření identifikátorů v následujícím formátu:

[![](nsuseractivity-images/type01.png "Klíč NSUserActivityTypes a požadované identifikátory v editoru plist.")](nsuseractivity-images/type01.png#lightbox)

V předchozím příkladu jsme vytvořili jeden nový identifikátor aktivity typu aktivity hledání (`com.xamarin.platform`). Při vytváření vlastních aplikací, nahraďte obsah `NSUserActivityTypes` pole s typem identifikátory aktivit, které jsou specifické pro aktivity aplikace podporuje.

<a name="createactivity" />

## <a name="creating-an-activity"></a>Vytvoření aktivity

Následuje příklad vytvoření aktivity pro vyhledávání hostované indexu na zařízení:

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.BecomeCurrent();
```

Nyní může přidat další podrobnosti o nastavením `ContentAttributeSet` vlastnost naše `NSUserActivity` následujícím způsobem:

[![](nsuseractivity-images/apphistory02.png "Přehled přidání podrobnosti vyhledávání")](nsuseractivity-images/apphistory02.png#lightbox)

Pomocí `ContentAttributeSet` můžete vytvořit bohaté vyhledávání výsledky, které přesvědčit koncového uživatele s nimi pracovat.

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>Odpovídá na aktivitu

Reagovat na uživatel klepnutím na výsledek hledání (`NSUserActivity`) vaší aplikace upravit **AppDelegate.cs** souborů a přepsat `ContinueUserActivity` metoda. Příklad:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

Všimněte si, že se jedná o stejnou přepsání metody používá reagovat na požadavky aby Handoff. Nyní když uživatel klikne na odkaz z vaší aplikace ve výsledcích vyhledávání Spotlight, vaší aplikace bude začlenění do popředí (nebo spustit, pokud ještě není spuštěná) a obsah, navigace nebo funkce reprezentována tento odkaz se zobrazí:

[![](nsuseractivity-images/apphistory03.png "Obnovení předchozího stavu z hledání")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>Indexování pro veřejné hledání

Jak jsme viděli výše, iOS 9 snadno poskytnout vyhledávání přístup k obsahu aplikace a funkce, které má uživatel již zjištění a použití na zařízeních iOS dané. S veřejné indexování iOS 9 poskytuje způsob pro uživatele, kteří nebyly zjištěny ještě obsah nebo funkce (nebo i kdo nenainstalovali aplikace) k získání výsledků v jejich hledání příliš.

Veřejné indexování funguje následujícím způsobem:

1. Když vytvoříte aktivitu pro vaši aplikaci označte ji jako veřejné.
2. Veřejné aktivity se pošle společnosti Apple a indexované v cloudu.
3. Jako další uživatelé komunikovat s vaší aplikací na zařízení a používat konkrétní funkce nebo obsah reprezentována aktivity, roste v pořadí.
4. Oblíbené veřejné výsledky bude k dispozici jiným uživatelům, i když nemají nainstalovanou aplikací.
5. Tyto veřejné výsledky se zobrazí v vyhledávání Spotlight a Safari (Pokud aktivity obsahuje adresu URL).

Můžeme trvat činnost privátní vyhledávání, který jsme vytvořili výše a rozšířit tak, aby se veřejné:

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.EligibleForPublicIndexing = true;
activity.BecomeCurrent();
```

Právě, protože aktivity byla nastavena pro veřejné indexování nastavením `EligibleForPublicIndexing = true`, neznamená, že ho bude automaticky přidat do indexu veřejného cloudu společnosti Apple. Nejdřív musí být splněny následující podmínky:

1. Je nutné se ve výsledcích hledání a vybrat mnoha uživateli. Výsledky zůstanou privátní, dokud prahové hodnoty aktivity engagement byla splněna.
2. Zřizování aplikace zabraňuje žádná uživatelská data z je indexované a zveřejněny.

<a name="benefits" />

## <a name="additional-benefits"></a>Další výhody

Přijetím vyhledávání aplikace prostřednictvím `NSUserActivity` ve vaší aplikaci, získáte také následující funkce:

- **Aby handoff** – vzhledem k tomu, že aplikace vyhledávání je vystavení obsahu, navigace a/nebo funkce, pomocí stejného mechanismu jako aby Handoff (`NSUserActivity`), můžete snadno povolit uživatelům vaší aplikace spustit aktivitu na jednom zařízení a pokračovat na další.
- **Návrhy Siri** -společně s standardní návrhy, které obvykle provádí návrhy Siri, aktivující z vaší aplikace automaticky navrhované.
- **Inteligentní připomenutí Siri** -uživatelé budou moct požádat Siri s připomínkou, o aktivitách z vaší aplikace.



## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Průvodce programováním v aplikaci vyhledávání](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [Příspěvek blogu & Ukázka](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
