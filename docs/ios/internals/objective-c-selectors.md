---
title: Selektory Objective-C v Xamarin.iosu
description: Tento dokument popisuje, jak pracovat s selektory Objective-C z jazyka C#. Popisuje, jak vyvolat selektory a technické aspekty, které musí vzít v úvahu, když to tak uděláte.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/12/2017
ms.openlocfilehash: 3083770fd2874eca317585b6bf949f3efe56f879
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351727"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Selektory Objective-C v Xamarin.iosu

Jazyka Objective-C je založena na *selektory*. Selektor je zpráva, kterou je možné odeslat do objektu nebo *třídy*. [Xamarin.iOS](~/ios/internals/api-design/index.md) mapy instance selektory metody instance a selektory pro statické metody třídy.

Na rozdíl od normální funkce jazyka C (a stejně jako členských funkcí jazyka C++), nejde volat přímo selektor pomocí [P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/).
(*Jste si poznamenali*: teoreticky můžete použít deklarace P/Invoke pro nevirtuální členských funkcí jazyka C++, ale je třeba si dělat starosti křížový kompilátor pozměnění názvu, což je také k celé řadě bolest lépe ignorováno.) Místo toho selektory se odesílají do třídy Objective-C nebo instance pomocí [ `objc_msgSend` funkce](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend).

Může být pro vás [Tato příručka pomůže v Objective-C pro zasílání zpráv](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html) užitečné.

<a name="Example" />

## <a name="example"></a>Příklad

