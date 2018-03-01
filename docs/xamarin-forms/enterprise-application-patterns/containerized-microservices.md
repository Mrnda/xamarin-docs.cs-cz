---
title: "Kontejnerizované Mikroslužeb"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5872ad92-04e0-4f1a-9691-79d5602f5683
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 3ecbd5a301a64417ab5fb27bd8632b6d9790a7ac
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="containerized-microservices"></a>Kontejnerizované Mikroslužeb

Vývoj aplikací klient server má za následek zaměřená na vytváření vrstvené aplikace, které používají konkrétní technologie v jednotlivých úrovních. Takové aplikace jsou často označovány jako *monolitický* aplikace a jsou zabalené do hardwaru předem škálovat zatížení ve špičce požadavků. Hlavní nevýhody tohoto přístupu vývoj jsou úzkou párování mezi součástmi v jednotlivých vrstvách, že nelze snadno škálovat jednotlivé komponenty a náklady na testování. Jednoduché aktualizace může mít nepředpokládaného dopady na zbytek vrstvě, a proto jeho celý vrstvy přezkoušena a nasazovat vyžaduje změnu určité součásti aplikace.

Zejména týkající se v stáří cloudu, je jednotlivých součástí nebylo možné snadno škálovat. Monolitický aplikace obsahuje funkce specifické pro doménu a obvykle dělení funkční vrstvy například front-endu, obchodní logiku a data úložiště. Monolitický aplikace je škálovat klonováním celé aplikace do více počítačů, jak ukazuje obrázek 8-1.

![](containerized-microservices-images/monolithicapp.png "Škálování přístup monolitický aplikace")

**Obrázek 8-1**: monolitický aplikace škálování přístup

## <a name="microservices"></a>Mikroslužeb

Mikroslužeb nabízejí jiný přístup k vývoji aplikací a nasazení, který je vhodný přístup k flexibility, měřítko a spolehlivost požadavky moderní cloudové aplikace. Aplikace mikroslužeb je rozložit na nezávislé součásti, které vzájemně spolupracují a poskytovat celkové funkcí aplikace. Mikroslužbu termín klade důraz, že aplikace by měla být složené služeb dostatečně malé, aby odrážela nezávislé otázky, tak, aby každý mikroslužbu implementuje jedné funkce. Kromě toho má každý mikroslužbu dobře definovaný kontrakty, aby mohli ostatní mikroslužeb komunikovat a sdílet data s ním. Typické příklady mikroslužeb zahrnují nákupních košíků, inventarizaci zpracování, nákupu subsystémy a zpracování platby.

Mikroslužeb můžete Škálováním na více systémů nezávisle, porovnání s obří monolitický aplikace, které společně škálování. To znamená, že určitou funkční oblast, která vyžaduje další zpracování napájení nebo síťové šířky pásma pro podporu poptávky, je možné rozšířit místo zbytečně škálování na více systémů jiných oblastí aplikace. Obrázek 8-2 ukazuje tento přístup, kde jsou mikroslužeb nasazená a škálovat nezávisle, vytváření instancí služeb mezi počítači.

![](containerized-microservices-images/microservicesapp.png "Škálování přístup Mikroslužeb aplikace")

**Obrázek 8-2**: Mikroslužeb aplikace škálování přístup

Mikroslužbu škálování může být téměř okamžité, umožní aplikaci přizpůsobit změnou zatížením. Například jeden mikroslužbu ve funkci směřujících webové aplikace může být pouze mikroslužbu v aplikaci, která je potřeba horizontální navýšení kapacity pro zpracování další příchozí přenosy.

Pro vyrovnávání zatížení sítě, bezstavové vrstvy s sdílené externí úložiště k ukládání dat je klasického modelu pro škálovatelnost aplikace. Stavová mikroslužeb spravovat jejich vlastní trvalé dat, obvykle je ukládání lokálně na serverech, na kterých jsou umístěny, aby se zabránilo režijní náklady na síť přístup a složitost mezi služba operations. To umožňuje nejrychlejší možné zpracování dat a může eliminovat potřebu pro ukládání do mezipaměti systémy. Kromě toho škálovatelné stavová mikroslužeb obvykle rozdělení dat mezi jejich instance, ke správě velikosti a přenos propustnost dat nad rámec, který může podporovat jeden server.

