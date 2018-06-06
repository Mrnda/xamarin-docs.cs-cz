---
title: Ladění aplikace Xamarin.iOS
description: Tento dokument popisuje postup použití ladicího programu v sadě Visual Studio pro Mac nebo Visual Studio 2017 k ladění aplikace pro Xamarin.iOS, včetně nastavení zarážek, bezdrátové ladění a další.
ms.prod: xamarin
ms.assetid: 05460010-99E1-DC38-F855-2D691EF54484
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 494dfad0ba3d26147604ce1bca1de49fac318811
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785431"
---
# <a name="debugging-xamarinios-apps"></a>Ladění aplikace Xamarin.iOS

_Pomocí předdefinovaných ladicího programu v sadě Visual Studio pro Mac nebo Visual Studio můžete ladit aplikace Xamarin.iOS._

Použijte sadu Visual Studio pro Mac na nativní podporu ladění pro ladění jazyka C# a další spravované jazyky kód a použít [LLDB](http://lldb.llvm.org/tutorial.html) když potřebujete ladění C, C++ nebo Objective C codethat může být připojujete projektu Xamarin.iOS.


> [!NOTE]
> Při kompilaci aplikace v režimu ladění, vygeneruje Xamarin.iOS pomalejší a mnohem větší aplikace jako musí být instrumentovány každý jednotlivý řádek kódu. Před uvolněním, ujistěte se, abyste provedli sestavení pro vydání.

Ladicí program Xamarin.iOS je integrována do vaší IDE a umožňuje vývojářům ladit aplikace Xamarin.iOS integrovaný s žádným z spravované jazyky nepodporuje Xamarin.iOS v simulátoru a na zařízení.

Ladicí program Xamarin.iOS používá [logicky ladicí program Mono](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/), což znamená, že generovaný kód a Mono prostředí runtime spolupracovat s IDE a poskytuje prostředí ladění. To se liší od ladicí pevný programy jako LLDB nebo MDB, které řídí program bez vědomí nebo spolupráce vyladěnou programu.

## <a name="setting-breakpoints"></a>Nastavení zarážek