Předpokládejme, že chcete vyvolat [-[NSString sizeWithFont:forWidth:lineBreakMode:]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:) selektor.
Deklarace (v dokumentaci Apple) je:

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  Návratový typ je *CGSize* pro sjednocené rozhraní API.
-  *Písmo* parametr je [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (a typ (nepřímo) odvozený od [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ) a proto je namapována na [System.IntPtr](xref:System.IntPtr) .
-  *Šířka* parametr, *CGFloat* , je namapována na *nfloat*.
-  *LineBreakMode* parametr, *UILineBreakMode* , již byl svázán v Xamarin.iosu jako [UILineBreakMode výčet](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/) .


Všech součástí dohromady, a chceme objc_msgSend deklarace, který odpovídá:

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

Bude potřeba ji deklarovat:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Po deklaraci, můžeme ho vyvolat Jakmile budeme mít příslušné parametry:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode);
```

Vrácená hodnota je struktura, která byla velikost menší než 8 bajtů (jako jsou starší `SizeF` nepoužili přepnutí na rozhraní Unified API) ve výše uvedeném kódu mají běžet na simulátoru ale chyb na zařízení. Proto pro volání, která vrací hodnotu menší než 8 bitů velikosti, proto jsme selektor *také* potřeba deklarovat `objc_msgSend_stret()` funkce:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Volání by pak můžou stát:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode);
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode);
```


<a name="Invoking_a_Selector" />

## <a name="invoking-a-selector"></a>Vyvolání selektor

Vyvolání selektor má tři kroky:

1.  Získá selektor cíl.
1.  Získání názvu selektoru.
1.  Volání objc_msgSend() s příslušnými argumenty.


<a name="Selector_Targets" />

### <a name="selector-targets"></a>Výběr cíle

Výběr cíle je instancí objektu nebo třídu Objective-C. Pokud cíl je instance a pochází z typu vazby Xamarin.iOS, použijte [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) vlastnost.

Pokud je cílem třídy, použijte [ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) k získání odkazu na instanci třídy, použije [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) vlastnost.


<a name="Selector_Names" />

### <a name="selector-names"></a>Selektor názvy

Selektor názvy jsou uvedeny v dokumentaci Apple. Například [UIKit NSString rozšiřující metody](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html) zahrnují [sizeWithFont:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:) a [sizeWithFont:forWidth:lineBreakMode:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:). Použití dvojteček vložené a koncové jsou důležité a část názvu selektoru.

Jakmile budete mít názvu selektoru, můžete vytvořit [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) instance pro něj.


<a name="Calling_objc_msgSend()" />

### <a name="calling-objcmsgsend"></a>Volání objc_msgSend()

 `objc_msgSend()` se používá k odeslání zprávy (výběr) na objekt. Tato řada funkcí přebírá vyžaduje minimálně dva argumenty: cíl selektor (instance nebo třídy zpracování), modulu pro výběr sama a pak žádné argumenty, které jsou vyžadované pro konkrétní výběr. Instance a selektor argumenty musí být `System.IntPtr`, a všechny zbývající argumenty musí odpovídat typu modulu pro výběr očekává, třeba `nint` pro `int`, nebo `System.IntPtr` pro všechny `NSObject`-odvozené typy. Použití [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) vlastnost k získání `IntPtr` pro instanci typu Objective-C.

Bohužel je více než jeden `objc_msgSend()` funkce.

Použití [ `objc_msgSend_stret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) pro selektory, které vracejí strukturu.
Aby vše zůstalo "zajímavé", na ARM to zahrnuje všechny návratové typy, které jsou *není* výčet nebo předdefinované typy jazyka C (char, short, int, long float, double). Na x86 (simulátor) to je potřeba použít pro všechny struktury větší než 8 bajtů. velikost. (CGSize--použitých v příkladu výše – přesně je 8 bajtů a proto nepoužívá `objc_msgSend_stret()` v simulátoru.)

Použití [ `objc_msgSend_fpret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret) pro selektory, které vracejí plovoucí desetinnou čárkou na x86 pouze. Tato funkce není potřeba použít na ARM; Místo toho použijte `objc_msgSend()`.

Hlavní [objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend) funkce se používá pro všechny ostatní selektorů.

Jakmile jste se rozhodli který `objc_msgSend()` funkce potřebné k volání (a může být více než jedna, např. pro zařízení a simulátoru), můžete použít normální [ `[DllImport]` ](xref:System.Runtime.InteropServices.DllImportAttribute) metody, chcete-li deklarovat funkci pro pozdější volání.

Sada předem vytvořené `objc_msgSend()` deklarace lze nalézt v [ `ObjCRuntime.Messaging` ](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/).


<a name="ugly" />

## <a name="the-ugly"></a>Bez zbytečných prvků

Objective-C má tři typy z `objc_msgSend` metody: jeden pro pravidelné volání, jeden pro volání, které vracejí hodnoty s plovoucí desetinnou (pouze x86) a jeden pro volání, které vracejí hodnoty struktury. Ten obsahuje příponu `_stret` v `ObjCRuntime.Messaging`.

Pokud vyvoláváte metodu, která vrátí určité struktury (pravidla jsou popsány níže), je nutné vyvolat metodu s návratovou hodnotou jako první parametr jako hodnotu mimo takto:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Tady získat bez zbytečných prvků věci a protože pravidla pro musí při použití _stret_ se liší na X86 a ARM, pokud chcete, aby vazby pro práci na simulátoru a zařízení, musíte přidat kód, který vypadá takto:

```csharp
if (Runtime.Arch == Arch.DEVICE){
    PointF ret;

    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);

    return ret;
} else
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
```

### <a name="using-the-objcmsgsendstret-method"></a>Použití objc\_msgSend\_stret – metoda

Pravidlo pro použití [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) jsou podobné pro **ARM**:

-  Některá hodnota typu, který není výčet nebo některé základní typy výčtu (int, byte, short, long, double, float).


Pravidlo pro použití [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) jsou podobné pro **X86**:

-  Některá hodnota typu, který není výčet, nebo některé základní typy výčtu (int, byte, short, long, double, float) a jehož nativní velikost je větší než 8 bajtů.


### <a name="creating-your-own-signatures"></a>Vytváří se vlastní podpisy.

Následující [gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) slouží k vytvoření vlastních podpisy v případě potřeby.



## <a name="related-links"></a>Související odkazy

- [Ukázka selektorů.](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
