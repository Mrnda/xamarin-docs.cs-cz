---
title: PassKit
description: "Peněženka je aplikace systému iOS, která uchovává a zobrazí čárové kódy a další informace o propojení transakce odběratele na svůj telefon s reálném světě."
ms.topic: article
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: beff54d2b2bb72b2adf1e77819c56004b92e13f7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="passkit"></a>PassKit

_Peněženka je aplikace systému iOS, která uchovává a zobrazí čárové kódy a další informace o propojení transakce odběratele na svůj telefon s reálném světě._

Peněženka je aplikace pro Iphony a dotykem iPod s iOS 6. Ukládá a zobrazí čárové kódy a další informace o propojení transakce odběratele na svůj telefon s reálném světě. Předává se generované obchodníků a odesílají do zákazníka prostřednictvím e-mailu, adresy URL nebo v aplikaci pro iOS na němž vlastní. Peněženka ukládá a slouží k uspořádání všechny předává na telefon a zobrazí průchodu připomenutí na zamykací obrazovce v závislosti na data a času nebo umístění zařízení.

Tento dokument uvádí Peněženka, pomocí rozhraní API Kit předat Xamarin.iOS a popisuje, jak implementovat předává na vašem serveru.

 [ ![](passkit-images/image1.png "Peněženka ukládání a uspořádání všechny předává na phone")](passkit-images/image1.png)


## <a name="requirements"></a>Požadavky

Funkce úložiště Kit popisovaný v tomto dokumentu vyžadují iOS 6 a 4.5 Xcode, společně s Xamarin.iOS 6.0.


## <a name="introduction"></a>Úvod

Klíče problém, který byl odstraněn předat Kit je distribuci a správě čárové kódy. Skutečné aktuálně použití čárové kódy. Příklady:

-   **Kdybyste kupovali film lístky online** – zákazníci jsou obvykle vám e-mailem čárový kód, který představuje jejich lístků. Tento čárový kód je vytisknout a přesměrováni na filmu ke skenování pro položku.
-   **Karty věrného** – zákazníci pořadové číslo různé specifické pro úložiště karet v jejich Peněženka či peněženky, zobrazit a kontrolu při jejich nákup zboží.
-   **Kupóny** – kupóny jsou distribuovány prostřednictvím e-mailu, jako tisknutelná webové stránky, prostřednictvím letterboxes a jako čárové kódy v novinových článcích a zásobníky. Zákazníkům, aby byly k úložišti pro kontrolu pro příjem zboží, služeb nebo slevy na oplátku.
-   **Nastupují předává** –. podobá se nákupu video lístku.


Pass Kit nabízí alternativu pro každou z těchto scénářů:

-   **Film lístky** – po nákupu zákazníkem přidá průchodu lístku událostí (prostřednictvím e-mailu nebo odkaz na web). Jako dobu přístupy film průchodu se automaticky zobrazí na zamykací obrazovce jako připomenutí a na příchodem filmu průchodu je snadno načíst a zobrazí v Peněženka pro kontrolu.
-   **Karty věrného** – místo (nebo kromě) poskytuje fyzickou kartu, úložiště můžete vydat (e-mailem nebo po přihlášení na web) do úložiště průchodu karty. Úložiště můžete zadat další funkce, například aktualizace zůstatek účtu při průchodu přes nabízená oznámení a pomocí služby informace o zeměpisné poloze průchodu může automaticky zobrazí na obrazovce zámku Pokud zákazník blíží umístění úložiště.
-   **Kupóny** – kupónů předává lze snadno vygenerovat pomocí jedinečných charakteristik, které pomáhají při sledování a distribuovány prostřednictvím e-mailu nebo webu odkazy. Stažený kupóny můžete automaticky zobrazí na obrazovce zámku, pokud je uživatel téměř konkrétní umístění nebo v určitém dni (například když se blíží datum vypršení platnosti). Protože kupóny se ukládají na telefon uživatele, jsou vždy aktivní užitečný a není získat k jejich chybnému umístění. Kupóny mohou mít tendenci zákazníci stáhnout doprovodné aplikace, protože odkazy na obchod s aplikacemi lze začlenit do průchodu, zvýšení zapojení zákazníka.
-   **Nastupují předává** – po online vrácení se změnami procesu, by zákazník se zobrazila jejich průchodu zařazení e-mailem nebo odkaz. Doprovodné aplikace, poskytované poskytovatelem přenosu může zahrnout proces vrácení se změnami a také povolit další funkce, jako je výběr jejich stanici nebo jídlem zákazníka. Zprostředkovatel přenosu můžete používat nabízená oznámení k aktualizaci průchodu, pokud je přenos odložené nebo došlo ke zrušení. Jako nástupu čas přístupy průchodu se zobrazí na obrazovce zámku jako připomenutí a umožníte rychlý přístup k průchodu.


