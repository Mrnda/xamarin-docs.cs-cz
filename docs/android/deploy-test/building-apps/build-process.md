---
title: Proces sestavení
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2018
ms.openlocfilehash: bf8dfb43115806f28935c6dec0ebd2d6d7bd2cdc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998235"
---
# <a name="build-process"></a>Proces sestavení


## <a name="overview"></a>Přehled

Proces sestavení Xamarin.Android zodpovídá za všechno dohromady připevnit: [generování `Resource.designer.cs` ](~/android/internals/api-design.md), podpůrné `AndroidAsset`, `AndroidResource`a další [akce sestavení](#Build_Actions), generování [obálek volatelných Android](~/android/platform/java-integration/android-callable-wrappers.md)a generovat `.apk` pro spouštění na zařízeních s Androidem.


## <a name="application-packages"></a>Balíčky aplikací

Široký řečeno, existují dva typy balíčků aplikace pro Android (`.apk` soubory), která mohou generovat sestavovacího systému Xamarin.Android: 

-   **Verze** sestavení, které jsou plně samostatná a nevyžadují další balíčky ke spuštění. Toto jsou balíčky, které by do App storu. 

-   **Ladění** sestavení, které nejsou. 

Není shodou okolností odpovídají MSBuild `Configuration` produkuje balíček.

### <a name="shared-runtime"></a>Sdílený modul Runtime

*Sdílený modul runtime* je pár dalších balíčky pro Android, které poskytují základní knihovny tříd (`mscorlib.dll`atd) a s Androidem vazby knihovny (`Mono.Android.dll`atd.). Ladění sestavení používá sdílený modul runtime namísto včetně základní knihovny tříd a vazby sestavení v rámci balíčku aplikace pro Android umožňuje ladit balíček bude menší.

Sdílený modul runtime může být zakázaná v sestavení pro ladění nastavením `$(AndroidUseSharedRuntime)` vlastnost `False`. 

<a name="Fast_Deployment" />

### <a name="fast-deployment"></a>Rychlé nasazení

*Rychlé nasazení* spolupracuje s sdílený modul runtime pro další zmenšit velikost balíčku aplikace pro Android. To se provádí není sdružování sestavení aplikace v rámci balíčku. Místo toho se zkopírují na cílový prostřednictvím `adb push`. Tento proces zrychluje cyklus sestavení/nasazení/debug, protože pokud *pouze* sestavení se mění, není přeinstalovat balíček. Místo toho jsou pouze aktualizované sestavení znovu synchronizované cílové zařízení. 

Rychlé nasazení se označuje selhání na zařízení, která zablokuje `adb` ze synchronizace do adresáře `/data/data/@PACKAGE_NAME@/files/.__override__`. 

Rychlé nasazení je standardně povolená a můžete zakázat v sestavení ladění tím, že nastavíte `$(EmbedAssembliesIntoApk)` vlastnost `True`.


## <a name="msbuild-projects"></a>Projekty MSBuild

Proces sestavení Xamarin.Android je založená na MSBuild, který je zároveň formát souboru projektu, který se používá sada Visual Studio pro Mac a Visual Studio.
Obvykle by uživatelé nebudou muset ručně upravit soubory MSBuild &ndash; rozhraní IDE vytvoří plně funkční projekty a aktualizuje jakékoliv změny a automatickému vyvolávání cílů pro sestavení podle potřeby. 

Pokročilí uživatelé mohou chtít provádět akce, které nepodporují grafického uživatelského rozhraní IDE, tedy procesu sestavení přizpůsobit úpravou souboru projektu přímo. Tato stránka dokumenty pouze funkce specifické pro Xamarin.Android a přizpůsobení &ndash; mnoho dalších věcí, které je možné s normální MSBuild položky, vlastnosti a cíle. 

<a name="Build_Targets" />

## <a name="build-targets"></a>Sestavení cíle

Následující cíle sestavení jsou definovány pro projekty Xamarin.Android:

-   **Sestavení** &ndash; sestaví balíček.

-   **Vyčištění** &ndash; odebere všechny soubory generované záznamem pro proces sestavení.

-   **Nainstalujte** &ndash; nainstaluje balíček do výchozího zařízení nebo virtuální zařízení.

-   **Odinstalujte** &ndash; Odinstaluje balíček z výchozího zařízení nebo virtuální zařízení.

-   **SignAndroidPackage** &ndash; vytvoří a podepíše balíček (`.apk`). Použití s `/p:Configuration=Release` generovat samostatné balíčky "Verze".

-   **UpdateAndroidResources** &ndash; aktualizace `Resource.designer.cs` souboru. Tento cíl je obvykle volána integrovaným vývojovým prostředím, když do projektu přidají nové prostředky.


## <a name="build-properties"></a>Vlastnosti sestavení

Vlastnosti nástroje MSBuild řídit chování cílů. V jakém jsou uvedeny v souboru projektu, například **MyApp.csproj**, v rámci [MSBuild PropertyGroup – element](https://docs.microsoft.com/visualstudio/msbuild/propertygroup-element-msbuild).

-   **Konfigurace** &ndash; Určuje konfiguraci sestavení, který chcete použít, jako je například "Debug" nebo "Vydané verze". Vlastnost konfigurace slouží k určení výchozí hodnoty pro další vlastnosti, které určují chování cíl. Další konfigurace, které mohou být vytvořeny v rámci prostředí (IDE).

    *Ve výchozím nastavení*, `Debug` konfigurace za následek `Install` a `SignAndroidPackage` cíle vytváření menší balíček pro Android, která vyžaduje přítomnost jiných souborů a balíčků pro provoz.

    Výchozí hodnota `Release` konfigurace za následek `Install` a `SignAndroidPackage` cíle vytváření balíčku Android, která je *samostatné*a můžou se používat bez instalace dalších balíčků nebo soubory.

-   **DebugSymbols** &ndash; logická hodnota, která určuje, zda je balíček Android *laditelný*, v kombinaci s `$(DebugType)` vlastnost. Laditelný balíček obsahuje symboly ladění, nastaví `//application/@android:debuggable` atribut `true`a automaticky přidá `INTERNET` oprávnění tak, aby ladicí program může připojit k procesu. Aplikace je laditelné Pokud `DebugSymbols` je `True` *a* `DebugType` je prázdný řetězec nebo `Full`.

-   **DebugType** &ndash; Určuje [typu symboly ladění](https://docs.microsoft.com/visualstudio/msbuild/csc-task) ke generování jako součást sestavení, která ovlivňuje také, zda je aplikace laditelný. Možné hodnoty:

    - **Úplné**: úplné symboly jsou generovány. Pokud `DebugSymbols` vlastnost MSBuild je také `True`, pak balíček aplikace není laditelný.

    - **PdbOnly**: jsou generovány symboly "PDB". Balíček aplikace se *není* být laditelná.

    Pokud `DebugType` není nastavena nebo je prázdný řetězec, pak bude `DebugSymbols` vlastnost určuje, zda je aplikace laditelný.


### <a name="install-properties"></a>Vlastnosti instalace

Vlastnosti instalace řídit chování `Install` a `Uninstall` cíle.

-   **AdbTarget** &ndash; určuje Android cílové zařízení s Androidem balíčku může nainstalovat nebo odebrání. Hodnota této vlastnosti je stejná jako [ `adb` cílové zařízení možnost](http://developer.android.com/tools/help/adb.html#issuingcommands):

    ```bash
    # Install package onto emulator via -e
    # Use `/Library/Frameworks/Mono.framework/Commands/msbuild` on OS X
    MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
    ```


### <a name="packaging-properties"></a>Vlastnosti vytváření balíčků

Vlastnosti vytváření balíčků řídit vytváření balíčku pro Android a jsou používány `Install` a `SignAndroidPackage` cíle.
[Podepisování vlastnosti](#Signing_Properties) jsou také důležité při balení verzi aplikace.


-   **AndroidApkSigningAlgorithm** &ndash; řetězcovou hodnotu, která určuje podpisový algoritmus pro použití s `jarsigner -sigalg`.

    Výchozí hodnota je `md5withRSA`.

    Přidáno v Xamarin.Android 8.2.

-   **AndroidApplication** &ndash; logická hodnota určující, zda je projekt pro aplikaci pro Android (`True`) nebo pro projekt knihovny pro Android (`False` nebo není k dispozici).

    Pouze jeden projekt s `<AndroidApplication>True</AndroidApplication>` může být k dispozici v rámci balíčku pro Android. (Bohužel to není ještě ověřit, což může vést k chybám zvláštní a současně lákavé týkající se prostředků s Androidem.)

-   **AndroidBuildApplicationPackage** &ndash; logická hodnota, která označuje, jestli se k vytvoření a podepsání balíčku (.apk). Tuto hodnotu nastavíte na `True` je ekvivalentní k použití [SignAndroidPackage](#Build_Targets) cíl sestavení.

    Přidala se podpora pro tuto vlastnost po Xamarin.Android 7.1.

    Tato vlastnost je `False` ve výchozím nastavení.

-   **AndroidEnableMultiDex** &ndash; vlastnost typu boolean určující, zda podpora Multi-Dex se použijí v konečné `.apk`.

    Přidala se podpora pro tuto vlastnost v Xamarin.Android 5.1.

    Tato vlastnost je `False` ve výchozím nastavení.

-   **AndroidEnableSGenConcurrent** &ndash; logická vlastnost, která určuje, zda Mono společnosti [souběžné uvolňování paměti kolekce](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen) se použije.

    Přidala se podpora pro tuto vlastnost v Xamarin.Android 7.2.

    Tato vlastnost je `False` ve výchozím nastavení.

-   **AndroidErrorOnCustomJavaObject** &ndash; vlastnost typu boolean určující, zda může implementovat typy `Android.Runtime.IJavaObject` 
     *bez* také dědí `Java.Lang.Object` nebo `Java.Lang.Throwable`:

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

    V případě hodnoty True, takové typy vygeneruje chybu XA4212, jinak se nevygeneruje XA4212 upozornění.

    Přidala se podpora pro tuto vlastnost v Xamarin.Android 8.1.

    Tato vlastnost je `True` ve výchozím nastavení.

-   **AndroidFastDeploymentType** &ndash; A `:` (dvojtečka) – seznam oddělených hodnot řídit, jaké typy mohou být nasazeny na [adresář pro rychlé nasazení](#Fast_Deployment) na cílovém zařízení při `$(EmbedAssembliesIntoApk)` Vlastnost MSBuild je `False`. Pokud prostředek je rychlé nasazení, je *není* vložen do vytvořeného `.apk`, které můžou urychlit doba nasazení. (Informace, které je rychlé nasazení pak Čím méně často `.apk` musí znovu sestavit a procesu instalace může být rychlejší.) Platné hodnoty jsou:

    - `Assemblies`: Sestavení aplikace nasaďte.

    - `Dexes`: Nasazení `.dex` soubory, prostředky Androidu a Assetů Androidu. **Tuto hodnotu můžete *pouze* použít na zařízeních s Androidem 4.4 nebo novější (rozhraní API 19).**

    Výchozí hodnota je `Assemblies`.

    **Experimentální**. Přidáno v Xamarin.Android 6.1.

-   **AndroidApplicationJavaClass** &ndash; úplný název třídy Java používat místo `android.app.Application` když třída dědí z [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/).

    Tato vlastnost je obvykle nastavena *jiných* vlastnosti, například `$(AndroidEnableMultiDex)` vlastnosti Msbuildu.

    Přidáno v Xamarin.Android 6.1.

-   **AndroidHttpClientHandlerType** &ndash; Určuje výchozí nastavení `System.Net.Http.HttpMessageHandler` implementace, které budou používat `System.Net.Http.HttpClient` výchozí konstruktor. Hodnota je název typu kvalifikovaného pro sestavení z `HttpMessageHandler` podtřídy vhodný pro použití s [ `System.Type.GetType(string)` ](/dotnet/api/system.type.gettype?view=netcore-2.0#System_Type_GetType_System_String_).

    Výchozí hodnota je `System.Net.Http.HttpClientHandler, System.Net.Http`.

    To může být potlačena za účelem místo toho obsahují `Xamarin.Android.Net.AndroidClientHandler`, který používá rozhraní Android API Java k provedení síťové požadavky. Díky přístupu k protokolu TLS 1.2 adresy URL v případě základní verzi systému Android podporuje TLS 1.2.  
    Jenom Android 5.0 nebo novější spolehlivě poskytne podporu protokolu TLS 1.2 pomocí Javy.

    *Poznámka:*: u verze Androidu starší než 5.0, se vyžaduje podpora protokolu TLS 1.2 Pokud *nebo* Pokud je požadován spolu s podpora protokolu TLS 1.2 `System.Net.WebClient` a souvisejících rozhraní API, pak `$(AndroidTlsProvider)` by měla sloužit.

    *Poznámka:*: podpora pro tuto vlastnost funguje tak, že nastavíte [ `XA_HTTP_CLIENT_HANDLER_TYPE` proměnnou prostředí](~/android/deploy-test/environment.md).
    A `$XA_HTTP_CLIENT_HANDLER_TYPE` hodnota nalezena v souboru pomocí akce sestavení `@(AndroidEnvironment)` bude mít přednost.

    Přidáno v Xamarin.Android 6.1.

-   **AndroidTlsProvider** &ndash; řetězcovou hodnotu, která určuje, který zprostředkovatel TLS slouží v aplikaci. Možné hodnoty jsou:

    - `btls`: Použijte v případě [přítomnost SSL](https://boringssl.googlesource.com/boringssl) pro komunikaci TLS s [HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.aspx).
      To umožňuje použití protokolu TLS 1.2 ve všech verzích.

    - `legacy`: Použijte historických spravovanou implementaci protokolu SSL pro interakci sítě. To *nemá* podporovala TLS 1.2.

    - `default`: Povolit *Mono* zvolit výchozí zprostředkovatel TLS.
      Jedná se o ekvivalent k `legacy`, i v Xamarin.Android 7.3.  
      *Poznámka:*: Tato hodnota je nepravděpodobné, že se zobrazí v `.csproj` hodnoty, jako rozhraní IDE "Výchozí" hodnota za následek *odebrání* z `$(AndroidTlsProvider)` vlastnost.

    - Zrušit nastavení nebo na prázdný řetězec: V Xamarin.Android 7.1, jedná se o ekvivalent k `legacy`.  
      V Xamarin.Android 7.3, jde o ekvivalent `btls`.

    Výchozí hodnota je prázdný řetězec.

    Přidáno v Xamarin.Android 7.1.

-   **AndroidLinkMode** &ndash; Určuje, jaký typ [propojení](~/android/deploy-test/linker.md) je třeba provést na sestaveních obsažených v rámci balíčku pro Android. Použít jenom v projektech aplikace pro Android. Výchozí hodnota je *SdkOnly*. Platné hodnoty jsou:

    - **Žádný**: žádné propojení se pokusí vytvořit.

    - **SdkOnly**: propojení se provedla pouze, knihovny základních tříd nejsou uživatele sestavení.

    - **Úplné**: propojení se provede na knihovny základních tříd a uživatelská sestavení. **Poznámka:** použití `AndroidLinkMode` hodnotu *úplné* často vede přerušeno aplikace, zejména pokud se používá reflexi. Pokud jste *skutečně* vědět, co děláte.

    ```xml
    <AndroidLinkMode>SdkOnly</AndroidLinkMode>
    ```

-   **AndroidLinkSkip** &ndash; určuje oddělený středníkem (`;`) seznam názvů sestavení bez přípony souborů, sestavení, které by neměly být propojeny. Použít pouze v projektech aplikace pro Android.

    ```xml
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
    ```

-   **AndroidManagedSymbols** &ndash; vlastnost typu boolean, ovládací prvky, zda body sekvence jsou generovány tak, aby soubor název a informace čísla řádku může být extrahována z `Release` trasování zásobníku.

    Přidáno v Xamarin.Android 6.1.

-   **AndroidManifest** &ndash; Určuje název souboru má použít jako šablony pro aplikace [ `AndroidManifest.xml` ](~/android/platform/android-manifest.md).
    Během sestavení, všechny potřebné hodnoty se sloučí do vytvoří skutečné `AndroidManifest.xml`.
    `$(AndroidManifest)` Musí obsahovat název balíčku v `/manifest/@package` atribut.

-   **AndroidSdkBuildToolsVersion** &ndash; poskytuje nástroje pro vytváření balíčku sada SDK pro Android **aapt** a **zipalign** nástroje, mimo jiné. Současně může být nainstalováno více verzí různých balíčku nástroje sestavení. Balíček nástroje sestavení pro balení se provádí vyhledávání a použití "upřednostňované" nástroje sestavení verze, pokud je k dispozici; Pokud je "upřednostňované" verze *není* k dispozici, pak se používá nejvyšší verze nainstalovaných nástrojů na vytváření balíčku.

    `$(AndroidSdkBuildToolsVersion)` Vlastnost MSBuild obsahuje verzi upřednostňované nástroje sestavení. Sestavení systému Xamarin.Android poskytuje výchozí hodnotu v `Xamarin.Android.Common.targets`, a výchozí hodnota může být přepsána v rámci souboru projektu zvolte verzi alternativní nástroje sestavení, pokud (například) nejnovější aapt dochází k chybám při předchozí verze aapt je známo, fungovat.

-   **AndroidSupportedAbis** &ndash; vlastnost řetězce, který obsahuje středníkem (`;`) – oddělený seznam instrukce ABI, které mají být zahrnuty do `.apk`.

    Mezi podporované hodnoty patří:

    -   `armeabi`
    -   `armeabi-v7a`
    -   `x86`
    -   `arm64-v8a`: Vyžaduje Xamarin.Android 5.1 a novější.
    -   `x86_64`: Vyžaduje Xamarin.Android 5.1 a novější.

-   **AndroidUseSharedRuntime** &ndash; logická vlastnost, která určuje, zda *sdílet balíčky runtime* jsou potřeba ke spuštění aplikace na cílovém zařízení. Spoléhání se na balíčky sdílený modul runtime umožňuje balíček aplikace do menších, zrychlení procesu vytváření a nasazování balíčků, výsledkem je rychlejší cyklus obrátky sestavení/nasazení/debug.

    Tato vlastnost by měla být `True` pro sestavení pro ladění a `False` pro uvolnění projektů.

-   **AotAssemblies** &ndash; vlastnost typu boolean určující, zda budou sestavení Ahead-of-Time kompilovány do nativního kódu a součástí `.apk`.

    Přidala se podpora pro tuto vlastnost v Xamarin.Android 5.1.

    Tato vlastnost je `False` ve výchozím nastavení.

-   **EmbedAssembliesIntoApk** &ndash; vlastnost typu boolean určující, zda sestavení aplikace má být vložen do balíčku aplikace.

    Tato vlastnost by měla být `True` pro buildy vydaných verzí a `False` pro sestavení pro ladění. To *může* musí být `True` v sestavení ladění, pokud rychlé nasazení nepodporuje cílové zařízení.

    Pokud je tato vlastnost `False`, pak bude `$(AndroidFastDeploymentType)` vlastnost MSBuild také určuje, co bude vložena do `.apk`, což může ovlivnit nasazení a znovu sestavte časy.

-   **EnableLLVM** &ndash; vlastnost typu boolean určující, zda kompilátor LLVM se použije při Ahead of Time kompilace sestavení do nativního kódu.

    Přidala se podpora pro tuto vlastnost v Xamarin.Android 5.1.

    Tato vlastnost je `False` ve výchozím nastavení.

    Tato vlastnost se ignoruje, pokud `$(AotAssemblies)` je vlastnost MSBuild `True`.

-   **EnableProguard** &ndash; vlastnost typu boolean určující, zda [proguard](http://developer.android.com/tools/help/proguard.html) je spuštěn jako součást procesu vytváření balíčků k propojení kódu v Javě.

    Přidala se podpora pro tuto vlastnost v Xamarin.Android 5.1.

    Tato vlastnost je `False` ve výchozím nastavení.

    Když `True`, [ProguardConfiguration](#ProguardConfiguration) soubory se použije k řízení `proguard` spuštění.

-   **JavaMaximumHeapSize** &ndash; Určuje hodnotu **java** 
     `-Xmx` hodnota parametru pro použití při vytváření `.dex` souboru jako součást procesu vytváření balíčků. Pokud není zadán, pak bude `-Xmx` možnost není k dispozici na **java**.

    Určení této vlastnosti je nezbytné Pokud [ `_CompileDex` cílit vyvolá výjimku `java.lang.OutOfMemoryError` ](https://bugzilla.xamarin.com/show_bug.cgi?id=18327).

    ```xml
    <JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
    ```

-   **JavaOptions** &ndash; Určuje další možnosti příkazového řádku k předání do **java** při sestavování `.dex` souboru.

-   **MandroidI18n** &ndash; určuje podpory iternacionalizace součástí aplikace, jako je například řazení a řazení tabulek. Hodnota je čárkami nebo středníkem oddělený seznam jednoho nebo více z následujících hodnot velká a malá písmena:

    -   **Žádný**: obsahovat žádné další kódování.

    -   **Všechny**: zahrnují všechny dostupné kódování.

    -   **CJK**: zahrnují čínštině, japonská a Korejská kódování, jako *japonština (EUC)* \[enc-jp, CP51932\], *japonština (Shift-JIS)* \[ ISO-2022-jp, shift\_jis CP932\], *japonština (JIS)* \[CP50220\], *zjednodušená čínština (GB2312)* \[gb2312 CP936\], *korejština (UHC)* \[lokálně\_c\_5601-1987 CP949\], *korejština (EUC)* \[euc-kr, CP51949\], *tradiční čínština (Big5)* \[big5 CP950\], a *zjednodušená čínština (GB18030)* \[ GB18030, CP54936\].

    -   **MidEast**: zahrnují Střední-východ kódování, jako *turečtina (Windows)* \[iso-8859-9, CP1254\], *Hebrejština (Windows)* \[ Windows-., CP1255 1255\], *Arabština (Windows)* \[windows-1256, CP1256\], *Arabština (ISO)* \[iso-8859-6, CP28596\], *Hebrejština (ISO)* \[iso-8859-8, CP28598\], *Latin 5 (ISO)* \[iso-8859-9, CP28599\]a *Hebrejština (Iso alternativní)* \[iso-8859-8, CP38598\].

    -   **Další**: obsahují jiné kódování, například *cyrilice (Windows)* \[CP1251\], *Pobaltské jazyky (Windows)* \[iso-8859-4 CP1257\], *Vietnamština (Windows)* \[CP1258\], *cyrilice (KOI8-R)* \[koi8-r, CP1251\], *ukrajinština (KOI8-U)*  \[koi8-u, CP1251\], *Pobaltské jazyky (ISO)* \[iso-8859-4 CP1257\], *cyrilice (ISO)* \[iso-8859-5, CP1251\], *ISCII Davenagari* \[x-iscii-de, CP57002\], *ISCII – bengálština* \[x-iscii-be, CP57003 \], *ISCII-tamilština* \[x-iscii – ta, CP57004\], *ISCII-telugština* \[x-iscii-te CP57005\], *ISCII-ásámština* \[x-iscii jako CP57006\], *ISCII-urijština* \[x-iscii nebo, CP57007\], *ISCII Kannadština* \[x-iscii-ka CP57008\], *ISCII-malajalámština* \[x-iscii-ma CP57009\], *ISCII Gudžarádština* \[x-iscii-gu CP57010\], *ISCII-pandžábština* \[x-iscii-pa, CP57011\], a *thajština (Windows)*  \[CP874\].

    -   **Výjimečných**: zahrnují výjimečných kódování jako *IBM EBCDIC (turečtina)* \[CP1026\], *IBM EBCDIC (otevřete Latin 1 systémy)* \[CP1047\], *IBM EBCDIC (USA a Kanada s Euro)* \[CP1140\], *IBM EBCDIC (Německo s Euro)* \[CP1141\], *IBM EBCDIC (Dánsko/Norsko s Euro)* \[CP1142\], *IBM EBCDIC (Finsko/Švédsko s Euro)* \[CP1143\], *IBM EBCDIC (Itálie s Euro)* \[CP1144\], *IBM EBCDIC (Latinská Amerika a Španělsko s Euro)* \[CP1145\], *IBM EBCDIC (Spojené Království s Euro)* \[CP1146\], *IBM EBCDIC (Francie s Euro)* \[CP1147\], *IBM EBCDIC (mezinárodní s Euro)*  \[CP1148\], *IBM EBCDIC (Island s Euro)* \[CP1149\], *IBM EBCDIC (Německo)* \[ CP20273\], *IBM EBCDIC (Dánsko/Norsko)* \[CP20277\], *IBM EBCDIC (Finsko/Švédsko)* \[CP20278\], *IBM EBCDIC (Itálie)* \[CP20280\], *IBM EBCDIC (Latinská Amerika a Španělsko)* \[CP20284\], *IBM EBCDIC (Česká republika)* \[CP20285\], *IBM EBCDIC (Extended japonské Katakana)* \[CP20290\], *IBM EBCDIC (Francie)* \[CP20297\], *IBM EBCDIC (Arabské)* \[CP20420\], *IBM EBCDIC (hebrejština)* \[CP20424\], *IBM EBCDIC (Island)* \[ CP20871\], *IBM EBCDIC (cyrilice - srbština, bulharština)* \[CP21025\], *IBM EBCDIC (USA a Kanada)* \[ CP37\], *IBM EBCDIC (mezinárodní)* \[CP500\], *Arabština (ASMO 708)* \[CP708\], *Stření Evropa (DOS)* \[CP852\], *cyrilice (DOS)* \[CP855\], *turečtina (DOS)* \[CP857\], *západní Evropa (DOS S Euro)* \[CP858\], *hebrejština (DOS)* \[CP862\], *arabština (DOS)* \[CP864\], *ruština (DOS)* \[CP866\], *řečtina (DOS)* \[CP869\], *IBM EBCDIC (latina 2)* \[CP870\], and *IBM EBCDIC (řečtina)* \[CP875\].

    -   **Západní**: západní zahrnují kódování, jako *Západoevropské jazyky (Mac)* \[macintosh CP10000\], *Islandština (Mac)* \[x-mac islandština CP10079\], *Středoevropské jazyky (Windows)* \[iso-8859-2, CP1250\], *Západoevropské jazyky (Windows)* \[iso-8859-1 CP1252\], *Řečtina (Windows)* \[iso-8859-7, CP1253\], *centrální jazyky (ISO)* \[iso-8859-2, CP28592\], *Latinky 3 (ISO)* \[iso-8859-3 CP28593\], *Řečtina (ISO)* \[iso-8859-7, CP28597\], *latinky 9 (ISO)*  \[iso-8859-15, CP28605\], *OEM-USA* \[CP437\], *západní Evropského (DOS)* \[CP850\], *portugalština (DOS)* \[CP860\], *Islandština (DOS)* \[CP861\],  *Kanadská francouzština (DOS)* \[CP863\], a *severské jazyky (DOS)* \[CP865\].


    ```xml
    <MandroidI18n>West</MandroidI18n>
    ```

-   **MonoSymbolArchive** &ndash; logická vlastnost, která určuje, zda `.mSYM` artefakty jsou vytvořeny pro pozdější použití s `mono-symbolicate`, pro extrakci &ldquo;skutečné&rdquo; název souboru a řádku číslo informací z Verze trasování zásobníku.

    To je PRAVDA ve výchozím nastavení pro &ldquo;vydání&rdquo; aplikace, které mají povolené symboly ladění: `$(EmbedAssembliesIntoApk)` má hodnotu True, `$(DebugSymbols)` má hodnotu True, a `$(Optimize)` má hodnotu True.

    Přidáno v Xamarin.Android 7.1.

-   **AndroidVersionCodePattern** &ndash; vlastnosti typu string, který umožňuje vývojářům pro přizpůsobení `versionCode` v manifestu.
    V tématu [vytváření kód verze pro APK](~/android/deploy-test/building-apps/abi-specific-apks.md) informace o tom `versionCode`.
    
    Některé příklady, pokud `abi` je `armeabi` a `versionCode` v manifestu je `123`, `{abi}{versionCode}` vytvoří versionCode z `1123` při `$(AndroidCreatePackagePerAbi)` má hodnotu True, jinak bude výsledkem hodnota 123.
    Pokud `abi` je `x86_64` a `versionCode` v manifestu je `44`. Vznikne `544` při `$(AndroidCreatePackagePerAbi)` má hodnotu True, jinak bude výsledkem hodnota z `44`.

    Pokud zahrnujeme vlevo vyplňující řetězec formátu `{abi}{versionCode:0000}`, byste mohli vytvořit `50044` protože jsme se odsazení `versionCode` s `0`. Alternativně můžete použít desetinné čárky, jako například odsazení `{abi}{versionCode:D4}` která dělá to samé jako v předchozím příkladu.

    Pouze "0" a Dx formát řetězce jsou podporovány, protože hodnota odsazení musí být celé číslo.
    
    Předem definované klíčové body

    -   **ABI** &ndash; vloží cílové abi pro aplikaci  
        -   1 &ndash; `armeabi`
        -   2 &ndash; `armeabi-v7a`
        -   3 &ndash; `x86`
        -   4 &ndash; `arm64-v8a`
        -   5 &ndash; `x86_64`

    -   **minSDK** &ndash; vloží minimální podporovaná hodnota sady Sdk z `AndroidManifest.xml` nebo `11` Pokud není definován.  

    -   **versionCode** &ndash; používá verzi kódu přímo z `Properties\AndroidManifest.xml`. 

    Můžete definovat vlastní položky pomocí `$(AndroidVersionCodeProperties)` vlastnosti (definované dále).

    Ve výchozím nastavení hodnota bude nastavena `{abi}{versionCode:D6}`. Pokud si vývojář chce uchovávat staré chování můžete přepsat výchozí nastavení `$(AndroidUseLegacyVersionCode)` vlastnost `true`

    Přidáno v Xamarin.Android 7.2.

-   **AndroidVersionCodeProperties** &ndash; vlastnosti typu string, který umožňuje vývojářům k definování vlastní položky pro použití s `AndroidVersionCodePattern`. Jsou ve formě `key=value` pár. Všechny položky v `value` musí být celočíselné hodnoty. Příklad: `screen=23;target=$(_SupportedApiLevel)`. Jak je vidět, můžete využít nástroje MSBuild, zvolte existující nebo vlastní vlastnosti v řetězci.

    Přidáno v Xamarin.Android 7.2.

-   **AndroidUseLegacyVersionCode** &ndash; vlastnost typu boolean bude umožňuje vývojářům vrátit výpočtu versionCode zpět do její původní pre Xamarin.Android 8.2 chování. To by měla sloužit pouze pro vývojáře se stávajícími aplikacemi v Google Play Store. Důrazně doporučujeme, který nový `$(AndroidVersionCodePattern)` vlastnost se používá.

    Přidáno v Xamarin.Android 8.2.

-  **AndroidUseManagedDesignTimeResourceGenerator** &ndash; logická vlastnost, která bude přejít v době návrhu sestavení pro použití analyzátoru spravovaný prostředek spíše než `aapt`.

    Přidáno v Xamarin.Android 8.1.

-  **AndroidUseApkSigner** &ndash; bool vlastnost, která umožňuje vývojářům používat na `apksigner` nástroj místo `jarsigner`.

    Přidáno v Xamarin.Android 8.2.

-  **AndroidApkSignerAdditionalArguments** &ndash; řetězcovou vlastnost, která umožňuje vývojářům zadat další argumenty pro `apksigner` nástroj.

    Přidáno v Xamarin.Android 8.2.

### <a name="binding-project-build-properties"></a>Vazby vlastnosti sestavení projektu

Následující vlastnosti nástroje MSBuild se používají s [projekty vazby](~/android/platform/binding-java-library/index.md):

-   **AndroidClassParser** &ndash; řetězcovou vlastnost, která určuje, jak `.jar` soubory jsou analyzovány. Možné hodnoty:

    - **Třída analýzy**: používá `class-parse.exe` přímo analyzovat bajtový kód Java bez pomoci JVM. Tato hodnota je experimentální. 


    - **jar2xml**: použijte `jar2xml.jar` pomocí reflexe Javy extrahovat typy a členy z `.jar` souboru.

    Výhody `class-parse` přes `jar2xml` jsou:

    - `class-parse` dokáže extrahovat názvy parametrů z bajtového kódu Javy obsahující *ladění* symboly (například bajtový kód zkompilovaný s `javac -g`).

    - `class-parse` "nepřeskočí" třídy, které dědí z členů nebo obsahují členy nerozpoznatelných typů.

    **Experimentální**. Přidáno v Xamarin.Android 6.0.

    Výchozí hodnota je `jar2xml`.

    Výchozí hodnota se změní v budoucích vydáních.

-   **AndroidCodegenTarget** &ndash; vlastnosti řetězce, které řídí cíl generování kódu ABI. Možné hodnoty:

    - **XamarinAndroid**: používá součástí od Mono JNI vazby pro rozhraní API pro Android 1.0. Vazba sestavení vytvořená pomocí Xamarin.Android 5.0 nebo novější můžete spouštět v Xamarin.Android 5.0 nebo novější (rozhraní API/ABI dodatky), jenom ale *zdroj* je kompatibilní s dřívější verze produktu.

    - **XAJavaInterop1**: použití Java.Interop pro volání JNI. Vazba sestavení pomocí `XAJavaInterop1` lze pouze sestavit a spustit s Xamarin.Android 6.1 nebo vyšší. Xamarin.Android 6.1 a novější vazby `Mono.Android.dll` s touto hodnotou.

      Výhody `XAJavaInterop1` patří:

      - Menší sestavení.

      - `jmethodID` ukládání do mezipaměti pro `base` volání metod, tak dlouho, dokud druhé vazby typů v hierarchii dědičnosti jsou vybaveny `XAJavaInterop1` nebo novější.

      - `jmethodID` Obálka volatelná aplikacemi Java konstruktory pro spravované podtřídy ukládání do mezipaměti.

    Výchozí hodnota je `XamarinAndroid`.

    Výchozí hodnota se změní v budoucích vydáních.


### <a name="resource-properties"></a>Vlastnosti prostředku

Vlastnosti prostředku řídit generování `Resource.designer.cs` soubor, který poskytuje přístup k prostředkům s Androidem. 

-   **AndroidResgenExtraArgs** &ndash; Určuje další možnosti příkazového řádku k předání **aapt** příkaz při zpracování assetů Androidu a prostředky.

-   **AndroidResgenFile** &ndash; Určuje název souboru prostředků pro generování. Nastaví tuto výchozí šablonu `Resource.designer.cs`.

-   **MonoAndroidResourcePrefix** &ndash; Určuje *předpona cesty* , který se odebere ze začátku názvů souborů pomocí akce sestavení `AndroidResource`. Toto je umožnit změnu, kde se prostředky nacházejí.

    Výchozí hodnota je `Resources`. Změňte tuto hodnotu na `res` struktura projektu jazyka Java.

-   **AndroidExplicitCrunch** &ndash; Pokud vytváříte aplikace s velmi velkým počtem místní drawables, minut na dokončení může trvat počáteční sestavení (nebo opětovné sestavení). Ke zrychlení procesu sestavení, zkuste včetně tuto vlastnost a nastavíte ho na `True`. Pokud je tato vlastnost nastavena, proces sestavení předem crunches soubory ve formátu PNG.

    **Experimentální**. Přidáno v Xamarin.Android 7.0.


<a name="Signing_Properties" />

### <a name="signing-properties"></a>Podepisování vlastnosti

Podepisování vlastností ovládacího prvku, jak je balíček aplikace podepsaný tak, aby ji můžou instalovat na zařízení s Androidem. Povolit rychlejší sestavení iterace, úlohy Xamarin.Android uživatel balíčky během procesu sestavení, protože podpis je velmi pomalé. Místo toho, že jsou podepsané (v případě potřeby) před zahájením instalace nebo během exportu, rozhraní IDE nebo *nainstalovat* cíl sestavení. Volání *SignAndroidPackage* cílové vytvoří balíček se `-Signed.apk` přípona ve výstupním adresáři.

Ve výchozím nastavení podepisování cíl vygenerování nového klíče podpisu ladění, v případě potřeby. Pokud chcete použít konkrétní klíč, například na serveru sestavení, následující vlastnosti nástroje MSBuild lze použít:

-   **AndroidKeyStore** &ndash; logickou hodnotu označující, zda má být použita vlastní podpisové informace. Výchozí hodnota je `False`, což znamená, že výchozí ladění podpisový klíč se použije k podepisování balíčků.

-   **AndroidSigningKeyAlias** &ndash; Určuje alias klíče v keystore. Toto je **keytool – alias** hodnotu použitou při vytváření úložiště klíčů. 

-   **AndroidSigningKeyPass** &ndash; Určuje heslo klíče v rámci souboru úložiště klíčů. Jedná se o hodnotu zadat při `keytool` požádá **zadejte heslo klíče pro $(AndroidSigningKeyAlias)**.

-   **AndroidSigningKeyStore** &ndash; Určuje název souboru úložiště klíčů vytvořené `keytool`. To odpovídá hodnotě k dispozici na **keytool - úložiště klíčů** možnost.

-   **AndroidSigningStorePass** &ndash; Určuje heslo pro `$(AndroidSigningKeyStore)`. Toto je hodnota zadaná pro `keytool` při vytváření souboru úložiště klíčů a ke správě **zadejte heslo úložiště klíčů:**. 

Představte si třeba následující `keytool` vyvolání:

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

Úložiště klíčů generované výše, použijte vlastnost skupiny:

```xml
<PropertyGroup>
    <AndroidKeyStore>True</AndroidKeyStore>
    <AndroidSigningKeyStore>filename.keystore</AndroidSigningKeyStore>
    <AndroidSigningStorePass>keystore.filename password</AndroidSigningStorePass>
    <AndroidSigningKeyAlias>keystore.alias</AndroidSigningKeyAlias>
    <AndroidSigningKeyPass>keystore.alias password</AndroidSigningKeyPass>
</PropertyGroup>
```

-   **AndroidDebugKeyAlgorithm** &ndash; Určuje výchozí algoritmus pro `debug.keystore`. Použije se výchozí `RSA`.

-   **AndroidDebugKeyValidity** &ndash; Určuje výchozí platnosti pro `debug.keystore`. Použije se výchozí `10950` nebo `30 * 365` nebo `30 years`.

<a name="Build_Actions" />

## <a name="build-actions"></a>Akce sestavení

*Akce sestavení* jsou [použít na soubory](https://docs.microsoft.com/visualstudio/msbuild/common-msbuild-project-items) v rámci projektu a jak se zpracovávají soubor ovládacího prvku. 

<a name="AndroidEnvironment" />

### <a name="androidenvironment"></a>AndroidEnvironment

Soubory pomocí akce sestavení `AndroidEnvironment` se používají pro [inicializaci proměnných prostředí a systémové vlastnosti během spuštění procesu](~/android/deploy-test/environment.md).
`AndroidEnvironment` Akci sestavení může použít na několik souborů a vyhodnocují bez určitého pořadí (takže nezadávejte stejnou vlastnost proměnné nebo systém prostředí ve více souborech).


### <a name="androidjavasource"></a>AndroidJavaSource

Soubory pomocí akce sestavení `AndroidJavaSource` jsou zdrojový kód v Javě, který bude zahrnut ve finálním balíčku s Androidem.


### <a name="androidjavalibrary"></a>AndroidJavaLibrary

Soubory pomocí akce sestavení `AndroidJavaLibrary` jsou archivy Java ( `.jar` soubory) která bude součástí poslední balíček pro Android.


### <a name="androidresource"></a>AndroidResource

Všechny soubory *AndroidResource* akci sestavení jsou zkompilovány do Android prostředků během procesu sestavení a přístupné přes `$(AndroidResgenFile)`.

```xml
<ItemGroup>
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
```

Pokročilejší uživatele může být možná chcete mít různé prostředky používané v různých konfiguracích, ale se stejnou cestou efektivní. Toho lze dosáhnout s více prostředků adresáře a soubory se stejnými relativními cestami v rámci těchto různých adresářích a použití podmínky nástroje MSBuild podmíněně zahrnout různé soubory v různých konfiguracích. Příklad:

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

**LogicalName** &ndash; explicitně určuje cestu prostředku. Umožňuje &ldquo;aliasing&rdquo; soubory tak, že budou k dispozici jako názvy více různých zdrojů.

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

[Nativní knihovny](~/android/platform/native-libraries.md) jsou přidány do sestavení, nastavením jejich akci sestavení na `AndroidNativeLibrary`.

Všimněte si, že vzhledem k tomu, že Android podporuje více aplikačních binárních rozhraní (ABI), sestavovací systém musíte znát ABI, které je nativní knihovna sestavena pro. Existují dva způsoby, jak se to dá udělat:

1.  Cesta "analýzy rozšíření".
2.  Použití `Abi` atribut položky.

Pomocí cesty pro analýzu sítě, název nadřazené adresáře nativní knihovny slouží k určení ABI, které cíle představující knihovny. Proto pokud chcete přidat `lib/armeabi/libfoo.so` k sestavení, pak ABI bude možné "zachycení" jako `armeabi`. 


#### <a name="item-attribute-name"></a>Název položky atributu

**ABI** &ndash; určuje ABI nativní knihovnu.

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```


### <a name="androidaarlibrary"></a>AndroidAarLibrary

Akce sestavení `AndroidAarLibrary` by měla sloužit k přímému odkazování .aar soubory. Tato akce sestavení se nejčastěji používá komponenty Xamarin. Konkrétně zahrnoval reference na .aar soubory, které jsou požadovány pro Google Play a dalších služeb.

S tímto sestavením akce bude zacházet podobným způsobem příliš vložené prostředky nalezeny soubory v projektech knihovny. .aar budou extrahovány do zprostředkujícím adresáři. Potom všechny prostředky, soubory prostředků a .jar bude obsahovat příslušnou položku skupiny.  

### <a name="content"></a>Obsah

Normální `Content` akci sestavení se nepodporuje (jak nebyly napadlo nás, jak podporovat bez může být nákladné krok při prvním spuštění).

Od verze 5.1 Xamarin.Android, pokus o použití `@(Content)` bude mít za následek akci sestavení `XA0101` upozornění.

### <a name="linkdescription"></a>LinkDescription

Soubory s *LinkDescription* akci sestavení se používají pro [řídit chování linkeru](~/cross-platform/deploy-test/linker.md).


<a name="ProguardConfiguration" />

### <a name="proguardconfiguration"></a>ProguardConfiguration

Soubory s *ProguardConfiguration* akci sestavení obsahují možnosti, které se používají k řízení `proguard` chování. Další informace o této akci sestavení naleznete v tématu [ProGuard](~/android/deploy-test/release-prep/proguard.md).

Tyto soubory se ignorovaly, pokud `$(EnableProguard)` je vlastnost MSBuild `True`.


## <a name="target-definitions"></a>Definice cílového

Specifické pro Xamarin.Android částí procesu sestavení, které jsou definovány v `$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets`, ale normální jazykově specifické cíle jako *Microsoft.CSharp.targets* jsou také nutné k vytvoření sestavení.

Následující vlastnosti sestavení musí být nastavena před importem cílů libovolný jazyk:

```xml
<PropertyGroup>
  <TargetFrameworkIdentifier>MonoDroid</TargetFrameworkIdentifier>
  <MonoDroidVersion>v1.0</MonoDroidVersion>
  <TargetFrameworkVersion>v2.2</TargetFrameworkVersion>
</PropertyGroup>
```

Všechny tyto cíle a vlastnosti metodou importování, půjdou zahrnout pro jazyk C# *Xamarin.Android.CSharp.targets*: 

```xml
<Import Project="$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets" />
```

Tento soubor lze snadno upravit pro jiné jazyky.
