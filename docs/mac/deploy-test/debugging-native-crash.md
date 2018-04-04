---
title: Ladění nativního havárií
description: Tato příručka popisuje postup ladění výjimky vzniklé v modulu runtime jazyka Objective-C.
ms.prod: xamarin
ms.assetid: B0C0CE31-2737-4969-8EA5-D39D3333E9C2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 211f85c32fae3ed947e01890916e0a646981a51b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="debugging-a-native-crash"></a>Ladění nativního havárií

## <a name="overview"></a>Přehled

Někdy programování chyby můžete příčina dojde k chybě v nativní modul runtime jazyka Objective-C. Na rozdíl od jazyka C# výjimky nemusíte tyto bodu na určitý řádek v kódu, které můžete zobrazit opravit. V některých případech může být trivial najít a opravit a dalších časy jejich může být velmi obtížné sledovat. 

Pojďme provede několik příkladů skutečných nativní havárií a vezmeme v úvahu.

## <a name="example-1-assertion-failure"></a>Příklad 1: Výraz je neplatný.

Tady je několik prvních řádků havárie v aplikaci jednoduchá testovací (tyto informace budou v **výstupu aplikace** Pad):

```csharp
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.381 NSOutlineViewHottness[79111:1304993] (
    0   CoreFoundation                      0x91888343 __raiseError + 195
    1   libobjc.A.dylib                     0x9a5e6a2a objc_exception_throw + 276
    2   CoreFoundation                      0x918881ca +[NSException raise:format:arguments:] + 138
    3   Foundation                          0x950742b1 -[NSAssertionHandler handleFailureInMethod:object:file:lineNumber:description:] + 118
    4   AppKit                              0x975db476 -[NSTableView _uncachedRectHeightOfRow:] + 373
    5   AppKit                              0x975db2f8 -[_NSTableRowHeightStorage _uncachedRectHeightOfRow:] + 143
    6   AppKit                              0x975db206 -[_NSTableRowHeightStorage _cacheRowHeights] + 167
    7   AppKit                              0x975db130 -[_NSTableRowHeightStorage _createRowHeightsArray] + 226
    8   AppKit                              0x975b5851 -[_NSTableRowHeightStorage _ensureRowHeights] + 73
    9   AppKit                              0x975b5790 -[_NSTableRowHeightStorage computeTableHeightForNumberOfRows:] + 89
    10  AppKit                              0x975b4c38 -[NSTableView _totalHeightOfTableView] + 220
```

Řádky s předponou čísla je trasování zásobníku nativní. Z tohoto uvidíte došlo k havárii někde uvnitř `NSTableView` zpracování výšky řádků. Potom `NSAssertionHandler` aktivuje `NSException (objc_exception_throw)` a vidíme selhání kontrolní výraz:

```csharp
rowHeight error: The value must be > 0 for row 0, but the delegate
<NSOutlineView_ViewDelegate: 0xaa01860> gave -1.000
```

Jakmile se zobrazí, se má poměrně vymazat někteří `NSOutlineViewDelegate` metoda vrací záporné číslo. To se problému:

```csharp
public override nfloat GetRowHeight (NSTableView tableView, nint row)
{
    return -1;
}
```

## <a name="example-2-callback-jumped-into-middle-of-nowhere"></a>Příklad 2: Zpětné volání, které budou vynechány do středu nikde