Jádro předat Kit poskytuje jednoduché a pohodlný způsob, jak uložit a zobrazit čárové kódy na zařízení iPhone nebo iPod touch. Další čas a integrace umístění zamykací obrazovka nabízená oznámení a doprovodné aplikace integrovat nabídky a základ pro velmi sofistikované prodej lístků a fakturace služby.


## <a name="pass-kit-ecosystem"></a>Předat ekosystém Kit

Kit průchodu není právě rozhraní API v rámci CocoaTouch, místo je součástí větší ekosystém aplikace, data a služby, které usnadňují zabezpečené sdílení a správu čárové kódy a další data. Tento nejvyšší úrovni diagram zobrazuje různé entity, které může být zahrnut ve vytváření a používání předává:

 [ ![](passkit-images/image2.png "Tento nejvyšší úrovni diagram znázorňuje entity zahrnutých ve vytváření a používání předává")](passkit-images/image2.png)

Každá část ekosystému má jasně definované role:

-   **Peněženka** – společnosti Apple aplikaci předdefinované iOS (pro iPhone a iPod touch), která ukládá a zobrazí předává. Toto je jediným místem, který předává jsou generovány pro použití v praxi (ie čárový kód se zobrazí, společně s lokalizovaná data v průchodu).
-   **Doprovodné aplikace** – vytvořené poskytovateli průchodu rozšířit funkce předává budou vydávat, jako je například přidávání hodnotu na kartu úložiště, změna stanici na nástupu úspěšná nebo jiné funkce specifické pro obchodní aplikace pro iOS 6. Doprovodné aplikace nejsou potřeba pro převedení, aby byla užitečná.
-   **Váš server** – zabezpečeného serveru, kde může předává generovat a podepsaný pro distribuci. Doprovodné aplikace může připojit k serveru ke generování nové předává nebo aktualizace stávající předává žádosti. Volitelně může implementovat služby webového rozhraní API, které Peněženka by volat průchodů aktualizovat.
-   **Servery služby APN** – váš server má schopnost oznámit Peněženka aktualizací, které se průchodu na daném zařízení pomocí služby APN. Nabízená oznámení Peněženka, který bude kontaktovat server podrobnosti o změnu. Doprovodné aplikace není nutné pro tuto funkci implementovat APN (můžete poslouchat `PKPassLibraryDidChangeNotification` ).
-   **Přenos aplikací** – aplikace, který není manipulaci s přímo předává (jako jsou doprovodné aplikace provést), ale které lze vylepšit jejich nástroj rozpozná předává a povolil jejich mají být přidány do Peněženka. Poštovních klientů, sociálních sítí prohlížeče a jinými aplikacemi agregace dat všechny setkat přílohy nebo odkazy na předává.


Celý ekosystém vypadá komplexní, takže je vhodné poznamenat, že některé součásti jsou volitelné a je možné, mnohem jednodušší implementací předat Kit.

## <a name="what-is-a-pass"></a>Co je Pass-?

Do průchodu je kolekce dat, která představují lístek, kupónů a karty. Může být žádoucí pro jedno použití konkrétního (a proto neobsahuje podrobnosti, například letu číslo a stanici přidělení) nebo může více použití token, který může být sdílen libovolný počet uživatele (například slevu kupónů). Podrobný popis je k dispozici v společnosti Apple [předat soubory](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html) dokumentu.


### <a name="types"></a>Typy

Aktuálně pět podporované typy, které může být v aplikaci Peněženka rozlišené rozložení a horní hrany průchodu:

-  **Události lístku** – malé Polokruhová vystřižení.
-   **Zobrazení průchodu** – zdířek v postranní, specifické pro přenos ikoně lze zadat (např. sběrnice, train, letadle).
-   **Karta ukládání** – zaokrouhlené nahoře, třeba kreditní nebo debetní karty.
-  **Kupónů** – perforované podél horního.
-  **Obecné** – je to stejné jako úložiště karty, zaokrouhlené nahoru.


Na tomto snímku obrazovky jsou uvedeny typy pět průchodu (v pořadí: kupónů obecnou nebo ukládání karty, nástupu průchodu a událostí lístku):

 [ ![](passkit-images/image3.png "Na tomto snímku obrazovky jsou uvedeny typy pět průchodu")](passkit-images/image3.png)

### <a name="file-structure"></a>Struktura souborů

Předejte soubor je ve skutečnosti archivu ZIP s **.pkpass** rozšíření obsahující některé konkrétní soubory JSON (povinné), celou řadu bitové kopie souborů (volitelné) a také lokalizované řetězce (volitelné).

-   **PASS.JSON** – vyžaduje. Obsahuje všechny informace pro průchod.
-   **manifest.JSON** – vyžaduje. Obsahuje hodnoty hash SHA1 pro každý soubor při průchodu s výjimkou podpisu souboru a tento soubor (manifest.json).
-   **podpis** – vyžaduje. Vytvořit po přihlášení `manifest.json` soubor s certifikátem, vygeneruje v iOS Provisioning Portal.
-  **Logo.PNG** – volitelné.
-  **Background.PNG** – volitelné.
-  **Icon.PNG** – volitelné.
-  **Lokalizovatelný řetězce soubory** – volitelné.


