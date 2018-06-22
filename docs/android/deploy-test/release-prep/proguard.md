---
title: ProGuard
description: ProGuard je knihovnu shrinker souboru Třída Java, optimalizátor, obfuscator a předběžné ověřovatele. Rozpozná a odebere nepoužívané kódu, analyzuje a optimalizuje bajtového kódu potom zastírá třídy a jejich členové. Tato příručka vysvětluje, jak funguje ProGuard, jak povolit ve vašem projektu a jeho konfiguraci. Je také několik příkladů ProGuard konfigurace.
ms.prod: xamarin
ms.assetid: 29C0E850-3A49-4618-9078-D59BE0284D5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: e65c78633ae91318bd8e9cce949bac9cc12675c0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771441"
---
# <a name="proguard"></a>ProGuard

_ProGuard je knihovnu shrinker souboru Třída Java, optimalizátor, obfuscator a předběžné ověřovatele. Rozpozná a odebere nepoužívané kódu, analyzuje a optimalizuje bajtového kódu potom zastírá třídy a jejich členové. Tato příručka vysvětluje, jak funguje ProGuard, jak povolit ve vašem projektu a jeho konfiguraci. Je také několik příkladů ProGuard konfigurace._


## <a name="overview"></a>Přehled

ProGuard rozpozná a odebere nepoužívané třídy, pole, metod a atributy ze zabalené aplikace. I ho můžete provést stejný pro odkazované knihovny (to vám může pomoct vyhnout limit pro odkaz na 64 kB). ProGuard nástroj ze sady Android SDK bude také optimalizovat bajtového kódu, odeberte nepoužívané kód pokyny a obfuskováním zbývající třídy, pole a metody s kratší názvy. ProGuard čtení **vstupní JAR** zmenšuje, optimalizuje, zastírá a předem ověří jejich; zapíše výsledky na jeden nebo více **výstup JAR**. 

ProGuard procesy vstup na APK pomocí následujících kroků: 

1.  **Zmenšení krok** &ndash; ProGuard rekurzivně Určuje, které třídy a jejich členové používají. Všechny ostatní třídy a jejich členové jsou zahozeny. 

2.  **Optimalizace krok** &ndash; ProGuard další optimalizuje kód. 
    Mezi další optimalizace, třídy a metody, které nejsou, že vstupní body můžete provedeny, privátní, statické nebo konečné lze odebrat nepoužité parametry a některé metody mohou být vložená. 

3.  **Krok maskováním** &ndash; ProGuard přejmenuje třídy a členy třídy, které nejsou vstupní body. Zachování vstupní body zajišťuje, lze přesto k nim jejich původní názvy. 

4.  **Preverification krok** &ndash; nekontroluje Java bytecodes před runtime a označí soubory třídy ve prospěch virtuálního počítače Java. Toto je jediný krok, který nemá vědět, vstupní body. 

Každý z těchto kroků je *volitelné*. Jak bude vysvětleno v další části, používá Xamarin.Android ProGuard pouze podmnožinu těchto kroků. 



## <a name="proguard-in-xamarinandroid"></a>ProGuard v Xamarin.Android

Xamarin.Android ProGuard konfigurace není obfuskováním APK. Ve skutečnosti není možné povolit maskováním prostřednictvím ProGuard (i prostřednictvím vlastní konfigurační soubory). Proto je Xamarin.Android ProGuard provádí pouze **zmenšení** a **optimalizace** kroky: 

