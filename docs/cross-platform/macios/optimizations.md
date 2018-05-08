---
title: Optimalizace sestavení
description: Tento dokument popisuje různé optimalizace, které se použijí v okamžiku sestavení pro aplikace Xamarin.iOS a Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
dateupdated: 04/16/2018
ms.openlocfilehash: 42d9903e75267a9578fb320b0bc532ad4f0fd71a
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="build-optimizations"></a>Optimalizace sestavení

Tento dokument popisuje různé optimalizace, které se použijí v okamžiku sestavení pro aplikace Xamarin.iOS a Xamarin.Mac.

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>Odebrat UIApplication.EnsureUIThread / NSApplication.EnsureUIThread

Odebere volání [UIApplication.EnsureUIThread] [ 1] (pro Xamarin.iOS) nebo `NSApplication.EnsureUIThread` (pro Xamarin.Mac).

Tato optimalizace změní následující typ kódu:

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

Tato optimalizace vyžaduje linkeru, aby byl povolen a se použije pouze na metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Ve výchozím nastavení je povolený pro verzi sestavení.

Výchozí chování lze přepsat pomocí předání `--optimize=[+|-]remove-uithread-checks` k mtouch/zhr.

[1]: https://developer.xamarin.com/api/member/UIKit.UIApplication.EnsureUIThread/

## <a name="inline-intptrsize"></a>Vložené IntPtr.Size

Inlines konstantní hodnota z `IntPtr.Size` podle cílové platformy.

Tato optimalizace změní následující typ kódu:

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

do následujících (při sestavování pro platformu 64bitová verze):

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

Tato optimalizace vyžaduje linkeru, aby byl povolen a se použije pouze na metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Ve výchozím nastavení je povoleno, pokud cílení na jednom architektura nebo pro sestavení platformy (**Xamarin.iOS.dll**, **Xamarin.TVOS.dll**, **Xamarin.WatchOS.dll** nebo  **Xamarin.Mac.dll**).

Pokud cílení na více architektury, optimalizace vytvoří různé sestavení pro 32bitová verze a 64bitová verze aplikace a mají být zahrnuty v aplikaci efektivně zvýšení velikosti konečné aplikace neklesne bude mít obě verze ho.

Výchozí chování lze přepsat pomocí předání `--optimize=[+|-]inline-intptr-size` k mtouch/zhr.

## <a name="inline-nsobjectisdirectbinding"></a>Vložené NSObject.IsDirectBinding

`NSObject.IsDirectBinding` je vlastnost instance, která určuje, jestli konkrétní instance je typu obálky, nebo ne (typu obálku pro instanci spravovaný; je spravovaný typ, který se mapuje na typ nativní `UIKit.UIView` zadejte mapuje nativního `UIView` typu – jako opak je typ uživatele v takovém případě `class MyUIView : UIKit.UIView` by být typ uživatele).

Je nutné znát hodnotu `IsDirectBinding` při volání do jazyka Objective-C, protože hodnota určuje, která verze `objc_msgSend` používat.

Daný pouze následující kód:

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

Můžeme určit, že v `UIView.SomeProperty` hodnota `IsDirectBinding` není konstantní a nemůže být vložená:

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