```csharp
Stacktrace:

 at <unknown> <0xffffffff>
 at (wrapper managed-to-native) MonoMac.AppKit.NSApplication.NSApplicationMain (int,string[]) <IL 0x000a4, 0xffffffff>
 at MonoMac.AppKit.NSApplication.Main (string[]) [0x00041] in /Users/donblas/Programming/xamcore-master/src/AppKit/NSApplication.cs:107
 at NSOutlineViewHottness.MainClass.Main (string[]) [0x00007] in /Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/Main.cs:14
 at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr) <IL 0x00050, 0xffffffff>

Native stacktrace:

Debug info from gdb:

(lldb) command source -s 0 '/tmp/mono-gdb-commands.qrHllW'
Executing commands in '/private/tmp/mono-gdb-commands.qrHllW'.
(lldb) process attach --pid 79229
Process 79229 stopped
Executable module set to "/Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/bin/Debug/NSOutlineViewHottness.app/Contents/MacOS/NSOutlineViewHottness".
Architecture set to: i386-apple-macosx.
(lldb) thread list
Process 79229 stopped
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 thread #2: tid = 0x142790, 0x9af768d2 libsystem_kernel.dylib`kevent64 + 10, queue = 'com.apple.libdispatch-manager'
 thread #3: tid = 0x142792, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #4: tid = 0x142794, 0x9af6fa6a libsystem_kernel.dylib`semaphore_wait_trap + 10
 thread #5: tid = 0x142795, 0x9af75772 libsystem_kernel.dylib`__recvfrom + 10
 thread #6: tid = 0x142799, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #7: tid = 0x14279a, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #8: tid = 0x14279b, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #9: tid = 0x1427f8, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #10: tid = 0x1427fe, 0x9af6fa2e libsystem_kernel.dylib`mach_msg_trap + 10
(lldb) thread backtrace all
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 * frame #0: 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10
   frame #1: 0x986bfb25 libsystem_c.dylib`waitpid$UNIX2003 + 48
   frame #2: 0x028ba36d libmono-2.0.dylib`mono_handle_native_sigsegv(signal=11, ctx=0x03115fe0) + 541 at mini-exceptions.c:2323
   frame #3: 0x0290a8bb libmono-2.0.dylib`mono_arch_handle_altstack_exception(sigctx=<unavailable>, fault_addr=<unavailable>, stack_ovf=0) + 155 at exceptions-x86.c:1159
   frame #4: 0x0280b4fd libmono-2.0.dylib`mono_sigsegv_signal_handler(_dummy=<unavailable>, info=<unavailable>, context=<unavailable>) + 445 at mini.c:6861
   frame #5: 0x91ef403b libsystem_platform.dylib`_sigtramp + 43
   frame #6: 0x9a5dd0bd libobjc.A.dylib`objc_msgSend + 45
   frame #7: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #8: 0x9773ba91 AppKit`-[NSApplication sendAction:to:from:] + 548
   frame #9: 0x9773b82d AppKit`-[NSControl sendAction:to:] + 102
   frame #10: 0x97934d36 AppKit`__26-[NSCell _sendActionFrom:]_block_invoke + 176
   frame #11: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #12: 0x97787975 AppKit`-[NSCell _sendActionFrom:] + 161
   frame #13: 0x979188ea AppKit`-[NSButtonCell _sendActionFrom:] + 55
   frame #14: 0x979366e6 AppKit`__48-[NSCell trackMouse:inRect:ofView:untilMouseUp:]_block_invoke965 + 43
   frame #15: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #16: 0x977a3d00 AppKit`-[NSCell trackMouse:inRect:ofView:untilMouseUp:] + 2815
   frame #17: 0x977a2df4 AppKit`-[NSButtonCell trackMouse:inRect:ofView:untilMouseUp:] + 524
   frame #18: 0x977a233b AppKit`-[NSControl mouseDown:] + 762
   frame #19: 0x97cbc112 AppKit`-[_NSThemeWidget mouseDown:] + 378
   frame #20: 0x97d36d74 AppKit`-[NSWindow _reallySendEvent:] + 12353
   frame #21: 0x977201f9 AppKit`-[NSWindow sendEvent:] + 409
   frame #22: 0x976cdc67 AppKit`-[NSApplication sendEvent:] + 4679
   frame #23: 0x9754807c AppKit`-[NSApplication run] + 1003
