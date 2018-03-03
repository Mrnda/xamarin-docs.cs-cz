---
title: "Zařazování výjimky"
description: "Xamarin.iOS obsahuje nové události, který mu umožní reagovat na výjimky, zejména v nativním kódu."
ms.topic: article
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/05/2017
ms.openlocfilehash: 48b7d953ac7a24c6228e4016d99b22adc1eaef17
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="exception-marshaling"></a>Zařazování výjimky

_Xamarin.iOS obsahuje nové události, který mu umožní reagovat na výjimky, zejména v nativním kódu._

Spravovaný kód a jazyka Objective-C je podpora výjimky za běhu (try/catch/finally – klauzule).

Ale jejich implementace se liší, což znamená, že knihovny modulu runtime (Mono runtime a knihovny modulu runtime jazyka Objective-C) mít problémy, ke zpracování výjimek a poté spusťte kód napsaný v jiných jazycích.

Tento dokument popisuje problémy, které může dojít a možná řešení.

Zahrnuje také ukázkový projekt [zařazování výjimka](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling), které můžete použít k testování různé scénáře a jejich řešení.

## <a name="problem"></a>Problém

Tento problém nastane, když výjimku je vyvolána výjimka a došlo během unwinding rámce zásobníku který neodpovídá typ výjimky, která byla vydána.

Typickým příkladem tohoto pro Xamarin.iOS nebo Xamarin.Mac je při výjimku jazyka Objective-C nativní rozhraní API a této výjimky jazyka Objective-C musí nějakým způsobem ji zpracovat při procesu unwinding zásobníku dosáhne spravované snímku.

Výchozí akce je se nic nestane. Pro vzorovou výše to znamená, když necháte rámce unwind spravovaný modul runtime jazyka Objective-C. Toto je problematické, protože modul runtime jazyka Objective-C nebude vědět, jak unwind spravované rámce; například ho nebude možné spustit žádné `catch` nebo `finally` klauzule v tomto snímku.

### <a name="broken-code"></a>Přerušený kódu

Vezměte v úvahu následující příklad kódu:

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

To vyvolá výjimku NSInvalidArgumentException jazyka Objective-C v nativním kódu:

    NSInvalidArgumentException *** setObjectForKey: key cannot be nil

A trasování zásobníku bude přibližně takto:

    0   CoreFoundation          __exceptionPreprocess + 194
    1   libobjc.A.dylib         objc_exception_throw + 52
    2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
    3   libobjc.A.dylib         objc_msgSend + 102
    4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
    5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
    6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()

Rámce 0 až 3 jsou nativní rámce a unwinder zásobníku v modulu runtime jazyka Objective-C _můžete_ unwind tyto snímky. Konkrétně provede Objective-C `@catch` nebo `@finally` klauzule.

Je však unwinder zásobníku jazyka Objective-C _není_ schopen správně unwinding spravované rámce (ty 4 až 6), v tom snímky bude oddělen, ale spravované výjimka logiku nebudou provedeny.

Což znamená, že obvykle není možné zachytit tyto výjimky následujícím způsobem:

```csharp
try {
    var dict = new NSMutableDictionary ();
    dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero);
} catch (Exception ex) {
    Console.WriteLine (ex);
} finally {
    Console.WriteLine ("finally");
}
```

Důvodem je, že unwinder zásobníku jazyka Objective-C neví o spravovaný `catch` klauzule a ani se `finally` klauzule provést.

Při výše uvedený ukázkový kód _je_ efektivní, je protože jazyka Objective-C má metodu o neošetřených výjimek jazyka Objective-C [ `NSSetUncaughtExceptionHandler` ] [ 2], které Xamarin.iOS a použití Xamarin.Mac a v tomto bodě pokusí převést jakékoli výjimky jazyka Objective-C na spravované výjimky.

## <a name="scenarios"></a>Scénáře

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>Scénář 1 - zachytávání výjimek jazyka Objective-C s obslužnou rutinu spravovaného catch

