---
title: Úvod do životního cyklu mobilní Software
description: Tento článek popisuje životního cyklu softwaru s ohledem na mobilní aplikace a popisuje některé aspekty při vytváření mobilních projekty vyžaduje. Pro vývojáře, kteří chtějí stačí přejít přímo na a spusťte vytváření může tento průvodce přeskočí a čtení později podrobnější vysvětlení nástroje pro vývoj mobilních řešení.
ms.prod: xamarin
ms.assetid: 420c5fdf-4610-4e71-9db5-fe894c961924
author: asb3993
ms.author: amburns
ms.date: 11/22/2016
ms.openlocfilehash: c93a063c9c933e1b9f397d172115471473cf8f35
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="introduction-to-the-mobile-software-development-lifecycle"></a>Úvod do životního cyklu mobilní Software

_Tento článek popisuje životního cyklu softwaru s ohledem na mobilní aplikace a popisuje některé aspekty při vytváření mobilních projekty vyžaduje. Pro vývojáře, kteří chtějí stačí přejít přímo na a spusťte vytváření může tento průvodce přeskočí a čtení později podrobnější vysvětlení nástroje pro vývoj mobilních řešení._

Vytváření mobilních aplikací může být stejně snadná jako otevírání až rozhraní IDE, vyvolání něco společně, provádění rychlé bit testování a odesílá na obchod s aplikacemi – všechny provádí za dnešní odpoledne. Nebo může být velmi zahrnutých proces, který zahrnuje přísných počáteční návrh, testování, QA testování na tisíce zařízení, životní cyklus úplné beta a pak nasazení několika různými způsoby použitelnost.

V tomto dokumentu vytvoříme trvat důkladné úvodní posouzení vytváření mobilních aplikací, včetně:

1.   **Proces** – proces vývoje softwaru se nazývá životního cyklu vývoj softwaru (SDLC). Podíváme všech fázích SDLC s ohledem na vývoj mobilních aplikací, včetně: inspiraci, návrh, vývoj, ustálení, nasazení a údržby.
1.   **Aspekty** – existuje několik aspektů při vytváření mobilních aplikací, hlavně na rozdíl od tradičních webových nebo desktopových aplikací. Podíváme těchto aspektů a jejich vliv na vývoj mobilních řešení.

Tento dokument je určený k odpovědi na základní dotazy týkající se vývoje mobilní aplikace, pro vývojáře aplikací nové a zkušenosti agentem. Představení většinu koncepty, které je potřeba spustit do během celý softwaru vývoj životního cyklu (SDLC) trvá poměrně komplexní přístup. Tento dokument nemusí být pro všechny uživatele, pokud jste právě začněte sestavovat aplikace itching, doporučujeme však přejít na bod dále [Úvod do vývoj mobilních řešení](~/cross-platform/get-started/introduction-to-mobile-development.md) průvodce a potom vracející se zpět k tomuto dokumentu později.

## <a name="mobile-development-sdlc"></a>Mobilní vývoj SDLC

Životní cyklus pro vývoj mobilních řešení je do značné míry nejsou jiné než SDLC pro aplikace pro web nebo plochy. S těmi, která, existují obvykle 5 hlavní části procesu:

1.   **Zahájení** – všechny aplikace začínat představu. Tento nápad je obvykle vylepšení do solidní základ pro aplikaci.
1.   **Návrh** – fáze návrhu se skládá z definice aplikace uživatelské prostředí (UX), například to, jaké Obecné rozložení se, jak to funguje, a také vypnutí této UX do správné návrh uživatelské rozhraní (UI), obvykle pomocí grafický Návrhář apod.
1.   **Vývoj** – nejvíc prostředků náročné fáze, to je obvykle skutečné sestavení aplikace.
1.   **Ustálení** – Pokud vývoj dost daleko hotová, QA obvykle začne testování aplikace a chyby odstraněny. Častým, které aplikace přejde do fáze omezené beta ve kterém širší skupiny uživatelů je zadána možnost ho použít k poskytnutí zpětné vazby a informovat změny.
1.  **Nasazení**

Často mnoho z těchto částí je překryté, například je běžné pro vývoj na děje, když je ukončen uživatelského rozhraní, a může i se obrátit na návrh uživatelského rozhraní. Kromě toho může být aplikace přejít do fáze ustálení zároveň aby se přidává nové funkce na novou verzi.

Kromě toho tyto fáze slouží v libovolný počet SDLC metodiky například Agile, Spirála, vodopádu atd.