Struktura adresářů průchodu souboru je uveden níže (Toto je obsah archivu ZIP):

 [ ![](passkit-images/image4.png "Struktura adresářů soubor průchodu je tady uvedené.")](passkit-images/image4.png)

### <a name="passjson"></a>pass.json

JSON je formát, protože na serveru se obvykle vytvářejí předává – znamená, že je generování kódu platformy bez ohledu na serveru. Jsou tři důležité informace při každé průchodu:

-   **teamIdentifier** – tím propojí všechny předává generovat ke svému účtu obchodu s aplikacemi. Tato hodnota je viditelný v iOS Provisioning Portal.
-   **passTypeIdentifier** – registrace portálu zřizování do skupiny předá společně (Pokud můžete vytvořit více než jeden typ). Například v kavárně může vytvořit úložiště předat typu karta umožňuje získat kredity věrnost zákazníků, ale také vytvořit a distribuovat slevu kupóny samostatný účet kupónů předat typu. Tento stejný kavárně může i podržte za provozu Hudba události a vydání lístku předává události pro ty.
-   **serialNumber** – řetězec jedinečný v rámci to `passTypeidentifier` . Hodnota je plné krytí k Peněženka, ale je důležité pro sledování konkrétní předává při komunikaci se serverem.


Existuje velký počet jiných klíčů JSON v každém průchodu, je příkladem jsou uvedeny níže:

``` 
{
   "passTypeIdentifier":"com.xamarin.passkitdoc.banana",  //Type Identifier (iOS Provisioning Portal)
   "formatVersion":1,                                     //Always 1 (for now)
   "organizationName":"Xamarin",                          //The name which appears on push notifications
   "serialNumber":"12345436XYZ",                          //A number for you to identify this pass
   "teamIdentifier":"XXXAAA1234",                         //Your Team ID
   "description":"Xamarin Demo",                          //
   "foregroundColor":"rgb(54,80,255)",                    //color of the data text (note the syntax)
   "backgroundColor":"rgb(209,255,247)",                  //color of the background
   "labelColor":"rgb(255,15,15)",                         //color of label text and icons
   "logoText":"Banana ",                                  //Text that appears next to logo on top
   "barcode":{                                            //Specification of the barcode (optional)
      "format":"PKBarcodeFormatQR",                       //Format can be QR, Text, Aztec, PDF417
      "message":"FREE-BANANA",                            //What to encode in barcode
      "messageEncoding":"iso-8859-1"                      //Encoding of the message
   },
   "relevantDate":"2012-09-15T15:15Z",                    //When to show pass on screen. ISO8601 formatted.
  /* The following fields are specific to which type of pass. The name of this object specifies the type, e.g., boardingPass below implies this is a boarding pass. Other options include storeCard, generic, coupon, and eventTicket */
   "boardingPass":{
/*headerFields, primaryFields, secondaryFields, and auxiliaryFields are arrays of field object. Each field has a key, label, and value*/
      "headerFields":[          //Header fields appear next to logoText
         {
            "key":"h1-label",   //Must be unique. Used by iOS apps to get the data.
            "label":"H1-label", //Label of the field
            "value":"H1"        //The actual data in the field
         },
         {
            "key":"h2-label",
            "label":"H2-label",
            "value":"H2"
         }
      ],
      "primaryFields":[       //Appearance differs based on pass type
         {
            "key":"p1-label",
            "label":"P1-label",
            "value":"P1"
         }
      ],
      "secondaryFields":[     //Typically appear below primaryFields
         {
            "key":"s1-label",
            "label":"S1-label",
            "value":"S1"
         }
      ],
      "auxiliaryFields":[    //Appear below secondary fields
         {
            "key":"a1-label",
            "label":"A1-label",
            "value":"A1"
         }
      ],
      "transitType":"PKTransitTypeAir"  //Only present in boradingPass type. Value can
                                        //Air, Bus, Boat, or Train. Impacts the picture
                                        //that shows in the middle of the pass.
   }
}
```

### <a name="barcodes"></a>Čárové kódy.

Jsou podporovány pouze 2D formáty: PDF417, Aztécko, QR. Apple deklarací, že nevhodným pro kontrolu na obrazovce phone podsvícený 1D čárové kódy.

Alternativní text zobrazený pod čárového kódu je volitelné – některé obchodníků chtějí mít možnost čtení/typ ručně.

ISO 8859-1 kódování je nejběžnější, zkontrolujte, které kódování prohledávání systémy, které budou číst vaše předává používá.

### <a name="relevancy-lock-screen"></a>Důležitosti (zámek obrazovky)

