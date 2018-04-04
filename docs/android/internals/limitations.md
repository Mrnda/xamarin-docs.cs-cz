---
title: Omezení
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 07353cfa3ce9bc20688b9a59c09a6e602f16dd60
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="limitations"></a>Omezení

Vzhledem k tomu, že aplikace v systému Android vyžadují generování Java proxy typů během procesu sestavení, není možné generovat všechny kód za běhu.

Toto jsou omezení Xamarin.Android ve srovnání s plochy Mono:


## <a name="limited-dynamic-language-support"></a>Omezené dynamické jazyková podpora

 [Android – obálky s možností](~/android/platform/java-integration/android-callable-wrappers.md) jsou potřeba kdykoli Android runtime třeba vyvolání spravovaného kódu. Android – obálky s možností jsou generovány při kompilaci, na základě statické analýzy IL. Net důsledkem: můžete *nelze* použijte dynamických jazyků (IronPython, IronRuby atd.) v nějaký scénář, kdy je potřeba (včetně nepřímých vytváření podtříd), vytváření podtříd Java typů jako neexistuje žádný způsob extrahování tyto dynamické typy Při kompilaci pro generování Android obálky nezbytné s možností.


## <a name="limited-java-generation-support"></a>Podpora generování omezené Java

[Android – obálky s možností](~/android/platform/java-integration/android-callable-wrappers.md) muset vygenerovat aby kód Java k volání spravovaného kódu. *Ve výchozím nastavení*, Android obálky s možností bude obsahovat pouze (určitých) deklarované konstruktorů a metod, které přepsat virtuální metodu Java (tj. má [ `RegisterAttribute` ](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)) nebo implementovat (metoda rozhraní Java rozhraní podobně má `Attribute`).
  
Před vydáním verze v 4.1 může být deklarován žádné další metody. Verze 4.1 [ `Export` a `ExportField` vlastní atributy lze deklarovat metody Java a pole v rámci Android obálka volatelná aplikacemi](~/android/platform/java-integration/working-with-jni.md).

### <a name="missing-constructors"></a>Chybějící konstruktory

Konstruktory zůstanou složité, pokud [ `ExportAttribute` ](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute) se používá. Algoritmus pro generování konstruktorů Android obálka volatelná je, že bude vygenerované konstruktor Java, pokud:

1. Existuje Java mapování pro všechny typy parametrů
2. Základní třída deklaruje stejné konstruktor &ndash; totiž Android obálka volatelná aplikacemi *musí* vyvolat odpovídající konstruktor základní třída; (jak neexistuje žádný snadný způsob, jak lze použít žádné výchozí argumenty určit, jaké hodnoty musí být použita v rámci Java).

Zvažte například následující třídy:

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

Při této perfektně logické vypadá výsledný Android obálka volatelná aplikacemi *v sestavení pro vydání* nebude obsahovat výchozí konstruktor. V důsledku toho pokud budete chtít tuto službu spustit (například [ `Context.StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)), se nezdaří:

```shell
E/AndroidRuntime(31766): FATAL EXCEPTION: main
E/AndroidRuntime(31766): java.lang.RuntimeException: Unable to instantiate service example.MyIntentService: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2347)
E/AndroidRuntime(31766):        at android.app.ActivityThread.access$1600(ActivityThread.java:130)
E/AndroidRuntime(31766):        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1277)
E/AndroidRuntime(31766):        at android.os.Handler.dispatchMessage(Handler.java:99)
E/AndroidRuntime(31766):        at android.os.Looper.loop(Looper.java:137)
E/AndroidRuntime(31766):        at android.app.ActivityThread.main(ActivityThread.java:4745)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invokeNative(Native Method)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invoke(Method.java:511)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:786)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:553)
E/AndroidRuntime(31766):        at dalvik.system.NativeStart.main(Native Method)
E/AndroidRuntime(31766): Caused by: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at java.lang.Class.newInstanceImpl(Native Method)
E/AndroidRuntime(31766):        at java.lang.Class.newInstance(Class.java:1319)
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2344)
E/AndroidRuntime(31766):        ... 10 more
```

Řešením je výchozí konstruktor deklarovat, její adorn `ExportAttribute`a nastavte [ `ExportAttribute.SuperStringArgument` ](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/): 

```csharp
[Service]
class MyIntentService : IntentService {
    [Export (SuperArgumentsString = "\"value\"")]
    public MyIntentService (): base("value")
    {
    }

    // ...
}
```


### <a name="generic-c-classes"></a>Obecné třídy jazyka C#

Obecné třídy jazyka C# jsou podporovány pouze částečně. Existují tato omezení:


-   Obecné typy nesmíte používat `[Export]` nebo `[ExportField`]. Pokusili, vygeneruje `XA4207` chyby.

    ```csharp
    public abstract class Parcelable<T> : Java.Lang.Object, IParcelable
    {
        // Invalid; generates XA4207
        [ExportField ("CREATOR")]
        public static IParcelableCreator CreateCreator ()
        {
            ...
    }
    ```

-   Obecné metody nemusejí podporovat použití `[Export]` nebo `[ExportField]`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        
        // Invalid; generates XA4207
        [Export]
        public static void Method<T>(T value)
        {
            ...
        }
    }
    ```

-   `[ExportField]` nesmí být použity pro metody, které vrací `void`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        // Invalid; generates XA4208
        [ExportField ("CREATOR")]
        public static void CreateSomething ()
        {
        }
    }
    ```

-   Instance obecných typů _musí není_ vytvořit z kódu v jazyce Java.
    Je lze vytvořit pouze bezpečně ze spravovaného kódu:

    ```csharp
    [Activity (Label="Die!", MainLauncher=true)]
    public class BadGenericActivity<T> : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);
        }
    }
    ```


## <a name="partial-java-generics-support"></a>Podpora částečné Java obecné typy

Java, které podporují obecné vazba je omezená. Zvlášť, jsou ponechána členy v obecné instance třídy, která je odvozena z jiné obecné třídy (bez instanci) zveřejněné jako Java.Lang.Object. Například [Android.Content.Intent.GetParcelableExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.GetParcelableExtra/p/System.String/) metoda vrátí Java.Lang.Object. Toto je z důvodu obecné typy vymazaných Java.
Máme některé třídy, která toto omezení neplatí, ale ručně upraveno.


## <a name="related-links"></a>Související odkazy

- [Obálky Androidu s možností volání](~/android/platform/java-integration/android-callable-wrappers.md)
- [Práce s JNI](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)
- [SuperString](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/)
- [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)
