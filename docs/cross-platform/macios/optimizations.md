---
title: Optimalizace buildu
description: Tento dokument popisuje různé optimalizace, které se použijí v okamžiku sestavení pro aplikace pro Xamarin.iOS a Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: 432511a35f9f285d2c0060b5521256f834bb35ea
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351636"
---
# <a name="build-optimizations"></a>Optimalizace buildu

Tento dokument popisuje různé optimalizace, které se použijí v okamžiku sestavení pro aplikace pro Xamarin.iOS a Xamarin.Mac.

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>Odebrat UIApplication.EnsureUIThread / NSApplication.EnsureUIThread

Odebere volání [UIApplication.EnsureUIThread] [ 1] (pro Xamarin.iOS) nebo `NSApplication.EnsureUIThread` (pro Xamarin.Mac).

Tato optimalizace se změní následující typ kódu:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    global::UIKit.UIApplication.EnsureUIThread ();
    // ...
}
```

do následující:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    // ...
}
```

Vyžaduje tato optimalizace linkeru, aby byly povoleny a platí jenom pro metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Ve výchozím nastavení je povolená pro verzi sestavení.

Výchozí chování můžete přepsat tím, že předáte `--optimize=[+|-]remove-uithread-checks` k mtouch/mmp.

[1]: https://developer.xamarin.com/api/member/UIKit.UIApplication.EnsureUIThread/

## <a name="inline-intptrsize"></a>Vložené IntPtr.Size

Inlines konstantní hodnoty z `IntPtr.Size` podle cílové platformy.

Tato optimalizace se změní následující typ kódu:

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

do následující (při sestavování pro 64bitovou platformu):

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

Vyžaduje tato optimalizace linkeru, aby byly povoleny a platí jenom pro metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Ve výchozím nastavení je povoleno, pokud je zaměřen na architektura samostatného nebo pro sestavení platformy (**Pokud**, **Xamarin.TVOS.dll**, **Xamarin.WatchOS.dll** nebo  **Xamarin.Mac.dll**).

Pokud cílení na více architekturách, tato optimalizace se vytvoří různá sestavení pro 32bitová verze a 64bitová verze aplikace a bude mít obě verze mají být zahrnuty v aplikaci, efektivně zvýšení velikosti konečná aplikace neklesne ho.

Výchozí chování můžete přepsat tím, že předáte `--optimize=[+|-]inline-intptr-size` k mtouch/mmp.

## <a name="inline-nsobjectisdirectbinding"></a>Vložené NSObject.IsDirectBinding

`NSObject.IsDirectBinding` je vlastnost instance, která určuje, zda konkrétní instance je typu obálky, nebo ne (typ obálky se spravovaným typem, který se mapuje na nativní typ; pro instanci spravovanou `UIKit.UIView` zadejte mapuje na nativní `UIView` typ - opak je třeba uživatelský typ v tomto případě `class MyUIView : UIKit.UIView` by bylo třeba uživatelský typ).

Je potřeba znát hodnotu `IsDirectBinding` při volání do Objective-C, protože hodnota určuje, která verze `objc_msgSend` používat.

Uveden pouze následující kód:

```csharp
class UIView : NSObject {
    public virtual string SomeProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class NSUrl : NSObject {
    public virtual string SomeOtherProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class MyUIView : UIView {
}
```

Můžeme určit, že v `UIView.SomeProperty` hodnotu `IsDirectBinding` není konstantní a se nedá vložit:

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

