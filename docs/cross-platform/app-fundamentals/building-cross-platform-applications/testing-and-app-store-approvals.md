---
title: Část 6 – testování a schválení obchodu s aplikacemi
ms.prod: xamarin
ms.assetid: 46E0578A-7EB9-C105-ABB0-A043E501F36B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 67f61da09861fac6f45faf80efde40302c05bfed
ms.sourcegitcommit: f52aa66de4d07bc00931ac8af791d4c33ee1ea04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/19/2018
---
# <a name="part-6---testing-and-app-store-approvals"></a>Část 6 – testování a schválení obchodu s aplikacemi

## <a name="testing"></a>Testování

Mnoho aplikací (i aplikace pro Android, na některé obchody) bude mít k předávání proces schvalování před publikováním; testování je důležité zajistit aplikace dosáhne trhu (let alone úspěšné s vašimi zákazníky). Testování mohou mít mnoho forem z jednotky úrovni developer testování pro správu testování verze beta napříč celou řadu hardwaru.

### <a name="test-on-all-platforms"></a>Test na všech platformách.

Jsou drobné rozdíly mezi .NET podporuje na Windows phone, tablet a stolních zařízení, a také omezení na iOS, která zabránila dynamický kód má být vygenerován za chodu. Buď plánování testování kód na více platforem při vývoji, nebo naplánovat dobu refactor a aktualizovat model část vaší aplikace na konci tohoto projektu.

Vždycky je dobrým zvykem pomocí emulátoru/simulátoru otestovat více verzí operačního systému a také možnosti/konfigurací různých zařízení.

Byste měli také otestovat na zařízeních jako v mnoha různých fyzický hardware jako.

#### <a name="devices-in-cloud"></a>Zařízení v cloudu

Mobilní telefon i tablet ekosystém roste vždy, takže je možné otestovat na stále rostoucí počet zařízení, které jsou k dispozici. Pokud chcete tento problém vyřešit několik služeb nabízí možnost vzdáleně řídit mnoha různých zařízení tak, aby aplikace lze nainstalovat a otestovat, aniž by museli přímo investovat do mnoha hardwaru.

[Testovací aplikace Center](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) nabízí snadný způsob, jak otestovat iOS a Android aplikace na stovky různých zařízení.

### <a name="test-management"></a>Správa testů

Při testování aplikací v rámci vaší organizace nebo správě beta program s externími uživateli, existují dva problémy:

- **Distribuce** – Správa procesu zřizování (hlavně u zařízení s iOS) a získávání aktualizované verze softwaru pro testery.
- **Zpětná vazba** – shromažďovat informace o použití aplikací a podrobné informace o na všechny chyby, které mohou nastat.


Existuje řada nápovědy služby tyto problémy řeší poskytuje infrastrukturu, která je integrovaná do vaší aplikace při úklidu a sestav o využití a chyb a také zjednodušení procesu zřizování, které vám pomůžou registrace a správa testery a jejich zařízení .

[Centrum aplikace Visual Studio](/appcenter/) nabízí řešení těchto problémů, zadáním distribučních zkušební verze, hlášení chyb a informace o využití sofistikované aplikace.

### <a name="test-automation"></a>Test automatizace