Všechny tyto fáze bude možné vysvětlené podrobněji podle následující části.

### <a name="inception"></a>Zahájení

Všudypřítomnosti a úroveň interakce, které mají uživatelé s mobilními zařízeními znamená, že téměř každý má představu mobilní aplikaci. Mobilní zařízení otevře zcela nový způsob, jak pracovat s computing, webové a i podnikové infrastruktury.

Zahájení fáze se točí kolem definování a upřesnění nápad pro aplikaci.
K vytvoření aplikace pro úspěšné, je potřeba některé základní ptají. Zde jsou některé věci, které je třeba zvážit před publikováním aplikace v jednom z veřejného App Storu:

-   **Konkurencí** – existují podobné aplikace odhlašování došlo již? Pokud ano, jak tato aplikace rozlišit od ostatních?

Pro aplikace, které budou distribuovány v podniku:

-   **Integrace infrastruktury** – co stávající infrastruktury se ji integrovat nebo rozšířit?

Kromě toho by měla v kontextu mobilním uspořádání vyhodnotí aplikace:

-   **Hodnota** – co hodnotu této aplikace přináší uživatelů? Jak se používají ho?
-   **Formuláře nebo Mobility** – jak bude tato aplikace fungovat v mobilním uspořádání? Jak můžete přidat hodnotu pomocí mobilní technologií, jako je například sledování umístění, fotoaparát, atd.?