V následujícím scénáři je možné zachytit výjimky jazyka Objective-C pomocí spravované `catch` obslužné rutiny:

1. Je vyvolána výjimka jazyka Objective-C.
2. Modul runtime jazyka Objective-C provede zásobníku (ale ne unwind), hledá nativní `@catch` obslužná rutina, která může zpracovat výjimku.
3. Modul runtime jazyka Objective-C nenajde žádný `@catch` obslužné rutiny, volání `NSGetUncaughtExceptionHandler`a volá obslužnou rutinu ve Xamarin.iOS/Xamarin.Mac nainstalovaná.
4. Xamarin.iOS/Xamarin.Mac's obslužná rutina výjimky jazyka Objective-C do spravovaných výjimky, převede a throw ho. Od jazyka Objective-C runtime nebyla unwind zásobníku (pouze šel ji), aktuální snímek je stejný jako ten, kde došlo k výjimce jazyka Objective-C.

Jiný problém je způsoben zde Mono runtime nebude vědět, jak unwind jazyka Objective-C rámce správně.

Při volání Xamarin.iOS' nezachycená zpětného volání výjimek jazyka Objective-C zásobník představuje takto:

     0 libxamarin-debug.dylib   exception_handler(exc=name: "NSInvalidArgumentException" - reason: "*** setObjectForKey: key cannot be nil")
     1 CoreFoundation           __handleUncaughtException + 809
     2 libobjc.A.dylib          _objc_terminate() + 100
     3 libc++abi.dylib          std::__terminate(void (*)()) + 14
     4 libc++abi.dylib          __cxa_throw + 122
     5 libobjc.A.dylib          objc_exception_throw + 337
     6 CoreFoundation           -[__NSDictionaryM setObject:forKey:] + 1015
     7 libxamarin-debug.dylib   xamarin_dyn_objc_msgSend + 102
     8 TestApp                  ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
     9 TestApp                  Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr) [0x00000]
    10 TestApp                  ExceptionMarshaling.Exceptions.ThrowObjectiveCException () [0x00013]

Zde pouze spravované rámce jsou snímky, 8-10, ale spravované výjimka je vyvolána v rámci 0. To znamená, že musí Mono runtime unwind nativní rámce 0-7, což způsobí, že problém ekvivalentní výše popsané problém: i když Mono runtime bude unwind nativní rámce, nebudou ho provést Objective-C `@catch` nebo `@finally` – klauzule .

Příklad kódu:

``` objective-c
-(id) setObject: (id) object forKey: (id) key
{
    @try {
        if (key == nil)
            [NSException raise: @"NSInvalidArgumentException"];
    } @finally {
        NSLog (@"This will not be executed");
    }
}
```

A `@finally` klauzule nebude nelze provést, protože Mono modul runtime, který unwinds tento snímek nebude vědět o něm.

Varianta to je spravovaný výjimku spravovaného kódu a pak unwinding prostřednictvím nativní rámce získat vyvolat prvního spravované `catch` klauzule:

``` csharp
class AppDelegate : UIApplicationDelegate {
    public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
    {
        throw new Exception ("An exception");
    }
    static void Main (string [] args)
    {
        try {
            UIApplication.Main (args, null, typeof (AppDelegate));
        } catch (Exception ex) {
            Console.WriteLine ("Managed exception caught.");
        }
    }
}
```