Xamarin [UITest](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) slouží k vytváření automatizovaných uživatelského rozhraní test skripty, které můžete spustit místně nebo nahrán do [testovací aplikace Center](https://docs.microsoft.com/appcenter/test-cloud/).

## <a name="unit-testing"></a>Testování částí

### <a name="touchunit"></a>Touch.Unit

Xamarin.iOS zahrnuje testování částí rozhraní nazývá Touch.Unit, který následuje styl JUnit nebo NUnit zápis testů.

Odkazovat na našem [jednotkové testování v Xamarin.iOS](~/ios/deploy-test/touch.unit.md) dokumentaci podrobné informace o zápis testů a systémem Touch.Unit.

### <a name="andrunit"></a>Andr.Unit

Je ekvivalentní open source nástroje Touch.Unit pro Android názvem Andr.Unit. Si můžete stáhnout z [githubu](https://github.com/spouliot/Andr.Unit) a přečtěte si o tomto nástroji [ @spouliotna blogu](http://spouliot.wordpress.com/2011/10/30/andr-unit-joins-the-family/).

## <a name="app-store-approvals"></a>Schválení obchodu s aplikacemi

Apple a Microsoft fungovat pouze úložiště na jejich platformy: na App Storu a Marketplace v uvedeném pořadí. Jak uzamčení svá zařízení a implementujte proces zkontrolujte přísných aplikace pro řízení kvality aplikace, které jsou k dispozici ke stažení. Otevřete povaze na Android znamená, že existuje řada možností úložiště od Google Play, která je k dispozici a nemá žádné kontrolní proces Appstore Amazon společnosti pro Android a hardwaru ve snaze jako Samsung aplikace, které mají omezenou více distribučních a implementovat proces schvalování.

Čekání na aplikace mají být zkontrolovány může být velmi stressful – firmy vlivů často znamená to, že v žádostech o schválení s velmi malé okraj došlo k chybě před datem "cílové" spuštění. Samotný proces může trvat až dva týdny a není nutně transparentní: je omezená zpětnou vazbu na průběh vaší aplikace. dokud nebude nakonec odmítl nebo schválení. Odmítání může zahrnovat chybí marketing okno příležitost, zejména v případě, že se stane více než jednou a týdny předat mezi původní data spuštění a, když je aplikace nakonec schváleny.

### <a name="be-prepared"></a>Připravte se

První část Rady: naplánovat spuštění vaší aplikace a předem a vyrovnání možné odmítání a opětovné odeslání. Každý úložiště vyžaduje k vytvoření účtu před odesláním aplikace – to co nejdříve.
Při registraci Google Play pouze trvá několik minut, pokud vaše aplikace jsou volné, získá proces mnohem složitější, pokud jsou prodejem a musí zadat bankovnictví a daňové informace. Apple a procesy společnosti Microsoft jsou mnohem složitější než Google, může to trvat týden i další účet schválení tak zohlednit tuto chvíli do spuštění plánu.

Jakmile schválí svůj účet, jste připravení odeslat aplikace. Samotný proces odeslat aplikace je popsaná v následující dokumentaci:

- [Publikování do společnosti Apple iOS App Storu](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Příprava aplikace na Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md)
- Navštivte Windows vývojáři [Centrum vývojářů pro Windows](https://developer.microsoft.com/en-us/windows/windows-apps) informace o odesílání svoje aplikace.

Zbytek této části popisuje věcí, které byste měli vzít v úvahu zajistit, že aplikace je schválená bez jakékoli hiccups.

### <a name="quality"></a>Kvalita

Vyznívá zřejmé, ale aplikace bude získat často odmítnuta, protože nesplňují úroveň kvality,:, to je důvod, proč kurátorované úložiště mají proces schvalování na prvním místě!

Dojde k chybě jsou běžné důvod zamítnutí. Pokud je velmi snadné, aby vaše aplikace havárií, má zaručit zamítnutí. Většina vývojářů nemáte odesílat své aplikace s tím, který se bude k chybě, ale často dělají. Testování aplikace důkladně před odesláním ji, zaměřením, ne jenom na tak, že vše funguje, ale také, které zpracování běžných scénářů mobilní chyba například problémy se sítí a omezení prostředků, jako je paměť nebo úložný prostor. Pomocí simulátoru i fyzického zařízení k testování – bez ohledu na to, jak dobře spuštění kódu v simulátoru, můžete pouze zařízení ukazují skutečné výkonu aplikace. Použijte jako v mnoha různých zařízení, jak můžete najít a zařazení tým beta testery, pokud můžete – služeb třetích stran můžou usnadnit správu beta rozdělení a zpětné vazby.

Všechny mobilní operační systémy se ukončit aplikaci, která není dostatečně rychle začít. Délka dobu povolenou se liší, ale obecně by měla aplikace zaměřte přizpůsobivý za několik sekund a pomocí úlohy na pozadí provádět veškerou práci, kterou bude trvat déle. Aplikace, které trvá příliš dlouho načíst nebo jsou není dostatečně přizpůsobivý běžně používané budou odmítnuty. Pokud něco se děje na pozadí nebo aplikace se zobrazí selhání a znovu získat odmítl vždy poskytovat zpětnou vazbu od uživatelů.

### <a name="check-your-edge-cases"></a>Zkontrolujte vaše případy Edge

Běžné depeše pro vývojáře se nedaří adresu hraniční případy, především těch, které vyžadují opětovná konfigurace jeho simulátoru nebo zařízení k testování správně. Může být snadno zapomenete, Ne každé zákazníka bude "Povolit" aplikaci pro přístup k jejich umístění, protože po vývojáře přijal žádost jednou, že budete nikdy vyzváni znovu. Oprávnění a využití sítě jsou konkrétně zaměřené na během procesu schvalování, což znamená, že malé dohledu v těchto oblastech může mít za následek odmítnutí.

V následujícím seznamu je to dobrý výchozí bod pro kontrolu edge případů, které může být provedena:

- **Zákazníci mohou 'odepřít' přístup ke službám** – hlavně v iOS, přístup k datům, například informace geografického umístění, je zadáváno, pouze když uživatel uděluje oprávnění k vaší aplikaci. Testery aplikace často znovu nainstalovat aplikaci v počátečního stavu a zakáže všechny žádosti o oprávnění k zajištění, že aplikace správně chová. Přepnutí na oprávnění a vypnout ověřte správné chování, jako jsou zákazníci změnit své rozhodnutí.
- **Zákazníci se everywhere** – Nepředpokládejte, že aplikace bude použit pouze ve městě nebo zemi, kde byla vyvinuta! Všechny kód, který funguje s GPS souřadnice, hodnoty data a času a měny může vliv podle umístění a nastavení národního prostředí zákazníka. Všechny platformy nabídka simulátoru, které umožňují zadat jiné umístění a národní prostředí – ji použít k testování umístění v jiných hemispheres a jazykové verze, které jinak formátování kalendářních dat a měny. Hodnoty zeměpisné šířky a délky mohou být kladná nebo záporná, oddělovač desetinných míst může být období nebo čárkou a kalendářních dat může být naformátovaný velkého způsoby - nakládat s ním!
- **Pomalé připojení k síti** – vývojáři aplikací často fungovat v "ideální svět" rychlé, vždy pracovat připojení k síti, která samozřejmě není velká písmena v reálném světě. Testování s pomalým síťovým připojením (například nízký 3 G připojení), a také s přístupem k žádné síti je důležité zajistit, že nemáte dodávat buggy aplikace. Proces schválení bude vždy zahrnovat testu se zařízením v režim v letadle, tak zajistit, že testování které si sami.
- **Hardwaru se liší** – nezapomeňte otestovat na nejstarší, nejpomalejší hardwaru, které chcete podporovat. Existují dva aspekty, které mohou ovlivnit vaše aplikace: výkon, což může být v nepoužitelném na starší zařízení a podpora pro funkce hardwaru, jako je například fotoaparátu, mikrofon, GPS, volný setrvačník nebo jiné volitelné součásti. Aplikace by měla snížit řádně (a ne zhroutí) není k dispozici při komponentu.

### <a name="guidelines-are-more-than-just-a-guide"></a>Pokyny jsou více než jen 'průvodce.

Apple je proslulá se striktní o dodržování pokynů k jejich Human Interface Guidelines jako jeden z klíčů síly jejich platformy je konzistence (a dosahovaný nárůst použitelnost). Microsoft trvá podobný postup s aplikací pro systém Windows implementace rozhraní stylu Metro. Proces schválení pro obě platformy bude zahrnovat ještě vyhodnocujeme, přijímá filosofie relevantní návrhu aplikace.

Není to znamená, že uživatelské rozhraní inovace není podporován nebo podporovat, ale existují některé aspekty "právě neměli byste" nebo aplikace budou odmítnuty.

V systému iOS jedná se o zneužití předdefinované ikony nebo pomocí jiné dobře zavedené metaphors-konzistentním způsobem; například pomocí ikony 'vytvořit' pro jakoukoli jinou hodnotu než vytváření nového obsahu.

Vývojáři Windows by měla být podobně opatrní; Obvyklou chybou se nedaří správně podporu hardwaru zpět tlačítko podle pokynů společnosti Microsoft.

Doporučte vaší Designer ke čtení a postupujte podle pokynů návrhu pro každou platformu.

### <a name="implementing-platform-specific-features"></a>Implementace funkce specifické pro platformu

Co jsou trochu přísnější při rozhodování o implementaci služby specifické pro platformu, zejména v systému iOS. Abyste se vyhnuli automatické odmítnutí společností Apple, jsou některá pravidla podle následující funkce iOS:

- **Nákupy v aplikaci** – aplikace musí implementovat není externí platebních mechanismy pro digitální produkty, včetně Měna ve hře, funkce aplikací, katalogu odběry a mnoho dalších. aplikace pro iOS, musíte použít službu založenou na iTunes společnosti Apple pro tento typ funkce. Je zadní vrátka - aplikace jako čtečky Kindle a některé aplikace na základě předplatného umožňují zakoupit jinde obsah, který získá připojen na "účet", které je dostupné prostřednictvím aplikace, ale v takovém případě nesmí obsahovat aplikace odkazy nebo odkazuje na se na aplikace zakoupit procesu (nebo znovu, budete odmítl).
- **zálohování do úložiště iCloud** – s nástupem Icloudu společnosti Apple kontroloři jsou mnohem víc striktní o tom, jak aplikace používat úložiště (zkontrolujte příjemný vzdálené možnosti zálohování zákazníka). Aplikace, může získat zamítnutí nakládání s zálohování může úložišti, odpovídajícím způsobem tak pomocí složky mezipaměti a postupujte podle Apple je jiné pokyny související s úložištěm.
- **Newsstand** – novinách a katalogu aplikací jsou skvělý přizpůsobit pro Newsstand společnosti Apple, ale aplikace musí implementovat alespoň jeden automatického obnovení předplatného a podporu pozadí stahování schválení.
- **Mapuje** – je stále chcete přidat do mobilních mapy překryvy a další funkce, ale buďte opatrní není nesrozumitelné mapy, kredity' informace (jako je logo Google v iOS5) jako tak bude mít za následek odmítnutí.

### <a name="manage-your-metadata"></a>Spravovat Metadata

Kromě zřejmé technických problémů, které mohou způsobovat v aplikaci odmítnuta jsou uvedeny některé další menší aspektů vaší odeslání, který může mít za následek zamítnutí, zejména kolem metadata (popis, klíčová slova a marketing obrázků), které odešlete s vaší aplikací pro zobrazení v App Storu nebo Marketplace.

- **Obrazů** – postupujte podle pokynů platformu pro ikon aplikací a ukládat obrázky. Nepoužívejte značkou bitové kopie, jsme viděli, že aplikace získat odmítnuta, protože jejich ikony vybrané kreslení zařízení typu iPhone!
- **Ochranné známky** – nepoužívejte žádné ochranné známky než vlastní. Aplikace pro zmínit, ochranné známky v popis aplikace nebo dokonce v klíčová slova na Apple App Store byl odepřen.
- **Popis** – použít slovo "beta, ani žádným způsobem znamenat, že aplikace není připraven pro prime čas. Nezveřejňujte jiné mobilní platformy (i v případě, že je vaše aplikace a platformy). Co je nejdůležitější zkontrolujte, zda že aplikace nemá přesně co sdělení, že se nepodporuje. Pokud uvedete spoustu funkcí v popisu, by měl lépe být zřejmé jejich používání těchto funkcí nebo získáte zamítnutí "uvedených v popisu aplikace funkce není implementována".

Uveďte tolik úsilí do metadat aplikace do vývoje a testování. Aplikace získat zamítnuto menších porušení v metadatech tak, aby byl smysl, čas, získat správné.

### <a name="app-stores-not-for-everyone"></a>Obchody s aplikacemi: Není pro všechny uživatele

Hlavním cílem tohoto úložiště na jednotlivých platformách je příjemce distribuce - schopnost přístupu jako mnoho zákazníků míře. Nicméně ne všechny aplikace jsou cíleny na příjemci, není rychle rostoucí základní interní a extranetu jako aplikací, které vyžadují omezeně distribuovatelných zaměstnanci, dodavatelů nebo zákazníků. Tyto aplikace nejsou "pro prodej" a není nutné schválení od vývojáře distribuce ovládacích prvků uzavřené skupinu uživatelů.
Podpora pro tento typ nasazení se liší podle platformy.

Android nabízí flexibilitu nejvíce v tomto ohledu: aplikace je možné nainstalovat přímo z adresy URL nebo e-mailových příloh (tak dlouho, dokud to umožňuje konfiguraci zařízení). To znamená, že je jednoduchá vytvořit a distribuovat interní podnikové aplikace nebo publikování aplikací pro konkrétní zákazníky nebo dodavatelů.

Apple nabízí možnost interní nasazení pro vývojáře, které jsou zaregistrované v knihovně iOS Developer Enterprise Program, který obchází procesu schvalování App Store a umožňuje společnostem distribuovat interní aplikace pro své zaměstnance.
Bohužel tuto licenci neřeší potřebu distribuce aplikací extranetu jako do jiných skupin uzavřené zákazníků nebo dodavatelů. [Enterprise (a Ad Hoc) nasazení](~/ios/deploy-test/app-distribution/ipa-support.md)

### <a name="app-store-summary"></a>Souhrn obchodu s aplikacemi

Proces kontroly nemusí být jednoduché, ale stejně jako ostatní životního cyklu může pomoci zajistit úspěch s některými plánování a zaměřit na podrobnosti. Pochází k několika jednoduchých kroků: čtení a pochopení pokyny uživatelské rozhraní, musí splňovat, postupujte podle pravidla při implementaci funkce specifické pro platformu, důkladně otestovat (pak test některé další) a nakonec nezapomeňte metadata aplikace Před odesláním je správná.

Jeden poslední slovo Rady pro vývojáře publikování na webu Google Play: nedostatek procesu schvalování může působí, jako je výrazně zjednodušuje úlohu - vašim zákazníkům bude ji však i další náročné než kontrolní tým. Postupujte podle těchto pokynů, jako když vaše aplikace může získat zamítnutí, jinak budou vaši zákazníci provádění odmítat.