Existují dva typy dat, která může způsobit Pass, který se má zobrazit na obrazovce uzamčení:

 **Poloha**

Při průchodu, např úložišť, která často navštíví zákazníka nebo umístění filmu nebo letiště můžete zadat až 10 umístění. Zákazník může nastavit tyto umístění prostřednictvím doprovodné aplikace nebo zprostředkovatel může zjistit, je z dat využití (Pokud shromážděné pomocí oprávnění zákazníka).

Při průchodu se zobrazí na obrazovce zámku, ochranná se vypočítává tak, aby při uživatele z oblasti průchodu je skrytý na obrazovce zámku. Poloměr je vázaný na předat styl, aby se zabránilo zneužití.

 **Datum a čas**

Při průchodu lze zadat pouze jeden datum a čas. Datum a čas je užitečné pro spuštění připomenutí zamykací obrazovka pro nástupu průchodů a událostí lístků.

Lze aktualizovat přes nabízená nebo přes PassKit API tak, aby mohlo dojít k aktualizaci data a času v případě více použití lístku (například lístek sezóny theatre nebo sportovní komplexní).

### <a name="localization"></a>Lokalizace

Překladu průchodu do různých jazyků je podobná lokalizace aplikace pro iOS – vytvořit konkrétní adresáře s jazyka `.lproj` rozšíření a umístěte lokalizované elementy uvnitř. Překlady text by měly být zadány do `pass.strings` souboru, zatímco lokalizované Image musí mít stejný název jako bitovou kopii nahrazují v kořenu průchodu.

## <a name="security"></a>Zabezpečení

Předává jsou podepsané privátní certifikát, který vytvoříte v iOS Provisioning Portal. Kroky k podepsání průchodu jsou:

1.  Vypočítat hodnotu hash SHA1 pro každý soubor v adresáři průchodu (nezahrnují `manifest.json` nebo `signature` souboru, ani z nich by měla existovat v této fázi přesto).
1.  Zápis `manifest.json` jako seznam každý název souboru s jeho hash JSON klíč/hodnota.
1.  Použít certifikát pro přihlášení `manifest.json` souboru a zápisu do souboru s názvem výsledek `signature` .
1.  ZIP všechno a poskytněte výsledný soubor `.pkpass` příponu souboru.


Vzhledem k tomu, že privátní klíč se vyžaduje k podepisování průchodu, tento proces musí provést pouze na zabezpečeného serveru, který ovládáte. Nedistribuovat klíče a zkuste to generovat předává v aplikaci.

 
## <a name="configuration-and-setup"></a>Konfigurace a instalace

Tato část obsahuje pokyny k nastavení podrobností o zřizování a vytvořit první fáze.

### <a name="provisioning-passkit"></a>Zřizování PassKit

Aby průchodu vstupovat do obchodu s aplikacemi musí být propojena k vývojářského účtu. To vyžaduje dva kroky:

1.  Průchodu musí být registrovaný pomocí jedinečný identifikátor, názvem ID předat typu.
1.  Platný certifikát musí vygenerovat k podepsání průchodu s digitální podpis pro vývojáře.

Chcete-li vytvořit předat ID typu proveďte následující.


<a name="create-passid"/>

#### <a name="create-a-pass-type-id"></a>Vytvoření ID typu Pass

Prvním krokem je nastavit a předat typ ID pro každý jiný _typ_ průchodu podporovaná. Předat ID (nebo identifikátor předat typu) vytvoří jedinečný identifikátor pro průchod. Toto ID budeme používat pro propojení průchodu vývojářského účtu pomocí certifikátu.