Mikroslužeb také podporují nezávislé aktualizace. Tato volné párování mezi mikroslužeb poskytuje rychlé a spolehlivé aplikace vývoj. Svou povahou nezávislé, distribuovaný podporuje kumulativní aktualizace, kde bude v každém okamžiku aktualizovat jenom podmnožinu instancí jedné mikroslužby. Proto pokud je zjištěn problém, buggy aktualizace můžete vrácena zpět, než všechny instance aktualizovat pomocí vadný kódu nebo konfigurace. Podobně mikroslužeb obvykle používají verze schématu, tak, aby klienti v tématu konzistentní verzi při aktualizace jsou aplikovány, bez ohledu na to, které mikroslužbu instance předávaných s.

Proto mikroslužbu aplikace mají mnoho výhod monolitický aplikace:

-   Každý mikroslužbu je poměrně malý, snadnou správu a vývoj.
-   Každý mikroslužbu můžete vyvíjí a nasadit nezávisle na jiné služby.
-   Každý mikroslužbu lze škálovat na více systémů nezávisle. Například služba katalogu nebo nákupní košík služby pravděpodobně možné upraveným více než služby řazení. Proto výsledné infrastruktury bude efektivněji využívat prostředky při zmenšování měřítka.
-   Každý mikroslužbu izoluje všechny problémy. Například pokud nastane problém ve službě ho ovlivní jenom dané služby. Další služby můžete nadále zpracovávat požadavky.
-   Každý mikroslužbu můžete použít nejnovější technologie. Protože mikroslužeb autonomní a spustit-souběžného, nejnovější technologie a architektury lze použít, místo vynucené použití představuje starší rozhraní, které by mohly používat monolitický aplikace.

Mikroslužbu na základě řešení má však také možné nevýhody:

-   Výběr způsobu oddílu aplikace do mikroslužeb může být náročné, protože každý mikroslužbu musí být zcela autonomního, klient server, včetně odpovědnost za její zdroje dat.
-   Vývojáři musí implementovat komunikace mezi službami, který přidá složitost a latence k aplikaci.
-   Jednotlivé transakce mezi více mikroslužeb obvykle nejsou možné. Proto musí obchodní požadavky zapojení konzistence typu případné mezi mikroslužeb.
-   V produkčním prostředí je provozní složitost v nasazení a správě systému dojde k ohrožení nezávislé služby.
-   Přímé komunikaci klienta mikroslužbu může ztížit Refaktorovat smluv mikroslužeb. Například v čase jak systému rozdělena do služby může být nutné změnit. Služba jednotného může rozdělit na dvě nebo více služeb a dvě služby může sloučení. Pokud klienti komunikují přímo s mikroslužeb, můžete tento refaktoringu pracovní porušit kompatibilitu s klientské aplikace.

## <a name="containerization"></a>Rozdělení do kontejnerů

Rozdělení do kontejnerů je přístup k vývoji softwaru, ve kterém aplikace jeho verzí sady závislosti a jeho konfigurace prostředí abstrahované jako soubory manifestu nasazení balíčku a jako obrázek na kontejner, testovat jako jednotku, a nasadit operační systém hostitele.

Kontejner je izolovaný, prostředků řízené a přenosné provozní prostředí, kde aplikace může pracovat bez zásahu prostředky z jiných kontejnerů nebo hostitele. Proto kontejner vypadá a funguje jako nově nainstalovaný fyzický počítač nebo virtuální počítač.

Existuje mnoho podobnosti mezi kontejnery a virtuálních počítačů, jak ukazuje obrázek 8-3.

![](containerized-microservices-images/containersvsvirtualmachines.png "Škálování přístup Mikroslužeb aplikace")

**Obrázek 8-3**: porovnání virtuálních počítačů a kontejnery

Kontejner běží operační systém, systém souborů a je přístupný prostřednictvím sítě, jako by šlo o fyzický nebo virtuální počítač. Technologie a koncepty používané kontejnery jsou však velmi liší od virtuálních počítačů. Virtuální počítače zahrnují úplné hostovaného operačního systému, aplikace a požadované závislosti. Kontejnery zahrnovat aplikace a jeho závislé součásti, ale sdílet operační systém s další kontejnery s izolovaných procesech systémem hostitelského operačního systému (kromě zajištění dostatečného kontejnery technologie Hyper-V, které spustit uvnitř speciální virtuální počítač na kontejneru). Proto kontejnery sdílet prostředky a obvykle vyžadují méně prostředků než virtuální počítače.

Výhodou kontejner přístup orientovaný na vývoj a nasazení je eliminuje většinu problémy, které vznikají z nastavení nekonzistentní prostředí a problémy, které jsou s nimi. Kromě toho kontejnery umožňují funkčnost škálování rychlé aplikace ve vytváření instancí nové kontejnery podle potřeby.

