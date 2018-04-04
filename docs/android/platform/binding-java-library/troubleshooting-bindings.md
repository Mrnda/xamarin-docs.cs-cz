---
title: Řešení potíží s vazby
description: Tento článek obsahuje souhrn ke schválení běžné chyby, které mohou nastat při vytváření vazby, spolu s možné příčiny a navrhované způsoby, jak je vyřešit.
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: da6286eed091114c117c723f462bbb8cac77034b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting-bindings"></a>Řešení potíží s vazby

_Tento článek obsahuje souhrn ke schválení běžné chyby, které mohou nastat při vytváření vazby, spolu s možné příčiny a navrhované způsoby, jak je vyřešit._


## <a name="overview"></a>Přehled

Vazba knihovna pro Android ( **.aar** nebo **.jar**) soubor je málokdy přehledné záležitost; obvykle vyžaduje další úsilí zmírnit problémy, které jsou výsledkem rozdíly mezi Java a rozhraní .NET.
Tyto problémy budou zabránit Xamarin.Android vazby knihovna pro Android a chovají jako chybové zprávy v protokolu sestavení. Tento průvodce zadejte tipy pro řešení potíží s problémy, seznam některých běžných problémů nebo scénáře a zadejte možná řešení úspěšně vazby knihovna pro Android.

Při vytváření vazby existující Android knihovny, je třeba mít na paměti následující body:

- **Vnější závislosti pro knihovnu** &ndash; Any Java závislosti požadované knihovna pro Android musí být zahrnutý v projektu Xamarin.Android jako **ReferenceJar** nebo jako  **EmbeddedReferenceJar**.

- **Úroveň rozhraní API systému Android, že knihovna pro Android je zaměřen** &ndash; ji není možné "downgradovat" úroveň rozhraní API systému Android; Ujistěte se, že vazba projektu Xamarin.Android cílí stejné rozhraní API úrovně (nebo vyšší) jako knihovna pro Android.

- **Verze Android JDK, která byla použita pro balíček Android knihovny** &ndash; chybám vazby může dojít, pokud byla knihovna pro Android vytvořené v jiné verzi JDK než ten, který používá Xamarin.Android. Pokud je to možné znovu zkompiluje knihovna pro Android pomocí stejnou verzi nástroje JDK, který je používán instalace Xamarin.Android.

Prvním krokem k řešení potíží s vazbou knihovnu Xamarin.Android je umožnit [výstup diagnostiky MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).
Po povolení výstup diagnostiky, Xamarin.Android vazby projekt znovu sestavte a vyhledejte v protokolu sestavení najít co je příčinou problému, který následuje.

Může být také užitečná dekompilaci knihovna pro Android a podívejte se na typy a metody, které Xamarin.Android se pokouší vytvořit vazbu. To je podrobněji později v tomto průvodci.


## <a name="decompiling-an-android-library"></a>Decompiling knihovna pro Android

