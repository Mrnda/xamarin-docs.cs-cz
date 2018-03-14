---
title: Chyby Xamarin.iOS
ms.topic: article
ms.prod: xamarin
ms.assetid: 9F76162B-D622-45DA-996B-2FBF8017E208
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 32a73232667e54eef7536f0bb0d1baa190269d8e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="xamarinios-errors"></a>Chyby Xamarin.iOS

## <a name="mt0xxx-mtouch-error-messages"></a>MT0xxx: mtouch chybové zprávy

Například parametry, prostředí, chybí nástroje.

<!--
 MT0xxx mtouch itself, e.g. parameters, environment (e.g. missing tools)
 https://github.com/xamarin/xamarin-macios/blob/master/tools/mtouch/error.cs
    -->

### <a name="a-namemt0000mt0000-unexpected-error---please-fill-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT0000"/>MT0000: Neočekávaná chyba – vyplňte sestavu chyb na http://bugzilla.xamarin.com

Došlo k neočekávané chybě. Prosím [hlášení o chybě](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s nejblíže tolik informací, včetně:

* Úplné sestavení protokoly, s maximální podrobnosti (například `-v -v -v -v` v **mtouch další argumenty**);
* Minimální testovacího případu reprodukujte chybu. a
* Všechny verze – informace

Nejjednodušší způsob, jak získat informace o přesnou verzi se má používat **Visual Studio pro Mac** nabídce **o sadě Visual Studio pro Mac** položku **zobrazit podrobnosti** tlačítko, kopírování a vkládání verze – informace (můžete použít **kopie informace** tlačítko).

### <a name="a-namemt0001mt0001--devname-was-provided-without-any-device-specific-action"></a><a name="MT0001"/>MT0001: "-devname' bylo zadáno bez jakékoli akce specifické pro zařízení

Toto je upozornění, které jsou vydávány Pokud - devname předaný mtouch Pokud žádná akce specifické pro zařízení (-logdev/installdev/killdev/launchdev /-listapps) byl požadován.

### <a name="a-namemt0002mt0002-could-not-parse-the-environment-variable-"></a><a name="MT0002"/>MT0002: Nebylo možné rozložit proměnnou prostředí *.

Této chybě dochází, pokud se pokusíte nastavit prostředí neplatný klíč = hodnota proměnné pár. Správný formát je: `mtouch --setenv=VARIABLE=VALUE`

### <a name="a-namemt0003mt0003-application-name-exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a><a name="MT0003"/>MT0003: Název aplikace "* .exe, je v konfliktu s SDK nebo produktu, název sestavení (.dll).

Název spustitelného souboru sestavení a název aplikace nesmí shodovat s názvem všechny knihovny DLL v aplikaci. Změňte prosím název vaší spustitelný soubor.

### <a name="a-namemt0004mt0004-new-refcounting-logic-requires-sgen-to-be-enabled-too"></a><a name="MT0004"/>MT0004: Nové logiku refcounting vyžaduje SGen příliš povolení.

Pokud povolíte rozšíření refcounting musíte zároveň povolit SGen garbage collector v projektu iOS sestavení možnosti (karta Upřesnit).

Počínaje Xamarin.iOS 7.2.1 Tento požadavek byl zrušen, nové logiku refcounting lze je aktivovat pomocí Boehm a Kolektory SGen – uvolňování paměti.

### <a name="a-namemt0005mt0005-the-output-directory--does-not-exist"></a><a name="MT0005"/>MT0005: Výstupnímu adresáři * neexistuje.

Vytvořte adresář.

Tato chyba se už nevygenerovala, mtouch automaticky vytvoří adresář, pokud neexistuje.

### <a name="a-namemt0006mt0006-there-is-no-devel-platform-at--use---platformplat-to-specify-the-sdk"></a><a name="MT0006"/>MT0006: Neexistuje žádné devel platformy *, použijte – platforma = PLAT k určení sady SDK.

Xamarin.iOS nelze najít adresář sady SDK v umístění uvedený v chybové zprávě. Ověřte, zda je cesta správná.

### <a name="a-namemt0007mt0007-the-root-assembly--does-not-exist"></a><a name="MT0007"/>MT0007: Sestavení kořenové * neexistuje.

Xamarin.iOS nelze najít sestavení v umístění uvedený v chybové zprávě. Ověřte, zda je cesta správná.

### <a name="a-namemt0008mt0008-you-should-provide-one-root-assembly-only-found--assemblies-"></a><a name="MT0008"/>MT0008: Měli byste jim poskytnout jeden kořenový sestavení pouze, který se nachází # sestavení: *.

Více než jednom sestavení kořenové byl předán mtouch, zatímco může existovat pouze jeden kořenový sestavení.

### <a name="a-namemt0009mt0009-error-while-loading-assemblies-"></a><a name="MT0009"/>MT0009: Chyba při načítání sestavení: *.

Došlo k chybě při načítání sestavení odkazů na kořenový sestavení. Další informace může zajišťovat ve výstupu sestavení.

### <a name="a-namemt0010mt0010-could-not-parse-the-command-line-arguments-"></a><a name="MT0010"/>MT0010: Nebylo možné rozložit argumenty příkazového řádku: *.

Došlo k chybě při analýze argumenty příkazového řádku. Ověřte, že jsou správně.

### <a name="a-namemt0011mt0011--was-built-against-a-more-recent-runtime--than-monotouch-supports"></a><a name="MT0011"/>MT0011: * byl sestaven s novější runtime (*), než MonoTouch podporuje.

Toto upozornění je obvykle uvést, protože projekt odkazuje na knihovny tříd, který nebyl vytvořen pomocí Xamarin.iOS BCL.

Stejným způsobem jako aplikace pomocí .NET 4.0 SDK nemusí fungovat v systému jenom podporu rozhraní .NET 2.0 knihovnu vyvíjené v rozhraní .NET 4.0, nemusí fungovat na Xamarin.iOS, se může používat rozhraní API není k dispozici na Xamarin.iOS.

Obecné řešení je sestavení knihovny jako knihovny tříd Xamarin.iOS. To můžete provést vytvořením nového projektu Xamarin.iOS knihovny tříd a přidejte do ní všechny zdrojové soubory. Pokud jste zdrojový kód pro knihovnu, doporučujeme obraťte se na dodavatele a žádostí, aby umožňovala Xamarin.iOS kompatibilní verzi své knihovny.

### <a name="a-namemt0012mt0012-incomplete-data-is-provided-to-complete-"></a><a name="MT0012"/>MT0012: Neúplná data je k dispozici k dokončení *.

Tato chyba není hlášena již v aktuální verzi Xamarin.iOS.

### <a name="a-namemt0013mt0013-profiling-support-requires-sgen-to-be-enabled-too"></a><a name="MT0013"/>MT0013: Profilace podporu vyžaduje sgen příliš povolení.

SGen – (--sgen) musí být povolena, pokud profilace (– profilace) je povoleno.

### <a name="a-namemt0014mt0014-the-ios--sdk-does-not-support-building-applications-targeting-"></a><a name="MT0014"/>MT0014: IOS * SDK nepodporuje vytváření aplikací cílení *.

K tomu může dojít v následujících případech:

*  ARMv6 zapnutá a je nainstalovaný Xcode 4.5 nebo novější.
*  ARMv7s zapnutá a je nainstalovaný Xcode 4.4 nebo dřívější.

Ověřte, že nainstalovaná verze systému Xcode podporuje zvolené architektury.

### <a name="a-namemt0015mt0015-invalid-abi--supported-abis-are-i386-x8664--armv7-armv7llvm-armv7llvmthumb2-armv7s-armv7sllvm-armv7sllvmthumb2-arm64-and-arm64llvm"></a><a name="MT0015"/>MT0015: Neplatný ABI: *. Jsou podporované bis : i386, x86_64, armv7, armv7 + llvm, armv7 + llvm + thumb2, armv7s, armv7s + llvm, armv7s + llvm + thumb2, arm64 a arm64 + llvm.

Byl předán neplatný ABI mtouch. Zadejte platný ABI.

### <a name="a-namemt0016mt0016-the-option--has-been-deprecated"></a><a name="MT0016"/>MT0016: Možnost * je zastaralá.

Možnost uvedených mtouch je zastaralá a budou ignorovány.

### <a name="a-namemt0017mt0017-you-should-provide-a-root-assembly"></a><a name="MT0017"/>MT0017: By měl poskytovat kořenové sestavení.

Je potřeba zadat sestavení kořenové (obvykle hlavní spustitelný soubor) při vytváření aplikace.

### <a name="a-namemt0018mt0018-unknown-command-line-argument-"></a><a name="MT0018"/>MT0018: Neznámý argument příkazového řádku: *.

Mtouch nerozpoznal argument příkazového řádku, který je uvedený v chybové zprávě.

### <a name="a-namemt0019mt0019-only-one---loginstallkilllaunchdev-or---launchdebugsim-option-can-be-used"></a><a name="MT0019"/>MT0019: Pouze jeden--[protokolu | nainstalovat | kill | spusťte] dev nebo--[spustit | ladění] sim možnost lze použít.

Existuje několik možností pro mtouch, která se nedá použít současně:

-  --logdev
-  --installdev
-  --killdev
-  --launchdev
-  --launchdebug
-  --launchsim

### <a name="a-namemt0020mt0020-the-valid-options-for--are-"></a><a name="MT0020"/>MT0020 Platné možnosti '\*"se"\*'.

### <a name="a-namemt0021mt0021-cannot-compile-using-gccg---use-gcc-when-using-the-static-registrar-this-is-the-default-when-compiling-for-device-either-remove-the---use-gcc-flag-or-use-the-dynamic-registrar---registrardynamic"></a><a name="MT0021"/>Nelze MT0021 kompilace pomocí RSZ / g ++ (--použití RSZ) při používání statické registrátora (Toto je výchozí nastavení, když kompilujete pro zařízení). Buď odeberte--použití RSZ příznak nebo použít dynamické registrátora (--registrátora: dynamické).

### <a name="a-namemt0022mt0022-the-options---unsupported--enable-generics-in-registrar-and---registrar-are-not-compatible"></a><a name="MT0022"/>MT0022 Možnosti '– Nepodporovaná – povolit – obecné typy v registrátora' a ' – registrátora ' nejsou kompatibilní.

Odebrat obě možnosti `--unsupported--enable-generics-in-registrar` a `--registrar`. Obecné typy počínaje Xamarin.iOS 7.2.1 registrátora výchozí podporuje.

Už se zobrazí tato chyba (argument příkazového řádku `--unsupported--enable-generics-in-registrar` byl odebrán z mtouch).

### <a name="a-namemt0023mt0023-application-name-exe-conflicts-with-another-user-assembly"></a><a name="MT0023"/>Název aplikace MT0023 ' * .exe, je v konfliktu s jinou sestavení uživatele.

Název spustitelného souboru sestavení a název aplikace nesmí shodovat s názvem všechny knihovny DLL v aplikaci. Změňte prosím název vaší spustitelný soubor.

### <a name="a-namemt0024mt0024-could-not-find-required-file-"></a><a name="MT0024"/>MT0024 nelze najít požadovaný soubor ' *'.

### <a name="a-namemt0025mt0025-no-sdk-version-was-provided-please-add---sdkxy-to-specify-which-ios-sdk-should-be-used-to-build-your-application"></a><a name="MT0025"/>Zadaná verze sady SDK MT0025 ne. Přidejte `--sdk=X.Y` k určení které iOS SDK slouží k vytvoření aplikace.

### <a name="a-namemt0026mt0026-could-not-parse-the-command-line-argument--"></a><a name="MT0026"/>Argument příkazového řádku MT0026 může analyzovat ' *': *

### <a name="a-namemt0027mt0027-the-options--and--are-not-compatible"></a><a name="MT0027"/>MT0027 Možnosti se\*'a'\*' nejsou kompatibilní.

### <a name="a-namemt0028mt0028-cannot-enable-pie--pie-when-targeting-ios-41-or-earlier-please-disable-pie--piefalse-or-set-the-deployment-target-to-at-least-ios-42"></a><a name="MT0028"/>MT0028 nelze povolit KRUHOVÝ (-pie) Pokud je cílem iOS 4.1 nebo starší. VÝSEČOVÉ zakažte (-kruhový: false), nebo nastavte cíl nasazení alespoň iOS 4.2

### <a name="a-namemt0029mt0029-repl---enable-repl-is-only-supported-in-the-simulator---sim"></a><a name="MT0029"/>MT0029: REPL (– Povolit repl) je podporována pouze v simulátoru (--sim).

REPL je podporována, pouze pokud vytváříte pro simulátoru. To znamená, že pokud předáte `--enable-repl` k mtouch, je třeba předat také `--sim`.

### <a name="a-namemt0030mt0030-the-executable-name--and-the-app-name--are-different-this-may-prevent-crash-logs-from-getting-symbolicated-properly"></a><a name="MT0030"/>MT0030: Název spustitelného souboru (\*) a název aplikace (\*) se liší, to může zabránit havárií protokoly získávání symbolicated správně.

Když Xcode symbolicates (přeloží adresy paměti funkce názvy a soubor nebo čáry čísla) proces může selhat, pokud spustitelný soubor a aplikací mít odlišné názvy (bez přípony).

Chcete-li odstranit tento změňte "Název aplikace" v projektu možnosti sestavení nebo iOS aplikací, nebo změňte 'název sestavení, ve výstupu sestavení nebo možnosti projektu.

### <a name="a-namemt0031mt0031-the-command-line-arguments---enable-background-fetch-and---launch-for-background-fetch-require---launchsim-too"></a><a name="MT0031"/>MT0031: Argumenty příkazového řádku, – enable--načítání na pozadí ' a ' – spuštění pro pozadí načítání se vyžadují ' – launchsim' příliš.

### <a name="a-namemt0032mt0032-the-option---debugtrack-is-ignored-unless---debug-is-also-specified"></a><a name="MT0032"/>MT0032: Možnost "– debugtrack' se ignoruje, pokud se – ladění se rovněž je zadán.

### <a name="a-namemt0033mt0033--a-xamarinios-project-must-reference-either-monotouchdll-or-xamariniosdll"></a><a name="MT0033"/>Projektu MT0033 A Xamarin.iOS musí odkazovat buď monotouch.dll nebo Xamarin.iOS.dll

### <a name="a-namemt0034mt0034--cannot-include-both-monotouchdll-and-xamariniosdll-in-the-same-xamarinios-project----is-referenced-explicitly-while--is-referenced-by-"></a><a name="MT0034"/>MT0034 nesmí obsahovat 'monotouch.dll' a "Xamarin.iOS.dll" ve stejném projektu Xamarin.iOS - '\*' je výslovně odkazována při '\*' odkazuje ' *'.

<!-- MT0035 unused -->

### <a name="a-namemt0036mt0036--cannot-launch-a--simulator-for-a--app-please-enable-the-correct-architectures-in-your-projects-ios-build-options-advanced-page"></a><a name="MT0036"/>Nelze MT0036 spuštění * simulátor pro * aplikace. Povolte správné architecture(s) v projektu na iOS možnosti sestavení (Upřesnit).

### <a name="a-namemt0037mt0037--monotouchdll-is-not-64-bit-compatible-either-reference-xamariniosdll-or-do-not-build-for-a-64-bit-architecture-arm64-andor-x8664"></a><a name="MT0037"/>MT0037 monotouch.dll je 64-bit kompatibilní. Buď odkazovat Xamarin.iOS.dll nebo nevytvářejte pro 64bitová architektura (ARM64 nebo x86_64).

### <a name="a-namemt0038mt0038--the-old-registrars---registraroldstaticolddynamic-are-not-supported-when-referencing-xamariniosdll"></a><a name="MT0038"/>MT0038 Staré registrátorů (--registrátora: oldstatic | olddynamic) při odkazování na Xamarin.iOS.dll nejsou podporovány.

### <a name="a-namemt0039mt0039--applications-targeting-armv6-cannot-reference-xamariniosdll"></a><a name="MT0039"/>MT0039 aplikace cílený na ARMv6 nesmí odkazovat na Xamarin.iOS.dll.

### <a name="a-namemt0040mt0040--could-not-find-the-assembly--referenced-by-"></a><a name="MT0040"/>MT0040 nelze najít sestavení "\*', odkazovaná'\*'.

### <a name="a-namemt0041mt0041--cannot-reference-both-monotouchdll-and-xamariniosdll"></a><a name="MT0041"/>MT0041 nesmí odkazovat na 'monotouch.dll' a 'Xamarin.iOS.dll'.

### <a name="a-namemt0042mt0042--no-reference-to-either-monotouchdll-or-xamariniosdll-was-found-a-reference-to-monotouchdll-will-be-added"></a><a name="MT0042"/>Nebyl nalezen žádný MT0042 odkaz monotouch.dll nebo Xamarin.iOS.dll. Přidá odkaz na monotouch.dll.

### <a name="a-namemt0043mt0043-the-boehm-garbage-collector-is-currently-not-supported-when-referencing-xamariniosdll-the-sgen-garbage-collector-has-been-selected-instead"></a><a name="MT0043"/>MT0043: Boehm garbage collector v současné době se nepodporuje při odkazování na 'Xamarin.iOS.dll'. SGen – uvolňování vybral místo.

S projekty jednotné je podporována pouze SGen – uvolňování paměti. Ujistěte se, neexistují žádné další mtouch příznaky určující Boehm jako uvolňování paměti.

### <a name="a-namemt0044mt0044---listsim-is-only-supported-with-xcode-60-or-later"></a><a name="MT0044"/>MT0044:--listsim je podporována pouze s Xcode 6.0 nebo novější.

Nainstalujte novou verzi Xcode.

### <a name="a-namemt0045mt0045---extension-is-only-supported-when-using-the-ios-80-or-later-sdk"></a><a name="MT0045"/>MT0045:--rozšíření je podporována pouze při použití iOS SDK 8.0 (nebo novější).

<!-- MT0046 is not reported anymore -->

### <a name="a-namemt0047mt0047-the-minimum-deployment-target-for-unified-applications-is-511-the-current-deployment-target-is--please-select-a-newer-deployment-target-in-your-projects-ios-application-options"></a><a name="MT0047"/>MT0047: Cíl minimální nasazení pro aplikace Unified je 5.1.1, je aktuální cíl nasazení ' *'. V projektu na iOS aplikaci možnosti vyberte novější nasazení cíl.

<!-- MT0048 is not reported anymore -->

### <a name="a-namemt0049mt0049-framework-is-supported-only-if-deployment-target-is-80-or-later--features-might-not-work-correctly"></a><a name="MT0049"/>MT0049: *.framework je podporováno pouze v případě, že cíl nasazení je 8.0 nebo novější. * funkce nemusí pracovat správně.

Zadaný framework není podporována ve verzi iOS, které odkazuje cíl nasazení. Buď aktualizujte cíl nasazení na novější verzi iOS, nebo odstraňte využití zadaného rámce z aplikace.

<!-- MT0050 is not reported anymore -->

### <a name="a-namemt0051mt0051-xamarinios--requires-xcode-50-or-later-the-current-xcode-version-found-in--is-"></a><a name="MT0051"/>MT0051: Xamarin.iOS * vyžaduje Xcode 5.0 nebo novější. Aktuální verze Xcode (v nalezen *) je *.

Nainstalujte novější Xcode.

### <a name="a-namemt0052mt0052-no-command-specified"></a><a name="MT0052"/>MT0052: Zadaný žádný příkaz.

Pro mtouch nebyla zadána žádná akce.

<!-- 0053 is used by mmp -->

### <a name="a-namemt0054mt0054-unable-to-canonicalize-the-path--"></a><a name="MT0054"/>MT0054: Nelze canonicalize cestu ' *': *

Jedná se o vnitřní chybu. Pokud se zobrazí tato chyba, Oznamte chybu [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt0055mt0055-the-xcode-path--does-not-exist"></a><a name="MT0055"/>MT0055: Xcode cesta ' *' neexistuje.

Cesta Xcode předaným pomocí `--sdkroot` neexistuje. Zadejte platnou cestu.

### <a name="a-namemt0056mt0056-cannot-find-xcode-in-the-default-location-applicationsxcodeapp-please-install-xcode-or-pass-a-custom-path-using---sdkroot-path"></a><a name="MT0056"/>MT0056: Nelze najít Xcode ve výchozím umístění (nebo Applications/Xcode.app). Nainstalujte Xcode nebo předat vlastní cesta pomocí – sdkroot <path>.
### <a name="a-namemt0057mt0057-cannot-determine-the-path-to-xcodeapp-from-the-sdk-root--please-specify-the-full-path-to-the-xcodeapp-bundle"></a><a name="MT0057"/>MT0057: Nelze zjistit cestu k Xcode.app z kořene sdk ' *'. Zadejte úplnou cestu k sadě Xcode.app.

Cesta předaným pomocí `--sdkroot` neurčuje platnou aplikaci Xcode. Zadejte cestu k aplikaci pro Xcode.

### <a name="a-namemt0058mt0058-the-xcodeapp--is-invalid-the-file--does-not-exist"></a><a name="MT0058"/>MT0058: Xcode.app '\*' je neplatný (soubor '\*' neexistuje).

Cesta předaným pomocí `--sdkroot` neurčuje platnou aplikaci Xcode. Zadejte cestu k aplikaci pro Xcode.

### <a name="a-namemt0059mt0059-could-not-find-the-currently-selected-xcode-on-the-system-"></a><a name="MT0059"/>MT0059: Nelze nalézt aktuálně vybrané Xcode systému: *

### <a name="a-namemt0060mt0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned--but-that-directory-does-not-exist"></a><a name="MT0060"/>MT0060: Nelze nalézt aktuálně vybrané Xcode v systému. 'xcode – vybrat – tisk path' vrátil ' *', ale tento adresář neexistuje.

### <a name="a-namemt0061mt0061-no-xcodeapp-specified-using---sdkroot-using-the-system-xcode-as-reported-by-xcode-select---print-path-"></a><a name="MT0061"/>MT0061: Žádné zadán (pomocí – sdkroot), Xcode.app pomocí systému Xcode vykazované 'xcode – vybrat – tisk path': *

Toto je informativní upozornění, která vysvětluje, který bude Xcode použít, protože nebyl zadán.

### <a name="a-namemt0062mt0062-no-xcodeapp-specified-using---sdkroot-or-xcode-select---print-path-using-the-default-xcode-instead-"></a><a name="MT0062"/>MT0062: Žádné zadán (pomocí – sdkroot nebo "xcode – vyberte – tisk cestu"), použijte místo něj výchozí Xcode Xcode.app: *

Toto je informativní upozornění, která vysvětluje, který bude Xcode použít, protože nebyl zadán.

### <a name="a-namemt0063mt0063-cannot-find-the-executable-in-the-extension--no-cfbundleexecutable-entry-in-its-infoplist"></a><a name="MT0063"/>MT0063: Nelze najít spustitelný soubor v rozšíření * (žádná je položka CFBundleExecutable v Info.plist)

Každý Info.plist musí mít spustitelný soubor (pomocí položka CFBundleExecutable), ale položku by měl být vygenerován automaticky během sestavení.

Obvykle to ukazuje na chybu v Xamarin.iOS; prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s testovacího případu.

### <a name="a-namemt0064mt0064-xamarinios-only-supports-embedded-frameworks-with-unified-projects"></a><a name="MT0064"/>MT0064: Xamarin.iOS podporuje pouze vložené architektury s Unified projekty.

Xamarin.iOS podporuje vložené architektury pouze v případě použití unifikované API; Aktualizujte prosím projektu pro použití unifikované API.

### <a name="a-namemt0065mt0065-xamarinios-only-supports-embedded-frameworks-when-deployment-target-is-at-least-80-current-deployment-target--embedded-frameworks-"></a><a name="MT0065"/>MT0065: Xamarin.iOS podporuje pouze vložené architektury při nasazení cíl je alespoň 8.0 (aktuální cíl nasazení: * vložených architektury: *)

Xamarin.iOS vložené architektury podporuje pouze, pokud jsou cílem nasazení alespoň 8.0 (protože starších verzí systému iOS nepodporuje vložené rozhraní).

Aktualizujte prosím cíl nasazení v projektu Info.plist 8.0 nebo vyšším.

### <a name="a-namemt0066mt0066-invalid-build-registrar-assembly-"></a><a name="MT0066"/>MT0066: Neplatný sestavení registrátora sestavení: *

Obvykle to ukazuje na chybu v Xamarin.iOS; prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s testovacího případu.

### <a name="a-namemt0067mt0067-invalid-registrar-"></a><a name="MT0067"/>MT0067: Neplatný registrátora: *

Obvykle to ukazuje na chybu v Xamarin.iOS; prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s testovacího případu.

### <a name="a-namemt0068mt0068-invalid-value-for-target-framework-"></a><a name="MT0068"/>MT0068: Neplatná hodnota pro cílové rozhraní: *.

Představuje rozhraní neplatný cíl byl předán pomocí--argument cílové rozhraní. Zadejte platné cílové rozhraní.

<!--### <a name="MT0069"/>MT0069: currently unused -->

### <a name="a-namemt0070mt0070-invalid-target-framework--valid-target-frameworks-are-"></a><a name="MT0070"/>MT0070: Neplatný cílový framework: *. Jsou platné cílové rozhraní: *.

Představuje rozhraní neplatný cíl byl předán pomocí--argument cílové rozhraní. Zadejte platné cílové rozhraní.

### <a name="a-namemt0071mt0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinios-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a><a name="MT0071"/>MT0071: Neznámý platformy: *. Obvykle to ukazuje na chybu v Xamarin.iOS; Sestava chyb prosím soubor na http://bugzilla.xamarin.com s testovacího případu.

Obvykle to ukazuje na chybu v Xamarin.iOS; prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s testovacího případu.

### <a name="a-namemt0072mt0072-extensions-are-not-supported-for-the-platform-"></a><a name="MT0072"/>MT0072: Rozšíření nejsou podporovány pro platformu ' *'.

Obvykle to ukazuje na chybu v Xamarin.iOS; prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s testovacího případu.

### <a name="a-namemt0073mt0073-xamarinios--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a><a name="MT0073"/>MT0073: Xamarin.iOS * nepodporuje cílem nasazení * (minimum je *). Vyberte novější nasazení cíl v Info.plist vašeho projektu.

Cíl minimální nasazení je uvedený v chybové zprávě; Vyberte novější nasazení cíl v projektu Info.plist.

Pokud není možné aktualizaci cíl nasazení, použijte starší verze Xamarin.iOS.

### <a name="a-namemt0074mt0074-xamarinios--does-not-support-a-minimum-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinios"></a><a name="MT0074"/>MT0074: Xamarin.iOS * nepodporuje minimální nasazení cílem * (maximální počet je *). Vyberte cíl starší nasazení v projektu na Info.plist nebo upgrade na novější verzi Xamarin.iOS.

Xamarin.iOS nepodporuje nastavení na vyšší verzi než verze, kterou tato konkrétní verzi Xamarin.iOS byl vytvořen pro cíl minimální nasazení.

Vyberte cíl starší minimální nasazení v projektu Info.plist nebo upgrade na novější verzi Xamarin.iOS.

### <a name="a-namemt0075mt0075-invalid-architecture--for--projects-valid-architectures-are-"></a><a name="MT0075"/>MT0075: Architektura neplatný ' *' pro * projekty. Jsou platné architektury: *

Byl zadán neplatný architekturu. Ověřte, že architektura je platný.

### <a name="a-namemt0076mt0075-no-architecture-specified-using-the---abi-argument-an-architecture-is-required-for--projects"></a><a name="MT0076"/>MT0075: Žádné architektura zadán (pomocí argumentu – abi). Je vyžadována pro architekturu * projekty.

Obvykle to ukazuje na chybu v Xamarin.iOS; prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s testovacího případu.

### <a name="a-namemt0077mt0076-watchos-projects-must-be-extensions"></a><a name="MT0077"/>MT0076: WatchOS projektů musí být rozšíření.

Obvykle to ukazuje na chybu v Xamarin.iOS; prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s testovacího případu.

### <a name="a-namemt0078mt0077-incremental-builds-are-enabled-with-a-deployment-target--80-currently--this-is-not-supported-the-resulting-application-will-not-launch-on-ios-9-so-the-deployment-target-will-be-set-to-80"></a><a name="MT0078"/>MT0077: Přírůstkové sestavení jsou povoleny v rámci nasazení cíl < 8.0 (aktuálně *). To není podporováno (výsledné aplikace se nespustí v systému iOS 9), takže cíl nasazení bude nastavena pro 8.0.

Toto je upozornění oznamující, že cíl nasazení byla nastavena na 8.0 pro toto sestavení tak, aby přírůstková sestavení pracovní správně.

Přírůstková sestavení jsou podporovány pouze, pokud jsou cílem nasazení alespoň 8.0 (protože výsledné aplikace jinak nespustí v systému iOS 9).

### <a name="a-namemt0079mt0078-the-recommended-xcode-version-for-xamarinios--is-xcode--or-later-the-current-xcode-version-found-in--is-"></a><a name="MT0079"/>MT0078: Doporučené Xcode verze pro Xamarin.iOS * je Xcode * nebo novější. Aktuální verze Xcode (v nalezen *) je *.

Toto je upozornění oznamující, že aktuální verzi Xcode není doporučenou verzi Xcode pro tuto verzi Xamarin.iOS.

Upgradujte Xcode zajistit optimální chování.

### <a name="a-namemt0080mt0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a><a name="MT0080"/>MT0080: Zakázání NewRefCount, – nové-refcount:false, je zastaralý.

Toto je upozornění, která informuje o požadavek na zakázání nové refcount (– nové - refcount:false) byla ignorována.

Nová funkce refcount je teď povinné pro všechny projekty, a proto není možné zakázat už.

### <a name="a-namemt0081mt0081-the-command-line-argument---download-crash-report-also-requires---download-crash-report-to"></a><a name="MT0081"/>MT0081: Argument příkazového řádku – stažení havárií – sestava také vyžaduje – stažení havárií sestavy.
### <a name="a-namemt0082mt0082-repl---enable-repl-is-only-supported-when-linking-is-not-used---nolink"></a><a name="MT0082"/>MT0082: REPL (– Povolit repl) je podporována pouze při propojování nepoužívá (--nolink).
### <a name="a-namemt0083mt0083-asm-only-bitcode-is-not-supported-on-watchos-use-either---bitcodemarker-or---bitcodefull"></a><a name="MT0083"/>MT0083: Nepodporuje bitcode jen Asm watchOS. Použijte buď – bitcode: značka nebo--bitcode: úplná.
### <a name="a-namemt0084mt0084-bitcode-is-not-supported-in-the-simulator-do-not-pass---bitcode-when-building-for-the-simulator"></a><a name="MT0084"/>MT0084: V simulátoru nepodporuje Bitcode. Při sestavování simulátoru nepředávejte – bitcode.
### <a name="a-namemt0085mt0085-no-reference-to--was-found-it-will-be-added-automatically"></a><a name="MT0085"/>MT0085: Žádný odkaz na ' *' nebyl nalezen. Přidá se automaticky.
### <a name="a-namemt0086mt0086-a-target-framework---target-framework-must-be-specified-when-building-for-tvos-or-watchos"></a><a name="MT0086"/>MT0086: Cílové rozhraní (--cílové rozhraní) se musí zadat při vytváření pro TVOS nebo WatchOS.

Obvykle to ukazuje na chybu v Xamarin.iOS; prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s testovacího případu.

### <a name="a-namemt0087mt0087-incremental-builds---fastdev-is-not-supported-with-the-boehm-gc-incremental-builds-will-be-disabled"></a><a name="MT0087"/>MT0087: Přírůstková sestavení (--fastdev) není podporován s Boehm GC. Přírůstková sestavení bude zakázána.

### <a name="a-namemt0088mt0088-the-gc-must-be-in-cooperative-mode-for-watchos-apps-please-remove-the---coopfalse-argument-to-mtouch"></a><a name="MT0088"/>MT0088: V spolupráci režimu pro watchOS aplikace musí být globální Katalog. Odeberte argument mtouch--coop: false.

### <a name="a-namemt0089mt0089-the-option--cannot-take-the-value--when-cooperative-mode-is-enabled-for-the-gc"></a><a name="MT0089"/>MT0089: Možnost "\*'nemůže vrátit hodnotu'\*, když je povolený spolupráci režim pro globální Katalog.

### <a name="a-namemt0091mt0091-this-version-of-xamarinios-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-set-the-managed-linker-behaviour-to-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a><a name="MT0091"/>MT0091: Tato verze aplikace Xamarin.iOS vyžaduje * SDK (dodávané s Xcode *). Buď upgradujte Xcode na získání požadované hlavičky souborů nebo nastavením chování spravované linkeru na odkaz Framework SDK pouze (pokusit se vyhnout nových rozhraní API).

Xamarin.iOS vyžaduje soubory hlaviček, z SDK verze zadaná v chybové zprávě, chcete-li sestavit aplikaci. Doporučeným způsobem, jak vyřešit tuto chybu se k upgradu Xcode získat požadované SDK, to bude zahrnovat všechny soubory požadované hlavičky. Pokud máte více verzí Xcode nainstalován, nebo chcete použít Xcode v jiné než výchozí umístění, ujistěte se, zda je nastavení správné umístění Xcode v předvolbách vaší IDE.

Potenciální, alternativním řešením je povolit spravované linkeru. Tato akce odebere nepoužívané včetně rozhraní API, ve většině případů nové rozhraní API, kde jsou soubory hlaviček chybí (nebo jsou neúplné). To nebude fungovat, pokud váš projekt používá rozhraní API byla zavedena v novější SDK než jeden vaší Xcode ale nabízí.

Poslední podají řešení může být použití starší verze Xamarin.iOS, ten, který podporuje sadu SDK do projektu vyžaduje.

<!-- MT0092 used by mlaunch -->

### <a name="a-namemt0093mt0093-could-not-find-mlaunch"></a><a name="MT0093"/>MT0093: Nepodařilo se najít 'mlaunch'.

<!-- MT0094 is not reported anymore -->

### <a name="a-namemt0095mt0095-aot-files-could-not-be-copied-to-the-destination-directory-dest-error"></a><a name="MT0095"/>MT0095: Aot soubory nelze zkopírovat na cílový adresář {cíle}: {error}

### <a name="a-namemt0096mt0096-no-reference-to-xamariniosdll-was-found"></a><a name="MT0096"/>MT0096: Nebyl nalezen žádný odkaz na Xamarin.iOS.dll.

<!-- MT0097: used by mmp -->
<!-- MT0098: used by mmp -->

### <a name="a-namemt0099mt0099-internal-error--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a><a name="MT0099"/>MT0099: Vnitřní chyba *. Prosím soubor zprávu o chybě s testovacího případu (http://bugzilla.xamarin.com).

Tato chybová zpráva se hlásí, když se nezdaří Kontrola interní konzistence v Xamarin.iOS.

To ukazuje na chybu v Xamarin.iOS; prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s testovacího případu.

### <a name="a-namemt0100mt0100-invalid-assembly-build-target--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a><a name="MT0100"/>MT0100: Neplatné sestavení sestavení cíl: ' *'. Prosím soubor zprávu o chybě s testovacího případu (http://bugzilla.xamarin.com).

Tato chybová zpráva se hlásí, když se nezdaří Kontrola interní konzistence v Xamarin.iOS.

Toto je vždy chyby v Xamarin.iOS; prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s testovacího případu.

### <a name="a-namemt0101mt0101-the-assembly--is-specified-multiple-times-in---assembly-build-target-arguments"></a><a name="MT0101"/>MT0101: Sestavení ' *' v--argumenty cíl sestavení sestavení je zadán vícekrát.

Sestavení uvedený v chybové zprávě je zadán vícekrát v--argumenty cíl sestavení sestavení. Zkontrolujte prosím, že každé sestavení je uveden pouze jednou.

### <a name="a-namemt0102mt0102-the-assemblies--and--have-the-same-target-name--but-different-targets--and-"></a><a name="MT0102"/>MT0102: Sestavení se\*'a'\*' mají stejný název cílové ('\*'), ale odlišným cíle ('\*' a ' *').

Sestavení uvedený v chybové zprávě mít konfliktní sestavení cílů.

Příklad:

    --assembly-build-target:Assembly1.dll=framework=MyBinary --assembly-build-target:Assembly2.dll=dynamiclibrary=MyBinary

Při pokusu o vytvoření dynamické knihovny a rozhraní s stejné značky je v tomto příkladu (`MyBinary`).

### <a name="a-namemt0103mt0103-the-static-object--contains-more-than-one-assembly--but-each-static-object-must-correspond-with-exactly-one-assembly"></a><a name="MT0103"/>MT0103: Statický objekt '\*' obsahuje více než jednom sestavení ('\*'), ale každý statické objekt musí být shodná s přesně jeden sestavení.

Sestavení uvedený v chybové zprávě se zkompilovat pro jediný objekt statické. Tato operace není povolena, všechna sestavení musí být zkompilovány do jiného statické objektu.

Příklad:

    --assembly-build-target:Assembly1.dll=staticobject=MyBinary --assembly-build-target:Assembly2.dll=staticobject=MyBinary

V tomto příkladu se pokusí vytvořit objekt statické (`MyBinary`) skládá ze dvou sestavení (`Assembly1.dll` a `Assembly2.dll`), což není povolené.

### <a name="a-namemt0105mt0105-no-assembly-build-target-was-specified-for-"></a><a name="MT0105"/>MT0105: Nebyl zadán žádný cíl sestavení sestavení pro ' *'.

Při zadání sestavení sestavení pomocí cíl `--assembly-build-target`, všechna sestavení v aplikaci musí mít přiřazen cíl sestavení.

Tato chyba se nahlásí při sestavení uvedený v chybové zprávě nemá sestavení sestavení cíl přiřazen.

Najdete v dokumentaci `--assembly-build-target` Další informace.

### <a name="a-namemt0106mt0106-the-assembly-build-target-name--is-invalid-the-character--is-not-allowed"></a><a name="MT0106"/>MT0106: Sestavení sestavení cílová '\*' je neplatná: znak '\*' není povolen.

Název cílové sestavení sestavení musí být platný název souboru.

Tyto hodnoty se aktivuje například tuto chybu:

    --assembly-build-target:Assembly1.dll=staticobject=my/path.o

protože `my/path.o` není platný název souboru z důvodu directory oddělovací znak.

### <a name="a-namemt0107mt0107-the-assemblies--have-different-custom-llvm-optimizations--which-is-not-allowed-when-they-are-all-compiled-to-a-single-binary"></a><a name="MT0107"/>MT0107: Sestavení se\*se mají různé vlastní optimalizace LLVM (\*), což není povolené, pokud by byly všechny zkompilovány do jednoho binárního souboru.

### <a name="a-namemt0108mt0108-the-assembly-build-target--did-not-match-any-assemblies"></a><a name="MT0108"/>MT0108: Sestavení sestavení cíl ' *' neodpovídá žádné sestavení.

### <a name="a-namemt0109mt0109-the-assembly-0-was-loaded-from-a-different-path-than-the-provided-path-provided-path-1-actual-path-2"></a><a name="MT0109"/>MT0109: Sestavení: {0} byl načten z jiné cestě než zadaná cesta (Zadaná cesta: {1}, skutečné cesty: {2}).

Toto je upozornění znamenající, že byla sestavení odkazuje aplikaci načíst z jiného místa než požadovaný.

To může znamenat, že aplikace odkazuje na více sestavení se stejným názvem, ale z různých umístění, což může vést k neočekávaným výsledkům (pouze první sestavení se použije).

### <a name="a-namemt0110mt0110-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-third-party-binding-libraries-and-that-compiles-to-bitcode"></a><a name="MT0110"/>MT0110: Přírůstkové sestavení byly zakázány, protože tato verze Xamarin.iOS nepodporuje přírůstkové sestavení v projektech, zahrnují knihovny, vazba třetích stran a který se zkompiluje do bitcode.

Přírůstková sestavení byly zakázány, protože tato verze Xamarin.iOS nepodporuje přírůstkové sestavení v projektech, zahrnují knihovny, vazba třetích stran a který se zkompiluje do bitcode (tvOS a watchOS projekty).

Je zapotřebí žádná akce z vaší strany, tato zpráva je pouze informační.

Další informace najdete v části Chyba č[51710](https://bugzilla.xamarin.com/show_bug.cgi?id=51710).

Toto upozornění není hlášena už.

### <a name="a-namemt0111mt0111-bitcode-has-been-enabled-because-this-version-of-xamarinios-does-not-support-building-watchos-projects-using-llvm-without-enabling-bitcode"></a><a name="MT0111"/>MT0111: Bitcode má povoleno, protože tato verze Xamarin.iOS nepodporuje vytváření watchOS projekty pomocí LLVM bez povolení bitcode.

Bitcode má automaticky povoleno, protože tato verze Xamarin.iOS nepodporuje vytváření watchOS projekty pomocí LLVM bez povolení bitcode.

Je zapotřebí žádná akce z vaší strany, tato zpráva je pouze informační.

Další informace najdete v části Chyba č[51634](https://bugzilla.xamarin.com/show_bug.cgi?id=51634).

### <a name="a-namemt0112mt0112-native-code-sharing-has-been-disabled-because-"></a><a name="MT0112"/>MT0112: Sdílení nativní kód byl zakázán, protože *

Existuje několik důvodů, které je možné zakázat sdílení kódu:

* vzhledem k tomu cíl nasazení aplikace kontejneru je starší než iOS 8.0 (má *)).

Nativní kód sdílení vyžaduje iOS 8.0 protože sdílení nativního kódu je implementovaná pomocí uživatelského rozhraní, která byla zavedena v systému iOS 8.0.

* protože aplikace kontejneru zahrnuje I18N sestavení (*).

Nativní kód sdílení se aktuálně nepodporuje, pokud aplikace kontejneru zahrnuje I18N sestavení.

* protože aplikace kontejneru obsahuje definice vlastní kód xml pro spravované linkeru (*).

Nativní kód sdílení vyžaduje není podporována pro projekty, které používají vlastní kód xml definice pro spravované linkeru.

### <a name="a-namemt0113mt0113-native-code-sharing-has-been-disabled-for-the-extension--because-"></a><a name="MT0113"/>MT0113: Sdílení nativního kódu byla zakázána pro rozšíření ' *' protože *.

* protože bitcode lišit mezi aplikace kontejneru (\*) a rozšíření (\*).

  Nativní kód sdílení vyžaduje, aby možnosti bitcode shodovala mezi projekty, které sdílejí kódu.

* protože – sestavení –-cíl sestavení možnosti se liší mezi aplikace kontejneru (\*) a rozšíření (\*).

  Nativní kód sdílení vyžaduje, aby – sestavení –-cíl sestavení možnosti jsou identické projekty, které sdílejí kódu.

  K tomuto stavu může dojít, pokud přírůstková sestavení jsou buď není zapnutá nebo vypnutá ve všech projektech.

* protože I18N sestavení jsou rozdíly mezi aplikace kontejneru (\*) a rozšíření (\*).

  Nativní kód sdílení není aktuálně podporováno pro rozšíření, které zahrnují I18N sestavení.

* protože jsou argumenty pro kompilátor AOT liší aplikace kontejneru (\*) a rozšíření (\*).

  Nativní kód sdílení vyžaduje argumenty, které mají kompilátoru AOT se neliší mezi projekty, které sdílejí kódu.

* protože další argumenty pro kompilátor AOT se liší aplikace kontejneru (\*) a rozšíření (\*).

  Nativní kód sdílení vyžaduje argumenty, které mají kompilátoru AOT se neliší mezi projekty, které sdílejí kódu.

  K tomuto stavu dochází, pokud 'provádět všechny 32bitové float operace jako 64-bit float' mezi projekty liší.

* protože LLVM není zapnutá nebo vypnutá v kontejneru aplikace (\*) a rozšíření (\*).

  Nativní kód pro sdílení vyžaduje, aby LLVM je buď povoleno nebo zakázáno pro všechny projekty, které sdílejí kódu.

* vzhledem k tomu, že nastavení spravovaných linkeru se liší mezi aplikace kontejneru (\*) a rozšíření (\*).

  Nativní kód sdílení vyžaduje spravované linkeru nastavení, musí být stejné pro všechny projekty, které sdílejí kódu.

* protože přeskočené sestavení pro spravované linkeru se liší aplikace kontejneru (\*) a rozšíření (\*).

  Nativní kód sdílení vyžaduje spravované linkeru nastavení, musí být stejné pro všechny projekty, které sdílejí kódu.

* vzhledem k tomu, že rozšíření má definice vlastní kód xml pro spravované linkeru (*).

  Nativní kód sdílení vyžaduje není podporována pro projekty, které používají vlastní kód xml definice pro spravované linkeru.

* protože nelze sestavit aplikaci kontejneru pro ABI * (při rozšíření je pro tento ABI vytváření).

  Nativní kód pro sdílení vyžaduje, že pro všechny architektury rozšíření všechny aplikace sestavení pro sestavení aplikace kontejneru.

  Například: k tomuto stavu dochází, když sestavení rozšíření pro ARM64 + ARMv7, ale aplikace kontejner vytvoří pouze pro ARM64.

* vzhledem k tomu, že je sestavení aplikace kontejneru pro ABI \*, který není kompatibilní s rozšíření ABI (\*).

  Nativní kód pro sdílení vyžaduje, aby všechny projekty sestavení pro přesnou stejné rozhraní API.

  Například: k tomuto stavu dochází, když sestavení rozšíření pro ARMv7 + llvm + thumb2, ale aplikace kontejner vytvoří pouze pro ARMv7 + llvm.

* protože aplikace kontejneru odkazuje na sestavení '\*'z'\*', zatímco rozšíření odkazuje na jiné verzi z ' *'.

  Nativní kód sdílení vyžaduje, aby všechny projekty, které sdílejí kód používal shodovat s verzemi pro všechna sestavení.

<!-- MT0114: used by mmp -->

### <a name="a-namemt0115mt0115-it-is-recommended-to-reference-dynamic-symbols-using-code---dynamic-symbol-modecode-when-bitcode-is-enabled"></a><a name="MT0115"/>MT0115: Doporučujeme tak, aby odkazovaly dynamické symboly pomocí kódu (--dynamické symbol-mode = kód) Pokud je povolená bitcode.

Projekty Xamarin.iOS bude často odkazovat nativní symboly dynamicky, což znamená, že nativní linkeru může odebrat takové nativní symboly během nativní proces propojení, protože nativní linkeru nejsou vidět, že se používají tyto symboly.

Obvykle Xamarin.iOS požádá nativní linkeru zachovat tyto značky (pomocí `-u symbol` příznak linkeru), ale při kompilaci pro bitcode nativní linkeru nepřijímá `-u` příznak.

Xamarin.iOS implementovala alternativní řešení: jsme generovat velmi nativní kód, který odkazuje na těchto symbolů, a proto nativní linkeru uvidí, že se používají tyto symboly. Děje se automaticky při kompilaci pro bitcode.

Pokud `--dynamic-symbol-mode=linker` předaný mtouch, tento alternativní řešení bude zakázáno a Xamarin.iOS se pokusí předat `-u` nativní linkeru. Výsledkem bude s největší pravděpodobností nativní linkeru chyby.

Řešení je k odebrání `--dynamic-symbol-mode=linker` argument z argumentů další mtouch v možnostech sestavení projektu.

<!-- 0116 - 0124: free to use -->

### <a name="a-namemt0116mt0116-invalid-architecture-arch-32-bit-architectures-are-not-supported-when-deployment-target-is-11-or-later-make-sure-the-project-does-not-build-for-a-32-bit-architecture"></a><a name="MT0116"/>MT0116: Architektura neplatný: {architektura}. 32bitové architektury nejsou podporované, když cíl nasazení je 11 nebo novější. Zajistěte, aby že projekt nelze sestavit pro architekturu 32-bit.

iOS 11 neobsahuje podpora 32bitových aplikací, takže není možné vytvořit pro 32bitovou aplikaci, pokud jsou cílem nasazení iOS 11 nebo novější.

Buď změnit cílový architektura v možnostech sestavení projektu iOS arm64, ani změnit cíl nasazení v projektu Info.plist na starší verzi iOS.

### <a name="a-namemt0117mt0117-cant-launch-a-32-bit-app-on-a-simulator-that-only-supports-64-bit"></a><a name="MT0117"/>MT0117: Nelze spustit 32bitovou aplikaci v simulátoru, který podporuje pouze 64-bit.

### <a name="a-namemt0118mt0118-aot-files-could-not-be-found-at-the-expected-directory-msymdir"></a><a name="MT0118"/>MT0118: Soubory Aot nebyl nalezen v adresáři očekávané {msymdir}.

<!-- 0119 - 0123: free to use -->

### <a name="a-namemt0123mt0123-the-executable-assembly--does-not-reference-"></a><a name="MT0123"/>MT0123: Spustitelný soubor sestavení * neodkazuje *.

Nenašla se žádný odkaz na sestavení platformy (Xamarin.iOS.dll / Xamarin.TVOS.dll / Xamarin.WatchOS.dll) v spustitelný soubor sestavení.

To se obvykle stává tam, kde není žádný kód v spustitelný projekt, který používá nic z platformy sestavení; pro instanci prázdnou metodu Main (a žádné jiné kód) by zobrazit tuto chybu:

```csharp
class Program {
    void Main (string[] args)
    {
    }
}
```

Pomocí rozhraní API z platformy sestavení vyřeší chyby:

```csharp
class Program {
    void Main (string[] args)
    {
        System.Console.WriteLine (typeof (UIKit.UIWindow));
    }
}
```

### <a name="a-namemt0124mt0124-could-not-set-the-current-language-to-lang-according-to-langlang-exception"></a><a name="MT0124"/>MT0124: Nelze nastavit aktuální jazyk na {jazyk} (podle LANG = {{jazyk}): {výjimka}

Toto je upozornění, která určuje, že aktuální jazyk nebylo možné nastavit na jazyk v chybové zprávě.

Aktuální jazyk bude použita výchozí systémový jazyk.

### <a name="a-namemt0125mt0125-the---assembly-build-target-command-line-argument-is-ignored-in-the-simulator"></a><a name="MT0125"/>MT0125:--Sestavení build cíl argument příkazového řádku je ignorován v simulátoru.

Není vyžadována žádná akce, tato zpráva je pouze informační.

### <a name="a-namemt0126mt0126-incremental-builds-have-been-disabled-because-incremental-builds-are-not-supported-in-the-simulator"></a><a name="MT0126"/>MT0126: Přírůstkové sestavení byly zakázány, protože přírůstkové sestavení nejsou podporovány v simulátoru.

Není vyžadována žádná akce, tato zpráva je pouze informační.

### <a name="a-namemt0127mt0127-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-more-than-one-third-party-binding-libraries"></a><a name="MT0127"/>MT0127: Přírůstkové sestavení byly zakázány, protože tato verze Xamarin.iOS nepodporuje přírůstkové sestavení v projektech, které zahrnují knihovny, vazba více než jeden třetích stran.

Přírůstková sestavení byly automaticky zakázány, protože tato verze Xamarin.iOS není vždy sestavení projektů s více třetích stran vazba knihoven správně.

Není vyžadována žádná akce, tato zpráva je pouze informační.

Další informace najdete v části Chyba č[52727](https://bugzilla.xamarin.com/show_bug.cgi?id=52727).

### <a name="a-namemt0128mt0128-could-not-touch-the-file--"></a><a name="MT0128"/>MT0128: Nelze touch souboru ' *': *

Při klepnou na soubor (který slouží k zajištění, že jsou správně provedené částečné sestavení) došlo k selhání.

Toto upozornění můžete ignorovat s největší pravděpodobností; v případě potíže založení záznamu o chybě (https://bugzilla.xamarin.com] (https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)) a bude prozkoumat.

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: Projektu související chybové zprávy

### <a name="mt10xx-installer--mtouch"></a>MT10xx: Instalační program / mtouch

<!--
 MT1xxx file copy / symlinks (project related)
  MT10xx installer.cs / mtouch.cs
  -->

### <a name="a-namemt1001mt1001-could-not-find-an-application-at-the-specified-directory"></a><a name="MT1001"/>MT1001 nenalezl aplikace v určeném adresáři

### <a name="a-namemt1002mt1002-could-not-create-symlinks-files-were-copied"></a><a name="MT1002"/>MT1002 nelze vytvořit symbolických odkazů, soubory byly zkopírovány.

### <a name="a-namemt1003mt1003-could-not-kill-the-application--you-may-have-to-kill-the-application-manually"></a><a name="MT1003"/>Může MT1003 není ukončit aplikaci ' *'. Možná budete muset ručně ukončit aplikaci.

### <a name="a-namemt1004mt1004-could-not-get-the-list-of-installed-applications"></a><a name="MT1004"/>MT1004 může dojít, není v seznamu nainstalovaných aplikací.

### <a name="a-namemt1005mt1005-could-not-kill-the-application--on-the-device----you-may-have-to-kill-the-application-manually"></a><a name="MT1005"/>Může MT1005 není ukončení aplikace se\*'na zařízení,\*': *-možná budete muset ručně ukončit aplikaci.

### <a name="a-namemt1006mt1006-could-not-install-the-application--on-the-device--"></a><a name="MT1006"/>Není MT1006 může instalace aplikace "\*'na zařízení,\*': *.

### <a name="a-namemt1007mt1007-failed-to-launch-the-application--on-the-device---you-can-still-launch-the-application-manually-by-tapping-on-it"></a><a name="MT1007"/>MT1007 spuštění aplikace se nezdařilo '\*'na zařízení,\*': *. Aplikace můžete pořád spustit ručně klepnutím na něm.

### <a name="a-namemt1008mt1008-failed-to-launch-the-simulator"></a><a name="MT1008"/>MT1008: Nepodařilo se spustit simulátoru

Tato chyba se nahlásí, pokud mtouch simulátoru spuštění se nezdařilo.   Někdy tomu může docházet proto je již zastaralé nebo mrtvou simulátoru proces běží.

Následující příkaz na příkazovém řádku Unix slouží k ukončení zablokované simulátoru procesy:

```bash
$ launchctl list|grep UIKitApplication|awk '{print $3}'|xargs launchctl remove
```

### <a name="a-namemt1009mt1009-could-not-copy-the-assembly--to--"></a><a name="MT1009"/>Může MT1009 není zkopírujte sestavení '\*'do'\*': *

Jedná se o známý problém v určitých verzí aplikace Xamarin.iOS.

Pokud k tomu dojde vám, zkuste následující alternativní řešení:

```bash
sudo chmod 0644 /Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/*/*.mdb
```

Ale vzhledem k tomu, že tento problém byl vyřešen v nejnovější verzi Xamarin.iOS, oznamte nové chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s plnou verzí informace a výstup protokolu sestavení.

### <a name="a-namemt1010mt1010-could-not-load-the-assembly--"></a><a name="MT1010"/>Není MT1010 může načíst sestavení ' *': *

### <a name="a-namemt1011mt1011-could-not-add-missing-resource-file-"></a><a name="MT1011"/>MT1011 není přidat chybí soubor prostředků: ' *'

### <a name="a-namemt1012mt1012-failed-to-list-the-apps-on-the-device--"></a><a name="MT1012"/>Seznam aplikací na zařízení se nezdařila MT1012 ' *': *

### <a name="a-namemt1013mt1013-dependency-tracking-error-no-files-to-compare-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a><a name="MT1013"/>MT1013 závislost sledování Chyba: žádné soubory k porovnání. Sestava chyb prosím soubor na http://bugzilla.xamarin.com s testovacího případu.

To ukazuje na chybu v Xamarin.iOS. Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s caes testu.

### <a name="a-namemt1014mt1014-failed-to-re-use-cached-version-of--"></a><a name="MT1014"/>MT1014 se nepodařilo znovu použít uložené v mezipaměti verzi ' *': *.

### <a name="a-namemt1015mt1015--failed-to-create-the-executable--"></a><a name="MT1015"/>MT1015 se nepodařilo vytvořit spustitelný soubor ' *': *

### <a name="a-namemt1015mt1015--failed-to-create-the-executable--"></a><a name="MT1015"/>MT1015 se nepodařilo vytvořit spustitelný soubor ' *': *

### <a name="a-namemt1016mt1016-failed-to-create-the-notice-file-because-a-directory-already-exists-with-the-same-name"></a><a name="MT1016"/>MT1016: Nepodařilo se vytvořit soubor oznámení, protože adresář se stejným názvem již existuje.

Odebrat adresář `NOTICE` z projektu.

### <a name="a-namemt1017mt1017-failed-to-create-the-notice-file-"></a><a name="MT1017"/>MT1017: Nepodařilo se vytvořit soubor oznámení: *.

### <a name="a-namemt1018mt1018-your-application-failed-code-signing-checks-and-could-not-be-installed-on-the-device--check-your-certificates-provisioning-profiles-and-bundle-ids-probably-your-device-is-not-part-of-the-selected-provisioning-profile-error-0xe8008015"></a><a name="MT1018"/>MT1018: Aplikace se nezdařilo kontroly podepisování kódu a nebylo možné nainstalovat na zařízení ' *'. Zkontrolujte vaše certifikáty, profily, zřizování a ID sady. Pravděpodobně zařízení není součástí vybrané profilu pro zřizování (Chyba: 0xe8008015).
### <a name="a-namemt1019mt1019-your-application-has-entitlements-not-supported-by-your-current-provisioning-profile-and-could-not-be-installed-on-the-device--please-check-the-ios-device-log-for-more-detailed-information-error-0xe8008016"></a><a name="MT1019"/>MT1019: Vaše aplikace má oprávnění nepodporuje váš aktuální profil zřizování a nebylo možné nainstalovat na zařízení ' *'. Podrobnější informace, zkontrolujte protokol zařízení iOS (Chyba: 0xe8008016).

K tomu může dojít, pokud:

* Vaše aplikace nemá oprávnění, která nepodporuje aktuální profil zřizování.
  Možná řešení:
  - Zadejte jiný profil pro zřizování, který podporuje oprávnění potřebám vaší aplikace.
  - Odebrání oprávnění není podporována v aktuální profil zřizování.
* Zařízení, které se pokoušíte nasadit do není zahrnutý v profilu pro zřizování, kterou používáte.
  Možná řešení:
  - Vytvořit novou aplikaci ze šablony v Xcode, vyberte stejný profil zřizování a nasazovat do stejného zařízení. Xcode můžete někdy zřizovacích profilů automaticky aktualizovat nová zařízení (v ostatních případech Xcode se zeptá, co dělat).
  -Přejděte na Dev Center pro iOS a aktualizovat profil zřizování s novým zařízením potom stáhnout aktualizovaný profil zřizování k vašemu počítači.

Ve většině případů, které další informace o selhání budou vytištěny na iOS zařízení protokolu, který může pomoci diagnostikování problému.

### <a name="a-namemt1020mt1020-failed-to-launch-the-application--on-the-device--"></a><a name="MT1020"/>MT1020: Nepodařilo se spustit aplikaci se\*'na zařízení,\*': *

### <a name="a-namemt1021mt1021-could-not-copy-the-file--to--2"></a><a name="MT1021"/>MT1021: Nemohl zkopírovat soubor '\*'do'\*': {2}

Soubor nelze zkopírovat. Chybová zpráva z operace kopírování obsahuje další informace o této chybě.

### <a name="a-namemt1022mt1022-could-not-copy-the-directory--to--2"></a><a name="MT1022"/>MT1022: Nemohl zkopírovat do adresáře se\*'do'\*': {2}

Nelze zkopírovat do adresáře. Chybová zpráva z operace kopírování obsahuje další informace o této chybě.

### <a name="a-namemt1023mt1023-could-not-communicate-with-the-device-to-find-the-application---"></a><a name="MT1023"/>MT1023: Nešlo komunikovat s zařízení k vyhledání aplikace ' *': *

Došlo k chybě při pokusu o vyhledání aplikace na zařízení.

Postupů k vyzkoušení. Chcete-li tento problém vyřešit:

* Odstranit aplikaci ze zařízení a zkuste to znovu.
* Odpojte zařízení a znovu se připojit.
* Restartování zařízení.
* Restartujte počítač Mac.

### <a name="a-namemt1024mt1024-the-application-signature-could-not-be-verified-on-device--please-make-sure-that-the-provisioning-profile-is-installed-and-not-expired-error-0xe8008017"></a><a name="MT1024"/>MT1024: Nebylo možné ověřit podpis aplikace na zařízení ' *'. Ujistěte se, že je nainstalována a nevypršela profilu pro zřizování (Chyba: 0xe8008017).

Zařízení odmítl instalace aplikace, protože podpis nelze ověřit.

Zkontrolujte prosím, že je nainstalována a nevypršela profilu pro zřizování.

### <a name="a-namemt1025mt1025-could-not-list-the-crash-reports-on-the-device-"></a><a name="MT1025"/>MT1025: Nelze seznam sestav havárií na zařízení *.

Došlo k chybě při pokusu o seznam sestav havárií na zařízení.

Postupů k vyzkoušení. Chcete-li tento problém vyřešit:

* Odstranit aplikaci ze zařízení a zkuste to znovu.
* Odpojte zařízení a znovu se připojit.
* Restartování zařízení.
* Restartujte počítač Mac.
* Synchronizujte zařízení s iTunes (Tato akce odstraní všechny sestavy havárií ze zařízení).

### <a name="a-namemt1026mt1026-could-not-download-the-crash-report--from-the-device-"></a><a name="MT1026"/>MT1026: Nepovedlo se stáhnout sestavu havárií * ze zařízení *.

Došlo k chybě při pokusu o stažení sestavy havárií ze zařízení.

Postupů k vyzkoušení. Chcete-li tento problém vyřešit:

* Odstranit aplikaci ze zařízení a zkuste to znovu.
* Odpojte zařízení a znovu se připojit.
* Restartování zařízení.
* Restartujte počítač Mac.
* Synchronizujte zařízení s iTunes (Tato akce odstraní všechny sestavy havárií ze zařízení).

### <a name="a-namemt1027mt1027-cant-use-xcode-7-to-launch-applications-on-devices-with-ios--xcode-7-only-supports-ios-6"></a><a name="MT1027"/>MT1027: Nelze pro spuštění aplikace na zařízeních s iOS použijte Xcode 7 + * (Xcode 7 podporuje pouze systém iOS 6 +).

Není možné použít ke spuštění aplikací v zařízeních s iOS verzi nižší než 6.0 Xcode 7 +.

Použít starší verzi Xcode, nebo klepněte na aplikaci ručně spustíte.

### <a name="a-namemt1028mt1028-invalid-device-specification--expected-ios-watchos-or-all"></a><a name="MT1028"/>MT1028: Specifikace neplatný zařízení: "*". Očekávaný 'ios', 'watchos' nebo 'všechny

Specifikace zařízení předaným pomocí – zařízení je neplatná. Platné hodnoty jsou: 'ios', 'watchos' nebo 'všechny.

### <a name="a-namemt1029mt1029-could-not-find-an-application-at-the-specified-directory-"></a><a name="MT1029"/>MT1029: Nelze nalézt aplikaci na zadaný adresář: *

Cesta k aplikaci předán – launchdev neexistuje. Zadejte platnou aplikaci sady.

### <a name="a-namemt1030mt1030-launching-applications-on-device-using-a-bundle-identifier-is-deprecated-please-pass-the-full-path-to-the-bundle-to-launch"></a><a name="MT1030"/>MT1030: Spuštění aplikace na zařízení pomocí identifikátor svazku je zastaralý. Úplná cesta prosím předejte sada, která má spustit.

Se doporučuje předat cesta k aplikaci v zařízení, nikoli pouze id sady.

### <a name="a-namemt1031mt1031-could-not-launch-the-app--on-the-device--because-the-device-is-locked-please-unlock-the-device-and-try-again"></a><a name="MT1031"/>MT1031: Nelze spustit aplikaci se\*'na zařízení,\*' vzhledem k tomu, že je zařízení zamčené. Odemknutí zařízení a zkuste to znovu.

Odemknutí zařízení a zkuste to znovu.

### <a name="a-namemt1032mt1032-this-application-executable-might-be-too-large--mb-to-execute-on-device-if-bitcode-was-enabled-you-might-want-to-disable-it-for-development-it-is-only-required-to-submit-applications-to-apple"></a><a name="MT1032"/>MT1032: Tento spustitelný soubor aplikace může být příliš velký (* MB) ke spouštění na zařízení. Pokud bylo povolené bitcode můžete chtít zakázat pro vývoj, je potřeba jenom k odeslání aplikací do společnosti Apple.

### <a name="a-namemt1033mt1033-could-not-uninstall-the-application--from-the-device--"></a><a name="MT1033"/>MT1033: Nelze odinstalovat aplikaci,\*'ze zařízení,\*': *

<!-- 1034 used by mmp -->

### <a name="a-namemt1035mt1035-cannot-include-different-versions-of-the-framework-name"></a><a name="MT1035"/>MT1035: Nemůže obsahovat různé verze rozhraní {name}.

Není možné propojit s různými verzemi nástroje rozhraní stejné.

Obvykle se to stane, když rozšíření odkazuje na jinou verzi rozhraní než aplikace kontejneru (pravděpodobně prostřednictvím třetích stran vazby sestavení).

Následující tato chyba bude existovat více [MT1036](#MT1036) chyby zobrazení seznamu cest pro každý jiný framework.

### <a name="a-namemt1036mt1036-framework-name-included-from-path-related-to-previous-error"></a><a name="MT1036"/>MT1036: Framework {name}' obsahuje – pomocí: {cesta} (související k předchozí chyby)

Tato chyba se nahlásí pouze společně s [MT1036](#MT1036). Najdete v tématu [MT1036](#MT1036) Další informace.

### <a name="mt11xx-debug-service"></a>MT11xx: Debug Service

<!--
  MT11xx DebugService.cs
  -->

### <a name="a-namemt1101mt1101-could-not-start-app"></a><a name="MT1101"/>MT1101 nelze spustit aplikaci

### <a name="a-namemt1102mt1102-could-not-attach-to-the-app-to-kill-it-"></a><a name="MT1102"/>Není MT1102 může připojit k aplikaci (pro příkaz kill ji): *

### <a name="a-namemt1103mt1103-could-not-detach"></a><a name="MT1103"/>Může MT1103 není odpojení

### <a name="a-namemt1104mt1104-failed-to-send-packet-"></a><a name="MT1104"/>MT1104 Nepodařilo se odeslat paket: *

### <a name="a-namemt1105mt1105-unexpected-response-type"></a><a name="MT1105"/>MT1105 neočekávaný typ odpovědi

### <a name="a-namemt1106mt1106-could-not-get-list-of-applications-on-the-device-request-timed-out"></a><a name="MT1106"/>Může MT1106 není získat seznam aplikací na zařízení: Vypršel časový limit požadavku.

### <a name="a-namemt1107mt1107-application-failed-to-launch-"></a><a name="MT1107"/>MT1107: Aplikace se nepodařilo spustit: *

Zkontrolujte, jestli zařízení je uzamčen.

Pokud nasazujete aplikaci enterprise nebo pomocí volné profil pro zřizování, můžete mít vztah důvěryhodnosti vývojář (to se vysvětluje <a href="http://stackoverflow.com/a/30726375/183422">sem</a>).

### <a name="a-namemt1108mt1108-could-not-find-developer-tools-for-this-xx-yy-device"></a><a name="MT1108"/>MT1108: Nelze najít nástroje pro vývojáře pro toto zařízení XX (RR).

Vyžadovat několik operací z mtouch <tt>DeveloperDiskImage.dmg</tt> soubor nacházet.   Tento soubor je součástí Xcode a se obvykle nachází relativně k sadě SDK, kterou používáte k sestavení před v <tt>Xcode.app/Contents/Developer/iPhoneOS.platform/DeviceSupport/VERSION/DeveloperDiskImage.dmg</tt>.

Tato chyba buď může dojít, protože nemáte DeveloperDiskImage.dmg, který odpovídá zařízení, které jste se připojili.

### <a name="a-namemt1109mt1109-application-failed-to-launch-because-the-device-is-locked-please-unlock-the-device-and-try-again"></a><a name="MT1109"/>MT1109: Aplikace se nepodařilo spustit, protože je zařízení zamčené. Odemknutí zařízení a zkuste to znovu.

Zkontrolujte, jestli zařízení je uzamčen.

### <a name="a-namemt1110mt1110-application-failed-to-launch-because-of-ios-security-restrictions-please-ensure-the-developer-is-trusted"></a><a name="MT1110"/>MT1110: Aplikace se nepodařilo spustit z důvodu omezení zabezpečení systému iOS. Zkontrolujte, zda že je důvěryhodný vývojář.

Pokud nasazujete aplikaci enterprise nebo pomocí volné profil pro zřizování, můžete mít vztah důvěryhodnosti vývojář (to se vysvětluje <a href="http://stackoverflow.com/a/30726375/183422">sem</a>).

### <a name="a-namemt1111mt1111-application-launched-successfully-but-its-not-possible-to-wait-for-the-app-to-exit-as-requested-because-its-not-possible-to-detect-app-termination-when-launching-using-gdbserver"></a><a name="MT1111"/>MT1111: Aplikace spuštěna úspěšně, ale není možné počkat aplikaci ukončíte podle požadavku, protože není možné zjistit ukončení aplikace při spuštění pomocí gdbserver.

### <a name="mt12xx-simulator"></a>MT12xx: simulátoru

<!--
  MT12xx simcontroller.cs
  -->

### <a name="a-namemt1201mt1201-could-not-load-the-simulator-"></a><a name="MT1201"/>MT1201: Nepodařilo se načíst simulátoru: *
### <a name="a-namemt1202mt1202-invalid-simulator-configuration-"></a><a name="MT1202"/>MT1202: Konfigurace neplatný simulátoru: *
### <a name="a-namemt1203mt1203-invalid-simulator-specification-"></a><a name="MT1203"/>MT1203: Specifikace neplatný simulátoru: *
### <a name="a-namemt1204mt1204-invalid-simulator-specification--runtime-not-specified"></a><a name="MT1204"/>MT1204: Specifikace neplatný simulátoru ' *': runtime není zadaný.
### <a name="a-namemt1205mt1205-invalid-simulator-specification--device-type-not-specified"></a><a name="MT1205"/>MT1205: Specifikace neplatný simulátoru ' *': není zadán typ zařízení.
### <a name="a-namemt1206mt1206-could-not-find-the-simulator-runtime-"></a><a name="MT1206"/>MT1206: Nelze nalézt modul runtime simulátoru ' *'.
### <a name="a-namemt1207mt1207-could-not-find-the-simulator-device-type-"></a><a name="MT1207"/>MT1207: Nepodařilo se najít typ zařízení simulátoru ' *'.
### <a name="a-namemt1208mt1208-could-not-find-the-simulator-runtime-"></a><a name="MT1208"/>MT1208: Nelze nalézt modul runtime simulátoru ' *'.
### <a name="a-namemt1209mt1209-could-not-find-the-simulator-device-type-"></a><a name="MT1209"/>MT1209: Nepodařilo se najít typ zařízení simulátoru ' *'.
### <a name="a-namemt1210mt1210-invalid-simulator-specification--unknown-key-"></a><a name="MT1210"/>MT1210: Specifikace neplatný simulátoru: \*, neznámé klíč '\*.
### <a name="a-namemt1211mt1211-the-simulator-version--does-not-support-the-simulator-type-"></a><a name="MT1211"/>MT1211: Simulátoru verze '\*'nepodporuje typ simulátoru'\*.
### <a name="a-namemt1212mt1212-failed-to-create-a-simulator-version-where-type---and-runtime--"></a><a name="MT1212"/>MT1212: Nepodařilo se vytvořit verze simulátoru kde zadejte = * a prostředí runtime = *.
### <a name="a-namemt1213mt1213-invalid-simulator-specification-for-xcode-4-"></a><a name="MT1213"/>MT1213: Specifikace neplatný simulátoru Xcode 4: *
### <a name="a-namemt1214mt1214-invalid-simulator-specification-for-xcode-5-"></a><a name="MT1214"/>MT1214: Specifikace neplatný simulátor pro prostředí Xcode 5: *
### <a name="a-namemt1215mt1215-invalid-sdk-specified-"></a><a name="MT1215"/>MT1215: Zadán neplatný SDK: *
### <a name="a-namemt1216mt1216-could-not-find-the-simulator-udid-"></a><a name="MT1216"/>MT1216: Nelze nalézt simulátoru UDID ' *'.
### <a name="a-namemt1217mt1217-could-not-load-the-app-bundle-at-"></a><a name="MT1217"/>MT1217: Nepodařilo se načíst sady prostředků aplikace v ' *'.
### <a name="a-namemt1218mt1218-no-bundle-identifier-found-in-the-app-at-"></a><a name="MT1218"/>MT1218: Nalezen žádný identifikátor svazku v aplikaci na ' *'.
### <a name="a-namemt1219mt1219-could-not-find-the-simulator-for-"></a><a name="MT1219"/>MT1219: Nelze nalézt simulátor pro ' *'.
### <a name="a-namemt1220mt1220-could-not-find-the-latest-simulator-runtime-for-device-"></a><a name="MT1220"/>MT1220: Nenašla se nejnovější modul runtime simulátor pro zařízení ' *'.

To se obvykle označuje potíže s Xcode.

Co je potřeba tento problém vyřešit:

* Pomocí simulátoru jednou v Xcode.
* Předat explicitní sadu SDK verze pomocí sady sdk – <version>.
* Znovu nainstalujte Xcode.

### <a name="a-namemt1221mt1221-could-not-find-the-paired-iphone-simulator-for-the-watchos-simulator-"></a><a name="MT1221"/>MT1221: Nelze nalézt simulátoru spárované iPhone pro simulátoru WatchOS ' *'.

Při spouštění WatchOS aplikaci v simulátoru WatchOS, musí být spárované simulátoru iOS i.

Podívejte se na simulátorů může být spárována s iOS pomocí uživatelského rozhraní na Xcode zařízení simulátorů (nabídky okna -> zařízení).

### <a name="mt13xx-linkwith"></a>MT13xx: [LinkWith]

<!--
  MT13xx [LinkWith]
  -->

### <a name="a-namemt1301mt1301-native-library---was-ignored-since-it-does-not-match-the-current-build-architectures-"></a><a name="MT1301"/>MT1301 nativní knihovny `*` (\*) byl ignorován, protože neodpovídá aktuální architecture(s) sestavení (\*)

### <a name="a-namemt1302mt1302-could-not-extract-the-native-library--from--please-ensure-the-native-library-was-properly-embedded-in-the-managed-assembly-if-the-assembly-was-built-using-a-binding-project-the-native-library-must-be-included-in-the-project-and-its-build-action-must-be-objcbindingnativelibrary"></a><a name="MT1302"/>Může MT1302 není extrahovat nativní knihovny ' *' z '+'. Zkontrolujte, zda že nativní knihovny správně vložená ve spravované sestavení (Pokud sestavení bylo vytvořeno pomocí vazby projektu, nativní knihovny musí být zahrnutý v projektu a jeho akce sestavení musí být 'ObjcBindingNativeLibrary').

### <a name="a-namemt1303mt1303-could-not-decompress-the-native-framework--from--please-review-the-build-log-for-more-information-from-the-native-zip-command"></a><a name="MT1303"/>MT1303: Nelze dekomprimovat nativní framework '\*'z'\*'. Přečtěte si další informace z tohoto příkazu nativní 'zip' protokolu sestavení.

Zadaný nativní rozhraní z knihovny vazby nelze dekomprimovat.

Zkontrolujte protokol bulid Další informace o této chybě z příkazu nativní 'zip'.

### <a name="a-namemt1304mt1304-the-embedded-framework--in--is-invalid-it-does-not-contain-an-infoplist"></a><a name="MT1304"/>MT1304: Embedded framework ' *' v * je neplatný: neobsahuje Info.plist.

Rozhraní zadaný embedded neobsahuje Info.plist a není proto platný framework.

Zkontrolujte prosím, že rozhraní je platný.

### <a name="a-namemt1305mt1305-the-binding-library--contains-a-user-framework--but-embedded-user-frameworks-require-ios-80-the-current-deployment-target-is--please-set-the-deployment-target-in-the-infoplist-file-to-at-least-80"></a><a name="MT1305"/>MT1305: Vazba knihovny '\*' obsahuje uživatelské rozhraní (\*), ale vyžadují iOS 8.0 embedded uživatelské rozhraní (aktuální cíl nasazení je *). Nastavte prosím cíl nasazení v souboru Info.plist na alespoň 8.0.

Zadaná vazba knihovna obsahuje vložené framework, ale Xamarin.iOS podporuje pouze vložené architektury na iOS 8.0 nebo novější.

Nastavte cíl nasazení v souboru Info.plist na alespoň 8.0 při řešení této chyby (nebo nepoužívají embedded rozhraní).

### <a name="mt14xx-crash-reports"></a>MT14xx: Hlášení selhání

<!--
  MT14xx    CrashReports.cs
  -->

### <a name="a-namemt1400mt1400-could-not-open-crash-report-service-afcconnectionopen-returned-"></a><a name="MT1400"/>MT1400: Nelze otevřít služba sestav havárií: AFCConnectionOpen vrátil *

Došlo k chybě při pokusu o přístup k hlášení selhání ze zařízení.

Postupů k vyzkoušení. Chcete-li tento problém vyřešit:

* Odstranit aplikaci ze zařízení a zkuste to znovu.
* Odpojte zařízení a znovu se připojit.
* Restartování zařízení.
* Restartujte počítač Mac.
* Synchronizujte zařízení s iTunes (Tato akce odstraní všechny sestavy havárií ze zařízení).

### <a name="a-namemt1401mt1401-could-not-close-crash-report-service-afcconnectionclose-returned-"></a><a name="MT1401"/>MT1401: Nepodařilo se zavřít služba sestav havárií: AFCConnectionClose vrátil *

Došlo k chybě při pokusu o přístup k hlášení selhání ze zařízení.

Postupů k vyzkoušení. Chcete-li tento problém vyřešit:

* Odstranit aplikaci ze zařízení a zkuste to znovu.
* Odpojte zařízení a znovu se připojit.
* Restartování zařízení.
* Restartujte počítač Mac.
* Synchronizujte zařízení s iTunes (Tato akce odstraní všechny sestavy havárií ze zařízení).

### <a name="a-namemt1402mt1402-could-not-read-file-info-for--afcfileinfoopen-returned-"></a><a name="MT1402"/>MT1402: Nelze číst informace o souboru pro *: AFCFileInfoOpen vrátil *

Došlo k chybě při pokusu o přístup k hlášení selhání ze zařízení.

Postupů k vyzkoušení. Chcete-li tento problém vyřešit:

* Odstranit aplikaci ze zařízení a zkuste to znovu.
* Odpojte zařízení a znovu se připojit.
* Restartování zařízení.
* Restartujte počítač Mac.
* Synchronizujte zařízení s iTunes (Tato akce odstraní všechny sestavy havárií ze zařízení).

### <a name="a-namemt1403mt1403-could-not-read-crash-report-afcdirectoryopen--returned-"></a><a name="MT1403"/>MT1403: Nelze číst hlášení o selhání: AFCDirectoryOpen (*) vrátí: *

Došlo k chybě při pokusu o přístup k hlášení selhání ze zařízení.

Postupů k vyzkoušení. Chcete-li tento problém vyřešit:

* Odstranit aplikaci ze zařízení a zkuste to znovu.
* Odpojte zařízení a znovu se připojit.
* Restartování zařízení.
* Restartujte počítač Mac.
* Synchronizujte zařízení s iTunes (Tato akce odstraní všechny sestavy havárií ze zařízení).

### <a name="a-namemt1404mt1404-could-not-read-crash-report-afcfilerefopen--returned-"></a><a name="MT1404"/>MT1404: Nelze číst hlášení o selhání: AFCFileRefOpen (*) vrátí: *

Došlo k chybě při pokusu o přístup k hlášení selhání ze zařízení.

Postupů k vyzkoušení. Chcete-li tento problém vyřešit:

* Odstranit aplikaci ze zařízení a zkuste to znovu.
* Odpojte zařízení a znovu se připojit.
* Restartování zařízení.
* Restartujte počítač Mac.
* Synchronizujte zařízení s iTunes (Tato akce odstraní všechny sestavy havárií ze zařízení).

### <a name="a-namemt1405mt1405-could-not-read-crash-report-afcfilerefread--returned-"></a><a name="MT1405"/>MT1405: Nelze číst hlášení o selhání: AFCFileRefRead (*) vrátí: *

Došlo k chybě při pokusu o přístup k hlášení selhání ze zařízení.

Postupů k vyzkoušení. Chcete-li tento problém vyřešit:

* Odstranit aplikaci ze zařízení a zkuste to znovu.
* Odpojte zařízení a znovu se připojit.
* Restartování zařízení.
* Restartujte počítač Mac.
* Synchronizujte zařízení s iTunes (Tato akce odstraní všechny sestavy havárií ze zařízení).

### <a name="a-namemt1406mt1406-could-not-list-crash-reports-afcdirectoryopen--returned-"></a><a name="MT1406"/>MT1406: Nelze zobrazit seznam sestav havárií: AFCDirectoryOpen (*) vrátí: *

Došlo k chybě při pokusu o přístup k hlášení selhání ze zařízení.

Postupů k vyzkoušení. Chcete-li tento problém vyřešit:

* Odstranit aplikaci ze zařízení a zkuste to znovu.
* Odpojte zařízení a znovu se připojit.
* Restartování zařízení.
* Restartujte počítač Mac.
* Synchronizujte zařízení s iTunes (Tato akce odstraní všechny sestavy havárií ze zařízení).

<!--- 1407 used by mmp -->

### <a name="mt16xx-macho"></a>MT16xx: MachO

<!--
  MT16xx    MachO.cs
  -->

### <a name="a-namemt1600mt1600-not-a-mach-o-dynamic-library-unknown-header-0x-"></a><a name="MT1600"/>MT1600: Není prac-O dynamické knihovny (Neznámý záhlaví "0 x *"): *.

Došlo k chybě při zpracování dynamickou knihovnu nejistá.

Zkontrolujte prosím, že je platný dynamické knihovny prac-O dynamickou knihovnu.

Formát knihovnu lze ověřit pomocí `file` příkazu z terminálu:

    file -arch all -l /path/to/library.dylib

### <a name="a-namemt1601mt1601-not-a-static-library-unknown-header--"></a><a name="MT1601"/>MT1601: Nejsou statické knihovny (Neznámý záhlaví ' *'): *.

Došlo k chybě při zpracování se statickou knihovnou nejistá.

Zkontrolujte prosím, že se statickou knihovnou je platný prac-O statickou knihovnu.

Formát knihovnu lze ověřit pomocí `file` příkazu z terminálu:

    file -arch all -l /path/to/library.a

### <a name="a-namemt1602mt1602-not-a-mach-o-dynamic-library-unknown-header-0x-"></a><a name="MT1602"/>MT1602: Není prac-O dynamické knihovny (Neznámý záhlaví "0 x *"): *.

Došlo k chybě při zpracování dynamickou knihovnu nejistá.

Zkontrolujte prosím, že je platný dynamické knihovny prac-O dynamickou knihovnu.

Formát knihovnu lze ověřit pomocí `file` příkazu z terminálu:

    file -arch all -l /path/to/library.dylib

### <a name="a-namemt1603mt1603-unknown-format-for-fat-entry-at-position--in-"></a><a name="MT1603"/>MT1603: Neznámý formát pro fat položku na pozici * v *.

Došlo k chybě při zpracování dotyčném fat archivu.

Zkontrolujte prosím, že fat archivu je neplatný.

Formát fat archivu lze ověřit pomocí `file` příkazu z terminálu:

    file -arch all -l /path/to/file

### <a name="a-namemt1604mt1604-file-of-type--is-not-a-macho-file-"></a><a name="MT1604"/>MT1604: Soubor typu * není souborem MachO (*).

Došlo k chybě při zpracování souboru MachO nejistá.

Ujistěte se, zda je soubor platný prac-O dynamické knihovny.

Formát souboru můžete ověřit pomocí `file` příkazu z terminálu:

    file -arch all -l /path/to/file

## <a name="mt2xxx-linker-error-messages"></a>MT2xxx: Linkeru chybové zprávy

<!--
 MT2xxx Linker
  MT20xx Linker (general) errors
  -->

### <a name="a-namemt2001mt2001-could-not-link-assemblies"></a><a name="MT2001"/>Může MT2001 není odkaz sestavení

Tato chyba znamená, že spravované linkeru došlo k neočekávané chybě, například výjimku a nebylo možné dokončit nebo uložit sestavení zpracovává. Další informace o přesné informace o chybě bude například součástí protokolu sestavení

```
error MT2001: Could not link assemblies.
    Method: `System.Void Todo.TodoListPageCS/<<-ctor>b__1_0>d::SetStateMachine(System.Runtime.CompilerServices.IAsyncStateMachine)`
    Assembly: `QuickTodo, Version=1.0.6297.28241, Culture=neutral, PublicKeyToken=null`
Reason: Value cannot be null.
Parameter name: instruction
```

Je důležité k hlášení o chybě pro tyto problémy. Ve většině případů lze zadat alternativní řešení až po publikování správné opravu. Výše uvedené informace jsou kritické (spolu s testovacího případu nebo binairy sestavení) k vyřešení problému.

### <a name="a-namemt2002mt2002-can-not-resolve-reference-"></a><a name="MT2002"/>Můžete MT2002 nelze přeložit odkaz: *

### <a name="a-namemt2003mt2003-option--will-be-ignored-since-linking-is-disabled"></a><a name="MT2003"/>Možnost MT2003 ' *' bude ignorován, protože propojení je zakázán.

### <a name="a-namemt2004mt2004-extra-linker-definitions-file--could-not-be-located"></a><a name="MT2004"/>Soubor definice MT2004 navíc linkeru ' *' nebyl nalezen.

### <a name="a-namemt2005mt2005-definitions-from--could-not-be-parsed"></a><a name="MT2005"/>Definice MT2005 z ' *' nebylo možné analyzovat.

### <a name="a-namemt2006mt2006-can-not-load-mscorlibdll-from--please-reinstall-xamarinios"></a><a name="MT2006"/>MT2006: Nelze načíst mscorlib.dll z: *. Přeinstalujte Xamarin.iOS.

To obvykle znamená, že dojde k problému s instalací aplikace Xamarin.iOS. Zkuste to prosím znovu nainstalovat Xamarin.iOS.

<!--- 2007 used by mmp -->
<!--- 2009 used by mmp -->

### <a name="a-namemt2010mt2010-unknown-httpmessagehandler--valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a><a name="MT2010"/>MT2010: Neznámý objekt HttpMessageHandler `*`. Platné hodnoty jsou HttpClientHandler (výchozí), CFNetworkHandler nebo NSUrlSessionHandler

### <a name="a-namemt2011mt2011-unknown-tlsprovider---valid-values-are-default-or-appletls"></a><a name="MT2011"/>MT2011: Neznámý TlsProvider `*`.  Platné hodnoty jsou výchozí nebo appletls.

Hodnota zadané `tls-provider=` není platným zprostředkovatelem TLS (Transport Layer Security).

`default` a `appletls` jsou jedinými platnými hodnotami a obě představují stejnou možnost, která je poskytnout podporu protokolu SSL/TLS pomocí nativních rozhraní API TLS Apple.

<!--- 2012 used by mmp -->
<!--- 2013 used by mmp -->
<!--- 2014 used by mmp -->

### <a name="a-namemt2015mt2015-invalid-httpmessagehandler--for-watchos-the-only-valid-value-is-nsurlsessionhandler"></a><a name="MT2015"/>MT2015: Neplatný objekt HttpMessageHandler `*` pro watchOS. Jediná platná hodnota je NSUrlSessionHandler.

Toto je upozornění, ke kterému dochází neplatný HttpMessageHandler Určuje soubor projektu.

Starší verze nástrojů pro naše preview vygeneroval ve výchozím nastavení neplatnou hodnotu v souboru projektu.

Pokud chcete odstranit toto upozornění, v textovém editoru otevřete soubor projektu a odebrat všechny uzly HttpMessageHandler ze souboru XML.

### <a name="a-namemt2016mt2016-invalid-tlsprovider-legacy-option-the-only-valid-value-appletls-will-be-used"></a><a name="MT2016"/>MT2016: Neplatný TlsProvider `legacy` možnost. Jedinou platnou hodnotou `appletls` se použije.

`legacy` Poskytovatele, který je plně spravovaná SSLv3 / TLSv1 jediného poskytovatele, není součástí Xamarin.iOS už. Projekty, které byly pomocí tohoto staré zprostředkovatele a nyní vytvořit s novější `appletls` jeden.

Pokud chcete odstranit toto upozornění, v textovém editoru otevřete soubor projektu a odeberte všechny ' MtouchTlsProvider s uzly z XML.

### <a name="a-namemt2017mt2017-could-not-process-xml-description"></a><a name="MT2017"/>MT2017: Nebylo možné zpracovat popis XML.

To znamená, že dojde k chybě na [vlastní konfigurační soubor XML linkeru](https://developer.xamarin.com/guides/cross-platform/advanced/custom_linking/) jste zadali, Zkontrolujte prosím váš soubor.

### <a name="a-namemt2018mt2018-the-assembly--is-referenced-from-two-different-locations--and-"></a><a name="MT2018"/>MT2018: Sestavení '\*se odkazuje z různých míst: '\*' a ' *'.

Z několika míst je načteno sestavení uvedený v chybové zprávě. Zajistěte, aby vždy nutné použít stejnou verzi sestavení.

### <a name="a-namemt2019mt2019-can-not-load-the-root-assembly-"></a><a name="MT2019"/>MT2019: Nelze načíst sestavení kořenové ' *'

Sestavení kořenové nelze načíst. Ověřte, že složka v chybové zprávě odkazuje na existující soubor, a že je platné hodnoty .NET assembly.

### <a name="a-namemt202xmt202x-binding-optimizer-failed-processing-"></a><a name="MT202x"/>MT202x: Zpracování se nezdařilo vazby Optimalizátor `...`.

Něco neočekávaného došlo k chybě při optimalizaci generované vazby kódu. Element příčinou problému je pojmenována v chybové zprávě. Chcete tento problém vyřešit, sestavení s názvem (nebo obsahující typ nebo metoda s názvem) se musí být zadané v [Sestava chyb](http://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **další mtouch argumenty**).

Poslední číslice `x` bude:
* `0` pro název sestavení;
* `1` pro název typu;
* `3` pro název metody;

### <a name="a-namemt2030mt2030-remove-user-resources-failed-processing-"></a><a name="MT2030"/>MT2030: Prostředky odebrat uživatele se nezdařilo zpracování `...`.

Něco neočekávaného došlo k chybě při pokusu odebrat prostředky uživatele. Sestavení, která způsobila problém jmenuje v chybové zprávě. Chcete tento problém vyřešit sestavení se musí být zadané v [Sestava chyb](http://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **mtouch další argumenty**).

Prostředky uživatele, jsou soubory, které jsou zahrnuty do vnitřní sestavení (jako prostředky), které potřebuje k extrakci, v okamžiku sestavení, chcete-li vytvořit sadu aplikací. Sem patří:

* `__monotouch_content_*` a `__monotouch_pages_*` prostředků; a
* Nativní knihovny vložená do vazby sestavení;

### <a name="a-namemt2040mt2040-default-httpmessagehandler-setter-failed-processing-"></a><a name="MT2040"/>MT2040: HttpMessageHandler výchozí nastavení se nezdařilo zpracování `...`.

Něco neočekávaného došlo k chybě při pokusu o nastavení výchozí `HttpMessageHandler` pro aplikaci. Prosím soubor [Sestava chyb](http://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **mtouch další argumenty**).

### <a name="a-namemt2050mt2050-code-remover-failed-processing-"></a><a name="MT2050"/>MT2050: Nástroje pro odebrání kódu se nezdařilo zpracování `...`.

Něco neočekávaného došlo k chybě při pokusu o odebrání kódu BCL přesouvání s aplikací. Prosím soubor [Sestava chyb](http://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **mtouch další argumenty**).

### <a name="a-namemt2060mt2060-sealer-failed-processing-"></a><a name="MT2060"/>MT2060: Zpracování se nezdařilo Sealer `...`.

Něco neočekávaného došlo k chybě při pokusu o zapečetění typy nebo metody (poslední), nebo když devirtualizing některé metody. Sestavení, která způsobila problém jmenuje v chybové zprávě. Chcete tento problém vyřešit sestavení se musí být zadané v [Sestava chyb](http://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **mtouch další argumenty**).

### <a name="a-namemt2070mt2070-metadata-reducer-failed-processing-"></a><a name="MT2070"/>MT2070: Reduktorem metadat se nezdařilo zpracování `...`.

Něco neočekávaného došlo k chybě při pokusu o snížit metadata z aplikace. Sestavení, která způsobila problém jmenuje v chybové zprávě. Chcete tento problém vyřešit sestavení se musí být zadané v [Sestava chyb](http://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **mtouch další argumenty**).

### <a name="a-namemt2080mt2080-marknsobjects-failed-processing-"></a><a name="MT2080"/>MT2080: Zpracování se nezdařilo MarkNSObjects `...`.

Něco neočekávaného došlo k chybě při pokusu označit `NSObject` podtřídy z aplikace. Sestavení, která způsobila problém jmenuje v chybové zprávě. Chcete tento problém vyřešit sestavení se musí být zadané v [Sestava chyb](http://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **mtouch další argumenty**).

### <a name="a-namemt2090mt2090-inliner-failed-processing-"></a><a name="MT2090"/>MT2090: Zpracování se nezdařilo Inliner `...`.

Něco neočekávaného došlo k chybě při pokusu o vloženého kódu z aplikace. Sestavení, která způsobila problém jmenuje v chybové zprávě. Chcete-li tento problém vyřešit sestavení se musí být zadané v [Sestava chyb](https://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **mtouch další argumenty**).

<!-- MT21xx: more linker errors -->

<!--- 2100 used by mmp -->

### <a name="a-namemt2100mt2100-smart-enum-conversion-preserver-failed-processing-"></a><a name="MT2100"/>MT2100: Zpracování se nezdařilo inteligentní zachovat převod výčtu `...`.

Něco neočekávaného došlo k chybě při pokusu označit metody převod pro inteligentní výčty z aplikace. Sestavení, která způsobila problém jmenuje v chybové zprávě. Chcete-li tento problém vyřešit sestavení se musí být zadané v [Sestava chyb](https://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **mtouch další argumenty**).

### <a name="a-namemt2101mt2101-cant-resolve-the-reference--referenced-from-the-method--in-"></a><a name="MT2101"/>MT2101: Nelze přeložit odkaz '\*', odkazované z metody'\*' v ' *'.

Odkaz na neplatný sestavení došlo při zpracování metody uvedené v chybové zprávě.

Sestavení, která způsobila problém jmenuje v chybové zprávě. Chcete tento problém vyřešit sestavení se musí být zadané v [Sestava chyb](https://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **mtouch další argumenty**).

### <a name="a-namemt2102mt2102-error-processing-the-method--in-the-assembly--"></a><a name="MT2102"/>MT2102: Chyba při zpracování metodu '\*"v sestavení"\*': *

Něco neočekávaného došlo k chybě při pokusu označit metody uvedené v chybové zprávě.

Sestavení, která způsobila problém jmenuje v chybové zprávě. Chcete tento problém vyřešit sestavení se musí být zadané v [Sestava chyb](https://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **mtouch další argumenty**).

### <a name="a-namemt2103mt2103-error-processing-assembly--"></a><a name="MT2103"/>MT2103: Chyba zpracování sestavení '\*': *

Došlo k neočekávané chybě při zpracování sestavení.

Sestavení, která způsobila problém jmenuje v chybové zprávě. Chcete-li tento problém vyřešit sestavení se musí být zadané v [Sestava chyb](https://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **mtouch další argumenty**).

### <a name="a-namemt2104mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a><a name="MT2104"/>MM2104: Nelze vytvořit odkaz sestavení: {0}, protože je ve smíšeném režimu.

Ve smíšeném režimu sestavení nelze zpracovat linkeru.

Https://msdn.microsoft.com/en-us/library/x0w2664k.aspx Další informace najdete ve smíšeném režimu sestavení.

## <a name="mt3xxx-aot-error-messages"></a>MT3xxx: AOT chybové zprávy

<!--
 MT3xxx AOT
  MT30xx AOT (general) errors
  -->

### <a name="a-namemt3001mt3001-could-not-aot-the-assembly-"></a><a name="MT3001"/>Může MT3001 AOT sestavení ' *'

Obvykle to ukazuje na chybu v kompilátoru AOT. Oznamte chybu [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) s projekt, který slouží k tomu chybu reprodukovat.

V některých případech je možné tento problém obejít tím, že zakážete přírůstkové sestavení v projektu iOS možnost sestavení (ale je stále chyby, proto prosím oznamte přesto).

### <a name="a-namemt3002mt3002-aot-restriction-method--must-be-static-since-it-is-decorated-with-monopinvokecallback-see-httpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbackshttpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbacks"></a><a name="MT3002"/>Omezení MT3002 AOT: Metoda ' *' musí být statické, protože je upravena pomocí [MonoPInvokeCallback]. See [https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks](https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks)

Tato chybová zpráva pochází z kompilátoru AOT.

### <a name="a-namemt3003mt3003-conflicting---debug-and---llvm-options-soft-debugging-is-disabled"></a><a name="MT3003"/>MT3003 Konfliktní – ladění a--llvm možnosti. Ladění soft je zakázána.

Pokud je povoleno LLVM není podporováno ladění. Pokud potřebujete k ladění aplikace, zakažte nejdřív LLVM.

### <a name="a-namemt3004mt3004-could-not-aot-the-assembly--because-it-doesnt-exist"></a><a name="MT3004"/>Může MT3004 AOT sestavení ' *, protože neexistuje.

### <a name="a-namemt3005mt3005-the-dependency--of-the-assembly--was-not-found-please-review-the-projects-references"></a><a name="MT3005"/>MT3005 Závislost '\*'z sestavení'\*' nebyla nalezena. Zkontrolujte odkazy projekt.

K tomu obvykle dochází, když sestavení odkazuje na jinou verzi aplikace platformy sestavení (obvykle verze .NET 4 mscorlib.dll).

To není podporováno a nemusí sestavení nebo spusťte správně (sestavení může použít rozhraní API z verze .NET 4 mscorlib.dll, která verze Xamarin.iOS nemá).

### <a name="a-namemt3006mt3006-could-not-compute-a-complete-dependency-map-for-the-project-this-will-result-in-slower-build-times-because-xamarinios-cant-properly-detect-what-needs-to-be-rebuilt-and-what-does-not-need-to-be-rebuilt-please-review-previous-warnings-for-more-details"></a><a name="MT3006"/>Může MT3006 není výpočetní mapu dokončení závislostí pro projekt. Protože Xamarin.iOS nelze správně zjistit, co je třeba znovu sestavit (a co není nutné znovu sestavit), tato akce způsobí pomalejší časy sestavení. Přečtěte si předchozí varování další podrobnosti.

 sestavení nebo spustit správně (sestavení může použít rozhraní API z verze .NET 4 mscorlib.dll, která verze Xamarin.iOS nemá).

### <a name="a-namemt3007mt3007-debug-info-files-mdb-will-not-be-loaded-when-llvm-is-enabled"></a><a name="MT3007"/>MT3007: Ladění údaje, které soubory (*.mdb), nebudou načteny, pokud je povoleno llvm.

### <a name="a-namemt3008mt3008-bitcode-support-requires-the-use-of-the-llvm-aot-backend---llvm"></a><a name="MT3008"/>MT3008: Podpoře Bitcode vyžaduje použití LLVM AOT back-end (--llvm)

Podpoře Bitcode vyžaduje použití LLVM AOT back-end (--llvm).

Buď zakažte podpoře Bitcode nebo povolit LLVM.

<!--- 3009 used by mmp -->
<!--- 3010 used by mmp -->

## <a name="mt4xxx-code-generation-error-messages"></a>MT4xxx: Kód generování chybové zprávy

### <a name="mt40xx-main"></a>MT40xx: Main

<!--
 MT4xxx code generation
  MT40xx main.m
  -->

### <a name="a-namemt4001mt4001-the-main-template-could-not-be-expanded-to-"></a><a name="MT4001"/>MT4001 Hlavní šablonu nelze rozšířit tak, aby `*`.

Došlo k chybě při generování main.m. Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4002mt4002-failed-to-compile-the-generated-code-for-pinvoke-methods-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4002"/>Kompilace generovaného kódu pro volání metody MT4002 se nezdařila. Prosím http://bugzilla.xamarin.com souborů sestav chyb

Kompilace generovaného kódu pro volání metody se nezdařila. Prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="mt41xx-registrar"></a>MT41xx: Registrar

<!--
  MT41xx registrar.m
  -->

### <a name="a-namemt4101mt4101-the-registrar-cannot-build-a-signature-for-type-"></a><a name="MT4101"/>MT4101 Registrátora nemůže vytvořit podpis pro typ `*`.

Typ byl nalezen v exportovaných rozhraní API, které modul runtime není známo, jak přeuspořádat z Objective-c

Pokud se domníváte, Xamarin.iOS by měly podporovat typ Nejistá, prosím soubor požadavek vylepšení v [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4102mt4102-the-registrar-found-an-invalid-type--in-signature-for-method--use--instead"></a><a name="MT4102"/>MT4102 Registrátora nalezen neplatný typ `*` v podpis pro metodu `*`. Místo nich se používá `*`.

Tato aktuálně situace nastane pouze s jedním typem: System.DateTime. Místo toho použijte ekvivalentní jazyka Objective-C (NSDate).

### <a name="a-namemt4103mt4103-the-registrar-found-an-invalid-type--in-signature-for-method--the-type-implements-inativeobject-but-does-not-have-a-constructor-that-takes-two-intptr-bool-arguments"></a><a name="MT4103"/>MT4103 Registrátora nalezen neplatný typ `*` v podpis pro metodu `*`: typ implementuje INativeObject, ale nemá konstruktor, který přebírá dva (IntPtr, bool) argumenty

K tomu dojde při registrátora dojde k typu v podpis s uvedených charakteristikami. Registrátora může být nutné k vytvoření nové instance typu a v takovém případě vyžaduje konstruktor s (IntPtr bool,) podpis – první argument (IntPtr) určuje spravované popisovač při sekundu, pokud má volající předá přes vlastnictví nativního zpracování (Pokud je tato hodnota je false 'zachovat, bude volána pro objekt).

### <a name="a-namemt4104mt4104-the-registrar-cannot-marshal-the-return-value-for-type--in-signature-for-method-"></a><a name="MT4104"/>MT4104 Registrátora nelze zařazování návratovou hodnotu pro typ `*` v podpis pro metodu `*`.

Typ byl nalezen v exportovaných rozhraní API, které modul runtime není známo, jak přeuspořádat z Objective-c

Pokud se domníváte, Xamarin.iOS by měly podporovat typ Nejistá, prosím soubor požadavek vylepšení v [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4105mt4105-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a><a name="MT4105"/>MT4105 Registrátora nelze zařazování s parametrem typu `*` v podpis pro metodu `*`.

Pokud se domníváte, Xamarin.iOS by měly podporovat typ Nejistá, prosím soubor požadavek vylepšení v [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4106mt4106-the-registrar-cannot-marshal-the-return-value-for-structure--in-signature-for-method-"></a><a name="MT4106"/>MT4106 Registrátora nelze zařazování návratovou hodnotu pro strukturu `*` v podpis pro metodu `*`.

Typ byl nalezen v exportovaných rozhraní API, které modul runtime není známo, jak přeuspořádat z Objective-c

Pokud se domníváte, Xamarin.iOS by měly podporovat typ Nejistá, prosím soubor požadavek vylepšení v [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4107mt4107-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a><a name="MT4107"/>MT4107 Registrátora nelze zařazování s parametrem typu `*` v podpis pro metodu `+`.

Typ byl nalezen v exportovaných rozhraní API, které modul runtime není známo, jak přeuspořádat z Objective-c

Pokud se domníváte, Xamarin.iOS by měly podporovat typ Nejistá, prosím soubor požadavek vylepšení v [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4108mt4108-the-registrar-cannot-get-the-objectivec-type-for-managed-type-"></a><a name="MT4108"/>MT4108 Registrátora nelze získat typ ObjectiveC pro spravovaný typ `*`.

Typ byl nalezen v exportovaných rozhraní API, které modul runtime není známo, jak přeuspořádat z Objective-c

Pokud se domníváte, Xamarin.iOS by měly podporovat typ Nejistá, prosím soubor požadavek vylepšení v [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4109mt4109-failed-to-compile-the-generated-registrar-code-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4109"/>Kompilace kódu generovaného registrátora MT4109 se nezdařila. Prosím http://bugzilla.xamarin.com souborů sestav chyb

Kompilace generovaného kódu pro registrátora se nezdařila. V protokolu sestavení bude obsahovat výstup z nativního kompilátoru, která vysvětluje, proč není kompilování kódu.

Toto je vždy chyby v Xamarin.iOS; prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com) s projektu nebo testovací případ.

### <a name="a-namemt4110mt4110-the-registrar-cannot-marshal-the-out-parameter-of-type--in-signature-for-method-"></a><a name="MT4110"/>MT4110 Registrátora nelze zařazování výstupní parametr typu `*` v podpis pro metodu `*`.

### <a name="a-namemt4111mt4111-the-registrar-cannot-build-a-signature-for-type--in-method-"></a><a name="MT4111"/>MT4111 Registrátora nemůže vytvořit podpis pro typ `*` v metodě `*`.

### <a name="a-namemt4112mt4112-the-registrar-found-an-invalid-type--registering-generic-types-with-objective-c-is-not-supported-and-may-lead-to-random-behavior-andor-crashes-for-backwards-compatibility-with-older-versions-of-xamarinios-it-is-possible-to-ignore-this-error-by-passing---unsupported--enable-generics-in-registrar-as-an-additional-mtouch-argument-in-the-projects-ios-build-options-page-see-developerxamarincomguidesiosadvancedtopicsregistrarhttpsdeveloperxamarincomguidesiosadvancedtopicsregistrar-for-more-information"></a><a name="MT4112"/>MT4112 Registrátora nalezen neplatný typ `*`. Registrace obecné typy jazyka Objective-C není podporován a může dojít k náhodné chování nebo dojde k chybě (pro zpětnou kompatibilitu s starší verze Xamarin.iOS je možné tuto chybu ignorovat předáním `--unsupported--enable-generics-in-registrar` jako další mtouch argument v projektu iOS sestavení možnosti na stránce. V tématu [developer.xamarin.com/guides/ios/advanced_topics/registrar](https://developer.xamarin.com/guides/ios/advanced_topics/registrar) informace).

### <a name="a-namemt4113mt4113-the-registrar-found-a-generic-method--exporting-generic-methods-is-not-supported-and-will-lead-to-random-behavior-andor-crashes"></a><a name="MT4113"/>MT4113 Registrátora nalezen obecná metoda: '\*.\*'. Export obecné metody není podporováno a povede k náhodné chování nebo dojde k chybě.

### <a name="a-namemt4114mt4114-unexpected-error-in-the-registrar-for-the-method----please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4114"/>MT4114 Neočekávaná chyba v registrátora pro metodu '\*.\*'-prosím http://bugzilla.xamarin.com souborů sestav chyb

### <a name="a-namemt4116mt4116-could-not-register-the-assembly--"></a><a name="MT4116"/>MT4116 nelze zapsat sestavení ' *': *

### <a name="a-namemt4117mt4117-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-method-takes--parameters-while-the-managed-method-has--parameters"></a><a name="MT4117"/>MT4117 Registrátora nalezen neshoda signatury v metodě '*.*'-modulu pro výběr označuje metodu trvá * parametry, zatímco spravované metoda má * parametry.

### <a name="a-namemt4118mt4118-cannot-register-two-managed-types--and--with-the-same-native-name-"></a><a name="MT4118"/>MT4118 nelze zaregistrovat dva spravované typy ('\*'a'\*') se stejným názvem nativní ('* ').

### <a name="a-namemt4119mt4119-could-not-register-the-selector--of-the-member--because-the-selector-is-already-registered-on-a-different-member"></a><a name="MT4119"/>MT4119 nelze zapsat modulu pro výběr '\*'člena'\*. *, protože modulu pro výběr je již zaregistrován na jiný člen.

### <a name="a-namemt4120mt4120-the-registrar-found-an-unknown-field-type--in-field--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4120"/>MT4120 Registrátora nalezen typ neznámé pole "\*, v poli"\*. * ". Prosím http://bugzilla.xamarin.com souborů sestav chyb

Tato chyba označuje chyby v Xamarin.iOS. Prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4121mt4121-cannot-use-gccg-to-compile-the-generated-code-from-the-static-registrar-when-using-the-accounts-framework-the-header-files-provided-by-apple-used-during-the-compilation-require-clang-either-use-clang---compilerclang-or-the-dynamic-registrar---registrardynamic"></a><a name="MT4121"/>MT4121 nelze použít RSZ / G ++ kompilace generovaného kódu statických registrátorem při použití rozhraní účty (soubory hlaviček poskytovaných společností Apple během kompilace vyžadují Clang). Buď použijte Clang (--kompilátoru: clang) nebo dynamické registrátora (--registrátora: dynamické).

### <a name="a-namemt4122mt4122-cannot-use-the-clang-compiler-provided-in-the--sdk-to-compile-the-generated-code-from-the-static-registrar-when-non-ascii-type-names--are-present-in-the-application-either-use-gccg---compilergccg-the-dynamic-registrar---registrardynamic-or-a-newer-sdk"></a><a name="MT4122"/>MT4122 nelze použít kompilátoru Clang součástí *.* SDK kompilace generovaného kódu statických registrátorem, když jiné sady než ASCII zadejte názvy ('* ') jsou k dispozici v aplikaci. Buď použijte RSZ / G ++ (--kompilátoru: RSZ | g ++), dynamické registrátora (--registrátora: dynamické) nebo novější SDK.

### <a name="a-namemt4123mt4123-the-type-of-the-variadic-parameter-in-the-variadic-function--must-be-systemintptr"></a><a name="MT4123"/>MT4123 Typ variadická parametru ve funkci variadická ' *' musí být System.IntPtr.

### <a name="a-namemt4124mt4124-invalid--found-on--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4124"/>Neplatný MT4124 * najít na ' *'. Prosím http://bugzilla.xamarin.com souborů sestav chyb

Tato chyba označuje chyby v Xamarin.iOS. Prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4125mt4125-the-registrar-found-an-invalid-type--in-signature-for-method--the-interface-must-have-a-protocol-attribute-specifying-its-wrapper-type"></a><a name="MT4125"/>MT4125 Registrátora nalezen neplatný typ '\*"v podpis pro metodu'\*': rozhraní musí mít atribut protokolu určení jeho typu obálku.

### <a name="a-namemt4126mt4126-cannot-register-two-managed-protocols--and--with-the-same-native-name-"></a><a name="MT4126"/>MT4126 nelze zaregistrovat dva spravované protokoly ('\*'a'\*') se stejným názvem nativní ('* ').

### <a name="a-namemt4127mt4127-cannot-register-more-than-one-interface-method-for-the-method--which-is-implementing-"></a><a name="MT4127"/>MT4127 nelze zaregistrovat více než jedné metody rozhraní '\*"(což je implementace '\*').

### <a name="a-namemt4128mt4128--the-registrar-found-an-invalid-generic-parameter-type--in-the-method--the-generic-parameter-must-have-an-nsobject-constraint"></a><a name="MT4128"/>MT4128 Registrátora nalezen typ neplatný obecný parametr '\*'v metodě'\*'. Obecný parametr musí mít omezení 'NSObject'.

### <a name="a-namemt4129mt4129--the-registrar-found-an-invalid-generic-return-type--in-the-method--the-generic-return-type-must-have-an-nsobject-constraint"></a><a name="MT4129"/>MT4129 Registrátora nalezen neplatný návratový typ Obecné '\*'v metodě'\*'. Obecné návratový typ musí mít omezení 'NSObject'.

### <a name="a-namemt4130mt4130--the-registrar-cannot-export-static-methods-in-generic-classes-"></a><a name="MT4130"/>MT4130 Registrátora nelze exportovat statických metod v obecné třídy ('* ').

### <a name="a-namemt4131mt4131--the-registrar-cannot-export-static-properties-in-generic-classes-"></a><a name="MT4131"/>MT4131 Registrátora nelze exportovat statické vlastnosti v obecné třídy ('\*.\*').

### <a name="a-namemt4132mt4132--the-registrar-found-an-invalid-generic-return-type--in-the-property--the-return-type-must-have-an-nsobject-constraint"></a><a name="MT4132"/>MT4132 Registrátora nalezen neplatný návratový typ Obecné '\*'ve vlastnosti'\*'. Návratový typ musí mít omezení 'NSObject'.

### <a name="a-namemt4133mt4133--cannot-construct-an-instance-of-the-type--from-objective-c-because-the-type-is-generic-runtime-exception"></a><a name="MT4133"/>MT4133 nelze vytvořit instanci typu ' *' z jazyka Objective-C protože typ je obecná. [Výjimku modulu Runtime]

### <a name="a-namemt4134mt4134--your-application-is-using-the--framework-which-isnt-included-in-the-ios-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-ios--while-youre-building-with-the-ios--sdk-please-select-a-newer-sdk-in-your-apps-ios-build-options"></a><a name="MT4134"/>MT4134 Vaše aplikace používá ' *' framework, který není součástí sada SDK, který používáte k sestavení aplikace iOS (toto rozhraní byla zavedena v systému iOS *, zatímco již vytváříte s iOS * SDK.) Vyberte novější SDK v vaší aplikace iOS možnosti sestavení.

### <a name="a-namemt4135mt4135--the-member--has-an-export-attribute-that-doesnt-specify-a-selector-a-selector-is-required"></a><a name="MT4135"/>MT4135 Člen '\*.\*' má atribut exportu, která neurčuje selektor. Selektor je povinný.

### <a name="a-namemt4136mt4136--the-registrar-cannot-marshal-the-parameter-type--of-the-parameter--in-the-method-"></a><a name="MT4136"/>MT4136 Registrátora nelze zařazování typ parametru se\*'parametru'\*'v metodě'\*. *.

<!-- MT4137 is unused -->

### <a name="a-namemt4138mt4138--the-registrar-cannot-marshal-the-property-type--of-the-property-"></a><a name="MT4138"/>MT4138 Registrátora nelze zařazování typ vlastnosti '\*'vlastnosti'\*. * ".

### <a name="a-namemt4139mt4139--the-registrar-cannot-marshal-the-property-type--of-the-property--properties-with-the-connect-attribute-must-have-a-property-type-of-nsobject-or-a-subclass-of-nsobject"></a><a name="MT4139"/>MT4139 Registrátora nelze zařazování typ vlastnosti '\*'vlastnosti'\*. * ". Vlastnosti s atributem [Connect] musí mít typ vlastnosti NSObject (nebo podtřídou třídy NSObject).

### <a name="a-namemt4140mt4140--the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-variadic-method-takes--parameters-while-the-managed-method-has--parameters"></a><a name="MT4140"/>MT4140 Registrátora nalezen neshoda signatury v metodě '*.*'-modulu pro výběr označuje variadická metoda přebírá * parametry, zatímco spravované metoda má * parametry.

### <a name="a-namemt4141mt4141--cannot-register-the-selector--on-the-member--because-xamarinios-implicitly-registers-this-selector"></a><a name="MT4141"/>MT4141 nelze zaregistrovat modulu pro výběr '\*'na člena,\*, protože Xamarin.iOS implicitně zaregistruje tento selektor.

K tomu dojde při vytvoření podtřídy typu framework a při pokusu o implementaci 'zachovat', 'vydání' nebo 'dealloc' metody:

```csharp
class MyNSObject : NSObject
{
    [Export ("retain")]
    new void Retain () {}

    [Export ("release")]
    new void Release () {}

    [Export ("dealloc")]
    new void Dealloc () {}
}
```

Je ale možné přepsat tyto metody, pokud třída není první podtřídou třídy rozhraní zadejte:

```csharp
class MyNSObject : NSObject
{
}

class MyCustomNSObject : MyNSObject
{
    [Export ("retain")]
    new void Retain () {}

    [Export ("release")]
    new void Release () {}

    [Export ("dealloc")]
    new void Dealloc () {}
}
```

V takovém případě se přepíše Xamarin.iOS `retain`, `release` a `dealloc` na `MyNSObject` třída, a nedojde ke konfliktu.

### <a name="a-namemt4142mt4142-failed-to-register-the-type-"></a><a name="MT4142"/>MT4142: Nepodařilo se zaregistrovat typ ' *'.
### <a name="a-namemt4143mt4143-the-objectivec-class--could-not-be-registered-it-does-not-seem-to-derive-from-any-known-objectivec-class-including-nsobject"></a><a name="MT4143"/>MT4143: ObjectiveC třídy ' *' nebylo možné zaregistrovat, je zřejmě není odvozena od všechny známé ObjectiveC třídy (včetně NSObject).
### <a name="a-namemt4144mt4144-cannot-register-the-method--since-it-does-not-have-an-associated-trampoline-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4144"/>MT4144: Nelze zaregistrovat metodu, *, protože nemá přidružené trampoline. Sestava chyb prosím soubor na http://bugzilla.xamarin.com.

To ukazuje na chybu v Xamarin.iOS. Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4145mt4145-invalid-enum--enums-with-the-native-attribute-must-have-a-underlying-enum-type-of-either-long-or-ulong"></a><a name="MT4145"/>MT4145: Neplatný výčtu ' *': výčty s atributem [nativní] musí mít základní typ výčtu 'dlouhý' nebo 'ulong'.
### <a name="a-namemt4146mt4146-the-name-parameter-of-the-registrar-attribute-on-the-class---contains-an-invalid-character--"></a><a name="MT4146"/>MT4146: Název parametru atribut registrátora pro třídu\*' ('\*') obsahuje neplatný znak: '\*' (\*).

Název třídy Objectice-C nesmí obsahovat prázdné znaky, což znamená, že `Register` nemůže mít atribut na odpovídající spravované třídy `Name` parametr nesmí obsahovat prázdné znaky buď.

Ověřte, že `Register` atribut na spravované třídy uvedený v chybové zprávě neobsahuje žádné prázdné znaky.

### <a name="a-namemt4147mt4147-detected-a-protocol-inheriting-from-the-jsexport-protocol-while-using-the-dynamic-registrar-it-is-not-possible-to-export-protocols-to-javascriptcore-dynamically-the-static-registrar-must-be-used-add---registrarstatic-to-the-additional-mtouch-arguments-in-the-projects-ios-build-options-to-select-the-static-registrar"></a><a name="MT4147"/>MT4147: Zjištěna protokol dědění z protokol JSExport při použití dynamické registrátora. Není možné exportovat protokoly do JavaScriptCore dynamicky; statické registrátora se musí použít (přidání ' – registrátora: statické k další mtouch argumentů v projektu iOS můžete vybrat jednu statickou registrátora možností sestavení).
### <a name="a-namemt4148mt4148-the-registrar-found-a-generic-protocol--exporting-generic-protocols-is-not-supported"></a><a name="MT4148"/>MT4148: Registrátora nalezen obecný protokol: ' *'. Obecné protokoly export není podporován.
### <a name="a-namemt4149mt4149-cannot-register-the-method--because-the-type-of-the-first-parameter--does-not-match-the-category-type-"></a><a name="MT4149"/>MT4149: Nelze zaregistrovat metodu '\*.\*, protože typ prvního parametru ('\*') neodpovídá typu kategorie ('\*').
### <a name="a-namemt4150mt4150-cannot-register-the-type--because-the-type-property--in-its-category-attribute-does-not-inherit-from-nsobject"></a><a name="MT4150"/>MT4150: Nelze zaregistrovat typ '\*, protože vlastnost Type ('\*') ve své kategorii atribut nedědí z NSObject.
### <a name="a-namemt4151mt4151-cannot-register-the-type--because-the-type-property-in-its-category-attribute-isnt-set"></a><a name="MT4151"/>MT4151: Nelze zaregistrovat typ ' *, protože není nastavena vlastnost Type v jeho atribut kategorie.
### <a name="a-namemt4152mt4152-cannot-register-the-type--as-a-category-because-it-implements-inativeobject-or-subclasses-nsobject"></a><a name="MT4152"/>MT4152: Nelze zaregistrovat typ ' *' jako kategorii protože implementuje INativeObject nebo podtřídy NSObject.
### <a name="a-namemt4153mt4153-cannot-register-the-type--as-a-category-because-its-generic"></a><a name="MT4153"/>MT4153: Nelze zaregistrovat typu '\*' jako kategorii vzhledem k tomu, že je obecná.
### <a name="a-namemt4154mt4154-cannot-register-the-method--as-a-category-method-because-its-generic"></a><a name="MT4154"/>MT4154: Nelze zaregistrovat metodu '\*se jako metoda kategorie vzhledem k tomu, že je obecná.
### <a name="a-namemt4155mt4155-cannot-register-the-method--with-the-selector--as-a-category-method-on--because-the-objective-c-already-has-an-implementation-for-this-selector"></a><a name="MT4155"/>MT4155: Nelze zaregistrovat metodu '\*'se selektorem'\*se jako metoda kategorie na ' *' protože jazyka Objective-C již implementace pro tento selektor.
### <a name="a-namemt4156mt4156-cannot-register-two-categories--and--with-the-same-native-name-"></a><a name="MT4156"/>MT4156: Nelze zaregistrovat dvě kategorie ('\*'a'\*') se stejným názvem nativní ('* ').
### <a name="a-namemt4157mt4157-cannot-register-the-category-method--because-at-least-one-parameter-is-required-and-its-type-must-match-the-category-type-"></a><a name="MT4157"/>MT4157: Nelze zaregistrovat metodu kategorie '\*, protože je vyžadován minimálně jeden parametr (a její typ se musí shodovat s typem kategorie '\*')
### <a name="a-namemt4158mt4158-cannot-register-the-constructor--in-the-category--because-constructors-in-categories-are-not-supported"></a><a name="MT4158"/>MT4158: Nelze zaregistrovat konstruktoru * v kategorii * protože konstruktory v kategoriích nejsou podporovány.
### <a name="a-namemt4159mt4159-cannot-register-the-method--as-a-category-method-because-category-methods-must-be-static"></a><a name="MT4159"/>MT4159: Nelze zaregistrovat metodu ' *' jako metodu kategorie protože kategorie metody musí být statické.

### <a name="a-namemt4160mt4160-invalid-character---found-in-selector--for-"></a><a name="MT4160"/>MT4160: Neplatný znak '\*' (\*) v selektoru nalezen '\*'pro'\*'.

### <a name="a-namemt4161mt4161-the-registrar-found-an-unsupported-structure--all-fields-in-a-structure-must-also-be-structures-field--with-type-2-is-not-a-structure"></a><a name="MT4161"/>MT4161: Registrátora nalezen nepodporovaný struktura '\*': všechna pole ve struktuře musí být také struktury (pole '\*' typu '{2}' není struktury).

Registrátora najít struktura s Nepodporovaná pole.

Všechna pole ve struktuře, která je vystaven jazyka Objective-C musí být také struktury (ne třídy).

### <a name="a-namemt4162mt4162-the-type--used-as--2-is-not-available-in---it-was-introduced-in---please-build-with-a-newer--sdk-usually-done-by-using-the-most-recent-version-of-xcode"></a><a name="MT4162"/>MT4162: Typ '\*"(použít jako * {2}) není k dispozici v ** (byla zavedena v * *)\* prosím sestavení s novější * SDK (obvykle provádí pomocí nejnovější verzi Xcode.

Registrátora najít typ, který není zahrnutý v aktuální sadě SDK.

Upgradujte Xcode.

### <a name="a-namemt4163mt4163-internal-error-in-the-registrar--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4163"/>MT4163: Interní chyba v registrátora (*). Prosím http://bugzilla.xamarin.com souborů sestav chyb

Tato chyba označuje chyby v Xamarin.iOS. Prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4164mt4164-cannot-export-the-property--because-its-selector--is-an-objective-c-keyword-please-use-a-different-name"></a><a name="MT4164"/>MT4164: Nelze exportovat vlastnost '\*, protože jeho selektor '\*' je – klíčové slovo jazyka Objective-C. Použijte prosím jiný název.

Selektor pro dané nemovitosti není platný identifikátor jazyka Objective-C.

Použijte platný identifikátor jazyka Objective-C jako selektorů.

### <a name="a-namemt4165mt4165-the-registrar-couldnt-find-the-type-systemvoid-in-any-of-the-referenced-assemblies"></a><a name="MT4165"/>MT4165: Registrátora nejde najít typ 'System.Void' v žádné z odkazovaných sestaveních.

Tuto chybu pravděpodobně ukazuje na chybu v Xamarin.iOS. Prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4166mt4166-cannot-register-the-method--because-the-signature-contains-a-type--that-isnt-a-reference-type"></a><a name="MT4166"/>MT4166: Nelze zaregistrovat metodu '\*, protože podpis obsahuje typ (\*), není typu odkazu.

Obvykle to ukazuje na chybu v Xamarin.iOS; Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4167mt4167-cannot-register-the-method--because-the-signature-contains-a-generic-type--with-a-generic-argument-type-that-isnt-an-nsobject-subclass-"></a><a name="MT4167"/>MT4167: Nelze zaregistrovat metodu '\*, protože podpis obsahuje obecného typu (\*) s typem obecné argument, který není podtřídy NSObject (*).

Obvykle to ukazuje na chybu v Xamarin.iOS; Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4168mt4168-cannot-register-the-type-managedname-because-its-objective-c-name-exportedname-is-an-objective-c-keyword-please-use-a-different-name"></a><a name="MT4168"/>MT4168: Nelze zaregistrovat typ ' {spravované\_název}, protože jeho název jazyka Objective-C ' {exportovat\_název}' je – klíčové slovo jazyka Objective-C. Použijte prosím jiný název.

Název jazyka Objective-C pro dotyčném typ není platný identifikátor jazyka Objective-C.

Použijte platný identifikátor jazyka Objective-C.

### <a name="a-namemt4169mt4169-failed-to-generate-a-pinvoke-wrapper-for-method-message"></a><a name="MT4169"/>MT4169: Nepodařilo se vygenerovat obálku P/Invoke {metody}: {message}

Generování funkci P/Invoke obálku pro uvedených Xamarin.iOS se nezdařilo.
Zkontrolujte prosím hlášené chybovou zprávu pro základní příčinu.

### <a name="a-namemt4170mt4170-the-registrar-cant-convert-from-managed-type-to-native-type-for-the-return-value-in-the-method-method"></a><a name="MT4170"/>MT4170: Registrátora nelze převést z {spravovaného typu} na {nativní type} pro vrácenou hodnotu v metodě {metoda}.

Naleznete v popisu chyby <a href="#MT4172">MT4172</a>.

### <a name="a-namemt4171mt4171-the-bindas-attribute-on-the-member-member-is-invalid-the-bindas-type-type-is-different-from-the-property-type-type"></a><a name="MT4171"/>MT4171: Atribut BindAs na člena, {člen} je neplatný: typ BindAs {type} se liší od vlastnost typu {type}.

Zkontrolujte prosím, že typ atributu BindAs shoduje s typem člena, který je připojen k.

### <a name="a-namemt4172mt4172-the-registrar-cant-convert-from-native-type-to-managed-type-for-the-parameter-parameter-name-in-the-method-method"></a><a name="MT4172"/>MT4172: Registrátora nelze převést z {nativní type} na {spravovaného typu} pro parametr {parametr name}' v metodě {metoda}.

Registrátora nepodporuje převod mezi uvedených typů.

Toto je chyby v Xamarin.iOS, když rozhraní API dotyčném poskytl Xamarin.iOS; Oznamte chybu v [http://bugzilla.xamarin.com][1].

Pokud spustíte do této při vývoji vazby projektu nativní knihovny, nám otevřený a přidat podporu pro nové kombinace typů. Pokud je to tento případ, prosím soubor požadavek vylepšení ([http://bugzilla.xamarin.com][2]) s testovací případ a my vám hodnocení.

[1]: https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS
[2]: https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS&component=General&bug_severity=enhancement

## <a name="mt5xxx-gcc-and-toolchain-error-messages"></a>MT5xxx: Chybové zprávy RSZ a nástrojů

### <a name="mt51xx-compilation"></a>MT51xx: kompilace

<!--
 MT5xxx GCC and toolchain
  MT51xx compilation
  -->

### <a name="a-namemt5101mt5101-missing--compiler-please-install-xcode-command-line-tools-component"></a><a name="MT5101"/>Chybí MT5101 ' *' kompilátoru. Nainstalujte Xcode součásti, nástroje příkazového řádku.

### <a name="a-namemt5102mt5102-failed-to-assemble-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5102"/>Ke kompilaci souboru se nezdařilo MT5102 ' *'. Prosím http://bugzilla.xamarin.com souborů sestav chyb

### <a name="a-namemt5103mt5103-failed-to-compile-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5103"/>MT5103 se nezdařilo zkompilovat soubor ' *'. Prosím http://bugzilla.xamarin.com souborů sestav chyb

### <a name="a-namemt5104mt5104-could-not-find-neither-the--nor-the--compiler-please-install-xcode-command-line-tools-component"></a><a name="MT5104"/>MT5104 nenalezl žádná '\*'ani'\*' kompilátoru. Nainstalujte Xcode součásti, nástroje příkazového řádku.

<!-- 5105 is used by mmp -->

### <a name="a-namemt5106mt5106-could-not-compile-the-files--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5106"/>MT5106: Nelze zkompilovat soubory ' *'. Prosím http://bugzilla.xamarin.com souborů sestav chyb

Obvykle to ukazuje na chybu v Xamarin.iOS; Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="mt52xx-linking"></a>MT52xx: Linking

<!--
  MT52xx linking
  -->

### <a name="a-namemt5201mt5201-native-linking-failed-please-review-the-build-log-and-the-user-flags-provided-to-gcc-"></a><a name="MT5201"/>Nativní MT5201 propojení se nezdařilo. Zkontrolujte v protokolu sestavení a příznaky uživatele poskytované RSZ: *

### <a name="a-namemt5202mt5202-native-linking-failed-please-review-the-build-log"></a><a name="MT5202"/>Nativní MT5202 propojení se nezdařilo. Zkontrolujte protokol sestavení.

### <a name="a-namemt5203mt5203-native-linking-warning-"></a><a name="MT5203"/>MT5203 nativní propojení upozornění: *

<!--- 5204-5208 are not used -->

### <a name="a-namemt5209mt5209-native-linking-error-"></a><a name="MT5209"/>MT5209 nativní chyba propojení: *

### <a name="a-namemt5210mt5210-native-linking-failed-undefined-symbol--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-are-properly-linked-in"></a><a name="MT5210"/>MT5210: Nativní propojení se nezdařilo, nedefinované symbol: *. Prosím ověřte, zda odkazujete všechny nezbytné architektury a nativní knihovny jsou správně propojené v.

To se stane, když nativní linkeru nelze najít symbol, který se odkazuje někde. Tady je několik důvodů, že to se může stát:

* Vazba třetí strany vyžaduje rozhraní, ale vazba není uveden v jeho `[LinkWith]` atribut. Řešení:
  - Pokud chcete vytvořit vazbu třetích stran, nebo mají přístup ke svému zdroji, Upravit vazby `[LinkWith]` atribut rozhraní je nutné zahrnout:

            [LinkWith ("mylib.a", Frameworks = "SystemConfiguration")]

  - Pokud nelze upravit vazby třetích stran, můžete ručně propojit s požadované framework předáním `-gcc_flags '-framework SystemFramework'` k `mtouch` (to se provádí úpravou argumenty další mtouch v projektu iOS sestavení možnosti na stránce. Mějte na paměti, že to musíte udělat pro každý projekt konfigurace).
* V některých případech spravované vazba se skládá z několika nativní knihovny, a všechny musí být obsaženy ve vazbách. Je možné, že více než jeden nativní knihovny do každého projektu vazby tak řešení, je právě přidejte všechny požadované nativní knihovny do projektu vazby.</li>
* Spravované vazba odkazuje na nativní symboly, které neexistují v knihovně nativní.
    Obvykle se to stane, když vazbu již nějakou dobu a nativní kód byla změněna během této doby tak, aby určité třídy nativní má buď přesunutý nebo přejmenovaný, zatímco vazba nebyla aktualizována.
* P/Invoke odkazuje na nativní symbol, který neexistuje. Počínaje Xamarin.iOS 7.4 <a href="#MT5214">MT5214</a> chyba bude nahlášena pro tento případ (Další informace naleznete v tématu MT5214).
* Vazba třetí strany / knihovny bylo vytvořeno prostřednictvím C++, ale vazba není uveden v jeho `[LinkWith]` atribut. To je obvykle poměrně snadno rozpoznat, protože symboly jsou pozměnění symboly C++ (jeden Běžným příkladem jsou `__ZNKSt9exception4whatEv`).
  - Pokud chcete vytvořit vazbu třetích stran, nebo mají přístup ke svému zdroji, Upravit vazby `[LinkWith]` atribut nastavit `IsCxx` příznak:

            [LinkWith ("mylib.a", IsCxx = true)]

  - Pokud nelze upravit vazby třetích stran nebo s knihovnou třetích stran propojujete ručně, můžete nastavit příznak ekvivalentní předáním <code>-cxx</code> k mtouch (to se provádí úpravou argumenty další mtouch v projektu iOS sestavení možnosti na stránce . Mějte na paměti, že to musíte udělat pro každý projekt konfigurace).

### <a name="a-namemt5211mt5211-native-linking-failed-undefined-objective-c-class--the-symbol--could-not-be-found-in-any-of-the-libraries-or-frameworks-linked-with-your-application"></a><a name="MT5211"/>MT5211: Nativní propojení třídy se nezdařilo, nedefinované jazyka Objective-C: \*. Symbol '\*' nebyl nalezen v žádném z knihovny nebo rozhraní, které jsou propojeny s vaší aplikací.

To se stane, když nativní linkeru nelze najít třídu jazyka Objective-C, který se odkazuje někde. Tady je několik důvodů, to se může stát: stejná jako u [MT5210](#MT5210) a dále:

* Vázaný protokol jazyka Objective-C vazbu třetích stran, ale není anotaci její <code>[Protocol]</code> atribut v jeho definice rozhraní api. Řešení:
  - Přidejte chybějící `[Protocol]` atribut:

              [BaseType (typeof (NSObject))]
              [Protocol] // Add this
              public interface MyProtocol
              {
              }

### <a name="a-namemt5212mt5212-native-linking-failed-duplicate-symbol-"></a><a name="MT5212"/>MT5212: Nativní propojení se nezdařilo, duplicitní symbol: *.

To se stane, když nativní linkeru zaznamená duplicitní symboly mezi všechny nativní knihovny. Po této chybě došlo pravděpodobně jeden nebo více [MT5213](#MT5213) chyby s umístění pro každý výskyt symbolu. Možné důvody této chyby:

* Stejné nativní knihovny je zahrnuta dvakrát.
* Dvě odlišné nativní knihovny dojít k definování stejné symboly.
* Nativní knihovny není správně vytvořen a stejného symbolu obsahuje více než jednou.
  Můžete to ověřit pomocí následující sadu příkazů na terminálu (nahraďte i386 x86_64/armv7/armv7s/arm64 podle které architektura umožňuje vytvářet pro):

        # Native libraries are usually fat libraries, containing binary code for
        # several architectures in the same file. First we extract the binary
        # code for the architecture we're interested in.
        lipo libNative.a -thin i386 -output libNative.i386.a

        # Now query the native library for the duplicated symbol.
        nm libNative.i386.a | fgrep 'SYMBOL'

        # You can also list the object files inside the native library.
        # In most cases this will reveal duplicated object files.
        ar -t libNative.i386.a

  Chcete-li několik možných způsoby:

  - Požádejte zprostředkovatele nativní knihovny opravit a poskytují aktualizovaná verze.
  - Opravte si sami a to odebráním souborů navíc objektu (to funguje jenom v případě problému je ve skutečnosti soubory duplicitní objektů)

            # Find out if the library is a fat library, and which
            # architectures it contains.
            lipo -info libNative.a

            # Extract each architecture (i386/x86_64/armv7/armv7s/arm64) to a separate file
            lipo libNative.a -thin ARCH -output libNative.ARCH.a

            # Extract the object files for the offending architecture
            # This will remove the duplicates by overwriting them
            # (since they have the same filename)
            mkdir -p ARCH
            cd ARCH
            ar -x ../libNative.ARCH.a

            # Reassemble the object files in an .a
            ar -r ../libNative.ARCH.a *.o
            cd ..

            # Reassemble the fat library
            lipo *.a -create -output libNative.a

  - Požádejte linkeru odebrat nepoužité kódu. Pokud jsou splněny všechny z následujících podmínek bude Xamarin.iOS provádět automaticky:
    - Všechny třetích stran vazby `[LinkWith]` atributy povolili SmartLink:

            [assembly: LinkWith ("libNative.a", SmartLink = true)]

    - Ne `-gcc_flags` předaný mtouch (v poli Další mtouch argumenty projektu iOS sestavení možnosti).
    - Je také možné požádat linkeru přímo na odebrat nepoužité kód přidáním `-gcc_flags -dead_strip` k další mtouch argumentů v projektu iOS možnosti sestavení.

### <a name="a-namemt5213mt5213-duplicate-symbol-in--location-related-to-previous-error"></a><a name="MT5213"/>MT5213: Duplicitní symbol v: * (umístění souvisejících s předchozí chyby)

Tato chyba se nahlásí pouze společně s [MT5212](#MT5212). Najdete v tématu [MT5212](#MT5212) Další informace.

### <a name="a-namemt5214mt5214--native-linking-failed-undefined-symbol--this-symbol-was-referenced-the-managed-member--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-linked"></a><a name="MT5214"/>MT5214 nativní propojení se nezdařilo, není definovaná symbol: *. Bylo odkazováno tento symbol spravované člen *. Ověřte, že všechny potřebné architektury byla odkazované a nativní knihovny propojený.

Tato chyba se nahlásí, když spravovaného kódu obsahuje P/Invoke na nativní metodu, která neexistuje. Příklad:

```csharp
using System.Runtime.InteropServices;
class MyImports {
   [DllImport ("__Internal")]
   public static extern void MyInexistentFunc ();
}
```

Existuje několik možných řešení:

  -  Odeberte dotyčném P/vyvolá ze zdrojového kódu.
  -  Povolte spravované linkeru pro všechny sestavení (to se provádí v projektu iOS sestavení možnosti nastavením "Linkeru chování" na "Ve všech sestaveních"). Tato akce odebere všechny P/vyvolá nepoužíváte efektivně z aplikace (automaticky, místo ručně, jako předchozí bod). Nevýhodou je, že bude simulátoru buildy poněkud pomalejší a ukončovat aplikace, pokud používá reflexe – naleznete další informace o linkeru [sem](~/ios/deploy-test/linker.md) )
  -  Vytvořte druhý nativní knihovny, která obsahuje chybějící nativní symboly. Všimněte si, že toto je jenom alternativní řešení (Pokud jste dojít k pokusu o volat tyto funkce, vaše aplikace dojde k chybě).

### <a name="a-namemt5215mt5215-references-to--might-require-additional--frameworkxxx-or--lxxx-instructions-to-the-native-linker"></a><a name="MT5215"/>MT5215: Odkazuje na ' *' může vyžadovat další - framework = XXX nebo - lXXX pokyny nativní linkeru

Toto je upozornění, která určuje, že P/Invoke zjistilo odkazovat v knihovně, ale aplikace není propojení s ním.

### <a name="a-namemt5216mt5216-native-linking-failed-for--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5216"/>MT5216: Nativní propojení se nezdařilo pro *. Prosím http://bugzilla.xamarin.com souborů sestav chyb

Tato chyba se nahlásí při propojování výstup kompilátoru AOT.

Tuto chybu pravděpodobně ukazuje na chybu v Xamarin.iOS. Prosím soubor sestavy chyb v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt5217mt5217-native-linking-possibly-failed-because-the-linker-command-line-was-too-long--characters"></a><a name="MT5217"/>MT5217: Nativní propojení se pravděpodobně nezdařila, protože příkazový řádek linkeru byla příliš dlouhá (* znaků).

Nativní propojení se nezdařilo a je možné že k situaci došlo proto linkeru příkaz byla příliš dlouhá.

Projekty Xamarin.iOS bude často odkazovat nativní symboly dynamicky, což znamená, že nativní linkeru může odebrat takové nativní symboly během nativní proces propojení, protože nativní linkeru nejsou vidět, že se používají tyto symboly.

Obvykle Xamarin.iOS požádá nativní linkeru zachovat tyto značky pomocí `-u symbol` příznak linkeru, pokud je mnoho tyto značky, celý příkazového řádku ale může překročit maximální délku příkazového řádku podle operačního systému.

Existuje několik možných zdrojů pro takové dynamické symboly:

* Kódů do metody v staticky propojené knihovny (kde je název souboru dll `__Internal` do atribut DllImport `[DllImport ("__Internal")]`).
* Pole odkazy na umístění v paměti v staticky propojené knihovny z vazby projekty (`[Field]` atributy).
* Třídy jazyka Objective-C odkazované v staticky propojené knihovny z vazby projekty (při použití přírůstkové sestavení nebo bez použití statické registrátora).

Možná řešení:

* Povolte spravované linkeru (Pokud je to možné pro všechny sestavení místo pouze sestavení SDK). Pro dynamické symboly tak, aby je příkazového řádku linkeru není byla překročena maximální to může odebrat dostatečné zdroje.
* Snižte počet P/vyvolá, odkazy na pole nebo třídy jazyka Objective-C.
* Přepsání dynamického symboly kratší názvy.
* Předat `-dlsym:false` jako argument další mtouch v projektu iOS možnosti sestavení. Pomocí této možnosti Xamarin.iOS vygeneruje nativní referenční dokumentace v AOT zkompilovaný kód a nebudete muset požádat linkeru zachovat tento symbol. Ale sestavení tento pracuje pouze pro zařízení a způsobí chyby, pokud existují P/vyvolá na funkce, která nejsou ve se statickou knihovnou.
* Předat `--dynamic-symbol-mode=code` jako argumenty další mtouch v projektu iOS možnosti sestavení. Pomocí této možnosti Xamarin.iOS vygeneruje další nativní kód, který odkazuje na těchto symbolů místo žádostí nativní linkeru zachovat tyto symboly pomocí argumenty příkazového řádku. Nevýhodou tohoto přístupu je, že se zvýší velikost ke spustitelnému souboru poněkud.
* Povolte statické registrátora předáním `--registrar:static` jako argument další mtouch v projektu iOS možnosti sestavení (pro simulátoru sestavení, protože statické registrátora je již výchozí nastavení pro zařízení sestavení). Statické registrátora vygeneruje kód, který odkazuje na třídy jazyka Objective-C staticky, takže není nutné požádat nativní linkeru zachovat těchto tříd.
* Zakážete přírůstkové sestavení (pro zařízení sestavení). Pokud jsou povolené přírůstkové sestavení, kód vygenerovaný statické registrátora se nepovažuje nativní linkeru, což znamená, že Xamarin.iOS musíte požádat stále linkeru zachovat odkazuje třídy jazyka Objective-C. Proto vypnutí přírůstkové sestavení zabrání tohoto požadavku.

### <a name="a-namemt5218mt5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a><a name="MT5218"/>MT5218: Nelze ignorovat dynamické symbol {symbol} (--Ignorovat symbol dynamické = {symbol}) protože nebyla rozpoznána jako dynamické symbol.

Argument příkazového řádku `--ignore-dynamic-symbol=symbol` byl předán, ale tento symbol není symbol, který byl rozpoznán jako dynamické symbol, který musí být zachováno ručně.

Existují dva hlavní důvody:

* Symbol název je nesprávný.
    * Nemáte předřazení podtržítkem k názvu symbolu.
    * Symbol pro třídy jazyka Objective-C `OBJC_CLASS_$_<classname>`.
* Symbol je správný, ale je symbol, který je již zachovaná normálním způsobem (některé sestavení možnosti příčiny přesný seznam dynamické symbolů k odlišení).

### <a name="mt53xx-other-tools"></a>MT53xx: Dalších nástrojů

<!--
  MT53xx other tools
  -->

### <a name="a-namemt5301mt5301-missing-strip-tool-please-install-xcode-command-line-tools-component"></a><a name="MT5301"/>MT5301: Chybí 'pruhu' nástroj. Nainstalujte Xcode součásti, nástroje příkazového řádku.

### <a name="a-namemt5302mt5302-missing-dsymutil-tool-please-install-xcode-command-line-tools-component"></a><a name="MT5302"/>MT5302: Chybí nástroj 'dsymutil'. Nainstalujte Xcode součásti, nástroje příkazového řádku.

### <a name="a-namemt5303mt5303-failed-to-generate-the-debug-symbols-dsym-directory-please-review-the-build-log"></a><a name="MT5303"/>MT5303: Nepodařilo se vygenerovat symboly ladění (dSYM adresář). Zkontrolujte protokol sestavení.

Došlo k chybě při spuštění dsymutil na konečné .app adresář, který chcete vytvořit ladicími symboly. Zkontrolujte protokol sestavení dsymutil na výstup a zobrazit, jak můžete opravit.

### <a name="a-namemt5304mt5304-failed-to-strip-the-final-binary-please-review-the-build-log"></a><a name="MT5304"/>MT5304: Nepodařilo se odstranit poslední binárního souboru. Zkontrolujte protokol sestavení.

Došlo k chybě při spouštění nástroje 'pruhu, chcete-li odebrat informace o ladění z aplikace.

### <a name="a-namemt5305mt5305-missing-lipo-tool-please-install-xcode-command-line-tools-component"></a><a name="MT5305"/>MT5305: Chybí nástroj 'lipo'. Nainstalujte Xcode součásti, nástroje příkazového řádku.

### <a name="a-namemt5306mt5306-failed-to-create-the-a-fat-library-please-review-the-build-log"></a><a name="MT5306"/>MT5306: Nepodařilo se vytvořit knihovnu fat. Zkontrolujte protokol sestavení.

Došlo k chybě při spuštění nástroje 'lipo'. Zkontrolujte protokol sestavení zobrazíte 'lipo' nahlásila chybu.

### <a name="a-namemt5307mt5307-failed-to-sign-the-executable-please-review-the-build-log"></a><a name="MT5307"/>MT5307: Nepodařilo se přihlásit ke spustitelnému souboru. Zkontrolujte protokol sestavení.

Došlo k chybě při podepisování aplikace. Zkontrolujte protokol sestavení zobrazíte 'codesign' nahlásila chybu.

<!-- 5308 is used by mmp -->
<!-- 5309 is used by mmp -->
<!-- 5310 is used by mmp -->

## <a name="mt6xxx-mtouch-internal-tools-error-messages"></a>MT6xxx: mtouch interní nástroje chybové zprávy

### <a name="mt600x-stripper"></a>MT600x: Stripper

<!--
 MT6xxx mtouch internal tools
  MT600x Stripper
  -->

### <a name="a-namemt6001mt6001-running-version-of-cecil-doesnt-support-assembly-stripping"></a><a name="MT6001"/>MT6001: Spuštěná verze Cecil nepodporuje odstraňování sestavení

### <a name="a-namemt6002mt6002-could-not-strip-assembly-"></a><a name="MT6002"/>MT6002: Nelze odstranit sestavení `*`.

Došlo k chybě při odstraňování spravovaného kódu (odebrání kód IL) ze sestavení v aplikaci.

### <a name="a-namemt6003mt6003-unauthorizedaccessexception-message"></a><a name="MT6003"/>MT6003: [UnauthorizedAccessException zpráv]

Zabezpečení došlo k chybě při odstraňování ladění symboly z aplikace.

## <a name="mt7xxx-msbuild-error-messages"></a>MT7xxx: Chybové zprávy nástroje MSBuild

<!--
 MT7xxx msbuild errors
  -->

### <a name="a-namemt7001mt7001-could-not-resolve-host-ips-for-wifi-debugger-settings"></a><a name="MT7001"/>MT7001: Nebylo možné přeložit hostitele IP adresy pro nastavení ladicího programu Wi-Fi.

*Úlohy nástroje MSBuild: DetectDebugNetworkConfigurationTaskBase*

Řešení potíží:

- Zkuste spustit `csharp -e 'System.Net.Dns.GetHostEntry (System.Net.Dns.GetHostName ()).AddressList'` (který měl dát IP adresa a není chybu samozřejmě).
- Zkuste spustit "ping \`hostname\`" což může získáte další informace, jako je třeba: `cannot resolve MyHost.local: Unknown host`

V některých případech je problém "místní sítě" a je možné ho řešit přidáním Neznámý hostitel `127.0.0.1   MyHost.local` v `/etc/hosts`.

### <a name="a-namemt7002mt7002-this-machine-does-not-have-any-network-adapters-this-is-required-when-debugging-or-profiling-on-device-over-wifi"></a><a name="MT7002"/>MT7002: Tento počítač nemá žádné síťové adaptéry. To je potřeba, při ladění nebo profilace přes Wi-Fi na zařízení.

*Úlohy nástroje MSBuild: DetectDebugNetworkConfigurationTaskBase*

### <a name="a-namemt7003mt7003-the-app-extension--does-not-contain-an-infoplist"></a><a name="MT7003"/>MT7003: Rozšíření aplikace ' *' neobsahuje Info.plist.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7004mt7004-the-app-extension--does-not-specify-a-cfbundleidentifier"></a><a name="MT7004"/>MT7004: Rozšíření aplikace ' *' neurčuje CFBundleIdentifier.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7005mt7005-the-app-extension--does-not-specify-a-cfbundleexecutable"></a><a name="MT7005"/>MT7005: Rozšíření aplikace ' *' neurčuje CFBundleExecutable.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7006mt7006-the-app-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7006"/>MT7006: Rozšíření aplikace se\*"má neplatný CFBundleIdentifier (\*), nezačíná CFBundleIdentifier hlavní aplikace sady (*).

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7007mt7007-the-app-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a><a name="MT7007"/>MT7007: Rozšíření aplikace se\*' má CFBundleIdentifier (\*) který končí příponou neplatný "s příponou Key".

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7008mt7008-the-app-extension--does-not-specify-a-cfbundleshortversionstring"></a><a name="MT7008"/>MT7008: Rozšíření aplikace ' *' neurčuje CFBundleShortVersionString.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7009mt7009-the-app-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a><a name="MT7009"/>MT7009: Rozšíření aplikace ' *' má neplatný Info.plist: neobsahuje slovníku NSExtension.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7010mt7010-the-app-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionpointidentifier-value"></a><a name="MT7010"/>MT7010: Rozšíření aplikace ' *' má neplatný Info.plist: NSExtension slovník neobsahuje hodnotu NSExtensionPointIdentifier.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7011mt7011-the-watchkit-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionattributes-dictionary"></a><a name="MT7011"/>MT7011: WatchKit rozšíření ' *' má neplatný Info.plist: slovníku NSExtension neobsahuje slovníku NSExtensionAttributes.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7012mt7012-the-watchkit-extension--does-not-have-exactly-one-watch-app"></a><a name="MT7012"/>MT7012: WatchKit rozšíření ' *' nemá přesně jednu aplikaci sledovat.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7013mt7013-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-must-contain-the-watch-companion-capability"></a><a name="MT7013"/>MT7013: WatchKit rozšíření ' *' má neplatný Info.plist: UIRequiredDeviceCapabilities musí obsahovat možnosti průvodce sledováním vyhledáváním.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7014mt7014-the-watch-app--does-not-contain-an-infoplist"></a><a name="MT7014"/>MT7014: Aplikace sledovat ' *' neobsahuje Info.plist.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7015mt7015-the-watch-app--does-not-specify-a-cfbundleidentifier"></a><a name="MT7015"/>MT7015: Sledování aplikace "*" neurčuje CFBundleIdentifier.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7016mt7016-the-watch-app--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7016"/>MT7016: Sledování aplikace "\*" má neplatný CFBundleIdentifier (\*), nezačíná CFBundleIdentifier hlavní aplikace sady (*).

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7017mt7017-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected-watch-4-but-found--"></a><a name="MT7017"/>MT7017: Sledování aplikace "\*" nemá platnou hodnotu UIDeviceFamily. Očekávané, podívejte se na (4), ale nalezen '\* (*)'.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7018mt7018-the-watch-app--does-not-specify-a-cfbundleexecutable"></a><a name="MT7018"/>MT7018: Sledování aplikace "*" neurčuje CFBundleExecutable

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7019mt7019-the-watch-app--has-an-invalid-wkcompanionappbundleidentifier-value--it-does-not-match-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7019"/>MT7019: Sledování aplikace "\*' má neplatnou hodnotu WKCompanionAppBundleIdentifier ('\*'), neodpovídá CFBundleIdentifier hlavní aplikace sady ('* ').

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7020mt7020-the-watch-app--has-an-invalid-infoplist-the-wkwatchkitapp-key-must-be-present-and-have-a-value-of-true"></a><a name="MT7020"/>MT7020: Sledování aplikace "*" má neplatný Info.plist: WKWatchKitApp klíč musí existovat a mít hodnotu "true".

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7021mt7021-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a><a name="MT7021"/>MT7021: Sledování aplikace "*" má neplatný Info.plist: klíč LSRequiresIPhoneOS se nesmějí vyskytovat.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7022mt7022-the-watch-app--does-not-contain-a-watch-extension"></a><a name="MT7022"/>MT7022: Aplikace sledovat ' *' neobsahuje příponu sledovat.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7023mt7023-the-watch-extension--does-not-contain-an-infoplist"></a><a name="MT7023"/>MT7023: Sledování rozšíření ' *' neobsahuje Info.plist.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7024mt7024-the-watch-extension--does-not-specify-a-cfbundleidentifier"></a><a name="MT7024"/>MT7024: Sledování rozšíření ' *' neurčuje CFBundleIdentifier.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7025mt7025-the-watch-extension--does-not-specify-a-cfbundleexecutable"></a><a name="MT7025"/>MT7025: Sledování rozšíření ' *' neurčuje CFBundleExecutable.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7026mt7026-the-watch-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7026"/>MT7026: Sledování rozšíření '\*"má neplatný CFBundleIdentifier (\*), nezačíná CFBundleIdentifier hlavní aplikace sady (*).

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7027mt7027-the-watch-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a><a name="MT7027"/>MT7027: Sledování rozšíření '\*' má CFBundleIdentifier (\*) který končí příponou neplatný "s příponou Key".

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7028mt7028-the-watch-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a><a name="MT7028"/>MT7028: Sledování rozšíření ' *' má neplatný Info.plist: neobsahuje slovníku NSExtension.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7029mt7029-the-watch-extension--has-an-invalid-infoplist-the-nsextensionpointidentifier-must-be-comapplewatchkit"></a><a name="MT7029"/>MT7029: Sledování rozšíření ' *' má neplatný Info.plist: NSExtensionPointIdentifier musí být "com.apple.watchkit".

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7030mt7030-the-watch-extension--has-an-invalid-infoplist-the-nsextension-dictionary-must-contain-nsextensionattributes"></a><a name="MT7030"/>MT7030: Sledování rozšíření ' *' má neplatný Info.plist: slovníku NSExtension musí obsahovat NSExtensionAttributes.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7031mt7031-the-watch-extension--has-an-invalid-infoplist-the-nsextensionattributes-dictionary-must-contain-a-wkappbundleidentifier"></a><a name="MT7031"/>MT7031: Sledování rozšíření ' *' má neplatný Info.plist: slovníku NSExtensionAttributes musí obsahovat WKAppBundleIdentifier.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7032mt7032-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-should-not-contain-the-watch-companion-capability"></a><a name="MT7032"/>MT7032: WatchKit rozšíření ' *' má neplatný Info.plist: UIRequiredDeviceCapabilities nesmí obsahovat možnosti průvodce sledováním vyhledáváním.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7033mt7033-the-watch-app--does-not-contain-an-infoplist"></a><a name="MT7033"/>MT7033: Aplikace sledovat ' *' neobsahuje Info.plist.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7034mt7034-the-watch-app--does-not-specify-a-cfbundleidentifier"></a><a name="MT7034"/>MT7034: Sledování aplikace "*" neurčuje CFBundleIdentifier.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7035mt7035-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected--but-found--"></a><a name="MT7035"/>MT7035: Sledování aplikace "\*" nemá platnou hodnotu UIDeviceFamily. Očekává se\*, ale nalezený'\* (\*)'.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7036mt7036-the-watch-app--does-not-specify-a-cfbundleexecutable"></a><a name="MT7036"/>MT7036: Sledování aplikace "*" neurčuje CFBundleExecutable.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7037mt7037-the-watchkit-extension-extensionname-has-an-invalid-wkappbundleidentifier-value--it-does-not-match-the-watch-apps-cfbundleidentifier-"></a><a name="MT7037"/>MT7037: Rozšíření WatchKit {Název_rozšíření} má neplatnou hodnotu WKAppBundleIdentifier ('\*'), neodpovídá aplikace sledovat CFBundleIdentifier ('\*').

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7038mt7038-the-watch-app--has-an-invalid-infoplist-the-wkcompanionappbundleidentifier-must-exist-and-must-match-the-main-app-bundles-cfbundleidentifier"></a><a name="MT7038"/>MT7038: Sledování aplikace "*" má neplatný Info.plist: WKCompanionAppBundleIdentifier musí existovat a CFBundleIdentifier sadu hlavní aplikace se musí shodovat.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7039mt7039-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a><a name="MT7039"/>MT7039: Sledování aplikace "*" má neplatný Info.plist: klíč LSRequiresIPhoneOS se nesmějí vyskytovat.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7040mt7040-the-app-bundle-appbundlepath-does-not-contain-an-infoplist"></a><a name="MT7040"/>MT7040: Sady prostředků aplikace {AppBundlePath} neobsahuje Info.plist.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7041mt7041-main-infoplist-path-does-not-specify-a-cfbundleidentifier"></a><a name="MT7041"/>MT7041: Cesta hlavní Info.plist neurčuje CFBundleIdentifier.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7042mt7042-main-infoplist-path-does-not-specify-a-cfbundleexecutable"></a><a name="MT7042"/>MT7042: Cesta hlavní Info.plist neurčuje CFBundleExecutable.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7043mt7043-main-infoplist-path-does-not-specify-a-cfbundlesupportedplatforms"></a><a name="MT7043"/>MT7043: Cesta hlavní Info.plist neurčuje CFBundleSupportedPlatforms.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7044mt7044-main-infoplist-path-does-not-specify-a-uidevicefamily"></a><a name="MT7044"/>MT7044: Cesta hlavní Info.plist neurčuje UIDeviceFamily.

*Úlohy nástroje MSBuild: ValidateAppBundleTaskBase*

### <a name="a-namemt7045mt7045-unrecognized-format-"></a><a name="MT7045"/>MT7045: Nerozpoznaný formát: *.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

Kde * může být:

- odkazy řetězců
- pole
- dict
- bool
- reálná
- integer
- Datum
- data

### <a name="a-namemt7046mt7046-add-entry--incorrectly-specified"></a><a name="MT7046"/>MT7046: Přidat: položka, *, nesprávně zadán.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7047mt7047-add-entry--contains-invalid-array-index"></a><a name="MT7047"/>MT7047: Přidat: položka, *, obsahuje neplatný Index pole.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7048mt7048-add--entry-already-exists"></a><a name="MT7048"/>MT7048: Přidat: * položka již existuje.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7049mt7049-add-cant-add-entry--to-parent"></a><a name="MT7049"/>MT7049: Přidat: nelze přidat položku, *, aby nadřazený.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7050mt7050-delete-cant-delete-entry--from-parent"></a><a name="MT7050"/>MT7050: Odstranit: nelze odstranit položku, *, z nadřazeného objektu.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7051mt7051-delete-entry--contains-invalid-array-index"></a><a name="MT7051"/>MT7051: Odstranit: položka, *, obsahuje neplatný Index pole.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7052mt7052-delete-entry--does-not-exist"></a><a name="MT7052"/>MT7052: Odstranit: položka, *, neexistuje.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7053mt7053-import-entry--incorrectly-specified"></a><a name="MT7053"/>MT7053: Import: položka, *, nesprávně zadán.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7054mt7054-import-entry--contains-invalid-array-index"></a><a name="MT7054"/>MT7054: Import: položka, *, obsahuje neplatný Index pole.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7055mt7055-import-error-reading-file-"></a><a name="MT7055"/>MT7055: Import: Chyba při čtení souboru: *.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7056mt7056-import-cant-add-entry--to-parent"></a><a name="MT7056"/>MT7056: Import: nelze přidat položku, *, aby nadřazený.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7057mt7057-merge-cant-add-array-entries-to-dict"></a><a name="MT7057"/>MT7057: Sloučení: nelze přidat pole položky do dict.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7058mt7058-merge-specified-entry-must-be-a-container"></a><a name="MT7058"/>MT7058: Sloučení: Zadaná položka musí být kontejner.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7059mt7059-merge-entry--contains-invalid-array-index"></a><a name="MT7059"/>MT7059: Sloučení: položka, *, obsahuje neplatný Index pole.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7060mt7060-merge-entry--does-not-exist"></a><a name="MT7060"/>MT7060: Sloučení: položka, *, neexistuje.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7061mt7061-merge-error-reading-file-"></a><a name="MT7061"/>MT7061: Sloučení: Chyba při čtení souboru: *.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7062mt7062-set-entry--incorrectly-specified"></a><a name="MT7062"/>MT7062: Nastavit: položka, *, nesprávně zadán.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7063mt7063-set-entry--contains-invalid-array-index"></a><a name="MT7063"/>MT7063: Nastavit: položka, *, obsahuje neplatný Index pole.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7064mt7064-set-entry--does-not-exist"></a><a name="MT7064"/>MT7064: Nastavit: položka, *, neexistuje.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7065mt7065-unknown-propertylist-editor-action-"></a><a name="MT7065"/>MT7065: Neznámý Seznam_vlastností editor akce: *.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7066mt7066-error-loading--"></a><a name="MT7066"/>MT7066: Chyba při načítání ' *': *.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

### <a name="a-namemt7067mt7067-error-saving--"></a><a name="MT7067"/>MT7067: Chyba při ukládání ' *': *.

*Úlohy nástroje MSBuild: PropertyListEditorTaskBase*

## <a name="mt8xxx-runtime-error-messages"></a>MT8xxx: Chybové zprávy modulu Runtime

<!--
 MT8xxx runtime
  MT800x misc
  -->

### <a name="a-namemt8001mt8001--version-mismatch-between-the-native-xamarinios-runtime-and-monotouchdll-please-reinstall-xamarinios"></a><a name="MT8001"/>Neshoda verzí MT8001 mezi nativní modul runtime Xamarin.iOS a monotouch.dll. Přeinstalujte Xamarin.iOS.

### <a name="a-namemt8002mt8002--could-not-find-the-method--in-the-type-"></a><a name="MT8002"/>MT8002 nenalezl metodu '\*'v typu'\*'.

### <a name="a-namemt8003mt8003--failed-to-find-the-closed-generic-method--on-the-type-"></a><a name="MT8003"/>MT8003 se nepodařilo najít uzavřená obecná metoda '\*, na typu'\*'.

### <a name="a-namemt8004mt8004-cannot-create-an-instance-of--for-the-native-object-0x-of-type--because-another-instance-already-exists-for-this-native-object-of-type-"></a><a name="MT8004"/>MT8004: Nelze vytvořit instanci * pro nativní objekt 0 x * (typu ' *'), protože jiná instance již existuje pro tento objekt nativní (typu *).

### <a name="a-namemt8005mt8005-wrapper-type--is-missing-its-native-objectivec-class-"></a><a name="MT8005"/>MT8005: Typ obálky '\*'chybí jeho nativních tříd ObjectiveC'\*'.

### <a name="a-namemt8006mt8006-failed-to-find-the-selector--on-the-type-"></a><a name="MT8006"/>MT8006: Nepodařilo se najít modulu pro výběr '\*'na typ"\*.

### <a name="a-namemt8007mt8007-cannot-get-the-method-descriptor-for-the-selector--on-the-type--because-the-selector-does-not-correspond-to-a-method"></a><a name="MT8007"/>MT8007: Nelze získat popisovač metody pro selektor '\*, na typu'\*', protože nebude odpovídat modulu pro výběr metody

### <a name="a-namemt8008mt8008-the-loaded-version-of-xamariniosdll-was-compiled-for--bits-while-the-process-is--bits-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8008"/>MT8008: Načíst verzi Xamarin.iOS.dll byl kompilován pro * bits, když je proces * bits. V http://bugzilla.xamarin.com Oznamte chybu.

To znamená, že je něco špatně v procesu sestavení. Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8009mt8009-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-s-parameter--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8009"/>MT8009: Nepodařilo se najít bloku delegovat převod metody *.* s parametr #*. V http://bugzilla.xamarin.com Oznamte chybu.

To znamená, že rozhraní API nebyl vázán správně. Pokud je to rozhraní API vystavené Xamarin, Oznamte chybu v našem bugzilla ([http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)), pokud je vazba třetích stran, obraťte se na dodavatele.

### <a name="a-namemt8010mt8010-native-type-size-mismatch-between-xamariniosmacdll-and-the-executing-architecture-xamariniosmacdll-was-built-for--bit-while-the-current-process-is--bit"></a><a name="MT8010"/>MT8010: Nativní typ velikost Neshoda mezi Xamarin. [iOS | Mac] .dll a provádění architektura. Xamarin. [iOS | .Dll Mac] byl vytvořen pro *-bit, zatímco je aktuální proces *-bit.

To znamená, že je něco špatně v procesu sestavení. Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8011mt8011-unable-to-locate-the-delegate-to-block-conversion-attribute-delegateproxy-for-the-return-value-for-the-method--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8011"/>MT8011: Nepodařilo se najít delegát bloku převod atributu ([DelegateProxy]) pro návratovou hodnotu metody *.*. V http://bugzilla.xamarin.com Oznamte chybu.

Xamarin.iOS se nepodařilo najít požadované metoda za běhu (pro převod delegáta blok).

Obvykle to ukazuje na chybu v Xamarin.iOS; Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8012mt8012-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8012"/>MT8012: Neplatný DelegateProxyAttribute pro návratovou hodnotu metody *.*: DelegateType má hodnotu null. V http://bugzilla.xamarin.com Oznamte chybu.

Atribut DelegateProxy pro danou metodu je neplatný.

Obvykle to ukazuje na chybu v Xamarin.iOS; Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8013mt8013-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-2-specifies-a-type-without-a-handler-field-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8013"/>MT8013: Neplatný DelegateProxyAttribute pro návratovou hodnotu metody *.*: DelegateType ({2}) určuje typ bez 'obslužná rutina, pole. V http://bugzilla.xamarin.com Oznamte chybu.

Atribut DelegateProxy pro danou metodu je neplatný.

Obvykle to ukazuje na chybu v Xamarin.iOS; Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8014mt8014-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8014"/>MT8014: Neplatný DelegateProxyAttribute pro návratovou hodnotu metody *.*: The DelegateType společnosti ({2}), obslužné rutiny, pole má hodnotu null. V http://bugzilla.xamarin.com Oznamte chybu.

Atribut DelegateProxy pro danou metodu je neplatný.

Obvykle to ukazuje na chybu v Xamarin.iOS; Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8015mt8015-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-not-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8015"/>MT8015: Neplatný DelegateProxyAttribute pro návratovou hodnotu metody *.*: The DelegateType společnosti ({2}), obslužné rutiny, pole není delegáta, je *. V http://bugzilla.xamarin.com Oznamte chybu.

Atribut DelegateProxy pro danou metodu je neplatný.

Obvykle to ukazuje na chybu v Xamarin.iOS; Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8016mt8016-unable-to-convert-delegate-to-block-for-the-return-value-for-the-method--because-the-input-isnt-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8016"/>MT8016: Nebylo možné převést delegáta blokování pro návratovou hodnotu metody *.*, protože vstup není s delegátem, je *. V http://bugzilla.xamarin.com Oznamte chybu.

Atribut DelegateProxy pro danou metodu je neplatný.

Obvykle to ukazuje na chybu v Xamarin.iOS; Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<!-- 8017 is used by mmp -->

### <a name="a-namemt8018mt8018-internal-consistency-error-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT8018"/>MT8018: Interní konzistence došlo k chybě. Sestava chyb prosím soubor na http://bugzilla.xamarin.com.

To ukazuje na chybu v Xamarin.iOS. Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8019mt8019-could-not-find-the-assembly--in-the-loaded-assemblies"></a><a name="MT8019"/>MT8019: Nelze nalézt sestavení * v načíst sestavení.

To ukazuje na chybu v Xamarin.iOS. Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8020mt8020-could-not-find-the-module-with-metadatatoken--in-the-assembly-"></a><a name="MT8020"/>MT8020: Nebyl nalezen modul s MetadataToken * v sestavení *.

To ukazuje na chybu v Xamarin.iOS. Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8021mt8021-unknown-implicit-token-type-"></a><a name="MT8021"/>MT8021: Neznámý implicitní typ tokenu: *.

To ukazuje na chybu v Xamarin.iOS. Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8022mt8022-expected-the-token-reference--to-be-a--but-its-a--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT8022"/>MT8022: Očekává odkaz tokenu * být *, ale je *. Sestava chyb prosím soubor na http://bugzilla.xamarin.com.

To ukazuje na chybu v Xamarin.iOS. Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8023mt8023-an-instance-object-is-required-to-construct-a-closed-generic-method-for-the-open-generic-method--token-reference--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT8023"/>MT8023: Instance objektu je potřeba vytvořit uzavřená obecná metoda pro otevřete obecná metoda: * (referenční dokumentace token: *). Sestava chyb prosím soubor na http://bugzilla.xamarin.com.

To ukazuje na chybu v Xamarin.iOS. Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8024mt8024-could-not-find-a-valid-extension-type-for-the-smart-enum-smarttype-please-file-a-bug-at-httpsbugzillaxamarincom"></a><a name="MT8024"/>MT8024: Nepodařilo se najít typ rozšíření platný pro inteligentní výčtu {smart_type}. V https://bugzilla.xamarin.com Oznamte chybu.

To ukazuje na chybu v Xamarin.iOS. Oznamte chybu v [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).
