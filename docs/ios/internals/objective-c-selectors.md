---
title: Selektory jazyka Objective-C
ms.topic: article
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 3fa01d8f28dc1c86f9d4a8ee4d9fc0a9cdb8ee9c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="objective-c-selectors"></a>Selektory jazyka Objective-C

Podle jazyka Objective-C *selektory*. Selektor je zprávu, která lze odeslat buď do objektu nebo *třída*. [Xamarin.iOS](~/ios/internals/api-design/index.md) mapy instance selektory metod, které a selektorů statické metody třídy.

Na rozdíl od normální funkcí jazyka C (a jako C++ členské funkce), nejde volat přímo selektor pomocí [P/Invoke](http://www.mono-project.com/Dllimport).
(*z produkce*: teoreticky můžete použít P/Invoke pro bez virtuální funkce člen C++, ale potřebovali byste si dělat starosti za kompilátoru úprava názvu, který je do světa problémové lépe ignorovat.) Místo toho selektory se odesílají do třídy jazyka Objective-C nebo pomocí [ `objc_msgSend` funkce](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend).

Možná [tuto příručku užitečné zasílání zpráv jazyka Objective-C](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html) užitečné.

<a name="Example" />

## <a name="example"></a>Příklad

Předpokládejme, že chcete vyvolat [-[NSString sizeWithFont:forWidth:lineBreakMode:]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:) selektor.
Deklarace (od společnosti Apple dokumentace) je:

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  Návratový typ *CGSize* pro unifikované API.
-  *Písma* parametr [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (a z (nepřímo) odvozený typ. [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ) a proto je namapována na [System.IntPtr](https://developer.xamarin.com/api/type/System.IntPtr/) .
-  *Šířka* parametr, *CGFloat* , je namapovaný na *nfloat*.
-  *LineBreakMode* parametr, *UILineBreakMode* , již byla svázána v Xamarin.iOS jako [UILineBreakMode výčtu](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/) .


Všech součástí dohromady a chceme, aby objc_msgSend deklaraci, která odpovídá:

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

Budeme muset deklarujte ji:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Jakmile deklarovat, jsme ji použít po máme příslušné parametry:

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

Vrácená hodnota je strukturu, která byla menší než velikost 8 bajtů (jako jsou starší `SizeF` používá před přepnutím do rozhraní API Unified) ve výše uvedeném kódu mají běžet na simulátoru ale zhroucené na zařízení. Proto k volání selektor, která vrátí hodnotu menší než 8 bitů velikost, proto jsme *také* potřeba deklarovat `objc_msgSend_stret()` funkce:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Volání by pak mohou stát:

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
1.  Získáte název selektor.
1.  Volání objc_msgSend() s odpovídající argumenty.


<a name="Selector_Targets" />

### <a name="selector-targets"></a>Výběr cíle

Selektor cíl je instanci objektu nebo třídu jazyka Objective-C. Pokud cíl je instance a vychází z typu vázané Xamarin.iOS, použijte [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) vlastnost.

Pokud cíl třídu, použijte [ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) Pokud chcete získat odkaz na instanci třídy, použije [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) vlastnost.


<a name="Selector_Names" />

### <a name="selector-names"></a>Selektor názvy

Selektor názvy jsou uvedeny v dokumentaci společnosti Apple. Například [UIKit NSString rozšiřující metody](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html) zahrnují [sizeWithFont:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:) a [sizeWithFont:forWidth:lineBreakMode:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:). Dvojtečky embedded a koncové jsou důležité a část názvu selektor.

Až budete mít název pro výběr, můžete vytvořit [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) instance pro ni.


<a name="Calling_objc_msgSend()" />

### <a name="calling-objcmsgsend"></a>Volání metody objc_msgSend()

 `objc_msgSend()` se používá k odeslání zprávy (selektor) k objektu. Tato řada funkcí trvá vyžaduje minimálně dva argumenty: cíl selektor (instance nebo třídy zpracování), modulu pro výběr sám sebe a potom všechny argumenty, které jsou potřeba pro konkrétní selektor. Argumenty instance a selektor musí být `System.IntPtr`, a všechny zbývající argumenty musí shodovat s typem modulu pro výběr očekává, například `nint` pro `int`, nebo `System.IntPtr` pro všechny `NSObject`-odvozených typů. Použití [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) vlastnost k získání `IntPtr` pro instanci typu jazyka Objective-C.

Bohužel je více než jedna `objc_msgSend()` funkce.

Použití [ `objc_msgSend_stret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) pro selektory, které vrací struktury.
"Zajímavé", na ARM to z důvodu zjednodušení zahrnuje všechny návratové typy, které jsou *není* výčet nebo předdefinované typy jazyka C (char, short, int, long, float, double). Na x86 (simulátor) to je potřeba použít pro všechny struktury větší než velikost 8 bajtů. (CGSize – použít v příkladu nahoře – přesně je 8 bajtů a proto nepoužívá `objc_msgSend_stret()` v simulátoru.)

Použití [ `objc_msgSend_fpret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret) pro selektory, které vrací plovoucí bodu na x86 pouze hodnotu. Tato funkce není nutné k použití v ARM; Místo toho použijte `objc_msgSend()`.

Hlavní [objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend) funkce se používá pro všechny ostatní selektorů.

Jakmile jste se rozhodli, který `objc_msgSend()` byly funkce je třeba volat (a může být více než jeden, např. v případě simulátoru a zařízení), můžete použít normální [ `[DllImport]` ](https://developer.xamarin.com/api/type/System.Runtime.InteropServices.DllImportAttribute/) metoda deklarovat funkci pro pozdější volání.

Sadu předem vytvořené `objc_msgSend()` deklarace lze nalézt v [ `ObjCRuntime.Messaging` ](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/).


<a name="ugly" />

## <a name="the-ugly"></a>Ugly

Má tři typy jazyka Objective-C z `objc_msgSend` metody: jeden pro regulární volání, jeden pro volání, které vracejí hodnot s plovoucí desetinnou (pouze x86) a jeden pro volání, které vracejí struktura hodnoty. Ta zahrnuje přípona `_stret` v `ObjCRuntime.Messaging`.

Pokud jsou volání metody, která vrátí určité struktury (pravidla jsou popsána v následující části), je nutné vyvolat metodu s návratovou hodnotu jako první parametr jako hodnotu mimo takto:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Tady získat ugly věcí a kvůli tomu, když musíte použít pravidlo _stret_ se liší na X86 a ARM, pokud chcete, aby vaše vazby pro práci v simulátoru a zařízení, je nutné přidat kód, který vypadá takto:

```csharp
if (Runtime.Arch == Arch.DEVICE){
    PointF ret;

    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);

    return ret;
} else
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
```

### <a name="using-the-objcmsgsendstret-method"></a>Pomocí objc\_msgSend\_stret – metoda

Kdy použít pravidlo [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) jsou stejné jako to pro **ARM**:

-  Některé hodnoty typ, který není výčet nebo základní typy pro výčet (int, byte, short, long, double, float).


Kdy použít pravidlo [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) jsou stejné jako to pro **X86**:

-  Některé hodnoty typ, který není výčet nebo základní typy pro výčet (int, byte, short, long, double, float) a jehož nativní velikost je větší než 8 bajtů.


### <a name="creating-your-own-signatures"></a>Vytváření vlastní podpisy.

Následující [gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) lze použít k vytvoření vlastního podpisy, v případě potřeby.



## <a name="related-links"></a>Související odkazy

- [Ukázka selektorů.](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