Probíhá kontrola třídy a metody třídy Java může poskytnout cenné informace, která vám pomůže při vytvoření vazby knihovny.
[JD grafickým uživatelským rozhraním](http://jd.benow.ca/) je grafický nástroj, který může zobrazit zdrojový kód Java z **třída** soubory obsažené v JAR. Spuštěním jako samostatný aplikace nebo jako modul plug-in pro IntelliJ nebo Eclipse.

Dekompilovat otevřenou knihovna pro Android **. JAR** soubor s Java decompiler. Pokud je knihovna **. AAR** soubor, je nezbytné, aby se extrahoval soubor **classes.jar** ze souboru archivu. Tady je ukázka snímek použití JD grafického uživatelského rozhraní k analýze [Picasso](http://square.github.io/picasso/) JAR:

![Použití Java Decompiler k analýze picasso 2.5.2.jar](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Jakmile budete mít decompiled knihovna pro Android, zkontrolujte zdrojový kód. Obecně řečeno vyhledejte:

- **Třídy, které mají charakteristické vlastnosti třídy maskováním** &ndash; vlastnosti třídy zkomolené patří:

    - Název třídy obsahuje **$**, tj. **$.class**
    - Název třídy je zcela ohrožení zabezpečení z malých písmen, tj. **a.class**      

- **`import` příkazy pro neregistrované knihovny** &ndash; Identifikujte neregistrované knihovny a přidejte do projektu Xamarin.Android vazba s těchto závislostí **akce sestavení** z **ReferenceJar**  nebo **EmbedddedReferenceJar**.

> [!NOTE]
> Knihovna Java decompiling může zakázáno nebo vztahují právní omezení podle místních zákonů nebo licenci, ve kterém byla publikována knihovna Java. V případě potřeby zahrnovat služby právní professional před pokusem o dekompilaci knihovna Java a prozkoumejte zdrojový kód.


## <a name="inspect-apixml"></a>Kontrola rozhraní API. XML

Jako součást sestavení projektu vazby, Xamarin.Android vygeneruje název souboru XML **obj/Debug/api.xml**:

![Vygenerovaný api.xml pod obj/Debug](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

Tento soubor obsahuje seznam všech rozhraní API Java, Xamarin.Android se pokouší vazby. Obsah tohoto souboru může pomoci identifikovat všechny chybějící typy nebo metod, duplicitní vazbu. I když kontroly tohoto souboru je zdlouhavé a časově náročné, může poskytovat pro různá vodítka na co může způsobovat problémy vazby. Například **api.xml** může odhalit to, že vlastnost vrací typ nevhodný nebo jsou dva typy tato sdílená složka se stejným názvem spravované.


## <a name="known-issues"></a>Známé problémy

Této části jsou uvedeny některé běžné chybové zprávy nebo příznaky, Moje dojít při pokusu o navázání knihovna pro Android.


### <a name="problem-java-version-mismatch"></a>Problém: Java verze.

Někdy se nevygeneruje typy nebo dojde k neočekávané chybě může dojít, protože používáte buď novější nebo starší verzi ve srovnání s co knihovny bylo kompilováno s Java. Znovu zkompiluje knihovna pro Android se stejnou verzí JDK, který používá projekt Xamarin.Android.


### <a name="problem-at-least-one-java-library-is-required"></a>Problém: alespoň jeden Java vyžaduje se knihovna

Se zobrazí chyba "nejméně jedna knihovna Java je povinné," Přestože. JAR byla přidána.

#### <a name="possible-causes"></a>Možné příčiny:

Ujistěte se, že akce sestavení je nastavena na `EmbeddedJar`. Vzhledem k tomu, že existují různé akce sestavení pro. JAR soubory (například `InputJar`, `EmbeddedJar`, `ReferenceJar` a `EmbeddedReferenceJar`), generátor vazby nelze snadno uhodnout automaticky používané ve výchozím nastavení. Další informace o akcích sestavení najdete v tématu [akce sestavení](~/android/platform/binding-java-library/index.md).


### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>Problém: Vytvoření vazby nástroje nelze načíst. Knihovna JAR

Generátor knihovny vazby nepodaří načíst. Knihovna JAR.

#### <a name="possible-causes"></a>Možné příčiny

Některé. JAR knihoven, které použít kód maskováním (prostřednictvím nástrojů, jako je Proguard), nelze načíst pomocí nástroje Java. Vzhledem k tomu, že naše nástroj umožňuje použití reflexe Java a kód bajtů ASM technici knihovny, může tyto závislé nástroje odmítnout zkomolené knihovny, zatímco Android runtime nástrojů může předat. Alternativní řešení pro tuto je ruční bind tyto knihovny místo použití generátor vazby.



### <a name="problem-missing-c-types-in-generated-output"></a>Problém: Chybí typy C# v generovaný výstup.

Vazba **.dll** sestavení ale nezdařených přístupů některé typy Java nebo z důvodu chyby s oznámením, jsou chybějící typy nelze sestavit generovaných zdrojových C#.

#### <a name="possible-causes"></a>Možné příčiny:

Této chybě může dojít z několika důvodů, jak je uvedeno dále:

-   Knihovna je svázán, může odkazovat na druhý knihovna Java. Pokud veřejné rozhraní API pro knihovnu vázané používá typy z druhé knihovny, musíte odkázat spravované vazby pro druhý knihovny.

-   Je možné, že byl knihovnu vložit z důvodu reflexe Java, podobně jako důvod Chyba načtení knihovny výše, způsobuje neočekávané načítání metadat. Nástroje pro Xamarin.Android nelze vyřešit aktuálně tuto situaci. V takovém případě musí být ručně vázaný knihovny.

-   Modul runtime rozhraní .NET 4.0, který se nepodařilo načíst sestavení, pokud by měl mít obsahoval chyby. Tento problém byl opraven v modulu runtime rozhraní .NET 4.5.

-   Java umožňuje odvozování veřejnou třídu od třídy neveřejný, ale to není podporována v rozhraní .NET. Vzhledem k tomu, že vazba generátor negeneruje vazby pro neveřejný třídy, odvozené třídy, jako je to nemůže být generována správně. Chcete-li odstranit tento problém, buď odeberte položku metadata pro tyto odvozené třídy pomocí uzlu odebrat v **Metadata.xml**, nebo odstraňte metadata, která je zajistit, že třída neveřejný veřejné. I když druhé řešení vytvoří vazbu tak, aby se sestavení C# zdroje, není vhodné používat třídu neveřejný.

    Příklad:

    ```xml
    <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
        name="visibility">public</attr>
    ```

-   Nástroje, které obfuskováním Java knihovny může narušovat generátor vazby Xamarin.Android a schopnost vygenerovat obálku třídy jazyka C#. Následující fragment kódu ukazuje, jak aktualizovat **Metadata.xml** k unobfuscate název třídy:

    ```xml
    <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
        name="obfuscated">false</attr>
    ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>Problém: Generovaného C# zdroje nelze sestavit. kvůli neshodě typů parametrů

Nelze sestavit generovaných zdrojových C#. Přepsat parametr metody, které typy se neshodují.

#### <a name="possible-causes"></a>Možné příčiny:

Xamarin.Android zahrnuje celou řadu Java pole, které jsou mapované na výčty ve vazbách C#. Typ problémům s kompatibilitou tyto může způsobit v generované vazby. Tento problém vyřešili, metoda podpisů vytvořené z vazby generátor muset upravit pro použití výčty. Další imformation, najdete v tématu [opravách výčty](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md).

### <a name="problem-noclassdeffounderror-in-packaging"></a>Problém: NoClassDefFoundError v balíčku

`java.lang.NoClassDefFoundError` v kroku balení, je vyvolána.

#### <a name="possible-causes"></a>Možné příčiny:

Nejpravděpodobnější příčiny této chyby je, že musí být přidán do projektu aplikace povinné knihovna Java (**.csproj**). . Souborů JAR nejsou vyřešit automaticky. Vazba knihovna Java nevygenerovala vždy proti uživatelského sestavení, která neexistuje v cílovém zařízení nebo emulátoru (jako je například Google Maps **maps.jar**). To tak není pro podporu projektu Knihovna pro Android, jako knihovny. JAR vložené do knihovny dll. Příklad: [4288 chyb](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>Problém: Duplicitní vlastní typy EventArgs

Sestavení se nezdaří z důvodu duplicitní vlastní typy EventArgs. Dojde k chybě takto:

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>Možné příčiny:

Toto je z důvodu některých konfliktu mezi typy událostí, které pocházejí z více než jeden typ rozhraní "naslouchací proces", který sdílí metod s identické názvy. Například pokud existují dvě rozhraní Java, jak je vidět v následujícím příkladu, generátor vytvoří `DismissScreenEventArgs` pro obě `MediationBannerListener` a `MediationInterstitialListener`, což je chyba.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

Jedná se o návrhu tak, aby se tak předejde zdlouhavé názvy na typy argumentů událostí. Konfliktům, některé transformaci metadat je požadovaná. Upravit [ **Transforms\Metadata.xml** ](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml) a přidejte `argsType` atribut na některý z rozhraní (nebo na metodu rozhraní):

```xml
<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationBannerListener']/method[@name='onDismissScreen']"
        name="argsType">BannerDismissScreenEventArgs</attr>

<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationInterstitialListener']/method[@name='onDismissScreen']"
        name="argsType">IntersitionalDismissScreenEventArgs</attr>

<attr path="/api/package[@name='android.content']/
        interface[@name='DialogInterface.OnClickListener']"
        name="argsType">DialogClickEventArgs</attr>
```

### <a name="problem-class-does-not-implement-interface-method"></a>Problém: Třída neimplementuje metodu rozhraní

Chybová zpráva se vytvářejí, která udává, zda generovaná třída neimplementuje metodu, která je vyžadována pro rozhraní, která implementuje generovaná třída. Však prohlížení generovaného kódu, uvidíte, že je metoda implementována.

Tady je příklad chyby:

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>Možné příčiny:

Toto je problém způsobený s vazbou Java metody s kovariantní návratové typy. V tomto příkladu metoda `Oauth.Signpost.Http.IHttpRequest.UnWrap()` musí vrátit `Java.Lang.Object`. Ale metodu `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` má návratový typ `HttpURLConnection`. Existují dva způsoby, jak tento problém vyřešit:

-   Přidejte deklaraci třídu pro `HttpURLConnectionRequestAdapter` a explicitní implementace `IHttpRequest.Unwrap()`:

    ```csharp
    namespace Oauth.Signpost.Basic {
        partial class HttpURLConnectionRequestAdapter {
            Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
                return Unwrap();
            }
        }
    }
    ```

-   Odebrání kovarianci generovaného kódu C#. To zahrnuje následující transformace k přidání **Transforms\Metadata.xml** které způsobí generovaného kódu C# do mají návratový typ `Java.Lang.Object`:

    ```xml
    <attr
        path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
        name="managedReturn">Java.Lang.Object
    </attr>
    ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>Problém: Název kolizí v informacích o vnitřní tříd nebo vlastností

Konfliktní viditelnost na zděděné objekty.

V jazyce Java není nutné, že do odvozené třídy mají stejně viditelné jako svůj nadřazený uzel. Java stejně to opravíme za vás. V jazyce C#, který má být explicitní, proto musíte zajistit, všechny třídy v hierarchii mají odpovídající viditelnosti. Následující příklad ukazuje, jak změnit název balíčku Java z `com.evernote.android.job` k `Evernote.AndroidJob`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>Problém: A **.so** knihovny vyžadují vazba je, není načítání

Některé projekty vazba může také závisí na funkcích systému **.so** knihovny. Je možné, že nebude automaticky načíst Xamarin.Android **.so** knihovny. Když provede zabalené kódu v jazyce Java, aby volání JNI a chybová zpráva se nezdaří Xamarin.Android _java.lang.UnsatisfiedLinkError: Nativní metoda nebyla nalezena:_ se objeví v logcat limit pro aplikaci.

To je ručně načíst **.so** knihovny pomocí volání `Java.Lang.JavaSystem.LoadLibrary`. Například za předpokladu, že projektu Xamarin.Android sdílí knihovny **libpocketsphinx_jni.so** zahrnutý v projektu vazbu pomocí akce sestavení **EmbeddedNativeLibrary**, následující načte fragmentu kódu (provést před použitím sdílené knihovny) **.so** knihovny:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>Souhrn

V tomto článku jsme uvedené řešení běžných problémů přidružené vazby Java a vysvětlení jejich řešení.


## <a name="related-links"></a>Související odkazy

- [Projekty knihovny](http://developer.android.com/tools/projects/index.html#LibraryProjects)
- [Práce s JNI](~/android/platform/java-integration/working-with-jni.md)
- [Povolit výstup diagnostiky](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Xamarin pro vývojáře v systému Android](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
