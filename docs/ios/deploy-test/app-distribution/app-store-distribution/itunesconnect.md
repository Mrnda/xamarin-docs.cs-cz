---
title: Konfigurace aplikace v iTunes Connect
description: "Tento článek popisuje kroky potřebné k vytvoření a údržba aplikace pro Xamarin.iOS v iTunes připojení tak, aby mohou být vydány pro distribuci na webu App Store."
ms.topic: article
ms.prod: xamarin
ms.assetid: 74587317-4b15-4904-9582-dcd914827cbc
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: dc21b21e28de155aa7a0e7b5cf9734e752cce9a2
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="configuring-an-app-in-itunes-connect"></a>Konfigurace aplikace v iTunes Connect

_Tento článek popisuje kroky potřebné k vytvoření a údržba aplikace pro Xamarin.iOS v iTunes připojení tak, aby mohou být vydány pro distribuci na webu App Store._

iTunes připojení je sada webových nástrojů, mimo jiné, Správa aplikací iOS na webu App Store. Aplikace pro Xamarin.iOS bude muset být správně nastavili a nakonfigurovali v iTunes připojit předtím, než může být odeslány společnosti Apple k posouzení a nakonec vydané pro prodej, nebo jako bezplatnou aplikaci v App Storu.

iTunes připojení lze použít pro následující:

- Nastavte název aplikace (jak je uvedeno v obchodu s aplikacemi).
- Na zařízeních iOS, které podporuje zadejte snímky obrazovky nebo video o aplikaci v akci.
- Zadejte popis, zrušte zaškrtnutí, stručného aplikace včetně její funkce a využívat pro koncového uživatele.
- Zadejte, kategorie a dílčí kategorie pomoct uživateli najít aplikaci v App Storu.
- Zadejte všechny klíčové slovo, které může uživatel aplikaci najdete další pomoc.
- Zadejte kontakt a adres URL podpory na váš web (vyžadováno Apple).
- Nastavte hodnocení aplikace, který slouží k informování rodičovské kontroly na webu App Store.
- Vyberte prodejní cenu nebo určit, že aplikace budou vydány zdarma.
- Nakonfigurujte volitelné obchodu s aplikacemi technologie jako herním centru a nákupy v aplikaci.

