---
title: iOS mechanismy sestavení
description: Tato příručka popisuje postup čas aplikace a jak používat metody, které mohou být použity pro rychlejší sestavení pro všechny konfigurace sestavení.
ms.prod: xamarin
ms.assetid: 06FD3940-D666-4C9E-BC3E-BBE481EF8012
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: df84e78709b0ff16087c4bb9816c5d45f6ec33ed
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="ios-build-mechanics"></a>iOS mechanismy sestavení

_Tato příručka popisuje postup čas aplikace a jak používat metody, které mohou být použity pro rychlejší sestavení pro všechny konfigurace sestavení._

Vývoji kvalitních aplikací je více než jen psaní kódu, který funguje. Správně vytvořená aplikace by měla obsahovat optimalizace, které provést rychlejší sestavení s aplikacemi, které jsou menší a rychlejší spuštěné. Tyto optimalizace nebude mít pouze za následek lepší prostředí pro uživatele, ale také pro vás nebo všechny vývojář pracující na projekt. Je nutné zajistit, že při plánování práce s vaší aplikací všechno, co je vypršel časový limit často. 

Mějte na paměti, že jsou bezpečné a rychlé výchozí možnosti, ale nejsou optimální pro každé situaci. Kromě toho mnoho možností můžete buď zpomalit nebo zrychlení cyklu vývoje v závislosti na jednotlivých projektů. Například nativní odstraňování trvá určitou dobu, ale pokud je minimální velikost získávají pak čas strávený odstraňování se nevyžaduje podle rychlejší nasazení. Na druhé straně nativní odstraňování můžete zmenšit aplikace výrazně, v takovém případě bude rychlejší nasazení. To se liší mezi projekty a jediný způsob, jak vědět, je otestovat.

Rychlost sestavení Xamarin můžou mít vliv také různé kapacity a možnosti počítače, než může ovlivnit výkon: funkce procesoru, rychlosti sběrnice, množství fyzické paměti, rychlosti sítě a rychlost disku. Tato omezení výkonu jsou nad rámec tohoto dokumentu a je zodpovědností vývojář.


## <a name="timing-apps"></a>Aplikace časování

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud chcete povolit výstup diagnostiky MSBuild v sadě Visual Studio pro Mac:

