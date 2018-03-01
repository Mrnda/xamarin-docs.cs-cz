---
title: "Vložení chyby rozhraní .NET"
ms.topic: article
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 90d30b92069bcd6a5c008fa8009c0392c4d26473
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="em0xxx-binding-error-messages"></a>EM0xxx: vazba chybové zprávy

Například parametry, prostředí

<!-- 0xxx: the generator itself, e.g. parameters, environment -->
<h3><a name="EM0000"/>EM0000: Neočekávaná chyba – vyplňte sestavu chyb na https://github.com/mono/Embeddinator-4000/issues</h3>

Došlo k neočekávané chybě. Prosím [souboru problém](https://github.com/mono/Embeddinator-4000/issues) s nejblíže tolik informací, včetně:

* Úplné sestavení protokoly, s maximální podrobnosti
* Minimální testovacího případu tomu chybu reprodukovat
* Všechny verze – informace

Nejjednodušší způsob, jak získat informace o přesnou verzi se má používat **Xamarin Studio** nabídce **Xamarin Studio** položku **zobrazit podrobnosti** tlačítko, kopírování a vkládání verze – informace (můžete použít **kopie informace** tlačítko).

<h3><a name="EM0001"/>EM0001: Nelze vytvořit výstupního adresáře. `X`</h3>

Název adresáře určeného `-o=DIR` neexistuje a nelze jej vytvořit. Může to být neplatný název pro systém souborů.

<h3><a name="EM0002"/>EM0002: Možnost `X` není podporován</h3>

Nástroj nepodporuje možnost `X`. Je možné, že jiná verze tohoto nástroje ji podporuje nebo nevztahuje v tomto prostředí.

<h3><a name="EM0003"/>EM0003: Platformou `X` není platný.</h3>

Nástroj nepodporuje platformu `X`. Je možné, že jiná verze tohoto nástroje ji podporuje nebo nevztahuje v tomto prostředí.

<h3><a name="EM0004"/>EM0004: Cíl `X` není platný.</h3>

Nástroj nepodporuje cíl `X`. Je možné, že jiná verze tohoto nástroje ji podporuje nebo nevztahuje v tomto prostředí.

<h3><a name="EM0005"/>EM0005: Kompilace cíl `X` není platný.</h3>

Nástroj nepodporuje cíl kompilace `X`. Je možné, že jiná verze tohoto nástroje ji podporuje nebo nevztahuje v tomto prostředí.

<h3><a name="EM0006"/>EM0006: Nenalezl umístění pro Xcode.</h3>

Nástroj nelze najít pomocí umístění aktuálně vybraného Xcode `xcode-select -p` příkaz. Ověřte prosím, tento příkaz úspěšné a vrátí na správné místo Xcode.

<h3><a name="EM0007"/>EM0007: Nelze získat verze sady sdk pro {sdk}.</h3>

Tento nástroj se nepodařilo získat SDK pomocí verze `xcrun --show-sdk-version --sdk {sdk}` příkaz. Ověřte, že tento příkaz úspěšný a vrátí verze sady SDK.

<h3><a name="EM0008"/>EM0008: Architektura {architektura} není platný pro {platformu}. Jsou platné architektury pro {platformu}: {architektury}.</h3>

Architektura v chybové zprávě není platný pro cílové platformy. Ověřte, že možnost--abi je předán platný architektura.

<h3><a name="EM0009"/>EM0009: Funkci `X` v současnosti není implementovaná generátorem</h3>

Jde o známý problém, který jsme v úmyslu opravit v budoucí verzi generátoru. Příspěvky jsou úvodní.

<h3><a name="EM0010"/>EM0010: Nelze sloučit architektury {simulatorFramework} a {deviceFramework}, protože soubor {soubor} existuje v obou.</h3>

Nástroj nelze sloučit architektury uvedený v chybové zprávě, protože je běžné souborů mezi nimi.

To může znamenat chybu v Embeddinator-4000; prosím soubor sestavy chyb v [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues) s testovacího případu.

<h3><a name="EM0011"/>EM0011: Sestavení `X` neexistuje.</h3>

Nástroj nelze nalézt sestavení `X` zadané v argumentech.

<h3><a name="EM0012"/>EM0012: Název sestavení `X` není jedinečný.</h3>

Více než jednom sestavení zadaný mají stejné, interní název a nebude možné rozlišit mezi nimi za běhu.

Nejpravděpodobnější příčinou je, že sestavení je zadán více než jednou na argumenty příkazového řádku. Nelze přejmenovat sestavení stále udržuje je původní název a více kopií ale koexistence.

<h3><a name="EM0013"/>EM0013: Nelze najít sestavení "X", "Y" odkazuje.</h3>

Nástroj nelze najít sestavení "X" odkazované sestavením "Y". Zkontrolujte, zda jsou všechny odkazované sestavení ve stejném adresáři jako sestavení, které má být vázána.

<h3><a name="EM0014"/>EM0014: Nelze nalézt {produktu} ({produktu} {min_version} je vyžadována).</h3>

Závislost uvedený v chybové zprávě se nenašel v systému.

<h3><a name="EM0015"/>EM0015: Nelze nalézt platnou verzi {produktu} ({version} nalezen, ale alespoň {min_version} je vyžadováno).</h3>

Závislost uvedený v chybě v systému byla nalezena zpráva, ale je příliš starý. Proveďte aktualizaci na novější verzi.

<h3><a name="EM0016">EM0016: Nepodařilo se vytvořit symlink-'{souboru}' > "{cíle}": chyby {number}</h3>

Nelze vytvořit symlink uvedený v chybové zprávě.

<h3><a name="EM0026"/>Argument příkazového řádku "A" EM0026 může analyzovat: *</h3>

Syntaxe pro možnost příkazového řádku zadané `A` nebylo možné analyzovat nástrojem. Je pravděpodobně nesprávná, obraťte se na dokumentaci nebo nápovědu pro správnou syntaxi.

<h3><a name="EM0099"/>EM0099: Internal error *. Prosím soubor zprávu o chybě s testovacího případu (https://github.com/mono/Embeddinator-4000/issues).</h3>

Tato chybová zpráva se hlásí, když se nezdaří Kontrola interní konzistence v Embeddinator-4000.

To ukazuje na chybu v Embeddinator-4000; prosím soubor sestavy chyb v [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues) s testovacího případu.


<!-- 1xxx: code processing -->

# <a name="em1xxx-code-processing"></a>EM1xxx: Zpracování kódu

<h3><a name="EM1010"/>Typ `T` není vygenerovat, protože `X` nejsou podporovány.</h3>

Toto je **upozornění** který typ `T` budou ignorovány (tj. nic se budou generovat) protože používá `X`, funkce, která není podporována.

Poznámka: Podporované funkce se vyvíjet s novými verzemi nástroje.


<h3><a name="EM1011"/>Typ `T` nevygenerovala, protože jí chybí nativní protějšku.</h3>

Toto je **upozornění** který typ `T` budou ignorovány (tj. nic se budou generovat) protože ji používá zveřejněte něco z rozhraní .NET framework, který nemá protějšek v nativní platformy.


<h3><a name="EM1011"/>Typ `T` nevygenerovala, protože jí chybí zařazování kódu pomocí nativní protějšku.</h3>

Toto je **upozornění** který typ `T` budou ignorovány (tj. nic se budou generovat) protože ji používá zveřejněte něco z rozhraní .NET framework, který vyžaduje velmi zařazování.

Poznámka: Toto je objekt, který se může získat podporována s určitými omezeními v budoucí verzi nástroje.


<h3><a name="EM1020"/>Konstruktor `C` Binder negeneruje z důvodu typ parametru `T` není podporován.</h3>

Toto je **upozornění** který konstruktoru `C` budou ignorovány (tj. nic se budou generovat) protože parametr typu `T` není podporován.

Měla by existovat starší upozornění, která poskytuje další informace, proč zadejte `T` není podporován.

Poznámka: Podporované funkce se vyvíjet s novými verzemi nástroje.


<h3><a name="EM1021"/>Konstruktor `C` má výchozí hodnoty, pro které je vygenerována žádná obálku.</h3>

Toto je **upozornění** , parametry výchozí konstruktor `C` nejsou generování žádný další kód. Nejčastější příčinou je, že stávající metodu již stejným podpisem. Například v rozhraní .net je možné, že:

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

V takových případech jenom dva generované `init` selektory bude vytvořen, obě volání do mono, ale bude existovat žádné obálku pro později.


<h3><a name="EM1030"/>Metoda `M` není vygenerovat, protože návratový typ `T` není podporován.</h3>

Toto je **upozornění** , metoda `M` budou ignorovány (tj. nic se budou generovat) vzhledem k tomu, že je návratový typ `T` není podporován.

Měla by existovat starší upozornění, která poskytuje další informace, proč zadejte `T` není podporován.

Poznámka: Podporované funkce se vyvíjet s novými verzemi nástroje.


<h3><a name="EM1031"/>Metoda `M` Binder negeneruje z důvodu typ parametru `T` není podporován.</h3>

Toto je **upozornění** , metoda `M` budou ignorovány (tj. nic se budou generovat) protože parametr typu `T` není podporován.

Měla by existovat starší upozornění, která poskytuje další informace, proč zadejte `T` není podporován.

Poznámka: Podporované funkce se vyvíjet s novými verzemi nástroje.


<h3><a name="EM1032"/>Metoda `M` má výchozí hodnoty, pro které je vygenerována žádná obálku.</h3>

Toto je **upozornění** , výchozí parametry metody `M` nejsou generování žádný další kód. Nejčastější příčinou je, že stávající metodu již stejným podpisem. Například v rozhraní .net je možné, že:

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

V takových případech jenom dva generované `increment` selektory bude vytvořen, obě volání do mono, ale bude existovat žádné obálku pro později.


<h3><a name="EM1033"/>Metoda `M` není vygenerovat, protože jiná metoda zpřístupní operátor s popisným názvem.</h3>

Toto je **upozornění** , metoda `M` není vygenerovat, protože jiná metoda zpřístupní operátor s popisným názvem. (https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx)


<h3><a name="EM1034"/>Metody rozšíření `M` nevygenerovala uvnitř kategorii, protože nemohou být vytvářeny na primitivní typ `T`. Normální, statickou metodu byl vygenerován.</h3>

Toto je **upozornění** , zadejte na primivite metody rozšíření (například `System.Int32`) nebyl nalezen. V ObjC není možné vytvořit na primitivní typ kategorie. Generátor místo toho bude vytvořit normální, statickou metodu.



<h3><a name="EM1040"/>Vlastnost `P` Binder negeneruje z důvodu typ parametru `T` není podporován.</h3>

Toto je **upozornění** , vlastnost `P` budou ignorovány (tj. nic se budou generovat) protože zveřejněné typ `T` není podporován.

Měla by existovat starší upozornění, která poskytuje další informace, proč zadejte `T` není podporován.

Poznámka: Podporované funkce se vyvíjet s novými verzemi nástroje.

<h3><a name="EM1041"/>Indexované vlastnosti na `T` není vygenerovat, protože více indexované vlastnosti nejsou podporovány.</h3>

Toto je **upozornění** , indexované vlastnosti na `T` budou ignorovány (tj. nic se budou generovat) více indexované vlastnosti nejsou podporovány.


<h3><a name="EM1050"/>Pole `F` Binder negeneruje z důvodu typ pole `T` není podporován.</h3>

Toto je **upozornění** , pole `F` budou ignorovány (tj. nic se budou generovat) protože zveřejněné typ `T` není podporován.

Měla by existovat starší upozornění, která poskytuje další informace, proč zadejte `T` není podporován.

Poznámka: Podporované funkce se vyvíjet s novými verzemi nástroje.

<h3><a name="EM1051"/>Element `E` generuje se místo toho jako `F` protože jeho název je v konfliktu s důležité selektor jazyka objective-c.</h3>

Toto je **upozornění** , element `E` místo toho se budou generovat jako `F` protože jeho název je v konfliktu s důležité selektor jazyka objective-c.

Selektory na [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) mít důležité význam v objective-c a musí být přepsána pečlivě.

Poznámka: Seznam vyhrazené selektory se vyvíjet s novými verzemi nástroje.


<!-- 2xxx: code generation -->

# <a name="em2xxx-code-generation"></a>EM2xxx: Generování kódu


<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
