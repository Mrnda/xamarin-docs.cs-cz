---
title: Architektura
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 9ce1d790f5dea00ac47d5639ae8424793006445a
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
ms.locfileid: "32019292"
---
# <a name="architecture"></a>Architektura

Aplikace Xamarin.Android spustit v prostředí Mono provádění.
Tato spuštění prostředí spustí-souběžného s virtuálním počítačem Android Runtime (obrázky). Obě prostředí runtime běží nad Linux jádra a vystavit různé rozhraní API pro uživatele kód, který umožňuje vývojářům pro přístup k základním systému. Modul runtime Mono je napsán v jazyce C.

Můžete použít [systému](http://msdn.microsoft.com/en-us/library/system.aspx), [System.IO](http://msdn.microsoft.com/en-us/library/system.io.aspx), [System.Net](http://msdn.microsoft.com/en-us/library/system.net.aspx) a zbytek .NET třídy knihovny pro přístup k podkladové zařízení operačního systému Linux.

V systému Android, nejsou k dispozici přímo do nativních aplikací většinu systémová zařízení jako zvuk, obrázky, OpenGL a telefonii, jsou přístupné pouze prostřednictvím rozhraní Android API Java Runtime umístěný v jednom z [Java](https://developer.xamarin.com/api/namespace/Java.Lang/). * obory názvů nebo [Android](https://developer.xamarin.com/api/namespace/Android/). * obory názvů. Architektura je přibližně takto:

[![Diagram Mono a obrázky nad jádra a pod .NET/Java + vazby](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Vývojáři Xamarin.Android přístup k různým funkcím v operačním systému buď volání do rozhraní API technologie .NET, které znají (pro přístup k nízké úrovně) nebo pomocí třídy v obory názvů systému Android, která poskytuje mostu k rozhraním API Java, které jsou vystavené Android modulu Runtime.

Další informace o způsobu třídy Android komunikují s třídy Android modulu Runtime najdete [rozhraní API návrhu](~/android/internals/api-design.md) dokumentu.


## <a name="application-packages"></a>Balíčky aplikací

Balíčky aplikace pro Android jsou kontejnery ZIP s *.apk* příponu souboru. Balíčky aplikací Xamarin.Android mít stejnou strukturu a rozložení jako normální Android balíčky s těmito přídavky:

-   Sestavení aplikace (obsahující IL) jsou *uložené* nekomprimované v rámci *sestavení* složky. Během procesu spuštění ve verzi sestavení *.apk* je *mmap()* ed do procesu a sestavení jsou načteny z paměti. To umožňuje rychlejší spouštění aplikace, protože sestavení není potřeba extrahovat před spuštění. - *Poznámka:* sestavení informace o umístění, jako [Assembly.Location](https://developer.xamarin.com/api/property/System.Reflection.Assembly.Location/) a [Assembly.CodeBase](https://developer.xamarin.com/api/property/System.Reflection.Assembly.CodeBase/)
    *nelze spoléhat* verze sestavení. Neexistují jako odlišné filesystem položky a mají žádné použitelné umístění.


-   Nativní knihovny obsahující Mono runtime jsou k dispozici v rámci *.apk* . Aplikace pro Xamarin.Android musí obsahovat nativní knihovny pro potřeby cílové Android architektury, například *armeabi* , *armeabi v7a* , *x86* . Aplikace Xamarin.Android nelze spustit na platformě, pokud obsahuje odpovídající modulu runtime knihoven.


Aplikace Xamarin.Android také obsahují *Android obálky s možností* umožňující Android provést volání do spravovaného kódu.



## <a name="android-callable-wrappers"></a>Android – obálky s možností

- **Android – obálky s možností** jsou [JNI](http://en.wikipedia.org/wiki/Java_Native_Interface) mostu, který se použije, kdykoli Android runtime potřebuje k vyvolání spravovaného kódu. Android – obálky s možností způsoby jak virtuální lze přepsat a může být implementováno rozhraní Java. Najdete v článku [Přehled integrace Java](~/android/platform/java-integration/index.md) doc Další informace.


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>Spravované obálky s možností volání

Spravované obálky s možností jsou JNI mostu, který se použije, kdykoli spravovaného kódu musí vyvolání kódu pro systém Android a poskytovat podporu pro přepsání virtuální metody a implementace rozhraní Java. Celý [Android](https://developer.xamarin.com/api/namespace/Android/). * a související obory názvů jsou spravované – obálky s možností generované prostřednictvím [.jar vazby](~/android/platform/binding-java-library/index.md).
Spravované obálky s možností jsou zodpovědní za převádění mezi spravovanými a Android typy a vyvolání metod základní platformu Android prostřednictvím JNI.

Každý odkaz globální Java, která je přístupná prostřednictvím vytvořit blokování spravovaná obálka volatelná aplikacemi [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) vlastnost. Používají se globální odkazy pro mapování mezi instancemi Java a spravované instance. Globální odkazy jsou omezené prostředků: emulátorů povolit pouze 2000 globální odkazy na existovat současně, zatímco většina hardware umožňuje více než 52,000 globální odkazy na existovat současně.

Pokud chcete sledovat, kdy se vytváří a zničen globální odkazy, můžete nastavit [debug.mono.log](~/android/troubleshooting/index.md) vlastnost systému tak, aby obsahovala [gref](~/android/troubleshooting/index.md).

Globální odkazy mohou být explicitně uvolněna voláním [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) na spravovaná obálka volatelná aplikacemi. Tímto odeberete mapování mezi Java instance a spravované instance a povolit instance Java, které se mají shromažďovat. Pokud Java instance znovu získat přístup ze spravovaného kódu, vytvoří se pro ni nového spravovaná obálka volatelná aplikacemi.

Pozor musí provést při uvolnění spravované obálky s možností, pokud lze instanci nechtěně sdílet mezi vlákny jako uvolnění instance ovlivní odkazy z jakékoliv jiné vlákno. Pro maximální zabezpečení pouze `Dispose()` instancí, které byly přiděleny prostřednictvím `new` *nebo* z metod které *vědět* vždy přidělit nové instance a není v mezipaměti instance, které mohou způsobit náhodných instance sdílení mezi vlákny.



## <a name="managed-callable-wrapper-subclasses"></a>Spravovaná obálka volatelná aplikacemi podtřídy

Spravovaná obálka volatelná aplikacemi podtřídy jsou může bydlišti všechny "zajímavé" specifické pro aplikaci logiky. Patří mezi ně vlastní [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) podtřídy (například [aktivity "activity1"](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) zadejte výchozí šablona projektu). (Konkrétně tyto jsou *Java.Lang.Object* podtřídy, které se *není* obsahovat [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) vlastních atributů nebo [ RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) je *false*, který je výchozí.)

Jako spravované obálky s možností spravovaná obálka volatelná aplikacemi podtřídy také obsahovat globální odkaz, přístupný prostřednictvím [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) vlastnost. Stejně jako s spravované obálky s možností, může být explicitně uvolněno globální odkazy voláním [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/).
Na rozdíl od spravované obálky s možností *pozor* by měla být provedena před uvolnění těchto instancí jako *Dispose()* ing instance poruší mapování mezi Java instanci (instance Android obálka volatelná aplikacemi) a spravované instance.


### <a name="java-activation"></a>Aktivace Java

Když [Android obálka volatelná aplikacemi](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) je vytvořen z prostředí Java, konstruktor ACW způsobí, že odpovídající C# konstruktoru má být volána. Například ACW pro *MainActivity* bude obsahovat výchozí konstruktor, který bude vyvolán *MainActivity*na výchozí konstruktor. (To se provádí prostřednictvím *TypeManager.Activate()* volání konstruktorů ACW.)

Neexistuje jeden další podpis konstruktor důsledků: *(IntPtr, JniHandleOwnership)* konstruktor. *(IntPtr, JniHandleOwnership)* konstruktor vyvolán vždy, když je objekt Java vystaven do spravovaného kódu a obálka volatelná aplikacemi spravované musí zkonstruovat ke správě JNI popisovač. To se obvykle provádí automaticky.

Existují dva scénáře, ve kterém *(IntPtr, JniHandleOwnership)* konstruktor je třeba ručně zadat na podtřídy spravovaná obálka volatelná aplikacemi:

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) podtřídou třídy. *Aplikace* je zvláštní; výchozí *aplikace* konstruktor bude *nikdy* vyvolat a [(IntPtr, JniHandleOwnership) konstruktor místo toho je třeba zadat ](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Volání metody virtuální z konstruktoru základní třídy.


Všimněte si, že (2) je vyzařovací abstrakce. V jazyce Java, jako v C# volání metod virtuální z konstruktoru vždy vyvolat nejodvozenějších implementace metod. Například [TextView (kontextu, AttributeSet, int) konstruktor](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/) vyvolá metodu virtuální [TextView.getDefaultMovementMethod()](http://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()), který je vázaný jako [ Vlastnost TextView.DefaultMovementMethod](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/).
Proto pokud typu [LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) chtěli (1) [podtřídami TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [přepsat TextView.DefaultMovementMethod](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)a (3) [aktivovat instanci, Třída prostřednictvím XML,](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) přepsané *DefaultMovementMethod* vlastnost by být volána před konstruktor ACW využili příležitost provést, a by k výskytu dojde před konstruktor C# využili příležitost provést.

To je podporováno po vytvoření instance instance LogTextBox prostřednictvím [LogTextView (IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) konstruktor když instance ACW LogTextBox nejprve zadá spravovaný kód a potom vyvolání [ LogTextBox (kontextu, IAttributeSet, int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) konstruktor *ve stejné instanci* při konstruktor ACW provede.

Pořadí událostí:

1.  Rozložení XML je načten do [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41).

2.  Android vytvoří grafu objektu rozložení a vytvoří instanci *monodroid.apidemo.LogTextBox* , ACW pro *LogTextBox* .

3.  *Monodroid.apidemo.LogTextBox* konstruktoru [android.widget.TextView](http://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) konstruktor.

4.  *TextView* volá konstruktor *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* .

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* vyvolá *LogTextBox.n_getDefaultMovementMethod()* , který vyvolá *TextView.n_GetDefaultMovementMethod()* , který vyvolá [Java.Lang.Object.GetObject&lt;TextView&gt; (zpracování, JniHandleOwnership.DoNotTransfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* instanci zkontroluje, jestli je už odpovídající C# pro *zpracování* . Pokud existuje, bude vrácen. V tomto scénáři není k dispozici, tak *Object.GetObject&lt;T&gt;()* musíte vytvořit.

7.  *Object.GetObject&lt;T&gt;()* hledá *LogTextBox (IntPtr, JniHandleOwneship)* konstruktoru, jej spustí, vytvoří mapování mezi *zpracování* a vytvořená instance a vrátí vytvořená instance.

8.  *TextView.n_GetDefaultMovementMethod()* vyvolá *LogTextBox.DefaultMovementMethod* metoda getter vlastnosti.

9.  Vrátí ovládací prvek *android.widget.TextView* konstruktor, který dokončí provádění.

10. *Monodroid.apidemo.LogTextBox* provede konstruktor vyvolání *TypeManager.Activate()* .

11. *LogTextBox (kontextu, IAttributeSet, int)* konstruktoru *ve stejné instanci vytvořené v (7)* .

12. Pokud (IntPtr, JniHandleOwnership) konstruktor nelze najít, pak System.MissingMethodException] (https://developer.xamarin.com/api/type/System.MissingMethodException/) bude vyvolána.

<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>Předčasné Dispose() volání

Existuje mapování mezi popisovač JNI a odpovídající instance jazyka C#. Java.Lang.Object.Dispose() dělí toto mapování. Pokud obslužná rutina JNI zadá spravovaného kódu po mapování bylo přerušeno, vypadá aktivace Java a *(IntPtr, JniHandleOwnership)* konstruktor se zkontrolovat a volání. Pokud konstruktoru neexistuje, bude vyvolána výjimka.

Například uděleno následující podtřídy spravované s Wraper:

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

Pokud jsme vytvoření instance, Dispose() a způsobit, že obálka volatelná aplikacemi spravované znovu vytvořit:

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

Tento program bude kostka:

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

Pokud podtřídy neobsahuje *(IntPtr, JniHandleOwnership)* konstruktoru, pak *nové* bude možné vytvořit instanci typu. V důsledku toho instance zobrazí "neztratit" všechna data instance, protože je novou instanci. (Všimněte si, že hodnota je null.)

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Pouze *Dispose()* z spravovaná obálka volatelná aplikacemi podtřídy, když víte, že objekt Java se nepoužijí už nebo podtřídy neobsahuje žádná data instance a *(IntPtr, JniHandleOwnership)* bylo zadáno konstruktor.



## <a name="application-startup"></a>Spuštění aplikace

Při aktivity, služby, atd. je spuštěn, Android nejdřív zkontroluje, pokud již existuje proces, který běží na hostitele aktivity/service/atd. Pokud žádný takový proces existuje, vytvoří se nový proces, [AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-intro.html) pro čtení, a to s typem zadaným v [ /manifest/application/@android:name ](http://developer.android.com/guide/topics/manifest/application-element.html#nm) atribut je načíst a vytvoření instance. Další, všechny typy určeného [ /manifest/application/provider/@android:name ](http://developer.android.com/guide/topics/manifest/provider-element.html#nm) hodnoty atributů jsou vytvářeny instance a mít jejich [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/) volaná metoda. Háky Xamarin.Android do této přidáním *mono. MonoRuntimeProvider* *ContentProvider* k AndroidManifest.xml během procesu vytváření. *Mono. MonoRuntimeProvider.attachInfo()* metoda zodpovídá za načítání Mono runtime do procesu.
Všechny pokusy o použití Mono před tento bod se nezdaří. ( *Poznámka*: to je důvod, proč typy které podtřídami [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) muset zadat [(IntPtr, JniHandleOwnership) konstruktor](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103), jako je instance aplikace Vytvořit předtím, než může být inicializována Mono.)

Po dokončení procesu inicializace `AndroidManifest.xml` je konzultaci s nalézt název třídy aktivity, služby a další ke spuštění. Například [ /manifest/application/activity/@android:name atribut](http://developer.android.com/guide/topics/manifest/activity-element.html#nm) slouží k určení názvu aktivity načíst. Pro aktivity, musí tento typ dědí [android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/).
Zadaný typ je načten prostřednictvím [Class.forName()](http://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (který vyžaduje, aby typ Java typ, proto Android obálky s možností), pak vytvořit instance. Vytvoření instance Android obálka volatelná aplikacemi aktivují vytvoření instance příslušného typu C#. Android se pak vyvolání [Activity.onCreate(Bundle)](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , což způsobí, že odpovídající [Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) má být volána, a vy budete vypnout na RAS.