```

Jedná se o problém, který byl mnohem obtížnější sledovat. Až se zobrazí `at <unknown> <0xffffffff>` nebo `MonoMac.ObjCRuntime.Runtime.GetNSObject (IntPtr ptr)` v horní části trasování zásobníku spravované, doporučuje se s vámi snažíme provést některé spravovaného kódu k objektu, které bylo shromážděno uvolňování paměti. Ukazuje trasování zásobníku nativní `trackMouse:inRect:ofView:untilMouseUp` do `NSCell _sendActionFrom` tak jsme někde zpracování události klikněte na tlačítko se pokusu zpětného volání do jazyka C# a ukončuje činnost.

Chyby jako Toto jsou obecně obtížné sledovat. Po přidání `GC.Collect(2)` na obslužnou rutinu tlačítko usnadňující sledování tento problém (vynucení uvolnění paměti) Chcete-li problém reprodukovat. Po bisecting příklad nějakou dobu, dokud problém odebrání sekcí kódu smazán, ukázalo jako [této chyby](https://bugzilla.xamarin.com/show_bug.cgi?id=23378):

```csharp
mainWindowController.Window.StandardWindowButton (NSWindowButton.CloseButton).Activated += HandleActivated;
```

`NSButton` Vrácený `StandardWindowButton()` byl shromažďovaných, i když byl zaregistrován událost (které je chyb). Při pokusu o volání tuto událost kliknutím, pokud tlačítko byl uvolnění z paměti jsme dojít k chybě.

I když ho nebyl hlavní příčinu tohoto konkrétního problému, trasování zásobníku jako to může být způsobeno nesprávnou metoda podpisy ve funkcích `[Export]`ed na cíl C. Pokud metoda očekává parametr, jako například `out string` a zadat jako `string`, jsme může dojít k selhání stejným způsobem.

## <a name="example-3-callbacks-and-managed-objects"></a>Příklad 3: Zpětná volání a spravované objekty

Rozhraní API pro mnoho kakao zahrnovat "volané zpět" knihovnou Pokud dojde k nějaké události, s možností reagovat, nebo pokud některá data je nutné k provedení úlohy. Když si může představit především **delegáta** a **DataSource** vzory, existují velkého množství rozhraní API, které pracují tímto způsobem. Například při přepsání metody `NSView` a vložte ji do vizuálním stromu, očekáváte, že AppKit zavolat zpět při určité události.

Ve většině případů Xamarin.Mac správně zabráníte spravovaného objektu cílem těchto zpětná volání uklizeny při jejich stále může být zpětné volání. Ale zřídka chyby vazba může dojít k přerušení to. Pokud k tomu dojde, můžete zjistit nepříjemným havárií podobná této:

```csharp
Thread 0 Crashed:: Dispatch queue: com.apple.main-thread

0   libsystem_kernel.dylib              0x98c2f69a __pthread_kill + 10
1   libsystem_pthread.dylib             0x90341f19 pthread_kill + 101
2   libsystem_c.dylib                   0x9453feee abort + 156
3   libmonosgen-2.0.dylib               0x020bfba5 mono_handle_native_sigsegv + 757
4   libmonosgen-2.0.dylib               0x0210b812 mono_arch_handle_altstack_exception + 162
5   libmonosgen-2.0.dylib               0x0200c55e mono_sigsegv_signal_handler + 446
6   libsystem_platform.dylib            0x9513003b _sigtramp + 43
7   ???                                 0xffffffff 0 + 4294967295
8   libmonosgen-2.0.dylib               0x0200c3a0 mono_sigill_signal_handler + 48
9   com.apple.AppKit                    0x99f76041 -[NSView setFrame:] + 448
10  com.apple.AppKit                    0x9a1fd4ea -[NSToolbarView adjustToWindow:attachedToEdge:] + 198
11  com.apple.AppKit                    0x9a1fd414 -[NSToolbar _adjustViewToWindow] + 68
12  com.apple.AppKit                    0x9a01eb0d -[NSToolbar _windowWillShowToolbar] + 79

