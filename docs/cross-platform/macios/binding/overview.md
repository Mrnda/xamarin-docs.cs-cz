---
title: Přehled vazeb Objective-C
description: Tento dokument obsahuje přehled různých způsobů, jak vytvořit vazby C# pro kód Objective-C, včetně příkazového řádku vazby, projekty vazby a Sharpie cíle. Také popisuje princip vazby.
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: asb3993
ms.author: amburns
ms.openlocfilehash: 97d0c5b9f61d4dafe144d2b2f22df6d465cbbccb
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855270"
---
# <a name="overview-of-objective-c-bindings"></a>Přehled vazeb Objective-C

_Podrobnosti o tom, jak funguje proces vytváření vazby_

Vytvoření vazby knihovnou Objective-C pro použití s využitím kódu Xamarin přebírá ze tří kroků:

1. Zápis jazyka C# "Definice rozhraní API" k popisu jak nativní rozhraní API je zveřejněné v rozhraní .NET a jak je namapován na základní Objective-C. To se provádí pomocí standard jazyka C# vytvoří jako `interface` a různých vazby **atributy** (najdete v tomto [jednoduchý příklad](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. Jakmile jste napsali "definice rozhraní API" v jazyce C#, zkompilovat jej k vytvoření sestavení "vazba". To lze provést na [ **příkazového řádku** ](#commandline) nebo pomocí [ **vazby projektu** ](#bindingproject) v sadě Visual Studio pro Mac nebo Visual Studio.

3. Toto sestavení "vazba" se pak přidá do projektu aplikace Xamarin, aby měli přístup k nativní funkce pomocí rozhraní API služby jste definovali.
  Vazby projektu je zcela nezávislý vaše projekty aplikace.

**Poznámka:** kroku 1 je možné automatizovat za pomoci [ **cíle Sharpie**](#objectivesharpie). Prozkoumá rozhraní API jazyka Objective-C a generuje navrhovaných C# "Definice rozhraní API." Můžete přizpůsobit soubory vytvořené Sharpie cíle a jejich použití v projektu vazby (nebo na příkazovém řádku) k vytvoření vazby sestavení. Cíle Sharpie nevytváří žádné vazby samostatně, je to čistě volitelná součást většího procesu.

Také můžete přečíst další technické podrobnosti o [jak to funguje](#howitworks), který vám pomůže psát vazby.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Vazby příkazového řádku

Můžete použít `btouch-native` pro Xamarin.iOS (nebo `bmac-native` Pokud používáte Xamarin.Mac) k vytvoření vazby přímo. Funguje díky předávání definic rozhraní API jazyka C#, které ručně vytvoříte (nebo pomocí Objective Sharpie) na nástroj příkazového řádku (`btouch-native` pro iOS nebo `bmac-native` pro Mac).


Obecnou syntaxi pro volání těchto nástrojů je:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Výše uvedený příkaz vygeneruje soubor `cocos2d.dll` v aktuálním adresáři a bude obsahovat plně vázané knihovny, kterou můžete použít ve vašem projektu. Toto je nástroj, který používá Visual Studio for Mac pro vytvoření vazby, pokud používáte vazbu projektu (popsané [níže](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>Vazební projekt

Vazby projektu lze vytvořit v sadě Visual Studio pro Mac nebo Visual Studio (Visual Studio podporuje pouze vazby s Iosem) a usnadňuje upravovat a vytvářet definice rozhraní API pro vazbu (oproti použití příkazového řádku).

Použít tento [– Příručka Začínáme](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) se dozvíte, jak vytvořit a používat vazby projektu pro vytvoření vazby.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Cíle Sharpie

Cíle Sharpie je nástroj jiný, samostatný příkazový řádek, který pomáhá při vytváření vazby v počátečních fázích. Nevytvoří vazbu samostatně, spíše automatizuje generování definice rozhraní API pro nativní knihovnu cíl v prvním kroku.

Čtení [cíle Sharpie dokumentace](~/cross-platform/macios/binding/objective-sharpie/index.md) informace o analýze CocoaPods, nativních knihoven a nativních architektur do definice rozhraní API, který může být součástí vazby.

<a name="howitworks" />

## <a name="how-binding-works"></a>Princip vazby

Je možné použít [[zaregistrovat]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) atribut [[exportovat]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) atribut, a [ruční vyvolání selektor Objective-C](~/ios/internals/objective-c-selectors.md) společně k ručnímu vázání nové (dříve nevázaný) typy jazyka Objective-C.

Nejdříve vyhledejte typ, který chcete vytvořit vazbu. Pro diskuse účely (a jednoduchost), jsme vám vytvořit vazbu [NSEnumerator](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) typ (který již byl svázán v [Foundation.NSEnumerator](https://developer.xamarin.com/api/type/Foundation.NSEnumerator/); implementace níže je právě například účely).

Za druhé potřebujeme vytvořit typ jazyka C#. Chceme budete pravděpodobně to umístíte do oboru názvů; protože Objective-C nepodporuje obory názvů, budeme muset použít `[Register]` atribut, chcete-li změnit název typu, který Xamarin.iOS se registrují s modulem runtime Objective-C. Typ jazyka C# také musí dědit z [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

Třetí, projděte si dokumentaci k Objective-C a vytvořit [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) instance pro každý selektor, které chcete použít. Umístíte v rámci těla třídy:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

Čtvrtý váš typ muset zadat konstruktory. Můžete *musí* zřetězit vaše vyvolání konstruktoru pro konstruktor základní třídy. `[Export]` Atributy povolit kód Objective-C pro volat konstruktor s názvem zadaný selektor:

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

Páté poskytují metody pro každou voliče deklaraci v kroku 3. Tyto výrazy se používají `objc_msgSend()` k vyvolání selektor na nativní objekt. Všimněte si použití [Runtime.GetNSObject()](https://developer.xamarin.com/api/member/ObjCRuntime.Runtime.GetNSObject/(System.IntPtr)) převést `IntPtr` do správně zadaný `NSObject` (sub) typu. Pokud chcete, aby metoda bude volat z kódu jazyka Objective-C, člen *musí* být **virtuální**.

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

Vložení všechno dohromady:

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

## <a name="related-links"></a>Související odkazy

- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)