Je však možné podívat všechny typy v aplikaci a určit, že neexistují žádné typy, které dědí z `NSUrl`, a proto je bezpečný pro vložení `IsDirectBinding` hodnotu na konstantu `true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

Konkrétně tato optimalizace se změní následující typ kódu (to je vazební kód pro `NSUrl.AbsoluteUrl`):

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

do následující (Pokud lze určit, že neexistují žádné podtřídy třídy `NSUrl` v aplikaci):

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Vyžaduje tato optimalizace linkeru, aby byly povoleny a platí jenom pro metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Ve výchozím nastavení vždy povolena pro Xamarin.iOS a vždy ve výchozím nastavení zakázané pro Xamarin.Mac (protože je možné dynamicky načíst sestavení v Xamarin.Mac, není možné určit, že se nikdy rozčlenění určité třídy).

Výchozí chování můžete přepsat tím, že předáte `--optimize=[+|-]inline-isdirectbinding` k mtouch/mmp.

## <a name="inline-runtimearch"></a>Vložené Runtime.Arch

Tato optimalizace se změní následující typ kódu:

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

do následující (při sestavování pro zařízení):

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

Vyžaduje tato optimalizace linkeru, aby byly povoleny a platí jenom pro metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Ve výchozím nastavení je vždy povoleno pro Xamarin.iOS (není k dispozici pro Xamarin.Mac).

Výchozí chování můžete přepsat tím, že předáte `--optimize=[+|-]inline-runtime-arch` k mtouch.

## <a name="dead-code-elimination"></a>Odstranění neaktivní kód

Tato optimalizace se změní následující typ kódu:

```csharp
if (true) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

do:

```csharp
Console.WriteLine ("Doing this");
```

Také se vyhodnotí konstanta porovnání, následujícím způsobem:

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

a určí, že výraz `8 == 8` je vždy hodnotu true a snížit tak:

```csharp
Console.WriteLine ("Doing this");
```

Výkonné optimalizace spolupráci s vkládání optimalizace, důvodem je, že ji můžete převést následující typ kódu (to je vazební kód pro `NFCIso15693ReadMultipleBlocksConfiguration.Range`):

```csharp
NSRange ret;
if (IsDirectBinding) {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret (out ret, this.Handle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    }
} else {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret (out ret, this.SuperHandle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    }
}
return ret;
```

do tohoto (vývoj aplikací pro zařízení s 64bitovým kompilátorem a data také schopni zajistit nejsou žádné `NFCIso15693ReadMultipleBlocksConfiguration` podtřídy v aplikaci):

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

Kompilátor AOT již má eliminovat mrtvý kód tímto způsobem, ale tato optimalizace se provádí v linkeru, což znamená, že má linker uvidíte, že existuje několik metod, které nepoužívají a může proto být odebrán (Pokud nepoužíváte někde jinde) :

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

Vyžaduje tato optimalizace linkeru, aby byly povoleny a platí jenom pro metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Je vždy povoleno ve výchozím nastavení (Pokud je povolena linker).

Výchozí chování můžete přepsat tím, že předáte `--optimize=[+|-]dead-code-elimination` k mtouch/mmp.

## <a name="optimize-calls-to-blockliteralsetupblock"></a>Optimalizace volání BlockLiteral.SetupBlock

Modul runtime Xamarin.iOS/Mac musí znát signatura bloku při vytváření blok Objective-C pro spravované delegovat. To může být velmi náročná operace. Tato optimalizace se vypočítat podpis blok v okamžiku sestavení a upravte IL pro volání `SetupBlock` metodu, která jako argument přijímá podpis. Tím se vyhnete nutnosti výpočtu podpisu v době běhu.

Srovnávací testy zobrazit, že urychluje volání blok faktorem 10 až 15.

Transformuje následující [kód](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211):

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlock (callback, completionHandler);
    // ...
}
```

do:

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlockImpl (callback, completionHandler, true, "v@?B");
    // ...
}
```

Vyžaduje tato optimalizace linkeru, aby byly povoleny a platí jenom pro metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Ve výchozím nastavení je povoleno při používání statické doménový Registrátor (v Xamarin.iosu statické doménový Registrátor je povolené ve výchozím nastavení u sestavení zařízení a v statické doménový Registrátor je povoleno standardně pro vydání verze Xamarin.Mac sestavení).

Výchozí chování můžete přepsat tím, že předáte `--optimize=[+|-]blockliteral-setupblock` k mtouch/mmp.

## <a name="optimize-support-for-protocols"></a>Optimalizace podpora protokolů

Modul runtime Xamarin.iOS/Mac potřebuje informace o protokolech implementuje Objective-C spravovaných typů. Tyto informace jsou uloženy v rozhraní (a atributů v těchto rozhraní), která není velmi efektivní formátu ani není přívětivá linkeru.