Je však možné všechny typy v aplikaci a určit, že neexistují žádné typy, které dědí od `NSUrl`, a proto je bezpečné vložené `IsDirectBinding` hodnotu konstanty `true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

Konkrétně tato optimalizace změní následující typ kódu (Toto je kód vazby pro `NSUrl.AbsoluteUrl`):

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

do následujících (Pokud lze určit, že neexistují žádné měly podtřídy `NSUrl` v aplikaci):

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Tato optimalizace vyžaduje linkeru, aby byl povolen a se použije pouze na metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Je vždy povolena ve výchozím nastavení pro Xamarin.iOS a vždy ve výchozím nastavení zakázaná pro Xamarin.Mac (protože je možné se dynamicky načíst sestavení v Xamarin.Mac, není možné určit, že určité třídy podtřídou nikdy).

Výchozí chování lze přepsat pomocí předání `--optimize=[+|-]inline-isdirectbinding` k mtouch/zhr.

## <a name="inline-runtimearch"></a>Vložené Runtime.Arch

Tato optimalizace změní následující typ kódu:

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

do následujících (při sestavování zařízení):

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

Tato optimalizace vyžaduje linkeru, aby byl povolen a se použije pouze na metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Vždy je povolena ve výchozím nastavení pro Xamarin.iOS (není k dispozici pro Xamarin.Mac).

Výchozí chování lze přepsat pomocí předání `--optimize=[+|-]inline-runtime-arch` k mtouch.

## <a name="dead-code-elimination"></a>Odstranění neaktivní kódu

Tato optimalizace změní následující typ kódu:

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

Také vyhodnotí konstantní porovnání takto:

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

a určit, který výraz `8 == 8` je vždy hodnotu true a snížit na:

```csharp
Console.WriteLine ("Doing this");
```

Toto je výkonné optimalizace při použití spolu s vložené optimalizace, protože ji můžete převést následující typ kódu (Toto je kód vazby pro `NFCIso15693ReadMultipleBlocksConfiguration.Range`):

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

do této (při sestavování 64-bit zařízení a kdy taky moct zajistěte žádné `NFCIso15693ReadMultipleBlocksConfiguration` podtřídy v aplikaci):

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

Kompilátor AOT již moci vyloučit neaktivní kód takto, ale tato optimalizace je provést uvnitř linkeru, což znamená, že linkeru uvidí, že existuje několik metod, které se již nepoužívají a může být odebrán proto (s výjimkou použití jinde) :

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

Tato optimalizace vyžaduje linkeru, aby byl povolen a se použije pouze na metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Ho je vždy povolena ve výchozím nastavení (Pokud je povolená linkeru).

Výchozí chování lze přepsat pomocí předání `--optimize=[+|-]dead-code-elimination` k mtouch/zhr.

## <a name="optimize-calls-to-blockliteralsetupblock"></a>Optimalizace volání BlockLiteral.SetupBlock

Modul runtime Xamarin.iOS/Mac musí znát bloku podpis při vytváření bloku jazyka Objective-C pro spravované delegovat. To může být poměrně náročná operace. Tato optimalizace vypočítat podpis bloku v čase vytvoření buildu a upravit IL volání `SetupBlock` metody, která přijímá podpis jako argument místo. To zabraňuje potřebu výpočet podpis za běhu.

Srovnávacích testů ukazují, že tím se urychlí volání blok faktorem 10 až 15.

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

Tato optimalizace vyžaduje linkeru, aby byl povolen a se použije pouze na metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Ve výchozím nastavení je povolená při používání statické registrátora (v Xamarin.iOS statické registrátora je povoleno ve výchozím nastavení pro zařízení sestavení a v Xamarin.Mac statické registrátora je povoleno ve výchozím nastavení pro verzi sestavení).

Výchozí chování lze přepsat pomocí předání `--optimize=[+|-]blockliteral-setupblock` k mtouch/zhr.

## <a name="optimize-support-for-protocols"></a>Optimalizace podporu pro protokoly

Modul runtime Xamarin.iOS/Mac musí informace o tom, jak spravované typy implementuje jazyka Objective-C protokolech. Tyto informace jsou uloženy v rozhraní (a atributů v těchto rozhraní), což není velmi efektivní formátu, ani je popisný linkeru.

Jedním z příkladů je, že tato rozhraní uložení informací o všech členů protokolu `[ProtocolMember]` atribut, který mimo jiné obsahovat odkazy na parametr typy členů. To znamená, že jednoduše implementace takového rozhraní bude linkeru zachovat všechny typy používané v tomto rozhraní, i pro volitelné členy aplikace nikdy volá nebo implementuje.

Tato optimalizace budou statické registrátora ukládat všechny požadované informace ve formátu efektivní, který používá málo paměti, která je snadno a rychle najít za běhu.

Také se naučit linkeru, že nemusí nutně zachovat tato rozhraní ani žádný z atributů v relaci.

Tato optimalizace vyžaduje linkeru a statické registrátora povolit.

V Xamarin.iOS tato optimalizace je povoleno ve výchozím nastavení je aktivováno linkeru a statické registrátora.

Na Xamarin.Mac optimalizace nikdy ve výchozím nastavení zapnutá, protože Xamarin.Mac podporuje dynamické načítání sestavení a tyto sestavení nemusí byly známé v čase vytvoření buildu (a proto není optimalizovaný).

Výchozí chování lze přepsat pomocí předání `--optimize=-register-protocols` k mtouch/zhr.

## <a name="remove-the-dynamic-registrar"></a>Odebrat dynamické registrátora

Xamarin.iOS i Xamarin.Mac runtime zahrnují podporu pro [registrace spravovaných typů](https://developer.xamarin.com/guides/ios/advanced_topics/registrar/) s modulem runtime jazyka Objective-C. Je možné buď ji provést v okamžiku sestavení nebo za běhu (nebo částečně v čase vytvoření buildu a zbytek za běhu), ale pokud probíhá zcela v čase vytvoření buildu, znamená to, že lze odebrat podpůrné kód pro provádění za běhu. Výsledkem výrazného poklesu velikost aplikace, zejména pro menší aplikace, jako je například rozšíření nebo watchOS.

Tato optimalizace vyžaduje statické registrátora a linkeru povolit.

Linkeru se pokusí zjistit, jestli je dynamické registrátora a v případě, se pokusí odebrat ji odebrat.

Vzhledem k tomu, že Xamarin.Mac podporuje dynamicky načítání sestavení za běhu (což nebyly označuje v čase vytvoření buildu), je možné určit v čase vytvoření buildu, jestli je to bezpečné optimalizace. To znamená, že tato optimalizace je nikdy povoleno ve výchozím nastavení pro Xamarin.Mac aplikace.

Výchozí chování lze přepsat pomocí předání `--optimize=[+|-]remove-dynamic-registrar` k mtouch/zhr.

Pokud výchozí hodnota je potlačena za účelem odebrání dynamické registrátora, linkeru bude generovat upozornění pokud rozpozná, že to není bezpečné (ale dynamické registrátora stále se odstraní).

## <a name="inline-runtimedynamicregistrationsupported"></a>Vložené Runtime.DynamicRegistrationSupported

Inlines hodnota z `Runtime.DynamicRegistrationSupported` určené v čase vytvoření buildu.

Pokud je odebrán dynamické registrátora (najdete v článku [odebrat dynamické registrátora](#remove-the-dynamic-registrar) optimalizace), toto je konstanta `false` hodnotu, jinak je konstanta `true` hodnotu.

Tato optimalizace změní následující typ kódu:

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

do následujících při odebrání dynamické registrátora:

```csharp
throw new Exception ("dynamic registration is not supported");
```

do následujících při dynamické registrátora neodebere:

```csharp
Console.WriteLine ("do something");
```

Tato optimalizace vyžaduje linkeru, aby byl povolen a se použije pouze na metody s `[BindingImpl (BindingImplOptions.Optimizable)]` atribut.

Ho je vždy povolena ve výchozím nastavení (Pokud je povolená linkeru).

Výchozí chování lze přepsat pomocí předání `--optimize=[+|-]inline-dynamic-registration-supported` k mtouch/zhr.

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Předpočítání metody vytvoření spravovaného delegáti bloky jazyka Objective-C

Když jazyka Objective-C volá selektor, přebírá blok jako parametr a potom spravovaného kódu přepsal dané metody Xamarin.iOS / Xamarin.Mac runtime potřebuje k vytvoření delegáta pro tento blok.

Bude obsahovat kód vazby vygenerovaná generátorem vazby `[BlockProxy]` atribut, který určuje typ s `Create` metoda, která tuto operaci provést.

Zadaný kód jazyka Objective-C následující:

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

a následující kód vazby:

```csharp
[BaseType (typeof (NSObject))]
interface ObjCBlockTester
{
    [Export ("classCallback:")]
    void ClassCallback (Action completionHandler);
}
```

Vytvoří generátor:

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

Při volání jazyka Objective-C `[ObjCBlockTester callClassCallback]`, Xamarin.iOS / Xamarin.Mac runtime podívá `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` atribut na parametru. Potom budou prohledány `Create` metoda daný typ a volat tuto metodu pro vytvoření delegát.

Tato optimalizace najdete `Create` metoda v čase vytvoření buildu a statické registrátora vygeneruje kód, který vyhledá metoda za běhu pomocí tokenů metadat místo toho pomocí atributu a reflexe (to je mnohem rychlejší a také umožňuje linkeru Chcete-li odebrat odpovídajícího kódu runtime zmenšit aplikace).

Pokud se nepodařilo najít mmp/mtouch `Create` metoda, pak se zobrazí upozornění MT4174/MM4174 a vyhledávání se provede v době běhu místo.
Nejvíce pravděpodobné příčiny je zapsán ručně vazby kód bez požadované `[BlockProxy]` atributy.

Tato optimalizace vyžaduje statické registrátora povolení.

Ho je vždy povolena ve výchozím nastavení (Pokud se statický registrátora je povolená).

Výchozí chování lze přepsat pomocí předání `--optimize=[+|-]static-delegate-to-block-lookup` k mtouch/zhr.
