---
title: Změny StoreKit v iOS 6
description: 'iOS 6 zavádí dvě změny do rozhraní API úložiště Kit: možnost zobrazit iTunes (a obchodu s aplikacemi nebo iBookstore) produkty z v rámci vaší aplikace a nové v aplikaci zakoupit možnost Apple kterých bude hostovat vaše soubory ke stažení. Tento dokument vysvětluje, jak implementovat tyto funkce s Xamarin.iOS.'
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: ff717d1e4ea7da947d5534f1ce790b58d84fdfd4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787689"
---
# <a name="changes-to-storekit-in-ios-6"></a>Změny StoreKit v iOS 6

_iOS 6 zavádí dvě změny do rozhraní API úložiště Kit: možnost zobrazit iTunes (a obchodu s aplikacemi nebo iBookstore) produkty z v rámci vaší aplikace a nové v aplikaci zakoupit možnost Apple kterých bude hostovat vaše soubory ke stažení. Tento dokument vysvětluje, jak implementovat tyto funkce s Xamarin.iOS._

Hlavní změny do úložiště Kit v iOS6 jsou tyto dvě nové funkce:

-   **Zobrazení obsahu v aplikaci & nákup** – můžete koupit uživatelů a stažení aplikace, Hudba, knihy a jiné iTunes obsahu, aniž byste museli opustit vaší aplikace. Můžete také propojit své vlastní aplikace podporovat, nákup nebo právě Doporučte recenze a hodnocení. 
-   **Obsah nákupu hostované v aplikaci** – bude ukládat a doručovat obsah přidružený k vaší produkty nákupy v aplikaci, která eliminuje nutnost pro samostatný server k hostování souborů, automaticky podporuje pozadí stahování a umožní Apple zápis méně kódu. 


Doporučuje se, že tento dokument číst ve spojení s existující Xamarin.iOS [nákupy v aplikaci](~/ios/platform/in-app-purchasing/index.md) dokumentaci.

## <a name="requirements"></a>Požadavky

Funkce úložiště Kit popisovaný v tomto dokumentu vyžadují iOS 6 a 4.5 Xcode, společně s Xamarin.iOS 6.0.


## <a name="in-app-content-display--purchasing"></a>Zobrazení obsahu v aplikaci & nákupu

