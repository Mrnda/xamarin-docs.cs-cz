---
title: Architektura
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: e6a30247c13deab871bf230aba53b9963981fd02
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997397"
---
# <a name="architecture"></a>Architektura

Aplikace Xamarin.Android spuštění v prostředí provádění Mono.
Tento spuštění prostředí spuštění – souběžně s virtuálním počítačem s Androidem modulu Runtime (UMĚNÍ). Obě běhových prostředí běží nad linuxového jádra a vystavit různá rozhraní API pro uživatelský kód, který umožňuje vývojářům získat přístup k podkladové systému. Modul Mono runtime je napsán v jazyce C.

Je možné použít [systému](xref:System), [System.IO](xref:System.IO), [System.Net](xref:System.Net) a zbytek .NET třídy knihovny pro přístup k podkladové zařízení operačního systému Linux.

V Androidu, nejsou k dispozici přímo do nativních aplikací většinu systémová zařízení, jako je zvuk, obrázky, OpenGL a telefonního subsystému, jsou přístupné pouze prostřednictvím rozhraní Android API Java Runtime umístěný v jednom z [Java](https://developer.xamarin.com/api/namespace/Java.Lang/). * obory názvů nebo [Android](https://developer.xamarin.com/api/namespace/Android/). * obory názvů. Tato architektura je přibližně takto:

[![Diagram Mono a GRAFIKY nad jádra a pod .NET nebo Javě + vazby](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Vývojáře prostředí Xamarin.Android k různým funkcím v operačním systému tak, že volání do rozhraní API .NET, která ví (pro přístup nízké úrovně) nebo pomocí třídy v obory názvů systému Android, která zajišťuje přemostění rozhraní Java API, které jsou vystavené modulu Runtime Androidu.

Další informace o komunikaci třídy Android s třídami modulu Runtime Androidu najdete v článku [návrh rozhraní API](~/android/internals/api-design.md) dokumentu.


## <a name="application-packages"></a>Balíčky aplikací

Balíčky aplikace pro Android jsou kontejnery ZIP s *.apk* příponu souboru. Balíčky aplikací Xamarin.Android mají stejnou strukturu a rozložení jako normální balíčky pro Android, s těmito přídavky:

-   Sestavení aplikace (obsahující IL) jsou *uložené* nekomprimované v rámci *sestavení* složky. Během procesu sestavení po spuštění ve verzi *.apk* je *mmap()* ed do procesu a sestavení jsou načtena z paměti. To umožňuje rychlejší spuštění aplikace, protože sestavení není nutné před provedením extrahovat. - *Poznámka:* informace o umístění sestavení, jako [Assembly.Location](xref:System.Reflection.Assembly.Location) a [Assembly.CodeBase](xref:System.Reflection.Assembly.CodeBase)
    *nelze dovolávat* verze sestavení. Neexistují jako položky odlišné systému souborů a nemají žádné použitelné umístění.


-   Nativní knihovny obsahující modul Mono runtime jsou k dispozici v rámci *.apk* . Aplikace pro Xamarin.Android musí obsahovat nativních knihoven pro požadované cílové Android architektury, například *armeabi* , *armeabi v7a* , *x86* . Aplikace Xamarin.Android nemůžou běžet na platformě, pokud obsahuje odpovídající modul runtime knihovny.


Aplikace Xamarin.Android také obsahují *obálky androidu s možností* umožňující Android provést volání do spravovaného kódu.



## <a name="android-callable-wrappers"></a>Obálky androidu s možností

- **Obálky androidu s možností** jsou [JNI](http://en.wikipedia.org/wiki/Java_Native_Interface) mostu, který se použije, kdykoli modulu runtime Androidu je potřeba volat spravovaný kód. Obálky androidu s možností jsou virtuálních metod lze přepsat a je možné implementovat rozhraní Java. Najdete v článku [Přehled integrace Javy](~/android/platform/java-integration/index.md) doc Další informace.


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>Spravované obálky

Spravované obálky jsou JNI mostu, který se použije, kdykoli spravovaný kód je potřeba vyvolat kódu pro Android a poskytovat podporu pro přepisování virtuální metody a implementace rozhraní Java. Celý [Android](https://developer.xamarin.com/api/namespace/Android/). * a související obory názvů jsou spravované obálky s možností vygenerovanou přes [.jar vazby](~/android/platform/binding-java-library/index.md).
Pro převod mezi typy spravovaných a s Androidem a volání metody základní platformu Android prostřednictvím JNI zodpovídají spravované obálky.

Každý odkaz globální Java, která je dostupná prostřednictvím vytvořili blokování spravovaná obálka volatelná aplikacemi [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) vlastnost. Globální odkazy se používají k zajištění mapování mezi instancemi Java a spravované instance. Globální odkazy jsou omezené prostředky: emulátory povolit pouze 2000 globální odkazy existovat současně, zatímco většina hardwaru umožňuje více než 52,000 globální odkazy existovat současně.

Pokud chcete sledovat, kdy je vytvořeno a zničeno globální odkazy, můžete nastavit [debug.mono.log](~/android/troubleshooting/index.md) vlastnost systému tak, aby obsahovala [gref](~/android/troubleshooting/index.md).

Globální odkazy můžete explicitně uvolnit voláním [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) na spravovaná obálka volatelná aplikacemi. Tím odeberete mapování mezi Java instance a spravované instance a povolit Java instance se mají shromažďovat. Pokud Java instance znovu získat přístup ze spravovaného kódu, vytvoří se pro něj nový spravovaná obálka volatelná aplikacemi.

Nutné se ujistit, při uvolnění spravovaných obálek Volatelných, pokud je instance neúmyslně sdílet mezi vlákny jako disposing instance bude mít dopad na odkazy z jiných vláken. Pro maximální bezpečnost pouze `Dispose()` instancí, které byly přiděleny prostřednictvím `new` *nebo* z metody které *vědět* vždy přidělení nové instance a není v mezipaměti instancí, které mohou být způsobit nechtěné instance sdílení mezi vlákny.



## <a name="managed-callable-wrapper-subclasses"></a>Spravovaná obálka volatelná aplikacemi podtřídy

Spravovaná obálka volatelná aplikacemi podtřídy jsou, kde může provozu všechny "zajímavý" specifické pro aplikaci logiky. Patří mezi ně vlastní [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) podtřídy (například [aktivity "activity1"](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) zadejte výchozí šablony projektu). (Konkrétně jsou *Java.Lang.Object* podtřídy třídy, které se *není* obsahovat [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) vlastního atributu nebo [ RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) je *false*, což je výchozí hodnota.)

Jako spravovaných obálek volatelných spravovaná obálka volatelná aplikacemi podtřídy také obsahovat globální odkaz, přístupné prostřednictvím [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) vlastnost. Stejně jako s spravované obálky s možností, globální odkazy můžete explicitně uvolnit voláním [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/).
Na rozdíl od spravovaných obálek volatelných *pozornost* před uvolnění těchto instancí, jako třeba *Dispose()* ing instance přeruší mapování mezi Java instance (instance Obálka volatelná aplikacemi Android) a spravované instance.


### <a name="java-activation"></a>Aktivace Java

Když [Android obálku volatelnou](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) se vytvoří z Javy ACW konstruktoru způsobí, že odpovídající C# konstruktor má být volána. Například ACW pro *MainActivity* bude obsahovat výchozí konstruktor, který vyvolá *MainActivity*jeho výchozí konstruktor. (To se provádí prostřednictvím *TypeManager.Activate()* volání konstruktorů ACW.)

Existuje jeden další podpis konstruktoru důsledků: *(IntPtr, JniHandleOwnership)* konstruktoru. *(IntPtr, JniHandleOwnership)* konstruktor je vyvolána pokaždé, když se objekt Java zpřístupněn do spravovaného kódu a obálku volatelnou spravované je potřeba použít ke správě JNI popisovač. To se obvykle provádí automaticky.

Existují dva scénáře, ve kterém *(IntPtr, JniHandleOwnership)* konstruktor musí být prováděno manuálně na podtřídu spravovaná obálka volatelná aplikacemi:

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) je rozčlenit do podtříd. *Aplikace* je speciální; výchozí hodnota *aplikace* konstruktoru bude *nikdy* vyvolána a [(IntPtr, JniHandleOwnership) konstruktor musí být zadaná místo ](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Volání virtuální metody z konstruktor základní třídy.


Všimněte si, že (2) je děravé abstrakce. V jazyce Java, stejně jako v jazyce C# volání virtuální metody z konstruktoru vždy vyvolá metoda implementace Většina odvozených. Například [TextView (kontextu AttributeSet, int) konstruktor](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/) vyvolá virtuální metodu [TextView.getDefaultMovementMethod()](http://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()), který je vázaný jako [ Vlastnost TextView.DefaultMovementMethod](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/).
Proto pokud typ [LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) chtěli (1) [podtřídy TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [přepsat TextView.DefaultMovementMethod](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)a (3) [aktivovat instance, která Třída prostřednictvím XML,](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) přepsané *DefaultMovementMethod* vlastnost by být volána před konstruktor ACW využili příležitost dobře se spuštění a by nastaly předtím, než C# konstruktor využili příležitost dobře se spustit.

To platí po vytvoření instance instance LogTextBox prostřednictvím [LogTextView (IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) konstruktoru při instanci ACW LogTextBox poprvé vstoupí spravovaného kódu a pak volání [ LogTextBox (kontextu IAttributeSet, int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) konstruktor *ve stejné instanci* při ACW konstruktor provádí.

Řazení událostí:

1.  Rozložení XML je načten do [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41).

2.  Android vytvoří instanci objektu rozložení grafu a vytvoří instanci *monodroid.apidemo.LogTextBox* , ACW pro *LogTextBox* .

3.  *Monodroid.apidemo.LogTextBox* konstruktoru [android.widget.TextView](http://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) konstruktoru.

4.  *TextView* volá konstruktor *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* .

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* vyvolá *LogTextBox.n_getDefaultMovementMethod()* , která vyvolá *TextView.n_GetDefaultMovementMethod()* , což vyvolá [Java.Lang.Object.GetObject&lt;TextView&gt; (zpracování, JniHandleOwnership.DoNotTransfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* kontroluje, zda je již odpovídajícího jazyka C# instanci *zpracování* . Pokud existuje, bude vrácen. V tomto scénáři není k dispozici, takže *Object.GetObject&lt;T&gt;()* musíte ji vytvořit.

7.  *Object.GetObject&lt;T&gt;()* hledá *LogTextBox (IntPtr, JniHandleOwneship)* konstruktoru, vyvolá se, vytvoří mapování mezi *zpracování* a vytvořená instance a vrátí vytvořená instance.

8.  *TextView.n_GetDefaultMovementMethod()* vyvolá *LogTextBox.DefaultMovementMethod* metoda getter vlastnosti.

9.  Ovládací prvek vrátí *android.widget.TextView* konstruktor, který dokončí provádění.

10. *Monodroid.apidemo.LogTextBox* konstruktor provádí volání *TypeManager.Activate()* .

11. *LogTextBox (kontextu IAttributeSet, int)* konstruktoru *ve stejné instanci vytvořené v (7)* .

12. Pokud (IntPtr, JniHandleOwnership) se nenašel konstruktor a pak System.MissingMethodException](xref:System.MissingMethodException) bude vyvolána výjimka.

<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>Předčasné Dispose() volání

Existuje mapování mezi JNI popisovač a odpovídající instance jazyka C#. Java.Lang.Object.Dispose() dělí toto mapování. Pokud popisovač JNI přejde do spravovaného kódu po mapování bylo přerušeno, vypadá aktivace Java a *(IntPtr, JniHandleOwnership)* konstruktor se kontroluje a vyvolána. Pokud konstruktor neexistuje, bude vyvolána výjimka.

Mějme například následující spravované Volatelných Wraper podtřídy:

```csharp
class ManagedValue : Java.Lang.Object {

    public string Value {get; private set;}

    public ManagedValue (string value)
    {
        Value = value;
    }

    public override string ToString ()
    {
        return string.Format ("[Managed: Value={0}]", Value);
    }
}
```

Pokud jsme vytvořit instanci Dispose() a způsobit, že spravovaná Callable Wrapper musí znovu vytvořit:

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

Program bude kostka:

```shell
E/mono    ( 2906): Unhandled Exception: System.NotSupportedException: Unable to activate instance of type Scratch.PrematureDispose.ManagedValue from native handle 4051c8c8 --->
System.MissingMethodException: No constructor found for Scratch.PrematureDispose.ManagedValue::.ctor(System.IntPtr, Android.Runtime.JniHandleOwnership)
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateProxy (System.Type type, IntPtr handle, JniHandleOwnership transfer) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   --- End of inner exception stack trace ---
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object.GetObject (IntPtr handle, JniHandleOwnership transfer, System.Type type) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object._GetObject[IJavaObject] (IntPtr handle, JniHandleOwnership transfer) [0x00000
```

Pokud podtřídy neobsahuje *(IntPtr, JniHandleOwnership)* konstruktoru, o *nové* vytvoří instance daného typu. V důsledku toho instance se zobrazí "neztratit" všechna data instance, protože se jedná o novou instanci. (Všimněte si, že je hodnota null.)

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Pouze *Dispose()* z spravovaná obálka volatelná aplikacemi podtřídy, když víte, že už nebude používat objekt Java nebo podtřídy neobsahuje žádná data instance a *(IntPtr, JniHandleOwnership)* byl poskytnut konstruktor.



## <a name="application-startup"></a>Spuštění aplikace

Když aktivita, služby, atd. spustí se Android nejprve zkontroluje, zobrazíte, pokud již existuje procesu spuštěného k hostování aktivity/service/atd. Pokud žádný takový proces existuje, vytvoří se nový proces, [AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-intro.html) je pro čtení a typ zadaný v [ /manifest/application/@android:name ](http://developer.android.com/guide/topics/manifest/application-element.html#nm) atribut je načten a vytvořit instance. Další, všechny typy určené [ /manifest/application/provider/@android:name ](http://developer.android.com/guide/topics/manifest/provider-element.html#nm) hodnoty atributů jsou vytvořeny a mají jejich [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/) metoda vyvolána. Xamarin.Android háky do to tak, že přidáte *mono. MonoRuntimeProvider* *Contentprovideru* do souboru AndroidManifest.xml během procesu sestavení. *Mono. MonoRuntimeProvider.attachInfo()* metoda zodpovídá za načítání modulu Mono runtime do procesu.
Všechny pokusy o používat Mono před tento bod se nezdaří. ( *Poznámka*: to je důvod, proč typy, které jsou podtřídami tříd [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) muset zadat [(IntPtr, JniHandleOwnership) konstruktor](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103), jak je instance aplikace Vytvořit předtím, než mohou být inicializovány Mono.)

Po dokončení procesu inicializace `AndroidManifest.xml` konzultaci k vyhledání názvu třídy aktivity, služby a další ke spuštění. Například [ /manifest/application/activity/@android:name atribut](http://developer.android.com/guide/topics/manifest/activity-element.html#nm) slouží k určení názvu aktivity načíst. Pro aktivity, tento typ musí dědit [android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/).
Je zadaného typu načteného prostřednictvím [Class.forName()](http://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (který vyžaduje, aby typ Javě zadejte proto obálky androidu s), pak vytvořena instance. Vytvoření instance Android obálku volatelnou aktivují vytvoření instance odpovídajícího typu jazyka C#. Android bude poté vyvolat [Activity.onCreate(Bundle)](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , což způsobí, že odpovídající [Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) má být vyvolána, a všechno roztočí bude.