Spravovaný `UIApplication:Main` nativního bude volání metody `UIApplicationMain` metoda a iOS budou dělat spoustu spuštění nativního kódu před voláním nakonec spravovaný `AppDelegate:FinishedLaunching` metodu se stále spoustu nativní rámce v zásobníku při spravované výjimky vyvolána:

     0: TestApp                 ExceptionMarshaling.IOS.AppDelegate:FinishedLaunching (UIKit.UIApplication,Foundation.NSDictionary)
     1: TestApp                 (wrapper runtime-invoke) <Module>:runtime_invoke_bool__this___object_object (object,intptr,intptr,intptr) 
     2: libmonosgen-2.0.dylib   mono_jit_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
     3: libmonosgen-2.0.dylib   do_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
     4: libmonosgen-2.0.dylib   mono_runtime_invoke [inlined] mono_runtime_invoke_checked(method=<unavailable>, obj=<unavailable>, params=<unavailable>, error=0xbff45758)
     5: libmonosgen-2.0.dylib   mono_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>)
     6: libxamarin-debug.dylib  xamarin_invoke_trampoline(type=<unavailable>, self=<unavailable>, sel="application:didFinishLaunchingWithOptions:", iterator=<unavailable>), context=<unavailable>)
     7: libxamarin-debug.dylib  xamarin_arch_trampoline(state=0xbff45ad4)
     8: libxamarin-debug.dylib  xamarin_i386_common_trampoline
     9: UIKit                   -[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:]
    10: UIKit                   -[UIApplication _callInitializationDelegatesForMainScene:transitionContext:]
    11: UIKit                   -[UIApplication _runWithMainScene:transitionContext:completion:]
    12: UIKit                   __84-[UIApplication _handleApplicationActivationWithScene:transitionContext:completion:]_block_invoke.3124
    13: UIKit                   -[UIApplication workspaceDidEndTransaction:]
    14: FrontBoardServices      __37-[FBSWorkspace clientEndTransaction:]_block_invoke_2
    15: FrontBoardServices      __40-[FBSWorkspace _performDelegateCallOut:]_block_invoke
    16: FrontBoardServices      __FBSSERIALQUEUE_IS_CALLING_OUT_TO_A_BLOCK__
    17: FrontBoardServices      -[FBSSerialQueue _performNext]
    18: FrontBoardServices      -[FBSSerialQueue _performNextFromRunLoopSource]
    19: FrontBoardServices      FBSSerialQueueRunLoopSourceHandler
    20: CoreFoundation          __CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__
    21: CoreFoundation          __CFRunLoopDoSources0
    22: CoreFoundation          __CFRunLoopRun
    23: CoreFoundation          CFRunLoopRunSpecific
    24: CoreFoundation          CFRunLoopRunInMode
    25: UIKit                   -[UIApplication _run]
    26: UIKit                   UIApplicationMain
    27: TestApp                 (wrapper managed-to-native) UIKit.UIApplication:UIApplicationMain (int,string[],intptr,intptr)
    28: TestApp                 UIKit.UIApplication:Main (string[],intptr,intptr)
    29: TestApp                 UIKit.UIApplication:Main (string[],string,string)
    30: TestApp                 ExceptionMarshaling.IOS.Application:Main (string[])

Rámce 0-1 a 27 30 jsou spravovány, zatímco všichni, mezi nimi jsou nativní. Pokud Mono unwinds prostřednictvím těchto snímků, Objective-C `@catch` nebo `@finally` klauzule bude proveden.

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>Scénář 2 - nepodařilo se zachytit výjimky jazyka Objective-C

V následujícím scénáři je _není_ možné zachytit výjimky jazyka Objective-C pomocí spravovaného `catch` obslužné rutiny protože výjimky jazyka Objective-C bylo zpracováno jiným způsobem:

1. Je vyvolána výjimka jazyka Objective-C.
2. Modul runtime jazyka Objective-C provede zásobníku (ale ne unwind), hledá nativní `@catch` obslužná rutina, která může zpracovat výjimku.
3. Najde modul runtime jazyka Objective-C `@catch` obslužnou rutinu, unwinds zásobníku a spustí provádění `@catch` obslužné rutiny.

Tento scénář se běžně nachází v aplikacích pro Xamarin.iOS, protože na hlavní vlákno je obvykle kód takto:

