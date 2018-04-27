---
title: Proces sestavení
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2018
ms.openlocfilehash: 806ed841ec4db037a063bb458e1eed13226e08bd
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="build-process"></a>Proces sestavení


## <a name="overview"></a>Přehled

Proces sestavení Xamarin.Android zodpovídá za připevnění všechno, co společně: [generování `Resource.designer.cs` ](~/android/internals/api-design.md), podpůrné `AndroidAsset`, `AndroidResource`a dalších [akce sestavení](#Build_Actions), generování [Android – obálky s možností](~/android/platform/java-integration/android-callable-wrappers.md)a generování `.apk` pro spuštění na zařízeních s Androidem.


## <a name="application-packages"></a>Balíčky aplikací

V obecné podmínky, existují dva typy balíčků aplikace pro Android (`.apk` soubory) kterého může generovat systém Xamarin.Android sestavení: 

-   **Verze** sestavení, které jsou plně samostatné a nevyžadují další balíčky provést. Toto jsou balíčky, u nichž by na obchod s aplikacemi. 

-   **Ladění** sestavení, které nejsou. 

Toto nastavení není shodou odpovídat MSBuild `Configuration` produkuje balíčku.

### <a name="shared-runtime"></a>Sdílený modul Runtime

*Sdílený modul runtime* je pár dalších Android balíčky, které poskytují základní knihovny tříd (`mscorlib.dll`atd) a knihovně Android vazba (`Mono.Android.dll`atd.). Ladění sestavení závisí sdílený modul runtime místo včetně základní knihovny tříd a vazby sestavení v rámci balíčku aplikace pro Android, povolení ladění balíčku, který má být menší.

Sdílený modul runtime mohou být zakázány v sestavení pro ladění nastavením `$(AndroidUseSharedRuntime)` vlastnost `False`. 

<a name="Fast_Deployment" />

### <a name="fast-deployment"></a>Rychlé nasazení

*Rychlé nasazení* spolupracuje s modulem runtime sdílené další zmenšení velikosti balíček aplikace pro Android. K tomu potřeba není sdružování sestavení aplikace v rámci balíčku. Místo toho se zkopírují na cílový prostřednictvím `adb push`. Tento proces urychluje cyklus sestavení/nasadit/debug, protože pokud *pouze* sestavení se změní, balíček není přeinstalovat. Místo toho jsou pouze aktualizované sestavení znovu synchronizované cílového zařízení. 

Rychlé nasazení se označuje selhání na zařízení, které blokovat `adb` synchronizaci adresáře `/data/data/@PACKAGE_NAME@/files/.__override__`. 

Rychlé nasazení je ve výchozím nastavení povolené a mohou být zakázány v ladicí sestavení nastavením `$(EmbedAssembliesIntoApk)` vlastnost `True`.


## <a name="msbuild-projects"></a>Projektů MSBuild

Proces sestavení Xamarin.Android je založena na MSBuild, což je také použít Visual Studio pro Mac a Visual Studio formát souboru projektu.
Normálně, nebudou uživatelé muset ručně upravte soubory nástroje MSBuild &ndash; IDE vytvoří plně funkční projekty a aktualizuje všechny změny a podle potřeby automaticky vyvolání sestavení cílů. 

Pokročilí uživatelé chtít provádět akce, které nejsou podporované grafického uživatelského rozhraní IDE, takže procesu sestavení je přizpůsobitelné přímou úpravou souboru projektu. Tato stránka dokumenty pouze funkce specifické pro Xamarin.Android a přizpůsobení &ndash; je možné s normální MSBuild položky, vlastnosti a cíle, mnoho dalších věcí. 

<a name="Build_Targets" />

## <a name="build-targets"></a>Sestavení cílů

Následující cíle sestavení jsou definovány pro Xamarin.Android projekty:

-   **Sestavení** &ndash; sestavení balíčku.

-   **Vyčištění** &ndash; odebere všechny soubory vygenerovaných procesem sestavení.

-   **Nainstalujte** &ndash; nainstaluje balíček do výchozího zařízení nebo virtuální zařízení.

-   **Odinstalace** &ndash; Odinstaluje balíček z výchozího zařízení nebo virtuální zařízení.

-   **SignAndroidPackage** &ndash; vytvoří a podepíše balíčku (`.apk`). Použití s `/p:Configuration=Release` ke generování balíčků nezávislý "Verze".

-   **UpdateAndroidResources** &ndash; aktualizace `Resource.designer.cs` souboru. Tento cíl nazývá IDE obvykle, když se přidají nové prostředky do projektu.


## <a name="build-properties"></a>Vlastnosti sestavení

Vlastnosti nástroje MSBuild řídí chování cíle. Jsou uvedené v souboru projektu, například **MyApp.csproj**, uvnitř [MSBuild PropertyGroup – element](http://msdn.microsoft.com/en-us/library/t4w159bs.aspx). 

-   **Konfigurace** &ndash; Určuje konfiguraci sestavení, které chcete použít, například "Ladění" nebo "Verze". Vlastnosti konfigurace slouží k určení výchozí hodnoty pro ostatní vlastnosti, které určují chování cíl. Další konfigurace může být vytvořen v rámci vašeho rozhraní IDE.

    *Ve výchozím nastavení*, `Debug` bude mít za následek konfigurace `Install` a `SignAndroidPackage` cíle vytváření menší Android balíčku, který vyžaduje další soubory a balíčky pracovat.

    Výchozí hodnota `Release` bude mít za následek konfigurace `Install` a `SignAndroidPackage` cíle vytváření Android balíčku, který je *samostatné*a mohou být použity bez instalace dalších balíčků nebo soubory.

-   **DebugSymbols** &ndash; logickou hodnotu, která určuje, zda je balíček Android *debuggable*, v kombinaci s `$(DebugType)` vlastnost. Debuggable balíček obsahuje symboly ladění, nastaví `//application/@android:debuggable` atribut `true`a automaticky přidá `INTERNET` oprávnění tak, aby k procesu můžete připojit ladicí program. Aplikace je debuggable Pokud `DebugSymbols` je `True` *a* `DebugType` je buď prázdný řetězec nebo `Full`.

-   **DebugType** &ndash; Určuje [typ symboly ladění](http://msdn.microsoft.com/en-us/library/s5c8athz.aspx) ke generování jako součást sestavení, což také ovlivňuje, jestli je aplikace debuggable. Možné hodnoty patří:

    - **Úplné**: úplná symboly jsou generovány. Pokud `DebugSymbols` vlastnosti MSBuild je také `True`, pak je debuggable balíčku aplikace.

    - **PdbOnly**: "PDB" symboly jsou generovány. Balíček aplikace bude *není* být debuggable.

    Pokud `DebugType` není nastavena nebo je prázdný řetězec, pak se `DebugSymbols` vlastnost určuje, zda je debuggable aplikace.


### <a name="install-properties"></a>Vlastnosti instalace

Vlastnosti instalace řídí chování `Install` a `Uninstall` cíle.

-   **AdbTarget** &ndash; určuje Android cílové zařízení balíček Android může být nainstalována do nebo odebrat z. Hodnota této vlastnosti je stejná jako [ `adb` – možnost cílové zařízení](http://developer.android.com/tools/help/adb.html#issuingcommands):

    ```bash
    # Install package onto emulator via -e
    # Use `/Library/Frameworks/Mono.framework/Commands/msbuild` on OS X
    MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
    ```


### <a name="packaging-properties"></a>Balení vlastnosti

Balení vlastnosti řídit vytváření balíček Android a jsou používány `Install` a `SignAndroidPackage` cíle.
[Podepisování vlastnosti](#Signing_Properties) jsou také v případě relevantní balení verzi aplikace.


-   **AndroidApkSigningAlgorithm** &ndash; hodnotu řetězce, který určuje podpisový algoritmus pro použití s `jarsigner -sigalg`.

    Výchozí hodnota je `md5withRSA`.

    Přidat v Xamarin.Android 8.2.

-   **AndroidApplication** &ndash; logická hodnota, která určuje, zda je projekt pro aplikace pro Android (`True`) nebo pro projekt Android knihovny (`False` nebo není k dispozici).

    Pouze jeden projekt pomocí `<AndroidApplication>True</AndroidApplication>` mohou být přítomné v rámci balíček Android. (Bohužel to není ještě ověřit, což může vést k jemně a zvláštní chyby týkající se systémem Android prostředky.)

-   **AndroidBuildApplicationPackage** &ndash; logická hodnota, která označuje, zda se k vytvoření a podepsání balíčku (.apk). Nastavení této hodnoty na `True` je ekvivalentní k použití [SignAndroidPackage](#Build_Targets) sestavení cíl.

    Přidala se podpora pro tuto vlastnost po Xamarin.Android 7.1.

    Tato vlastnost je `False` ve výchozím nastavení.

-   **AndroidEnableMultiDex** &ndash; vlastnost typu boolean, která určuje, zda podporu více dex se použije v konečné `.apk`.

    V Xamarin.Android 5.1 přidala se podpora pro tuto vlastnost.

    Tato vlastnost je `False` ve výchozím nastavení.

-   **AndroidEnableSGenConcurrent** &ndash; vlastnost typu boolean, která určuje, zda Mono na [souběžné uvolňování paměti kolekce](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen) se použije.

    V Xamarin.Android 7.2 přidala se podpora pro tuto vlastnost.

    Tato vlastnost je `False` ve výchozím nastavení.

-   **AndroidErrorOnCustomJavaObject** &ndash; vlastnost typu boolean, která určuje, zda může implementovat typy `Android.Runtime.IJavaObject` 
     *bez* také dědění z `Java.Lang.Object` nebo `Java.Lang.Throwable`:

    ```csharp
    class BadType : IJavaObject {
        public IntPtr Handle {
            get {return IntPtr.Zero;}
        }

        public void Dispose()
        {
        }
    }
    ```

    V případě hodnoty True tyto typy vygenerují chybu XA4212, jinak se budou generovat XA4212 upozornění.

    V Xamarin.Android 8.1 přidala se podpora pro tuto vlastnost.

    Tato vlastnost je `True` ve výchozím nastavení.

-   **AndroidFastDeploymentType** &ndash; A `:` (dvojtečka) – seznam oddělených hodnot k řízení, jaké typy se dá nasadit na [adresáře rychlého nasazení](#Fast_Deployment) na cílovém zařízení při `$(EmbedAssembliesIntoApk)` Vlastnosti nástroje MSBuild `False`. Pokud prostředek je rychlé nasazení, je *není* vkládat do vygenerovaného `.apk`, můžete urychlit časů nasazení. (Další, který je rychlé nasazená, pak méně často `.apk` musí být znovu sestavit a proces instalace může být rychlejší.) Platné hodnoty patří:

    - `Assemblies`: Nasazení sestavení aplikace.

    - `Dexes`: Nasazení `.dex` souborů, Android prostředky a prostředky Android. **Tato hodnota může *pouze* použít na zařízeních se systémem Android 4.4 nebo novější (API-19).**

    Výchozí hodnota je `Assemblies`.

    **Experimentální**. Přidat v Xamarin.Android 6.1.

-   **AndroidApplicationJavaClass** &ndash; úplný název třídy Java používat místě `android.app.Application` při třídy dědí z [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/).

    Tato vlastnost se obvykle nastavuje *jiných* vlastnosti, například `$(AndroidEnableMultiDex)` vlastnosti MSBuild.

    Přidat v Xamarin.Android 6.1.

-   **AndroidHttpClientHandlerType** &ndash; výchozí ovládací prvky `System.Net.Http.HttpMessageHandler` implementace, které budou používat `System.Net.Http.HttpClient` výchozí konstruktor. Hodnota je typu sestavení kvalifikovaný název `HttpMessageHandler` podtřídami, vhodné pro použití s [ `System.Type.GetType(string)` ](/dotnet/api/system.type.gettype?view=netcore-2.0#System_Type_GetType_System_String_).

    Výchozí hodnota je `System.Net.Http.HttpClientHandler, System.Net.Http`.

    To může být potlačena za účelem místo toho obsahují `Xamarin.Android.Net.AndroidClientHandler`, který používá rozhraní Android API Java k provedení síťové požadavky. To umožňuje přístup k protokolu TLS 1.2 adresy URL, pokud základní verzi systému Android podporuje TLS 1.2.  
    Pouze Android 5.0 nebo novější spolehlivě poskytuje podporu protokolu TLS 1.2 prostřednictvím Java.

    *Poznámka:*: na Android verze starší než 5.0, se vyžaduje podpora protokolu TLS 1.2 Pokud *nebo* Pokud je to nutné podpora protokolu TLS 1.2 `System.Net.WebClient` a související rozhraní API, pak `$(AndroidTlsProvider)` by měl být použit.

    *Poznámka:*: podpora pro tuto vlastnost funguje tak, že nastavení [ `XA_HTTP_CLIENT_HANDLER_TYPE` proměnnou prostředí](~/android/deploy-test/environment.md).
    A `$XA_HTTP_CLIENT_HANDLER_TYPE` nalezena hodnota v souboru pomocí akce sestavení `@(AndroidEnvironment)` bude mít přednost.

    Přidat v Xamarin.Android 6.1.

-   **AndroidTlsProvider** &ndash; hodnotu řetězce, který určuje TLS poskytovatele, kterého má být použit v aplikaci. Možné hodnoty jsou:

    - `btls`: Použijte [přítomnost SSL](https://boringssl.googlesource.com/boringssl) pro TLS komunikaci s [HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.aspx).
      To umožňuje použití protokolu TLS 1.2 ve všech verzích systému Android.

    - `legacy`: Pomocí historických spravované implementaci SSL pro interakci sítě. To *nemá* podporovala TLS 1.2.

    - `default`: Povolit *Mono* vybrat výchozí poskytovatel TLS.
      Jde o ekvivalent `legacy`, i v Xamarin.Android 7.3.  
      *Poznámka:*: Tato hodnota je nepravděpodobné, že se zobrazí v `.csproj` hodnoty, jako IDE "Výchozí" hodnota má za následek *odebrání* z `$(AndroidTlsProvider)` vlastnost.

    - Nastavení, v prázdný řetězec: V Xamarin.Android 7.1, jde o ekvivalent `legacy`.  
      V Xamarin.Android 7.3, jde o ekvivalent `btls`.

    Výchozí hodnota je prázdný řetězec.

    Přidat v Xamarin.Android 7.1.

-   **AndroidLinkMode** &ndash; Určuje, jaký typ [propojení](~/android/deploy-test/linker.md) je třeba provést na sestavení, které jsou obsažené v balíček Android. Použít pouze v projektech aplikace pro Android. Výchozí hodnota je *SdkOnly*. Platné hodnoty jsou:

    - **Žádný**: žádné propojení se pokusí vytvořit.

    - **SdkOnly**: propojování bude provést na pouze, knihovny základní třída není uživatele sestavení.

    - **Úplné**: propojení se provede na základní třídy knihovny a sestavení uživatele. **Poznámka:** pomocí `AndroidLinkMode` hodnotu *úplné* často vede porušený aplikace, zejména v případě, že se používá reflexe. Pokud jste *skutečně* vědět, jaké úlohy.

    ```xml
    <AndroidLinkMode>SdkOnly</AndroidLinkMode>
    ```

-   **AndroidLinkSkip** &ndash; určuje oddělený středníkem (`;`) seznam sestavení názvů bez přípon souborů, sestavení, která nemá být propojena. Použít pouze v rámci aplikace pro Android projekty.

    ```xml
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
    ```

-   **AndroidManagedSymbols** &ndash; vlastnost typu boolean ovládací prvky zda pořadí body jsou generovány, aby název a řádek číslo informací o souboru lze extrahovat z `Release` trasování zásobníku.

    Přidat v Xamarin.Android 6.1.

-   **AndroidManifest** &ndash; Určuje název souboru má použít jako šablonu pro aplikace [ `AndroidManifest.xml` ](~/android/platform/android-manifest.md).
    Během vytváření sestavení, budou všechny potřebné hodnoty sloučena do k vytvoření skutečnou `AndroidManifest.xml`.
    `$(AndroidManifest)` Musí obsahovat název balíčku v `/manifest/@package` atribut.

-   **AndroidSdkBuildToolsVersion** &ndash; nástroje sestavení balíček Android SDK poskytuje **aapt** a **zipalign** nástroje, mimo jiné. Více různých verzích balíček nástroje sestavení může být nainstalována současně. Balíček nástroje sestavení zvolené pro balení se provádí kontrola a používáte verzi "upřednostňované" nástroje sestavení, pokud je k dispozici; Pokud je "upřednostňované" verze *není* k dispozici, pak se používá nejvyšší balíček verzí nainstalované nástroje sestavení.

    `$(AndroidSdkBuildToolsVersion)` Vlastnosti MSBuild obsahuje verzi upřednostňované nástroje sestavení. Sestavení systému Xamarin.Android poskytuje výchozí hodnotu v `Xamarin.Android.Common.targets`, a výchozí hodnota může být přepsána v souboru projektu zvolte alternativní nástroje sestavení verze, pokud (například) nejnovější aapt selhává se při předchozí verze aapt je zřejmé, fungovat.

-   **AndroidSupportedAbis** &ndash; ve vlastnosti string, který obsahuje středníkem (`;`)-oddělený seznam bis , které mají být zahrnuty do `.apk`.

    Podporované hodnoty:

    -   `armeabi`
    -   `armeabi-v7a`
    -   `x86`
    -   `arm64-v8a`: Vyžaduje Xamarin.Android 5.1 a novějším.
    -   `x86_64`: Vyžaduje Xamarin.Android 5.1 a novějším.

-   **AndroidUseSharedRuntime** &ndash; určuje vlastnost typu boolean, který je jestli *sdílený modul runtime balíčky* jsou potřeba ke spuštění aplikace na cílovém zařízení. Balíček aplikace pro menší, urychlení proces vytvoření a nasazení balíčku, výsledkem je rychlejší cyklus sestavení/nasadit/debug vyřízení spoléhat na balíčky sdílený modul runtime umožňuje.

    Tato vlastnost by měla být `True` pro sestavení pro ladění a `False` pro projekty verze.

-   **AotAssemblies** &ndash; vlastnost typu boolean, která určuje, zda bude sestavení Ahead předčasné zkompilovat do nativního kódu a součástí `.apk`.

    V Xamarin.Android 5.1 přidala se podpora pro tuto vlastnost.

    Tato vlastnost je `False` ve výchozím nastavení.

-   **EmbedAssembliesIntoApk** &ndash; vlastnost typu boolean, která určuje, zda sestavení aplikace měly by být zahrnuty do balíčku aplikace.

    Tato vlastnost by měla být `True` pro verze sestavení a `False` pro sestavení pro ladění. Ho *může* musí být `True` u sestavení ladicí verze, pokud rychlého nasazení nepodporuje cílové zařízení.

    Pokud je tato vlastnost `False`, pak se `$(AndroidFastDeploymentType)` MSBuild vlastnost také určuje, co budou vloženy do `.apk`, který může mít vliv nasazení a sestavte znovu časy.

-   **EnableLLVM** &ndash; vlastnost typu boolean, který určuje, zda LLVM se použije při Ahead dobu kompilace sestavení do nativního kódu.

    V Xamarin.Android 5.1 přidala se podpora pro tuto vlastnost.

    Tato vlastnost je `False` ve výchozím nastavení.

    Tato vlastnost se ignoruje, pokud `$(AotAssemblies)` vlastnosti MSBuild je `True`.

-   **EnableProguard** &ndash; vlastnost typu boolean, která určuje, jestli [proguard](http://developer.android.com/tools/help/proguard.html) se spouští jako součást procesu balení propojení kódu v jazyce Java.

    V Xamarin.Android 5.1 přidala se podpora pro tuto vlastnost.

    Tato vlastnost je `False` ve výchozím nastavení.

    Když `True`, [ProguardConfiguration](#ProguardConfiguration) soubory se použije k řízení `proguard` provádění.

-   **JavaMaximumHeapSize** &ndash; Určuje hodnotu **java** 
     `-Xmx` hodnota parametru pro použití při vytváření `.dex` souboru jako součást procesu vytváření balíčků. Pokud není zadaný, pak se `-Xmx` možnost není k dispozici na **java**.

    Určení Tato vlastnost je nutné Pokud [ `_CompileDex` cíl vrátí `java.lang.OutOfMemoryError` ](https://bugzilla.xamarin.com/show_bug.cgi?id=18327).

    ```xml
    <JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
    ```

-   **JavaOptions** &ndash; Určuje další možnosti příkazového řádku, které mají být předány **java** při sestavování `.dex` souboru.

-   **MandroidI18n** &ndash; Určuje podporu internacionalizace součástí aplikace, jako je například kolace a řazení tabulky. Hodnota je seznam oddělený čárkami nebo středníkem jednoho nebo více z následujících hodnot velká a malá písmena:

    -   **Žádný**: zahrnovat žádné další kódování.

    -   **Všechny**: zahrnují všechny dostupné kódování.

    -   **CJK**: kódování zahrnují čínština, japonština nebo korejština, jako *japonské (EUC)* \[šif jp, CP51932\], *japonština (Shift-JIS)* \[ Posunutí ISO-2022-jp,\_jis, CP932\], *japonské (JIS)* \[CP50220\], *zjednodušená čínština (GB2312)* \[gb2312, CP936\], *korejština (UHC)* \[lokálně\_c\_5601-1987 CP949\], *korejština (EUC)* \[euc-kr, CP51949\], *tradiční čínština (Big5)* \[big5, CP950\], a *zjednodušená čínština (GB18030)* \[ GB18030, CP54936\].

    -   **MidEast**: zahrnují střední-východní kódování jako *turečtina (Windows)* \[iso-8859-9, CP1254\], *Hebrejština (Windows)* \[ Windows-., CP1255 1255\], *Arabština (Windows)* \[windows-1256, CP1256\], *Arabština (ISO)* \[iso-8859-6, CP28596\], *Hebrejština (ISO)* \[iso-8859-8, CP28598\], *Latin 5 (ISO)* \[iso-8859-9, CP28599\]a *Hebrejština (Iso alternativní)* \[iso-8859-8, CP38598\].

    -   **Další**: obsahují další kódování, například *cyrilice (Windows)* \[CP1251\], *Baltského (Windows)* \[iso-8859-4, CP1257\], *Vietnamštině (Windows)* \[CP1258\], *cyrilice (KOI8-R)* \[koi8-r, CP1251\], *ukrajinská (KOI8-U)*  \[koi8-u, CP1251\], *Baltského (ISO)* \[iso-8859-4, CP1257\], *cyrilici (ISO)* \[iso-8859-5, CP1251\], *ISCII Davenagari* \[x-iscii-de, CP57002\], *ISCII bengálština* \[x-iscii se, CP57003 \], *ISCII Tamilské* \[x-iscii tových, CP57004\], *ISCII telugština* \[x-iscii-te CP57005\], *ISCII ásámština* \[x-iscii jako, CP57006\], *ISCII Uríské* \[x-iscii nebo, CP57007\], *ISCII Kannadské* \[x-iscii-ka CP57008\], *ISCII Malajálamské* \[x-iscii-ma CP57009\], *ISCII Gudžarátské* \[x-iscii-gu CP57010\], *ISCII paňdžábština* \[x-iscii-pa CP57011\], a *thajštině (Windows)*  \[CP874\].

    -   **Výjimečných**: zahrnují výjimečných kódování jako *IBM EBCDIC (turečtina)* \[CP1026\], *IBM EBCDIC (otevřete Latin 1 systémy)* \[CP1047\], *IBM EBCDIC (USA a Kanada s Euro)* \[CP1140\], *IBM EBCDIC (Německo s Euro)* \[CP1141\], *IBM EBCDIC (Dánsko/Norsko s Euro)* \[CP1142\], *IBM EBCDIC (Finsko/Švédsko s Euro)* \[CP1143\], *IBM EBCDIC (Itálie s Euro)* \[CP1144\], *IBM EBCDIC (Latinská Amerika a Španělsko s Euro)* \[CP1145\], *IBM EBCDIC (Spojené Království s Euro)* \[CP1146\], *IBM EBCDIC (Francie s Euro)* \[CP1147\], *IBM EBCDIC (mezinárodní s Euro)*  \[CP1148\], *IBM EBCDIC (Island s Euro)* \[CP1149\], *IBM EBCDIC (Německo)* \[ CP20273\], *IBM EBCDIC (Dánsko/Norsko)* \[CP20277\], *IBM EBCDIC (Finsko/Švédsko)* \[CP20278\], *IBM EBCDIC (Itálie)* \[CP20280\], *IBM EBCDIC (Latinská Amerika a Španělsko)* \[CP20284\], *IBM EBCDIC (Česká republika)* \[CP20285\], *IBM EBCDIC (Extended japonské Katakana)* \[CP20290\], *IBM EBCDIC (Francie)* \[CP20297\], *IBM EBCDIC (Arabské)* \[CP20420\], *IBM EBCDIC (hebrejština)* \[CP20424\], *IBM EBCDIC (Island)* \[ CP20871\], *IBM EBCDIC (cyrilice - srbština, bulharština)* \[CP21025\], *IBM EBCDIC (USA a Kanada)* \[ CP37\], *IBM EBCDIC (mezinárodní)* \[CP500\], *Arabština (ASMO 708)* \[CP708\], *Stření Evropa (DOS)* \[CP852\], *cyrilice (DOS)* \[CP855\], *turečtina (DOS)* \[CP857\], *západní Evropa (DOS S Euro)* \[CP858\], *hebrejština (DOS)* \[CP862\], *arabština (DOS)* \[CP864\], *ruština (DOS)* \[CP866\], *řečtina (DOS)* \[CP869\], *IBM EBCDIC (latina 2)* \[CP870\], and *IBM EBCDIC (řečtina)* \[CP875\].

    -   **Západní**: zahrnují západní kódování, jako *západní Evropa (Mac)* \[macintosh, CP10000\], *islandském (Mac)* \[x-mac islandském CP10079\], *Středoevropské jazyky (Windows)* \[iso 8859-2, CP1250\], *západní Evropa (Windows)* \[iso-8859-1 CP1252\], *Řečtina (Windows)* \[iso 8859-7, CP1253\], *centrální jazyky (ISO)* \[iso 8859-2, CP28592\], *Latin 3 (ISO)* \[iso-8859-3, CP28593\], *Řečtina (ISO)* \[iso 8859-7, CP28597\], *Latin 9 (ISO)*  \[iso-8859-15, CP28605\], *OEM Spojených států* \[CP437\], *západní Evropského (DOS)* \[CP850\], *portugalština (DOS)* \[CP860\], *islandském (DOS)* \[CP861\],  *Francouzština (Kanada) (DOS)* \[CP863\], a *severské (DOS)* \[CP865\].


    ```xml
    <MandroidI18n>West</MandroidI18n>
    ```

-   **MonoSymbolArchive** &ndash; vlastnost typu boolean, která určuje, zda `.mSYM` artefakty jsou vytvořené pro pozdější použití s `mono-symbolicate`, k extrakci &ldquo;skutečné&rdquo; a název souboru a řádku číslo informací z Verze trasování zásobníku.

    Toto je True ve výchozím nastavení pro &ldquo;verze&rdquo; aplikace, které mají povolené symboly ladění: `$(EmbedAssembliesIntoApk)` má hodnotu True, `$(DebugSymbols)` má hodnotu True, a `$(Optimize)` má hodnotu True.

    Přidat v Xamarin.Android 7.1.

-   **AndroidVersionCodePattern** &ndash; ve vlastnosti string, který umožňuje vývojáři přizpůsobit `versionCode` v manifestu.
    V tématu [vytváření kód verze pro APK](~/android/deploy-test/building-apps/abi-specific-apks.md) informace týkající se rozhodování `versionCode`.
    
    Některé příklady, pokud `abi` je `armeabi` a `versionCode` v manifestu je `123`, `{abi}{versionCode}` vytvoří versionCode z `1123` při `$(AndroidCreatePackagePerAbi)` má hodnotu True, v opačném případě bude vytvoření hodnoty 123.
    Pokud `abi` je `x86_64` a `versionCode` v manifestu je `44`. Vznikne tak `544` při `$(AndroidCreatePackagePerAbi)` má hodnotu True, v opačném případě bude vytvoření hodnoty `44`.

    Pokud jsme obsahovat left odsazení řetězec formátu `{abi}{versionCode:0000}`, by vytvořit `50044` vzhledem k tomu, že jsme nezbývají odsazení `versionCode` s `0`. Alternativně můžete použít desetinné odsazení, jako `{abi}{versionCode:D4}` která dělá to stejné jako v předchozím příkladu.

    Pouze '0' a 'DirectX odsazení formátu řetězce jsou podporovány, protože hodnota musí být celé číslo.
    
    Předem definované klíčové položky

    -   **ABI** &ndash; vloží abi cílové aplikace  
        -   1 &ndash; `armeabi`
        -   2 &ndash; `armeabi-v7a`
        -   3 &ndash; `x86`
        -   4 &ndash; `arm64-v8a`
        -   5 &ndash; `x86_64`

    -   **minSDK** &ndash; vloží minimální podporovaná hodnota Sdk z `AndroidManifest.xml` nebo `11` Pokud žádný je definována.

    -   **versionCode** &ndash; používá verzi kód přímo z `Properties\AndroidManifest.xml`. 

    Můžete definovat vlastní položky pomocí `$(AndroidVersionCodeProperties)` vlastnosti (definovaná Další).

    Ve výchozím nastavení je možnost Hodnota `{abi}{versionCode:D6}`. Pokud chce vývojář zachovat původní chování můžete přepsat výchozí nastavení `$(AndroidUseLegacyVersionCode)` vlastnosti `true`

    Přidat v Xamarin.Android 7.2.

-   **AndroidVersionCodeProperties** &ndash; ve vlastnosti string, který umožňuje definovat vlastní položky pro použití s vývojáři `AndroidVersionCodePattern`. Jsou ve formě `key=value` pár. Všechny položky v `value` musí být celočíselné hodnoty. Příklad: `screen=23;target=$(_SupportedApiLevel)`. Jak je vidět, můžete provést pomocí nástroje MSBuild existujících nebo vlastních vlastností v řetězci.

    Přidat v Xamarin.Android 7.2.

-   **AndroidUseLegacyVersionCode** &ndash; vlastnost typu boolean bude umožňuje vývojáři vrátit výpočtu versionCode zpět na jeho původní před Xamarin.Android 8.2 chování. Mělo by být použito pouze pro vývojáře se stávajícími aplikacemi v obchodě Google Play. Důrazně doporučujeme, nové `$(AndroidVersionCodePattern)` vlastnost se používá.

    Přidat v Xamarin.Android 8.2.

-  **AndroidUseManagedDesignTimeResourceGenerator** &ndash; vlastnost typu boolean, která se změní v době návrhu sestavení použít analyzátor spravovaných prostředků místo `aapt`.

    Přidat v Xamarin.Android 8.1.

-  **AndroidUseApkSigner** &ndash; bool vlastnost, která umožňuje vývojáři použít k `apksigner` nástroj místo `jarsigner`.

    Přidat v Xamarin.Android 8.2.

-  **AndroidApkSignerAdditionalArguments** &ndash; ve vlastnosti string, který umožňuje vývojáři poskytnout další argumenty, které mají `apksigner` nástroj.

    Přidat v Xamarin.Android 8.2.

### <a name="binding-project-build-properties"></a>Vlastnosti sestavení projektu vazby

Následující vlastnosti nástroje MSBuild se používají s [vazby projekty](~/android/platform/binding-java-library/index.md):

-   **AndroidClassParser** &ndash; ve vlastnosti string, který určuje, jak `.jar` soubory jsou analyzovány. Možné hodnoty patří:

    - **Třída analýzy**: používá `class-parse.exe` analyzovat bajtového kódu Java přímo, bez pomoci JVM. Tato hodnota je experimentální. 


    - **jar2xml**: použijte `jar2xml.jar` používat k extrahování typy a členy z reflexe Java `.jar` souboru.

    Výhody `class-parse` přes `jar2xml` jsou:

    - `class-parse` je možné extrahovat z bajtového kódu Java, který obsahuje názvy parametrů *ladění* symboly (například bajtového kódu kompilovat s `javac -g`).

    - `class-parse` nepodporuje "přeskočení" třídy, které dědí nebo obsahovat členy nepřeložitelný typů.

    **Experimentální**. Přidat v Xamarin.Android 6.0.

    Výchozí hodnota je `jar2xml`.

    Výchozí hodnota se změní v budoucí verzi.

-   **AndroidCodegenTarget** &ndash; ve vlastnosti string, který řídí cíl generování kódu ABI. Možné hodnoty patří:

    - **XamarinAndroid**: používá rozhraní API vazby JNI součástí od Mono pro Android 1.0. Vazba sestavení vytvořené s Xamarin.Android 5.0 nebo novější můžete pouze spouštět na Xamarin.Android 5.0 nebo novější (rozhraní API/ABI dodatky), ale *zdroj* je kompatibilní s dřívější verze produktu.

    - **XAJavaInterop1**: použití Java.Interop pro JNI volání. Vazba sestavení s využitím `XAJavaInterop1` můžete pouze sestavit a spustit s Xamarin.Android 6.1 nebo novější. Xamarin.Android 6.1 a novější vazby `Mono.Android.dll` s touto hodnotou.

      Výhody `XAJavaInterop1` zahrnují:

      - Menší sestavení.

      - `jmethodID` ukládání do mezipaměti pro `base` volání metod, tak dlouho, dokud druhé vazby typy v hierarchii dědičnosti jsou integrované s `XAJavaInterop1` nebo novější.

      - `jmethodID` Obálka volatelná aplikacemi Java konstruktory pro spravované podtřídy ukládání do mezipaměti.

    Výchozí hodnota je `XamarinAndroid`.

    Výchozí hodnota se změní v budoucí verzi.


### <a name="resource-properties"></a>Vlastnosti prostředku

Vlastnosti prostředku řídit generování `Resource.designer.cs` souboru, který poskytuje přístup k prostředkům Android. 

-   **AndroidResgenExtraArgs** &ndash; Určuje další možnosti příkazového řádku, které mají být předány **aapt** příkaz při zpracování Android prostředky a prostředky.

-   **AndroidResgenFile** &ndash; Určuje název souboru prostředků pro generování. Výchozí šablony to nastaví na `Resource.designer.cs`.

-   **MonoAndroidResourcePrefix** &ndash; Určuje *cesta předponu* , se odebere z počátku názvy souborů pomocí akce sestavení `AndroidResource`. Toto je umožnit změnu níž jsou umístěny prostředky.

    Výchozí hodnota je `Resources`. Změňte jej na `res` pro Java projektu struktury.

-   **AndroidExplicitCrunch** &ndash; Pokud vytváříte aplikace s velmi velkým počtem místní drawables, může počáteční sestavení (nebo opětovné sestavení) trvat minut. Pro urychlení procesu sestavení, zkuste včetně této vlastnosti a jeho nastavení na hodnotu `True`. Pokud je tato vlastnost nastavena, proces vytváření předem crunches soubory PNG.

    **Experimentální**. Přidat v Xamarin.Android 7.0.


<a name="Signing_Properties" />

### <a name="signing-properties"></a>Podepisování vlastnosti

Podepisování vlastnosti určují, jak je balíček aplikace podepsaný tak, aby může být nainstalována na zařízení se systémem Android. Umožňuje rychlejší iterace sestavení úlohy Xamarin.Android neregistrujte balíčky během procesu sestavení, protože podpis je velmi pomalé. Místo toho, že jsou podepsané (v případě potřeby) před instalací nebo během exportu, prostředí IDE nebo *nainstalovat* sestavení cíl. Vyvolání *SignAndroidPackage* cíl vytvoří balíček s `-Signed.apk` příponu v výstupnímu adresáři.

Ve výchozím nastavení podpisový cíl generuje nový ladění podpisový klíč v případě potřeby. Pokud chcete použít konkrétní klíč, například na serveru sestavení následující vlastnosti nástroje MSBuild lze použít:

-   **AndroidKeyStore** &ndash; logickou hodnotu označující, zda má být použita vlastní podpisový informace. Výchozí hodnota je `False`, což znamená, že výchozí ladění podpisový klíč se použije k podepisování balíčků.

-   **AndroidSigningKeyAlias** &ndash; Určuje alias pro klíč v úložišti klíčů. Toto je **keytool-alias** hodnotu použít při vytváření úložiště klíčů. 

-   **AndroidSigningKeyPass** &ndash; Určuje heslo klíče v rámci souboru úložiště klíčů. Toto je hodnota zadaná při `keytool` požádá **zadejte heslo klíče pro $(AndroidSigningKeyAlias)**.

-   **AndroidSigningKeyStore** &ndash; Určuje název souboru úložiště klíčů vytvořené `keytool`. To odpovídá hodnotě poskytnuté **keytool - úložiště klíčů** možnost.

-   **AndroidSigningStorePass** &ndash; Určuje heslo pro `$(AndroidSigningKeyStore)`. Toto je hodnota zadaná pro `keytool` při vytváření souboru úložiště klíčů a kladené **zadejte heslo úložiště klíčů:**. 

Například vezměte v úvahu následující `keytool` volání:

```shell
$ keytool -genkey -v -keystore filename.keystore -alias keystore.alias -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password: keystore.filename password
Re-enter new password: keystore.filename password
...
Is CN=... correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: ...
Enter key password for keystore.alias
        (RETURN if same as keystore password): keystore.alias password
[Storing filename.keystore]
```

Pokud chcete použít úložiště klíčů generované výše, použijte vlastnost skupiny:

```xml
<PropertyGroup>
    <AndroidKeyStore>True</AndroidKeyStore>
    <AndroidSigningKeyStore>filename.keystore</AndroidSigningKeyStore>
    <AndroidSigningStorePass>keystore.filename password</AndroidSigningStorePass>
    <AndroidSigningKeyAlias>keystore.alias</AndroidSigningKeyAlias>
    <AndroidSigningKeyPass>keystore.alias password</AndroidSigningKeyPass>
</PropertyGroup>
```

-   **AndroidDebugKeyAlgorithm** &ndash; Určuje výchozí algoritmus, který chcete použít pro `debug.keystore`. Výchozí hodnota `RSA`.

-   **AndroidDebugKeyValidity** &ndash; Určuje výchozí platnosti pro `debug.keystore`. Výchozí hodnota `10950` nebo `30 * 365` nebo `30 years`.

<a name="Build_Actions" />

## <a name="build-actions"></a>Akce sestavení

*Akce sestavení* jsou [použít na soubory](http://msdn.microsoft.com/en-us/library/bb629388.aspx) v rámci projektu a řízení způsobu zpracování souboru. 

<a name="AndroidEnvironment" />

### <a name="androidenvironment"></a>AndroidEnvironment

Soubory pomocí akce sestavení `AndroidEnvironment` se používají pro [inicializace proměnné prostředí a vlastnosti systému během spuštění procesu](~/android/deploy-test/environment.md).
`AndroidEnvironment` Akce sestavení může použít na několik souborů a se vyhodnotí seřazeny (takže nezadávejte stejnou vlastnost proměnnou nebo systému prostředí ve více souborech).


### <a name="androidjavasource"></a>AndroidJavaSource

Soubory pomocí akce sestavení `AndroidJavaSource` jsou Java zdrojového kódu, které budou zahrnuty do konečné balíček Android.


### <a name="androidjavalibrary"></a>AndroidJavaLibrary

Soubory pomocí akce sestavení `AndroidJavaLibrary` jsou Java archivy ( `.jar` soubory) které budou zahrnuty do konečné balíček Android.


### <a name="androidresource"></a>AndroidResource

Všechny soubory s *AndroidResource* akce sestavení se zkompiluje do Android prostředků během procesu vytváření a přístupné prostřednictvím `$(AndroidResgenFile)`.

```xml
<ItemGroup>
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
```

Pokročilejší uživatele může možná chcete mít různé prostředky používané v různých konfiguracích, ale se stejnou cestou efektivní. Toho lze dosáhnout s více prostředků adresářů a souborů pomocí stejné relativní cesty v rámci tyto různé adresáře a pomocí podmínky nástroje MSBuild podmíněně zahrnout různé soubory v různých konfiguracích. Příklad:

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
<ItemGroup  Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug\values\strings.xml"/>
</ItemGroup>
<PropertyGroup>
  <MonoAndroidResourcePrefix>Resources;Resources-Debug<MonoAndroidResourcePrefix>
</PropertyGroup>
```

**LogicalName** &ndash; explicitně určuje cesta prostředku. Umožňuje &ldquo;aliasy&rdquo; soubory tak, aby byla k dispozici jako názvy více různých prostředků.

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources/values/strings.xml"/>
</ItemGroup>
<ItemGroup Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug/values/strings.xml">
    <LogicalName>values/strings.xml</LogicalName>
  </AndroidResource>
</ItemGroup>
```


### <a name="androidnativelibrary"></a>AndroidNativeLibrary

[Nativní knihovny](~/android/platform/native-libraries.md) jsou přidány do sestavení nastavením jejich akce sestavení na `AndroidNativeLibrary`.

Všimněte si, že Android podporuje více aplikací binární rozhraní (bis ), musíte znát systém sestavení, které ABI nativní knihovny je vytvořené pro. To můžete provést dvěma způsoby:

1.  Cesta k "analýzy rozšíření".
2.  Pomocí `Abi` atribut položky.

Pomocí sledování toku dat cesta, název nadřazeného adresáře nativní knihovny slouží k určení ABI, knihovna cíle. Proto pokud přidáte `lib/armeabi/libfoo.so` Build, pak ABI bude možné "zachycení" jako `armeabi`. 


#### <a name="item-attribute-name"></a>Název atributu položky

**ABI** &ndash; určuje ABI nativní knihovny.

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```


### <a name="androidaarlibrary"></a>AndroidAarLibrary

Akce sestavení `AndroidAarLibrary` se má použít pro přímý odkaz .aar soubory. Tato akce sestavení se nejčastěji používá Xamarin součásti. Konkrétně odkazy na soubory .aar, které jsou vyžadované k získání Google Play a dalším službám práce.

Pomocí tohoto sestavení akce považovat podobným způsobem příliš vložené prostředky nalezeny soubory v projektech knihovny. .aar budou extrahovány do adresáře zprostředkující. Potom všechny prostředky, soubory prostředků a .jar bude součástí skupiny odpovídající položky.  

### <a name="content"></a>Obsah

Normální `Content` sestavení akce není podporována (jak jsme nebyly započítáno jak ho podporují bez pravděpodobně nákladná krok při prvním spuštění).

Od verze 5.1 Xamarin.Android, pokus o použití `@(Content)` výsledkem akce sestavení `XA0101` upozornění.

### <a name="linkdescription"></a>LinkDescription

Soubory s *LinkDescription* akce sestavení se používají pro [řízení chování linkeru](~/cross-platform/deploy-test/linker.md).


<a name="ProguardConfiguration" />

### <a name="proguardconfiguration"></a>ProguardConfiguration

Soubory s *ProguardConfiguration* akce sestavení obsahovat možnosti, které se používají k řízení `proguard` chování. Další informace o této akce sestavení najdete v tématu [ProGuard](~/android/deploy-test/release-prep/proguard.md).

Tyto soubory se ignoruje, pokud `$(EnableProguard)` vlastnosti MSBuild je `True`.


## <a name="target-definitions"></a>Definice cílového

Specifické pro Xamarin.Android části procesu sestavení jsou definovány v `$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets`, ale normální cíle pro specifický jazyk, jako *Microsoft.CSharp.targets* se taky požadovat, aby sestavení sestavení.

Následující vlastnosti sestavení musí být nastavená před importem veškerých cílů jazyka:

```xml
<PropertyGroup>
  <TargetFrameworkIdentifier>MonoDroid</TargetFrameworkIdentifier>
  <MonoDroidVersion>v1.0</MonoDroidVersion>
  <TargetFrameworkVersion>v2.2</TargetFrameworkVersion>
</PropertyGroup>
```

Všechny tyto cíle a vlastnosti lze nastavit pro jazyk C# importováním *Xamarin.Android.CSharp.targets*: 

```xml
<Import Project="$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets" />
```

Tento soubor lze snadno přizpůsobit pro jiné jazyky.
