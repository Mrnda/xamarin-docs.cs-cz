---
title: Úvod do průběžnou integraci s funkcí Xamarin
description: Průběžnou integraci je software inženýrství postupem, ve kterém automatizované sestavení zkompiluje a volitelně otestuje aplikace při přidání nebo změně vývojáři v úložišti řízení projektu verze kódu. Tento článek popisuje jsou obecné koncepty průběžnou integraci a některé z možností k dispozici pro nepřetržitou integraci s projekty Xamarin.
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 07/19/2017
ms.openlocfilehash: 017691ece68f979eea1627c0442f49018d5742fb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Úvod do průběžnou integraci s funkcí Xamarin

_Průběžnou integraci je software inženýrství postupem, ve kterém automatizované sestavení zkompiluje a volitelně otestuje aplikace při přidání nebo změně vývojáři v úložišti řízení projektu verze kódu. Tento článek popisuje jsou obecné koncepty průběžnou integraci a některé z možností k dispozici pro nepřetržitou integraci s projekty Xamarin._

Je běžné v projektech softwaru paralelně práci vývojářům. V určitém okamžiku je potřeba integrovat všechny tyto paralelní datové proudy práce do jednoho základu kódu, které tvoří konečné produktu. V současné době vývoj softwaru Tato integrace probíhalo na konci tohoto projektu, který byl proces obtížné a rizikové.

Nepřetržité integrace (CI) vyhnout sloučením každý vývojář změny do běžné základu kódu trvale za dodržování takových složité kroky, obvykle vždy, když všechny vývojáře zkontroluje změny do projektu sdíleného úložiště kódu. Každý změnami aktivuje automatické sestavení a spuštění automatizovaných testů pro ověření, že kód nově přináší nebyla rozdělit existující kód.  Tímto způsobem CI okamžitě zobrazí chyb a problémů a zajistí, že všichni členové týmu zůstane až do data s každou ostatními pracovní. Výsledkem codebase získá na ucelenosti a stabilní.

Průběžnou integraci systémy mají dvě hlavní části:

-  **Správa verzí** – verze ovládacího prvku (VC), označovaný taky jako řízení zdrojů nebo správu zdrojového kódu, slučuje všechny kódu projektu do jednoho sdíleného úložiště a uchovává úplnou historii každé změně pro každý soubor. Toto úložiště, často označuje jako *mainline* nebo *hlavní* větev, obsahuje zdrojový kód, který se nakonec použije k vytvoření provozní nebo vydání verze aplikace. Existuje mnoho s otevřeným zdrojem a pro tuto úlohu komerční produkty, které obvykle umožňují týmy nebo jednotlivce, chcete-li rozvětvit kopii kód na sekundární větve, kde můžete provést rozsáhlé změny nebo proveďte experimenty bez rizika do hlavní větve. Po ověření jsou změny v sekundární větev, je pak lze všechny společně sloučen zpět do hlavní větve.
-  **Server pro nepřetržitou integraci** – v nepřetržité integrace serveru je zodpovědná za shromažďování všechny artefakty projektu (zdrojový kód, obrázků, videí, databáze, automatizovaných testů atd.), kompilace aplikace a spouštění automatizovaných testů. Znovu existuje mnoho s otevřeným zdrojem a komerční nástroje Konfigurace serveru.

Vývojáři obvykle mají pracovní kopie jednu nebo více větví na jejich pracovních stanicích, kde se pracovní původně provádí. Po dokončení příslušné sady pracovní změny jsou "zkontrolovat do" nebo "potvrzené" do odpovídající firemní pobočky, které rozšíří je pracovní kopie jinými vývojáři. Toto je, jak tým zajišťuje, že všechny pracují na stejný kód.

Znovu s průběžnou integraci operace potvrzení změn způsobí, že server položky konfigurace sestavení projektu a spuštění automatizovaných testů ověřit správnost zdrojového kódu. Pokud jsou chyby sestavení nebo testování selhání, CI server upozorní zodpovědná developer (prostřednictvím e-mailu, zasílání rychlých zpráv, Twitter, Growl, atd.), potvrdí můžete vyřešit problém. (CI servery můžete i odmítnout potvrzení Pokud tam jsou chyby, které se nazývají "ověřované vrácení se změnami".)

