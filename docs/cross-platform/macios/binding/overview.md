---
title: Přehled
description: Podrobnosti o tom, jak funguje proces vytváření vazby
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: d1a90934cf7a9a832172f32ed95cf3e254e04385
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="overview"></a>Přehled

_Podrobnosti o tom, jak funguje proces vytváření vazby_

Vazba knihovna jazyka Objective-C pro použití s Xamarinem přijímá tři kroky:

1. Zápis C# "Definice rozhraní API" k popisu jak nativní rozhraní API vystavený v rozhraní .NET a jak se mapuje na základní Objective-c To se provádí pomocí standardní jazyka C# vytvoří jako `interface` a různé vazby **atributy** (Toto [jednoduchý příklad](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. "Definice rozhraní API" mít jednou napsané v C#, zkompilujete ji k vytvoření sestavení "vazba". To lze provést na [ **příkazového řádku** ](#commandline) nebo pomocí [ **vazby projektu** ](#bindingproject) v sadě Visual Studio pro Mac nebo Visual Studio.

3. Toto sestavení. "vazba" se pak přidá do projektu aplikace Xamarin, dostanete nativních funkcí pomocí rozhraní API, které jste definovali.
  Projekt vazba je zcela oddělené od vašich projektů aplikace.

**Poznámka:** kroku 1 je možné automatizovat s pomocí [ **cíl Sharpie**](#objectivesharpie). Prověří rozhraní API jazyka Objective-C a generuje navrhované C# "Definice rozhraní API." Můžete upravit soubory vytvořené Sharpie cíl a použít je v projektu vazba (nebo na příkazovém řádku) k vytvoření vašeho vazby sestavení. Cíle Sharpie nevytvoří vazby samostatně, je jenom tato volitelná součást procesu větší.

Také můžete přečíst další technické podrobnosti o [jak to funguje](#howitworks), který vám pomůže zapsat vaše vazby.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Vazby příkazového řádku

Můžete použít `btouch-native` pro Xamarin.iOS (nebo `bmac-native` Pokud používáte Xamarin.Mac) k vytvoření vazby přímo. Funguje díky předávání definice rozhraní API jazyka C#, které jste vytvořili ručně (nebo pomocí Sharpie cíl) k nástroji příkazového řádku (`btouch-native` pro iOS nebo `bmac-native` pro systém Mac).


Obecná syntaxe pro vyvolání těchto nástrojů je:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Výše uvedený příkaz vygeneruje soubor `cocos2d.dll` v aktuálním adresáři, a bude obsahovat plně vázané knihovny, která můžete použít ve vašem projektu. Toto je nástroj, který Visual Studio pro Mac používá k vytvoření vazby, pokud používáte vazby projektu (popsané [pod](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>Vytvoření vazby projektu

Vazba projektu lze vytvořit v sadě Visual Studio pro Mac nebo Visual Studio (Visual Studio podporuje pouze vazby iOS) a usnadňuje upravit a vytvořit definice rozhraní API pro vazbu (oproti pomocí příkazového řádku).

Potom zadejte [Příručka Začínáme](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) informace o tom, jak vytvořit a používat projektu vazby k vytvoření vazby.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Cíle Sharpie

Cíle Sharpie je příkazového řádku jiný, samostatný nástroj, který pomáhá s počáteční fáze vytváření vazby. Nedojde k vytvoření vazby samostatně, místo ho automatizuje generování definice rozhraní API pro nativní knihovny cíl v prvním kroku.

Pro čtení [Sharpie cíl dokumentace](~/cross-platform/macios/binding/objective-sharpie/index.md) se dozvíte, jak analyzovat nativní knihovny, nativní rozhraní a CocoaPods do defintions rozhraní API, který může být součástí vazby.

<a name="howitworks" />

## <a name="how-binding-works"></a>Jak funguje vazby

Je možné použít [[zaregistrovat]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) atribut [[Export]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) atribut a [ruční volání selektor jazyka Objective-C](~/ios/internals/objective-c-selectors.md) společně se ručně vytvořit vazbu nové (dříve typy jazyka Objective-C nevázaný).

Nejdříve vyhledejte typ, který chcete vytvořit vazbu. Pro diskusní účely (a jednoduchost), jsme budete vazbu [NSEnumerator](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) typu (která již byla svázána v [Foundation.NSEnumerator](https://developer.xamarin.com/api/type/Foundation.NSEnumerator/); níže implementace je právě například účely).

Druhý je potřeba vytvořit typ C#. Jsme budete pravděpodobně chtít to umístěte do oboru názvů; vzhledem k tomu, že jazyka Objective-C nepodporuje obory názvů, budeme muset použít `[Register]` atribut Chcete-li změnit název typu, který Xamarin.iOS se registrují s modulem runtime jazyka Objective-C. Typ C# musí také dědit z [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

Třetí, přečtěte si dokumentaci k jazyka Objective-C a vytvořit [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) instance pro každý selektor, které chcete použít. Umístíte tyto v těle třídy:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

Typ vašeho čtvrté, bude nutné zadat konstruktory. Můžete *musí* řetězu vaší konstruktor volání konstruktoru základní třídy. `[Export]` Atributy povolení kódu jazyka Objective-C volat konstruktory s názvem zadaný selektor:

```csharp
[Export("init")]
public NSEnumerator()
    : base(NSObjectFlag.Empty)
{
    Handle = Messaging.IntPtr_objc_msgSend(this.Handle, selInit.Handle);
}
```

```csharp
// This constructor must be present so that Xamarin.iOS
// can create instances of your type from Objective-C code.
public NSEnumerator(IntPtr handle)
    : base(handle)
{
}
```

Páté poskytují metody pro každou z na selektory deklarované v kroku 3. Tyto použije `objc_msgSend()` k vyvolání modulu pro výběr v nativní objektu. Všimněte si použití [Runtime.GetNSObject()](https://developer.xamarin.com/api/member/ObjCRuntime.Runtime.GetNSObject/(System.IntPtr)) převést `IntPtr` do správně typové `NSObject` (dílčí) typu. Pokud chcete, aby metoda pro být možné volat z kódu jazyka Objective-C, člen *musí* být **virtuální**.

```csharp
[Export("nextObject")]
public virtual NSObject NextObject()
{
    return Runtime.GetNSObject(
        Messaging.IntPtr_objc_msgSend(this.Handle, selNextObject.Handle));
}
```

```csharp
// Note that for properties, [Export] goes on the get/set method:
public virtual NSArray AllObjects {
    [Export("allObjects")]
    get {
        return (NSArray) Runtime.GetNSObject(
            Messaging.IntPtr_objc_msgSend(this.Handle, selAllObjects.Handle));
    }
}
```

Vložení všechny společně:

```csharp
using System;
using Foundation;
using ObjCRuntime;

namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        static Selector selInit       = new Selector("init");
        static Selector selAllObjects = new Selector("allObjects");
        static Selector selNextObject = new Selector("nextObject");

        [Export("init")]
        public NSEnumerator()
            : base(NSObjectFlag.Empty)
        {
            Handle = Messaging.IntPtr_objc_msgSend(this.Handle,
                selInit.Handle);
        }

        public NSEnumerator(IntPtr handle)
            : base(handle)
        {
        }

        [Export("nextObject")]
        public virtual NSObject NextObject()
        {
            return Runtime.GetNSObject(
                Messaging.IntPtr_objc_msgSend(this.Handle,
                    selNextObject.Handle));
        }

        // Note that for properties, [Export] goes on the get/set method:
        public virtual NSArray AllObjects {
            [Export("allObjects")]
            get {
                return (NSArray) Runtime.GetNSObject(
                    Messaging.IntPtr_objc_msgSend(this.Handle,
                        selAllObjects.Handle));
            }
        }
    }
}
```

