---
title: Tipy k řešení potíží Xamarin.Mac
description: Tento dokument popisuje přístupy k řešení problémů při vývoji aplikací Xamarin.Mac. Také popisuje, jak získat podporu.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 5e6cd5b338034cfa9956244015d4585b4f005793
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792964"
---
# <a name="xamarinmac-troubleshooting-tips"></a>Tipy k řešení potíží Xamarin.Mac

## <a name="overview"></a>Přehled

Někdy jsme všechny uváznout při práci na projektu, nemohou získat rozhraní API pro pracovat způsobem, který má být buď v pokusu o obejít chyby. V Xamarin Naším cílem je pro vás být úspěšný zápis mobilních a desktopových aplikací, a nabízíme některé prostředky, které pomohou.

S žádným z těchto prostředků existují některé kroky přípravy, který vám pomáhá jim rychle vyřešit problém:

- Určení kořenové příčiny problému, jako nejlepší možné sestavy dojde k chybě:
 
     - "Moje aplikace spadne" je obtížné diagnostikovat. "Moje aplikace spadne po návratu prázdné pole pro toto volání" je mnohem snazší pracovat na řešení.

     - "Nelze získat NSTable pracovat" je užitečné menší než "Žádné metody na Moje NSTableDelegate zdá se, že má být volána v tomto případě."

- Pokud je to možné poskytnout malé příklad program zobrazující problém. Tápat prostřednictvím stránky zdrojový kód hledá problém trvá pořadí podle velikosti další čas a úsilí.

- Zároveň budete vědět, co změny, které jste udělali u aplikace dojít k problémům se objeví můžete rychle zúžit zdroj problému. Poznamenat, pokud jste nedávno upgradován verzích Xamarin.Mac, ořezávání si části vaší aplikace se najít součást, která způsobila problém, nebo testování předchozí sestavení najít, jaké změny zavedl tento problém může být velmi užitečné.


### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>Co dělat, když dojde k chybě aplikace s žádný výstup

Ve většině případů v ladicím programu sady Visual Studio pro Mac zachytí výjimky a dojde k chybě ve vaší aplikaci a můžete sledovat hlavní příčinu. Jsou ale případy, kde bude aplikace kolísají na ukotvení a poté ukončete s minimální nebo žádný výstup. Mohou mezi ně patří:

- Problémy pro podpis kódu.
- Některé mono runtime dojde k chybě.
- Některé výjimky jazyka Objective-c a dojde k chybě.
- Některé chyby velmi brzy životnosti procesu.
- Některé zásobníku přetečení.
- Verze systému macOS uvedené v vaše **Info.plist** je novější než vaše systému macOS aktuálně nainstalované verze nebo je neplatný.

Ladění tyto programy může být frustrující, jako hledání informace potřebné může být obtížné. Tady je několik přístupů, které mohou pomoci:

- Zajistěte, aby uvedené v systému macOS verze **Info.plist** je stejný jako ten, jako je verze systému macOS aktuálně nainstalovaný v počítači.
- Zkontrolujte sady Visual Studio pro Mac aplikace výstup (**zobrazení** -> **dotyková zařízení** -> **výstupu aplikace**) pro trasování zásobníku nebo výstupní červeně z Kakao, který může popisují výstup.
- Spusťte aplikaci z příkazového řádku a podívejte se na výstup (v **Terminálové** aplikace) pomocí: 

     `MyApp.app/Contents/MacOS/MyApp` (kde `MyApp` je název vaší aplikace)
