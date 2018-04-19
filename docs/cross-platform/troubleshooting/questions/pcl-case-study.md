---
title: Vyřešte problémy související s System.Diagnostics.Tracing a toku dat TPL
description: 'Případová studie PCL: jak můžete vyřešit problémy související s System.Diagnostics.Tracing pro balíček NuGet toku dat TPL Microsoft?'
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.technology: xamarin-cross-platform
ms.date: 04/17/2018
author: asb3993
ms.author: amburns
ms.openlocfilehash: b1b56b0e831edbb6327f3ca66f6ec8dc780b46f2
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>Případová studie PCL: jak můžete vyřešit problémy související s System.Diagnostics.Tracing pro balíček NuGet toku dat TPL Microsoft?

> [!IMPORTANT]
> Tento příklad konkrétní `System.Diagnostic.Tracing` již vytváří případné chyby v nejnovější verze Xamarin ve výchozím nastavení. Při řešení navrhované budou i nadále fungovat, Všimněte si, že některé z chyb uvedených v části "vrstev chyby" byl opraven.
> Kromě toho si všimněte, že .NET Standard je nyní upřednostňovaný způsob implementace rozhraní API pro různé platformy .NET.

## <a name="summary"></a>Souhrn

Xamarin.iOS a Xamarin.Android neimplementují 100 % každých PCL profilu, který povolí jako odkazy. Pro praktické usnadnění práce v sadě Visual Studio pro Mac, Visual Studio a Správce balíčků NuGet projekty Xamarin umožňuje použít několik profilů, které mají pouze _neúplné_ implementace. Například Xamarin.iOS ani Xamarin.Android aktuálně obsahuje kompletní implementace typů v "System.Diagnostics.Tracing" PCL oboru názvů. Toto omezení vede k tři vrstvy chyb při použití výchozí `portable-net45+win8+wpa81` verze balíčku NuGet toku dat TPL Microsoft.

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>Alternativní řešení: Přepínač projekt aplikace tak, aby odkazovaly `portable-net45+win8+wp8+wpa81` verzi knihovny toku dat TPL

(To zabraňuje všechny tři vrstvy chyb a platí pro všechny poslední verze Xamarin.)

1. Otevřete projekt aplikace **.csproj** soubor v textovém editoru.

2. Najděte řádek, který vypadá podobně jako tento:

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. Změna `portable-net45+win8+wpa81` k `portable-net45+win8+wp8+wpa81` (`+wp8` je přidána):

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>Vysvětlení

`portable-net45+win8+wp8+wpa81` Verzi knihovny neodkazuje **System.Diagnostics.Tracing.dll** _vůbec_, takže ho úplně zabraňuje všechny tři vrstvy problémy.

### <a name="limitations"></a>Omezení

- `portable-net45+win8+wp8+wpa81` Verzi knihovny nemusí zahrnovat 100 % funkcí `portable-net45+win8+wpa81` verze.

- Nainstaluje Správce balíčků NuGet `portable-net45+win8+wpa81` verze balíčku PCL NuGet ve výchozím nastavení, takže je nutné ručně upravit odkaz.

## <a name="details-about-the-three-layers-of-errors"></a>Podrobnosti o tři vrstvy chyb

1. **System.Diagnostics.Tracing.dll** právě průčelí za sestavení chybí z všechny verze Mac Xamarin.Android (neveřejný chyb 34888) a chybí ze všech Xamarin.iOS verze nižší než 9.0 (nebo nižší než XamarinVS 3.11.1443 v systému Windows) (pevná ve [chyb 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388)). Tento problém jednu z těchto chyb v závislosti na cíl nasazení a linkeru způsobí nastavení:

    - Xamarin.Android.Common.targets: Chyba: výjimka při načítání sestavení: System.IO.FileNotFoundException: nebylo možné načíst sestavení ' System.Diagnostics.Tracing, verze = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a'. Možná neexistuje mono Android profilu?

    - Nepodařilo se načíst soubor nebo sestavení 'System.Diagnostics.Tracing nebo jednu ze závislostí. Systém nemůže najít zadaný soubor. (System.IO.FileNotFoundException)

    - MTOUCH: Chyba MT3001: může AOT sestavení ' / Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll.

    - MTOUCH: Chyba MT2002: se nepovedlo přeložit sestavení: ' System.Diagnostics.Tracing, verze = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a.