Když budete chtít spustit ladění aplikace prvním krokem je [nastavit zarážky aplikace](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/). Děje se tak kliknutím v oblasti okraje editoru vedle číslo řádku kódu, kterou chcete rozdělit na:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging1.png "Nastavení zarážek")](debugging-in-xamarin-ios-images/debugging1.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging1a.png "Nastavení zarážek")](debugging-in-xamarin-ios-images/debugging1a.png#lightbox)

-----

Můžete zobrazit všechny zarážky, které byly nastaveny ve vašem kódu přechodem na **zarážky pad**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/image0a.png "Odsazení zarážky")](debugging-in-xamarin-ios-images/image0a.png#lightbox)

 Pokud pad zarážky nezobrazí automaticky, můžete u ní nastavit viditelné výběrem _zobrazení > ladění Windows > zarážky_
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/image0.png "Odsazení zarážky")](debugging-in-xamarin-ios-images/image0.png#lightbox)

 Pokud pad zarážky nezobrazí automaticky, můžete u ní nastavit viditelné výběrem _ladění > Windows > zarážky_
 
-----

Než začnete ladění jakékoli aplikace, vždy zajistěte, že konfigurace je nastavená na **ladění**, protože tato položka obsahuje sadu užitečné nástroje k podpoře ladění například zarážky pomocí vizualizérech dat a zobrazení zásobníku volání:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7.png "Ladění na simulátoru")](debugging-in-xamarin-ios-images/debugging7.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7a.png "ladění na fyzické zařízení")](debugging-in-xamarin-ios-images/debugging7a.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7c.png "Ladění na simulátoru")](debugging-in-xamarin-ios-images/debugging7c.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7d.png "ladění na fyzické zařízení")](debugging-in-xamarin-ios-images/debugging7d.png#lightbox)

-----

## <a name="start-debugging"></a>Spuštění ladění
Spustit ladění, vyberte cílové zařízení nebo podobné ve vaší IDE:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7b.png "Vyberte cílové zařízení")](debugging-in-xamarin-ios-images/debugging7b.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7e.png "Vyberte cílové zařízení")](debugging-in-xamarin-ios-images/debugging7e.png#lightbox)

-----



Potom nasaďte aplikaci stisknutím **přehrání** tlačítko.

Při zásahu zarážku kód bude zvýrazněná žlutý:

[![](debugging-in-xamarin-ios-images/image2.png "Kód bude zvýrazněná žlutý")](debugging-in-xamarin-ios-images/image2.png#lightbox)

Ladicí nástroje, jako je například zkontrolujete hodnoty objektů, lze použít v tomto okamžiku získat další informace o co se děje ve vašem kódu:

[![](debugging-in-xamarin-ios-images/image3.png "Zobrazení hodnoty barev")](debugging-in-xamarin-ios-images/image3.png#lightbox)

## <a name="conditional-breakpoints"></a>Podmíněné zarážky

Můžete také nastavit pravidla diktování v případech, za kterých by měl nastat zarážku, to je vědět, jak je přidání *podmíněného zarážek*.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pro nastavení podmíněné zarážky, přístup **zarážku – vlastnosti – okno**, což lze provést dvěma způsoby:


- Pokud chcete přidat nový podmíněné zarážky, klikněte pravým tlačítkem na editor okrajem, nalevo od číslo řádku pro kód, který chcete nastavit zarážky a vyberte nové zarážek:

    [![](debugging-in-xamarin-ios-images/image4.png "Vyberte nové zarážek")](debugging-in-xamarin-ios-images/image4.png#lightbox)

- Pokud chcete přidat do existující zarážek podmínku, klikněte pravým tlačítkem na zarážek a vyberte **zarážek vlastnosti** nebo v **zarážky Pad** vyberte tlačítko Vlastnosti, které jsou znázorněné dole:

    [![](debugging-in-xamarin-ios-images/image5.png "Odsazení zarážky")](debugging-in-xamarin-ios-images/image5.png#lightbox)


Potom můžete zadat podmínky, pod kterým chcete zarážku proběhnout:

[![](debugging-in-xamarin-ios-images/image6.png "Zadejte podmínku pro zarážek proběhnout")](debugging-in-xamarin-ios-images/image6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pro nastavení podmíněné zarážky v sadě Visual Studio 2015, nejprve [regulární zarážku](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/). Klikněte pravým tlačítkem myši na zarážce zobrazíte její místní nabídce:

 [![](debugging-in-xamarin-ios-images/image4vs.png "Místní nabídky zarážek")](debugging-in-xamarin-ios-images/image4vs.png#lightbox)

Vyberte **podmínky...**  zobrazíte _nastavení zarážek_ nabídky:

 [![](debugging-in-xamarin-ios-images/image6vs.png "V nabídce nastavení zarážek")](debugging-in-xamarin-ios-images/image6vs.png#lightbox)

Zde můžete zadat podmínky, za kterých mají zarážek proběhnout

Další informace o použití zarážek podmínky v dřívějších verzích sady Visual Studio, najdete v části [dokumentaci sady Visual Studio](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints) v tomto tématu.

-----

## <a name="navigating-through-code"></a>Procházení kódu

Pokud bylo dosaženo zarážky, nástroje pro ladění umožňují získat kontrolu nad spuštění tohoto programu. Rozhraní IDE zobrazí čtyři tlačítka, který vám umožní spustit a projděte kód.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V sadě Visual Studio pro Mac bude vypadat takto:

 [![](debugging-in-xamarin-ios-images/image7.png "Nástroje pro ladění povolit vývojáři získat kontrolu nad spuštění programu")](debugging-in-xamarin-ios-images/image7.png#lightbox)

Jsou to:

- **Play a zastavení** – to bude začít a zastavení provádění kód, dokud na další zarážku.
- **Krokovat s přeskočením** – tato funkce spustí dalším řádku kódu. Pokud na další řádek je volání funkce, krok přes spustit funkci a bude zastavení v dalším řádku kódu _po_ funkce.
- **Krokovat s vnořením** – to budou také spuštěny při dalším řádku kódu. Pokud na další řádek je volání funkce, Krokovat s vnořením se zastaví na prvním řádku funkce, abyste mohli pokračovat, řádek po řádku ladění funkce. Pokud na další řádek není funkce, bude se chovat stejná jako Krokovat s přeskočením.
- **Krokovat s Vystoupením** – to se vrátí na řádek, kde byl volán aktuální funkce.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V sadě Visual Studio bude vypadat takto:

[![](debugging-in-xamarin-ios-images/image7vs.png "Nástroje pro ladění povolit vývojáři získat kontrolu nad spuštění programu")](debugging-in-xamarin-ios-images/image7vs.png#lightbox)

Jsou to:

- **Play a zastavení** – to bude začít a zastavení provádění kód, dokud na další zarážku.
- **Krokovat s přeskočením (F11)** – tato funkce spustí dalším řádku kódu. Pokud na další řádek je volání funkce, krok přes spustit funkci a bude zastavení v dalším řádku kódu _po_ funkce.
- **Krokovat s vnořením (F10)** – to budou také spuštěny při dalším řádku kódu. Pokud na další řádek je volání funkce, Krokovat s vnořením se zastaví na prvním řádku funkce, abyste mohli pokračovat, řádek po řádku ladění funkce. Pokud na další řádek není funkce, bude se chovat stejná jako Krokovat s přeskočením.
- **Krokovat s Vystoupením (Shift + F11)** – to se vrátí na řádek, kde byl volán aktuální funkce.

Další informace v dokumentaci hloubku na ladění najdete [přejděte kódu s Visual Studio Debugger](https://docs.microsoft.com/visualstudio/debugger/navigating-through-code-with-the-debugger).

-----

### <a name="breakpoints"></a>Zarážky

Je důležité upozorníme na dává iOS aplikace jenom pár sekund (10) pro spuštění a dokončení `FinishedLaunching` metoda v delegáta aplikace. Pokud aplikace nedokončí tuto metodu za 10 sekund, bude iOS ukončit proces.

To znamená, že je téměř nemožné nastavit zarážky na spuštění kódu vašeho programu. Pokud chcete ladit kód po spuštění, doporučujeme zpoždění některé inicializace a put, do časovač vyvolat metodu, nebo jiné formy metoda zpětného volání, která se spustí po FinishedLaunching ukončilo.

## <a name="device-diagnostics"></a>Diagnostika zařízení

Pokud dojde k chybě nastavení ladicího programu, můžete povolit podrobné diagnostiky přidáním "-v - v - v" k další mtouch argumentů v možnosti projektu. To bude tisknout podrobné informace o chybě do konzoly zařízení.

 <a name="WiFi_Debugging" />

## <a name="wireless-debugging"></a>Bezdrátové ladění

Ve výchozím nastavení v Xamarin.iOS se ladění aplikace na zařízení prostřednictvím připojení USB. Někdy zařízení USB může být potřeba otestovat zapojení nebo odpojením kabelu pro vývoj aplikací používá technologii ExternalAccessory. V takových případech můžete ladění přes bezdrátové sítě.

Další informace o nasazení bezdrátové sítě a ladění, najdete v části [nasazení bezdrátové](~/ios/deploy-test/wireless-deployment.md) průvodce.

<a name="Technical_Details" />

## <a name="technical-details"></a>Technické podrobnosti

Xamarin.iOS používá nové Mono logicky ladicí program. Na rozdíl od standardní Mono ladicí, což je program, který určuje samostatný proces, pomocí rozhraní operačního systému k řízení samostatný proces logicky ladicí program funguje tak, že Mono runtime ladění funkcionalitu prostřednictvím síťový protokol.

Při spuštění aplikace, která má být vyladěnou kontakty ladicího programu a ladicí program začne fungovat. Xamarin.iOS pro sadu Visual Studio Xamarin Mac Agent funguje jako střední man mezi aplikací (v sadě Visual Studio) a ladicí program.

Při spuštěné na zařízení, vyžaduje tento logicky ladicí program spolupráci ladění schématu. To znamená, že vaše binární sestavení při ladění bude větší jako kód je instrumentovány tak, aby obsahovala další kód v každém bodě pořadí pro podporu ladění.

<a name="Accessing_the_Console" />


## <a name="accessing-the-console"></a>Přístup ke konzole

Selhat protokoly a výstup konzoly třídy budou odeslány do konzoly nástroje iPhone. Tato konzola můžete přistupovat pomocí Xcode použitím "Organizátoru." a vybrat zařízení z médií.

Případně, pokud nechcete, aby bylo možné spustit Xcode, můžete použít společnosti Apple [iPhone konfigurační nástroj](http://www.apple.com/support/iphone/enterprise/) přímo přistupovat ke konzole. Tato akce nemá přidané bonusové, že máte přístup protokoly konzoly z počítače s Windows Pokud ladíte problém v poli.

Pro uživatele, Visual Studio v okně výstupu jsou k dispozici několik protokoly, ale měli přepnutí na počítači Mac pro důkladnější a podrobné protokoly.

-----

<a name="Debugging_Mono's_Class_Libraries" />


## <a name="debugging-monos-class-libraries"></a>Ladění na Mono knihovny tříd

Xamarin.iOS se dodává se zdrojovým kódem pro knihovny tříd pro Mono a můžete ho do jednoho kroku z ladicího programu najdete v článku Jak funguje věcí pod pokličkou.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Vzhledem k tomu, že tato funkce vyžaduje další paměť během ladění, to je vypnutý ve výchozím nastavení.


Chcete-li povolit tuto funkci, ujistěte se, **ladění projektu kódu pouze; není kroku do kódu framework** možnost není vybraná pod _Visual Studio pro Mac > Předvolby > ladicí program_ nabídky, jak ukazuje následující níže:

[![](debugging-in-xamarin-ios-images/debugging6.png "Ladění na Mono knihovny tříd")](debugging-in-xamarin-ios-images/debugging6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chcete-li ladit knihovny tříd v sadě Visual Studio, je nutné zakázat **pouze můj kód** pod _ladění > Možnosti_ nabídky. V _ladění > Obecné_ uzlu, zrušte **povolit volbu pouze vlastní kód** políčko:

[![](debugging-in-xamarin-ios-images/debugging6vs.png "Ladění na Mono knihovny tříd")](debugging-in-xamarin-ios-images/debugging6vs.png#lightbox)

-----

Jakmile to uděláte, můžete spustit aplikaci a jeden krok do jakéhokoli Mono na základní třídy knihovny.


## <a name="related-links"></a>Související odkazy

- [Ladění pomocí Xamarinu](/visualstudio/mac/debugging/)
- [Vizualizace dat](/visualstudio/mac/data-visualizations/)
- [Nastavit zarážky](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/)
- [Krok prostřednictvím kódu](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/)
- [Výstupní informace do protokolu okna](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/)
