---
title: "Příprava aplikace pro vydání"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9C8145B3-FCF1-4649-8C6A-49672DDA4159
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2018
ms.openlocfilehash: baaa40bc89a1ca6728189563c8350f9c9f011762
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="preparing-an-application-for-release"></a>Příprava aplikace pro vydání


Po aplikaci je programového a otestovat, je nezbytné pro přípravu balíček pro distribuci. První úlohou Příprava tento balíček je vytvořit aplikaci pro verzi, která především zahrnuje nastavení některých atributů aplikací.

Použijte následující postup k vytvoření aplikace pro verze:

-   **[Zadejte ikona aplikace](#Specify_the_Application_Icon)**  &ndash; aplikace pro Xamarin.Android každý musí mít zadaný ikony aplikace. I když technicky nezbytné, vyžadují některé trhy, jako je například Google Play ho.

-   **[Verze aplikace](#Versioning)**  &ndash; tento krok zahrnuje inicializace nebo aktualizuje informace o správě verzí. To je důležité pro budoucí aplikace aktualizace a zajistit, že uživatelé jsou vědět, kterou verzi aplikace jste nainstalovali.

-   **[Zmenšit APK](#shrink_apk)**  &ndash; velikost poslední APK se může podstatně snížit pomocí linkeru Xamarin.Android na spravovaný kód a ProGuard na bajtového kódu Java.

-   **[Ochrana aplikace](#protect_app)**  &ndash; uživatele nebo útočníkům zabránit ladění, zneužití nebo zpětnou analýzu zakázáním ladění, zmatení data spravovaného kódu, přidání proti ladění a proti zfalšování, aplikace a pomocí nativní kompilace.

-   **[Nastavení vlastností balení](#Set_Packaging_Properties)**  &ndash; vlastností balení řídí vytváření balíčku aplikace pro Android (APK). Tento krok optimalizuje APK, chrání jeho prostředky a modularizes balení podle potřeby.

-   **[Kompilace](#Compile)**  &ndash; tento krok kompilovaný kód a prostředky k ověření, že sestavení v režimu vydání.

-   **[Archivu pro publikování](#archive)**  &ndash; tento krok sestavení aplikace a umístí jej do archiv pro registraci a publikování.

Každá z těchto kroků je popsána níže podrobněji.

<a name="Specify_the_Application_Icon" />

## <a name="specify-the-application-icon"></a>Zadejte ikona aplikace

Důrazně doporučujeme, aby každá aplikace Xamarin.Android určení ikony aplikace. Některé aplikace tržišť neumožní aplikace platformy Android k publikování bez jeden. `Icon` Vlastnost `Application` atribut slouží k určení ikony aplikace pro Xamarin.Android projekt.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V sadě Visual Studio 2015 a novější, zadejte na ikonu aplikace prostřednictvím **Android Manifest** části projektu **vlastnosti**, jak je znázorněno na následujícím snímku obrazovky:

[![Nastavit ikona aplikace](images/vs/01-application-icon-sml.png)](images/vs/01-application-icon.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V sadě Visual Studio pro Mac, je také možné zadat ikonu aplikace prostřednictvím **aplikace pro Android** části **možnosti projektu**, jak je znázorněno na následujícím snímku obrazovky:

[![Nastavit ikona aplikace](images/xs/01-application-icon-sml.png)](images/xs/01-application-icon.png#lightbox)

-----

V těchto příkladech `@drawable/icon` odkazuje na soubor ikony, který se nachází v **Resources/drawable/icon.png** (Všimněte si, že **.png** rozšíření není součástí názvu prostředku). Tento atribut lze deklarovat také v souboru **Properties\AssemblyInfo.cs**, jak je znázorněno v této ukázkové fragmentu kódu:

```csharp
[assembly: Application(Icon = "@drawable/icon")]
```

Za normálních okolností `using Android.App` je deklarován v horní části **AssemblyInfo.cs** (obor názvů `Application` atribut je `Android.App`), ale, budete muset přidat to `using` příkaz, pokud již není přítomen.


<a name="Versioning" />

## <a name="version-the-application"></a>Verze aplikace

Správa verzí je důležité pro aplikace pro Android údržby a distribuci. Bez nějaká správu verzí v místě je obtížné zjistit, jestli nebo způsob aktualizace aplikace. Jako pomoc s Správa verzí, rozpozná Android dva různé typy informací: 

-   **Číslo verze** &ndash; celočíselná hodnota (používá se interně Android a aplikace), která představuje verzi aplikace. Většina aplikací začít s Tato hodnota nastavena na hodnotu 1, a pak se zvýší s každé sestavení. Tato hodnota nemá žádný vztah nebo spřažení s názvem atributu verze (viz níže). Tato hodnota by neměla má uživatelům zobrazit aplikace a publikování služby. Tato hodnota je uložena v **AndroidManifest.xml** souboru jako `android:versionCode`. 

-   **Název verze** &ndash; řetězec, který se používá pouze pro komunikaci informace pro uživatele o verzi aplikace (jako je nainstalované na konkrétní zařízení). Název verze je určena k zobrazení na uživatele nebo na webu Google Play. Tento řetězec se používá interně Android. Název verze může být libovolnou hodnotu řetězce, které by pomoci určit sestavení, který je nainstalován na jejich zařízení uživatele. Tato hodnota je uložena v **AndroidManifest.xml** souboru jako `android:versionName`. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V sadě Visual Studio, tyto hodnoty lze nastavit v **Android Manifest** části projektu **vlastnosti**, jak je znázorněno na následujícím snímku obrazovky:

[![Nastavit číslo verze](images/vs/02-versioning-sml.png)](images/vs/02-versioning.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Tyto hodnoty lze nastavit prostřednictvím **sestavení > aplikace pro Android** části **možnosti projektu** jak je znázorněno na následujícím snímku obrazovky:

[![Nastavit číslo verze](images/xs/02-versioning-sml.png)](images/xs/02-versioning.png#lightbox)

-----

<a name="shrink_apk" />

## <a name="shrink-the-apk"></a>Zmenšit APK

Můžete provést Xamarin.Android APKs menší pomocí kombinace linkeru Xamarin.Android, která odebere nepotřebné *spravované* kód a *ProGuard* nástroj ze sady Android SDK, který odebere nepoužívané *bajtového kódu Java*. Procesu sestavení nejprve používá linkeru Xamarin.Android za účelem optimalizace aplikace na úrovni spravovaného kódu (C#) a pak ho později používá ProGuard (Pokud je povoleno) k optimalizaci APK na úrovni bajtového kódu Java.


### <a name="configure-the-linker"></a>Konfigurace Linkeru

Verze režimu vypne sdílený modul runtime a spustí propojení tak, aby aplikace jenom dodává kusy Xamarin.Android požadované za běhu. *Linkeru* v Xamarin.Android pomocí statické analýzy určuje, které sestavení, typy a členy typu jsou použít nebo odkazuje aplikace pro Xamarin.Android. Linkeru pak zahodí všechny nepoužívané sestavení, typy a členy, kteří nejsou používá (nebo je odkazované). Výsledkem může být k výraznému snížení velikost balíčku. Představte si třeba [HelloWorld](~/android/deploy-test/linker.md) vzorku, ke které dojde 83 % snížení konečné velikosti o jeho APK: 

-   Konfigurace: Žádné &ndash; Xamarin.Android 4.2.5 velikost = 17.4 MB.

-   Konfigurace: Sestavení SDK pouze &ndash; Xamarin.Android 4.2.5 velikost = 3.0 MB.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Nastavení možností linkeru prostřednictvím **Android možnosti** části projektu **vlastnosti**:

[![Možnosti linkeru](images/vs/03-linking-sml.png)](images/vs/03-linking.png#lightbox)

**Propojování** rozevírací nabídce poskytuje následující možnosti pro řízení linkeru:

-   **Žádný** &ndash; tím dojde k vypnutí linkeru; žádné propojení bude provedena.

-   **Pouze sestavení sady SDK** &ndash; to pouze odkaz sestavení, které jsou [vyžadovanou Xamarin.Android](~/cross-platform/internals/available-assemblies.md). 
    Nebudou propojené ostatních sestavení.

-   **Sadu SDK a sestavení uživatele** &ndash; propojí všechny sestavení, které jsou požadované aplikací a nikoli pouze ty, které vyžadují Xamarin.Android.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Nastavení možností linkeru prostřednictvím **Linkeru** ve **Android sestavení** části **možnosti projektu**, jak je znázorněno na následujícím snímku obrazovky:

[![Možnosti linkeru](images/xs/03-linking-sml.png)](images/xs/03-linking.png#lightbox)

Možnosti pro řízení linkeru jsou následující:

-   **Nemáte odkaz** &ndash; tím dojde k vypnutí linkeru; žádné propojení bude provedena.

-   **Odkaz sestavení SDK pouze** &ndash; to pouze odkaz sestavení, které jsou [vyžadovanou Xamarin.Android](~/cross-platform/internals/available-assemblies.md). Nebudou propojené ostatních sestavení.

-   **Odkaz ve všech sestaveních** &ndash; propojí všechny sestavení, které jsou požadované aplikací a nikoli pouze ty, které vyžadují Xamarin.Android.

-----

Propojování může vytvářet nezamýšleným vedlejší účinky, proto je důležité, aby aplikace znovu testovány v režimu vydání na fyzické zařízení.


### <a name="proguard"></a>ProGuard

*ProGuard* je nástroj pro Android SDK, který odkazuje a zastírá kódu v jazyce Java. ProGuard se obvykle používá k vytvoření menší aplikací, protože snižuje nároky na velké zahrnuté knihoven (například služby Google Play) ve vaší APK. ProGuard odebere nepoužívané bajtového Java kódu, díky čemuž výsledná aplikace menší. Například lze dosáhnout pomocí ProGuard na malé aplikace Xamarin.Android obvykle o 24 % snížení velikosti &ndash; pomocí ProGuard na větší aplikace s více závislostí knihovny obvykle dosahuje ještě lepší zmenšení velikosti. 

ProGuard není alternativa k linkeru Xamarin.Android. Odkazy linkeru Xamarin.Android *spravované* kód při ProGuard odkazy bajtového kódu Java. Procesu sestavení nejprve používá linkeru Xamarin.Android k optimalizaci spravovaného (C#) kód v aplikaci a pak ho později používá ProGuard (Pokud je povoleno) k optimalizaci APK na úrovni bajtového kódu Java. 

Když **povolit ProGuard** je zaškrtnuto, Xamarin.Android spustí nástroj ProGuard na výsledný APK. ProGuard konfigurační soubor je generovat a používat ProGuard v čase vytvoření buildu. Xamarin.Android také podporuje vlastní *ProguardConfiguration* akce sestavení. Můžete přidat vlastní ProGuard konfigurační soubor do projektu, pravým tlačítkem myši a vyberte jako akce sestavení, jak je uvedeno v následujícím příkladu: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Akce proguard sestavení](images/vs/05-proguard-build-action-sml.png)](images/vs/05-proguard-build-action.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Akce proguard sestavení](images/xs/05-proguard-build-action-sml.png)](images/xs/05-proguard-build-action.png#lightbox)

-----

Ve výchozím nastavení vypnutá ProGuard. **Povolit ProGuard** možnost je dostupná pouze, když je projekt nastavená na **verze** režimu. Všechny akce ProGuard sestavení se ignoruje, pokud **povolit ProGuard** je zaškrtnuté. Xamarin.Android ProGuard konfigurace není obfuskováním APK a není možné povolit maskováním, i když vlastní konfigurační soubory. Pokud chcete použít maskováním, najdete v tématu [Ochrana aplikace s Dotfuscatoru](~/android/deploy-test/release-prep/index.md#dotfuscator). 

Podrobné informace o používání nástroje ProGuard, najdete v části [ProGuard](~/android/deploy-test/release-prep/proguard.md).

<a name="protect_app" />

## <a name="protect-the-application"></a>Ochrana aplikace

<a name="Disable_Debugging" />

### <a name="disable-debugging"></a>Zakázat ladění

Během vývoje aplikace platformy Android, ladění se provádí s použitím *Java ladění síťový protokol* (JDWP). Toto je technologie, která umožňuje nástroje, jako **adb** ke komunikaci s JVM pro účely ladění. JDWP je zapnutá ve výchozím nastavení pro sestavení pro ladění aplikace pro Xamarin.Android. Při JDWP je důležité při vývoji, může představovat problém zabezpečení pro vydané aplikace. 

> [!IMPORTANT]
> Je možné (prostřednictvím JDWP) získat úplný přístup k procesu Java a spustit libovolný kód v kontextu aplikace, pokud není tento stav ladění zakázán vždy zakážete ladění stavu vydané aplikace.

Android Manifest obsahuje `android:debuggable` atribut, který určuje, zda může ladit aplikace. Bude považován za vhodné nastavit `android:debuggable` atribut `false`. Nejjednodušší způsob, jak to udělat, je přidání příkazu podmíněné kompilace v **AssemblyInfo.cs**: 

```csharp
#if DEBUG
[assembly: Application(Debuggable=true)]
#else
[assembly: Application(Debuggable=false)]
#endif
```

Všimněte si, že ladicí sestavení automaticky nastavit některá oprávnění, aby snazší ladění (například **Internet** a **ReadExternalStorage**). Verze sestavení, ale použít jenom oprávnění, která explicitně nakonfigurovat. Pokud zjistíte, že přepnutí na sestavení pro vydání způsobí, že aplikaci ztratíte oprávnění, která byla k dispozici v sestavení ladicí verze, ověřte, že je výslovně povoleno toto oprávnění v **požadovaná oprávnění** seznamu, jak je popsáno v [ Oprávnění](~/android/app-fundamentals/permissions.md). 
 

<a name="dotfuscator" id="dotfuscator" />

### <a name="application-protection-with-dotfuscator"></a>Ochrana aplikací pomocí Dotfuscatoru

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

I když [zakázáno ladění](#Disable_Debugging), je stále možné pro útočníky znovu zabalit aplikace, přidání nebo odebrání možnosti konfigurace nebo oprávnění. To jim umožňuje provádět zpětnou analýzu, ladění a manipulovat s aplikací.
[Dotfuscatoru Community Edition (CE)](https://www.preemptive.com/products/dotfuscator/overview) slouží k obfuskováním spravovaného kódu a vložit runtime bezpečnostní stav detekce kód do aplikace Xamarin.Android v čase vytvoření buildu.

Dotfuscatoru CE je obsažen v sadě Visual Studio, ale pouze Visual Studio 2015 Update 3 (a vyšší) má správná verze pro práci s Xamarin.Android. Chcete-li použít Dotfuscatoru, klikněte na tlačítko **nástroje > preemptivní ochrana – Dotfuscatoru**.

Pokud chcete konfigurovat Dotfuscatoru CE, najdete v tématu [pomocí Dotfuscatoru Community Edition s Xamarinem](https://www.preemptive.com/obfuscating-xamarin-with-dotfuscator).
Jakmile se nakonfiguruje, Dotfuscatoru CE automaticky chránit každé sestavení, který je vytvořen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

I když [zakázáno ladění](#Disable_Debugging), je stále možné pro útočníky znovu zabalit aplikace, přidání nebo odebrání možnosti konfigurace nebo oprávnění. To jim umožňuje provádět zpětnou analýzu, ladění a manipulovat s aplikací.
I když nepodporuje sady Visual Studio pro Mac, můžete použít [Dotfuscatoru Community Edition (CE)](https://www.preemptive.com/products/dotfuscator/overview) pomocí sady Visual Studio obfuskováním spravovaného kódu a vložit runtime bezpečnostní stav detekce kód do aplikace Xamarin.Android v okamžiku sestavení .

Pokud chcete konfigurovat Dotfuscatoru CE, najdete v tématu [pomocí Dotfuscatoru Community Edition s Xamarinem](https://www.preemptive.com/obfuscating-xamarin-with-dotfuscator).
Jakmile se nakonfiguruje, Dotfuscatoru CE automaticky chránit každé sestavení, který je vytvořen.

-----

<a name="bundle" />

### <a name="bundle-assemblies-into-native-code"></a>Sestavení sady do nativního kódu

Pokud je tato možnost povolena, sestavení jsou seskupeny do nativní sdílenou knihovnu. Tato možnost udržuje kód bezpečné; Spravovaná sestavení chrání jejich vložením do nativní binární soubory.

Tato možnost vyžaduje licenci Enterprise a je k dispozici pouze při **použití rychlého nasazení** je zakázána. **Sady sestavení do nativního kódu** ve výchozím nastavení vypnutá.

Všimněte si, že **sady do nativního kódu** nemá možnost *není* znamená, že sestavení se zkompiluje do nativního kódu. Není možné použít [ **AOT kompilace** ](#aot) kompilace sestavení do nativního kódu (aktuálně pouze povolenými experimentálními funkci a ne pro produkční použití).

<a name="aot" />

### <a name="aot-compilation"></a>AOT kompilace

**AOT kompilace** možnost (na [balení vlastnosti](#Set_Packaging_Properties) stránky) umožňuje Ahead předčasné (AOT) kompilace sestavení. Pokud je tato možnost povolena, pouze v čas (JIT) Režijní náklady na spuštění minimalizovat předkompilaci sestavení před modulu runtime. Výsledný nativního kódu je součástí APK společně s nezkompilované sestavení. Výsledek v kratší doba spuštění aplikace, ale za cenu mírně větší velikosti APK.

**AOT kompilace** možnost vyžaduje licenci Enterprise nebo vyšší. **Kompilace AOT** je k dispozici pouze, když projekt je nakonfigurovaný pro režimu vydání, a ve výchozím nastavení vypnutá. Další informace o kompilace AOT najdete v tématu [AOT](http://www.mono-project.com/docs/advanced/aot/).


#### <a name="llvm-optimizing-compiler"></a>Optimalizace kompilátoru LLVM

_LLVM optimalizace kompilátoru_ vytvořit menší a rychlejší zkompilovaný kód a převést zkompilovat AOT sestavení do nativního kódu, ale v expense o něco pomalejší sestavení časy. Kompilátor LLVM ve výchozím nastavení vypnutá. Použít kompilátor LLVM **AOT kompilace** nejprve musí být povolena možnost (na [balení vlastnosti](#Set_Packaging_Properties) stránky).


> [!NOTE]
> **LLVM optimalizace kompilátoru** možnost vyžaduje licenci firmy.  

<a name="Set_Packaging_Properties" />

## <a name="set-packaging-properties"></a>Nastavení vlastností balení

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Balení vlastnosti lze nastavit v **Android možnosti** části projektu **vlastnosti**, jak je znázorněno na následujícím snímku obrazovky:

[![Balení vlastnosti](images/vs/04-packaging-sml.png)](images/vs/04-packaging.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Balení vlastnosti lze nastavit v **možnosti projektu**, jak je znázorněno na následujícím snímku obrazovky:

[![Balení vlastnosti](images/xs/04-packaging-sml.png)](images/xs/04-packaging.png#lightbox)

-----

Mnoho z těchto vlastností, jako například **použití sdílených Runtime**, a **použití rychlého nasazení** jsou určené pro režim ladění. Pokud aplikace je nakonfigurovaná pro verzi režim, jsou ale další nastavení, které určují, jak je aplikace [optimalizované pro velikost a provádění rychlost](#shrink_apk), [jak je chráněná před neoprávněnou manipulací](#protect_app)a jak může být zabalené pro podporu různé architektury a omezení velikosti.


### <a name="specify-supported-architectures"></a>Zadat podporované architektury

Příprava aplikace Xamarin.Android pro verzi, je nutné zadat architektury procesoru, které jsou podporovány. Jeden APK může obsahovat počítače kódu, které podporují víc různé architektury. V tématu [architektury procesoru](~/android/app-fundamentals/cpu-architectures.md) podrobnosti o podpoře více architekturami procesoru.


### <a name="generate-one-package-apk-per-selected-abi"></a>Generovat jeden balíček (. APK) podle vybrané ABI

Pokud je tato možnost povolena, jeden APK se vytvoří pro každou z podporovaných ABI (vybrané na **Upřesnit** kartě, jak je popsáno v [architektury procesoru](~/android/app-fundamentals/cpu-architectures.md)) místo jedné, velké APK pro všechny podporované ABI společnosti. Tato možnost je dostupná, pouze pokud projekt je nakonfigurován pro režim vydání a ve výchozím nastavení vypnutá.


### <a name="multi-dex"></a>Multi-Dex

Když **povolit více Dex** je povolena možnost, sady SDK pro Android nástroje se používají k vynechat metoda limit 65 KB **.dex** formát souboru. Metoda omezení na 65 kB je založen na řadě metod Java, aplikace _odkazy_ (včetně těch v žádné knihovny, které závisí aplikace na) &ndash; není založena na počtu metody, které jsou _napsané v zdrojový kód_. Pokud aplikace jenom definuje několik metod ale používá mnoho (nebo velké knihovny), je možné, že bude překročen limit 65 kB.

Je možné, že aplikace není v každé knihovny, který se odkazuje; pomocí every – metoda Proto je možné, že nástroje, jako je ProGuard (viz výše) můžete odebrat nepoužité metody z kódu. Osvědčeným postupem je umožnit **povolit více Dex** pouze pokud je to nezbytně nutné i.e.the aplikace stále odkazuje víc než 65 kB Java metody i po použití ProGuard.

Další informace o více Dex najdete v tématu [konfigurace aplikace s přes 64 tisíc metody](http://developer.android.com/tools/building/multidex.html).

<a name="Compile" />

## <a name="compile"></a>Kompilace

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po dokončení všechny výše uvedené kroky aplikace je připravená pro kompilaci. Vyberte **sestavení > znovu sestavit řešení** k ověření, že úspěšně sestavuje v režimu vydání. Všimněte si, že tento krok ještě nevytváří APK.

[Při podpisu balíčku aplikace](~/android/deploy-test/signing/index.md) popisuje balení a podepisování podrobněji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po dokončení všechny výše uvedené kroky kompilace aplikace (vyberte **sestavení > sestavení všechny**) Chcete-li ověřit, že úspěšně sestavuje v režimu vydání. Všimněte si, že tento krok ještě nevytváří APK.

-----


<a name="archive" />

## <a name="archive-for-publishing"></a>Archiv pro publikování

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chcete-li zahájit proces publikování, klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **archivu...**  položky kontextové nabídky:

[![Archiv aplikace](images/vs/07-archive-for-publishing-sml.png)](images/vs/07-archive-for-publishing.png#lightbox)

**Archiv...**  spustí **Manager archivu** a zahájí se proces archivace sady prostředků aplikace, jak je vidět na tomto snímku obrazovky:

[![Správce archivu](images/vs/08-archive-manager-sml.png)](images/vs/08-archive-manager.png#lightbox)

Jiný způsob, jak vytvořit archivu je pravým tlačítkem na řešení v **Průzkumníku řešení** a vyberte **archivu všechny...** , kde jsou vytvářeny řešení a archivy všechny projekty Xamarin, které mohou generovat archivu:

[![Všechny archivu](images/vs/09-archive-all-sml.png)](images/vs/09-archive-all.png#lightbox)


Obě **archivu** a **archivu všechny** automaticky spustit **Manager archivu**. Ke spuštění **Manager archivu** přímo, klikněte na tlačítko **nástroje > archivu Manager...**  položku nabídky:

[![Spusťte správce archivu](images/vs/10-launch-archive-manager-sml.png)](images/vs/10-launch-archive-manager.png#lightbox)

Toto řešení archivy kdykoli kliknutím pravým tlačítkem **řešení** uzlu a výběrem **zobrazení archivy**:

[![Zobrazit archivy](images/vs/11-view-archives-sml.png)](images/vs/11-view-archives.png#lightbox)

### <a name="the-archive-manager"></a>Správce archivu

**Manager archivu** se skládá z **řešení seznamu** podokně **archivy seznamu**a **Panel podrobnosti o**:

[![Podokna Manager archivu](images/vs/12-archive-manager-detail-sml.png)](images/vs/12-archive-manager-detail.png#lightbox)

**Řešení seznamu** zobrazí všechna řešení s projektem alespoň jeden archivovaný. **Řešení seznamu** obsahuje následující oddíly:

* **Aktuální řešení** &ndash; zobrazí aktuální řešení. Všimněte si, že tato oblast může být prázdný, pokud aktuální řešení nemá stávajícího archivu.
* **Všechny archivy** &ndash; zobrazí všechna řešení, které mají archivu.
* **Hledání** textového pole (nahoře) &ndash; filtry řešení uvedené v **všechny archivy** seznam podle zadané v textovém poli hledaný řetězec.

**Archivy seznamu** zobrazí seznam všech archivy vybraného řešení. **Archivy seznamu** obsahuje následující oddíly:

* **Název vybraného řešení** &ndash; zobrazí název řešení vybraný v **řešení seznamu**. Všechny informace zobrazené v **archivy seznamu** odkazuje na tato vybraného řešení.
* **Filtr platformy** &ndash; toto pole umožňuje filtrovat archivy podle typu platformy (třeba iOS nebo Android).
* **Archivovat položky** &ndash; seznam archivy vybraného řešení. Každá položka v tomto seznamu zahrnuje název projektu, datum vytvoření a platformu. Pokud položku se může také zobrazit další informace, jako např. proces publikování nebo archivovat.

**Panel podrobnosti o** zobrazí další informace o jednotlivých archivu. Také umožňuje uživateli spustit pracovní postup distribuce nebo otevřete složku, kde byl vytvořen rozdělení. **Vytvoření komentáře** části umožňuje zahrnout komentáře sestavení v archivu.

### <a name="distribution"></a>Distribuce

Když archivované verze aplikace je připravena k publikování, vyberte archivu v **Manager archivu** a klikněte na tlačítko **distribuci...**  tlačítko:

[![Tlačítko distribuovat](images/vs/13-distribute-sml.png)](images/vs/13-distribute.png#lightbox)

**Distribuční kanál** dialogové okno zobrazuje informace o aplikaci, údaje o průběhu pracovního postupu distribuční a možnost výběru distribučních kanálů. Při prvním spuštění uvádíme dvě možnosti:

[![Vyberte distribuční kanál](images/vs/14-distribution-channel-sml.png)](images/vs/14-distribution-channel.png#lightbox)

Je možné zvolit jednu z následujících distribučních kanálů:

* **Ad Hoc** &ndash; uloží podepsaný APK na disk, který může být zkušebně načtené na zařízení s Androidem. Nadále [podpisu balíčku aplikace](~/android/deploy-test/signing/index.md) Pokud chcete informace o vytvoření Android podepisování identity, vytvořte nový podpisový certifikát pro aplikace pro Android a publikovat _ad hoc_ verze aplikace na disk. To je dobrý způsob, jak vytvořit APK pro testování.

* **Google Play** &ndash; publikuje podepsaný APK na web Google Play. Nadále [publikování na webu Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md) se dozvíte, jak k podepisování a publikovat APK v obchodě Google Play.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Chcete-li zahájit proces publikování, vyberte **sestavení > archivu pro publikování**:

[![Archiv pro publikování](images/xs/07-archive-for-publishing-sml.png)](images/xs/07-archive-for-publishing.png#lightbox)

**Archiv pro publikování** vytvoří projekt a obsahuje ureitou ho do souboru archivu. **Archivu všechny** volbu nabídky archivy všechny archivovat projekty v řešení. Obě možnosti automaticky otevře **Manager archivu** při dokončení operace sestavení a sdružování:

[![Zobrazení archivu](images/xs/08-archives-view-sml.png)](images/xs/08-archives-view.png#lightbox)

V tomto příkladu **Manager archivu** uvádí jen jeden archivovat aplikace, **Moje aplikace**. Všimněte si, že pole Poznámka umožňuje krátký komentář uložit s archivu. K publikování archivované verze aplikace pro Xamarin.Android, vyberte aplikaci, v **Manager archivu** a klikněte na tlačítko **přihlášení a distribuci...**  jako v příkladu nahoře. Výsledná **přihlášení a distribuci** dialogové okno zobrazí dvě možnosti:

[![Podepsání a distribuci](images/xs/09-sign-and-distribute-sml.png)](images/xs/09-sign-and-distribute.png#lightbox)


Zde je možné vybrat distribuční kanál:

-   **Ad Hoc** &ndash; uloží podepsaný APK na disk, aby ho bylo možné zkušebně načtené na zařízení s Androidem. Nadále [podpisu balíčku aplikace](~/android/deploy-test/signing/index.md) Pokud chcete informace o vytvoření Android podepisování identity, vytvořte nový podpisový certifikát pro aplikace pro Android a publikovat &ldquo;ad hoc&rdquo; verze aplikace na disk. To je dobrý způsob, jak vytvořit APK pro testování.


-   **Google Play** &ndash; publikuje podepsaný APK na web Google Play.
    Nadále [publikování na webu Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md) se dozvíte, jak k podepisování a publikovat APK v obchodě Google Play.

-----


## <a name="related-links"></a>Související odkazy

- [Zařízení s více jádry a Xamarin.Android](~/android/deploy-test/multicore-devices.md)
- [Architektury procesorů](~/android/app-fundamentals/cpu-architectures.md)
- [AOT](http://www.mono-project.com/docs/advanced/aot/)
- [Zmenšit kód a prostředky](http://developer.android.com/tools/help/proguard.html)
- [Konfigurace aplikací s více než 64 tisíc metody](http://developer.android.com/tools/building/multidex.html)
