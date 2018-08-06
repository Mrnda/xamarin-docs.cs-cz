---
title: Základní informace o nákupy v aplikaci a konfigurace v Xamarin.iOS
description: Tento dokument popisuje nákupy v aplikaci v Xamarin.iOS, hovoříte o relevantní informace o pravidlech, konfigurace a iTunes připojit.
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 9ded160ad4b31346c400e63d739a3dc21f6304d3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787240"
---
# <a name="in-app-purchase-basics-and-configuration-in-xamarinios"></a>Základní informace o nákupy v aplikaci a konfigurace v Xamarin.iOS

Implementace nákupy v aplikaci vyžaduje, aby aplikace využívat rozhraní API StoreKit na zařízení. StoreKit spravuje veškerou komunikaci se servery společnosti Apple iTunes k získání informací o produktu a provádění transakcí. Profil zřizování musí být nakonfigurované k nákupu v aplikaci a informace o produktu je třeba zadat v iTunes připojit.

 [![](in-app-purchase-basics-and-configuration-images/image1.png "StoreKit spravuje veškerou komunikaci s společnosti Apple, jak je znázorněno v tomto grafu")](in-app-purchase-basics-and-configuration-images/image1.png#lightbox)

Použití obchodu s aplikacemi pro poskytnutí nákupu v aplikaci vyžaduje následující nastavení a konfiguraci:

-  **iTunes Connect** – konfigurace produkty, které prodeje a nastavení izolovaného prostoru uživatelské účty pro testovací nákupu. Jste musí také poskytli vaše bankovnictví a daň informace společnosti Apple, že můžete uhradit fondů shromážděných vaším jménem.
-   **iOS Provisioning Portal** – vytvoření identifikátor svazku a povolení přístupu k obchodu s aplikacemi pro vaši aplikaci.
-  **Uložení Kit** – přidání kódu do vaší aplikace pro zobrazení produktů, nákup produktů a obnovení transakce.
-  **Vlastní kód** – ke sledování nákupy provedené zákazníci a poskytnout produkty nebo služby zakoupili. Může také potřebovat implementaci serverové proces ověření potvrzení, pokud vaše produkty obsahovat stažený ze serveru (například knihy a katalogu problémy) obsah.


Existují dvě úložiště Kit "serveru prostředí":

-  **Produkční** – transakce s skutečné peníze. Dostupné jenom přes aplikace, které byla odeslána a schválené společností Apple. Nákupy v aplikaci produkty musí také zkontrolovat a schválit předtím, než jsou k dispozici v provozním prostředí.
-  **Izolovaný prostor** – kde se stane testování. Produkty jsou k dispozici zde okamžitě po vytvoření (procesu schvalování se vztahuje pouze do produkčního prostředí). Transakce v izolovaném prostoru vyžadují testovacích uživatelů (ne skutečné Apple ID) k provedení transakce.

## <a name="in-app-purchase-rules"></a>Pravidla nákupy v aplikaci

Nelze přijmout jiné způsoby platby digitální produkty nebo služby uvnitř vaší aplikace, ani je zmínili nebo odkazovat vaši uživatelé k nim z aplikace. To znamená, že při nákupu v aplikaci je nejvhodnější mechanismus platebních nelze přijmout platební karty nebo PayPal. Jsou ve speciálním případě k nákupu digitální produkty mimo aplikaci, ale pro používání v aplikaci, jako je například nákup knih na webu, které jsou spojeny s konkrétním "login" a pomocí, "login" v aplikaci umožní uživateli přístup zakoupené knihy.
Není povoleno zmínili nebo odkaz na externí nákupu funkce aplikací, které tímto způsobem – vývojáři musí komunikovat tato funkce uživatelům v jiné způsoby (možná prostřednictvím e-mailové marketingové nebo jiných přímý kanál).

Ale vzhledem k tomu, že nemůžete použít nákupy v aplikaci pro fyzické zboží, v tomto případě, kdy jsou povoleny použít mechanismus alternativní platby (např. platební karty či účtu PayPal) z aplikace.

Apple musí před probíhá na prodej – název, popis a snímek 'produktu, je potřeba ke kontrole schválit každého produktu. Zkontrolujte dobu produktu jsou stejné jako pro recenze aplikace.

Všechny ceny nelze vybrat pro svůj produkt – můžete vybrat pouze 'cena vrstvou', která má určitou hodnotu v každé země a měny, který podporuje Apple. Vrstvu jinou cenu nemůže mít v různých trhů.

## <a name="configuration"></a>Konfigurace

Před psaní kódu v aplikaci nákupu je nutné provést některé nastavení práce v iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) a iOS Provisioning Portal ( [developer.apple.com/iOS](http://developer.apple.com/iOS)).

Před psaní jakéhokoli kódu by měly být dokončené tyto tři kroky:

-  **Účet pro vývojáře Apple** – odeslat bankovnictví a zdanění informace společnosti Apple.
-  **iOS Provisioning Portal** – Ujistěte se, má vaše aplikace platné ID aplikace (není zástupný znak hvězdičkou * v ní) a v aplikaci zakoupení aktivoval.
-  **iTunes Správa aplikací připojit** – přidání produktů do aplikace.


### <a name="apple-developer-account"></a>Účet pro vývojáře Apple

Vytváření a distribuci bezplatných aplikací vyžaduje velmi malé konfigurace v nástroji [iTunes Connect](https://itunesconnect.apple.com), ale prodávat placené aplikace nebo nákupy v aplikaci, musíte poskytnout informace o bankovnictví a zdanění Apple. Klikněte na **smlouvy, daň a bankovnictví** z hlavní nabídky znázorněno zde:

 [![](in-app-purchase-basics-and-configuration-images/image2.png "Klikněte na smlouvy, daň a bankovnictví z hlavní nabídky")](in-app-purchase-basics-and-configuration-images/image2.png#lightbox)

By měl mít vývojářského účtu **aplikace pro iOS placené** smlouvy platit, jak je vidět na tomto snímku obrazovky:

 [![](in-app-purchase-basics-and-configuration-images/image3.png "Vývojářský účet by měl mít iOS, které platí smlouvy placené aplikace")](in-app-purchase-basics-and-configuration-images/image3.png#lightbox)

Nebudete moci testovat žádné funkce StoreKit, dokud nebudete mít **placené aplikace pro iOS** kontrakt – volání StoreKit ve vašem kódu se nezdaří, dokud zpracovala Apple vaší **smlouvy, daň a bankovnictví** informace.

### <a name="ios-provisioning-portal"></a>iOS Provisioning Portal

Nové aplikace jsou nastaveny v **ID aplikace** části **iOS Provisioning Portal**. Chcete-li vytvořit nové ID aplikace, přejděte na [Member Center IOS Provisioning Portal](https://developer.apple.com/membercenter/index.action), přejděte na **identifikátory, certifikátů a profilů** části portálu a kliknutím na tlačítko **identifikátory** pod *aplikací iOS*. Klikněte "+" v horní pravé generovat nové ID aplikace.


Formulář pro vytváření nových **ID aplikace**

 vypadá takto:

 [![](in-app-purchase-basics-and-configuration-images/image4.png "Formulář pro vytvoření nového ID aplikace")](in-app-purchase-basics-and-configuration-images/image4.png#lightbox)

Zadejte něco vhodné pro *popis*, abyste mohli snadno identifikovat toto ID aplikace v seznamu. Pro *předponu ID aplikace*, vyberte tým ID.

#### <a name="bundle-identifierapp-id-suffix-format"></a>Formát ID příponu sady identifikátor/aplikace

Můžete použít libovolný řetězec, který chcete pro vaše **identifikátor svazku** (za předpokladu, bude jednoznačný ve vašem účtu), ale doporučuje Apple, můžete podle formátu DNS zpětného místo použít libovolný libovolný řetězec. Ukázkovou aplikaci, která doprovází tento článek používá com.xamarin.storekit.testing pro identifikátor balíku, ale je stejně platná pro použití identifikátor jako my_store_example (i když Apple to nedoporučuje se).

> [!IMPORTANT]
> Apple také umožňuje zástupný znak – hvězdičku má být přidán na konec **identifikátor svazku** tak, aby jeden ID aplikace se dá použít pro více aplikací, ale _zástupnému znaku ID aplikace nemůže být použit pro AppPurchase_. Příklad zástupnému znaku identifikátor svazku může být com.xamarin.*

#### <a name="enabling-app-services"></a>Povolení aplikaci služby

Všimněte si, že **nákupy v aplikaci** automaticky povolí v seznamu služeb:

 [![](in-app-purchase-basics-and-configuration-images/image5.png "Nákupy v aplikaci se automaticky povolí v seznamu služeb")](in-app-purchase-basics-and-configuration-images/image5.png#lightbox)

#### <a name="provisioning-profiles"></a>Profily zřizování

Vytváření profilů zřizování produkční a vývoj, jako za normálních okolností byste, výběr ID aplikace, které jste nastavili pro nákup v aplikaci. Odkazovat [zřizování zařízení iOS](~/ios/get-started/installation/device-provisioning/index.md) a [publikování do obchodu s aplikacemi](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) příručky pro další informace.

## <a name="itunes-connect"></a>iTunes Connect

Klikněte na tlačítko **Moje aplikace** v iTunes připojit k vytvoření nebo úprava položky aplikace iOS. Zobrazí se zde Přehled stránky aplikace:

 [![](in-app-purchase-basics-and-configuration-images/image6.png "Přehled stránky aplikace")](in-app-purchase-basics-and-configuration-images/image6.png#lightbox)

Klikněte na tlačítko **nákupy v aplikaci** vytváření nebo editace vaši produktů pro prodej. Tento snímek obrazovky znázorňuje ukázkové aplikace s několika produkty již přidán:

 [![](in-app-purchase-basics-and-configuration-images/image7.png "Ukázkové aplikace s několika produkty již přidán.")](in-app-purchase-basics-and-configuration-images/image7.png#lightbox)

Postup přidání nové produkty má dva kroky:

1.   Zvolte typ produktu: [![](in-app-purchase-basics-and-configuration-images/image8.png "zvolte typ produktu")](in-app-purchase-basics-and-configuration-images/image8.png#lightbox) 
2.   Zadejte atributy produktu, včetně Id produktu, cenová úroveň a lokalizované popisy: [![](in-app-purchase-basics-and-configuration-images/image9.png "zadání atributů produktů")](in-app-purchase-basics-and-configuration-images/image9.png#lightbox)

Pole, vyžaduje se pro každý produkt nákupy v aplikaci jsou následující:


### <a name="reference-name"></a>Název odkazu

Název odkazu nezobrazuje se uživatelům; je pro interní použití a zobrazí se pouze v iTunes připojit.

### <a name="product-id-format"></a>Formát ID produktu

Identifikátor produktu může obsahovat pouze alfanumerické znaky (A-Z,-z, 0-9), podtržítko (_) a znaky tečka (.). Přestože je možné použít libovolný řetězec pro vaše identifikátory, Apple doporučuje formát zpětného DNS. Například ukázková aplikace používá tento identifikátor svazku:

 `com.xamarin.storekit.testing`

Proto konvence k identifikaci nákupy v aplikaci produkty vypadat takto:

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

Tyto zásady vytváření názvů nevynucuje, jednoduše doporučení vám pomůžou spravovat vaše produkty. Navíc navzdory podle stejné konvenci DNS zpětného, jsou identifikátory produktu *nesouvisejí* identifikátor svazku a jsou není nezbytné ke spuštění stejné řetězcem. Stále je platná pro použití identifikátory jako photo_product_greyscale (i když Apple to nedoporučuje se).

ID produktu se uživatelům nezobrazí, ale slouží k odkazování produktu v kódu aplikace.

### <a name="product-type"></a>Typ produktu

Existuje pět typů, které můžete nabídnout produktu nákupy v aplikaci:

1.  **Použití** – věcí, které jsou 'použité nahoru", například ve hře měny, který může trávit přehrávač. Pokud uživatel nemá zálohování a obnovení nebo v opačném případě se aktualizovala jejich zařízení, použití transakce nejsou získat obnovené také (což by efektivně předat přehrávač stejné přes znovu). Kód aplikace musí být nezapomeňte poskytnout použití položky při dokončení transakce.
1.  **Non-consumable** – produkty, které uživatel "vlastní" jednou zakoupili, jako je například herní úrovni nebo digitální katalogu problém.
1.  **Automatické obnovitelných odběry** – stejně jako katalogu odběru reálného na konci období odběru Apple automaticky znovu účtuje zákazníka a rozšiřuje doby platnosti předplatného navždy nebo dokud zákazník explicitně Zruší ho. Toto je upřednostňovaný platby pro Newsstand aplikace (ve skutečnosti, aplikace musí podporovat tento způsob platby schválení pro distribuci Newsstand).
1.  **Uvolněte předplatné** – může být pouze nabídnut v Newsstand povolené aplikace a může zákazník pro přístup k obsahu předplatné na svých zařízeních. Bezplatných předplatných nikdy nevyprší.
1.  **Obnovení bez předplatného** – se má použít pro prodeje časově omezené přístup k statický obsah, jako je například jeden měsíc přístup k archivu fotografii.


 *Tento dokument popisuje aktuálně pouze první dva typy (Consumable a Non-Consumable).*

 <a name="Price_Tiers" />

### <a name="price-tiers"></a>Cena vrstev

App Store neumožňuje zvolte libovolné ceny pro vaše produkty – Apple poskytuje vrstvy pevné ceny, které můžete vybrat z. V každé měně jsou pevné ceny a Apple si vyhrazuje právo upravit relativní ceny (například po dlouhodobě změnu rychlost relativní cizích měn mezi konkrétní měny a dolaru USA).

Apple poskytuje matici cena vám pomohou vybrat správnou úroveň měna/cenu, který chcete. Zobrazí se zde výňatek matice ceny (srpen 2012):

 [![](in-app-purchase-basics-and-configuration-images/image10.png "Výňatek matice cena srpen 2012")](in-app-purchase-basics-and-configuration-images/image10.png#lightbox)

V době psaní (článku červen 2013) jsou 87 vrstev z USD 0,99 k 999,99 USD. Ceny matice zobrazí cenu, budou platit vašim zákazníkům a také dobu, který obdržíte od společnosti Apple – to je méně za 30 % a také všechny místní daně jsou požadována ke shromažďování (Všimněte si v příkladu, že USA a Kanadě prodejci příjem 70 c pro 99 p c roduct, zatímco australské prodejci přijímat pouze 63 c kvůli ' zboží &amp; služby daň se uloženo na prodejní ceny).

Ceny svůj produkt lze aktualizovat kdykoli, včetně změn naplánované ceny, které se projeví na budoucí datum. Tento snímek obrazovky ukazuje, jak přidat změnu s datem budoucí cena – ceny dočasně mění se z vrstvy 1 do vrstvy 3 pro měsíc září pouze:

 [![](in-app-purchase-basics-and-configuration-images/image11.png "Kde cenu dočasně mění se z vrstvy 1 do vrstvy 3 pro měsíc září pouze změnu s datem budoucí cena")](in-app-purchase-basics-and-configuration-images/image11.png#lightbox)

### <a name="free-products-not-supported"></a>Volné produkty není podporován

I když Apple poskytl speciální možnost bezplatné předplatné pro Newsstand aplikace, není možné nastavit nulové (zdarma) ceny pro všechny ostatní typy nákupy v aplikaci. Zatímco můžete upravit (ie. nižší) ceny pro různé propagační akce, nelze provádět volné přes iTunes Connect nákupy v aplikaci.

### <a name="localization"></a>Lokalizace

V iTunes připojení můžete zadat jiný název a popis text pro libovolný počet podporovaných jazyků. Jednotlivé jazyky lze přidat nebo upravit v prostřednictvím místní okno:

 [![](in-app-purchase-basics-and-configuration-images/image12.png "Jednotlivé jazyky lze přidat nebo upravit v prostřednictvím místní okno")](in-app-purchase-basics-and-configuration-images/image12.png#lightbox)   
   
   
   
 Při zobrazení informací o produktu ve vaší aplikaci, je k dispozici budete moci zobrazit prostřednictvím StoreKit lokalizované text. Zobrazení měny musí také lokalizované zobrazíte správné symbolů a decimal formátování – toto formátování je zmínka v dalších částech dokumentu.

### <a name="app-store-review"></a>Zkontrolujte obchodu s aplikacemi

Stejné jako aplikace – každý produkt je zkontrolovat společností Apple dříve, než mohou přejít na prodej. Produkty mohou být odmítnuty nevhodných obsahu v názvu nebo popisu nebo Apple rozhodnout, zda jste vybrali typ nesprávný produktu (např. jste si vytvořili knihy nebo katalogu problém, ale používá typ použití produktu). Recenzemi produktů může trvat stejně dlouho jako revizi aplikace.

Prvním aplikace je odeslána s v aplikaci zakoupení povolené (ať už je novou aplikaci nebo funkci se přidal do některého ze stávajících), musíte také zvolit některé produkty odeslat s ním. Na portálu Connect iTunes vás vyzve k tomu, jak je vidět na tomto snímku obrazovky:

 [![](in-app-purchase-basics-and-configuration-images/image13.png "Na portálu Connect iTunes vás vyzve k odeslání některé produkty, i")](in-app-purchase-basics-and-configuration-images/image13.png#lightbox)   
   
   
   
 Aplikace a nákupy v aplikaci projde kontrolou společně, aby se všechny získat schválení najednou (tak tuto aplikaci nelze přejít do úložiště bez jakékoli schválené produkty!).

Po schválení vaší první verzí funkce nákupy v aplikaci, můžete přidat další produkty a odesílat je ke kontrole kdykoli. Můžete také odeslat novou verzi společně s produkty konkrétní funkce nákupy v aplikaci, pomocí **podrobnosti o verzi** stránky jako navrhuje řádku.

Odkazovat [App Store kontrolní pokyny](https://developer.apple.com/appstore/guidelines.html) Další informace.

 [Část 2 - Přehled Kit úložiště a informace o načítání produktu](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
