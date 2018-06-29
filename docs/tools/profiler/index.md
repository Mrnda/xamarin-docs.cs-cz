---
title: Xamarin Profiler
description: Tato příručka popisuje klíčové funkce profileru Xamarin. Ji vyhledat v profilery, profilace a kdy by měly být použity a v pracovním postupu standardní profily – aplikace Xamarin.
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
author: topgenorth
ms.author: toopge
ms.date: 06/03/2018
ms.openlocfilehash: 8882cb9cd84940e12865a730f75e36ecbaf9b6f0
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066673"
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_Tato příručka popisuje klíčové funkce profileru Xamarin. Ji vyhledat v profilery, profilace a kdy by měly být použity a v pracovním postupu standardní profily – aplikace Xamarin._

Aplikace úspěch závisí na prostředí koncového uživatele. Jako vývojář může implementovat některé skutečně Super funkce ve vaší aplikaci, ale pokud je aplikace pomalá nebo úplné havárií, uživatel bude pravděpodobně se jich zbavit.

V minulosti má Mono vybrané výkonné příkazového řádku profileru pro shromažďování informací o programům spuštěným v Mono runtime volat [Mono protokolu profileru](http://www.mono-project.com/docs/debug+profile/profile/profiler/). Xamarin profileru grafické rozhraní pro profileru Mono protokolu a podporuje vytváření profilů Android, iOS, tvOS a aplikací systému Mac v systému Mac a Android, iOS a tvOS aplikace v systému Windows.

Xamarin profileru má několik dostupných nástrojů pro profilaci – přidělení, cykly a čas profileru. Tato příručka jsou zde popsány co měřit těchto nástrojů a jak se analyzovat vaše aplikace a vysvětluje význam údaje zobrazené na jednotlivých obrazovky.

Tato příručka prozkoumá běžné scénáře profilování a zavádí profileru jako nástroj vám usnadní analýzu a optimalizaci iOS a Android aplikace.

## <a name="download-and-install"></a>Stáhnout a nainstalovat

> [!NOTE]
> Budete muset být [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/) odběratele odemknout tuto funkci v buď Visual Studio Enterprise v systému Windows nebo Visual Studio pro Mac v počítačích Mac.

Xamarin profileru je samostatná aplikace a je integrována s Visual Studio pro Mac a Visual Studio, chcete-li povolit profilace z prostředí IDE.

Stáhněte si balíček instalace pro vaši platformu:

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/profiler-windows.msi)

Po stažení, spusťte instalační program pro přidání do systému profileru Xamarin.

## <a name="profilers-and-profiling"></a>Profilery a profilace

Profilace je důležité a často přehlíženým krok při vývoji aplikace. Profilace je forma **analýzy dynamické programu** -analyzuje program, když je spuštěná a používá. Profileru je nástroj dolování dat, který shromažďuje informace o složitost čas, použití konkrétní metody a paměti přidělovány. Profileru můžete přejít k podrobnostem hloubky a analyzovat tyto metriky ke kotvícímu bodu problémových oblastí v kódu.

Při navrhování a vývoji aplikace, je důležité není optimalizovat předčasně; To znamená výdaje čas vývoj kódu v oblastech, které budou zřídka přístup. Toto je sílu profilace. Profileru poskytuje přehled o nejvíc běžně používané části základní kódu a pomáhá vyhledejte oblasti, kde byste se měli něco využívajícího vylepšení čas. Vývojáři měli postará pochopit, kde je většinu času stráveného v aplikaci a použití paměti v aplikaci.

Profilace je užitečné pro všechny typy vývoje, ale je obzvláště důležité v pro vývoj mobilních řešení. Neoptimalizované kód je mnohem významnější na mobilních platformách než pro stolní počítače a úspěch vaší aplikace závisí na Krásný a optimalizované kód, který běží efektivně.

## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin profileru poskytuje vývojářům způsob, jak profil aplikace v sadě Visual Studio pro Mac nebo Visual Studio. Profileru shromažďuje a zobrazuje informace o aplikaci, která lze poté použít vývojáře k analýze chování aplikace. Existuje několik různých způsobů, jak profilu aplikace s Profilerem Xamarin, a to profilace paměti a statistické vzorkování. Tyto se provádějí pomocí přidělování a čas profileru instruments v uvedeném pořadí.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V současné době profileru Xamarin slouží k testování aplikace Xamarin.iOS, Xamarin.Android a Xamarin.Mac v systému Mac (prostřednictvím Visual Studio pro Mac). Profileru je samostatný proces z prostředí IDE, a tedy kromě spuštění ze sady Visual Studio pro Mac, může sloužit jako samostatná aplikace prozkoumat .exe a `.mlpd` soubory, které byly získány ze [mono protokolu profileru](http://www.mono-project.com/docs/debug+profile/profile/profiler/).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V současné době profileru Xamarin slouží k testování aplikace Xamarin.Android v systému Windows (prostřednictvím sady Visual Studio a Visual Studio pro Mac). Profileru je samostatný proces z prostředí IDE, a tedy kromě spuštění ze sady Visual Studio, může sloužit jako samostatná aplikace prozkoumat .exe a `.mlpd` soubory, které byly získány ze [mono protokolu profileru](http://www.mono-project.com/docs/debug+profile/profile/profiler/) .

