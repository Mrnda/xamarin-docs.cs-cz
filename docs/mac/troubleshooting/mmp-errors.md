---
title: Xamarin.Mac chybové zprávy (zhr)
description: Tento dokument obsahuje chyby generovaných zhr, tento nástroj umožňuje balíčků zkompilovaných sestavení do spustitelná aplikace Mac.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: f5cf4a9003d3fb468ffcf337c33730cb12238c44
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792755"
---
# <a name="xamarinmac-error-messages-mmp"></a>Xamarin.Mac chybové zprávy (zhr)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: zhr chybové zprávy

Například parametry, prostředí, chybí nástroje.

<a name="MM0000" />

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM0000: Neočekávaná chyba: soubor chyby nahlaste v http://bugzilla.xamarin.com

Došlo k neočekávané chybě. Prosím [hlášení o chybě](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) s nejblíže tolik informací, včetně:

* Úplné sestavení protokoly, s maximální podrobnosti (například `-v -v -v -v` v **zhr další argumenty**);
* Minimální testovacího případu reprodukujte chybu. a
* Všechny verze – informace

Nejjednodušší způsob, jak získat informace o přesnou verzi se má používat **Xamarin Studio** nabídce **Xamarin Studio** položku **zobrazit podrobnosti** tlačítko, kopírování a vkládání verze – informace (můžete použít **kopie informace** tlačítko).

<a name="MM0001" />

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001: Tato verze Xamarin.Mac vyžaduje Mono {0} (aktuální verze Mono je {1}). Aktualizujte prosím Mono.framework z http://mono-project.com/Downloads

<a name="MM0003" />

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003: Název aplikace '{0}.exe, je v konfliktu s SDK nebo produktu, název sestavení (.dll).

<a name="MM0007" />

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007: Sestavení kořenové '{0}' neexistuje

<a name="MM0008" />

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008: By měl poskytovat jeden kořenový sestavení pouze, který se nachází {0} sestavení: '{1}.

<a name="MM0010" />

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010: Nebylo možné rozložit argumenty příkazového řádku: {0}

<!-- 0013 is unused -->

<a name="MM0016" />

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016: Možnost '{0}' se již nepoužívá.

<a name="MM0017" />

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017: By měl poskytovat kořenové sestavení

<a name="MM0018" />

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018: Neznámý argument příkazového řádku: "{0}.

<a name="MM0020" />

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020: Platné možnosti '{0}"se"{1}'.

<a name="MM0023" />

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023: Název aplikace '{0}.exe, je v konfliktu s jinou sestavení uživatele.

<a name="MM0026" />

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026: Nebylo možné rozložit argument příkazového řádku '{0}': {1}

<a name="MM0043" />

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043: Uvolňování paměti Boehm není podporováno. SGen – uvolňování vybral místo.

<a name="MM0050" />

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050: Sestavení kořenové nemůžete zadat, pokud – ne kořenové sestavení je předán.

<a name="MM0051" />

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051: Výstupnímu adresáři (– výstupní) je povinný, pokud – ne kořenové sestavení je předán.

<a name="MM0053" />

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053: Nelze zakázat nový refcount s unifikované API.

<a name="MM0056" />

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056: Nelze najít Xcode některý z našich výchozí umístění. Nainstalujte Xcode nebo předat vlastní cesta pomocí – sdkroot =<path>

<a name="MM0059" />

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059: Nelze nalézt v systému aktuálně vybrané Xcode: {0};

<a name="MM0060" />

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060: Nelze nalézt aktuálně vybrané Xcode v systému. 'xcode – vybrat – tisk path' vrátila se{0}', ale tento adresář neexistuje.

<a name="MM0068" />

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068: Neplatná hodnota pro cílové rozhraní: {0}.

<a name="MM0071" />

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071: Neznámý platformy: *. Obvykle to ukazuje na chybu v Xamarin.Mac; prosím soubor sestavy chyb v https://bugzilla.xamarin.com s testovacího případu.

