---
title: Ladění více procesů
ms.prod: xamarin
ms.assetid: 852F8AB1-F9E2-4126-9C8A-12500315C599
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: ec2203b668365290fc9df78e63b401fb71ead7e6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="multi-process-debugging"></a>Ladění více procesů

Je velmi běžné pro moderní řešení vyvinuté v sadě Visual Studio pro Mac tak, aby měl více projektů, že různé cílové platformy. Řešení může mít například projekt mobilní aplikace, který využívá údaje poskytnuté projektu webové služby. Při vývoji řešení, může vývojář musí být oba projekty současné spuštění k řešení chyb. Počínaje [cyklus 9 verzi Xamarin](https://releases.xamarin.com/stable-release-cycle-9/), je nyní možné pro sadu Visual Studio pro Mac k ladění více procesů, které jsou spuštěné ve stejnou dobu. Díky tomu je možné nastavit zarážky, chcete-li prověřit proměnné a k zobrazení vláken ve více než jeden spuštěné projektu. To se označuje jako _ladění více procesů_. 

Tato příručka popisuje některé změny provedené v sadě Visual Studio pro Mac pro podporu ladění více procesů, jak nakonfigurovat řešení k ladění více procesů a jak pro připojení k stávající procesy pomocí sady Visual Studio for Mac.

## <a name="requirements"></a>Požadavky

Ladění více procesů vyžaduje Visual Studio for Mac.

## <a name="ide-changes"></a>Změny IDE

Což vývojářům s více procesu ladění, Visual Studio pro Mac prošla některé změny v jeho uživatelském rozhraní. Visual Studio pro Mac má aktualizované **ladění nástrojů**a nový **konfigurace řešení** tématu **možnosti řešení** složky. Kromě toho **vláken Pad** bude nyní zobrazení systémem vláken pro jednotlivé procesy a procesy. Visual Studio pro Mac se také zobrazí více PAD pro ladění, jeden pro jednotlivé procesy pro některé aspekty **výstupu aplikace**.

### <a name="solution-configurations"></a>Konfigurace řešení

Ve výchozím nastavení, zobrazí Visual Studio pro Mac jednotlivých projektů v **konfigurace řešení** oblast panelu nástrojů ladění. Když je zahájena ladicí relace, a je to projekt, který spustí a připojit ladicí program k sadě Visual Studio pro Mac.

Spuštění a ladění více procesů v sadě Visual Studio pro Mac, je nutné vytvořit _konfigurace řešení_. Konfigurace řešení popisuje, co by měl být projekty v řešení zahrnuty při zahájení relace ladění s kliknutím **spustit** tlačítko nebo když &#8984; &#8617; (**zadejte Cmd**) je stisknutí tlačítka. Na následujícím snímku obrazovky je příklad řešení v sadě Visual Studio pro Mac, který má více konfigurace řešení:

![](multi-process-debugging-images/mpd01-xs.png "Řešení s konfigurací s více řešení")

### <a name="parts-of-the-debug-toolbar"></a>Součástí nástrojů ladění

Ladění nástrojů se změnila na povolit konfiguraci řešení, kterou chcete vybrat prostřednictvím místní nabídky. Tento snímek obrazovky ukazuje části panelu nástrojů ladění:

![](multi-process-debugging-images/mpd02-xs.png "Části panelu nástrojů ladění")

1. **Konfigurace řešení** -je možné nastavit konfiguraci řešení tak, že kliknete na konfiguraci řešení na panelu nástrojů ladění vyberete z místní nabídky konfigurace:

    ![](multi-process-debugging-images/mpd03-xs.png "Ukázka místní okno s konfigurace řešení")

2. **Vytvořit cíl** – ten identifikuje cíl sestavení pro projekty. To se neliší od předchozích verzí sady Visual Studio for Mac.
3. **Cíle zařízení** – to vybere zařízení, která se spustí řešení na. Je možné identifikovat samostatné zařízení nebo emulátor pro každý projekt.:

    ![](multi-process-debugging-images/mpd04-xs.png "Místní nabídka zobrazení zařízení pro projekt")

### <a name="multiple-debug-pads"></a>Více dotyková zařízení ladění

Při spuštění více řešení konfigurace, některé sady Visual Studio pro Mac dotyková zařízení vícekrát, se zobrazí jeden pro jednotlivé procesy. Například následující snímek obrazovky ukazuje dva **výstupu aplikace** dotyková zařízení pro řešení, které běží dva projekty:

![](multi-process-debugging-images/mpd05-xs.png "Odsazení výstup pro konfiguraci řešení")

### <a name="multiple-processes-and-the-active-thread"></a>Více procesů a _aktivních vláken_

Když zarážku došlo v jednom procesu, že proces se pozastaví provádění, zatímco jiné procesy dál běžet. Ve scénáři jeden procesu Visual Studio pro Mac můžete snadno zobrazit informace, jako je vláken, místní proměnné, výstupní aplikace v jedné sady dotyková zařízení. Ale pokud je více procesů pomocí více zarážky a potenciálně více vláken, může prokázat, čtenáře vývojáře zvládnout informacemi z relace ladění, který se snaží zobrazení všech informací z všechna vlákna (a procesy) najednou.

K řešení tohoto problému, Visual Studio pro Mac zobrazí pouze informace z jednoho vlákna současně, to se označuje jako _aktivních vláken_. První podproces, který pozastavuje na zarážce považuje za _aktivních vláken_. Aktivních vláken je podproces, který je zaměřen vývojáře pozornost. Ladění příkazy, jako například **Krokovat s přeskočením** &#8679; &#8984;O (Shift-Cmd-O), budou vydané aktivních vláken.

**Vlákno Pad** budou zobrazovat informace pro všechny procesy a vláken, které jsou v části kontroly v konfiguraci řešení a poskytují vizuální upozornění, co je aktivních vláken:

![](multi-process-debugging-images/mpd06-xs.png "Odsazení pro konfiguraci řešení přístup z více vláken")

Vláken jsou seskupené podle procesu, který je hostitelem. Název projektu a ID aktivních vláken zobrazí tučně a šipka vpravo se zobrazí v oddělovací mezery u aktivních vláken. V předchozím snímku obrazovky **vláken #1** v **zpracovat Id 48703** (**FirstProject**) je aktivních vláken.

Při ladění více procesů, je možné přepnout aktivních vláken zobrazíte informace pro tento proces (nebo vlákno) ladění pomocí **vlákno Pad**. Přepnout aktivních vláken, vyberte požadované vlákno v **vlákno Pad** a potom dvakrát klikněte na něj.

#### <a name="stepping-through-code-when-multiple-projects-are-stopped"></a>Procházení kódu, pokud jsou zastaveny více projektů

Pokud dva (nebo více) projekty body rozdělení, Visual Studio pro Mac se pozastaví oba tyto procesy. Je možné k **Krokovat s přeskočením** kód v aktivních vláken. Jiného procesu bude pozastaven, dokud nebude změna oboru umožňuje ladicí program na přepnutí z aktivních vláken. Zvažte například následující snímek obrazovky sady Visual Studio pro Mac ladění dva projekty:

![](multi-process-debugging-images/mpd09-xs.png  "Visual Studio pro Mac ladění dva projekty")

Každé řešení na tomto snímku obrazovky, má svou vlastní zarážek. Při spuštění ladění je první zarážky vyskytnout na **řádek 10** z `MainClass` v **SecondProject**. Vzhledem k tomu, že oba projekty zarážky, se zastavilo jednotlivých procesů. Po zarážku je zjištěna, každé vyvolání sady **Krokovat s přeskočením** způsobí, že Visual Studio pro Mac, aby krok přes kód v aktivních vláken.

Procházení kódu je omezený na aktivních vláken, aby Visual Studio pro Mac bude krokovat, řádek kódu současně, zatímco jiný proces je nadále pozastaven.

Jako příklad, použijeme předchozí snímek obrazovky při `for` smyčky dokončení, Visual Studio pro Mac by umožnilo **FirstProject** můžou běžet, dokud zarážek v **řádek 11** v `MainClass` byl byla zjištěna. Pro každou **Krokovat s přeskočením** příkazu ladicího programu by zálohy v řádek po řádku **FirstProject**, dokud vnitřní heuristické algoritmy sady Visual Studio pro Mac přepínač aktivních vláken zpět do  **SecondProject**.

Pokud jenom jeden z projektů měl nastavit bod přerušení a potom pouze procesu by být pozastaveno. Jiný projekt by nadále spustit, dokud pozastavena vývojáře nebo byl přidán zarážky.

### <a name="pausing-and-resuming-a-processes"></a>Pozastavení a obnovení procesy

Je možné pozastavit nebo obnovit proces kliknutím pravým tlačítkem myši na proces a výběrem **pozastavit** nebo **obnovit** v místní nabídce:

![](multi-process-debugging-images/mpd08-xs.png "Pozastavení nebo obnovení v panelu pro přístup z více vláken")

Vzhledu ladění nástrojů se změní v závislosti na stavu projektů, které se právě ladí. Pokud více projektů současně spuštěné, ladění panelu nástrojů zobrazí i **pozastavení** a **obnovit** tlačítko tam, kde je spuštěná alespoň jeden projekt a pozastavena jeden projektu:

![](multi-process-debugging-images/mpd07-xs.png  "Ladění panelu nástrojů")

Kliknutím na **pozastavit** tlačítka na **ladění nástrojů** bude pozastavit všechny procesy, které se právě ladí při kliknutí na **obnovit** tlačítka obnoví všechny pozastavené procesy .

### <a name="debugging-a-second-project"></a>Ladění druhý projektu

Je také možné ladění projektu pro druhý, jakmile se spustí první projekt sady Visual Studio for Mac. Po spuštění první projekt **klikněte pravým tlačítkem na* na projekt v **řešení Pad** a vyberte **spustit ladění položky**:

![](multi-process-debugging-images/mpd13-xs.png  "Spustit ladění – položky")

## <a name="creating-a-solution-configuration"></a>Vytvoření konfigurace řešení

A _konfigurace řešení_ informuje Visual Studio pro Mac, jaké projektu spustit, když je relace ladění iniciovala s **spustit** tlačítko. Může existovat více než jednu konfiguraci řešení za řešení. Díky tomu je možné k určení, jaké projekty jsou spuštěny při ladění projektu.

Chcete-li vytvořit novou konfiguraci řešení v sadě Xamaring Studio:

1. Otevřete **možnosti řešení** dialogové okno v sadě Visual Studio pro Mac, vyberte **spustit > Konfigurace**:

    ![](multi-process-debugging-images/mpd10-xs.png "Konfigurace řešení v dialogovém okně Možnosti řešení")

2. Klikněte na **nový** tlačítko a zadejte název nové konfigurace řešení a klikněte na **vytvořit**. Zobrazí se nová konfigurace řešení v **konfigurace** okno:

    ![](multi-process-debugging-images/mpd11-xs.png  "Pojmenování novou konfiguraci řešení")

3. V seznamu konfigurace vyberte novou konfiguraci spuštění. **Možnosti řešení** dialogové okno se zobrazí každý projekt v řešení. Zkontrolujte vypnout každý projekt, který by měl být spuštěn při zahájení relace ladění:

    ![](multi-process-debugging-images/mpd12-xs.png "Výběr zahájení projektu")

**MultipleProjects** konfigurace řešení se nyní zobrazí v **ladění nástrojů**, která umožňuje vývojáři k současně ladění dva projekty.

## <a name="summary"></a>Souhrn

Tato příručka popsané ladění více procesů v sadě Visual Studio for Mac. Je popsaná některé změny do integrovaného vývojového prostředí pro podporu více procesů ladění a popsané některé související chování.

## <a name="related-links"></a>Související odkazy

- [Poznámky k verzi Xamarin cyklus 9](https://releases.xamarin.com/stable-release-cycle-9/)
