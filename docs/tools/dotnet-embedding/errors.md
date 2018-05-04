---
title: Vložení .NET chyby
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/11/2018
ms.openlocfilehash: 677242ea12f8fd87d82f337eafd96a1743ad806a
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="net-embedding-errors"></a>Vložení chyby rozhraní .NET

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: Vytvoření vazby chybové zprávy

Například parametry, prostředí

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000" />

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: Neočekávaná chyba – vyplňte sestavy chyb v https://github.com/mono/Embeddinator-4000/issues

Došlo k neočekávané chybě. Prosím [souboru problém](https://github.com/mono/Embeddinator-4000/issues) s nejblíže tolik informací, včetně:

* Úplné sestavení protokoly, s maximální podrobnosti
* Minimální testovacího případu tomu chybu reprodukovat
* Všechny verze – informace

Nejjednodušší způsob, jak získat informace o přesnou verzi se má používat **Xamarin Studio** nabídce **Xamarin Studio** položku **zobrazit podrobnosti** tlačítko, kopírování a vkládání verze – informace (můžete použít **kopie informace** tlačítko).

<a name="EM0001" />

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: Nelze vytvořit výstupního adresáře. `X`

Název adresáře určeného `-o=DIR` neexistuje a nelze jej vytvořit. Může to být neplatný název pro systém souborů.

<a name="EM0002" />

### <a name="em0002-option-x-is-not-supported"></a>EM0002: Možnost `X` není podporován

Nástroj nepodporuje možnost `X`. Je možné, že jiná verze tohoto nástroje ji podporuje nebo nevztahuje v tomto prostředí.

<a name="EM0003" />

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: Platformou `X` není platný.

Nástroj nepodporuje platformu `X`. Je možné, že jiná verze tohoto nástroje ji podporuje nebo nevztahuje v tomto prostředí.

<a name="EM0004" />

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: Cíl `X` není platný.

Nástroj nepodporuje cíl `X`. Je možné, že jiná verze tohoto nástroje ji podporuje nebo nevztahuje v tomto prostředí.

<a name="EM0005" />

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: Kompilace cíl `X` není platný.

Nástroj nepodporuje cíl kompilace `X`. Je možné, že jiná verze tohoto nástroje ji podporuje nebo nevztahuje v tomto prostředí.

<a name="EM0006" />

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: Nenalezl umístění pro Xcode.

Nástroj nelze najít pomocí umístění aktuálně vybraného Xcode `xcode-select -p` příkaz. Ověřte prosím, tento příkaz úspěšné a vrátí na správné místo Xcode.

<a name="EM0007" />

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: Nelze získat verze sady sdk pro {sdk}.

Tento nástroj se nepodařilo získat SDK pomocí verze `xcrun --show-sdk-version --sdk {sdk}` příkaz. Ověřte, že tento příkaz úspěšný a vrátí verze sady SDK.

<a name="EM0008" />

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: Architektura {architektura} není platný pro {platformu}. Jsou platné architektury pro {platformu}: {architektury}.

Architektura v chybové zprávě není platný pro cílové platformy. Ověřte, že možnost--abi je předán platný architektura.

<a name="EM0009" />

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: Funkci `X` v současnosti není implementovaná generátorem

Jde o známý problém, který jsme v úmyslu opravit v budoucí verzi generátoru. Příspěvky jsou úvodní.

<a name="EM0010" />

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: Nelze sloučit architektury {simulatorFramework} a {deviceFramework}, protože soubor {soubor} existuje v obou.

Nástroj nelze sloučit architektury uvedený v chybové zprávě, protože je běžné souborů mezi nimi.

To může znamenat chyby v rozhraní .NET vložení; prosím soubor sestavy chyb v [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) s testovacího případu.

<a name="EM0011" />

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011: Sestavení `X` neexistuje.

Nástroj nelze nalézt sestavení `X` zadané v argumentech.

<a name="EM0012" />

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: Název sestavení `X` není jedinečný.

Více než jednom sestavení zadaný mají stejné, interní název a nebude možné rozlišit mezi nimi za běhu.

Nejpravděpodobnější příčinou je, že sestavení je zadán více než jednou na argumenty příkazového řádku. Nelze přejmenovat sestavení stále udržuje je původní název a více kopií ale koexistence.

<a name="EM0013" />

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013: Nelze najít sestavení "X", "Y" odkazuje.

Nástroj nelze najít sestavení "X" odkazované sestavením "Y". Zkontrolujte, zda jsou všechny odkazované sestavení ve stejném adresáři jako sestavení, které má být vázána.

<a name="EM0014" />

### <a name="em0014-could-not-find-product-product-minversion-is-required"></a>EM0014: Nelze nalézt {produktu} ({produktu} {min_version} je vyžadována).

Závislost uvedený v chybové zprávě se nenašel v systému.

<a name="EM0015" />

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-minversion-is-required"></a>EM0015: Nelze nalézt platnou verzi {produktu} ({version} nalezen, ale alespoň {min_version} je vyžadováno).

Závislost uvedený v chybě v systému byla nalezena zpráva, ale je příliš starý. Proveďte aktualizaci na novější verzi.

<a name="EM0016" />

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016: Nepodařilo se vytvořit symlink-'{souboru}' > "{cíle}": chyby {number}

Nelze vytvořit symlink uvedený v chybové zprávě.

<a name="EM0026" />

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>Argument příkazového řádku "A" EM0026 může analyzovat: *

Syntaxe pro možnost příkazového řádku zadané `A` nebylo možné analyzovat nástrojem. Je pravděpodobně nesprávná, obraťte se na dokumentaci nebo nápovědu pro správnou syntaxi.

<a name="EM0099" />

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099: Vnitřní chyba *. Prosím soubor zprávu o chybě s testovacího případu (https://github.com/mono/Embeddinator-4000/issues).

Tato chybová zpráva se hlásí, když se nezdaří Kontrola interní konzistence v rozhraní .NET vložení.

To ukazuje na chybu v rozhraní .NET vložení; prosím soubor sestavy chyb v [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) s testovacího případu.

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: Zpracování kódu

<a name="EM1010" />

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: Zadejte `T` není vygenerovat, protože `X` nejsou podporovány.

Toto je **upozornění** který typ `T` budou ignorovány (tj. nic se budou generovat) protože používá `X`, funkce, která není podporována.

Poznámka: Podporované funkce se vyvíjet s novými verzemi nástroje.

<a name="EM1011" />

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: Zadejte `T` nevygenerovala, protože jí chybí zařazování kódu pomocí nativní protějšku.

Toto je **upozornění** který typ `T` budou ignorovány (tj. nic se budou generovat) protože zpřístupňuje něco z rozhraní .NET framework, který vyžaduje velmi zařazování.

Poznámka: Toto je něco, co může získat podporována, s určitými omezeními v budoucí verzi nástroje.

<a name="EM1020" />

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: Konstruktor `C` Binder negeneruje z důvodu typ parametru `T` není podporován.

Toto je **upozornění** který konstruktoru `C` budou ignorovány (tj. nic se budou generovat) protože parametr typu `T` není podporován.

Měla by existovat starší upozornění, která poskytuje další informace, proč zadejte `T` není podporován.

Poznámka: Podporované funkce se vyvíjet s novými verzemi nástroje.

<a name="EM1021" />

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: Konstruktor `C` má výchozí hodnoty, pro které je vygenerována žádná obálku.

Toto je **upozornění** , parametry výchozí konstruktor `C` nejsou generování žádný další kód. Nejčastější příčinou je, že stávající metodu již stejným podpisem. Například v rozhraní .net je možné, že:

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

V takových případech jenom dva generované `init` selektory bude vytvořen, obě volání do Mono, ale bude existovat žádné obálku pro později.

<a name="EM1030" />

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: Metoda `M` není vygenerovat, protože návratový typ `T` není podporován.

Toto je **upozornění** , metoda `M` budou ignorovány (tj. nic se budou generovat) vzhledem k tomu, že je návratový typ `T` není podporován.

Měla by existovat starší upozornění, která poskytuje další informace, proč zadejte `T` není podporován.

Poznámka: Podporované funkce se vyvíjet s novými verzemi nástroje.

<a name="EM1031" />

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: Metoda `M` Binder negeneruje z důvodu typ parametru `T` není podporován.

Toto je **upozornění** , metoda `M` budou ignorovány (tj. nic se budou generovat) protože parametr typu `T` není podporován.

Měla by existovat starší upozornění, která poskytuje další informace, proč zadejte `T` není podporován.

Poznámka: Podporované funkce se vyvíjet s novými verzemi nástroje.

<a name="EM1032" />

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: Metoda `M` má výchozí hodnoty, pro které je vygenerována žádná obálku.

Toto je **upozornění** , výchozí parametry metody `M` nejsou generování žádný další kód. Nejčastější příčinou je, že stávající metodu již stejným podpisem. Například v rozhraní .net je možné, že:

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

V takových případech jenom dva generované `increment` selektory bude vytvořen, obě volání do Mono, ale bude existovat žádné obálku pro později.

<a name="EM1033" />

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: Metoda `M` není vygenerovat, protože jiná metoda zpřístupní operátor s popisným názvem.

Toto je **upozornění** , metoda `M` není vygenerovat, protože jiná metoda zpřístupní operátor s popisným názvem. (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034" />

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: Metody rozšíření `M` nevygenerovala uvnitř kategorii, protože nemohou být vytvářeny na primitivní typ `T`. Normální, statickou metodu byl vygenerován.

Toto je **upozornění** , zadejte na primivite metody rozšíření (například `System.Int32`) nebyl nalezen. V Objective-C není možné vytvořit na primitivní typ kategorie. Generátor místo toho bude vytvořit normální, statickou metodu.

<a name="EM1040" />

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: Vlastnost `P` Binder negeneruje z důvodu typ parametru `T` není podporován.

Toto je **upozornění** , vlastnost `P` budou ignorovány (tj. nic se budou generovat) protože zveřejněné typ `T` není podporován.

Měla by existovat starší upozornění, která poskytuje další informace, proč zadejte `T` není podporován.

Poznámka: Podporované funkce se vyvíjet s novými verzemi nástroje.

<a name="EM1041" />

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041: Indexovaných vlastností na `T` není vygenerovat, protože více indexované vlastnosti nejsou podporovány.

Toto je **upozornění** , indexované vlastnosti na `T` budou ignorovány (tj. nic se budou generovat) více indexované vlastnosti nejsou podporovány.

<a name="EM1050" />

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: Pole `F` Binder negeneruje z důvodu typ pole `T` není podporován.

Toto je **upozornění** , pole `F` budou ignorovány (tj. nic se budou generovat) protože zveřejněné typ `T` není podporován.

Měla by existovat starší upozornění, která poskytuje další informace, proč zadejte `T` není podporován.

Poznámka: Podporované funkce se vyvíjet s novými verzemi nástroje.

<a name="EM1051" />

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: Element `E` generuje se místo toho jako `F` protože jeho název je v konfliktu s důležité selektor jazyka objective-c.

Toto je **upozornění** , element `E` místo toho se budou generovat jako `F` protože jeho název je v konfliktu s důležité selektor jazyka objective-c.

Selektory na [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) mít důležité význam v objective-c a musí být přepsána pečlivě.

Poznámka: Seznam vyhrazené selektory se vyvíjet s novými verzemi nástroje.

<a name="EM1052" />

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: Element `E` nevygenerovala jeho název je v konfliktu s další prvky u stejné třídy.

Toto je **upozornění** daný Element `E` není vygenerovat, protože jeho název je v konfliktu s další prvky u stejné třídy.

<a name="EM1053" />

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: Cíl `E` není podporována pro Xamarin.iOS a Xamarin.Mac. Pouze `framework` možnost považuje za podporovány a by měl být použit.

Toto je **upozornění** cílených `E` se považuje za nepodporovaný pro Xamarin.iOS a Xamarin.Mac případy použití. 

Spotřeba statické nebo dynamické knihovny .NET vložení může vyžadovat další pracovní postup nebo vylepšení a je nutno ve většině případů použití.

Zvažte odebrání vaší `--target` , nebo parametr předejte `--target=framework` místo.

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: Generování kódu

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