``` objective-c
void UIApplicationMain ()
{
    @try {
        while (true) {
            ExecuteRunLoop ();
        }
    } @catch (NSException *ex) {
        NSLog (@"An unhandled exception occured: %@", exc);
        abort ();
    }
}

```

To znamená, že na hlavní vlákno se nikdy skutečně k neošetřené výjimce jazyka Objective-C, a proto je naše zpětné volání, které převede výjimky jazyka Objective-C na spravované výjimky nikdy názvem.

Toto je také celkem běžné při ladění aplikace Xamarin.Mac ve starší verzi systému macOS než Xamarin.Mac podporuje vzhledem k tomu, že zkontrolujete většinu objektů uživatelského rozhraní v ladicím programu se pokusí načíst vlastnosti, které odpovídají na selektory, které neexistují v provádění (platforma protože Xamarin.Mac zahrnuje podporu pro vyšší verzi systému macOS). Volání metody takové selektory vyvolá výjimku `NSInvalidArgumentException` ("nerozpoznané selektor odesílá do..."), což může způsobit, že proces havárií.

To Shrneme, s modulem runtime jazyka Objective-C nebo rámce unwind Mono runtime, které budou nejsou naprogramovaný tak, aby obslužná rutina může vést k nedefinované chování, například dojde k chybě, nevracení paměti a dalších typů (mis) nepředvídatelné chování.

## <a name="solution"></a>Řešení

V Xamarin.iOS 10 a Xamarin.Mac 2.10 přidali jsme podporu pro zachytávání výjimek spravovaných i jazyka Objective-C na ohraničení spravované nativní a pro převod na jiný typ této výjimky.

V pseudo kódu vypadá přibližně takto:

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

P/Invoke k objc_msgSend je zachycen a tomu se říká místo:

``` objective-c
void
xamarin_dyn_objc_msgSend (id obj, SEL sel)
{
    @try {
        objc_msgSend (obj, sel);
    } @catch (NSException *ex) {
        convert_to_and_throw_managed_exception (ex);
    }
}
```

A něco podobného probíhá zpětná případu (zařazování spravované výjimky k výjimkám jazyka Objective-C).

Zachytávání výjimek na hranici spravované nativní není bez nákladů, takže ji není vždy ve výchozím nastavení zapnutá:

- Xamarin.iOS/tvOS: zachycení výjimek jazyka Objective-C je povolený v simulátoru.
- Xamarin.watchOS: zachycení je vyžadována ve všech případech, protože když necháte unwind runtime jazyka Objective-C, spravované rámce bude matou uvolňování paměti a buď nastavte zablokování nebo chyba.
- Xamarin.Mac: zachycení výjimek jazyka Objective-C je povolený pro ladění.