Pomoci při navrhování funkce aplikace, může být užitečné k definování aktéři a [případy použití](http://en.wikipedia.org/wiki/Use_case). Aktéři jsou role v aplikaci a uživatelé jsou často. Případy použití jsou obvykle akce nebo tříd Intent.

Například aplikaci pro sledování úloh může mít dva aktéři: *uživatele* a *Friend*. Uživatel může *vytvoření úlohy*, a *sdílet úlohu* s přáteli. V takovém případě sdílení úlohu a vytvoření úlohy jsou dva případy odlišné použití, které v kombinaci s aktéři, bude informovat o tom, co jste obrazovky budete muset vytvořit, a také jaké obchodní entity a logiku budou muset být vyvinutá.

Po zachycení odpovídající počet případů použití a aktéři je mnohem snazší zahájíte navrhování aplikace. Vývoj pak zaměřit na tom, jak vytvořit aplikaci, nikoli co aplikace je, nebo má provést.

### <a name="designing-mobile-applications"></a>Návrh mobilních aplikací

Po byly určeny funkce a funkce aplikace, v dalším kroku se snažíte vyřešit činnost koncového uživatele nebo UX start

#### <a name="ux-design"></a>Návrh UX

UX se obvykle provádí prostřednictvím wireframes nebo modelování pomocí jedné z dalších [návrh sadách](https://docs.microsoft.com/windows/uwp/design/downloads/). Modelování UX povolit UX třeba navrhnout bez nutnosti starat o aktuální návrh uživatelského rozhraní:

 [![](introduction-to-mobile-sdlc-images/balsamiq.png "UX se obvykle provádí prostřednictvím wireframes nebo pomocí nástrojů, jako je Balsamiq modelování")](introduction-to-mobile-sdlc-images/balsamiq.png#lightbox)

Při vytváření UX modelování, je důležité vzít v úvahu rozhraní pokyny pro různé platformy, které se zaměří na aplikaci. Aplikace by měla "pohodlné" na každou platformu. Oficiální návrhu pokyny pro každou platformu jsou:

1.   **Apple** -  [lidské Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/overview/themes/)
1.   **Android** – [pokyny návrhu](http://developer.android.com/design/index.html)
1.   **UWP** – [základy UWP návrhu](https://docs.microsoft.com/windows/uwp/design/basics/)

Například každá aplikace má jedná pro přepínání mezi oddílů v aplikaci. iOS používá karta panelu v dolní části obrazovky, Android používá pás karet v horní části obrazovky a UWP [Pivot či kartě](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tabs-pivot) zobrazení.

Kromě toho samotného hardwaru také určuje UX rozhodnutí. Například zařízení s iOS mít žádné fyzické *zpět* tlačítko a proto znamenat jedná navigační řadiče:

 ![](introduction-to-mobile-sdlc-images/01-navigation-controller.png "zařízení s iOS mít žádné fyzické tlačítko Zpět a proto znamenat jedná navigační řadiče")

Faktor formuláře navíc vliv UX rozhodnutí. Tablet má podstatně více nemovitosti a proto můžete zobrazit další informace. Často toho, co je více obrazovky na telefon se komprimují do jedné pro tablet:

 [![](introduction-to-mobile-sdlc-images/iphone-vs-ipad.png "Často se toho, co je více obrazovky na phone komprimované do jedné pro tablet")](introduction-to-mobile-sdlc-images/iphone-vs-ipad.png#lightbox)

A z důvodu velkého počtu velikostem odhlašování došlo, je často faktory střední velikost formuláře (někde mezi telefon i tablet), které může také chcete cílit.

#### <a name="user-interface-ui-design"></a>Návrh uživatelského rozhraní (UI)

Jakmile je určena UX, dalším krokem je vytvoření návrhu uživatelského rozhraní. Při UX je obvykle stačí černobílý modelování, do fáze návrhu uživatelského rozhraní je kde barvy, grafiky atd., jsou umístěna a dokončené. Obecně platí, nejoblíbenější aplikací mít odborníky v oblasti návrhu výdaje čas na dobrý návrh uživatelského rozhraní je důležitá.

Stejně jako u UX, je důležité si uvědomit, že každá platforma má ho je vlastní návrh jazyka, takže dobře navržených aplikace přesto může dojít vypadají na každou platformu:

 [![](introduction-to-mobile-sdlc-images/multiplatform-1.png "Dobře navržených aplikace může stále vypadají na každou platformu")](introduction-to-mobile-sdlc-images/multiplatform-1.png#lightbox)

### <a name="development"></a>Vývoj

Obvykle fázi vývoje spustí velmi brzy. Ve skutečnosti Jakmile představu některé zrání ve fázi koncepční/inspiraci, často prototyp pracovní vyvinutý která ověřuje funkce, předpoklady a pomůže získat představu o oboru práce.

Ve zbývající části kurzů k budete zaměříme převážně na fázi vývoje.

### <a name="stabilization"></a>Ustálení

Ustálení je proces práce si chyby ve vaší aplikaci. Ne jen z hlediska funkčnosti, například: "Jej dojde k chybě při klepnutí na toto tlačítko" ale také použitelnost a výkon. Je vhodné spustit ustálení velmi brzy v rámci procesu vývoje tak, aby během opravy může dojít předtím, než vstoupí nákladná. Obvykle se aplikace přejděte do *prototypu*, *Alpha*, *Beta*, a *Release Candidate* fázích. Jiné osoby definovat tyto jinak, ale budou obecně postupujte podle vzoru následující:

1.   **Prototyp** – aplikace je stále ve fázi testování konceptu a pouze základní funkce, nebo konkrétní části aplikace pracují. Hlavní chyb jsou k dispozici.
1.   **Alpha** – základní funkce je obecně doplňování kódu (vytvořen, ale není plně testováno). Hlavní chyby jsou stále přítomen, odlehlé funkce nemusí být stále existuje.
1.   **Beta** – většina funkce je nyní dokončen a došlo minimálně světla testování a opravě chyb. Hlavní známých problémů může být stále existuje.
1.   **Release Candidate** – všechny funkce je úplný a testované. Aby se zabránilo nové chyby tato aplikace je kandidátem na verzi zástupné.

Je nikdy příliš brzké zahájíte testování aplikace. Například pokud je ve fázi prototypu závažný problém, UX aplikace můžete stále upravit tak, aby ji bylo možné ošetřit. Pokud se problém s výkonem nenajde ve fázi alfa, je dostatečně včas k úpravě architekturu před velké množství kódu sestavený nad false předpoklady.

Má obvykle při přesunu aplikace v průběhu životního cyklu, dále na Otevřít další osoby si vyzkoušet, otestovat ji, poskytnout zpětnou vazbu, atd. Například prototypu aplikace může pouze zobrazen nebo zpřístupněn klíčových zúčastněných stran, zatímco release candidate aplikace mohou být distribuovány zákazníkům, kteří zaregistrovat projevily.

Časná testování a nasazení na relativně malý počet zařízení je obvykle nasazení přímo z vývojovém počítači dostatečná. Však jako cílová skupina rozšiřuje, to může být náročná. Jako takový existuje řada možností testovací nasazení odhlašování došlo které jednodušší tento proces tím, že se vám umožňuje vyzvat uživatele k testování fondu, verze sestavení prostřednictvím webu a poskytují nástroje, které umožňují pro zpětnou vazbu od uživatelů.

Pro účely testování a nasazení, můžete použít [aplikace Center](https://appcenter.ms/) pro nepřetržitě vytváření, testování, verzi a monitorování aplikací.

### <a name="distribution"></a>Distribuce

Jakmile aplikace byla stabilizována, je čas získat odhlašování do zástupné. Existuje několik možností jiný distribuční, v závislosti na platformu.

#### <a name="ios"></a>iOS

V úplně stejně, jako jsou distribuovány Xamarin.iOS a aplikace jazyka Objective-C:

1.   **Apple App Store** – společnosti Apple App Store je globálně dostupnou online aplikace úložiště, který je součástí systému Mac OS X přes iTunes. Je zdaleka metodu nejoblíbenější distribuce pro aplikace a umožňuje vývojářům na trh a distribuovat aplikace online s velmi malé úsilí.
1.   **Interní nasazení** – interní nasazení jsou určené výhradně pro interní distribuční podnikovým aplikacím, které nejsou k dispozici veřejně prostřednictvím App Storu.
1.   **Nasazení Ad-Hoc** – Ad-hoc nasazení slouží především pro vývoj a testování a umožňuje nasadit do omezeného počtu správně zřízené zařízení. Když nasadíte do zařízení pomocí Xcode nebo Visual Studio pro Mac, se označuje jako ad-hoc nasazení.

#### <a name="android"></a>Android

Všechny aplikace pro Android musí být podepsané před probíhá distribuce. Vývojáři podepsat svých aplikacích pomocí svůj vlastní certifikát chráněn privátní klíč. Tento certifikát může poskytovat řetěz pravosti, který přiřazuje vývojář aplikací k aplikacím, které má vývojáře vytvořené a vydání.
Ho musí být poznamenat, že při vývojový certifikát pro Android mohou být podepsány rozpoznaný certifikační autority, Většina vývojářů není opt využívat tyto služby a samoobslužné podepsat své certifikáty. Hlavním účelem pro certifikáty je rozlišit mezi různé vývojářům a aplikací.
Android používá tyto informace vám pomůže při vynucení delegování oprávnění mezi aplikací a součástí, které jsou spuštěné v operační systém Android.

Na rozdíl od jiných oblíbených mobilních platformách Android trvá velmi otevřete přístup k distribuci aplikací. Zařízení není uzamčené jeden schválené obchod.
Místo toho každý, kdo je zdarma vytvořit obchod s aplikacemi a nejvíce telefony Android povolit aplikace, které budou instalovány z těmito obchody třetích stran.

To umožňuje vývojářům potenciálně větší ještě složitější distribuční kanál pro své aplikace. [Google Play](https://play.google.com/store?hl=en) je oficiální obchod Google, ale existuje mnoho dalších. Několik oblíbených ty, které jsou:

1.  [AppBrain](http://www.appbrain.com/)
1.  [Obchod Amazon pro Android](http://www.amazon.com/mobile-apps/b?ie=UTF8&amp;node=2350149011)
1.  [Handango](http://www.handango.com/)
1.  [GetJar](http://www.getjar.com/)

#### <a name="uwp"></a>UWP 

Aplikace UWP jsou distribuovány prostřednictvím Microsoft Store. Vývojáři odesílat své aplikace na schválení, po jejímž uplynutí se objeví v úložišti. Další informace o publikování aplikací pro Windows, najdete v článku na UWP [publikovat](https://docs.microsoft.com/windows/uwp/publish/) dokumentaci.

## <a name="mobile-development-considerations"></a>Důležité informace pro vývoj mobilních řešení

Při vývoji mobilních aplikací není podstatně liší této vývoj tradiční webu na ploše z hlediska procesu nebo architekturu, existují některé aspekty znát.

### <a name="common-considerations"></a>Většinou je potřeba zvážit

#### <a name="multitasking"></a>Multitasking

Existují dva důležité problémy na multitasking (s více spuštěnými aplikacemi v jednou) na mobilním zařízení. Nejprve zadány nemovitosti omezené obrazovky, je obtížné současně zobrazit více aplikací. Proto na mobilních zařízeních jenom jedna aplikace může být v popředí najednou. Za druhé s více aplikací, které otevřené a provádění úloh můžete rychle použít až energie z baterie.

Každou platformu zpracovává multitasking jiným způsobem, který jsme vám prozkoumat za chvilku.

#### <a name="form-factor"></a>Faktor formuláře

Mobilní zařízení obvykle spadají do dvou kategorií, telefonů a tabletů, s několika zařízeními kříženého v rozmezí. Vývoj pro tyto faktory formuláře je obecně velmi podobné, ale navrhování aplikací u nich může být příliš neliší.
Telefony mít velmi omezený obrazovky místa a tablety, při větší, jsou stále mobilních zařízení pomocí méně místa na obrazovce než i většině přenosné počítače. Z toho důvodu byly navrženy ovládacích prvků uživatelského rozhraní mobilní platformu speciálně pro účinný na menší faktorech formuláře.

#### <a name="device-and-os-fragmentation"></a>Zařízení a fragmentace operačního systému

Je důležité vzít v úvahu různých zařízení v průběhu životního cyklu celý softwaru:

1.   **Koncepci a plánování** – mějte na paměti hardwaru a funkce se budou lišit od zařízení pro zařízení, aplikace, která závisí na některé funkce nemusí fungovat správně v určitých zařízení. Ne všechna zařízení mít například kamery, takže pokud vytváříte video zasílání zpráv na úrovni aplikace, možná bude moci přehrávání videa, ale nemusí provádět žádné některá zařízení.
1.   **Návrh** – při navrhování aplikace uživatelské prostředí (UX) věnujte pozornost poměrům stran různých obrazovek a velikosti mezi zařízeními. Kromě toho při navrhování aplikace uživatelské rozhraní (UI), měli zvážit různá rozlišení obrazovky.
1.   **Vývoj** – při použití funkce z kódu, přítomnost této funkce by měl vždycky být testována nejdřív. Třeba před použitím funkce zařízení, jako je například fotoaparátu, vždy dotaz operačního systému na přítomnost této funkce nejprve. Potom při inicializaci funkce a zařízení, zkontrolujte, zda žádost aktuálně podporované z operačního systému o zařízení a pak použít tato nastavení konfigurace.
1.   **Testování** – je velmi důležité k testování aplikace již v rané fázi a často na skutečné zařízení. To i v zařízeních pomocí stejné specifikace hardwaru se může lišit široce v jejich chování.

#### <a name="limited-resources"></a>Omezené prostředky

Mobilní zařízení získat více efektivní vždy, ale jsou stále mobilní zařízení, která mají omezené možnosti oproti počítače plocha nebo poznámkového bloku. Například stolní vývojáři obvykle nemusíte si dělat starosti o kapacitu paměti; slouží k nutnosti fyzické i virtuální paměti v velkým počty, zatímco u mobilních zařízení, můžete rychle využívat všechny dostupné paměti právě načtením několik vysoce kvalitní obrázky.

Kromě toho můžete aplikace náročné na prostředky procesoru, jako je například hry nebo rozpoznávání textu skutečně daně mobilní procesoru a nepříznivě ovlivnit výkon zařízení.

Kvůli takovéto je důležité, aby ale kódu a nasazovat včas a často pro skutečné zařízení k ověření odezvy.

### <a name="ios-considerations"></a>iOS aspekty

#### <a name="multitasking"></a>Multitasking

Multitasking velmi úzce řídí v iOS a existuje několik pravidel a chování, které aplikace musí odpovídat jiná aplikace vycházejí na popředí, jinak vaše aplikace bude ukončen v iOS.

#### <a name="device-specific-resources"></a>Prostředky pro konkrétní zařízení

V rámci příslušného formuláře faktoru může značně lišit hardwaru mezi různými modely. Například některá zařízení mají směřující dozadu fotoaparát, některé mít také fotoaparátu před přístupem a některé nemáte nic.

Některá starší zařízení (iPhone 3G a starší) i Nepovolit multitasking.

Z důvodu tyto rozdíly mezi modely zařízení je důležité zkontrolovat přítomnost funkce dřív, než se ji použít.

#### <a name="os-specific-constraints"></a>Omezení pro konkrétní operační systém

Pokud chcete mít jistotu, že aplikace budou reakce a zabezpečení, iOS vynucuje několik pravidel, které musí aplikace splňovat. Kromě pravidel ohledně multitasking existuje několik metod událostí, z které aplikace musí vracet určité množství času, v opačném případě, že bude získat ukončen v iOS.

Také vhodné poznamenat, aplikace spouštět v, která se označuje jako izolovaného prostoru, prostředí, které vynutí omezení zabezpečení, které omezují vaší aplikace, které je přístupné. Například aplikace může číst a zapisovat do vlastní adresáře, ale pokud se pokusí zapsat do jiného adresáře aplikace, bude ukončena.

### <a name="android-considerations"></a>Android aspekty

#### <a name="multitasking"></a>Multitasking

Multitasking v Android má dvě součásti; První je životní cyklus aktivity. Každý obrazovky v aplikaci pro Android je reprezentována aktivitu a není konkrétní sadu událostí, které dojít, když aplikace je umístěn na pozadí nebo dodává na popředí. Aplikace musí splňovat tohoto životního cyklu k vytvoření reakce, dobře behaved aplikací. Další informace najdete v tématu [životního cyklu aktivity](~/android/app-fundamentals/activity-lifecycle/index.md) průvodce.

Druhá součást, kterou multitasking v Android je použití služby.
Služby jsou dlouho běžící procesy, které existují nezávisle na aplikaci a se používají ke spouštění procesů sice aplikace na pozadí. Další informace najdete v článku [vytváření služeb](~/android/app-fundamentals/services/index.md) průvodce.

#### <a name="many-devices-and-many-form-factors"></a>Mnoho zařízení a mnoha faktorech formuláře

Google nemá uložit žádné omezení na zařízení, která můžete spustit operační systém Android. Tento otevřený zlepší výsledků v prostředí produktu nenaplnil velkého počtu různých zařízení s velmi jiný hardware, rozlišení obrazovky a poměr, zařízení funkce a možnosti.

Z důvodu extrémně fragmentaci zařízení se systémem Android se většina lidí zvolte nejoblíbenější 5 nebo 6 zařízení k navrhování a testování pro a určit jejich prioritu ty.

#### <a name="security-considerations"></a>Důležité informace o zabezpečení

Aplikace v operační systém Android všechny spuštění pod identitou distinct, izolované s omezenými oprávněními. Ve výchozím nastavení můžete provést aplikace velmi málo. Například bez zvláštní oprávnění aplikace nelze odeslání textové zprávy, určit stav telefonu nebo i přístup k Internetu! Přístup k těmto funkcím, aplikace musíte zadat v jejich souboru manifestu aplikace oprávnění, která se bude jako a v případě, že probíhá instalace; operační systém přečte tato oprávnění, upozorní uživatele, že aplikace požaduje tato oprávnění a pak umožňuje uživateli pokračovat nebo zrušit instalaci.
To je základním krokem v modelu Android distribuční kvůli otevřete aplikaci modelu úložiště, protože aplikace nejsou kurátorované způsob, jak jsou pro iOS, pro instanci. Seznam oprávnění aplikací najdete v tématu [Manifest oprávnění](http://developer.android.com/reference/android/Manifest.permission.html) odkaz na článek v Android dokumentaci.

### <a name="windows-considerations"></a>Důležité informace o systému Windows

#### <a name="multitasking"></a>Multitasking

Multitasking v UWP má dvě části: životního cyklu pro stránky a aplikace a procesy na pozadí. Každý obrazovky v aplikaci je instance třídy stránky, která má události související s prováděné aktivní nebo neaktivní (s zvláštní pravidla pro zpracování neaktivního stavu, nebo se "neplatné"). 

Druhá část poskytuje agentů na pozadí pro zpracování úloh, i v případě, že aplikace není spuštěná v popředí. 

#### <a name="device-capabilities"></a>Možnosti zařízení

I když je poměrně homogenní UWP hardware, jsou stále součásti, které jsou volitelné a proto vyžaduje speciální zvažování při kódování. Volitelné hardwaru schopnosti zahrnují fotoaparát, kompas a volný setrvačník. Je taky speciální třídu nedostatku paměti (256MB) vyžadující obzvláštní pozornost, nebo vývojáři mohou výslovný nesouhlas s podpory nedostatku paměti.

#### <a name="security-considerations"></a>Důležité informace o zabezpečení

Informace o důležité informace o zabezpečení v UPW, najdete v části [zabezpečení](https://docs.microsoft.com/windows/uwp/security/) dokumentaci.

## <a name="summary"></a>Souhrn

Tato příručka přiřadil Úvod do SDLC, protože se týká pro vývoj mobilních řešení. Je zavedená obecné požadavky pro vytváření mobilních aplikací a číslo specifické pro platformu faktorů, včetně návrh, testování a nasazení.

## <a name="related-links"></a>Související odkazy

- [Úvod do vývoje pro mobilní zařízení](~/cross-platform/get-started/introduction-to-mobile-development.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, Android](http://developer.xamarin.com/get-started-droid/)
- [Principy aplikací](~/cross-platform/app-fundamentals/index.md)