2. Aktuální [Mono implementace typů v "System.Diagnostics.Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) chybí některé přetížení metody ([chyb 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Tento problém způsobí jednu z těchto chyb linkeru při vytváření aplikace Xamarin:

    - / Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: Chyba: Chyba provádění úkolů LinkAssemblies: Chyba XA2006: odkaz na položka metadat ' System.Void System.Diagnostics.Tracing.EventSource:: WriteEvent(System.Int32,System.Object[])' (definované v ' System.Threading.Tasks.Dataflow, verze 4.5.24.0, Culture = = neutral, PublicKeyToken = b03f5f7f11d50a3a') z ' System.Threading.Tasks.Dataflow, verze = 4.5.24.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a' nebylo možné přeložit.

    - MTOUCH: Chyba MT2002: Nepodařilo se vyřešit "System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])" odkaz z "System.Diagnostics.Tracing, verze = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a"

3. Aktuální [Mono implementace typů v "System.Diagnostics.Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) aktuálně je také _prázdný_ "fiktivní" implementace ([chyb 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)). Všechny pokusy o použití těchto metod v době běhu proto může vést k neočekávaným výsledkům. Pro _konkrétní_ případ knihovna toku dat TPL Microsoft se zdá být volání `WriteEvent(System.Int32,System.Object[])` nejsou nezbytně nutné pro většinu chování této knihovny, proto oprava "layer 2" ([chyb 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337), přidání prázdný implementace) bude pravděpodobně dostatečné pro většinu případy použití toku dat TPL společnosti Microsoft.

## <a name="questions--answers"></a>Otázky a odpovědi

### <a name="i-was-able-to-leave-linking-enabled-with-the-codeportable-net45win8wpa81code-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>Bylo možné k nechte propojení s povolenou <code>portable-net45+win8+wpa81</code> verzi knihovny na starších verzích Xamarin.iOS nebo Xamarin.Android. Jak který pracoval?

#### <a name="answer"></a>Odpověď

Je _možné_ k získání sestavení "úspěšně" (s propojení povoleno) ve starších verzích Xamarin.iOS nebo v Xamarin.Android v systému Mac Pokud obsahovat odkaz na `System.Diagnostics.Tracing.dll` _referenční sestavení_ \[1\] místo _průčelí za sestavení_ \[2], ale bohužel není to "Opravit" alternativní řešení. Referenční sestavení jsou určeny pouze pro použití při vytváření _přenosné knihovny_, kód není specifických pro platformy, jako jsou aplikace. Probíhá pokus o _spustit_ kód obsažené v referenční sestavení (nikoli pouze sestavení u ní) je pravděpodobně vést k neočekávaným výsledkům. Správná oprava bude pro Mono tým přidat chybějící `WriteEvent(System.Int32,System.Object[])` přetížení k [ `EventSource` ](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) typu ([chyb 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Nyní je nejlepší možnost přepnout do `portable-net45+win8+wp8+wpa81` verzi knihovny toku dat TPL Microsoft, jak je popsáno v části řešení výše.

(Pro každý, kdo může po prohlédnutí tohoto související starší, briefer odpovědí z StackOverflow přečtení tohoto článku (<http://stackoverflow.com/a/23591322/2561894>), Všimněte si, že byl rozdíl mezi referenční sestavení a sestavení průčelí za _není_ uvedených existuje.)

**\[1\] "Referenční sestavení" umístění**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] umístění "Facade sestavení"**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>Pomůže ho Pokud I ručně přidejte odkaz na sestavení průčelí za "System.Diagnostics.Tracing"?

_Konkrétně může vyřešit problém s tímto postupem 2?_

1. _Kopírování `System.Diagnostics.Tracing.dll` průčelí za sestavení do složky projektu aplikace z jednoho z následujících umístění:_

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Přidáte odkaz na sestavení průčelí za v projektu aplikace Xamarin.iOS nebo Xamarin.Android._

#### <a name="answer"></a>Odpověď

Ne, nebude moci.

- Pro Xamarin.iOS 9.0 nebo všechny nejnovější verzi Xamarin.Android v systému Windows toto řešení je výhradně redundantní a může způsobit chyby při kompilaci podobná "sestavení 'System.Diagnostics.Tracing' se stejnou identitou již byla naimportována.".

- Pro Xamarin.iOS 8.10 nebo nižší, nebo pro Xamarin.Android v systému Mac, pomůže toto řešení ale _pouze_ pro sestavení problém chybějící "vrstvy 1". Zruší _není_ vyřešit chybami linkeru "vrstvy 2", takže není kompletního řešení.

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>Je možné používat [balíček System.Diagnostics.Tracing NuGet](https://www.nuget.org/packages/System.Diagnostics.Tracing/) problém vyřešit?

#### <a name="answer"></a>Odpověď

Balíček NuGet 3.0 "System.Diagnostics.Tracing" Ne, obsahuje pouze implementace specifické pro platformu pro "DNXCore50" a "netcore50". Ji explicitně _vynechá_ implementace pro Xamarin.Android ("MonoAndroid") a Xamarin.iOS ("MonoTouch" a "xamarinios"). To znamená, že instalace balíčku _neplatí_ pro Xamarin.Android a Xamarin.iOS projekty. Balíček NuGet se předpokládá, že oba tyto platformy poskytují jejich _vlastní_ implementace typů. Tento předpoklad je "Opravit" v tom smyslu, že máte _Mono_ implementace oboru názvů, ale jako popsaných v části body \#2 a \#3 "Podrobnosti o 3 vrstvy chyby" nad implementace není aktuálně úplná. A tak správné opravu pro Mono tým vyřešit [chyb 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337) a [chyb 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890).

## <a name="next-steps"></a>Další kroky

O další pomoc, kontaktujte nás, nebo pokud tento problém zůstane i po použití výše uvedené informace najdete v tématu [jaké možnosti podpory jsou dostupná pro Xamarin?](~/cross-platform/troubleshooting/support-options.md) informace o možnostech kontaktní, návrhy, a také jak soubor nové chyby v případě potřeby.