[Čase vytvoření buildu příznaky](#build_time_flags) části vysvětluje, jak povolit zachycení, pokud není ve výchozím nastavení povolené.

## <a name="events"></a>Události

Existují dvě nové události, které jsou vyvolány po zachycení výjimku: `Runtime.MarshalManagedException` a `Runtime.MarshalObjectiveCException`.

Obě události jsou předávány `EventArgs` objekt, který obsahuje původní výjimka, která byla vydána ( `Exception` vlastnost) a `ExceptionMode` vlastnost definovat, jak by měl být zařazen výjimku.

`ExceptionMode` Vlastnost lze změnit v obslužné rutiny můžete změnit chování podle všechny vlastní zpracování provést v obslužné rutině. Příkladem může být proces přerušit, pokud dojde k určité výjimky.

Změna `ExceptionMode` vlastnost se vztahuje na jedinou událost, nemá vliv na všechny výjimky zachytili v budoucnu.

K dispozici jsou následující režimy:

- `Default`: Výchozí hodnota se liší podle platformy. Je `ThrowObjectiveCException` Pokud globální Katalog je v režimu spolupráci (watchOS), a `UnwindNativeCode` jinak (iOS / watchOS / systému macOS). Výchozí nastavení může v budoucnu změnit.
- `UnwindNativeCode`: Jedná se o předchozí (nedefinované) chování. Toto není k dispozici při použití globální Katalog v spolupráci režimu (což je jedinou možností na watchOS; proto toto není platná možnost na watchOS), ale je výchozí možnost pro jiné platformy.
- `ThrowObjectiveCException`: Převést spravované výjimka výjimku jazyka Objective-C a výjimku výjimky jazyka Objective-C. Toto je výchozí hodnota na watchOS.
- `Abort`: Proces přerušit.
- `Disable`: Zakáže zachycením výjimky, takže nemá smysl k nastavení této hodnoty v obslužné rutiny události, ale po událost se vyvolá, je příliš pozdě ji zakázat. V každém případě Pokud nastavíte, se budou chovat jako `UnwindNativeCode`.

Pro zařazování výjimky jazyka Objective-C do spravovaného kódu, jsou k dispozici v následujících režimech:

- `Default`: Výchozí hodnota se liší podle platformy. Je `ThrowManagedException` Pokud globální Katalog je v režimu spolupráci (watchOS), a `UnwindManagedCode` jinak (iOS / tvOS / systému macOS). Výchozí nastavení může v budoucnu změnit.
- `UnwindManagedCode`: Jedná se o předchozí (nedefinované) chování. Toto není k dispozici při použití globální Katalog v spolupráci režimu (což je platné pouze režim GC na watchOS; proto to není platná možnost na watchOS), ale je výchozí hodnota pro jiné platformy.
- `ThrowManagedException`: Převést výjimky jazyka Objective-C na spravované výjimky a výjimku spravované výjimky. Toto je výchozí hodnota na watchOS.
- `Abort`: Proces přerušit.
- `Disable`: Zakáže zachycení výjimky, takže nemá smysl nastavit tato hodnota v případě, že obslužná rutina, ale jednou událost se vyvolá, je příliš pozdě ji zakázat. V každém případě Pokud nastavíte, se zruší proces.

Tím, že pokud chcete zobrazit pokaždé, když je zařazené výjimku, můžete to udělat:

``` csharp
Runtime.MarshalManagedException += (object sender, MarshalManagedExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling managed exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
    
};
Runtime.MarshalObjectiveCException += (object sender, MarshalObjectiveCExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling Objective-C exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
};
```

<a name="build_time_flags" />

## <a name="build-time-flags"></a>Příznaky době sestavení

Je možné předat následujících možností **mtouch** (pro aplikace Xamarin.iOS) a **mmp** (pro Xamarin.Mac aplikace), který bude určí, zda je povoleno zachycení výjimky a nastavení výchozí akce k by mělo dojít:

- `--marshal-managed-exceptions=`
  - `default`
  - `unwindnativecode`
  - `throwobjectivecexception`
  - `abort`
  - `disable`

- `--marshal-objectivec-exceptions=`
  - `default`
  - `unwindmanagedcode`
  - `throwmanagedexception`
  - `abort`
  - `disable`

S výjimkou `disable`, tyto hodnoty jsou shodné s `ExceptionMode` hodnoty, které se budou předávat `MarshalManagedException` a `MarshalObjectiveCException` události.

`disable` Možnost bude _většinou_ zakázat zachycením, s výjimkou jsme budete stále zachytávat výjimky, když nepřidá žádné režijní náklady na spuštění. Zařazování události jsou stále vyvolány pro tyto výjimky s výchozí režim je výchozí režim pro provádění platformu.

## <a name="limitations"></a>Omezení

Jsme může zachytit kódů do `objc_msgSend` rodiny funkcí při zachycení výjimky jazyka Objective-C. To znamená, že P/Invoke jiné C funkci, která pak vyvolává jakékoli výjimky jazyka Objective-C, spustí stále do původního a Nedefinovaná chování (to je možné zvýšit v budoucnosti).

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc


## <a name="related-links"></a>Související odkazy

- [Výjimka Marshaling (ukázka)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