Následující diagram znázorňuje tento proces:

[![](intro-to-ci-images/intro01-small.png "Tento diagram znázorňuje tento proces")](intro-to-ci-images/intro01.png#lightbox)

Mobilní aplikace zavést jedinečné výzvy pro nepřetržitou integraci. Aplikace může vyžadovat snímače například GPS nebo fotoaparát, které jsou dostupné jenom na fyzických zařízení. Kromě toho simulátorů emulátorů jsou pouze sblížení hardwaru a může skrýt nebo skrývat problémy. Díky tomu je potřeba otestovat mobilní aplikace na skutečné hardwaru, který má být jistý, že je skutečně zákazníka.

[Test Center aplikace](https://docs.microsoft.com/en-us/appcenter/test-cloud) testování aplikací přímo na stovky fyzických zařízení řeší konkrétní problém. Vývojáři psát přijetí automatizované testy, které umožňují efektivní testování uživatelského rozhraní. Jakmile tyto testy se odešlou do Center aplikace, CI serveru můžete spustit je automaticky jako součást procesu CI jak je znázorněno v následujícím diagramu:

[![](intro-to-ci-images/intro02-small.png "Tyto testy se odešlou do aplikace Center, CI serveru můžete spustit po je automaticky v rámci procesu CI znázorněné v tomto diagramu")](intro-to-ci-images/intro02.png#lightbox)

# <a name="components-of-continuous-integration"></a>Součástí průběžnou integraci

Není rozsáhlé prostředí komerční a open source nástrojů pro podporu položek konfigurace. Tato část popisuje několik nejobvyklejší z nich.

## <a name="version-control"></a>Správa verzí

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services a serveru Team Foundation Server

[Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs) (VSTS) a [Team Foundation Server](http://msdn.microsoft.com/en-us/vstudio/ff637362.aspx) (TFS) se sestavení spolupráce nástroje společnosti Microsoft pro nepřetržitou integraci služby, úloha, která sleduje, agilní plánování a vytváření sestav nástroje a verze ovládací prvek. S verzí služby VSTS a pracovní sady TFS můžete svůj vlastní systém (správy verzí Team Foundation nebo TFVC) nebo s projekty, které jsou hostované na Githubu.

 - Visual Studio Team Services poskytuje služby přes cloud. Primární výhod je, že vyžaduje vyhrazený hardware a infrastruktury a je přístupný z odkudkoli prostřednictvím webových prohlížečů a pomocí Oblíbené vývojové nástroje, jako je například Visual Studio, takže je přitažlivými pro týmy, které jsou geograficky distribuován. Je zdarma pro týmy vývojářů pět nebo méně, po které další licence lze zakoupit Abychom vyhověli narůstajícím týmu.
 - TFS je určený pro místní servery Windows a získat přístup prostřednictvím místní sítě nebo připojení k síti VPN. Jeho primární výhodou je, že plně řídit konfiguraci serverů sestavení a můžete nainstalovat libovolnou dalšího softwaru a služeb, jsou potřeba. TFS má volné vstupní úrovně edice Express pro menší týmy.

Služby VSTS i sady TFS jsou pevně integrovány pomocí sady Visual Studio a umožňují vývojářům provádět mnoho verzí a úkoly položek konfigurace z v rámci pohodlí jednoho IDE. K dispozici je také plugin Team Explorer Everywhere pro Eclipse (viz níže). Visual Studio pro Mac nenabízí žádné podporu pro TFS nebo služby VSTS.

Visual Studio Team služby sestavení systém má přímé podpory pro Xamarin projekty, ve které můžete vytvořit definici sestavení pro každou platformu, kterou chcete k cíli (Android, iOS a Windows). Licence vhodnou Xamarin je potřeba pro každý definici sestavení. Je také možné se připojit místní, podporující Xamarin TFS sestavení serveru pro Visual Studio Team Services pro tento účel. S tímto nastavením bude sestavení, které jsou zařazené do služby VSTS delegovat na místním serveru. Podrobnosti najdete v části [nasadit a nakonfigurovat server sestavení](https://msdn.microsoft.com/en-us/library/ms181712.aspx). Alternativně můžete použít jiný nástroj pro sestavení třeba volaných nebo města týmu.

Úplný přehled všechny funkce správy životního cyklu aplikací (ALM) najdete v sadě Visual Studio, Visual Studio Team Services a serveru Team Foundation Server [Správa životního cyklu aplikací s aplikacemi Xamarin](https://msdn.microsoft.com/en-us/library/mt162217(v=vs.140).aspx) na webu MSDN.


### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](http://msdn.microsoft.com/en-us/library/gg413285.aspx) power Team Foundation Server a Visual Studio Team Services umožňuje týmy vývoj mimo Visual Studio. To umožňuje vývojářům připojení k týmovým projektům místně nebo v cloudu z klienta napříč platformami příkazový řádek nebo prostředí Eclipse pro OS X a Linux. Team Explorer Everywhere poskytuje plný přístup do správy verzí (včetně Git), pracovní položky a sestavení možnosti pro jiných platforem než Windows.


### <a name="git"></a>Git

[Git](http://git-scm.com) je řešení řízení populární open source verze, které byl původně vyvinut ke správě zdrojového kódu pro Linux jádra. Je velmi rychlé a flexibilní systém, který je Oblíbené s softwaru projektů všech velikostí. Ho snadno škáluje z jedné vývojářům nízký přístup k Internetu k velkými týmy, které jsou rozmístěny v celém světě. Vytvoření větve velmi snadné, který pak může podporovat paralelní proudy vývoj s minimální rizikové umožňuje také Git.

Git může fungovat zcela prostřednictvím webových prohlížečů nebo přes [grafickým uživatelským rozhraním klienti](http://git-scm.com/downloads/guis) spuštěné v systému Windows, Linux a Mac OSX. Je zdarma pro veřejné úložiště; privátní úložiště vyžadují [placené plán](https://github.com/pricing).

Visual Studio 2015 a Visual Studio pro Mac poskytuje nativní podporu pro Git; pro starší verze, společnost Microsoft poskytuje [ke stažení rozšíření pro Git](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c). Jak jsme uvedli výše, Visual Studio Team Services a TFS můžete použít Git pro správu verzí místo TFVC.


### <a name="subversion"></a>Podverze

[Subversion](http://subversion.apache.org) (SVN) je systém správy verzí oblíbených open source, který byl používán od 2000. SVN běží na všechny moderní verze OS X, Windows, FreeBSD, Linux a Unix. Visual Studio pro Mac má nativní podporu pro SVN. Existují rozšíření třetích stran, které přinášejí podporu SVN pro Visual Studio.


## <a name="continuous-integration-environments"></a>Průběžnou integraci prostředí

Nastavení prostředí průběžnou integraci znamená kombinování systém správy verzí službou sestavení.  K tomu jsou dvě nejběžnější tyto:

- [Team Foundation Build](https://msdn.microsoft.com/Library/vs/alm/Build/overview) je systém sestavení Visual Studio Team Services a sady TFS. Je úzce integrovaná s Visual Studio, které díky vhodné pro vývojáře k aktivaci sestavuje, automaticky spouštět testy a zobrazit výsledky.
- Volaných je server CI open source, s bohatý ekosystém modulů plug-in pro podporu nejrůznějších druhy vývoj softwaru. Běží na systému Windows a Mac OS X. volaných není integrované se žádné konkrétní IDE. Místo toho je nakonfigurovat a spravovat přes webové rozhraní. CI volaných je také usnadňuje instalaci a konfiguraci, která umožňuje přitažlivými pro menší týmy.

Sady TFS nebo služby VSTS můžete použít samostatně nebo volaných můžete použít v kombinaci s TFS nebo služby VSTS nebo Git, jak je popsáno v následujících částech.

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services a serveru Team Foundation Server

Jak je popsáno, Visual Studio Team Services a serveru Team Foundation Server poskytuje obě verze řízení a vytvářet služby. Sestavení služby vždy vyžadují licenci Xamarin firmy nebo organizace pro každou cílovou platformu.

S Visual Studio Team Services vytvoření definice samostatné sestavení pro každou cílovou platformu a zadejte příslušné licence. Po nakonfigurování služby VSTS spustí sestavení a testů v cloudu. V tématu [Team Foundation Build](https://msdn.microsoft.com/Library/vs/alm/Build/overview) další podrobnosti.

S produktem Team Foundation Server nakonfigurujete počítač sestavení pro konkrétní cílové platformy následujícím způsobem:

- **Android a Windows:** nainstalovat Visual Studio a Xamarin nástroje (pro Android a Windows obě) a konfigurace s licencí Xamarin. Je také nutné přesunout SDK pro Android do sdíleného umístění na serveru, kde TFS sestavení agenta najdete ho. Podrobnosti najdete v tématu [konfigurace TFVC](https://docs.microsoft.com/vsts/tfvc/overview).
- **iOS a Xamarin:** instalaci sady Visual Studio a Xamarin nástrojů v systému Windows server s příslušnou licenci. Nainstalujte Visual Studio pro Mac na síťově přístupné počítači Mac OS X, která bude sloužit jako hostitel sestavení a vytvoření balíčku poslední aplikace (soubor IPA pro iOS, aplikace pro OS X).

Následující diagram znázorňuje tento topografie:

[![](intro-to-ci-images/intro03-small.png "Tento diagram znázorňuje tento topografie")](intro-to-ci-images/intro03.png#lightbox)

Je také možné propojit místní server TFS projekt Visual Studio Team Services tak, aby služby VSTS sestavení jsou delegovanými k místnímu serveru. Podrobnosti najdete v tématu [nasadit a nakonfigurovat server sestavení](http://msdn.microsoft.com/en-us/library/ms181712.aspx) na webu MSDN.

### <a name="visual-studio-team-services-and-jenkins"></a>Visual Studio Team Services a volaných

Pokud použijete volaných k sestavení aplikace, můžete ukládat kódu ve Visual Studio Team Services nebo Team Foundation Server a pokračovat v používání volaných pro vaše buildy CI. Sestavení volaných můžete aktivovat po stisknutí kód do úložiště Git nebo když jste se změnami kódu TFVC týmového projektu. Podrobnosti najdete v tématu [volaných s Visual Studio Team Services](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/services/jenkins).

[![](intro-to-ci-images/intro04-small.png "Pokud použijete volaných k sestavení aplikace, můžete ukládat kódu ve Visual Studio Team Services nebo Team Foundation Server a pokračovat v používání volaných pro vaše buildy CI")](intro-to-ci-images/intro04.png#lightbox)

### <a name="git-and-jenkins"></a>Git a volaných

Další běžné CI prostředí může být zcela OS X na základě. Tento scénář zahrnuje pomocí Git pro správu zdrojového kódu a volaných serveru sestavení. Obě tyto běží v jednom počítači Mac OS X pomocí sady Visual Studio pro Mac, které jsou nainstalované. To je velmi podobné Visual Studio Team Services + volaných prostředí popsané v předchozí části:

[![](intro-to-ci-images/intro05-small.png "To je velmi podobné Visual Studio Team Services + volaných prostředí popsané v předchozí části")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Poznámka: Je volaných [nepodporuje Xamarin](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md).**


# <a name="summary"></a>Souhrn

Tento dokument zaveden koncept průběžnou integraci a výhody, které přináší k softwaru vývojové týmy. Význam verzí byla popsané společně s role a odpovědnosti sestavení serveru. Dokument se pak na popisují některé nástroje, které se dají použít pro řízení zdrojového kódu a sestavení servery. Zavedli jsme taky Test Center aplikace, která pomáhá vývojářům spuštěním automatizovaných testů, které bude prokázat kvality a funkce aplikací, jejich publikování kvalitních aplikací. Podrobnější dokumentaci na odesílání aplikací a otestuje aplikace Center najdete [zde](https://docs.microsoft.com/en-us/appcenter/test-cloud). Nakonec pomohou pochopit, jak tyto nástroje a součásti zapadají, jsme uvedeno několik různých prostředích položek konfigurace, které organizace může vytvořit pro nepřetržitou integraci. Další informace pomocí Xamarin projektů Visual Studio Team Services a serveru Team Foundation Server najdete v tématu [konfigurace TFVC](https://docs.microsoft.com/vsts/tfvc/overview) a tato [průběžnou integraci ÚVOD](https://docs.microsoft.com/en-us/vsts/build-release/actions/ci-cd-part-1). Podobně, pokud používáte volaných, přečtěte si téma [volaných pomocí xamarinu](~/tools/ci/jenkins-walkthrough.md) podrobné informace o nastavení nepřetržité integrace.