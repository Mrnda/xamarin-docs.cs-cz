---
title: "Tipy pro odstraňování potíží"
ms.topic: article
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: ce62e844a9ec76217947c0f0f5ed5e9a81336c7e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting-tips"></a>Tipy pro odstraňování potíží

<a name="Getting_Diagnostic_Information" />

## <a name="getting-diagnostic-information"></a>Získání diagnostických informací

Xamarin.Android má několik místa pro hledání při sledování dolů různé chyby.
Mezi ně patří:

1.  Diagnostické nástroje MSBuild výstup.
2.  Protokoly nasazení zařízení.
3.  Android ladění výstup protokolu.


 <a name="Diagnostic_MSBuild_Output" />


## <a name="diagnostic-msbuild-output"></a>Výstup diagnostiky nástroje MSBuild

Diagnostické nástroje MSBuild může obsahovat další informace týkající se vytváření balíčku a může obsahovat některé informace o nasazení balíčku.

Chcete-li povolit výstup diagnostiky MSBuild v sadě Visual Studio:

1.  Klikněte na tlačítko **nástroje > Možnosti...**
2.  V levém stromové zobrazení, vyberte **projekty a řešení > sestavit a spustit**
3.  V pravém panelu nastavte rozevírací nabídce MSBuild sestavení výstupu podrobností na diagnostiky
4.  Klikněte na tlačítko **OK**
5.  Vyčistěte a sestavte svůj balíček znovu.
6.  Výstup diagnostiky se zobrazí v panelu Výstup.


Pokud chcete povolit výstup diagnostiky MSBuild v sadě Visual Studio pro Mac nebo OS X:

1.  Klikněte na tlačítko **Visual Studio pro Mac > Předvolby...**
2.  V levém stromové zobrazení, vyberte **projekty > sestavení**
3.  V pravém panelu nastavení podrobností protokolu rozevíracího seznamu pro diagnostiku
4.  Klikněte na tlačítko **OK**
5.  Restartujte Visual Studio pro Mac
6.  Vyčistěte a sestavte svůj balíček znovu.
7.  Výstup diagnostiky je viditelné v rámci Pad chyby (**zobrazení > dotyková zařízení > chyby** ), kliknutím na tlačítko vytvořit výstup.


 <a name="Device_Deployment_Logs" />


## <a name="device-deployment-logs"></a>Protokoly nasazení zařízení

Chcete-li povolit protokolování nasazení zařízení v sadě Visual Studio:

1.  **Nástroje > Možnosti...**>
2.  V levém stromové zobrazení, vyberte **Xamarin > Nastavení pro Android**
3.  V pravém panelu povolit [X] **protokolování ladění rozšíření (zapíše monodroid.log ploše)** zaškrtávací políčko.
4.  Zprávy protokolu se zapisují do souboru monodroid.log na ploše.


Visual Studio pro Mac vždycky zapíše protokoly nasazení zařízení. Hledání je je mírně obtížnější; *AndroidUtils* soubor protokolu se vytvoří pro každý den a čas, dojde k nasazení, například: **AndroidTools-2012-10-24_12-35-45.log**.

-  V systému Windows, soubory protokolu se zapisují do `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`.
-  V systému OS X, soubory protokolu se zapisují do `$HOME/Library/Logs/XamarinStudio-{VERSION}`.


 <a name="Android_Debug_Log_Output" />


## <a name="android-debug-log-output"></a>Výstup protokolu Android ladění

