---
title: Ladění aplikace Xamarin.iOS
description: Tento dokument popisuje způsob použití ladicího programu v sadě Visual Studio pro Mac nebo Visual Studio 2017 pro ladění aplikace Xamarin.iOS, včetně můžete nastavovat zarážky, bezdrátové ladění a další.
ms.prod: xamarin
ms.assetid: 05460010-99E1-DC38-F855-2D691EF54484
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4b21a69e49c8c7fd79de8edac9858c4714657f1c
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242313"
---
# <a name="debugging-xamarinios-apps"></a>Ladění aplikace Xamarin.iOS

_Aplikace Xamarin.iOS můžete ladit pomocí ladicího programu nástroje integrované v sadě Visual Studio pro Mac nebo Visual Studio._

Pomocí sady Visual Studio pro Mac nativní podporu ladění pro ladění jazyka C# a další spravované jazyky kód a použít [LLDB](http://lldb.llvm.org/tutorial.html) když budete potřebovat pro ladění jazyka C, C++ nebo Objective C codethat vám může být propojení s projektu Xamarin.iOS.


> [!NOTE]
> Při kompilaci aplikace v režimu ladění, Xamarin.iOS vygeneruje pomalejší a větší aplikace, jako musí být instrumentována každý jednotlivý řádek kódu. Před uvolněním, ujistěte se, že provést sestavení pro vydání.

Ladicí program Xamarin.iOS je integrován do prostředí (IDE) a umožňuje vývojářům ladit aplikace Xamarin.iOS vytvořených pomocí některé z spravovaných jazycích podporovaných Xamarin.iOS v simulátoru a na zařízení.

Používá ladicí program Xamarin.iOS [Obnovitelně ladicí program Mono](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/), což znamená, že generovaného kódu a modul Mono runtime spolupracovat s integrovaným vývojovým prostředím poskytnout možnosti ladění. To se liší od pevné ladící jako LLDB nebo MDB, které řídí programu bez vědomí nebo spolupráci od laděného programu.

## <a name="setting-breakpoints"></a>Nastavení zarážek