```

Tento průvodce vám pomůže sledovat chyby této povahy pokud jejich oříznout, správně sestavy je tak, aby odstraněny a práci za je ve službě se váš kód do té doby.

### <a name="locating"></a>Hledání

V téměř každé případu se chyby této povahy primární symptomem je nativní havárií, obvykle se něco podobného jako `mono_sigsegv_signal_handler`, nebo `_sigtrap` v horní rámce zásobníku. Kakao pokusu zpětného volání do kódu jazyka C#, stiskne shromážděných objekt uvolňování paměti a chybám. Ale nemusí být vždy havárie se tyto symboly je způsobeno problémem vazby takto, budete muset provést některé další tápat a ověřte, zda se že jedná o problém. 

Díky obtížné sledovat tyto chyby je, že pouze nastávají **po** uvolňování paměti má vyřazen u daného objektu. Pokud se domníváte, že jste dosáhl jednu z těchto chyb, přidejte následující kód někde v pořadí spouštění:

```csharp
new System.Threading.Thread (() => 
{
    while (true) {
         System.Threading.Thread.Sleep (1000);
         GC.Collect ();
    }
}).Start (); 
```

Tato akce vynutí aplikace na spouštění systému uvolňování paměti za sekundu. Znovu spusťte aplikaci a pokuste se znovu provést chyb. Pokud jste místo náhodně havárií okamžitě, nebo konzistentní, jste na správné sledování.

### <a name="reporting"></a>Generování sestav

Dalším krokem je problém nahlásit Xamarin, vazba odstraněny v budoucích verzích. Pokud jste obchodní nebo držitel licence enterprise, otevřete lístek v 

[http://xamarin.com/support](http://xamarin.com/support)

Jinak vyhledejte existující problém:

- Zkontrolujte [Xamarin.Mac fóra](https://forums.xamarin.com/categories/mac)
- Hledání [problém úložiště](https://github.com/xamarin/xamarin-macios/issues)
- Před přepnutím na Githubu problémy, problémy Xamarin byly sledovány pro [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Pro párování problémy, vyhledávání existuje.
- Pokud nemůžete najít odpovídající problém, prosím soubor nový problém v [potíže úložiště GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub problémy jsou všechny veřejné. Není možné skrýt komentáře nebo přílohy. 

Uveďte co nejvíc následující kroky jako možné:

- Jednoduchý příklad reprodukci problému. Toto je **neocenitelnou pomocí** kde je to možné. 
- Trasování zásobníku úplné havárie.
- C# kódu havárii.   

### <a name="working-around"></a>Alternativní řešení

Jakmile jste můžete sledovat dolů problém, může být jednoduchý opravy se vyřešit problém, dokud odstraněny vazby. Cílem je, aby nedošlo k objektu (**zobrazení**, **delegáta**, **DataSource**), nesprávně má být uvolněn mimo paměti udržováním otevřete odkaz.

Jednoduché případech, kde existuje pouze jedna instance objektu, změnit kód z tohoto:

```csharp
void AddObject ()
{
    item.View = new MyView ();
    ...
}
```

Pomocí statické proměnné, jako je tato:

```csharp
static NSObject view;
...

void AddObject ()
{
    view = new MyView ();
    item.View = view;
    ...
}
```

V případech, kde více instancí mohou být vytvořeny, statického `HashSet` mohou být použity:

```csharp
static HashSet<NSObject> collection = new HashSet<NSObject> ();
...

void AddObject ()
{
    item.View = new MyView ();
    collection.Add (item.View );
    ...
}
```

Jakmile byl opraven vazby a jste upgrade na verzi Xamarin.Mac, která zahrnuje opravu, se může odebrat kód řešení.

## <a name="exception-bubbling-and-objective-c"></a>Výjimka šíření a jazyka Objective-C

Výjimka C# "řídicí" spravovaný kód k metodě volání jazyka Objective-C byste nikdy neměli povolit. Pokud tak učiníte, výsledky nejsou definovány ale obvykle zahrnují chybám. Obecně platí, provedeme všechno, co můžeme k bublin až užitečné informace pro nativní a spravovaná havárií pomohou rychle vyřešit vaše potíže.

Bez příliš zabřednete technické důvody proč, nastavení infrastruktury pro zachycení spravovaných výjimek na každé hranici spravované/nativní je jiný trivially nákladné a existují _mnoho_ přechodů, který nastat v Mnoho běžných operací. Mnoho operací, konkrétně na ta, která zahrnují vlákna uživatelského rozhraní musí dokončit rychle nebo vaše aplikace bude trhaně a mít nepřijatelné výkonové charakteristiky. Mnoho z těchto zpětná volání provést velmi jednoduché věcí, které zřídka mají možnost vyvolání, tak tato dodatečná režie by být nákladné a nepotřebné v těchto případech.

Proto by nepodporujeme nastavení ty, zkuste / zachytí za vás. Pro místa, kde code nemá netriviální věcí (i mimo vyslovení vrácení logických výrazů nebo jednoduchý matematické), můžete zkusit catch sami. 