Kromě toho aplikace musí být také kresby na atraktivní, s vysokým rozlišením, které jsou k dispozici v případě, že Apple rozhodne funkci v úložišti aplikací. Další informace najdete v tématu společnosti Apple [iTunes Příručka vývojáře Connect](https://developer.apple.com/support/itunes-connect/).

## <a name="managing-agreements-tax-and-banking"></a>Správa smluv, daně a bankovnictví

**Smlouvy, daň a bankovnictví** části iTunes Connect se používá k poskytování požadované finanční informace týkající se plateb vývojáře iTunes a daň withholdings a sledovat stav veškeré smlouvy máte zavedenou s Apple. Před vydáním aplikace pro iOS v App Storu (zdarma nebo pro prodej), budete muset zavedené správné smlouvy a uzavřeli na veškeré úpravy existující smlouvy.

[![](itunesconnect-images/agreement01.png "Správa smluv, daně a bankovnictví")](itunesconnect-images/agreement01.png#lightbox)

Odsud můžete:

- Zadejte **agenta Team** a definovat jiným rolím uživatelů pro vaše iTunes účtu připojit jako **správce** nebo **finanční**.
- Zaregistrovat se a udržovat **kontrakty** které umožňují organizaci distribuovat aplikace v úložišti aplikací.
- Zadejte **právní subjekt** název (prodejce na webu App Store) používá odpovídající kontrakty, bankovnictví informace, a daňové informace, které jsou přidruženy k vaší organizaci.
- Zadejte **bankovnictví** a **daň** informace, pokud se chystáte prodeje aplikací prostřednictvím App Storu.

Znovu, tyto informace _musí_ být správně nastavené, až na místě a aktuální před iOS můžete žádost pro službu iTunes připojit ke kontrole a verzi. Další informace najdete v tématu společnosti Apple [Správa smluv, daně a bankovnictví](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/ManagingContractsandBanking.html#//apple_ref/doc/uid/TP40011225-CH21-SW1) dokumentaci.

<a name="creating" />

## <a name="creating-an-itunes-connect-record"></a>Vytvoření záznamu připojit iTunes

Před Xamarin.iOS, které aplikace může být nahrán do iTunes Connect pro distribuci přes obchod s aplikacemi musíte vytvořit záznam pro aplikace v iTunes připojit. Tento záznam zahrnuje všechny informace o aplikaci se zobrazí v App Storu (v jako v mnoha jazycích podle potřeby) a všechny informace potřebné ke správě aplikace prostřednictvím procesu rozdělení. Kromě toho se budou použít ke konfiguraci obchodu s aplikacemi technologie jako iAd síťové aplikace nebo herní centrum.

Přidání aplikace iOS pro službu iTunes Connect bude muset být **agenta Team** nebo uživatele, který má **správce** nebo **technické** role.

Proveďte [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Klikněte na **Moje aplikace**:

    [![](itunesconnect-images/add01.png "Klikněte na Moje aplikace")](itunesconnect-images/add01.png#lightbox)
2. Klikněte  **+**  v horní levé dolním rohu a vyberte možnost **nové aplikace pro iOS**:

    [![](itunesconnect-images/add02.png "Přidání nové aplikace pro iOS")](itunesconnect-images/add02.png#lightbox)
3. iTunes Connect se zobrazí **nové aplikace pro iOS** dialogové okno:

    [![](itunesconnect-images/add03.png "Dialogové okno nové aplikace iOS")](itunesconnect-images/add03.png#lightbox)
4. Zadejte **název** a **číslo verze** pro aplikaci tak, jak by měla být zobrazena v úložišti aplikací.
5. Vyberte **primární jazyk**.
6. Zadejte **SKU** číslo, toto je jedinečný, konstanta, identifikátor, který bude použit sledování aplikace. Se nezobrazí pro koncové uživatele a jeho _nelze_ změnit po vytvoření aplikace.
7. Vyberte **ID sady** pro aplikaci, kterou jste vytvořili v Centru pro vývojáře při zřizování aplikace. Toto stejné ID sady prostředků bude nutné použít při přihlašování sady distribuce v sadě Visual Studio pro Mac nebo Visual Studio. Další informace najdete v tématu naše [vytváření profil distribuce](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) a [vyberete profil distribuce v projektu Xamarin.iOS](~/ios/get-started/installation/device-provisioning/index.md) dokumentaci.
8. Klikněte **vytvořit** tlačítko Vytvořit nový iTunes připojit záznam pro aplikaci.

Nová aplikace budou vytvořeny v iTunes připojit a budou připraveny k zadejte požadované informace, například popis, cena, kategorií, hodnocení atd.:

[![](itunesconnect-images/add04.png "Vytvoří se nová aplikace v iTunes Connect")](itunesconnect-images/add04.png#lightbox)

<a name="managing" />

## <a name="managing-app-videos-and-screenshots"></a>Správa aplikací videa a snímky

Jedním z nejdůležitějších prvků pro úspěšně marketingové aplikace iOS v App Storu je skvělým sada snímky obrazovky a volitelně přináší náhled video. Použijte skutečný zobrazení vaší aplikace spuštěná, zvýrazněte interakci s uživatelem a prezentují její jedinečné funkce. Pomocí aplikace preview videa uživatelé získat představu o jaká je k používání aplikace.

Při přijímání snímky obrazovky, Apple navrhuje následující:

- Optimalizace váš snímek pro nejlepší prezentace na zařízeních iOS, které jsou podporovány v aplikaci a ujistěte se, že obsah je čitelná.
- Nemáte rámce na snímku obrazovky v obraze zařízení iOS.
- Nastaví skutečný zobrazení vaší aplikace pomocí přes celou obrazovku, aniž by museli grafiky nebo ohraničení kolem na snímku obrazovky.
- Stavový řádek vždy odebrání snímky obrazovky, očekává iTunes připojit snímky obrazovky dimenzí, které vyloučit této oblasti.
- Pokud je to možné, pořizovat snímky obrazovky na skutečné, s vysokým rozlišením hardwaru iOS sítnice (ne v simulátoru iOS).
- První snímek obrazovky se zobrazí jako výsledek hledání v App Storu na zařízení iPhone a iPad video preview žádné aplikace je k dispozici, takže umístěte nejlepší snímek obrazovky nejprve.
- Použijte všechny snímky obrazovky pět vyhrazení aplikace při zvýraznění situacích, které přesvědčivé.

Apple vyžaduje snímky obrazovky a videa, je třeba zadat v každé velikost obrazovky a řešení, které podporuje vaše aplikace. Kromě toho verze na výšku a šířku, musí být uvedeny podle orientace podporována.

Aktuálně vyžadují se následující obrazovka velikosti a řešení:

[!include[](~/ios/includes/table-itunesconnect.md)]

### <a name="editing-screenshots-in-itunes-connect"></a>Úpravy v iTunes připojit snímky obrazovky

Proveďte [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Klikněte na **Moje aplikace**.
2. Klikněte na vaší aplikace **ikonu**.
3. Vyberte **verze** kartě.
4. Přejděte k položce **snímky obrazovky** části.
5. Vyberte **velikost bitové kopie** a přetáhněte ji do požadované bitových kopií (až 5 na velikost obrazovky):

    [![](itunesconnect-images/screenshot01.png "Vyberte velikost bitové kopie a přetáhněte ji do požadované bitových kopií")](itunesconnect-images/screenshot01.png#lightbox)
6. Opakujte pro všechny požadované obrazovky velikosti.
7. Klikněte **Uložit** tlačítka v horní části obrazovky a uložit provedené změny.

> [!NOTE]
> Poznámka: Apple odmítnou vaše odeslání, pokud snímky obrazovky nebo Video aplikace Preview neshodují funkci aktuální ve vaší aplikaci.

<a name="metadata" />

## <a name="managing-name-description-whats-new-keywords-and-urls"></a>Správa název, popis, co je nového, klíčová slova a adresy URL

Tato část iTunes připojit aplikace záznam obsahuje lokalizované informace o aplikaci, jak funguje, všechny změny na nové verze, klíčová slova použit ve vyhledávání a podpora iAd a všechny podpůrné adresy URL.

### <a name="app-name"></a>Název aplikace.

Zvolte aplikaci popisný název, který zobrazuje, jaké jsou vaše aplikace. Pokuste se jej jako krátký a stručným jako možné zachovat. Název vaší aplikace hraje důležitou roli v tom, jak uživatelé hledat a zjistit, takže zachovat název jednoduchý a snadno pamatovat. Věnujte zvláštní pozornost jak název se zobrazí při prohlížení na zařízení se systémem iOS (iPad, iPhone a iPod touch).

Při výběru názvu aplikace, Apple navrhuje podle následujících pokynů:

- Být krátká, jednoduchý a snadno pamatovat.
- Zkontrolujte, zda že jej není porušení autorských práv ochranné 3. stran.
- Díky odpovídat funkci aplikace.
- Zadejte pro cizí trhy lokalizované názvy, kde je to vhodné.

### <a name="description"></a>Popis

Zápis jasné, stručným a informativní popis aplikace a jejich funkce. Několik prvních řádků jsou nejdůležitější a vám dát šanci skvělé první dojem a kreslení uživatele. Popisují, co vytvoří zvláštní aplikace a která ho odděluje od jiné podobné aplikace.

Apple navrhuje pro zápis aplikace popis následující:

- Obsahují stručné úvodní odstavec nebo dva a krátký seznamu s odrážkami hlavní funkce.
- Zadejte pro cizí trhy lokalizované popisy, kde je to vhodné.
- Zahrňte uživatelské recenze, accolades nebo certifikáty pouze na konci, pokud vůbec.
- Vylepšení čitelnosti pomocí konců řádků a odrážek.
- Zajímat, jak se zobrazí popis aplikace v obchodu s aplikacemi pro každý typ zařízení, abyste měli jistotu, že jsou snadno vidět nejdůležitějším věty v popisu.

### <a name="whats-new"></a>Co je nového

Při nahrávání nové verze aplikace, **co je nového v této verzi** pole bude k dispozici, by se měly dokončit důkladně a vytvoříte promyšlené.

Při vyplňování co je nové informace, Apple navrhuje podle následujících pokynů:

- Přidejte zasílání zpráv na podporu uživatelům aktualizovat.
- Seznam položek v pořadí podle důležitosti a identifikovat změny a opravy chyb.
- Změny v prostý a platná místo technické žargonu k dispozici.

### <a name="keywords"></a>Klíčová slova

Žádný jazyk a strategické klíčová slova, která se týkají funkce aplikace, pomáhají uživatelům snadno najít aplikace při vyhledávání na webu App Store. Kromě toho pokud vaše aplikace obsluhuje až iAd reklamy, iAd aplikace sítě používá klíčová slova při výběru reklamy, jehož cílem je ve vaší aplikaci.

Při výběru klíčová slova, Apple navrhuje následující:

- Nepoužívejte názvy konkurující si aplikace, společnost nebo produkt názvů nebo značkou názvy.
- Nepoužívají obecná slova nebo podmínky.
- Vyhněte se nevhodný nebo problematický termíny nebo důležité slova, jako jsou názvy celebrit.
- Klíčová slova pro cizí trhů v případě nutnosti lokalizovat.

### <a name="urls"></a>URL – adresy

Apple vyžaduje, aby vývojáři zadejte odkaz na jejich webu pro podporu jakékoli dotazy, které uživatel může mít o aplikaci, nebo problém. Také vyžadují odkaz na zásady ochrany osobních údajů aplikace, (a která znovu, musí být hostované na vašem webu).

Volitelně můžete zadat odkaz na marketing informací hostovaných na vašem webu, které je možné poskytnout další informace o aplikaci, než je zadaná v úložišti aplikací.

### <a name="editing-name-description-whats-new-keywords-and-urls-in-itunes-connect"></a>Úpravy názvu, popisu, co je nového, klíčová slova a adresy URL v iTunes připojit

Proveďte [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Klikněte na **Moje aplikace**.
2. Klikněte na vaší aplikace **ikonu**.
3. Vyberte **verze** kartě.
4. Přejděte k položce **název** části.
5. Zadejte všechny požadované informace:

    [![](itunesconnect-images/name01.png "Úprava názvu, popisu, co je nového, klíčová slova a adresy URL v iTunes Connect")](itunesconnect-images/name01.png#lightbox)
6. Klikněte **Uložit** tlačítka v horní části obrazovky a uložit provedené změny.

> [!IMPORTANT]
> Poznámka: Apple odmítnou vaše odeslání, pokud název, popis, co je nového, klíčová slova nebo adresy URL neshodují funkci aktuální ve vaší aplikaci.

<a name="general" />

## <a name="maintaining-general-app-information"></a>Údržba aplikace obecné informace

Tato část iTunes připojit aplikace záznam poskytuje jedinečné ID aplikace (jak přiřazené společností Apple), kategorií, které patří aplikace, hodnocení, copyright a informace o společnosti, vydání aplikace.

### <a name="app-icon"></a>Ikona aplikace

> [!IMPORTANT]
>  **Poznámka:**: ikony aplikace jsou již odeslány prostřednictvím iTunes připojit. Musí být odeslána prostřednictvím **AppIcon** bitovou kopii sady ve vašem projektu **Assets.xcassets** souboru. Další informace najdete v tématu [App Store ikonu](~/ios/app-fundamentals/images-icons/app-store-icon.md) průvodce.

Ikona aplikace je písmo aplikace uživatelům, takže musí být snadno zapamatovatelný a zobrazení dobře na malou velikost. Čistá, jednoduchý a okamžitě rozpoznatelném jsou snadno zapamatovatelný ikony.

Apple navrhuje následující pokyny při navrhování vaší ikona aplikace:

- Ujistěte se, ikonu vhodné pro vaši aplikaci.
- Vytvoření jednoduchého ikonu, která je konzistentní s návrhem vaší aplikace.
- Nepoužívejte slova ve vašem ikonu.
- Vezměte v úvahu globálně: ikonou jedna aplikace se používá v všechny oblasti úložiště.

Obrázku 1 024 x 1 024 pixelů je vyžadována pro ikonu aplikace, která se zobrazí v úložišti aplikací.

Další informace najdete v tématu společnosti Apple [iOS Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html#//apple_ref/doc/uid/TP40006556) a popis v tématu velkých ikon aplikace [obecné informace o aplikaci](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Appendices/Properties.html#//apple_ref/doc/uid/TP40011225-CH26-SW7) dokumentace

### <a name="app-id"></a>ID aplikace

Toto je jedinečné identifikační číslo přiřazené k vaší aplikaci společností Apple při iTunes připojit záznamu. Toto číslo můžete použít při volání metody několik webových rozhraní této Apple poskytuje, včetně informací o obchodu s aplikacemi ve vašem webu.

### <a name="version-number"></a>Číslo verze

Toto je aktuální, aktivní verzi vaší aplikace, zobrazovat uživateli v úložišti aplikací.

### <a name="category-and-sub-category"></a>Kategorie a dílčí kategorie

Důležité aspekty možnosti rozpoznání pro vaši aplikaci je kategorii, kterou se zobrazuje v na webu App Store. Kategorie umožňují uživatelům procházet přes kolekci aplikací a najděte ty, které mají zájem. iTunes Connect umožňuje přiřadit až dvou různých kategorií při definování vaší aplikace. Ujistěte se, že pečlivě zvolte kategorie, které nejlépe popisují hlavní funkce vaší aplikace.

### <a name="ratings"></a>Hodnocení

Všechny aplikace musí mít hodnocení v úložišti aplikací. Tato hodnocení se používá k informovat rodičovský řízení a omezit přístup k podřízené objekty na základě typu a obsahu aplikace. Při definování aplikace, iTunes Connect obsahuje seznam popisů obsahu, pro které identifikujete jak často se zobrazí obsah ve vaší aplikaci. Tyto možnosti se převedou na hodnocení, který se zobrazí v úložišti aplikací.

Při vytváření aplikací pro děti, má obchodu s aplikacemi pro podřízené objekty stará 11 a v části zvláštní kategorie. I v případě, že vaše aplikace není specificky zaměřený na děti, můžete pomoct vašim zákazníkům dobrý rozhodování zadáním příslušné hodnocení obsahu.

> [!IMPORTANT]
> Poznámka: Apple odmítnou jakékoli poskytnuté příspěvky aplikace, která zjistí, obscénního, pornografické, urážlivé nebo POMLOUVAČNÉHO.

### <a name="copyright-and-company-information"></a>Informace o autorských právech a společnosti

Apple umožňují poskytovat informace o autorských právech pro vaši aplikaci a vyžaduje informace o společnosti, vydání aplikaci, například jeho adresy a kontaktních informací (které jsou nezbytné pro aplikace, které jsou vydané k obchodu s aplikacemi korejština). Tyto informace se zobrazí v úložišti aplikací podle potřeby.

### <a name="editing-general-app-information-in-itunes-connect"></a>Úpravy v iTunes Connect obecné informace o aplikaci.

Proveďte [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Klikněte na **Moje aplikace**.
2. Klikněte na vaší aplikace **ikonu**.
3. Vyberte **verze** kartě.
4. Přejděte k položce **obecné informace o aplikaci** části.
5. Zadejte všechny požadované informace:

    [![](itunesconnect-images/general01.png "Úpravy v iTunes Connect obecné informace o aplikaci.")](itunesconnect-images/general01.png#lightbox)
6. Klikněte na **upravit** tlačítko podle **hodnocení** nastavení informací o hodnocení:

    [![](itunesconnect-images/general02.png "Úpravy hodnocení")](itunesconnect-images/general02.png#lightbox)
6. Klikněte **Uložit** tlačítka v horní části obrazovky a uložit provedené změny.

> [!NOTE]
> Poznámka: Apple odmítnou vaše odeslání, pokud kategorie nebo hodnocení neshodují funkci aktuální ve vaší aplikaci.

<a name="game-center" />

## <a name="maintaining-game-center-information"></a>Uchovávání informací herní Centrum

Pro herní aplikace iOS, které podporují herní centrum společnosti Apple, můžete zadat informace, jako je vedoucí panely a herních bonusů ve hře, které jsou k dispozici pro uživatele a pokud je aplikace kompatibilní s více hráčů síťového připojení.

### <a name="editing-game-center-information-in-itunes-connect"></a>Úpravy herní Centrum informace v iTunes Connect

Proveďte [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Klikněte na **Moje aplikace**.
2. Klikněte na vaší aplikace **ikonu**.
3. Vyberte **verze** kartě.
4. Přejděte k položce **herní Centrum** části.
5. Překlopit přepínač podle **herní Centrum** části k **na** pozici.
5. Zadejte všechny požadované informace:

    [![](itunesconnect-images/gamecenter01.png "Úpravy herní Centrum informace v iTunes Connect")](itunesconnect-images/gamecenter01.png#lightbox)
6. Klikněte **Uložit** tlačítka v horní části obrazovky a uložit provedené změny.

Použití **herní Centrum** aktivovat herním centru a spravovat všechny dostupné **tabulky** nebo **herních bonusů** pro tuto aplikaci:

[![](itunesconnect-images/gamecenter02.png "Aktivovat herní Centrum")](itunesconnect-images/gamecenter02.png#lightbox)

[![](itunesconnect-images/gamecenter03.png "Spravovat všechny dostupné tabulky nebo herních bonusů pro tuto aplikaci")](itunesconnect-images/gamecenter03.png#lightbox)

## <a name="maintaining-app-review-information"></a>Údržba aplikace zkontrolujte informace

V této části použijte k poskytování požadované informace Apple pracovníky, kteří budou mít kontrola vaší aplikaci, například kontaktní informace (Pokud se na technickou má nějaké otázky), ukázku účty, které může být potřeba a případné poznámky, které mohou pomoci nástroje tester Zkontrolujte úspěšně vaší aplikace.

### <a name="editing-app-review-information-in-itunes-connect"></a>Úpravy aplikace zkontrolujte informace v iTunes Connect

Proveďte [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Klikněte na **Moje aplikace**.
2. Klikněte na vaší aplikace **ikonu**.
3. Vyberte **verze** kartě.
4. Přejděte k položce **aplikace zkontrolujte informace** části.
5. Zadejte všechny požadované informace:

    [![](itunesconnect-images/review01.png "Úpravy aplikace zkontrolujte informace v iTunes Connect")](itunesconnect-images/review01.png#lightbox)
6. Vyberte, jakým způsobem chcete aplikaci k uvolnění do App Storu po byly úspěšně zkontrolovány:

    [![](itunesconnect-images/review02.png "Úprava informací o verzi v iTunes Connect")](itunesconnect-images/review02.png#lightbox)
6. Klikněte **Uložit** tlačítka v horní části obrazovky a uložit provedené změny.


## <a name="maintaining-pricing-information"></a>Zachování informace o cenách

Pokud máte v plánu na vydání aplikace pro prodej, budete muset nastavit prodejní ceny výběrem jedné z dostupné cenové úrovně společnosti Apple a datum, danou cenovou vstoupí v platnost. Například od verze době psaní tohoto textu **vrstvy 1** ceny vypadá takto:

[![](itunesconnect-images/price01.png "Zachování informace o cenách")](itunesconnect-images/price01.png#lightbox)

### <a name="educational-discount"></a>Výukových slevu

Toto políčko zaškrtněte, pokud má vaše aplikace se nabízí se slevou pro vzdělávací instituce při zakoupení více kopií najednou. Podrobnosti o slevy, které se nacházejí v nejnovější **placené smlouvy aplikace**, které se musí zaregistrovat před této aplikace bude k dispozici pro education zákazníků.

### <a name="custom-business-to-business-application"></a>Vlastní obchodní pro obchodní aplikace

Aplikace, která je nastavený jako **vlastní obchodní pro obchodní aplikace** pouze bude k dispozici **Volume Purchase Program** zákazníků, které zadáte v iTunes připojit, a bude pouze k dispozici v příslušné teritoria (například USA Zákazníci Volume Purchase Program musí používat USA App Store programu Volume Purchase Program for Business).

Vlastní obchodní do obchodních aplikací není k dispozici pro vzdělávací instituce nebo obecné odběratele obchodu s aplikacemi. Další informace o *App Store Volume Purchase Program for Business*, navštivte společnosti Apple [– nejčastější dotazy](http://vpp.itunes.apple.com/faq) stránky. Další informace o tom, jak vaši zákazníci můžete zaregistrovat **Volume Purchase Program**, navštivte společnosti Apple [programy nasazení](http://enroll.vpp.itunes.apple.com) stránky.

### <a name="editing-pricing-information-in-itunes-connect"></a>Úpravy informace o cenách v iTunes Connect

Proveďte [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Klikněte na **Moje aplikace**.
2. Klikněte na vaší aplikace **ikonu**.
3. Vyberte **cenová** karty:

    [![](itunesconnect-images/price02.png "Úpravy informace o cenách v iTunes Connect")](itunesconnect-images/price02.png#lightbox)
4. Vyberte **datum dostupnosti**.
5. Vyberte požadovanou cenu, než **úroveň ceny** rozevíracího seznamu.
5. Volitelně můžete povolit **výukových slevy**.
6. Volitelně můžete definovat aplikace jako **vlastní obchodní pro obchodní aplikace**.
6. Klikněte **Uložit** tlačítko uložte provedené změny.

<a name="iap" />

## <a name="maintaining-in-app-purchase-information"></a>Uchovávání informací nákupy v aplikaci

Pokud plánujete prodávané virtuální, produkty z vaší aplikace (například nové herní úrovně nebo funkce aplikací) budete používat v této části vytvořit a udržovat ty zakoupit položky.

[![](itunesconnect-images/inapp01.png "Uchovávání informací nákupy v aplikaci")](itunesconnect-images/inapp01.png#lightbox)

Další informace o práci s nákupy v aplikaci v aplikaci Xamarin.iOS, najdete v tématu naše [nákupu v aplikaci](~/ios/platform/in-app-purchasing/index.md) dokumentaci.

## <a name="viewing-application-reviews"></a>Zobrazení recenze aplikace

Jakmile aplikace byla vydána k obchodu s aplikacemi, uživatelé, kteří zakoupit či stáhnout zdarma aplikaci můžete napsat recenze aplikace a nechte hodnocení hvězdičkami. Pomocí této části najdete v těchto recenze. Příklad:

[![](itunesconnect-images/reviews01.png "Zobrazení recenze aplikace")](itunesconnect-images/reviews01.png#lightbox)

## <a name="summary"></a>Souhrn

Tento článek popisuje, jak používat iTunes Connect Příprava aplikace pro Xamarin.iOS pro verzi k obchodu s aplikacemi a jak udržovat všechny informace o vaší aplikaci v úložišti.

## <a name="related-links"></a>Související odkazy

- [Práce s obrázky](~/ios/app-fundamentals/images-icons/index.md)
- [iOS aplikace vývoj pracovní postup průvodce: distribuce aplikace](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [Tipy pro odeslání obchodu s aplikacemi](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Pokyny pro recenze v App Storu](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [iTunes Connect – Příručka vývojáře](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/About.html#//apple_ref/doc/uid/TP40011225-CH1-SW1)