Jedním z příkladů je, že tato rozhraní ukládání informací o všechny členy protokolu v `[ProtocolMember]` atribut, který mimo jiné obsahují odkazy na typy parametrů těchto členů. To znamená, že jednoduše implementovat toto rozhraní způsobí, že má linker zachovat všechny typy použité v tomto rozhraní, i pro volitelnými členy aplikace nikdy volání nebo implementuje.

Tato optimalizace způsobí, že statická doménový Registrátor uložit všechny požadované informace do efektivní formát, který používá málo paměti, který se snadno a rychle najdete v době běhu.

Také se dozvíte linkeru, že nemusí nutně zachovat tato rozhraní ani žádný z souvisejících atributů.

Vyžaduje tato optimalizace linkeru a statické doménový Registrátor povolit.

V Xamarin.iOS tyto optimalizace povolen ve výchozím nastavení, pokud jsou povolené linkeru a statické doménový Registrátor.

Na Xamarin.Mac tato optimalizace je nikdy standardně povolená, protože Xamarin.Mac podporuje dynamické načítání sestavení a tato sestavení nemusí byly známé v okamžiku sestavení (a tedy neoptimalizovaná).

Výchozí chování můžete přepsat tím, že předáte `--optimize=-register-protocols` k mtouch/mmp.

## <a name="remove-the-dynamic-registrar"></a>Odebrat dynamické doménový Registrátor