Obvykle to ukazuje na chybu v Xamarin.Mac; prosím soubor sestavy chyb v [ https://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) s testovacího případu.

<a name="MM0079" />

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079: Vnitřní chyba - žádný spustitelný soubor byl zkopírován do sady prostředků aplikace. Obraťte se na 'support@xamarin.com.

<a name="MM0080" />

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080: Zakázání NewRefCount, – nové-refcount:false, je zastaralý.

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091" />

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091: Vyžaduje tato verze Xamarin.Mac * SDK (dodávané s Xcode *). Buď upgradujte Xcode získání požadované hlavičky souborů nebo použít dynamické registrátora nebo nastavit chování spravované linkeru na odkaz platformy nebo pouze odkaz Framework sady SDK (a pokuste se vyhnout nových rozhraní API).

Xamarin.Mac vyžaduje soubory hlaviček, z SDK verze zadaná v chybové zprávě, k vytvoření vaší aplikace pomocí statické registrátora. Doporučeným způsobem, jak vyřešit tuto chybu se k upgradu Xcode získat požadované SDK, to bude zahrnovat všechny soubory požadované hlavičky. Pokud máte více verzí Xcode nainstalován, nebo chcete použít Xcode v jiné než výchozí umístění, ujistěte se, zda je nastavení správné umístění Xcode v předvolbách vaší IDE.

Jeden potenciální a alternativní řešení, je umožnit spravované linkeru. Tato akce odebere nepoužívané včetně rozhraní API, ve většině případů nové rozhraní API, kde jsou soubory hlaviček chybí (nebo jsou neúplné). To nebude fungovat, pokud váš projekt používá rozhraní API byla zavedena v novější SDK než jeden vaší Xcode ale nabízí.

Druhý potenciální a alternativní řešení, je místo toho použijte dynamické registrátora. To bude jiné než náklady na spuštění registrace dynamicky typů ale odebrat požadavek na záhlaví souboru. 

Poslední podají řešení může být použití starší verze Xamarin.Mac, ten, který podporuje sadu SDK do projektu vyžaduje.

<a name="MM0097" />

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: souboru machine.config '{0}' nebyl nalezen.

<a name="MM0098" />

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098: AOT kompilace je k dispozici pouze na Unified

<a name="MM0099" />

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MM0099: Vnitřní chyba {0}. Prosím soubor zprávu o chybě s testovacího případu (http://bugzilla.xamarin.com).

<a name="MM0114" />

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114: Hybridní AOT kompilace vyžaduje ve všech sestaveních být AOT zkompilovat.

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: kopírování souboru / symbolických odkazů (související projektu)

<a name="MM1034" />

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034: Nepodařilo se vytvořit symlink-'{souboru}' > "{cíle}": chyby {number}

### <a name="mm14xx-product-assemblies"></a>MM14xx: Sestavení produktu

<a name="MM1401" />

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401: Vyžaduje se{0}' sestavení chybí z odkazů

<a name="MM1402" />

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402: Sestavení '{0}' není kompatibilní s tímto nástrojem

<a name="MM1403" />

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403: {0} '{1}' nebyl nalezen. Cílová architektura '{0}' nelze použít pro zabalení aplikace.

<a name="MM1404" />

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404: Cílová architektura '{0}' je neplatný.

<a name="MM1405" />

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: useFullXamMacFramework musí vždy cíl framework .NET 4.5, není '{0}, která není platná

<a name="MM1406" />

#### <a name="mm1406-target-framework-0-is-invalid-when-targetting-xamarinmac-45-net-framwork"></a>MM1406: Cílové rozhraní,{0}' je neplatné, při které se budou zaměřovat Xamarin.Mac 4.5 .NET framwork.

<a name="MM1407" />

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407: Došlo k neshodě mezi Xamarin.Mac odkaz '{0}'a cílové rozhraní vybrána'{1}'.

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx: Chyby shromažďování (které nevyžadují linkeru) sestavení

<a name="MM1501" />

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501: Nelze přeložit odkaz: {0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600" />

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600: Není prac-O dynamické knihovny (Neznámý záhlaví 0 x{0}"): {1}.

<a name="MM1601" />

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601: Nejsou statické knihovny (Neznámý záhlaví '{0}"): {1}.

<a name="MM1602" />

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602: Není prac-O dynamické knihovny (Neznámý záhlaví 0 x{0}"): {1}.

<a name="MM1603" />

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603: Neznámý formát pro fat položku na pozici {0} v {1}.

<a name="MM1604" />

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604: Soubor typu {0} není souborem MachO ({1}).

## <a name="mm2xxx-linker"></a>MM2xxx: Linkeru

### <a name="mm20xx-linker-general-errors"></a>MM20xx: Chybami Linkeru (Obecné)

<a name="MM2001" />

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001: Nelze propojit sestavení

<a name="MM2002" />

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002: Nelze přeložit odkaz: {0}

<a name="MM2003" />

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003: Možnost '{0}' bude ignorován, protože propojení je zakázán.

<a name="MM2004" />

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004: Soubor definice linkeru navíc '{0}' nebyl nalezen.

<a name="MM2005" />

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005: Definice z '{0}' nebylo možné analyzovat.

<a name="MM2006" />

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006: Nativní knihovny '{0}' se odkazuje, ale nebyl nalezen.

<a name="MM2007" />

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007: Xamarin.Mac unifikované API proti úplné profil .NET nepodporuje propojení. Předejte příznak - nolink.

<a name="MM2009" />

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009: Odkazuje {0}.{1}     ** Tato zpráva se vztahuje k MM2006 **

<a name="MM2010" />

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010: Neznámý objekt HttpMessageHandler `{0}`. Platné hodnoty jsou HttpClientHandler (výchozí), CFNetworkHandler nebo NSUrlSessionHandler

<a name="MM2011" />

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011: Neznámý TLSProvider '{0}.  Platné hodnoty jsou výchozí nebo appletls

<a name="MM2012" />

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012: Pouze první {0} z {1} "Odkazuje" zobrazí upozornění. ** Tato zpráva související s 2009 **

<a name="MM2013" />

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013: Nepodařilo se vyřešit odkaz na "{0}", kterou se odkazuje v"{1}". Aplikace nebude obsahovat odkazované sestavení a může dojít k selhání za běhu.

<a name="MM2014" />

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014: Xamarin.Mac rozšíření nepodporují propojení. Žádost o propojení budou ignorovány. ** Tato zpráva je zastaralé v XM 3.6 + **

<!-- 2015 used by mtouch -->

<a name="MM2016" />

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016: Neplatný TlsProvider `{0}` možnost. Jedinou platnou hodnotou `{1}` se použije.

<a name="MM2017" />

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017: Nebylo možné zpracovat popis XML: {0}

<a name="MM202x" />

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x: Zpracování se nezdařilo vazby Optimalizátor `...`.

<a name="MM2100" />

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100: Xamarin.Mac klasické rozhraní API nepodporuje propojování platformy.

<a name="MM2103" />

#### <a name="mm2103-error-processing-assembly--"></a>MM2103: Chyba zpracování sestavení '\*': *

Došlo k neočekávané chybě při zpracování sestavení.

Sestavení, která způsobila problém jmenuje v chybové zprávě. Chcete-li tento problém vyřešit sestavení se musí být zadané v [Sestava chyb](https://bugzilla.xamarin.com) společně s protokol dokončení sestavení s podrobností povolené (tj. `-v -v -v -v` v **mtouch další argumenty**).

<a name="MM2104" />

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: Nelze vytvořit odkaz sestavení '{0}, protože je ve smíšeném režimu.

Ve smíšeném režimu sestavení nelze zpracovat linkeru.

V tématu https://msdn.microsoft.com/library/x0w2664k.aspx Další informace o sestavení ve smíšeném režimu.

## <a name="mm3xxx-aot"></a>MM3xxx: AOT

### <a name="mm30xx-aot-general-errors"></a>MM30xx: Chyby AOT (Obecné)

<a name="MM3001" />

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001: Může AOT sestavení '{0}.

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009" />

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009: AOT z '{0}' byla požadována, ale nebyl nalezen

<a name="MM3010" />

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010: Vyloučení AOT z '{0}' byla požadována, ale nebyl nalezen

## <a name="mm4xxx-code-generation"></a>MM4xxx: generování kódu

### <a name="mm40xx-driverm"></a>MM40xx: driver.m

<a name="MM4001" />

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001: Hlavní šablonu nelze rozšířit tak, aby `{0}`.

### <a name="mm41xx-registrar"></a>MM41xx: registrátora

<a name="MM4134" />

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134: Vaše aplikace používá '{0}' framework, který není zahrnutý v systému MacOS SDK, který používáte k sestavení aplikace (toto rozhraní byla zavedena v OSX {2}, zatímco již vytváříte s systému MacOS {1} SDK.) Tato konfigurace není podporována u registrátora statické (pass – registrátora: dynamické jako argument další zhr a vyberte možnost Mac sestavení vašeho projektu). Případně vyberte novější SDK v možnostech Mac sestavení vaší aplikace.

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx: RSZ a nástrojů

### <a name="mm51xx-compilation"></a>MM51xx: kompilace

<a name="MM5101" />

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101: Chybí '{0}' kompilátoru. Nainstalujte Xcode součásti, nástroje příkazového řádku'.

<!-- 5102 used by mtouch -->

<a name="MM5103" />

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103: Kompilace se nezdařila. Kód chyby - {0}. Prosím soubor sestavy chyb v http://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: propojování

<a name="MM5202" />

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202: Mono.framework MDK nebyl nalezen. Nainstalujte prosím MDK pro vaši verzi Mono.framework z http://mono-project.com/Downloads

<a name="MM5203" />

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203: Nelze najít libxammac.a, pravděpodobně kvůli poškozená Xamarin.Mac instalaci. Přeinstalujte Xamarin.Mac.

<a name="MM5204" />

#### <a name="mm5204-invalid-architecture-x8664-is-only-supported-on-non-classic-profiles"></a>MM5204: Architektura neplatný. X86_64 je podporována pouze u profilů ne klasický.

<a name="MM5205" />

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x8664-when---profilemobile"></a>MM5205: Architektura neplatný '{0}'. Platný architektury jsou i386 a x86_64 (když – profil = mobilní).

<a name="MM5218" />

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218: Nelze ignorovat dynamické symbol {symbol} (--Ignorovat symbol dynamické = {symbol}) protože nebyla rozpoznána jako dynamické symbol.

Najdete v článku [ekvivalentní mtouch upozornění](~/ios/troubleshooting/mtouch-errors.md#MT5218).

<!-- 5206 used by mtouch -->
<!-- 5207 used by mtouch -->
<!-- 5208 used by mtouch -->
<!-- 5209 used by mtouch -->
<!-- 5210 used by mtouch -->
<!-- 5211 used by mtouch -->
<!-- 5212 used by mtouch -->
<!-- 5213 used by mtouch -->
<!-- 5214 used by mtouch -->
<!-- 5215 used by mtouch -->
<!-- 5216 used by mtouch -->
<!-- 5217 used by mtouch -->

### <a name="mm53xx-other-tools"></a>MM53xx: dalších nástrojů

<a name="MM5301" />

#### <a name="mm5301-pkg-config-could-not-be-found-please-install-the-monoframework-from-httpmono-projectcomdownloads"></a>MM5301: pkg-config nebyl nalezen. Nainstalujte prosím Mono.framework z http://mono-project.com/Downloads

<!-- 5302 used by mtouch -->
<!-- 5303 used by mtouch -->
<!-- 5304 used by mtouch -->

<a name="MM5305" />

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305: Chybí nástroj 'otool'. Nainstalujte Xcode součásti, nástroje příkazového řádku.

<a name="MM5306" />

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306: Chybějící závislosti. Nainstalujte Xcode součásti, nástroje příkazového řádku.

<a name="MM5308" />

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308: Xcode licenční smlouvy nesmí byly přijaty.  Spusťte Xcode.

<a name="MM5309" />

#### <a name="mm5309-native-linking-failed-with-error-code-1--check-build-log-for-details"></a>MM5309: Nativní propojení se nezdařilo s kódem chyby 1.  Podrobnosti naleznete v protokolu sestavení.

<a name="MM5310" />

#### <a name="mm5310-installnametool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310: install_name_tool se nezdařilo s kódem chyby '{0}'. Podrobnosti naleznete v protokolu sestavení.

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx: modul runtime

### <a name="mm800x-misc"></a>MM800x: různé

<!-- 8000 used by mtouch -->
<!-- 8001 used by mtouch -->
<!-- 8002 used by mtouch -->
<!-- 8003 used by mtouch -->
<!-- 8004 used by mtouch -->
<!-- 8005 used by mtouch -->
<!-- 8006 used by mtouch -->
<!-- 8007 used by mtouch -->
<!-- 8008 used by mtouch -->
<!-- 8009 used by mtouch -->
<!-- 8010 used by mtouch -->
<!-- 8011 used by mtouch -->
<!-- 8012 used by mtouch -->
<!-- 8013 used by mtouch -->
<!-- 8014 used by mtouch -->
<!-- 8015 used by mtouch -->
<!-- 8016 used by mtouch -->

<a name="MM8017" />

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017: Uvolňování paměti Boehm není podporováno. Místo toho použijte SGen.