- Výstup můžete zvýšit přidáním "MONO_LOG_LEVEL" do příkazu na příkazovém řádku, například: 

     `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- Můžete připojit nativní ladicí program (`lldb`) na váš proces chcete zobrazit, pokud, která poskytuje žádné další informace (vyžaduje placené licence). Například takto:

    1. Zadejte `lldb MyApp.app/Contents/MacOS/MyApp` v terminálu.
    2. Zadejte `run` v terminálu.
    3. Zadejte `c` v terminálu.
    4. Po dokončení ladění ukončit.
- Jako poslední možnost, před voláním `NSApplication.Init` ve vaší `Main` – metoda (nebo na jiných místech podle potřeby), může zápis textu do souborů v zadaném umístění vysledovat, v jaké kroku spuštění běží do řešení problémů.

## <a name="known-issues"></a>Známé problémy

Následující části se věnují známé problémy a jejich řešení.

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>Nelze se připojit k aplikacím v izolovaném prostoru v ladicím programu

Ladicí program se připojí k Xamarin.Mac aplikací prostřednictvím protokolu TCP, což znamená, že ve výchozím nastavení při povolení sandboxing, nelze se připojit k aplikaci, takže pokud se pokusíte spustit aplikaci bez oprávnění povoleno, dojde k chybě *"nelze se připojit k ladicí program"*. 

[![Úpravy oprávnění](troubleshooting-images/debug01.png "úpravy oprávnění")](troubleshooting-images/debug01-large.png#lightbox)

**Povolit odchozí síťové připojení (klienta)** oprávnění, která je potřeba v ladicím programu, povolíte tento jeden, bude mít ladění normálně. Vzhledem k tomu, že ladíte nelze bez ní, jste aktualizovali jsme `CompileEntitlements` cíle pro `msbuild` pro každou aplikaci, která je v izolovaném prostoru pro ladění sestavení pouze automaticky přidat tato oprávnění pro oprávnění. Verze sestavení by měli používat oprávnění zadaný v souboru oprávnění ponechat beze změny.

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: k dispozici pro kódování 437 žádná data
 
Při zahrnutí 3. stran knihovny v aplikaci Xamarin.Mac, může dojde k chybě ve tvaru "System.NotSupportedException: nejsou k dispozici pro kódování 437 data" při pokusu o zkompilování a spuštění aplikace. Například knihovny, například `Ionic.Zip.ZipFile`, může vyvolat výjimku během operace.

To lze vyřešit tak, že otevřete Možnosti projektu Xamarin.Mac, přejdete na **sestavení Mac** > **internacionalizace** a kontrolu **– západ** mezinárodní prostředí:

[![Možnosti sestavení úprav](troubleshooting-images/issue01.png "sestavení možnosti úprav")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>Kompilace (mm5103) se nezdařila.

Tato chyba se obvykle stává, pokud je nová verze Xcode verze a instalaci nové verze, ale ne dosud ji spustit. Před pokusem o kompilaci s novou verzi Xcode, musíte nejprve alespoň jednou spustit tuto verzi.

Při prvním spuštění novou verzi Xcode, nainstaluje několik nástrojů příkazového řádku, které jsou vyžadované Xamarin.Mac. Kromě toho byste měli provést čisté sestavení po aktualizaci vaší verze Xamarin.Mac nebo Xcode.

Pokud chcete tento problém nevyřešíte, [založení záznamu o chybě](#filing-a-bug).

### <a name="missing-entitlementsplist"></a>Chybějící entitlements.plist

Nejnovější verze sady Visual Studio pro Mac odebrala v části oprávnění z **Info.plist** editoru a uloženy v oddělených **Entitlements.plist** editor (pro lepší podporu více platforem s Xamarin.iOS).

Pomocí nové sady Visual Studio pro Mac, když vytvoříte nový projekt aplikace Xamarin.Mac **Entitlements.plist** soubor bude automaticky přidán do stromu projektu:

![Výběr oprávnění](troubleshooting-images/entitlements01.png "výběr oprávnění")

Pokud dvakrát kliknete **Entitlements.plist** editoru oprávnění souboru se zobrazí:

[![Úpravy oprávnění](troubleshooting-images/entitlements02.png "úpravy oprávnění")](troubleshooting-images/entitlements02-large.png#lightbox)

Pro existující projekty Xamarin.Mac, budete muset ručně vytvořit **Entitlements.plist** kliknutím pravým tlačítkem myši na projekt v souboru **řešení Pad** a výběrem **přidat**  >  **Nový soubor...** . Potom vyberte **Xamarin.Mac** > **prázdný seznam vlastností**:

![Přidání nového seznamu vlastností](troubleshooting-images/entitlements03.png "přidání nového seznamu vlastností")

Zadejte `Entitlements` pro název a klikněte **nový** tlačítko. Pokud váš projekt dříve zahrnuty soubor nároků, vyzve k přidání do projektu místo vytvoření nového souboru:

[![Ověření přepsání soubor](troubleshooting-images/entitlements04.png "ověření přepsat souboru")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="contacting-support-business-or-enterprise-licenses"></a>Kontaktování podpory (podnikové licence)

Pokud máte obchodní nebo licenci enterprise, jste oprávněni žádat o pomoc přímo Xamarin technici prostřednictvím lístky žádostí o podporu. V tématu [xamarin.com/support](http://xamarin.com/support) podrobnosti.

## <a name="community-support-on-the-forums"></a>Podpora komunity ve fórech

Komunita vývojářů pomocí Xamarin produkty jsou úžasné a mnoho navštivte naše [Xamarin.Mac fóra](http://forums.xamarin.com/categories/mac) sdílet prostředí a své znalosti. Kromě toho Xamarin technici pravidelně navštivte fórum pomohou.

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>Vyplnění chyby

Váš názor je pro nás důležitá. Pokud narazíte na potíže s Xamarin.Mac:

- Hledání [problém úložiště](https://github.com/xamarin/xamarin-macios/issues) 
- Před přepnutím na Githubu problémy, problémy Xamarin byly sledovány pro [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Pro párování problémy, vyhledávání existuje.
- Pokud nemůžete najít odpovídající problém, prosím soubor nový problém v [potíže úložiště GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub problémy jsou všechny veřejné. Není možné skrýt komentáře nebo přílohy. 

Uveďte co nejvíc následující kroky jako možné:                                                                                                                                          

- Jednoduchý příklad reprodukci problému. Toto je **neocenitelnou pomocí** kde je to možné. 
- Trasování zásobníku úplné havárie.
- C# kódu havárii. 
