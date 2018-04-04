---
title: Obecné podtřídy NSObject
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 89df751d74b9b54ae8138d2e1b24c61d82c3cac8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="generic-subclasses-of-nsobject"></a>Obecné podtřídy NSObject

## <a name="using-generics-with-nsobjects"></a>Pomocí NSObjects obecné typy

Počínaje Xamarin.iOS 7.2.1 můžete použít obecné typy v podtřídách z `NSObject` (například [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/)).

Nyní můžete vytvořit obecné třídy, jako je tato:

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

Protože objekty tohoto podtřídami `NSObject` jsou registrované s modulem runtime jazyka Objective-C existují určitá omezení, co je možné pomocí obecné měly podtřídy `NSObject` typy.
    
## <a name="considerations-for-generic-subclasses-of-nsobject"></a>Důležité informace pro obecné podtřídy NSObject

Tento dokument podrobně popisuje omezení v omezenou podporu pro obecné měly podtřídy `NSObjects` zavedené Xamarin.iOS 7.2.1.
    
### <a name="generic-type-arguments-in-member-signatures"></a>Argumenty obecného typu v signaturách členu

Všechny argumenty obecného typu v člen podpis vystavený jazyka Objective-C musí mít `NSObject` omezení.

**Dobrý**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Důvod**: Parametr obecného typu `NSObject`, takže podpis selektor pro `myMethod:` mohou být zpřístupněny bezpečně jazyka Objective-C (bude vždy `NSObject` nebo podtřídou třídy ji).

**Chybný**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Důvod**: není možné vytvořit podpis jazyka Objective-C pro exportovaný členy, kteří kód jazyka Objective-C můžete volat, protože podpis by lišit v závislosti na přesné typ obecného typu `T`.

**Dobrý**:

```csharp
class Generic<T> : NSObject
{
    T storage;

    [Export ("myMethod:")]
    public void MyMethod (NSObject value)
    {
    }
}
```

**Důvod**: je možné mít neomezeného argumenty obecného typu, dokud se neprovádí žádné součástí podpis exportovaný člen.

**Dobrý**:

```csharp
class Generic<T, U> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
        Console.WriteLine (typeof (U));
    }
}
```

**Důvod**: `T` exportovat parametr v Objective-C `MyMethod` je omezen na `NSObject`, typ neomezeným `U` není součástí podpis.
    
### <a name="instantiations-of-generic-types-from-objective-c"></a>Instancí možnosti z jazyka Objective-C – obecné typy

Vytváření instancí obecných typů z jazyka Objective-C není povoleno. K tomu obvykle dochází, když spravovaný typ se používá v xib.

Vezměte v úvahu tato definice třídy, která zpřístupňuje konstruktor, který přebírá `IntPtr` (Xamarin.iOS způsobu konstrukce objekt C# z nativní instance jazyka Objective-C):
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

Při výše konstrukce je v pořádku, za běhu, to vyvolá výjimku v případě jazyka Objective-C se pokusí vytvořit její instanci.

Jedná se stane, protože jazyka Objective-C nemá žádná koncepce obecné typy a nemůže určovat přesný obecného typu vytvořit.

Tento problém můžete fungovala kolem vytvořením specializované podtřídou třídy obecného typu.   Příklad:
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

Teď nedochází k nejednoznačnosti už, třída `GenericUIView` mohou být používány xibs.

## <a name="no-support-for-generic-methods"></a>Žádná podpora pro obecné metody

### <a name="generic-methods-are-not-allowed"></a>Obecné metody nejsou povoleny.

Nebude Zkompilujte následující kód:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**Důvod**: Tento není povolená, protože Xamarin.iOS nebude vědět, který typ používat pro argument typu `T` když se metoda volá z jazyka Objective-C.

Alternativou je vytvoření specializované metody a export, místo toho:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyUIViewMethod (UIView argument)
    {
        MyMethod<UIView> (argument);
    }
    public void MyMethod<T> (T argument)
    {
    }
}
```

### <a name="no-exported-static-members-allowed"></a>Žádné povolené exportovaný statické členy

Nelze vystavit statické členy do jazyka Objective-C, pokud je umístěn uvnitř obecné podtřídou třídy `NSObject`.

Příklad o nepodporovaný scénář:

```csharp
class Generic<T> : NSObject where T : NSObject
{
    [Export ("myMethod:")]
    public static void MyMethod ()
    {
    }

    [Export ("myProperty")]
    public static T MyProperty { get; set; }
}
```

**Důvod:** stejně jako obecné metody, je nutné modul runtime Xamarin.iOS moct vědět, co typ použít argument obecného typu T.

Pro instanci členů instance samotného slouží (vzhledem k tomu, že se nikdy nemůže mít instanci obecného<T>, bude vždy obecného<SomeSpecificClass>), ale pro statické členy tato informace není přítomna.

Všimněte si, že to platí i v případě, že příslušného člena nepoužívá argument typu T žádným způsobem.

V takovém případě je vytvořit specializované podtřídy alternativním:

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIVIewProperty {
        get { return MyProperty; }
        set { MyProperty = value; }
    }
}

class Generic<T> : NSObject where T : NSObject
{
    public static void MyMethod () {}
    public static T MyProperty { get; set; }
}
```

### <a name="requires-new-static-registrar"></a>Vyžaduje nové statické registrátora

Obecné typy podpora vyžaduje nové [registrace systému](~/ios/internals/registrar.md).

Pokud se pokusíte použít starý systém starší verze registrace se zobrazí upozornění, pokud se setká s obecné typy (kromě negeneruje správný kód, což vede k nedefinované chování).
    
## <a name="performance"></a>Výkon

Statické registrátora nelze vyřešit člena exportovaný v obecného typu v čase vytvoření buildu jako obvykle, musí být prohledávat za běhu. To znamená, že vyvolání metody, z jazyka Objective-C je mírně nižší než vyvolání členy z neobecné třídy.