Android zapíše mnoho zprávy a pokuste se [Android protokol ladění](~/android/deploy-test/debugging/android-debug-log.md).
Xamarin.Android používá k řízení generování dalších zpráv do protokolu Android ladění vlastnosti systému Android. Je možné nastavit vlastnosti systému Android prostřednictvím *funkce setprop* příkazu v rámci [most ladění Android (adb)](http://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

Vlastnosti systému čteny v průběhu spuštění procesu a proto musí být buď sadu dřív, než je aplikace spuštěna, nebo po změně vlastnosti systému, je nutné restartovat aplikaci.

<a name="Xamarin.Android_System_Properties" />


### <a name="xamarinandroid-system-properties"></a>Xamarin.Android System Properties

Xamarin.Android podporuje následující vlastnosti systému:

-   *Debug.mono.Debug*: neprázdný řetězec, pokud jde o ekvivalent `*mono-debug*`.

-   *Debug.mono.env*: kanálu oddělené ('*|*') seznamu proměnných prostředí pro export během spuštění aplikace *před* mono byl inicializován. Díky tomu, že řízení protokolování mono nastavení proměnných prostředí.

    - *Poznámka:*: vzhledem k tomu, že hodnota je '*|*'-oddělené, hodnota musí mít vyšší úroveň citací, jako \` *adb prostředí* \` příkaz odebere Sada uvozovky.

    - *Poznámka:*: hodnoty vlastností Android systému nesmí být delší než 92 znaků.

    - Příklad:

            adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"

-   *Debug.mono.log*: oddělenými čárkou ('*,*') seznam součástí, které by měl vytisknout další zprávy do protokolu Android ladění. Ve výchozím nastavení nic nastavena. Součásti zahrnují:

    -   *všechny*: tisk všechny zprávy
    -   *globální katalog*: tisk GC související zprávy.
    -   *gref*: Tisk zpráv přidělení a navrácení odkaz (slabé, globální).
    -   *lref*: tisk místní odkaz přidělení a navrácení zprávy.

    *Poznámka:*: Jedná se o *velmi* verbose. Nepovolujte Pokud skutečně potřebujete.

-   *Debug.mono.Trace*: umožňuje nastavit [mono – trasování](http://docs.go-mono.com/?link=man%3amono(1)) `=PROPERTY_VALUE` nastavení.



## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android nelze vyřešit System.ValueTuple

K této chybě dochází z důvodu nekompatibility s Visual Studio.

- **Visual Studio 2017 Update 1** (verze 15.1 nebo starší) je kompatibilní s jen **System.ValueTuple NuGet 4.3.0** (nebo starší).

- **Visual Studio 2017 Update 2** (verze 15.2 nebo novější) je kompatibilní s jen **System.ValueTuple NuGet 4.3.1** (nebo novější).

Vyberte prosím správný System.ValueTuple NuGet, který odpovídá s instalací sady Visual Studio 2017.

<a name="GC_Messages" />

## <a name="gc-messages"></a>Uvolňování paměti zpráv

GC součástí zprávy lze zobrazit pomocí nastavení vlastnosti systému debug.mono.log na hodnotu, která obsahuje gc.

Globální Katalog zprávy jsou generovány vždy, když provádí globální Katalog a poskytuje informace o tom, kolik práce globální Katalog se:

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

Další informace GC například časování informace může být generována nastavení `MONO_LOG_LEVEL` proměnnou prostředí `debug`:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

Tato akce způsobí (velké množství) další Mono zprávy, včetně těchto tří důsledkem:

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

V `GC_BRIDGE` zpráva `num-objects` je počet objektů most Tento průchod je vzhledem k tomu, a `num_hash_entries` je počet objektů zpracovaných za toto volání most kódu.

V `GC_MINOR` a `GC_MAJOR` zprávy, `total` je množství času, zatímco je pozastaven na světě (bez vláken jsou prováděny), zatímco `bridge` je množství času prováděné v most zpracování kód (který přistupuje k virtuálního počítače Java). Je na světě *není* pozastaven během zpracování most.

 *Obecně*, větší hodnotu `num_hash_entries`, více času, který `bridge` kolekce bude trvat a tím větší `total` čas strávený shromažďování bude.

 <a name="Global_Reference_Messages" />


## <a name="global-reference-messages"></a>Odkaz na globální zprávy

Chcete-li povolit protokolování, globální odkaz loggig (GREF) *debug.mono.log* musí obsahovat systém vlastnost *gref*, například:

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android používá Android globální odkazy k poskytnutí mapování mezi instancemi Java a přidružené spravované instance, jako při vyvolání metody Java, které je třeba zadat Java Java instance.

Bohužel Android emulátorů povolit pouze globální odkazy 2000 existovat současně. Hardwaru má mnohem vyšší limit 52000 globální odkazy. Může být při spuštění aplikace v emulátoru, takže znalost problematické dolní limit *kde* instance pocházet z může být velmi užitečná.

 *Poznámka:*: počet globální odkaz je interní Xamarin.Android a nemá (a nemůže) zahrnují globální odkazy provedenou další nativní knihovny načíst do procesu. Použijte odkaz na globální počet jako odhad.

```shell
I/monodroid-gref(12405): +g+ grefc 108 gwrefc 0 obj-handle 0x40517468/L -> new-handle 0x40517468/L from    at Java.Lang.Object.RegisterInstance(IJavaObject instance, IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object.SetHandle(IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object..ctor(IntPtr handle, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler, Boolean removable)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler)
I/monodroid-gref(12405):    at Android.App.Activity.RunOnUiThread(System.Action action)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.UseLotsOfMemory(Android.Widget.TextView textview)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.<OnCreate>m__3(System.Object o)
I/monodroid-gref(12405): handle 0x40517468; key_handle 0x40517468: Java Type: `mono/java/lang/RunnableImplementor`; MCW type: `Java.Lang.Thread+RunnableImplementor`
I/monodroid-gref(12405): Disposing handle 0x40517468
I/monodroid-gref(12405): -g- grefc 107 gwrefc 0 handle 0x40517468/L from    at Java.Lang.Object.Dispose(System.Object instance, IntPtr handle, IntPtr key_handle, JObjectRefType handle_type)
I/monodroid-gref(12405):    at Java.Lang.Object.Dispose()
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor.Run()
I/monodroid-gref(12405):    at Java.Lang.IRunnableInvoker.n_Run(IntPtr jnienv, IntPtr native__this)
I/monodroid-gref(12405):    at System.Object.c200fe6f-ac33-441b-a3a0-47659e3f6750(IntPtr , IntPtr )
I/monodroid-gref(27679): +w+ grefc 1916 gwrefc 296 obj-handle 0x406b2b98/G -> new-handle 0xde68f4bf/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1915 gwrefc 294 handle 0xde691aaf/W from take_global_ref_jni
```

Existují čtyři hlášení o důsledků:

-  Vytvoření globální odkaz: Jedná se o řádky, které začínají *+ g +* a budou tak poskytovat trasování zásobníku pro vytváření kódu cestu.
-  Odkaz na globální odstraňování: Jedná se o řádky, které začínají *- g-* a může poskytnout trasování zásobníku pro uvolnění odkaz na globální cestu kódu. Pokud globální Katalog je uvolnění gref, bude poskytnuta žádná trasování zásobníku.
-  Globální weak odkazovat vytvoření: Jedná se o řádky, které začínají *+ w +* .
-  Globální weak odkazovat odstraňování: Jedná se o řádky, které začínají *-w-* .


Ve všech zprávách *grefc* hodnota je počet globální odkazy, které se má vytvořit Xamarin.Android, zatímco *grefwc* hodnota je počet slabé globální odkazy, které vytvořil Xamarin.Android. *Zpracování* nebo *obj popisovač* hodnota je hodnota popisovač JNI a znak po '  */* ' je typ hodnoty popisovač: */L* pro místní odkaz */G* pro globální odkazy a */W* pro slabé globální odkazy.

Jako součást procesu GC globální odkazy (+ g +) se převedou na slabé globální odkazy (způsobuje + w + a - g-), je spuštěna GC straně Java a kontroluje slabé globální odkaz zobrazit, pokud byl shromážděný. Pokud je stále aktivní, vytvoří se nový gref kolem slabé ref (+ g +, -w-), jinak zničena slabé ref (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Java instance se vytvoří a zabalené službou MCW

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>Globální Katalog je prováděna...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>Objekt je stále aktivní, jako popisovač! = null
## <a name="wref-turned-back-into-a-gref"></a>vrátit zpět do gref hodnoty Wref.

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>Objekt je neaktivní, jako popisovač == hodnotu null
## <a name="wref-is-freed-no-new-gref-created"></a>je gref uvolněné, nové vytvoření hodnoty Wref.

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

Je zde jeden "zajímavé" zkrabacení: na cíle systémem Android starší než 4.0, gref hodnota je stejná na adresu objekt Java v paměti Android runtime. (To znamená, globální Katalog je bez přesouvání, omezená, kolekce a je blokováním out přímé odkazy na tyto objekty.) Proto po + g +, + w +, - g-, + g +, - w-pořadí, výsledná gref bude mít stejnou hodnotu jako původní hodnotu gref. Díky tomu grepping prostřednictvím protokolů přímočará.

Android 4.0, ale má přesunutí kolekce a už se přímé odkazy na Android runtime předá objekty virtuálních počítačů. V důsledku toho po + g +, + w +, - g-, + g +, - w pořadí, hodnota gref *se bude lišit*. Pokud objekt odolává více GC, protože budou přenášeny několik gref hodnotami, což těžší k určení, kde byla instance ve skutečnosti přidělené z.

### <a name="querying-programmatically"></a>Dotazování prostřednictvím kódu programu

GREF i hodnoty Wref počty se můžete dotazovat pomocí dotazu `JniRuntime` objektu.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` -Globální počet odkazů

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` -Slabé počet odkazů

 <a name="Offline_Activation" />


## <a name="offline-activation"></a>Offline aktivaci

Pokud jste nelze aktivovat Xamarin.Android v systému Windows, nebo nelze nainstalovat plnou verzi Xamarin.Android na Mac OS X, naleznete v tématu [Offline aktivaci](~/android/get-started/installation/index.md) stránky.

 <a name="Can't_upgrade_to_Indie/Business_from_Trial_Account" />


## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>Nelze upgradovat ze zkušební verzi účtu Indie nebo firmy

Pokud jste nedávno koupili Xamarin.Android a dříve spustila Xamarin.Android zkušební verzi, musíte provést následující kroky, chcete-li získat tuto změnu licence zachyceny pomocí sady Visual Studio pro Mac nebo Visual Studio.

-  Zavřete Visual Studio pro Mac nebo Visual Studio
-  Odeberte všechny soubory z ~/Library/MonoAndroid na Mac nebo %PROGRAMDATA%\Mono Android\License\ pro Windows
-  Znovu otevřete Visual Studio pro Mac nebo Visual Studio a sestavení projektu Xamarin.Android


To by měl zprovoznění je. Pokud potíže potrvají, můžete pokusit [Offline aktivaci](~/android/get-started/installation/index.md) k dokončení aktivace pracovní stanici.

 <a name="Receiving_'Activation_Incomplete'_Error_Message" />


## <a name="receiving-activation-incomplete-error-message"></a>Přijetí ' aktivace neúplné chybová zpráva

Tomuto problému může dojít při použití Xamarin.Android pro sadu Visual Studio. Chcete-li vyřešit tento problém, pošlete prosím protokoly z následujícího umístění na  *contact@xamarin.com* .

-  Umístění protokolu: **LocalAppData %\\Xamarin\\protokoly**


 <a name="Receiving_'Error_Retrieving_Update_Information'_Error_Message" />


## <a name="receiving-error-retrieving-update-information-error-message"></a>Zobrazuje chybová zpráva "Chyba při načítání informací o aktualizacích.

Čas od času aktualizace se nezdaří došlo k následující chybě, která se často stane, když probíhá kontrola aktualizací:

Většinu doby, tuto chybu můžete vyřešit jednoduše tak, že protokolování mimo váš účet Xamarin, a pak zpátky protokolování.

K tomu, najít vaši platformu volba níže a postupujte podle kroků:

**V systému Mac:**
1. Otevřete Visual Studio pro Mac
2. Vyberte sady Visual Studio pro Mac > účet...
3. Klikněte na tlačítko při odhlášení
4. Klikněte na tlačítko Přihlásit
5. Zadejte své přihlašovací údaje
6. Kontrola aktualizací

**Na počítači pomocí Visual Studo:**
1. Open Visual Studio
2. Vyberte položku Nástroje > účet Xamarin
3. Klikněte na tlačítko při odhlášení
4. Klikněte na tlačítko Přihlásit
5. Zadejte své přihlašovací údaje
6. Kontrola aktualizací

Pokud se tato chybová zpráva i nadále zobrazovat, prosím odesílání e-mailem  **contact@xamarin.com** .


 <a name="Android_Debug_Logs" />


## <a name="android-debug-logs"></a>Protokoly Android ladění

[Protokoly Androidu ladění](~/android/deploy-test/debugging/android-debug-log.md) může poskytnout další kontext případné chyby runtime zobrazeny.

 <a name="Floating-Point_performance_is_terrible!" />


## <a name="floating-point-performance-is-terrible"></a>S plovoucí desetinnou čárkou výkonu je strašlivých!

Alternativně "Moje aplikace běží 10 x rychlejší s sestavení ladicí verze než s sestavení pro vydání!"

Podporuje více zařízení bis Xamarin.Android: *armeabi*, *armeabi v7a*, a *x86*. Bis zařízení lze zadat v rámci **vlastnosti projektu > karta Application > podporované architektury**.

Sestavení pro ladění použít balíček Android, která poskytuje všechny bis a proto budou používat nejrychlejší ABI pro cílové zařízení.

Verze sestavení bude obsahovat pouze bis vybrán na kartě Vlastnosti projektu. Můžete vybrat více než jednou.

*armeabi* výchozí ABI a má nejširší podpora zařízení. *Ale*, armeabi nepodporuje více procesorů zařízení a hardware s plovoucí desetinnou čárkou, amont jiných věcí. Aplikace používající verzi modulu runtime armeabi v důsledku toho budou svázané s jedním jádrem a používat softwarové float implementace. Obě tyto může přispět k výrazně pomalejší výkon pro vaši aplikaci.

Pokud aplikace potřebuje dobré výkonu s plovoucí desetinnou čárkou (například hry), měli byste povolit *armeabi v7a* ABI. Můžete chtít podporují pouze *armeabi v7a* modul runtime, i když to znamená, že starší zařízení, které podporují pouze *armeabi* budou moci spouštět aplikace.

 <a name="Could_not_locate_Android_SDK" />


## <a name="could-not-locate-android-sdk"></a>Nepovedlo se najít sady SDK pro Android

Nejsou k dispozici z Google Android SDK pro Windows 2 stahování.
Pokud si zvolíte instalační program .exe, zapíše klíče registru, které informují Xamarin.Android, kam se nainstaloval. Zvolíte-li soubor .zip a rozbalte ho sami, Xamarin.Android nebude vědět, kde má být vyhledán sady SDK. Můžete zjistit Xamarin.Android kde sady SDK je v sadě Visual Studio přechodem na **nástroje > Možnosti > Xamarin > Nastavení Androidu**:

[![Umístění sady SDK pro Android v nastavení Xamarin Android](troubleshooting-images/01a.png)]()

<a name="IDE_does_not_display_target_device" />


## <a name="ide-does-not-display-target-device"></a>IDE nezobrazí cílového zařízení

Někdy se pokusí nasadit aplikaci na zařízení, ale zařízení, které chcete nasadit do není zobrazený v dialogovém okně vyberte zařízení. To může nastat při ladění most Android rozhodne, přejděte na dovolenou.

Chcete-li tento problém diagnostikovat, vyhledejte [adb program](~/android/deploy-test/debugging/android-debug-log.md), spusťte:

```shell
adb devices
```

Pokud zařízení není nainstalován, budete muset restartovat server most ladění Android tak, aby vaše zařízení lze nalézt:

```shell
adb kill-server
adb start-server
```

Synchronizace HTC softwaru může zabránit **adb start-server** z funguje správně. Pokud **adb start-server** příkaz není vytiskněte port, který spouští na, ukončete software HTC synchronizace a zkuste restartovat adb server.


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>Zadaný úkol spustitelný soubor "keytool" nebylo možné spustit.

To znamená, že vaše cesta neobsahuje adresáře, kde je umístěný adresáře bin sady Java SDK. Zkontrolujte, jestli jste postupovali podle těchto kroků z [instalace](~/android/get-started/installation/index.md) průvodce.


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe nebo aresgen.exe byl ukončen s kódem 1

Při ladění tohoto problému, přejděte do sady Visual Studio a změňte úroveň podrobností MSBuild, chcete-li to provést, vyberte: **nástroje > Možnosti > projekt** a **řešení > sestavení** a **spustit > Podrobnosti výstup sestavení projektu MSBuild** a nastavte tuto hodnotu **normální**.

Znovu sestavte a zkontrolovat, v podokně výstupu sady Visual Studio, který by měl obsahovat úplnou chyby.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>Není dostatek místa na zařízení k nasazení balíčku

K tomu dochází, když nemáte spusťte emulátor ze v sadě Visual Studio. Při spouštění v emulátoru mimo Visual Studio, je potřeba předat `-partition-size 512` možnosti, například

```shell
emulator -partition-size 512 -avd MonoDroid
```

Ujistěte se, použijete název správné simulátoru, tj. [název, který jste použili při konfiguraci simulátoru](~/android/get-started/installation/windows.md#device).

<a name="INSTALL_FAILED_INVALID_APK_when_installing_a_package" />

## <a name="installfailedinvalidapk-when-installing-a-package"></a>NAINSTALUJTE\_se nezdařilo\_neplatný\_APK při instalaci balíčku

Balíček Android názvy *musí* obsahovat tečku (.*.*'). Upravte název balíčku tak, aby obsahoval období.

-   Within Visual Studio:
    -   Klikněte pravým tlačítkem na projekt > Vlastnosti
    -   Klikněte na kartu Android Manifest na levé straně.
    -   Aktualizujte pole název balíčku.
        -   Pokud se zobrazí zpráva &ldquo;ne AndroidManifest.xml nalezen. Klikněte na tlačítko Přidat. &rdquo;, klikněte na odkaz a pak aktualizujte pole název balíčku.
-   V sadě Visual Studio pro Mac:
    -   Klikněte pravým tlačítkem na projekt > Možnosti.
    -   Přejděte do sestavení nebo část aplikace pro Android.
    -   Změňte pole název balíčku tak, aby obsahovala '.'.


<a name="INSTALL_FAILED_MISSING_SHARED_LIBRARY_when_installing_a_package" />


## <a name="installfailedmissingsharedlibrary-when-installing-a-package"></a>NAINSTALUJTE\_se nezdařilo\_CHYBÍ\_SDÍLENÉ\_KNIHOVNY při instalaci balíčku

"Sdílené knihovny" v tomto kontextu je *není* nativní sdílené knihovny (*libfoo.so*) souboru; se místo toho knihovnu, která musí být instalována samostatně na cílové zařízení, jako je například Google Maps.

Balíček Android Určuje, které sdílené knihovny jsou požadovány s `<uses-library/>` elementu. Pokud *požadované* knihovny se nenachází na cílovém zařízení (například `//uses-library/@android:required` je *true*, což je výchozí hodnota), instalace balíčku se nezdaří s *nainstalovat\_ Se nezdařilo\_CHYBÍ\_SDÍLENÉ\_KNIHOVNY*.

Pokud chcete zjistit, které sdílené knihovny jsou požadovány, podívejte se *generované*
**AndroidManifest.xml** souboru (například **obj\\ladění\\android \\AndroidManifest.xml**) a podívejte se `<uses-library/>` elementy. `<uses-library/>` elementy lze přidat ručně ve vašem projektu **vlastnosti\\AndroidManifest.xml** souboru a pomocí [UsesLibraryAttribute vlastní atribut](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/).

Například přidat odkaz na sestavení pro *Mono.Android.GoogleMaps.dll* implicitně přidá `<uses-library/>` pro službu mapy Google sdílenou knihovnu.

<a name="INSTALL_FAILED_UPDATE_INCOMPATIBLE_when_installing_a_package" />


## <a name="installfailedupdateincompatible-when-installing-a-package"></a>NAINSTALUJTE\_se nezdařilo\_aktualizace\_kompatibilní při instalaci balíčku

Android balíčky mají tři požadavky:

-   Musí obsahovat '. " (viz předchozí položce)
-   Musí mít název balíčku jedinečné řetězce (proto zpětného tld konvence zobrazená v aplikaci pro Android názvy, například com.android.chrome pro aplikace pro Chrome)
-   Při upgradu balíčky, balíček musí mít stejný podpisový klíč.

Proto Představte si tuto situaci:

1.  Můžete vytvořit a nasadit aplikaci jako ladění aplikace
2.  Můžete například změnit podpisový klíč na použít jako verze aplikace (nebo protože nemáte rádi zadaný výchozí ladění podpisový klíč)
3.  Instalace aplikace bez předchozího odstranění ji nejdřív, například ladění > Spustit bez ladění v sadě Visual Studio


V takovém případě balíček instalace se nezdaří s instalací\_se nezdařilo\_aktualizace\_NEKOMPATIBILNÍ chybě, protože při podepisování se nezměnil název balíčku nebyla klíč. [Android protokol ladění](~/android/deploy-test/debugging/android-debug-log.md) bude také obsahovat zpráva podobná:

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

Pokud chcete vyřešit tuto chybu, úplně odeberte aplikace ze zařízení před instalací znovu.

<a name="INSTALL_FAILED_UID_CHANGED_when_installing_a_package" />

## <a name="installfaileduidchanged-when-installing-a-package"></a>NAINSTALUJTE\_se nezdařilo\_UID\_změnit při instalaci balíčku

Pokud je nainstalován balíček Android, je přiřazen *id uživatele* (UID).
*Někdy*, momentálně Neznámý z důvodů, při instalaci přes již nainstalované aplikace, instalace se nezdaří s `INSTALL_FAILED_UID_CHANGED`:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

Chcete-li vyřešit tento problém *plně odinstalovat* balíček Android, buď nainstalujete aplikaci z Android cíl grafického uživatelského rozhraní nebo pomocí `adb`:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**Nepoužívejte** `adb uninstall -k`, jak to bude *zachovat* data aplikací a proto zachovat konfliktní UID na cílovém zařízení.


<a name="Release_apps_fail_to_launch_on_device" />

## <a name="release-apps-fail-to-launch-on-device"></a>Verze aplikace se nepodařilo otevřít na zařízení

Android protokol ladění výstup bude neobsahuje zpráva podobná:

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

Pokud ano, existují dvě možné příčiny tohoto:

1.  .apk neposkytuje ABI, který podporuje cílové zařízení.
    Například .apk pouze obsahuje binární soubory armeabi v7a a cílové zařízení podporuje pouze armeabi.

2.  [Android chyb](http://code.google.com/p/android/issues/detail?id=21670). Pokud je to tento případ, odinstalovat aplikaci, křížové prsty a znovu nainstalovat aplikaci.

Chcete-li opravit (1), upravte možnosti/vlastností projektu a [přidat podporu pro požadované ABI do seznamu podporované bis ](~/android/app-fundamentals/cpu-architectures.md). Chcete-li zjistit, které ABI, je nutné přidat, spusťte následující příkaz adb proti cílové zařízení:

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

Výstup bude obsahovat primární (a volitelné sekundární) a bis.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>Vlastnost OutPath není nastavená pro projekt &ldquo;MyApp.csproj&rdquo;

To obvykle znamená máte počítač s HP a proměnnou prostředí &ldquo;platformy&rdquo; byla nastavena na něco jako Měničem nebo HPD. Tato možnost v konfliktu s vlastností MSBuild platformu, která je obvykle nastavena na &ldquo;libovolný procesor&rdquo; nebo &ldquo;x86&rdquo;. Budete muset odebrat tuto proměnnou prostředí z počítače předtím, než může fungovat MSBuild:

-   Ovládací panely > Systém > Upřesnit > proměnné prostředí

Restartujte Visual Studio nebo Visual Studio pro Mac a pokuste se znovu sestavit. Co by měl nyní fungovat podle očekávání.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException: mono.android.runtime.JavaObject nelze převést na...

Xamarin.Android 4.x není zařazování správně vnořené obecné typy správně. Zvažte například následující C\# programování s využitím [SimpleExpandableListAdapter](https://developer.xamarin.com/api/type/Android.Widget.SimpleExpandableListAdapter/):


```csharp
// BAD CODE; DO NOT USE
var groupData = new List<IDictionary<string, object>> () {
        new Dictionary<string, object> {
                { "NAME", "Group 1" },
                { "IS_EVEN", "This group is odd" },
        },
};
var childData = new List<IList<IDictionary<string, object>>> () {
        new List<IDictionary<string, object>> {
                new Dictionary<string, object> {
                        { "NAME", "Child 1" },
                        { "IS_EVEN", "This group is odd" },
                },
        },
};
mAdapter = new SimpleExpandableListAdapter (
        this,
        groupData,
        Android.Resource.Layout.SimpleExpandableListItem1,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
        childData,
        Android.Resource.Layout.SimpleExpandableListItem2,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
);
```


Problém je, že Xamarin.Android nesprávně zařazuje vnořené obecné typy. `List<IDictionary<string, object>>` Je právě zařazen do [java.lang.ArrrayList](https://developer.xamarin.com/api/type/Java.Util.ArrayList/), ale `ArrayList` je obsahující `mono.android.runtime.JavaObject` instance (která odkaz `Dictionary<string, object>` instance) namísto něco, který implementuje [java.util.Map](https://developer.xamarin.com/api/type/Java.Util.IMap/), což následující výjimky:

```shell
E/AndroidRuntime( 2991): FATAL EXCEPTION: main
E/AndroidRuntime( 2991): java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to java.util.Map
E/AndroidRuntime( 2991):        at android.widget.SimpleExpandableListAdapter.getGroupView(SimpleExpandableListAdapter.java:278)
E/AndroidRuntime( 2991):        at android.widget.ExpandableListConnector.getView(ExpandableListConnector.java:446)
E/AndroidRuntime( 2991):        at android.widget.AbsListView.obtainView(AbsListView.java:2271)
E/AndroidRuntime( 2991):        at android.widget.ListView.makeAndAddView(ListView.java:1769)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillDown(ListView.java:672)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillFromTop(ListView.java:733)
E/AndroidRuntime( 2991):        at android.widget.ListView.layoutChildren(ListView.java:1622)
```

Alternativní řešení je použití poskytnutého [typy kolekcí Java](~/android/internals/api-design.md) místo `System.Collections.Generic` typy pro &ldquo;vnitřní&rdquo; typy. Při zařazování instance, tato akce způsobí odpovídající typy jazyka Java. (Následující kód je složitější než nezbytné, aby se snížit gref životnosti. Může být zjednodušenou změna původní kód prostřednictvím `s/List/JavaList/g` a `s/Dictionary/JavaDictionary/g` Pokud gref životnosti nejsou obavy.)

```csharp
// insert good code here
using (var groupData = new JavaList<IDictionary<string, object>> ()) {
    using (var groupEntry = new JavaDictionary<string, object> ()) {
        groupEntry.Add ("NAME", "Group 1");
        groupEntry.Add ("IS_EVEN", "This group is odd");
        groupData.Add (groupEntry);
    }
    using (var childData = new JavaList<IList<IDictionary<string, object>>> ()) {
        using (var childEntry = new JavaList<IDictionary<string, object>> ())
        using (var childEntryDict = new JavaDictionary<string, object> ()) {
            childEntryDict.Add ("NAME", "Child 1");
            childEntryDict.Add ("IS_EVEN", "This child is odd.");
            childEntry.Add (childEntryDict);
            childData.Add (childEntry);
        }
        mAdapter = new SimpleExpandableListAdapter (
            this,
            groupData,
            Android.Resource.Layout.SimpleExpandableListItem1,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
            childData,
            Android.Resource.Layout.SimpleExpandableListItem2,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
        );
    }
}
```

[Tento problém bude vyřešený v příští verzi](https://bugzilla.xamarin.com/show_bug.cgi?id=5401).

<a name="Unexpected_NullReferenceExceptions" />

## <a name="unexpected-nullreferenceexceptions"></a>Neočekávané NullReferenceExceptions

Čas od času [Android protokol ladění](~/android/deploy-test/debugging/android-debug-log.md) bude zmínili NullReferenceExceptions, &ldquo;nemůže dojít,&rdquo; nebo pocházejí z Mono pro Android runtime kód krátce před zemře aplikace:

```shell
E/mono(15202): Unhandled Exception: System.NullReferenceException: Object reference not set to an instance of an object
E/mono(15202):   at Java.Lang.Object.GetObject (IntPtr handle, System.Type type, Boolean owned)
E/mono(15202):   at Java.Lang.Object._GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Java.Lang.Object.GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Android.Views.View+IOnTouchListenerAdapter.n_OnTouch_Landroid_view_View_Landroid_view_MotionEvent_(IntPtr jnienv, IntPtr native__this, IntPtr native_v, IntPtr native_e)
E/mono(15202):   at (wrapper dynamic-method) object:b039cbb0-15e9-4f47-87ce-442060701362 (intptr,intptr,intptr,intptr)
```

or

```shell
E/mono    ( 4176): Unhandled Exception:
E/mono    ( 4176): System.NullReferenceException: Object reference not set to an instance of an object
E/mono    ( 4176): at Android.Runtime.JNIEnv.NewString (string)
E/mono    ( 4176): at Android.Util.Log.Info (string,string)
```

To může nastat při Android runtime rozhodne na zrušení procesu, který může nastat z nejrůznějších důvodů, včetně nedosáhli limitu GREF cílové složky nebo dělat něco &ldquo;nesprávný&rdquo; s JNI.

Pokud chcete zobrazit, pokud se jedná o tento případ, najdete v protokolu Android ladění zprávy z vaší podobný procesu:

```shell
E/dalvikvm(  123): VM aborting
```

<a name="Abort_due_to_Global_Reference_Exhaustion" />

## <a name="abort-due-to-global-reference-exhaustion"></a>Kvůli vyčerpání globálního odkaz k přerušení

Android runtime JNI vrstvy podporuje pouze omezený počet odkazů na objekty JNI byla platná v libovolném bodě v čase. Při překročení tohoto limitu rozdělit věcí.

GREF (*globální odkaz*) omezení je 2000 odkazy v emulátoru, ~ 52000 odkazuje na hardwaru.

Víte, že začínáte vytvořit příliš mnoho GREFs po zobrazení zprávy, jako je tato v Android ladění protokolu:

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

Po dosažení limitu GREF je vytištěno zprávu například následující:

```shell
D/dalvikvm(  602): GREF has increased to 2001
W/dalvikvm(  602): Last 10 entries in JNI global reference table:
W/dalvikvm(  602):  1991: 0x4057eff8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1992: 0x4057f010 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1993: 0x40698e70 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1994: 0x40698e88 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1995: 0x40698ea0 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1996: 0x406981f0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1997: 0x40698208 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1998: 0x40698220 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1999: 0x406956a8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  2000: 0x406956c0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602): JNI global reference table summary (2001 entries):
W/dalvikvm(  602):    51 of Ljava/lang/Class; 164B (41 unique)
W/dalvikvm(  602):    46 of Ljava/lang/Class; 188B (17 unique)
W/dalvikvm(  602):     6 of Ljava/lang/Class; 212B (6 unique)
W/dalvikvm(  602):    11 of Ljava/lang/Class; 236B (7 unique)
W/dalvikvm(  602):     3 of Ljava/lang/Class; 260B (3 unique)
W/dalvikvm(  602):     4 of Ljava/lang/Class; 284B (2 unique)
W/dalvikvm(  602):     8 of Ljava/lang/Class; 308B (6 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 316B
W/dalvikvm(  602):     4 of Ljava/lang/Class; 332B (3 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 356B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 380B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 452B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 476B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 500B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 548B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 572B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 596B (2 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 692B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 956B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1004B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1148B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 1172B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1316B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3452B
W/dalvikvm(  602):     1 of Ljava/lang/String; 28B
W/dalvikvm(  602):     2 of Ldalvik/system/VMRuntime; 12B (1 unique)
W/dalvikvm(  602):    10 of Ljava/lang/ref/WeakReference; 28B (10 unique)
W/dalvikvm(  602):     1 of Ldalvik/system/PathClassLoader; 44B
W/dalvikvm(  602):  1553 of Landroid/graphics/Point; 20B (1553 unique)
W/dalvikvm(  602):   261 of Landroid/graphics/Point; 28B (261 unique)
W/dalvikvm(  602):     1 of Landroid/view/MotionEvent; 100B
W/dalvikvm(  602):     1 of Landroid/app/ActivityThread$ApplicationThread; 28B
W/dalvikvm(  602):     1 of Landroid/content/ContentProvider$Transport; 28B
W/dalvikvm(  602):     1 of Landroid/view/Surface$CompatibleCanvas; 44B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$ControlledInputConnectionWrapper; 36B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$1; 12B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$W; 28B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/view/accessibility/AccessibilityManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout$LayoutParams; 44B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout; 332B
W/dalvikvm(  602):     2 of Lorg/apache/harmony/xnet/provider/jsse/TrustManagerImpl; 28B (1 unique)
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$MyWindow; 36B
W/dalvikvm(  602):     1 of Ltouchtest/RenderThread; 92B
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$3; 12B
W/dalvikvm(  602):     1 of Ltouchtest/DrawingView; 412B
W/dalvikvm(  602):     1 of Ltouchtest/Activity1; 180B
W/dalvikvm(  602): Memory held directly by tracked refs is 75624 bytes
E/dalvikvm(  602): Excessive JNI global references (2001)
E/dalvikvm(  602): VM aborting
```


V předchozím příkladu (který náhodně, pochází z [chyb 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)) problém je, že příliš mnoho Android.Graphics.Point vytváření instancí; viz [komentář \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2) seznam oprav pro této konkrétní chyby.

Obvykle je užitečné řešení k vyhledání typů, které se má příliš mnoho instancí přidělené &ndash; Android.Graphics.Point ve výše uvedené výpisu &ndash; vyhledejte, kde vytvoříte zdrojový kód a uvolnění z nich správně (tak, aby jejich Doba platnosti Java-Object bude zkráceno). To není vždy vhodné (\#685215 je s více vlákny, takže trivial řešení zabraňuje volání metody Dispose), ale je první věcí, které je třeba zvážit.

Můžete povolit [GREF protokolování](~/android/troubleshooting/index.md) vytvoření GREFs a jejich počet.

<a name="Abort_due_to_JNI_type_mismatch" />

## <a name="abort-due-to-jni-type-mismatch"></a>Kvůli neshodě typů JNI došlo k přerušení

Pokud jste ručně kumulativní JNI kódu, je možné, že nebude typy odpovídat správně, například pokud se pokusíte vyvolání `java.lang.Runnable.run` na typ, který neimplementuje `java.lang.Runnable`. V takovém případě bude zpráva podobná této Android protokolu ladění:

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>Podpora dynamických kódu

### <a name="dynamic-code-does-not-compile"></a>Dynamický kód nelze kompilovat

Pokud chcete použít C\# dynamické v aplikaci nebo knihovny, musíte přidat System.Core.dll, Microsoft.CSharp.dll a Mono.CSharp.dll do projektu.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>V sestavení pro vydání MissingMethodException dojde k dynamické kódu v době běhu.

-   Je pravděpodobné, že váš projekt aplikace nemá odkazy na System.Core.dll, Microsoft.CSharp.dll nebo Mono.CSharp.dll. Zajistěte, aby že se tyto sestavení odkazuje.

    -   Mějte na paměti této dynamický kód vždy náklady. Pokud potřebujete efektivní kódu, zvažte, není použití dynamický kód.

-   V první verzi preview byly vyloučeny tyto sestavení, pokud kód aplikace jsou explicitně použít typy v každé sestavení. Následující témata alternativní řešení: [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>Projekty vytvořené pomocí AOT + LLVM havárií na x86 zařízení

Při nasazování aplikace vytvořené s [AOT + LLVM](~/android/deploy-test/release-prep/index.md) v zařízeních se systémem x86, může zobrazit chybová zpráva podobná následující výjimky:

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

Jedná se o známý problém, jsou uvedeny v [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111). Alternativní řešení je zakázat LLVM.