Xamarin.iOS a Xamarin.Mac runtime zahrnují podporu pro [registrace spravovaných typů](https://developer.xamarin.com/guides/ios/advanced_topics/registrar/) s modulem runtime Objective-C. Buď lze provést, v okamžiku sestavení nebo za běhu (nebo částečně v době sestavení a zbytek za běhu), ale pokud provádí kompletně v okamžiku sestavení, znamená to, že je možné odebrat podpůrný kód pro provádění za běhu. Výsledkem je výrazné snížení velikosti aplikace, zejména pro menší aplikace, jako je například rozšíření nebo aplikace pro watchOS.

Tyto optimalizace vyžaduje statické doménový Registrátor a propojovací program povolit.

Linker se pokusí zjistit, jestli je bezpečná pro odebrání dynamické registrátora a if, pokusí se ho odebrat.

Protože Xamarin.Mac podporuje dynamicky načítání sestavení za běhu (která nebyla známa v okamžiku sestavení), není možné určit v okamžiku sestavení, zda je to bezpečné optimalizace. To znamená, že tato optimalizace se nikdy povoleno standardně pro aplikace Xamarin.Mac.

Výchozí chování můžete přepsat tím, že předáte `--optimize=[+|-]remove-dynamic-registrar` k mtouch/mmp.

Pokud výchozí hodnota je přepsána odebrat dynamické registrátora, linker se vygenerovat upozornění, pokud zjistí, že není bezpečné (ale dynamické doménový Registrátor stále se odstraní).

## <a name="inline-runtimedynamicregistrationsupported"></a>Vložené Runtime.DynamicRegistrationSupported

Inlines hodnotu z `Runtime.DynamicRegistrationSupported` jak určí v okamžiku sestavení.

Pokud je odebrán dynamické doménový Registrátor (naleznete v tématu [odebrat dynamické doménový Registrátor](#remove-the-dynamic-registrar) optimalizace), toto je konstanta `false` hodnota, v opačném případě je konstanta `true` hodnotu.

Tato optimalizace se změní následující typ kódu:

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

do následujících při odebrání dynamické doménový Registrátor:

```csharp
throw new Exception ("dynamic registration is not supported");
```

do následující, když se neodebere dynamické doménový Registrátor:

```csharp
Console.WriteLine ("do something");
```

Vyžaduje tato optimalizace linkeru, aby byly povoleny a platí jenom pro metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Je vždy povoleno ve výchozím nastavení (Pokud je povolena linker).

Výchozí chování můžete přepsat tím, že předáte `--optimize=[+|-]inline-dynamic-registration-supported` k mtouch/mmp.

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Předpočítání metody pro vytvoření spravované delegáty pro bloky Objective-C

Když Objective-C volá selektor, který přebírá bloku jako parametr a potom spravovaný kód přepsal metodu, Xamarin.iOS / Xamarin.Mac runtime potřebuje k vytvoření delegáta pro daný blok.

Vazební kód vygenerovaný generátor vazeb zahrne `[BlockProxy]` atribut, který určuje typ s `Create` metodu, která to lze provést.

Daný následující kód Objective-C:

```objc
@interface ObjCBlockTester : NSObject {
}
-(void) classCallback: (void (^)())completionHandler;
-(void) callClassCallback;
@end

@implementation ObjCBlockTester
-(void) classCallback: (void (^)())completionHandler
{
}

-(void) callClassCallback
{
    [self classCallback: ^()
    {
        NSLog (@"called!");
    }];
}
@end
```

a následující vazební kód:

```csharp
[BaseType (typeof (NSObject))]
interface ObjCBlockTester
{
    [Export ("classCallback:")]
    void ClassCallback (Action completionHandler);
}
```

Vytvoří generátor kódu:

```csharp
[Register("ObjCBlockTester", true)]
public unsafe partial class ObjCBlockTester : NSObject {
    // unrelated code...

    [Export ("callClassCallback")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public virtual void CallClassCallback ()
    {
        if (IsDirectBinding) {
            ApiDefinition.Messaging.void_objc_msgSend (this.Handle, Selector.GetHandle ("callClassCallback"));
        } else {
            ApiDefinition.Messaging.void_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("callClassCallback"));
        }
    }
    
    [Export ("classCallback:")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public unsafe virtual void ClassCallback ([BlockProxy (typeof (Trampolines.NIDActionArity1V0))] System.Action completionHandler)
    {
        // ...
        
    }
}

static class Trampolines
{
    [UnmanagedFunctionPointerAttribute (CallingConvention.Cdecl)]
    [UserDelegateType (typeof (System.Action))]
    internal delegate void DActionArity1V0 (IntPtr block);

    static internal class SDActionArity1V0 {
        static internal readonly DActionArity1V0 Handler = Invoke;
        
        [MonoPInvokeCallback (typeof (DActionArity1V0))]
        static unsafe void Invoke (IntPtr block) {
            var descriptor = (BlockLiteral *) block;
            var del = (System.Action) (descriptor->Target);
            if (del != null)
                del (obj);
        }
    }
    
    internal class NIDActionArity1V0 {
        IntPtr blockPtr;
        DActionArity1V0 invoker;
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe NIDActionArity1V0 (BlockLiteral *block)
        {
            blockPtr = _Block_copy ((IntPtr) block);
            invoker = block->GetDelegateForBlock<DActionArity1V0> ();
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        ~NIDActionArity1V0 ()
        {
            _Block_release (blockPtr);
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe static System.Action Create (IntPtr block)
        {
            if (block == IntPtr.Zero)
                return null;
            if (BlockLiteral.IsManagedBlock (block)) {
                var existing_delegate = ((BlockLiteral *) block)->Target as System.Action;
                if (existing_delegate != null)
                    return existing_delegate;
            }
            return new NIDActionArity1V0 ((BlockLiteral *) block).Invoke;
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        unsafe void Invoke ()
        {
            invoker (blockPtr);
        }
    }
}
```

Když volá Objective-C `[ObjCBlockTester callClassCallback]`, Xamarin.iOS / Xamarin.Mac runtime se podíváme na `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` atribut parametru. Pak budou prohledány `Create` metoda v typu a volat tuto metodu pro vytvoření delegáta.

Tato optimalizace najdou `Create` metoda v době sestavení a statické doménový Registrátor vygeneruje kód, který vyhledává metoda za běhu pomocí tokeny metadat, namísto použití atributu a reflexe (to je mnohem rychlejší a také umožňuje linkeru Chcete-li odebrat odpovídající kód modulu runtime zmenšit aplikace).

Pokud nemůže najít mmp/mtouch `Create` metodu, zobrazí upozornění MT4174/MM4174 a vyhledávání se provede v době běhu místo.
Nejvíce nejpravděpodobnější příčinou je zapsán ručně vazební kód bez požadované `[BlockProxy]` atributy.

Tyto optimalizace vyžaduje statické doménový Registrátor povolit.

Je vždy povoleno ve výchozím nastavení (za předpokladu, statické doménový Registrátor je povoleno).

Výchozí chování můžete přepsat tím, že předáte `--optimize=[+|-]static-delegate-to-block-lookup` k mtouch/mmp.