Až budete připravení Začít ladění aplikace prvním krokem je [nastavit zarážky aplikace](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint). To se provádí v oblasti okraje editoru vedle řádku počet kód, který chcete provést přerušení klikněte na:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging1.png "Nastavení zarážek")](debugging-in-xamarin-ios-images/debugging1.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging1a.png "Nastavení zarážek")](debugging-in-xamarin-ios-images/debugging1a.png#lightbox)

-----

Můžete zobrazit všechny zarážky, které jsou nastavené v kódu tak, že přejdete **zarážky panel**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/image0a.png "Panel zarážky")](debugging-in-xamarin-ios-images/image0a.png#lightbox)

 Pokud panel zarážky nezobrazí automaticky, můžete si je viditelné tak, že vyberete _zobrazení > ladění Windows > zarážky_
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/image0.png "Panel zarážky")](debugging-in-xamarin-ios-images/image0.png#lightbox)

 Pokud panel zarážky nezobrazí automaticky, můžete si je viditelné tak, že vyberete _ladit > Windows > zarážky_
 
-----

Před zahájením ladění všech aplikací, vždycky zkontrolujte, že konfiguraci je nastavená na **ladění**, jak tato položka obsahuje sadu nástrojů pro podporu ladění jako je například zarážek, vizualizérů dat pomocí a zobrazení zásobníku volání užitečné:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7.png "Ladění na simulátoru")](debugging-in-xamarin-ios-images/debugging7.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7a.png "ladění na fyzickém zařízení")](debugging-in-xamarin-ios-images/debugging7a.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7c.png "Ladění na simulátoru")](debugging-in-xamarin-ios-images/debugging7c.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7d.png "ladění na fyzickém zařízení")](debugging-in-xamarin-ios-images/debugging7d.png#lightbox)

-----

## <a name="start-debugging"></a>Spustit ladění
Chcete-li spustit ladění, vyberte cílové zařízení nebo v prostředí (IDE) podobné:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7b.png "Vyberte cílové zařízení")](debugging-in-xamarin-ios-images/debugging7b.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7e.png "Vyberte cílové zařízení")](debugging-in-xamarin-ios-images/debugging7e.png#lightbox)

-----



Potom nasaďte svoji aplikaci stisknutím klávesy **Přehrát** tlačítko.

Při dosažení zarážky kód bude zvýrazněné žlutou barvou:

[![](debugging-in-xamarin-ios-images/image2.png "Kód bude zvýrazněné žlutou barvou")](debugging-in-xamarin-ios-images/image2.png#lightbox)

Ladicí nástroje, jako je například kontrola hodnot objekty, lze v tomto okamžiku získat další informace o tom, co se děje ve vašem kódu:

[![](debugging-in-xamarin-ios-images/image3.png "Zobrazení hodnoty barev")](debugging-in-xamarin-ios-images/image3.png#lightbox)

## <a name="conditional-breakpoints"></a>Podmíněné zarážky

Můžete také nastavit pravidla diktování okolností, za kterých zarážky se budou objevovat, to je známo jako přidávání *podmíněné zarážky*.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud chcete nastavit podmíněné zarážky, přístup **okna Vlastnosti zarážky**, což lze provést dvěma způsoby:


- Chcete-li přidat nový podmíněné zarážky, klikněte pravým tlačítkem myši na okraj editoru, doleva číslo řádku kódu, který chcete nastavit zarážku na a vyberte Nová zarážka:

    [![](debugging-in-xamarin-ios-images/image4.png "Vyberte novou zarážku")](debugging-in-xamarin-ios-images/image4.png#lightbox)

- Přidání podmínky do existující zarážce, klikněte pravým tlačítkem na zarážku a vyberte **vlastnosti zarážky** nebo **zarážky panel** klikněte na tlačítko Vlastnosti znázorněno níže:

    [![](debugging-in-xamarin-ios-images/image5.png "Panel zarážky")](debugging-in-xamarin-ios-images/image5.png#lightbox)


Potom můžete zadat podmínky, u které chcete na zarážku na výskyt:

[![](debugging-in-xamarin-ios-images/image6.png "Zadejte podmínku pro zarážku. dojde k")](debugging-in-xamarin-ios-images/image6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chcete-li nastavit podmíněné zarážky v sadě Visual Studio 2015, nejprve [nastavte zarážku regulární](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint). Klikněte pravým tlačítkem na zarážku pro zobrazení jeho kontextovou nabídku:

 [![](debugging-in-xamarin-ios-images/image4vs.png "V místní nabídce zarážku")](debugging-in-xamarin-ios-images/image4vs.png#lightbox)

Vyberte **podmínky...**  zobrazíte _nastavení zarážek_ nabídky:

 [![](debugging-in-xamarin-ios-images/image6vs.png "V nabídce nastavení zarážek")](debugging-in-xamarin-ios-images/image6vs.png#lightbox)

Tady můžete zadat podmínky, za kterých mají zarážku na výskyt

Další informace o použití podmínky zarážky v dřívějších verzích sady Visual Studio najdete [dokumentace k sadě Visual Studio](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints) v tomto tématu.

-----

## <a name="navigating-through-code"></a>Procházení kódu

Když se dosáhne zarážky, nástroje pro ladění umožňují získat kontrolu nad vykonávání programu. Rozhraní IDE zobrazí čtyři tlačítka, díky tomu můžete spustit a krokovat kód.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V sadě Visual Studio pro Mac bude vypadat nějak takto:

 [![](debugging-in-xamarin-ios-images/image7.png "Ladicí nástroje umožňují vývojářům získat kontrolu nad spouštěním programu")](debugging-in-xamarin-ios-images/image7.png#lightbox)

Toto jsou:

- **Přehrát/Zastavit** – to se begin/zastavení provádění kódu, až k další zarážce.
- **Krokovat s přeskočením** – tím se spustí další řádek kódu. Pokud se na další řádek je volání funkce, krok více než spuštění funkce a bude ukončení na další řádek kódu _po_ funkce.
- **Krokovat s vnořením** – to také provede další řádek kódu. Pokud na další řádek je volání funkce, Krokovat s vnořením se zastaví na prvním řádku funkci, abyste mohli pokračovat v ladění řádek po řádku funkce. Pokud se na další řádek není funkce, budou chovat stejné jako Krokovat s přeskočením.
- **Krokovat s Vystoupením** – to se vrátí k řádku, kde byla volána aktuální funkce.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V sadě Visual Studio bude vypadat nějak takto:

[![](debugging-in-xamarin-ios-images/image7vs.png "Ladicí nástroje umožňují vývojářům získat kontrolu nad spouštěním programu")](debugging-in-xamarin-ios-images/image7vs.png#lightbox)

Toto jsou:

- **Přehrát/Zastavit** – to se begin/zastavení provádění kódu, až k další zarážce.
- **Krokovat s přeskočením (F11)** – tím se spustí další řádek kódu. Pokud se na další řádek je volání funkce, krok více než spuštění funkce a bude ukončení na další řádek kódu _po_ funkce.
- **Krokovat s vnořením (F10)** – to také provede další řádek kódu. Pokud na další řádek je volání funkce, Krokovat s vnořením se zastaví na prvním řádku funkci, abyste mohli pokračovat v ladění řádek po řádku funkce. Pokud se na další řádek není funkce, budou chovat stejné jako Krokovat s přeskočením.
- **Krokovat s Vystoupením (Shift + F11)** – to se vrátí k řádku, kde byla volána aktuální funkce.

Další informace v dokumentaci k hloubka v ladění, najdete v článku [procházení kódu s ladicím programu sady Visual Studio](https://docs.microsoft.com/visualstudio/debugger/navigating-through-code-with-the-debugger).

-----

### <a name="breakpoints"></a>Zarážky

Je důležité zdůraznit, že iOS poskytuje aplikacím jenom několik sekund (10) spuštění a dokončení `FinishedLaunching` metoda delegáta aplikace. Pokud aplikace nedokončí tuto metodu za 10 sekund, bude iOS ukončit proces.

To znamená, že je téměř nemožné nastavit zarážky v kódu při spuštění programu. Pokud chcete ladit spouštěcí kód, by měl zpoždění některé z jeho inicializaci a put, která na časovač vyvolat metodu nebo v nějaké jiné podobě metody zpětného volání, která se spustí po FinishedLaunching byl ukončen.

## <a name="device-diagnostics"></a>Diagnostika zařízení

Pokud dojde při nastavování ladicího programu, můžete povolit diagnostiku podrobné tak, že přidáte "-v - v - v" k další argumenty mtouch v možnostech projektu. Vytiskne se podrobné informace o chybě ke konzole zařízení.

 <a name="WiFi_Debugging" />

## <a name="wireless-debugging"></a>Bezdrátové ladění

Ve výchozím nastavení v Xamarin.iosu je vaše aplikace na svých zařízeních ladit přes připojení USB. Někdy USB zařízení může být nutné k testování připojení nebo odpojením kabelu pro vývoj aplikací s využitím ExternalAccessory. V takových případech můžete ladění přes bezdrátové sítě.

Další informace o nasazení bezdrátové sítě a ladění, naleznete [bezdrátovému nasazení](~/ios/deploy-test/wireless-deployment.md) průvodce.

<a name="Technical_Details" />

## <a name="technical-details"></a>Technické podrobnosti

Xamarin.iOS používá nové softwarové ladicí program Mono. Na rozdíl od standardních ladicí program Mono tedy program, který se řídí samostatným procesem to s využitím rozhraní operační systém pro řízení samostatný proces obnovitelně ladicí program funguje tak, že modul Mono runtime vystavit funkce ladění přes přenosový protokol.

Při spuštění aplikace, která má být ladění kontakty ladicí program a ladicí program začne pracovat. V Xamarin.iosu pro Visual Studio Xamarin Mac Agent funguje jako střední man mezi aplikací (v sadě Visual Studio) a ladicí program.

Tato obnovitelně ladicí program vyžaduje schéma ladění spolupráce při spuštění na zařízení. To znamená, že binární soubor sestavení při ladění bude větší, protože kód je instrumentováno tak, aby obsahovala zvláštní kód v každém okamžiku pořadí v zájmu podpory ladění.

<a name="Accessing_the_Console" />


## <a name="accessing-the-console"></a>Přístup ke konzole

Poruše protokolů a výstup konzoly třídy se odešlou do konzoly pro iPhone. Tato konzola můžete přistupovat pomocí Xcode pomocí "Médií" a vyberte vaše zařízení z médií.

Případně, pokud nechcete, aby se spouštěla Xcode, můžete od společnosti Apple [iPhone konfigurační nástroj](http://www.apple.com/support/iphone/enterprise/) přímo přístup ke konzole. Tato akce nemá přidání bonusové, že se dá dostat protokoly konzoly z počítače s Windows Pokud ladíte problém v poli.

Pro uživatele sady Visual Studio v okně výstupu jsou k dispozici několik protokolů, ale by měla přepnutí na počítači Mac pro důkladnější a podrobné protokoly.

-----

<a name="Debugging_Mono's_Class_Libraries" />


## <a name="debugging-monos-class-libraries"></a>Ladění knihoven tříd pro Mono

Xamarin.iOS se dodává se zdrojovým kódem pro knihovny tříd pro Mono, a to do jednoho kroku z ladicího programu můžete zobrazit, jak věci pracují pod pokličkou.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Protože tato funkce vyžaduje další paměť během ladění, to je vypnuto ve výchozím nastavení.


Pokud chcete tuto funkci povolit, ujistěte se, že **ladit jenom kód projektu, Nekrokovat s vnořením do kódu architektury** možnost vypnuta v rámci _Visual Studio for Mac > Předvolby > ladicí program_ nabídky, jak je znázorněno níže:

[![](debugging-in-xamarin-ios-images/debugging6.png "Ladění knihoven tříd pro Mono")](debugging-in-xamarin-ios-images/debugging6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

K ladění knihoven tříd v sadě Visual Studio, je nutné zakázat **pouze můj kód** pod _ladit > Možnosti_ nabídky. V _ladění > Obecné_ uzlu, zrušte **povolit volbu pouze vlastní kód** zaškrtávací políčko:

[![](debugging-in-xamarin-ios-images/debugging6vs.png "Ladění knihoven tříd pro Mono")](debugging-in-xamarin-ios-images/debugging6vs.png#lightbox)

-----

Jakmile to uděláte, můžete spustit svoji aplikaci a jeden krok do žádné Mono základní třídy knihovny.


## <a name="related-links"></a>Související odkazy

- [Ladění s využitím kódu Xamarin](/visualstudio/mac/debugging/)
- [Vizualizace dat](/visualstudio/mac/data-visualizations/)
- [Nastavit zarážku](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)
- [Procházejte kódem po krocích](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)
- [Výstupní informace do protokolu okna](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)