1. V [identifikátory, certifikátů a profilů části IOS Provisioning Portal](https://developer.apple.com/account/overview.action), přejděte na **identifikátory** a vyberte **předat ID typu** . Vyberte  **+**  tlačítko pro vytvoření nového typu pass: [ ![ ] (passkit-images/passid.png "vytvoříte nový typ průchodu")](passkit-images/passid.png)

2.   Zadejte **popis** (název) a **identifikátor** (jedinečného řetězce) pro průchod. Všimněte si, že všechna ID typu předat musí začínat řetězcem `pass.` v tomto příkladu používáme `pass.com.xamarin.coupon.banana` : [ ![ ] (passkit-images/register.png "zadejte popis a identifikátor")](passkit-images/register.png)


3.   Potvrďte ID předat stisknutím **zaregistrovat** tlačítko.


<a name="generate" />

#### <a name="generate-a-certificate"></a>Vygenerování certifikátu

Pokud chcete vytvořit nový certifikát pro tento typ ID předat, postupujte takto:

1.  Vyberte nově vytvořené ID předat ze seznamu a klikněte na tlačítko **upravit** : [ ![ ] (passkit-images/pass-done.png "vyberte nové ID předat ze seznamu")](passkit-images/pass-done.png)

    Pak vyberte **vytvořit certifikát...** :

    [ ![](passkit-images/cert-dist.png "Vyberte vytvoření certifikátu")](passkit-images/cert-dist.png)


2.  Postupujte podle kroků pro vytvoření podepsání certifikátu požadavku (CSR).
  
3. Stiskněte **pokračovat** tlačítko na portál pro vývojáře a nahrajte CSR k vygenerování vašeho certifikátu.

4. Stažení certifikátu a poklikejte na ji nainstalovat v řetězci klíčů.


Teď, když jsme vytvořili certifikát pro tento typ ID předat, další část popisuje postup předejte vytvořit ručně.

Další informace o zřizování pro Peněženka, najdete v části [práce s možností](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) průvodce.

 <a name="Create_a_Pass_Manually" />

### <a name="create-a-pass-manually"></a>Ruční vytvoření do průchodu

Teď, vytvořili jsme typ předat jsme ručně vytvořit průchodu k testování v simulátoru nebo na zařízení. Postup vytvoření předejte jsou:

-  Vytvořte adresář, který bude obsahovat soubory průchodu.
-  Vytvořte soubor pass.json, která obsahuje všechna požadovaná data.
-  Zahrňte bitové kopie do složky (v případě potřeby).
-  Výpočtu hodnoty hash SHA1 pro každý soubor ve složce a zapíše do manifest.json.
-  Podepište soubor stažený certifikát .p12 manifest.json.
-  ZIP obsah adresáře a přejmenujte s příponou .pkpass.


V ukázkový kód pro tento článek, který slouží ke generování předejte nejsou některé zdrojové soubory. Použít soubory v `CouponBanana.raw` adresář CreateAPassManually adresáře. Jsou dostupné následující soubory:

 [ ![](passkit-images/image18.png "Tyto soubory k dispozici")](passkit-images/image18.png)

Otevřete pass.json a upravte JSON. Je nutné aktualizovat minimálně `passTypeIdentifier` a `teamIdentifer` tak, aby odpovídaly účtu vývojáře Apple.

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

Pak musíte výpočtu hodnoty hash pro každý soubor a vytvořit `manifest.json` souboru. Po dokončení se bude vypadat přibližně takto:

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

Další podpis nutné pro tento soubor pomocí certifikátu (soubor .p12), který byl vygenerovaný pro toto ID průchodu typu.

 <a name="Signing_On_a_Mac" />


#### <a name="signing-on-a-mac"></a>Podepisování na Macu

Stažení **Peněženka počáteční hodnoty podporu materiály** z [stáhne Apple](https://developer.apple.com/downloads/index.action?name=Passbook) lokality. Použití `signpass` nástroj pro převod vaší složky na průchodu (to bude také počítat SHA1 hashuje a ZIP výstup do souboru .pkpass).

 <a name="Signing_On_a_PC" />


#### <a name="signing-on-a-pc"></a>Podpis na počítači

V ukázce je kód pro tento článek existuje projekt s názvem `signpassnet` spouští v prostředí .NET se v systému Windows. Pokusí se napodobovat nástroj společnosti Apple, ale obsahuje mnohem menší ověřovacího kódu.

 <a name="Testing" />


#### <a name="testing"></a>Testování

Pokud byste chtěli zkontrolujte výstup těchto nástrojů (podle nastaví název souboru .zip a pak ho otevřít), zobrazí se následující soubory (Poznámka: přidání `manifest.json` a `signature` soubory):

 [ ![](passkit-images/image19.png "Zkoumání výstupu tyto nástroje")](passkit-images/image19.png)

Jakmile podepsané, ZIPped a přejmenovat soubor (např. k `BananaCoupon.pkpass`) můžete ji přetáhněte do simulátoru k testování nebo e-mailu sami načíst na skutečné zařízení. Měli byste vidět obrazovce **přidat** průchodu, například takto:

 [ ![](passkit-images/image20.png "Přidat na obrazovce průchodu")](passkit-images/image20.png)

Za normálních okolností byste je možné automatizovat procesu na serveru, ale ruční vytvoření průchodu může být možnost pro malé firmy, které jsou pouze vytváření kupóny, které nevyžadují podpoře back-end serverů.

 <a name="Wallet" />

## <a name="wallet"></a>Peněženka

Peněženka je centrální druhem ekosystému předat Kit. Tento snímek obrazovky ukazuje prázdný Peněženka a vzhled průchodu seznamu a jednotlivých předává:

 [ ![](passkit-images/image21.png "Tento snímek obrazovky ukazuje prázdný Peněženka a vzhled průchodu seznamu a jednotlivých předává")](passkit-images/image21.png)

Funkce Peněženka:

-  Je jediným místem, který předává se vykreslují jejich čárový kód pro skenování.
-  Uživatel může změnit nastavení pro aktualizace. Pokud je povoleno, můžete aktivovat nabízená oznámení aktualizace dat v průchodu.
-  Uživatele můžete povolit nebo zakázat integrace zámku obrazovky. Pokud je povoleno, umožňuje průchod automaticky zobrazí na zamykací obrazovce, podle příslušných čas a umístění dat vložených průchodu.
-  Zadní straně průchodu podporuje – aktualizace obsahu, pokud je zadaný--adresy URL webového serveru ve formátu JSON předat.
-  Doprovodné aplikace můžete otevřít (nebo stáhnout) Pokud je zadaný ve formátu JSON předat ID aplikace.
-  Předává odstraněním (s cute skartace animace).


 <a name="Getting_Passes_into_Wallet" />

## <a name="adding-passes-into-wallet"></a>Přidání předává do peněženka

Předává lze přidat do Peněženka následujícími způsoby:

* **Přenos aplikací** – přímo tyto předává není manipulaci, jednoduše načíst průchodu soubory a prezentovat možnost jejich přidáním do Peněženka. 

* **Doprovodné aplikace** – tyto zapisují zprostředkovatelé distribuovat předává a nabízet další funkce a vyhledejte nebo je upravit. Aplikace Xamarin.iOS mají úplný přístup k rozhraní API Kit předat k vytvoření a předává manipulaci. Předává lze přidat pomocí Peněženka `PKAddPassesViewController`. Tento proces je popsán v podrobněji **doprovodné aplikace** část tohoto dokumentu.

### <a name="conduit-applications"></a>Přenos aplikací

Přenos aplikací jsou zprostředkující aplikace, které může přijímat předává jménem uživatele a musí být naprogramovaný tak, aby rozpoznat jejich typu obsahu a poskytovat funkce pro přidání do Peněženka. Příklady přenos aplikace:

-   **E-mailu** – rozpozná přílohy jako do průchodu.
-   **Safari** – při kliknutí na odkaz na adresu URL předat rozpozná předat Content-Type.
-   **Další vlastní aplikace** – jakékoli aplikaci, která dostanou přílohy nebo odkazy (klienti sociálních médií, e-mailu čtečky atd.).


Tento snímek obrazovky ukazuje jak **e-mailu** v iOS 6 rozpozná průchodu přílohy a (Pokud dotýkal) nabízí **přidat** jeho Peněženka.

 [ ![](passkit-images/image22.png "Tento snímek obrazovky ukazuje, jak e-mailu v iOS 6 rozpozná průchodu přílohy")](passkit-images/image22.png)

 [ ![](passkit-images/image23.png "Tento snímek obrazovky ukazuje, jak e-mailu nabízí přidání průchodu přílohy do peněženka")](passkit-images/image23.png)

Pokud vytváříte aplikace, které by mohly být kanál pro předává, můžete rozpoznáno:

-  **Příponu souboru** -.pkpass
-  **Typ MIME** -application/vnd.apple.pkpass
-  **UTI** – com.apple.pkpass


Základní operace aplikace přenos je načíst soubor průchodu a volání předat Kit `PKAddPassesViewController` poskytuje možnosti pro přidání do jejich Peněženka průchodu uživateli. Implementace tohoto řadiče zobrazení je popsaná v další části na **doprovodné aplikace**.

Přenos aplikace není nutné zřídit pro konkrétní ID typu předat v stejným způsobem jako doprovodné aplikace.

## <a name="companion-applications"></a>Doprovodné aplikace

Doprovodné aplikace poskytuje další funkce pro práci s úpravami, včetně vytváření Pass, Probíhá aktualizace informací, které jsou přidružené průchodu a jinak správu předává přidružené k aplikaci.

Duplicitní funkce Peněženka neměli doprovodné aplikace. Nejsou určeny k zobrazení předává kontrolu.

Tento zbytek této části popisuje, jak stavět základní doprovodné aplikaci, která komunikuje s předat Kit.

### <a name="provisioning"></a>Zřizování

Protože Peněženka je technologie úložiště, aplikace musí být zřízená samostatně a nejde použít profil zřizování týmu nebo zástupný znak ID aplikace. Odkazovat [práce s možností](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) Průvodce pro vytvoření jedinečné ID aplikace a profil zřizování pro aplikace Peněženka.

### <a name="entitlements"></a>Oprávnění

**Entitlements.plist** soubor by měl být součástí všechny poslední projektu Xamarin.iOS. Pokud chcete přidat nový soubor Entitlements.plist, postupujte podle kroků v [práce oprávnění](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

Chcete-li nastavit oprávnění, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Dvakrát klikněte na **Entitlements.plist** souboru v řešení pro otevření editoru Entitlements.plist:

![](passkit-images/image31.png "Entitlements.plst editor")

V části Peněženka, vyberte **povolit Peněženka** možnost

![](passkit-images/image32.png "Povolit Peněženka nároku")


Výchozí možnost je pro vaši aplikaci povolit všechny typy předat. Je však možné omezit vaší aplikace a povolit pouze podmnožinu team průchodu typy. Chcete-li povolit tuto vyberte **povolit podmnožinu team předat typy** a zadejte identifikátor typu pass podmnožinu, kterou chcete povolit.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dvakrát klikněte **Entitlements.plist** soubor otevřít zdrojový soubor XML.

Přidat Peněženka nárok, nastavte **vlastnost** k `Passbook Identifiers` v rozevírací nabídce, která bude automaticky nastavena **typ** `Array`. Potom nastavte řetězec **hodnotu** k `$(TeamIdentifierPrefix)*`:

![](passkit-images/image33.png "Povolit Peněženka nároku")

Tato akce povolí aplikaci, kterou chcete povolit všechny typy předat. Omezení aplikace a povolit pouze podmnožinu typy průchodu týmů, nastavte hodnotu řetězce:

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

Kde `pass.$(CFBundleIdentifier)` je předat ID, který byl vytvořen [výše](~/ios/platform/passkit.md)

-----

### <a name="debugging"></a>Ladění

Pokud máte potíže s nasazení aplikace, zkontrolujte, zda používáte správný **profil zřizování** a že `Entitlements.plist` je vybrán jako **vlastní oprávnění** soubor **iPhone podepisování sady** možnosti.

Pokud k této chybě dojde při nasazení:

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

pak se `pass-type-identifiers` oprávnění pole je nesprávný (nebo neodpovídá **profil zřizování**). Ověřte správnost ID předat typ a ID vašeho týmu.

 <a name="Classes" />

## <a name="classes"></a>Třídy

Následující třídy předat Kit jsou k dispozici pro aplikace pro přístup k předává:

-  **PKPass** – na instanci průchodu.
-  **PKPassLibrary** – poskytuje rozhraní API pro přístup k předává na zařízení.
-  **PKAddPassesViewController** – slouží k zobrazení průchodu k uložení v jejich Peněženka uživatelem.
-  **PKAddPassesViewControllerDelegate** – vývojáři Xamarin.iOS


## <a name="example"></a>Příklad

Odkazovat na projekt PassLibrary ukázkový kód pro tento článek. Ukazuje následující běžné funkce, které by byly zapotřebí v aplikaci doprovodné Peněženka:

### <a name="check-that-wallet-is-available"></a>Zkontrolujte, že Peněženka je k dispozici

Peněženka není k dispozici na Ipadu, takže aplikace měli zkontrolovat před pokusem o přístup k funkcím předat Kit.

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>Vytvoření Instance knihovny průchodu

Knihovna předat Kit není typu singleton, aplikace by měl vytvářet a ukládat a instanci pro přístup k rozhraní API předat Kit.

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>Získat seznam předává

Aplikace může požádat o seznam předává z knihovny. Tento seznam je filtrovaný podle předat Kit automaticky, tak, aby pouze vidíte předává, které byly vytvořeny s ID vašeho týmu a které jsou uvedeny v vaše oprávnění.

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

Všimněte si, simulátoru není filtrovat seznam předává vrátí, takže tato metoda by měla být testována vždy na skutečné zařízení. Tento seznam lze zobrazit v UITableView, jak vypadá ukázkové aplikace takto po dvou kupóny byly přidány:

 [ ![](passkit-images/image29.png "Vzhled ukázkové aplikace, jako to po byly přidány dva kupóny")](passkit-images/image29.png)


### <a name="displaying-passes"></a>Zobrazení předává

Omezená sada informace je k dispozici pro vykreslení předává v rámci doprovodné aplikací.

Vyberte z této sady standardních vlastností zobrazíte seznam předává, stejně jako ukázkový kód.

```csharp
string passInfo =
                "Desc:" + pass.LocalizedDescription
                + "\nOrg:" + pass.OrganizationName
                + "\nID:" + pass.PassTypeIdentifier
                + "\nDate:" + pass.RelevantDate
                + "\nWSUrl:" + pass.WebServiceUrl
                + "\n#" + pass.SerialNumber
                + "\nPassUrl:" + pass.PassUrl;
```

Tento řetězec je zobrazena jako výstrahu v ukázce:

 [ ![](passkit-images/image30.png "Výstraha kupónů vybrané v ukázce")](passkit-images/image30.png)

Můžete také `LocalizedValueForFieldKey()` metodu pro načtení dat z polí v předává navrženy (vzhledem k tomu, že budete vědět, co polí musí být přítomen). Tento příklad kódu nezobrazuje.

### <a name="loading-a-pass-from-a-file"></a>Načítání předejte ze souboru

Protože průchodu lze přidat pouze k Peněženka s oprávněními uživatele, musí být a dát jim rozhodnout předložen řadič zobrazení. Tento kód se používá v **přidat** tlačítko v příkladu se načíst předdefinovaných Pass, vložené v aplikaci (měli byste nahradit to ten, který jste se zaregistrovali):

```csharp
NSData nsdata;
using ( FileStream oStream = File.Open (newFilePath, FileMode.Open ) ) {
        nsdata = NSData.FromStream ( oStream );
}
var err = new NSError(new NSString("42"), -42);
var newPass = new PKPass(nsdata,out err);
var pkapvc = new PKAddPassesViewController(newPass);
NavigationController.PresentModalViewController (pkapvc, true);
```

Předloží průchodu **přidat** a **zrušit** možnosti:

 [ ![](passkit-images/image20.png "Pass možnosti Přidat a zrušit výběr")](passkit-images/image20.png)

### <a name="replace-an-existing-pass"></a>Nahraďte stávající průchodu

Nahrazení stávající průchodu nevyžaduje oprávnění uživatele, ale se nezdaří, pokud průchodu již neexistuje.

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>Úpravy průchodu

PKPass není měnitelný, takže nelze aktualizovat průchodu objekty v kódu. Ke změně dat při průchodu aplikace musí mít přístup k webovému serveru, která může uchovávat informace o předává a vygenerujte nový soubor průchodu s aktualizovanými hodnotami, které aplikace můžete stáhnout.

Vytvoření souboru průchodu musí na serveru provést, protože předává musí být podepsané certifikátem, který musí být udržovány privátní a zabezpečení.

Po vygenerování aktualizovaný soubor Pass, použijte `Replace` metoda přepsat původní data v zařízení.

### <a name="display-a-pass-for-scanning"></a>Zobrazení předejte pro skenování

Jako v předchozí části, jen Peněženka můžete zobrazit předejte pro skenování. Do průchodu lze zobrazit pomocí `OpenUrl` metoda, jak je znázorněno:

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>Příjem oznámení změn

Aplikace může naslouchat změny do knihovny předat pomocí `PKPassLibraryDidChangeNotification`. Změny může být způsobeno oznámení spuštění aktualizace na pozadí, takže je dobrým zvykem naslouchat je ve vaší aplikaci.

```csharp
noteCenter = NSNotificationCenter.DefaultCenter.AddObserver (PKPassLibrary.DidChangeNotification, (not) => {
    BeginInvokeOnMainThread (() => {
        new UIAlertView("Pass Library Changed", "Notification Received", null, "OK", null).Show();
        // refresh the list
        var passlist = library.GetPasses ();
        table.Source = new TableSource (passlist, library);
        table.ReloadData ();
    });
}, library);  // IMPORTANT: must pass the library in
```

Je důležité k předání instance knihovny při registraci pro oznámení, protože PKPassLibrary není typu singleton.

## <a name="server-processing"></a>Zpracování na serveru

Podrobnou diskuzi o vytvoření aplikace server pro podporu předat Kit je nad rámec této úvodní článek.

Kód .NET, které jsou součástí *signpassnet* ukázka by mohly být použity jako základ serverové metodu, která může generovat předává.

Zobrazení [WWDC Video: Úvod Passbook, část 2](https://developer.apple.com/videos/wwdc/2012/?include=309#309) 27:00 minut, další informace.

### <a name="other-resources"></a>Další zdroje

V tématu [dotnet passbook](https://github.com/tomasmcguinness/dotnet-passbook) otevřete kódu na straně serveru zdroj C#.

## <a name="push-notifications"></a>Nabízená oznámení

Podrobnou diskuzi o používání nabízených oznámení k aktualizaci předává je nad rámec této úvodní článek.

Je potřeba implementovat rozhraní API REST jako definované Apple reagovat na webové požadavky Peněženka, pokud je potřeba aktualizovat. Kód .NET, které jsou součástí *signpassnet* ukázka by mohly být použity jako základ generování nové předává v důsledku těchto požadavků.

Zobrazení [WWDC Video: Úvod Passbook, část 2](https://developer.apple.com/videos/wwdc/2012/?include=309#309) 27:00 minut, další informace.

## <a name="summary"></a>Souhrn

Tento článek zavedená předat Kit, uvedené některé z důvodů, proč je užitečné a popisuje různé části, které je nutné implementovat pro úplné řešení předat Kit. Je popsané kroky potřebné ke konfiguraci účtu vývojáře Apple vytvořit předává, tento proces zpřístupnění Pass, ručně a také jak získat přístup k rozhraní API Kit předat z aplikace pro Xamarin.iOS.

## <a name="related-links"></a>Související odkazy

- [CreateAPassManually (ukázka)](https://developer.xamarin.com/samples/PassKit/)
- [Ukázka PassKit](https://developer.xamarin.com/samples/monotouch/PassKit/)
- [Úvod do iOSu 6](~/ios/platform/introduction-to-ios6/index.md)
- [Průvodce programováním Passbook](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Conceptual/PassKit_PG/Chapters/Introduction.html)
- [Passbook pro vývojáře](https://developer.apple.com/passbook/)
- [O souborech průchodu](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [Předat Kit Framework – referenční informace](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Předat Kit Framework – referenční informace](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Odkaz na aplikaci Passbook webovou službu](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