[![Postup zmenšení a optimalizace](proguard-images/01-xa-chain-sml.png)](proguard-images/01-xa-chain.png#lightbox)

Jednu položku důležité předem znát před použitím ProGuard je, jak to funguje v rámci `Xamarin.Android` proces sestavení. Tento proces používá dva samostatné kroky: 

1.  Xamarin Android Linker

2.  ProGuard

Každý z těchto kroků je popsána dále.



### <a name="linker-step"></a>Krok linkeru

Linkeru Xamarin.Android využívá statické analýzu aplikace určit následující: 

-   Sestavení, ve skutečnosti se používají.

-   Typy, které se používají ve skutečnosti.

-   Členy, které se používají ve skutečnosti. 

Linkeru se vždy spustit před spuštěním kroku ProGuard. Z toho důvodu může linkeru pruhu sestavení nebo typ/člen, který by se dalo očekávat ProGuard ke spuštění na. (Další informace o propojení v Xamarin.Android najdete v tématu [propojení v systému Android](~/android/deploy-test/linker.md).)



### <a name="proguard-step"></a>ProGuard krok

Po úspěšném dokončení kroku linkeru ProGuard běží odebrat nepoužité bajtového kódu Java. Toto je krok, který optimalizuje APK. 



## <a name="using-proguard"></a>Pomocí ProGuard

Pokud chcete používat ProGuard v projektu aplikace, musíte nejdřív povolit ProGuard. Dále můžete buď nechat Xamarin.Android sestavení pomocí procesu výchozí ProGuard konfigurační soubor, nebo můžete vytvořit svůj vlastní soubor vlastní konfigurace pro ProGuard používat. 



### <a name="enabling-proguard"></a>Povolení ProGuard

Použijte následující postup pro povolení ProGuard v projektu aplikace:

1.  Ujistěte se, že váš projekt je nastavená na **verze** konfigurace (to je důležité, protože linkeru musí běžet v pořadí pro ProGuard ke spuštění): 

    [![Vyberte možnost konfigurace verze](proguard-images/02-set-release-sml.png)](proguard-images/02-set-release.png#lightbox)
   
2.  Povolit ProGuard kontrolou **povolit ProGuard** možnost pod **balení** kartě **vlastnosti > Android možnosti**: 

    [![Povolit Proguard možnost](proguard-images/03-enable-proguard-sml.png)](proguard-images/03-enable-proguard.png#lightbox)

Pro většinu aplikací Xamarin.Android, bude výchozí ProGuard konfigurační soubor poskytl Xamarin.Android dostatečná k odebrání všech (a pouze) nepoužívané kódu. Pokud chcete zobrazit výchozí konfigurace ProGuard, otevřete soubor v **obj\\verze\\proguard\\proguard_xamarin.cfg**. Další část popisuje, jak vytvořit vlastní ProGuard konfigurační soubor. 



### <a name="customizing-proguard"></a>Přizpůsobení ProGuard

Volitelně můžete přidat vlastní ProGuard konfiguračního souboru získat větší kontrolu nad ProGuard nástrojů. Můžete například explicitně říct ProGuard třídy, které chcete zachovat. K tomuto účelu vytvořte nový **.cfg** souboru a použít `ProGuardConfiguration` akce sestavení **vlastnosti** podokně **Průzkumníku řešení**: 

[![Vybrané akce ProguardConfiguration sestavení](proguard-images/04-build-action-sml.png)](proguard-images/04-build-action.png#lightbox)

Mějte na paměti, že tento konfigurační soubor nenahrazuje Xamarin.Android **proguard_xamarin.cfg** souboru vzhledem k tomu, jak jsou používány ProGuard. 

Následující příklad ilustruje soubor typické ProGuard konfigurace:
    

    # This is Xamarin-specific (and enhanced) configuration.

    -dontobfuscate

    -keep class mono.MonoRuntimeProvider { *; <init>(...); }
    -keep class mono.MonoPackageManager { *; <init>(...); }
    -keep class mono.MonoPackageManager_Resources { *; <init>(...); }
    -keep class mono.android.** { *; <init>(...); }
    -keep class mono.java.** { *; <init>(...); }
    -keep class mono.javax.** { *; <init>(...); }
    -keep class opentk.platform.android.AndroidGameView { *; <init>(...); }
    -keep class opentk.GameViewBase { *; <init>(...); }
    -keep class opentk_1_0.platform.android.AndroidGameView { *; <init>(...); }
    -keep class opentk_1_0.GameViewBase { *; <init>(...); }

    -keep class android.runtime.** { <init>(***); }
    -keep class assembly_mono_android.android.runtime.** { <init>(***); }
    # hash for android.runtime and assembly_mono_android.android.runtime.
    -keep class md52ce486a14f4bcd95899665e9d932190b.** { *; <init>(...); }
    -keepclassmembers class md52ce486a14f4bcd95899665e9d932190b.** { *; <init>(...); }

    # Android's template misses fluent setters...
    -keepclassmembers class * extends android.view.View {
       *** set*(***);
    }

    # also misses those inflated custom layout stuff from xml...
    -keepclassmembers class * extends android.view.View {
       <init>(android.content.Context,android.util.AttributeSet);
       <init>(android.content.Context,android.util.AttributeSet,int);
    }
    

Můžou nastat případy, kdy ProGuard nelze správně analyzovat vaše aplikace; potenciálně jej může odebrat kód, který je ve skutečnosti potřebuje vaše aplikace. Pokud k tomu dojde, můžete přidat `-keep` řádku vlastní ProGuard konfigurační soubor: 

    -keep public class MyClass

V tomto příkladu `MyClass` nastavena na skutečný název třídy, která chcete ProGuard tak, aby přeskočil.

Můžete také registrovat vlastní názvy s `[Register]` poznámky a použijte tyto názvy přizpůsobit ProGuard pravidla. Můžete zaregistrovat názvy pro adaptéry, zobrazení, BroadcastReceivers, služby, ContentProviders, aktivity a fragmenty. Další informace o používání `[Register]` vlastních atributů, najdete v části [práce s JNI](~/android/platform/java-integration/working-with-jni.md).


### <a name="proguard-options"></a>ProGuard možnosti

ProGuard nabízí řadu možností, které je možné nakonfigurovat a poskytují lepší kontrolu nad jeho provoz. [ProGuard ruční](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/introduction.html) poskytuje úplný referenční dokumentaci pro použití ProGuard. 

Xamarin.Android podporuje ProGuard následující možnosti: 


-    [Možnosti vstupu a výstupu](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#iooptions)

-    [Zachovat možnosti](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoptions)

-    [Zmenšení možnosti](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#shrinkingoptions)

-    [Obecné možnosti](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#generaloptions)

-    [Cesty – třída](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#classpath)

-    [Názvy souborů](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filename)

-    [Filtry souborů](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filefilters)

-    [Filtry](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filters)

-    [Přehled `Keep` možnosti](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoverview)

-    [Ponechat možnost modifikátory](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoptionmodifiers)

-    [Specifikace – třída](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#classspecification)

Jsou následující možnosti *Ignorovat* podle Xamarin.Android:

-    [Možnosti optimalizace](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#optimizationoptions)

-    [Možnosti maskováním](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#obfuscationoptions) 

-    [Preverification možnosti](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#preverificationoptions)



## <a name="proguard-and-android-nougat"></a>Cukrovinkách typu nugát proGuard a Android

Pokud se pokoušíte použít ProGuard proti Android 7.0 nebo novější, musíte stáhnout na novější verzi ProGuard, protože SDK pro Android se nedodává na novou verzi, která je kompatibilní s JDK 1.8.

Můžete to použít [balíček NuGet](https://www.nuget.org/packages/name.atsushieno.proguard.facebook/5.3.0) k instalaci novější verze `proguard.jar`. Další informace o aktualizaci výchozí sady SDK pro Android `proguard.jar`, najdete [Stack Overflow](http://stackoverflow.com/questions/39514518/xamarin-android-proguard-unsupported-class-version-number-52-0/39514706#39514706) diskuzi.

Můžete najít všechny verze ProGuard na [SourceForge stránky](https://sourceforge.net/projects/proguard/files/). 



## <a name="example-proguard-configurations"></a>Příklad ProGuard konfigurace

Dva příklad ProGuard konfigurační soubory jsou uvedeny níže. Zadejte Všimněte si, že v těchto případech Xamarin.Android proces sestavení **vstupní**, **výstup**, a **knihovny** JAR. Proto se můžete soustředit na jiné možnosti jako `-keep`. 

### <a name="a-simple-android-activity"></a>Jednoduchá aktivita Android

Následující příklad ilustruje konfigurace pro jednoduché Android aktivity:

    -injars  bin/classes
    -outjars bin/classes-processed.jar
    -libraryjars /usr/local/java/android-sdk/platforms/android-9/android.jar

    -dontpreverify
    -repackageclasses ''
    -allowaccessmodification
    -optimizations !code/simplification/arithmetic

    -keep public class mypackage.MyActivity

### <a name="a-complete-android-application"></a>Kompletní aplikace pro Android

Následující příklad ilustruje konfigurace pro dokončení Android:

    -injars  bin/classes
    -injars  libs
    -outjars bin/classes-processed.jar
    -libraryjars /usr/local/java/android-sdk/platforms/android-9/android.jar

    -dontpreverify
    -repackageclasses ''
    -allowaccessmodification
    -optimizations !code/simplification/arithmetic
    -keepattributes *Annotation*

    -keep public class * extends android.app.Activity
    -keep public class * extends android.app.Application
    -keep public class * extends android.app.Service
    -keep public class * extends android.content.BroadcastReceiver
    -keep public class * extends android.content.ContentProvider

    -keep public class * extends android.view.View {
    public <init>(android.content.Context);
    public <init>(android.content.Context, android.util.AttributeSet);
    public <init>(android.content.Context, android.util.AttributeSet, int);
    public void set*(...);
    }

    -keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet);
    }

    -keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet, int);
    }

    -keepclassmembers class * implements android.os.Parcelable {
    static android.os.Parcelable$Creator CREATOR;
    }

    -keepclassmembers class **.R$* {
    public static <fields>;
    }


## <a name="proguard-and-the-xamarinandroid-build-process"></a>ProGuard a procesu sestavení Xamarin.Android

Následující části popisují, jak probíhá ProGuard během Xamarin.Android **verze** sestavení.


### <a name="what-command-is-proguard-running"></a>Jaké příkaz běží ProGuard?

ProGuard je jednoduše `.jar` součástí sady SDK pro Android. Proto je volána v příkazu: 

```shell
java -jar proguard.jar options ...
```

### <a name="the-proguard-task"></a>ProGuard úloh

Úlohu ProGuard nachází uvnitř **Xamarin.Android.Build.Tasks.dll** sestavení. Je součástí `_CompileToDalvikWithDx` cíl, který je součástí systému `_CompileDex` cíl. 

Následující seznam obsahuje příklady výchozí parametry, které jsou generovány po můžete vytvoření nového projektu pomocí **soubor > Nový projekt**: 

    ProGuardJarPath = C:\Android\android-sdk\tools\proguard\lib\proguard.jar
    AndroidSdkDirectory = C:\Android\android-sdk\
    JavaToolPath = C:\Program Files (x86)\Java\jdk1.8.0_92\\bin
    ProGuardToolPath = C:\Android\android-sdk\tools\proguard\
    JavaPlatformJarPath = C:\Android\android-sdk\platforms\android-25\android.jar
    ClassesOutputDirectory = obj\Release\android\bin\classes
    AcwMapFile = obj\Release\acw-map.txt
    ProGuardCommonXamarinConfiguration = obj\Release\proguard\proguard_xamarin.cfg
    ProGuardGeneratedReferenceConfiguration = obj\Release\proguard\proguard_project_references.cfg
    ProGuardGeneratedApplicationConfiguration = obj\Release\proguard\proguard_project_primary.cfg
    ProGuardConfigurationFiles

      {sdk.dir}tools\proguard\proguard-android.txt;
      {intermediate.common.xamarin};
      {intermediate.references};
      {intermediate.application};
      ;
     
    JavaLibrariesToEmbed = C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\MonoAndroid\v7.0\mono.android.jar
    ProGuardJarInput = obj\Release\proguard\__proguard_input__.jar
    ProGuardJarOutput = obj\Release\proguard\__proguard_output__.jar
    DumpOutput = obj\Release\proguard\dump.txt
    PrintSeedsOutput = obj\Release\proguard\seeds.txt
    PrintUsageOutput = obj\Release\proguard\usage.txt
    PrintMappingOutput = obj\Release\proguard\mapping.txt

Další příklad ukazuje typické ProGuard příkaz, který je spuštěn z prostředí IDE:

```cmd
C:\Program Files (x86)\Java\jdk1.8.0_92\\bin\java.exe -jar C:\Android\android-sdk\tools\proguard\lib\proguard.jar -include obj\Release\proguard\proguard_xamarin.cfg -include obj\Release\proguard\proguard_project_references.cfg -include obj\Release\proguard\proguard_project_primary.cfg "-injars 'obj\Release\proguard\__proguard_input__.jar';'C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\MonoAndroid\v7.0\mono.android.jar'" "-libraryjars 'C:\Android\android-sdk\platforms\android-25\android.jar'" -outjars "obj\Release\proguard\__proguard_output__.jar" -optimizations !code/allocation/variable
```

## <a name="troubleshooting"></a>Poradce při potížích

### <a name="file-issues"></a>Soubor problémy

Při čtení konfiguračního souboru ProGuard, může se zobrazit tato chybová zpráva: 

    Unknown option '-keep' in line 1 of file 'proguard.cfg'

Tento problém obvykle dochází v systému Windows `.cfg` soubor má nesprávný kódování. Nelze zpracovat ProGuard _značka pořadí bajtů_ (BOM) což může být součástí textových souborů. Pokud se nachází BOM, ProGuard ukončete výše došlo k chybě. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

K tomuto problému předešli, upravte soubor vlastní konfigurace z textového editoru, který vám umožní soubor uložit bez Kusovník. Chcete-li tento problém vyřešit, zkontrolujte, zda textový editor jeho kódování nastavena na `UTF-8`. Například textovém editoru [Poznámkový blok ++](https://notepad-plus-plus.org/) můžete ukládat soubory bez Kusovníku pomocí **kódování &gt; kódovat ve formátu UTF-8 bez BOM** při ukládání souboru. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

K tomuto problému předešli, uložte soubor vlastní konfigurace z textového editoru, která umožňuje vynechat Kusovníku. 

-----


### <a name="other-issues"></a>Další problémy

ProGuard [Poradce při potížích s](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/troubleshooting.html) stránka popisuje běžné problémy, se můžete setkat (a řešení) při použití ProGuard.


## <a name="summary"></a>Souhrn

Tato příručka vysvětlené fungování ProGuard v Xamarin.Android, jak povolit v projektu aplikace a její konfiguraci. Je k dispozici příklad ProGuard konfigurace a ho popsané řešení běžných potíží. Další informace o ProGuard nástroj a Android, najdete v části [zmenšit si kód a prostředky](http://developer.android.com/tools/help/proguard.html). 


## <a name="related-links"></a>Související odkazy

- [Příprava aplikace pro vydání](~/android/deploy-test/release-prep/index.md)