Klíčové koncepty při vytváření a práci s kontejnery jsou:

-   Kontejner hostitele: Fyzický nebo virtuální počítač nakonfigurovaný tak, aby kontejnery hostitele. Kontejner hostitele spustí jeden nebo více kontejnerů.
-   Obrázek kontejneru: Bitovou kopii se skládá z spojení vrstveného systémy sebe a je základem kontejner. Obrázek nemá stavu a nikdy změní, když bude nasazen pro různá prostředí.
-   Kontejner: Kontejner je instance runtime bitové kopie.
-   Bitové kopie operačního systému kontejneru: Kontejnery byly nasazeny pomocí bitové kopie. Image operačního systému kontejneru je první vrstvu v potenciálně mnoho vrstvy bitové kopie, které tvoří kontejner. Operační systém kontejneru se nedá změnit a nemůže být upraven.
-   Kontejner úložiště: Pokaždé, když se vytvoří bitovou kopii kontejneru, bitové kopie a jeho závislosti jsou uloženy v místním úložišti. Tyto bitové kopie lze opětovně použít mnohokrát na hostiteli kontejneru. Kontejner bitové kopie může být také uložen v registru veřejných nebo privátních, jako například [úložiště Docker Hub](https://hub.docker.com/)tak, aby bylo možné mezi hostiteli jiný kontejner.

Podniky jsou stále přijetí kontejnery při implementaci mikroslužbu aplikace založené na a Docker se stala implementace standardní kontejneru, která přijal většina softwaru platformy a dodavatelé cloudu.

Odkaz na aplikaci eShopOnContainers Docker využívá k hostování čtyři kontejnerizované mikroslužeb back-end, jak ukazuje obrázek 8-4.

![](containerized-microservices-images/microservicesarchitecture.png "eShopOnContainers odkazovat mikroslužeb back-end aplikace")

**Obrázek 8-4**: eShopOnContainers odkazovat mikroslužeb back-end aplikace

Architektura služeb back-end v referenční aplikace je rozložit na více autonomních systémech podřízených ve formě spolupráce mikroslužeb a kontejnerů. Každý mikroslužbu poskytuje jedné oblasti funkcí: služby identity, katalogu služby, řazení služby a služby nákupní košík.

Každý mikroslužbu má svou vlastní databázi, díky kterému jej být plně odpojené od jiných mikroslužeb. V případě potřeby konzistenci mezi databází z různých mikroslužeb dosáhnout pomocí událostí na úrovni aplikace. Další informace najdete v tématu [komunikace mezi Mikroslužeb](#communication_between_microservices).

Další informace o odkaz na aplikaci najdete v tématu [Mikroslužeb .NET: architektura pro kontejnerové aplikace .NET](https://aka.ms/microservicesebook).

<a name="communication_between_client_and_microservices" />

## <a name="communication-between-client-and-microservices"></a>Komunikace mezi klientem a Mikroslužeb

EShopOnContainers mobilní aplikace komunikuje s kontejnerizované mikroslužeb back-end pomocí *přímé klienta mikroslužbu* komunikaci, která se zobrazí v obrázek 8-5.

![](containerized-microservices-images/directclienttomicroservicecommunication.png "Škálování přístup Mikroslužeb aplikace")

**Obrázek 8-5**: přímé komunikaci klienta mikroslužbu

Mobilní aplikace s přímou komunikaci klienta mikroslužbu, odešle požadavky každý mikroslužbu přímo přes svůj veřejný koncový bod s jiný port TCP za mikroslužby. V produkčním prostředí by koncový bod obvykle mapují mikroslužbu Vyrovnávání zatížení, který rozděluje požadavky na k dispozici instance.

> [!TIP]
> Zvažte použití brány komunikace rozhraní API. Při vytváření velké a komplexní mikroslužbu na základě aplikací, ale je více než dostačující pro malá aplikace, může mít přímou komunikaci klienta mikroslužbu nevýhody. Při navrhování velké mikroslužbu na základě aplikace s desítkami mikroslužeb, zvažte použití brány komunikace rozhraní API. Další informace najdete v tématu [Mikroslužeb .NET: architektura pro kontejnerové aplikace .NET](https://aka.ms/microservicesebook).

<a name="communication_between_microservices" />

## <a name="communication-between-microservices"></a>Komunikace mezi Mikroslužeb

Mikroslužeb na základě aplikace je distribuovaný systém, potenciálně spouštění na více počítačích. Každá instance služby je obvykle proces. Proto služby musí komunikovat pomocí protokol komunikaci mezi procesy, jako je například HTTP, TCP, rozhraní Advanced zpráv služby Řízení front Protocol (AMQP) nebo binární protokoly, v závislosti na povaze jednotlivých služeb.

Tyto dva přístupy společné pro komunikaci mikroslužbu mikroslužby jsou že HTTP komunikaci na bázi REST při dotazování na data a lightweight asynchronní zasílání zpráv při komunikaci aktualizace napříč více mikroslužeb.

Asynchronní zasílání zpráv na základě událostmi řízené komunikace je velmi důležité při šíření změny napříč více mikroslužeb. S tímto přístupem mikroslužbu publikuje událost v případě, že něco, co zajímavé se stane, například při aktualizaci obchodní entity. Tyto události přihlášení k odběru jiných mikroslužeb. Potom událost přijetí mikroslužbu aktualizuje svůj vlastní obchodní entity, které pak může vést k další události je publikován. To publikování a odběru funkce se obvykle dosáhne s sběrnici událostí.

Sběrnici událostí umožňuje komunikaci mezi mikroslužeb, bez nutnosti součásti, které budou zajímat, explicitně mezi sebou, jak ukazuje obrázek 8-6 publikování a odběru.

![](containerized-microservices-images/eventbus.png "Publikování a odběru s sběrnici událostí")

**Obrázek 8 – 6:** publikování a odběru s sběrnici událostí

Z hlediska aplikaci sběrnici událostí je jednoduše publish-přihlášení k odběru kanálu zveřejňovány prostřednictvím rozhraní. Ale způsob, jakým je implementováno sběrnici událostí se může lišit. Například použít implementaci sběrnice událostí RabbitMQ, Azure Service Bus nebo jiné služby sběrnicích například NServiceBus a MassTransit. Obrázek 8-7 znázorňuje, jak sběrnici událostí se používá v eShopOnContainers referenční aplikace.

![](containerized-microservices-images/microservicesarchitecturewitheventbus.png "Asynchronní komunikaci založeného na událostech v aplikaci odkaz")

**Obrázek 8-7:** asynchronní komunikaci založeného na událostech v aplikaci odkaz

Událost sběrnici eShopOnContainers implementovaná pomocí RabbitMQ, poskytuje jeden mnoho asynchronní publikování a odběru na funkce. To znamená, že po publikování události, může existovat více odběrateli naslouchání pro jednu událost. Obrázek 8-9 ukazuje tento vztah.

![](containerized-microservices-images/eventdrivencommunication.png "Komunikace jeden mnoho")

**Obrázek 8-9**: jeden mnoho komunikace

Tento přístup k více komunikace používá události k implementaci obchodní transakce, které jsou rozmístěny v několika služeb, zajištění konzistence typu případné mezi službami. Závěrečné konzistentní transakce se skládá z řady distribuované kroky. Proto příkaz UpdateUser přijetí mikroslužbu profilu uživatele se aktualizuje podrobnosti uživatele ve své databázi a publikuje událostí UserUpdated ke sběrnici událostí. Mikroslužbu košík a řazení mikroslužbu mít předplatné přijmout tato událost a v odpovědi aktualizovat své informace kupujících v příslušných databází.

> [!NOTE]
> Událost sběrnici eShopOnContainers implementovaná pomocí RabbitMQ, je určena pro použití pouze jako testování konceptu. Pro produkční systémy považovat implementace sběrnice alternativní událostí.

Informace o implementaci sběrnice událostí najdete v tématu [Mikroslužeb .NET: architektura pro kontejnerové aplikace .NET](https://aka.ms/microservicesebook).

## <a name="summary"></a>Souhrn

Mikroslužeb nabízí přístup k vývoji aplikací a nasazení, který je vhodný flexibility, měřítko a spolehlivost požadavkům moderní cloudové aplikace. Jedním z hlavní výhody mikroslužeb je, aby mohly být upraveným nezávisle, což znamená, že konkrétní funkční oblast je možné rozšířit vyžadující další zpracování napájení nebo síťové šířky pásma pro podporu poptávky, bez zbytečně škálování oblastí aplikace, která k těmto problémům zvýšené poptávky.

Kontejner je izolovaný, prostředků řízené a přenosné provozní prostředí, kde aplikace může pracovat bez zásahu prostředky z jiných kontejnerů nebo hostitele. Podniky jsou stále přijetí kontejnery při implementaci mikroslužbu aplikace založené na a Docker se stala implementace standardní kontejneru, která přijal většina softwaru platformy a dodavatelé cloudu.


## <a name="related-links"></a>Související odkazy

- [Stáhnout elektronická kniha (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (Githubu) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