Nová funkce v aplikaci nákupu v iOS umožňuje uživatelům zobrazit informace o produktu a zakoupit či stáhnout z produktu v rámci vaší aplikace.
Dříve aplikace by měla mít k aktivaci iTunes, obchodu s aplikacemi nebo iBookstore, což by způsobilo uživatele je původní aplikaci. Tato nová funkce automaticky vrátí uživatele do vaší aplikace, pokud se provádí.

 [![](changes-to-storekit-images/image1.png "Pokud chcete u aplikace automaticky vrácení po nákupu")](changes-to-storekit-images/image1.png#lightbox)

Existuje několik scénářů, kde může být užitečná, včetně (ale bez omezení):

-   **Podporu uživatelům hodnocení aplikace** – můžete otevřít stránku obchodu s aplikacemi, tak, aby uživatel můžete hodnocení a zkontrolovat vaše aplikace bez zároveň je nechává. 
-   **Povýšení mezi aplikací** – povolit uživatelům zobrazit další aplikace, které publikujete, možnost koupit nebo stažení okamžitě. 
-   **Pomoc uživatelům najít a stažení obsahu** – pomáhají uživatelům koupit obsah, který vaše aplikace vyhledá, spravuje nebo agreguje (např. aplikace související Hudba může poskytují seznam skladeb skladeb a povolit skladeb Nákup z aplikace). 


Jednou `SKStoreProductViewController` je zobrazené uživatele mohou komunikovat s informace o produktu, jako kdyby byly v iTunes, obchodu s aplikacemi nebo iBookstore. Sem patří:

-  Zobrazení snímky obrazovky (pro aplikace)
-  Vzorkování skladeb nebo video (pro Hudba, televizní pořady a filmy)
-  Zkontroluje čtení (a zápis),
-  Zakoupení & stahování, který se stane úplně v rámci řadiče zobrazení a úložiště Kit. Aplikace není nutné poskytovat žádné další kód pro tento postup. 


Mějte na paměti některé možnosti v rámci `SKStoreProductViewController` bude stále force uživateli nechte aplikaci a otevřete aplikaci příslušných obchodů, jako je například kliknutí na **související produkty** nebo aplikace **podporu** odkaz.

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

Rozhraní API zobrazíte produktu v jakékoli aplikaci, je velmi jednoduchý: vyžaduje pouze vytvořit a zobrazit `SKStoreProductViewController`. Postupujte podle těchto kroků k vytvoření a zobrazit produktu:

1.  Vytvoření `StoreProductParameters` objekt, který chcete předat parametry řadiče zobrazení, včetně `productId` v konstruktoru. 
1.  Vytvoření instance `SKProductViewController` . Přiřaďte ho na úrovni pole třídy. 
1.  Obslužná rutina přiřadit řadiče zobrazení `Finished` událost, která by měla zavřít řadiče zobrazení. Tato událost je volána, když uživatel stiskne zrušit; nebo v opačném případě dokončí transakce uvnitř řadiče zobrazení. 
1.  Volání `LoadProduct` metoda předávání v `StoreProductParameters` a dokončení obslužná rutina. Obslužná rutina dokončení zkontrolujte produktu žádost byla úspěšně a pokud ano, k dispozici `SKProductViewController` modálně. V případě, že produkt nelze načíst, musí být přidaní zpracování příslušné chyb. 

### <a name="example"></a>Příklad

*ProductView* v projektu *StoreKit* implementuje ukázkový kód pro tento článek `Buy` metoda, která přijme některý z produktů je Apple ID a zobrazí `SKStoreProductViewController`. Následující kód zobrazí informace o produktu pro jakékoli dané Apple ID:

```csharp
void Buy (int productId)
{
    var spp = new StoreProductParameters(productId);
    var productViewController = new SKStoreProductViewController ();
    // must set the Finished handler before displaying the view controller
    productViewController.Finished += (sender, err) => {
        // Apple's docs says to use this method to close the view controller
        this.DismissModalViewControllerAnimated (true);
    };
    productViewController.LoadProduct (spp, (ok, err) => { // ASYNC !!!
        if (ok) {
            PresentModalViewController (productViewController, true);
        } else {
            Console.WriteLine (" failed ");
            if (err != null)
                Console.WriteLine (" with error " + err);
        }
    });
}
```

Aplikace bude mít tento tvar při spuštění – stažení nebo zakoupení dojde k zcela uvnitř `SKStoreProductViewController`:

 [![](changes-to-storekit-images/image2.png "Při spuštění bude mít tento tvar aplikace")](changes-to-storekit-images/image2.png#lightbox)

### <a name="supporting-older-operating-systems"></a>Podpora starších operačních systémů

Ukázková aplikace obsahuje kód, který ukazuje, jak otevřít obchodu s aplikacemi, iTunes a iBookstore v dřívějších verzích systému iOS. Použití `OpenUrl` metoda otevřete správně vytvořený **itunes.com** adresy URL.

Můžete implementovat Kontrola verze a zjišťování, které kód pro spuštění, například takto:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (6,0)) {
    // do iOS6+ stuff, using SKStoreProductViewController as shown above
} else {
    // don't do stuff requiring iOS 6.0, use the old syntax 
    // (which will take the user out of your app)
    var nsurl = new NSUrl("http://itunes.apple.com/us/app/angry-birds/id343200656?mt=8");
    UIApplication.SharedApplication.OpenUrl (nsurl);
}
```

### <a name="errors"></a>Chyby

Pokud Apple ID, můžete použít není platný, protože může být matoucí měla znamená problém sítě nebo ověřování nějaká dojde k následující chybě.

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>Čtení jazyka Objective-C dokumentace

Vývojáři čtení o úložiště Kit na portál pro vývojáře Apple uvidí protokol – [SKStoreProductViewControllerDelegate](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html) – uvedené ve vztahu k této nové funkce. Protokol delegáta má pouze jednu metodu – productViewControllerDidFinish –, která vystavila jako `Finished` událostí na `SKStoreProductViewController` v Xamarin.iOS.


## <a name="determining-apple-ids"></a>Určení Apple ID

Apple ID, které vyžadují `SKStoreProductViewController` je *číslo* (si ji plést s ID sady prostředků jako "com.xamarin.mwc2012"). Existují několika různými způsoby, kterými můžete zjistit, Apple ID pro produkty, které chcete zobrazit, uvedené níže:

### <a name="itunesconnect"></a>iTunesConnect

Pro aplikace, které publikujete, se dají snadno najít **Apple ID** v iTunes připojení:

 [![](changes-to-storekit-images/image3.png "Hledání Apple ID v iTunes Connect")](changes-to-storekit-images/image3.png#lightbox)

 <a name="Search_API" />


### <a name="search-api"></a>Rozhraní API pro hledání

Apple poskytuje dynamické vyhledávání rozhraní API pro dotaz na všechny produkty v App Storu, iTunes a iBookstore. Informace o tom, jak získat přístup k rozhraní API pro vyhledávání naleznete v [přidružili prostředky společnosti Apple](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html), i když rozhraní API je vystaven na nikoho (není právě zaregistrovaná pobočky). Výsledný formát JSON lze analyzovat ke zjišťování `trackId` tedy Apple ID pro použití s `SKStoreProductViewController`.

Výsledky bude také obsahovat další metadata, včetně zobrazované informace a kresby adres URL, které lze použít k vykreslení produktu ve vaší aplikaci.

Následuje několik příkladů:

-   **aplikace iBooks*- [http://itunes.apple.com/search?term=ibooks&amp; entity = softwaru&amp;země = us](http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us) 
-   **Dot a ibooks, který má Kangaroo*- [http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp; entity = elektronická kniha&amp;země = us](http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us) 


### <a name="enterprise-partner-feed"></a>Organizace partnera kanálu

Apple poskytuje schválené partnery s kompletní datový výpis všech svých produktů ve formuláři ploché soubory ke stažení databáze připravené. Pokud máte nárok pro přístup k [Enterprise partnera kanálu](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-enterprise-partner-feed.html) pak Apple ID pro některý z produktů najdete v této datové sadě.

Upozorňujeme, že mnoho uživatelů z informačního kanálu Enterprise partnera jsou členy [přidružili Program](http://www.apple.com/itunes/affiliates) umožňuje Komise k být vytvořené v produktu prodej. `SKStoreProductViewController` nepodporuje přidružili ID (v době psaní; to může být doplněn Apple v budoucnosti).

### <a name="direct-product-links"></a>Přímé připojení produktu

Apple ID produktu lze odvodit z jeho iTunes Preview adresu URL.
V žádné iTunes odkaz produktu (pro aplikace, Hudba nebo knihy) vyhledejte část adresy URL začínající `id` a použití čísla, která následuje.

Je třeba přímý odkaz na iBooks

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

a Apple ID je **364709193**. Podobně jako u aplikace MWC2012 je přímý odkaz

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

a Apple ID je **496963922**.

## <a name="in-app-purchase-hosted-content"></a>Nákupy v aplikaci hostitelem obsahu

Pokud vaše nákupy v aplikaci se skládají ke stažení obsahu (například knihy nebo jiné médium, herní úrovně obrázky a konfigurace nebo další velké soubory) pak použít tyto soubory na vašem webovém serveru a aplikace musí obsahovat kód, který je po bezpečně stáhnout Zakupte. V systému iOS má 6 Společnost Apple vydala možnost kterých bude hostovat vaše soubory na jejich serverech odebrání potřeba samostatný server. Tato funkce je jen pro produkty Non-Consumable (ne Consumable nebo předplatných) k dispozici. Výhody použití služby hostování společnosti Apple:

-  Uložte náklady na hostování & šířky pásma.
-  Pravděpodobně lepší škálovatelnost než ať hostitele serveru, které právě používáte. 
-  Méně kódu zapsat, protože nemusíte vytvářet žádné zpracování na straně serveru. 
-  Stahování pozadí je pro vás implementováno.


Poznámka: testování hostované obsah nákupy v aplikaci v simulátoru není podporován, iOS, takže musíte otestovat s skutečné zařízení.

### <a name="hosted-content-basics"></a>Základy hostované obsahu

Před iOS 6, nebyly k dispozici produkt dvě možnosti (Další podrobně popsané v [nákupy v aplikaci Xamarin pro](~/ios/platform/in-app-purchasing/index.md) dokumentace):

-   **Předdefinované produkty** – funkce, které jsou 'odemknout' pomocí nákupu, ale které jsou integrované do aplikace (buď jako kód nebo vložené prostředky). Příklady předdefinovaných produkty: odemknout fotografií filtry nebo ve hře power-ups. 
-   **Server poskytovaná produkty** – po nákupu, musí aplikaci stáhnout obsah ze serveru, který můžete pracovat. Tento obsah je při nákupu stažena, uložených v zařízení a pak se vykresluje jako součást poskytování produktu. Mezi příklady patří knihy, katalogu problémy nebo herní úrovně, které se skládají z pozadí obrázky a konfiguračních souborů. 


V iOS 6 Apple nabízí varianta doručit serveru produkty: jejich bude hostitelem souborů obsahu na jejich serverech. Díky tomu je mnohem jednodušší vytvořit produkty serveru doručit, protože není nutné provozovat samostatný server, a Kit úložiště poskytuje stahování pozadí funkce, které jste dříve museli psát sami. Abyste mohli využívat hostování společnosti Apple, povolit hostování obsahu pro nové produkty nákupy v aplikaci a upravovat kód Kit úložiště a využívat jejich výhod. Soubory obsahu v produktu jsou pak vyvíjené v Xcode a nahrané do servery společnosti Apple ke kontrole a verzi.

 [![](changes-to-storekit-images/image4.png "Proces sestavení a doručit")](changes-to-storekit-images/image4.png#lightbox)

Použití obchodu s aplikacemi pro poskytnutí nákupu v aplikaci *s hostitelem obsahu* vyžaduje následující nastavení a konfiguraci:

-   **iTunes připojit** – můžete *musí* poskytovaly vaše informace bankovnictví a daň společnosti Apple, takže se můžete uhradit fondů shromážděných vaším jménem. Následně můžete konfigurovat produkty prodeje, a nastavte izolovaný prostor uživatelské účty k testování nákupu.  *Musíte také nakonfigurovat hostované obsahu* *pro tyto produkty nespotřebitelné, které chcete do hostitele s Apple* *.* 
-   **iOS Provisioning Portal** – vytvoření identifikátor svazku a povolení přístup k obchodu s aplikacemi pro vaši aplikaci, jako byste pro každou aplikaci, která podporuje nákupu v aplikaci. 
-   **Uložení Kit** – přidání kódu do vaší aplikace pro zobrazení produktů, nákup produktů a obnovení transakce.  *V iOS 6 Kit úložiště bude také spravovat, stahování obsahu produktu, na pozadí probíhá aktualizace.* 
-   **Vlastní kód** – ke sledování nákupy provedené zákazníci a poskytnout produkty nebo služby zakoupili. Využívat nové třídy úložiště Kit iOS 6 jako `SKDownload` k získání obsahu hostovaná společností Apple. 


Následující části popisují, jak implementovat hostované obsah z vytváří a odesílá balíček pro správu nákupu a stáhnout proces, pomocí ukázkový kód pro tento článek.


### <a name="sample-code"></a>Ukázkový kód

Ukázkový projekt *HostedNonConsumables* (v StoreKitiOS6.zip) ukazuje, jak sestavit aplikaci používá hostované obsah. Aplikace nabízí dva "kniha kapitolám" pro prodej, obsahu, pro který je hostitelem servery společnosti Apple. Obsah se skládá z textového souboru a bitovou kopii, i když může mnohem složitější obsah v reálné aplikaci.

Aplikace se před, během a po nákupu vypadat třeba takto:

 [![](changes-to-storekit-images/image5.png "Aplikace bude mít tento tvar před, během a po nákupu")](changes-to-storekit-images/image5.png#lightbox)

Textový soubor a bitové kopie stáhnou a zkopírují do adresáře dokumentů aplikace. Najdete v článku [práce v dokumentaci systému souborů](~/ios/app-fundamentals/file-system.md) Další informace o různých adresáře, které jsou k dispozici pro úložiště aplikací.

## <a name="itunes-connect"></a>iTunes Connect

Při vytváření nových produktů, které budou používat Apple je obsahu je nutné vybrat hostování **Non-Consumable** typ produktu. Jiné typy produktu nepodporují hostování obsahu. Kromě toho by neměla být povolena, hostování obsahu pro *existující* produkty, které prodeje; pouze zapnout hostování obsahu pro nové produkty.

 [![](changes-to-storekit-images/image6.png "Vyberte typ Non-Consumable produktu")](changes-to-storekit-images/image6.png#lightbox)

Zadejte **ID produktu**. To bude požadován později při vytváření obsahu pro tento produkt.

 [![](changes-to-storekit-images/image7.png "Zadejte ID produktu")](changes-to-storekit-images/image7.png#lightbox)

Hostování obsahu se nastavuje v části Podrobnosti. Před nákupy v aplikaci, budete za provozu jednoduše zrušte zaškrtnutí políčka "Hostitel obsahu s Apple" Pokud chcete zrušit (i když jste nahráli nějaký zkušební obsah). Ale obsahu hostování nelze odebrat po nákupy v aplikaci přešel za provozu.

 [![](changes-to-storekit-images/image8.png "Hostování obsahu s Apple")](changes-to-storekit-images/image8.png#lightbox)

Jakmile jste zapnuli hostování obsahu, bude produkt zadejte **čekání nahrávání** stav a zobrazit tato zpráva:

 [![](changes-to-storekit-images/image9.png "Zadáte čekání pro stav nahrávání a zobrazit tato zpráva produktu")](changes-to-storekit-images/image9.png#lightbox)

Obsah, musí být vytvořeny teď s Xcode a načten pomocí nástroje archivu. Pokyny pro vytváření obsahu balíčků je uveden v další části **vytváření. Soubory PKG**.

## <a name="creating-pkg-files"></a>Vytváření. PKG soubory

Soubory obsahu, které zasílat společnosti Apple, musí splňovat následující omezení:

-  Velikost nesmí překročit 2GB.
-  Nemůže obsahovat spustitelného kódu (nebo symbolických odkazů, které bod mimo obsah). 
-  Musí být ve správném formátu (včetně soubor s příponou .plist) a mají příponu .pkg. To bude provedeno automaticky Pokud budete postupovat podle těchto pokynů pomocí Xcode. 


Mnoho různých souborů a typy souborů, můžete přidat, dokud nebudou splňovat tato omezení. Obsah je metoda ZIP před doručením do vaší aplikace a rozbalené pomocí úložiště Kit před kódu přistupuje.

Po odeslání obsahu balíčku, lze nahradit s novější obsahem. Nový obsah musí být nahrán a pro revize nebo schválení prostřednictvím běžný proces. Musíte zvýšit `ContentVersion` pole aktualizované balíčcích obsahu.

### <a name="xcode-in-app-purchase-content-projects"></a>Projekty Xcode v aplikaci nákupu obsahu

Vytváření balíčků obsahu pro produkty nákupy v aplikaci aktuálně vyžaduje Xcode. Neexistuje žádné jazyka OBJECTIVE-C kódování požadované; Xcode je nový typ projektu pro tyto balíčky obsahující pouze vaše soubory a plist.

Naše ukázková aplikace má kapitolám adresáře pro prodej – každý balíček obsahu kapitoly bude obsahovat:

-  textový soubor, a
-  obrázek, který má představovat kapitoly.


Začněte výběrem **soubor > Nový projekt** v nabídce a výběr **v aplikaci nákupu obsahu**:

 [![](changes-to-storekit-images/image10.png "Vybrat obsah nákupy v aplikaci")](changes-to-storekit-images/image10.png#lightbox)

Zadejte **název produktu** a **identifikátor společnosti** tak, aby **identifikátor svazku** odpovídá **ID produktu** jste zadali v iTunes Připojení pro tento produkt.

 [![](changes-to-storekit-images/image11.png "Zadejte název a identifikátor")](changes-to-storekit-images/image11.png#lightbox)

Teď máte prázdný **v aplikaci nákupu obsahu** projektu. Můžete kliknout pravým tlačítkem a **přidat soubory...** nebo je přetáhněte do **navigátoru projektů**. Ujistěte se, že **ContentVersion** správné (měli začínají 1.0, ale pokud se později rozhodnete aktualizovat obsah, nezapomeňte se zvýší).

Tento snímek obrazovky ukazuje Xcode s soubory obsahu, který je součástí projektu a plist položky viditelné v hlavním okně:

 [![](changes-to-storekit-images/image12.png "Tento snímek obrazovky ukazuje Xcode s soubory obsahu, který je součástí projektu a plist položky viditelné v hlavním okně")](changes-to-storekit-images/image12.png#lightbox)

Po přidání všech souborů obsahu můžete uložit tento projekt a později ji upravit nebo zahájit proces odesílání.

## <a name="uploading-pkg-files"></a>Odesílání. PKG soubory

Nejjednodušší způsob, jak nahrát balíčcích obsahu je **Xcode archivu nástroj**. Zvolte **produktu > archivu** z nabídky zahájíte:

 ![](changes-to-storekit-images/image13.png "Zvolte Archiven")

Balíček obsahu se potom zobrazí v archivu, jak je uvedeno níže. Ikona a typ archivu zobrazit toto je upozornění **archivu obsahu v aplikaci nákupu**. Klikněte na tlačítko **ověření...** ke kontrole naše obsahu balíčku chyby bez ve skutečnosti preforming nahrávání.

 [![](changes-to-storekit-images/image14.png "Ověření balíčku")](changes-to-storekit-images/image14.png#lightbox)

Přihlaste se s vaší iTunes Connect přihlašovací údaje:

 [![](changes-to-storekit-images/image15.png "Přihlaste se s vaší iTunes přihlašovací údaje pro připojení")](changes-to-storekit-images/image15.png#lightbox)

Vyberte správné aplikací a nákupy v aplikaci Přidružte tento obsah se:

 [![](changes-to-storekit-images/image16.png "Vyberte správný aplikací a nákupy v aplikaci Přidružte tento obsah s")](changes-to-storekit-images/image16.png#lightbox)

Zobrazí se zpráva podobná následující:

 ![](changes-to-storekit-images/image17.png "Příklad žádná zpráva problémy")

Nyní přejděte prostřednictvím podobným způsobem, ale kliknutím na tlačítko **distribuci...** Tento obsah se ve skutečnosti nahrát.

 [![](changes-to-storekit-images/image18.png "Distribuce aplikace")](changes-to-storekit-images/image18.png#lightbox)

Vyberte první možnost, chcete-li tento obsah nahrát:

 ![](changes-to-storekit-images/image19.png "Tento obsah nahrát")

Přihlásit znovu:

 [![](changes-to-storekit-images/image15.png "Přihlášení")](changes-to-storekit-images/image15.png#lightbox)

Zvolte správné aplikací a nákupy v aplikaci záznam nahrát obsah do:

 [![](changes-to-storekit-images/image20.png "Zvolit záznam nákupu aplikace a v aplikaci")](changes-to-storekit-images/image20.png#lightbox)

Počkejte, Probíhá nahrávání souborů:

 [![](changes-to-storekit-images/image21.png "Dialogové okno nahrávání obsahu")](changes-to-storekit-images/image21.png#lightbox)

Po dokončení nahrávání se zobrazí zpráva poradit, že obsah byla odeslána na obchod s aplikacemi.

 [![](changes-to-storekit-images/image22.png "Příkladu úspěšné odeslání zprávy")](changes-to-storekit-images/image22.png#lightbox)

Po který bylo provedeno, když se vrátíte na stránku produktu na iTunes Connect bude zobrazit podrobnosti o balíčcích a být ve **připravené k odeslání** stavu. Pokud produkt je v tomto stavu, můžete zahájit testování v prostředí izolovaného prostoru. NENÍ nutné odeslat produktu pro testování v izolovaném prostoru.

 [![](changes-to-storekit-images/image23.png "iTunes Connect bude zobrazit podrobnosti o balíčcích a být ve připravené k odeslání stavu")](changes-to-storekit-images/image23.png#lightbox)

Může trvat delší dobu (např. několik minut) mezi odesílání archiv a iTunes se aktualizovat stav připojení. Můžete odeslat produktu ke kontrole samostatně, nebo odešlete ji ve spojení s binární soubory aplikace. Až po Apple oficiálně schválena obsahu se bude k dispozici v produkčním prostředí App Store zakoupit ve vaší aplikaci.

### <a name="pkg-file-format"></a>Formát souboru PKG

K vytvoření a nahrání hostované obsahu balíčku pomocí Xcode a nástroj v archivu znamená nikdy zobrazit obsah balení, sám sebe. Soubory a adresáře v balíčcích pro ukázkovou aplikaci vytvořit vypadat například takto, s `plist` v kořenu a soubory produktu v souboru `Contents` podadresáři:

 [![](changes-to-storekit-images/image24.png "Soubor plist v kořenu a soubory produktu v podadresáři obsah")](changes-to-storekit-images/image24.png#lightbox)

Všimněte si strukturu adresáře balíčku (zejména umístění souborů `Contents` podadresáři) vzhledem k tomu, že je potřeba pochopit, tyto informace k extrahujte soubory z balíčku na zařízení.

### <a name="updating-package-content"></a>Aktualizace obsahu balíčku

Postup aktualizace obsahu po schválení:

-  Upravte v aplikaci nákupu obsahu projektu v Xcode.
-  Zvýšit číslo verze.
-  Znovu nahrajte soubory do iTunes připojit. Následné kupujících bude automaticky získat nejnovější verzi, ale uživatelé, kteří již mají stará verze neobdržíte žádné oznámení. 
-  Aplikace je zodpovědná za upozornění uživatelů a podporou načtou novější verzi sady obsahu. Aplikace také musí vytvořit funkci, která soubory ke stažení na novou verzi. Musí to být pomocí funkce obnovení Kit úložiště. 
-  Chcete-li zjistit, pokud existuje novější verze je můžete vytvořit funkce do vaší aplikace načíst SKProducts (např. stejný postup, který se používá k načtení cen produktu) a porovnání vlastnost ContentVersion. 

## <a name="purchasing-overview"></a>Nákup – přehled

Před čtením této části, zkontrolujte stávající [nákupy v aplikaci dokumentaci](~/ios/platform/in-app-purchasing/index.md).

Posloupnost událostí, který nastane, když produkt hostované obsah je zakoupených a stažení je znázorněna v tomto diagramu:

 [![](changes-to-storekit-images/image25.png "Posloupnost událostí, který nastane, když produkt hostované obsah je zakoupených a stáhnout")](changes-to-storekit-images/image25.png#lightbox)

1.  Nové produkty lze vytvořit v iTunes připojit se s hostované obsahu povoleno. Skutečný obsah je v Xcode samostatně zkonstruovat (jako jednoduše přetahování soubory do složky) a potom archivovat a odeslat do iTunes (není vyžadováno žádné kódování). Každý produkt je potom odeslán na schválení, po jejímž uplynutí je k dispozici pro nákup. V ukázkovém kódu jsou tyto kódy Product ID pevně zakódované, ale hostování obsahu s Apple je flexibilnější, pokud uchováváte seznam dostupných produktů na vzdáleném serveru, aby při odesílání novými produkty a obsah tak, aby iTunes připojení lze aktualizovat. 
1.  Uživatel zakoupí produkt, transakce je umístěn v platebních fronty ke zpracování. 
1.  Úložiště Kit předá požadavek nákupu iTunes serverů pro zpracování. 
1.  Transakce je dokončena na serverech iTunes (např. je účtován zákazníka) a potvrzení se vrátí do aplikace, informace o produktu připojené včetně jestli je ke stažení (a pokud ano, velikost souboru a další metadata). 
1.  Váš kód by měl zkontrolujte, zda je produkt downloadble a pokud Ano proveďte stahování obsahu požadavku, který je také umístěna ve frontě platby. Úložiště Kit odešle tento požadavek na serverech iTunes. 
1.  Server vrátí soubor obsahu Kit úložiště, který poskytuje zpětné volání vrátí průběh stahování a zbývající odhady kódu čas. 
1.  Po dokončení získat upozornění a předán umístění souboru ve složce mezipaměti. 
1.  Váš kód by měl zkopírovat soubory a ověřte, uložte nějaký stav, který je třeba pamatovat, že produkt byl zakoupen. Využít této příležitosti a na nové soubory správně nastavené příznak zálohování (nápovědu: Pokud budou pocházet ze serveru a uživatel se nikdy upravit, pravděpodobně přeskočte zálohování, protože uživatel vždy je znovu načíst ze servery společnosti Apple v budoucnosti). 
1.  Volání FinishTransaction. To je důležité, protože ji z fronty platebních odebere transakce. Je také důležité, aby Nevolejte FinishTransaction až po zkopírování obsahu mimo adresář mezipaměti. Jakmile zavoláte FinishTransaction, budou pravděpodobně rychle vyprázdnit mezipaměť souborů. 


## <a name="implementing-hosted-content-purchase"></a>Implementace hostované obsahu nákupu

Následující informace obsaženou ve spojení s kompletní [nákupy v aplikaci dokumentaci](~/ios/platform/in-app-purchasing/index.md). Informace v tomto dokumentu se zaměřuje na rozdíly mezi hostované obsah a předchozí implementace.


### <a name="classes"></a>Třídy

Následující třídy byly přidány nebo změnit na obsahu podpory hostované v iOS 6:

-   **SKDownload** – novou třídu, která reprezentuje probíhající stahování. Rozhraní API umožňuje více než jeden za produktu, ale původně jenom jeden byl implementován. 
-   **SKProduct** – přidání nových vlastností: `Downloadable` , `ContentVersion` , `ContentLengths` pole. 
-   **SKPaymentTransaction** – přidat nové vlastnosti: `Downloads` , který obsahuje kolekci `SKDownload` objekty, pokud tento produkt má atribut hosted obsah k dispozici ke stažení. 
-   **SKPaymentQueue** – nová metoda přidán: `StartDownloads` . Volat tuto metodu s `SKDownload` objekty, které chcete načíst hostované obsah. Stahování se může objevit v pozadí. 
-   **SKPaymentTransactionObserver** – nová metoda: `UpdateDownloads` . Úložiště Kit volá tuto metodu o průběhu aktuální operace stažení. 


Podrobnosti o nové `SKDownload` třídy:

-   **Průběh** – hodnotu mezi 0-1, který můžete použít k zobrazení na procento dokončení ukazatel uživateli. Nepoužívejte průběh == 1 zjistí, zda je k dokončení stahování, zkontrolujte stav == dokončeno. 
-   **TimeRemaining** – odhad stažení zbývající dobu, v sekundách. -1 znamená, že se pořád je výpočet odhad. 
-   **Stav** – aktivní, čekání na dokončení, došlo k chybě, pozastavena, zrušena. 
-   **ContentURL** – umístění, kde obsah byla pozastavena na disku, v souboru `Cache` adresáře. Naplní pouze po dokončení stahování. 
-   **Chyba** – zaškrtněte tuto vlastnost, pokud stavu se nezdařilo. 


Interakce mezi třídami v ukázkovém kódu jsou uvedeny v tomto diagramu (kód specifické pro hostované obsahu nákupy je zobrazené zeleně):

 [![](changes-to-storekit-images/image26.png "Hostované obsahu nákupy je zobrazené zeleně do tohoto diagramu")](changes-to-storekit-images/image26.png#lightbox)

Ukázkový kód, kde byly použity tyto třídy je zobrazena ve zbývající části této části:

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>CustomPaymentObserver (SKPaymentTransactionObserver)

Změnit existující `UpdatedTransactions` přepsání kontroluje ke stažení obsahu a volání `StartDownloads` v případě potřeby:

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
    foreach (SKPaymentTransaction transaction in transactions) {
        switch (transaction.TransactionState) {
        case SKPaymentTransactionState.Purchased:
            // UPDATED FOR iOS 6
            if (transaction.Downloads != null && transaction.Downloads.Length > 0) {
                // Purchase complete, and it has downloads... so download them!
                SKPaymentQueue.DefaultQueue.StartDownloads (transaction.Downloads);
                // CompleteTransaction() call has moved after downloads complete
            } else {
                // complete the transaction now
                theManager.CompleteTransaction(transaction);
            }
            break;
        case SKPaymentTransactionState.Failed:
            theManager.FailedTransaction(transaction);
            break;
        case SKPaymentTransactionState.Restored:
            // TODO: you must decide how to handle restored transactions.
            // Triggering all the downloads at once is not advisable.
            theManager.RestoreTransaction(transaction);
            break;
        default:
            break;
        }
    }
}
```

Nová přepsaného metoda `UpdatedDownloads` jsou uvedeny níže. Úložiště Kit volá tuto metodu po `StartDownloads` se spustí v `UpdatedTransactions`. Tato metoda je volána *několikrát* v neurčitém intervalech poskytnout vám průběh stahování a pak opakujte po dokončení stahování. Všimněte si metoda přijímá pole `SKDownload` objekty, aby každý volání metody můžete poskytovat stav více souborů ke stažení ve frontě. Jak je znázorněno v níže implementace stavy stažení jsou kontrolovat každý čas a příslušné akce.

```csharp
// ENTIRELY NEW METHOD IN iOS6
public override void PaymentQueueUpdatedDownloads (SKPaymentQueue queue, SKDownload[] downloads)
{
    Console.WriteLine (" -- PaymentQueueUpdatedDownloads");
    foreach (SKDownload download in downloads) {
        switch (download.DownloadState) {
        case SKDownloadState.Active:
            // TODO: implement a notification to the UI (progress bar or something?)
            Console.WriteLine ("Download progress:" + download.Progress);
            Console.WriteLine ("Time remaining:   " + download.TimeRemaining); // -1 means 'still calculating'
            break;
        case SKDownloadState.Finished:
            Console.WriteLine ("Finished!!!!");
            Console.WriteLine ("Content URL:" + download.ContentUrl);

            // UNPACK HERE! Calls FinishTransaction when it's done
            theManager.SaveDownload (download);

            break;
        case SKDownloadState.Failed:
            Console.WriteLine ("Failed"); // TODO: UI?
            break;
        case SKDownloadState.Cancelled:
            Console.WriteLine ("Cancelled"); // TODO: UI?
            break;
        case SKDownloadState.Paused:
        case SKDownloadState.Waiting:
            break;
        default:
            break;
        }
    }
}
```

### <a name="inapppurchasemanager-skproductsrequestdelegate"></a>InAppPurchaseManager (SKProductsRequestDelegate)

Tato třída obsahuje nové metody `SaveDownload` která je volána po úspěšném dokončení každého stažení.

Hostované obsah byl úspěšně stažen a rozbalené do `Cache` adresáře. Struktura. PKG soubor vyžaduje, aby ho uložit ve všechny soubory `Contents` podadresáři, tak následující kód extrahuje soubory ze uvnitř `Contents` podadresáři.

Kód prochází všechny soubory v balíčku obsahu a zkopíruje je do `Documents` v podsložce s názvem pro adresář `ProductIdentifier`. Nakonec volá `CompleteTransaction`, který volá `FinishTransaction` odebrat z fronty platební transakce.

```csharp
// ENTIRELY NEW METHOD IN iOS 6
public void SaveDownload (SKDownload download)
{
    var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
    var targetfolder = System.IO.Path.Combine (documentsPath, download.Transaction.Payment.ProductIdentifier);
    // targetfolder will be "/Documents/com.xamarin.storekitdoc.montouchimages/" or something like that
    if (!System.IO.Directory.Exists (targetfolder))
        System.IO.Directory.CreateDirectory (targetfolder);
    foreach (var file in System.IO.Directory.EnumerateFiles 
             (System.IO.Path.Combine(download.ContentUrl.Path, "Contents"))) { // Contents directory is the default in .PKG files
        var fileName = file.Substring (file.LastIndexOf ("/") + 1);
        var newFilePath = System.IO.Path.Combine(targetfolder, fileName);
        if (!System.IO.File.Exists(newFilePath)) // HACK: this won't support new versions...
            System.IO.File.Copy (file, newFilePath);
        else
            Console.WriteLine ("already exists " + newFilePath);
    }
    CompleteTransaction (download.Transaction); // so it gets 'finished'
}
```

Když `FinishTransaction` je volána stažené soubory jsou již musí být v `Cache` adresáře. Všechny soubory by se měl zkopírovat před voláním `FinishTransaction`.


## <a name="other-considerations"></a>Ostatní úvahy

Výše uvedený příklad kódu ukazuje na docela Jednoduchá implementace hostované obsahu nákupu. Existují některé další body, které musíte zvážit:

### <a name="detecting-updated-content"></a>Zjišťování aktualizovaný obsah

I když je možné aktualizovat vaše hostované balíčky obsahu, úložiště Kit neposkytuje žádný mechanismus push tyto aktualizace uživatelům, kteří už stáhli a zakoupených produktu. K implementaci tato funkce může zkontrolujte kód nové `SKProduct.ContentVersion` vlastnost (Pokud `SKProduct` je `Downloadable`) pravidelně a zjistit, pokud se zvýší hodnota. Případně může vytvořit systému nabízených oznámení.

### <a name="installing-updated-content-versions"></a>Instalace aktualizované verze obsahu

Výše uvedeném ukázkovém kódu přeskočí kopírování souborů, pokud soubor již existuje. Toto není vhodné, pokud chcete podporovat novější verze stahování obsahu.

Alternativně může být kopírovat obsah do složky s názvem pro verzi a sledovat což je aktuální verze (např. v `NSUserDefaults` nebo kdekoli ukládání dokončené nákupu záznamů).

### <a name="restoring-transactions"></a>Obnovení transakce

Když `SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions` je volána, úložiště Kit vrátí všechny předchozí transakce pro uživatele. Pokud zakoupili velké množství položek, nebo pokud každý nákupu má velmi velkých balíčků obsahu, pak obnovení může mít za následek velký objem síťového provozu jako všechno, co získá zařazen do fronty ke stažení najednou.

Zvažte sledování, zda byla produktu zakoupena odděleně od skutečné stahování přidruženého obsahu balíčku.

### <a name="pausing-restarting-and-canceling-downloads"></a>Pozastavení, restartování a zrušení stahování

I když ukázkový kód nepředvádí tuto funkci, je možné pozastavit a restartujte hostované stahování obsahu. `SKPaymentQueue.DefaultQueue` Obsahuje metody pro `PauseDownloads`, `ResumeDownloads` a `CancelDownloads`.

Pokud kód volá `FinishTransaction` ve frontě platebních před probíhá stahování `Finished` pak tohoto stahování bylo zrušeno automaticky.

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>Nastavení příznaku přeskočit zálohování staženého obsahu

Pokyny zálohování serveru služby iCloud společnosti Apple doporučují, že obsah jiný uživatel, který je snadno obnovit ze serveru by měl *není* být zálohovaná (protože by zbytečně spotřebovávat Icloudu úložiště). Odkazovat [práce pomocí systému souborů](~/ios/app-fundamentals/file-system.md) dokumentace pro další podrobnosti o nastavení zálohování atributu.

## <a name="summary"></a>Souhrn

Tento článek obsahuje zavedla dvě nové funkce úložiště Kit v iOS6: nákup iTunes a další obsah z vaší aplikace a využití serveru společnosti Apple k hostování vlastní nákupy v aplikaci. Tento úvod obsaženou ve spojení s existujícím [dokumentace nákupy v aplikaci](~/ios/platform/in-app-purchasing/index.md) pro dokončení pokrytí implementace funkce úložiště Kit.

## <a name="related-links"></a>Související odkazy

- [StoreKit (ukázka)](https://developer.xamarin.com/samples/StoreKit/)
- [Nákupy v aplikaci](~/ios/platform/in-app-purchasing/index.md)
- [StoreKit Framework – referenční informace](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [Odkaz na SKStoreProductViewController – třída](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [iTunes referenční dokumentace rozhraní API pro vyhledávání](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)
- [SKDownload](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [SKPaymentQueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [SKProduct](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [WWDC Video: Prodávané produkty s Kit úložiště](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