-----

<a name="Profiler_Support" />

## <a name="profiler-support"></a>Podpora profileru

Podpora pro Xamarin profileru je dostupná na následujících platformách:

 - Visual Studio pro Mac (s licencí Enterprise macOS)
    - Android
        - Zařízení a emulátoru
    - iOS
        - Zařízení a simulátoru
    - tvOS (čas nástrojích není podporována.)
        - Zařízení a simulátoru
    - Mac

 - Visual Studio (pouze **Enterprise** verze)
    - Android
        - Zařízení a emulátoru
    - iOS [experimentální]
        - Zařízení a simulátoru
    - tvOS
        - Zařízení a simulátoru

Všimněte si, že můžete **pouze** profil **ladění** konfigurace.

## <a name="profiler-basics"></a>Základy profileru

Tato část uvádí části profileru Xamarin a kurzy její funkce.

### <a name="allow-profiling-in-your-app"></a>Povolit profilace v aplikaci

Předtím, než můžete úspěšně profilu aplikace, musíte povolit profilování v možnosti projektu aplikace.

 - iOS:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  **Sestavení > iOS ladění > Povolit profilace**

  ![](images/ios-options-mac.png "iOS dialogovém okně Možnosti v sadě Visual Studio pro Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **Vlastnosti > iOS sestavení > Povolit profilace**

  ![](images/ios-project-options-vs.png "iOS dialogovém okně Možnosti v sadě Visual Studio")

-----

 - Android:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  **Sestavení > Android ladění > Povolit vývojáře instrumentace**

  ![Dialogové okno Možnosti Android v sadě Visual Studio pro Mac](images/android-project-options.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **Sestavení > Android ladění > Povolit vývojáře instrumentace**

  ![Dialogové okno Možnosti Android v sadě Visual Studio pro Mac](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>Spuštění profileru

Xamarin profileru může být spuštěn v prostředí IDE při jsou profilace zařízení s iOS nebo Android aplikace nebo jako samostatnou aplikaci.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="launching-from-visual-studio-for-mac"></a>Spuštění ze sady Visual Studio pro Mac

1. Nejprve zkontrolujte, zda máte aplikaci načíst v sadě Visual Studio pro Mac a vyberte konfiguraci ladění (výchozí).
2. Přejděte do **spustit > spuštění profilace**v sadě Visual Studio pro Mac, nebo **analyzovat > Xamarin profileru** v sadě Visual Studio otevřete profileru, jak je ukázáno v následujícím diagramu:

  ![](images/start-profiling-xs.png "Spuštění profileru ze sady Visual Studio pro Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="launching-from-visual-studio"></a>Spuštění ze sady Visual Studio

1.  Nejprve zkontrolujte, zda máte aplikaci načíst v sadě Visual Studio a vyberte konfiguraci ladění (výchozí), jak je uvedeno výše.
2.  Přejděte do **analyzovat > Xamarin profileru** v sadě Visual Studio otevřete profileru, jak je ukázáno v následujícím diagramu:

![Spuštění profileru ze sady Visual Studio](images/start-profiling-vs.png)

-----

Pokud položky nabídky se nezobrazí, podívejte se na [Průvodce odstraňováním potíží s](~/tools/profiler/troubleshooting.md).

Tím se spustí profileru a automaticky spustí profilace aplikace.

Profileru slouží k měření paměti a výkon. Se dosahuje prostřednictvím přidělování a čas profileru nástroje, které se podíváme na podrobně v další části.

#### <a name="saving-and-loading-profiler-sessions"></a>Ukládání a načítání relace profileru

Chcete-li uložit relace profilování kdykoli, zvolte **soubor > Uložit jako...**  z řádku nabídek profileru. To umožňuje ušetřit souboru v _mlpd_ formát, zvláštní, vysoce komprimovaný formát pro profilace data.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po dokončení instalace profileru Xamarin naleznete ve složce aplikace jak ukazuje následující snímek obrazovky:

![](images/applications.png "Otevření samostatné profileru z Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po dokončení instalace aplikace Xamarin profileru naleznete v adresáři vaší aplikace:

![](images/applications-vs.png "Otevřete samostatný Profiler ze systému Windows ")

-----

Můžete načíst *.mlpd* soubory do profileru otevřením samostatná aplikace, výběr **zvolte cílový** a načítání souboru.

Další informace najdete v tématu [generování souborů .mlpd ](~/tools/profiler/troubleshooting.md#gen_mlpd).

## <a name="profiler-features"></a>Funkce profileru

Xamarin profileru se skládá z pěti částí, jak je uvedeno dále:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](images/profiler-mac-sml.png "Části profileru v sadě Visual Studio pro Mac")](images/profiler-mac.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/profiler-vs.png "Části profileru v sadě Visual Studio")](images/profiler-vs.png#lightbox)

-----

- **Panel nástrojů** – nachází v horní části profileru, to nabízí možnosti pro spuštění a zastavení profilování, vyberte cílový proces, zobrazení doba spuštění aplikace a vyberte zobrazení rozdělení, které tvoří profileru aplikace.
- **Instrumentace seznamu** – rutina Vypíše seznam všech nástrojů pro relace profilování načtena.
- **Vykreslení grafu** – tyto grafy se týkají vodorovně na příslušné nástroje v seznamu nástrojích. Jezdce (zobrazené pod čas profileru) umožňuje změnu měřítka.
- **Instrumentace oblasti podrobností** -obsahuje data v zobrazení vybrané zobrazení aktuální nástroje. Podíváme se na tato zobrazení podrobněji v následující části.
- **Zobrazení Inspector** – obsahuje oddíly, které lze vybrat segmentovaným řízení. V částech jsou závislé na nástrojích vybrané a zahrnuje: nastavení konfigurace, statistiky, informace trasování zásobníku a cestu ke kořenové adresáře.

### <a name="allocations"></a>Přidělení

Přidělení nástrojích obsahuje podrobné informace o objektech v aplikaci jako během vytváření a uklizeny.

V horní části profileru je graf přidělení, která zobrazuje množství paměti přidělené v pravidelných intervalech během profilace. Přidělení grafu aktuálně je celkový počet přidělených a není velikost haldy v tomto bodě v čase. V tom smyslu se nikdy přejděte, bude vždy jen zvýšit. To zahrnuje objekty přidělené v zásobníku. V závislosti na verzi modulu runtime, použít může vypadat jinak – i pro stejnou aplikaci grafu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](images/allocations1.png "Přidělení nástrojích")](images/allocations1.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/allocations1-vs.png "Přidělení nástrojích")](images/allocations1-vs.png#lightbox)

-----

Existují různé datové zobrazení s nástrojem přidělení, které umožňují vývojářům analyzovat, jak je svých aplikacích pomocí a uvolnění paměti. Tato zobrazení jsou následující:

 -   **Přidělení** – to zobrazí seznam všech přidělení a skupin je podle názvu třídy. To poskytuje skvělý přehled třídy a metody, které používá, jak často se používají a souhrnný velikost třídy používané. Dvojité kliknutí na třídu se zobrazí je paměť přidělená: 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/allocations3.png "Karta přidělení")](images/allocations3.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations2-vs.png "Karta přidělení")](images/allocations2-vs.png#lightbox)

-----

Zobrazení Inspector pro přidělení poskytuje možnosti pro filtrování a seskupení objekty, poskytuje statistiky na paměť přidělená a hlavní přidělení, a také zobrazení pro trasování zásobníku a cestu ke kořenové.

 -   **Strom volání** – to zobrazuje stromu celý volání všechna vlákna v aplikaci a obsahuje informace o paměti přidělené na každém uzlu. Pokud element je vybrali v seznamu, zobrazí se všechny uzly na stejné úrovni šedý symbol. Můžete rozbalte strom nebo dvakrát kliknout na elementu, který chcete zobrazit je. Při zobrazení toto zobrazení dat, zobrazení nastavení inspector zobrazení lze změnit způsob, jakým se zobrazí. Aktuálně jsou k dispozici dvě možnosti:
    1.  **Obrácený volání stromu** – to zvažuje trasování zásobníku shora dolů. To je vhodné zobrazení možnost jako znamená nejhlubší metody kde procesoru výdaje jeho čas.
    2.  **Samostatné vlákno** – tato možnost slouží k uspořádání stromu volání vlákno.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/allocations2.png "Karta stromu volání")](images/allocations2.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations3-vs.png "Karta stromu volání")](images/allocations3-vs.png#lightbox)

-----

 -   **Snímky** – v tomto podokně zobrazí informace o snímky paměti. Chcete-li vygenerovat tyto při profilace živé aplikace, klikněte na tlačítko _fotoaparát_ tlačítka na panelu nástrojů v každém bodu, který chcete zjistit, jaké paměti je uchovávají a vydané. Klikněte na jednotlivých snímků a prozkoumejte, co se děje pod pokličkou. Všimněte si, že snímky pouze děláte při provozu profilace aplikace. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/allocations4.png "Karta snímky")](images/allocations4.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations4-vs.png "Karta snímky")](images/allocations4-vs.png#lightbox)

-----

### <a name="time-profiler"></a>Čas profileru

Čas profileru nástrojích měří přesně je kolik času stráveného v každá metoda aplikace. Aplikace je pozastavená v pravidelných intervalech a trasování zásobníku se spouští na každý aktivních vláken. Každý řádek v těchto nástrojích zobrazí provádění cestu, která má byly provedeny.

Krabicový diagram, jak je vidět na tomto snímku obrazovky, zobrazuje počet vzorků přijaté aplikací při jeho spuštění:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Čas nástrojích profileru](images/time1.png)](images/time1.png#lightbox) 

[![Čas profileru nástrojích – ukázky seznamu](images/time3.png)](images/time3.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Čas nástrojích profileru](images/time1-vs.png)](images/time1-vs.png#lightbox) 

[![Čas profileru nástrojích – ukázky seznamu](images/time3-vs.png)](images/time3-vs.png#lightbox) 

-----

- **Strom volání** – ukazuje množství času stráveného v jednotlivých metod:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/time2.png "Čas profileru nástrojích – stromu volání")](images/time2.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/time2-vs.png "Čas profileru nástrojích – stromu volání")](images/time2-vs.png#lightbox) 

-----

### <a name="cycles"></a>Cykly

Pomocí jazyka C# a F # spravovaný kód může být celkem běžné a bohužel poměrně snadno vytvořit odkazy na objekty, které bude nikdy zlikvidován. Tento nástroj umožňuje přesné těchto objektů a zobrazení cykly odkazovaný ve vaší aplikaci.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Cykly nástrojích](images/cycles.m751-sml.png)](images/cycles.m751.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Cykly nástrojích](images/cycles-vs-sml.png)](images/cycles-vs.png#lightbox) 

-----

## <a name="profiling-applications"></a>Profilace – aplikace

V současné době může být profilovaným pouze výchozí konfigurace ladění.

Pokud je profil aplikace pro ostatní konfigurace, zobrazí se následující dialogové okno zprávy:


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Profilace chybové dialogové okno](images/image001.png)](images/image001.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/image1vs.png "Profilace chybové dialogové okno")](images/image1vs.png#lightbox) 

-----

Vyberte **aktualizace** pokračujte.

### <a name="sgen-garbage-collector-and-profiling"></a>SGen – uvolňování paměti a profilace

[SGen](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) systém uvolňování paměti se používá na všech platformách Xamarin.

SGen – je generační globální Katalog, který přiděluje objekty aplikace do tří haldách – mateřské, hlavní haldy a velký prostor objektu. To umožňuje rychlejší spouštění uvolňování paměti. SGen – je aktuálně výchozí globální Katalog pro Xamarin.Android a aplikace Xamarin.iOS Unified.

Aplikace pro Xamarin.iOS pomocí klasické rozhraní API používá Boehm GC – konzervativní-generační uvolňování. Jak je konzervativní, je méně pravděpodobné, že uvolněte paměť, což může vést k nesprávné výsledky při použití profileru. Z tohoto důvodu nelze použít s Boehm garbage collector v nástrojích přidělení.

Když budete vyzváni se dialogové okno zprávy Pokud vaše aplikace používá Boehm globální Katalog, nedoporučuje Xamarin přepínání existující aplikaci iOS, která použít Boehm k SGen bez pečlivě výzkum a důkladné testování. Xamarin také nedoporučuje přepnutí na SGen k profilování a potom přepnout zpět, podle těchto výsledků, nebudou zajišťovat přesné srovnávací testy využití paměti.

Další informace o správě paměti naleznete [paměti a osvědčené postupy z hlediska výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md) průvodce.

## <a name="summary"></a>Souhrn

V této příručce jsme se podívali na jaké profilace a jak je výhodné pro vývojáře. Zavedli jsme pak profileru Xamarin, poskytuje některé historie a informace o tom, jak funguje. Nakonec jsme toured funkce profileru Xamarin a prozkoumali přidělení a čas Profiler nástrojů.

## <a name="related-links"></a>Související odkazy

- [Doporučené postupy pro paměti a výkon](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Zpráva k vydání verze](https://developer.xamarin.com/releases/profiler/preview/)