1. Klikněte na tlačítko **Visual Studio pro Mac > Předvolby...**
2. V levém stromové zobrazení, vyberte **projekty > sestavení**
3. V pravém panelu nastavení podrobností protokolu rozevírací seznam pro **diagnostiky**: [ ![ ] (ios-build-mechanics-images/image2.png "nastavení podrobností protokolu")](ios-build-mechanics-images/image2.png#lightbox)
4. Klikněte na tlačítko **OK**
5. Restartujte Visual Studio pro Mac
6. Vyčistěte a sestavte svůj balíček znovu
7. Zobrazit výstup diagnostiky v rámci Pad chyby (Zobrazení > dotyková zařízení > chyby) kliknutím na tlačítko sestavení výstupu


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chcete-li povolit výstup diagnostiky MSBuild v sadě Visual Studio:

1. Klikněte na tlačítko **nástroje > Možnosti...**
2. V levém stromové zobrazení, vyberte **projekty a řešení > sestavit a spustit**
3. V pravém panelu nastavit *výstup nástroje MSBuild sestavení podrobností rozevírací* k **diagnostiky**: [ ![ ] (ios-build-mechanics-images/image2-vs.png "nastavení MSBuild sestavení výstupu podrobností")](ios-build-mechanics-images/image2-vs.png#lightbox)
4. Klikněte na tlačítko **OK**
5. Vyčistěte a sestavte svůj balíček znovu.
6. Výstup diagnostiky se zobrazí na panelu Výstup.

-----

## <a name="timing-mtouch"></a>Mtouch časování

Chcete-li zobrazit informace specifické pro proces mtouch sestavení, předat `--time --time` k mtouch argumentů ve vaší **možnosti projektu**. Výsledky se nacházejí ve výstupu sestavení tak, že `MTouch` úloh:

```csharp
Setup: 36 ms
Resolve References: 982 ms
Extracted native link info: 987 ms
...
Total time: 1554 ms
```

## <a name="connecting-from-visual-studio-with-build-host"></a>Připojení ze sady Visual Studio s hostitelem sestavení

Xamarin nástrojů technicky funguje na všech Mac, který můžete spustit OS X 10.10 Yosemite nebo novější. Ale vývojáře prostředí a časy sestavení může znesnadňovat výkon Mac.

V odpojeném stavu, Visual Studio v systému Windows pouze provede fázi kompilace jazyka C# a nebude pokoušet provést propojení nebo kompilace AOT, balíček aplikace do _.app_ sady nebo přihlašovací sady prostředků aplikace. (Fázi kompilace jazyka C# je zřídka kritických bodů výkonu.) Pokus o přesně určit, kde v kanálu sestavení je zpomalení podle budovy přímo na hostiteli Mac sestavení v sadě Visual Studio for Mac.


Kromě toho je jedno z míst častější pro sluggishness síťové připojení mezi počítači Windows a Mac sestavení hostitele. Může to být způsobeno fyzické překážka v síti pomocí bezdrátového připojení nebo obcházet prostřednictvím nasycených počítače (například Mac-in-the-cloud service).

## <a name="simulator-tricks"></a>Simulátor triky

 
Při vývoji mobilních aplikací, je nezbytné pro rychlé nasazení kódu. Pro z mnoha důvodů včetně rychlosti a chybějících zřizování požadavky vývojáři často zvolit pro nasazení předem nainstalovaná simulátoru nebo emulátor. U výrobců nástroje pro vývojáře rozhodnutí ohledně poskytuje simulátoru nebo emulátoru obsahuje kompromis mezi rychlostí a kompatibility. 

Apple poskytuje simulátor pro vývoj pro iOS, povýšení rychlost přes kompatibility vytvořením méně omezující prostředí pro spuštění kódu. Tento méně omezující prostředí umožňuje Xamarin používat jenom v čas (kompilátoru za běhu) pro simulátoru (Naproti tomu [AOT](~/ios/internals/architecture.md) na zařízení), což znamená, že je sestavení kompilováno do nativního kódu v době běhu. Mac je mnohem rychlejší než zařízení, to umožňuje lepší výkon.

Simulátor používá Spouštěč sdílené aplikace, což spouštěč znovu použije, a budou vytvářeny pokaždé, když, jako je potřeba zadat v zařízení.

Při vezme v úvahu výše uvedené informace, nabízí níže uvedeného seznamu některé informace o postupu při vytváření a nasazování aplikaci v simulátoru zajistit optimální výkon.
 
### <a name="tips"></a>Tipy

- Pro sestavení: 
  - Zrušte výběr **optimalizovat PNG image** možnost v možnosti projektu. Tato optimalizace je nezbytné pro sestavení v simulátoru.
  - Nastavte linkeru na **není odkaz**. Zakázání linkeru je rychlejší, protože její provedení trvá dlouhou dobu.
  - Zakázání spuštění sdílené aplikace pomocí `--nofastsim` příznak způsobí, že sestavení simulátoru být výrazně zpomalit. Odeberte tento příznak, pokud se už nevyžaduje.
  - Použití nativních knihoven je pomalejší, protože hlavní spustitelný soubor sdílený simlauncher nemůže znovu v takových případech a specifické pro aplikaci spustitelný soubor musí být zkompilovány pro každé sestavení.
- Pro nasazení
  - Vždy mít simulátoru spuštěn, když je to možné. Může trvat až 12 sekund studený start simulátoru.
- Další tipy
  - Přednost sestavení přes opětovné sestavení, protože opětovné sestavení vyčistí před vytvořením. Čištění může trvat dlouhou dobu, protože se odebere odkazy, které by bylo možné použít.
  - Výhodou fakt, že simulátoru nevynucuje izolovanému prostoru. Pokaždé, když je aplikace spuštěná v simulátoru, který velké prostředků, jako jsou videa nebo dalších prostředků zahrnut do projektu můžete vytvořit nákladná kopírování souborů. Tyto nákladná operace vyhnout tím, že tyto soubory na domovský adresář a odkazujte na ně ve vaší aplikaci pomocí úplnou cestou k souboru.  
  - Pokud máte pochybnosti, použijte `–time –time` příznak k měření změny

Následující snímek obrazovky ukazuje, jak nastavit tyto možnosti simulátoru v možnostech iOS:

[![](ios-build-mechanics-images/image3.png "Nastavení možností")](ios-build-mechanics-images/image3.png#lightbox)

## <a name="device-tricks"></a>Zařízení triky

Nasazení do zařízení je podobná nasazení do simulátoru, jako je simulátoru malou podmnožinu sestavení používá pro zařízení s iOS. Vytváření pro zařízení vyžaduje mnoho další kroky, ale nabízí výhodu v podobě poskytovat další možnosti optimalizace vaší aplikace.

### <a name="build-configurations"></a>Konfigurace sestavení

Existuje několik konfigurací sestavení poskytnutý při nasazování aplikací pro iOS. Je důležité mít dostatečné povědomí o každé konfiguraci, potřebujete vědět, kdy a proč je optimalizace.

 - Ladit
  - Toto je hlavní konfigurace, který by měl použít, pokud je aplikace ve vývoji a má, proto, být jako rychlé nejblíže.
 - Vydaná verze
  - Verzi sestavení jsou ty, které jsou sice pro vaše uživatele a je prvořadá zaměřená na výkon. Pokud používáte verzi konfigurace, můžete chtít použít optimalizace kompilátoru LLVM a optimalizovat soubory PNG.

 
Je také důležité pochopit vztah mezi vytváření a nasazování. Čas nasazení je funkce, velikosti aplikace. Větší aplikace trvá delší dobu nasadit. Minimalizace velikost aplikace, můžete snížit čas nasazení.

Minimalizace velikost aplikace může také snížit čase vytvoření buildu. To je proto odebrání kód z aplikace zabere to méně času než nativně kompilování kódu, která nebude použita. Menší soubory objektů znamená rychlejší propojování, která vytvoří menší spustitelný soubor s méně symboly pro generování. Ukládání místo, proto má dvojitý návratnost, což je důvod, proč **odkaz SDK** je sestavení na výchozí hodnoty pro všechna zařízení. 

> [!NOTE]
> **Odkaz SDK** možnost se může zobrazit pouze pouze odkaz Framework sady SDK nebo sady SDK odkaz sestavení v závislosti na prostředí IDE, který se používá.
 

### <a name="tips"></a>Tipy

- Build: 
  - Vytvoření jednoho architekturu (například ARM64) je rychlejší než FAT binární (např. ARMv7 + ARM64)
  - Vyhněte se optimalizace soubory PNG při ladění
  - Vezměte v úvahu propojení ve všech sestaveních. Optimalizace všechna sestavení 
  - Zakázání vytváření symboly ladění pomocí `--dsym=false`. Nicméně byste měli vědět, že zakázání to bude znamenat, hlášení selhání může být symbolicated pouze na tomto počítači, který vytvořené aplikace a pouze v případě, že aplikace nebyla odstraní.

 
Některé kroky, které je nutno jsou:

- Binárních souborů FAT (ladění) 
- Zakázat linkeru `–nolink` 
- Zakázání odstraňování 
  - Symboly `--nosymbolstrip` 
  - IL (verze) `--nostrip`.  
 
Další tipy 

- Jako v simulátoru, raději sestavení přes opětovné sestavení 
  - AOT by se do mezipaměti sestavení (soubory objektů) 
- Ladicí sestavení trvat déle kvůli symboly, systémem dsymutil a vzhledem k tomu, že se bude nakonec je větší, navíc čas k nahrání do zařízení. 
- Ve výchozím nastavení, provede verze sestavení IL pruhu sestavení. Který trvá jenom kousek čas a je pravděpodobně získaných zpět v případě, že nasazení menší .app do zařízení.
- Vyhněte se nasazení velkých statických souborů v každé sestavení (ladění) 
  - Použití UIFileSharingEnabled (info.plist) 
    - Prostředky je možné odeslat jednou 
- Pokud máte pochybnosti, použijte `–time –time` příznak k měření změny

Následující snímek obrazovky ukazuje, jak nastavit tyto možnosti simulátoru v možnostech iOS:

[![](ios-build-mechanics-images/image4.png "Nastavení možností")](ios-build-mechanics-images/image4.png#lightbox)

## <a name="using-the-linker"></a>Pomocí Linkeru

Při vytváření aplikace, používá mtouch linkeru pro spravovaný kód, který odebere kód, který nepoužívá aplikaci. Teoreticky výsledkem je menší a proto rychlejší sestavení. Další informace o linkeru najdete v části [propojení na iOS](~/ios/deploy-test/linker.md) průvodce.

Při použití Linkeru, zvažte následující možnosti:

- Výběr **není odkaz** pro zařízení sestavení trvá větší množství času a také vytváří větší aplikace. 
  - Apple odmítnou aplikace, pokud jsou větší než omezení velikosti. Závisí na `MinimumOSVersion`, může to být malá jako 60 MB. 
  - Nativní spustitelný soubor je nebudou zahrnuty. 
  - Pomocí není odkaz je rychlejší simulátoru sestavení, protože se používá JIT – kompilace (na rozdíl od AOT na zařízení).
- Odkaz SDK je výchozí možnost.
- Odkaz všechny nemusí být bezpečný chcete použít, zvlášť pokud používáte kód tedy ne vlastní takové NuGets nebo součástí. Pokud se rozhodnete odkaz sestavení jsou součástí vaší aplikace, potenciálně vytváření aplikací pro větší všechny kód z těchto služeb. 
  - Ale pokud se rozhodnete **odkaz všechny** aplikace může dojít k chybě, zvlášť pokud externí součásti jsou používány. Toto je z důvodu některé součásti na určitých typů pomocí reflexe.
  - Statické analýzy a reflexe nefungují společně. 

Nástroje může být nastavena, aby věcí uvnitř aplikace pomocí [ `[Preserve]` atribut](~/ios/deploy-test/linker.md). 

Pokud nemáte přístup ke zdrojovému kódu, nebo je generován nástroj a nechcete ho změnit, může stále propojený tak, že vytvoříte soubor XML, který popisuje všechny typy a členy, které je třeba možné zachovat. Poté můžete přidat příznak `--xml={file.name}.xml` na možnosti projektu, který zpracovat kód přesně, jako kdyby byly pomocí atributů.


### <a name="partially-linking-applications"></a>Částečně propojení aplikace 

Je také možné částečně propojení aplikace, které vám pomůžou optimalizovat okamžiku sestavení vaší aplikace:

- Použití `Link All` a přeskočit některá sestavení 
  - Některé optimalizaci velikosti aplikace bude ztracena.
  - Je vyžadován žádný přístup ke zdrojovému kódu.
  - Například `--linkall --linkskip=fieldserviceiOS` .
 
- Použít `Link SDK` a pomocí `[LinkerSafe]` atribut sestavení potřebujete 
  - Přístup ke zdrojovému kódu vyžaduje.
  - Říká systému, zda je sestavení bezpečné odkaz, jako kdyby byl Xamarin SDK, jsou zpracovávány.
 
### <a name="objective-c-bindings"></a>Objective-C Bindings 

- Pomocí `[Assembly: LinkerSafe]` atribut vazby můžete ušetřit čas a velikost.

- SmartLink 
  - Provádí na nativní straně 
  - Použití `[LinkWith (SmartLink=true)]` atribut
  - To pomáhá eliminovat nativního kódu z knihovny, které se připojujete proti nativní linkeru. 
  - Poznámka: Tento dynamické vyhledávání symbolů nebude fungovat se to. 

## <a name="summary"></a>Souhrn

Tato příručka prozkoumali postup časem aplikace pro iOS a možnosti ke zvážení, které jsou závislé na konfigurace sestavení projektu a možnosti. 

<!-----
# Benchmarks

## Layer 1: building again after making modifications, but _without_ cleaning should be faster 
 
The app should build a bit more quickly if you have only made changes to a subset of the libraries and you do not clean the build before re-deploying. 
 
 
 
### Clean build time 
178 seconds 
 
 
### Build again (without cleaning) after making _no changes_ 
12.5 seconds 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
3 trials: 45 seconds, 43 seconds, 43 seconds 
 
 
### Build again (without cleaning) after changing 1 line in each of the following files 
 
- ViewIOS/ImageResourcesHelper.cs 
- Sales.Native.Core.Tools/UIComponents/ListView/IListView.cs 
- View.Models/Mailing/MailingModel.cs 
 
3 trials: 45 seconds, 45 seconds, 45 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in each of the following files 
 
- ViewIOS/ImageResourcesHelper.cs 
- Sales.Native.Core.Tools/UIComponents/ListView/IListView.cs 
- View.Models/Mailing/MailingModel.cs 
- Sales.Native.Core.IOS.Ext/ServiceInterfaces/AlertDialog/Dialog.cs 
- Sales.Native.Core.Tools.IOS.Ext/BaseViews/BaseNavigationViewController.cs 
- View.Common/Services/DataTransferResult.cs 
 
45 seconds 
 
 
 
 
 
 
## Layer 2: "app thinning" aka "device specific builds" 
 
The idea of "app thinning" is that the IDE will only build the 1 architecture needed for the specific device that you're deploying to (rather than _both_ 32-bit and 64-bit architectures). 
 
As of the latest "Xamarin 4" builds, you can now enable "app thinning" in Visual Studio via the "Project Options -> iOS Build -> Enable device-specific builds" setting. 
 
Or if you prefer you can achieve a similar result by changing the "Project Options -> iOS Build -> Advanced [tab] -> Supported architectures" to select just _one_ architecture (for example ARM64 if you are developing on a 64-bit device). 
 
 
 
(Caveat: I ran the following builds in Visual Studio for Mac on the Mac rather than on the command line.) 
 
### Clean build time without "device specific builds" 
177 seconds 
 
 
 
### Clean build time _with_ "device specific builds"  
2 trials: 106 seconds, 98 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
2 trials: 31 seconds, 31 seconds 
 
 
* * * 
 
 
## Using the same strategy, but explicitly setting "Supported architectures" to select ARM64 _only_ (rather than using "device specific builds") 
 
(These builds were again run on the command line using `xbuild`.) 
 
 
 
### Clean build time with "Supported architectures" set to ARM64 _only_ 
2 trials: 80 seconds, 91 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
2 trials: 26 seconds, 26 seconds 
 
 
 
 
 
[1] Mac system used for testing: MacBookAir5,2 
 
- 2.0 GHz Core i7 (I7-3667U) 
 
2 Cores with hyper-threading 
 
L2 Cache (per Core): 256 KB 
L3 Cache: 4 MB 
 
- Standard MacBook soldered-in solid-state storage 
 
- 8 GB RAM 
---->


## <a name="related-links"></a>Související odkazy

- [Příspěvek blogu](https://blog.xamarin.com/xamarin-ios-build-improvements/)
- [Propojení v systému iOS](~/ios/deploy-test/linker.md)
- [Vlastní konfigurace linkeru](~/cross-platform/deploy-test/linker.md)
