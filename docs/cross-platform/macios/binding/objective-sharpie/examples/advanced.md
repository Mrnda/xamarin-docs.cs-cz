---
title: Rozšířené (ruční) Real-World – ukázka
description: Tento dokument popisuje, jak použít výstup xcodebuild jako vstup pro Sharpie cíle, které poskytuje přehled o tom, co dělá cíle Sharpie pod pokličkou.
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: c4f7f1e9702fb2ee0f5525343a52e3aacd85d68c
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855257"
---
# <a name="advanced-manual-real-world-example"></a>Rozšířené (ruční) Real-World – ukázka

**V tomto příkladu [POP knihovny ze sítě Facebook](https://github.com/facebook/pop).**

Tato část se věnuje pokročilejší přístup do vazby, kde budeme používat Apple `xcodebuild` nástroj nejdřív sestavit projekt POP a ručně odvodit vstup pro Sharpie cíle. To v podstatě popisuje, co dělá cíle Sharpie pod pokličkou v předchozí části.

```
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Protože knihovny POP má projekt Xcode (`pop.xcodeproj`), můžeme jednoduše použít `xcodebuild` sestavit POP. Tento proces pak může generovat soubory hlaviček, které cíle Sharpie možná bude nutné analyzovat. To je důvod, proč sestavení před vazba je důležité. Při sestavování prostřednictvím `xcodebuild` zajistit předáte stejný identifikátor sady SDK a architektury, které chcete předat Sharpie cíle (a nezapomeňte, že cíl Sharpie 3.0 můžete obvykle to udělal za vás!):

```
$ xcodebuild -sdk iphoneos9.0 -arch arm64

Build settings from command line:
    ARCHS = arm64
    SDKROOT = iphoneos8.1
 
=== BUILD TARGET pop OF PROJECT pop WITH THE DEFAULT CONFIGURATION (Release) ===
 
...
 
CpHeader pop/POPAnimationTracer.h build/Headers/POP/POPAnimationTracer.h
    cd /Users/aaron/src/sharpie/pop
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/Users/aaron/bin::/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/usr/local/git/bin:/Users/aaron/.rvm/bin"
    builtin-copy -exclude .DS_Store -exclude CVS -exclude .svn -exclude .git -exclude .hg -strip-debug-symbols -strip-tool /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/strip -resolve-src-symlinks /Users/aaron/src/sharpie/pop/pop/POPAnimationTracer.h /Users/aaron/src/sharpie/pop/build/Headers/POP
 
...
 
** BUILD SUCCEEDED **
```

Jako součást bude možné spoustu výstupní informace sestavení v konzole `xcodebuild`. Jak se zobrazuje výše, vidíme, že byl spuštěn "CpHeader" cíl ve které byly hlavičkové soubory zkopírovány do výstupního adresáře sestavení. To je často případ a usnadňuje vazby: jako součást sestavení nativní knihovny, hlavičkové soubory často zkopírován do "veřejně" použitelné umístění, které můžete provádět analýzy snazší pro vazbu. V tomto případě jsme si vědomi, že jsou soubory hlaviček na serveru POP v `build/Headers` adresáře.

Nyní jsme připraveni vytvořit vazbu POP. Víme, že chceme sestavení pro sadu SDK `iphoneos8.1` s `arm64` architektury a jestli jsou soubory hlaviček starosti v `build/Headers` pod POP git checkout. Když se podíváme `build/Headers` adresáře, uvidíme počet hlavičkové soubory:

```
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

Když se podíváte na `POP.h`, můžeme vidět je knihovny hlavní nejvyšší úrovně záhlaví souboru, který `#import`s další soubory. Z tohoto důvodu stačí předat `POP.h` do cíle Sharpie a clang udělá zbytek na pozadí:

```
$ sharpie bind -output Binding -sdk iphoneos8.1 \
    -scope build/Headers build/Headers/POP/POP.h \
    -c -Ibuild/Headers -arch arm64

Parsing Native Code...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Binding Analysis:
  Automated binding is complete, but there are a few APIs which have
  been flagged with [Verify] attributes. While the entire binding
  should be audited for best API design practices, look more closely
  at APIs with the following Verify attribute hints:

  ConstantsInterfaceAssociation (1 instance):
    There's no fool-proof way to determine with which Objective-C
    interface an extern variable declaration may be associated.
    Instances of these are bound as [Field] properties in a partial
    interface into a near-by concrete interface to produce a more
    intuitive API, possibly eliminating the 'Constants' interface
    altogether.

  StronglyTypedNSArray (4 instances):
    A native NSArray* was bound as NSObject[]. It might be possible
    to more strongly type the array in the binding based on
    expectations set through API documentation (e.g. comments in the
    header file) or by examining the array contents through testing.
    For example, an NSArray* containing only NSNumber* instances can
    be bound as NSNumber[] instead of NSObject[].

  MethodToProperty (2 instances):
    An Objective-C method was bound as a C# property due to
    convention such as taking no parameters and returning a value (
    non-void return). Often methods like these should be bound as
    properties to surface a nicer API, but sometimes false-positives
    can occur and the binding should actually be a method.

  Once you have verified a Verify attribute, you should remove it
  from the binding source code. The presence of Verify attributes
  intentionally cause build failures.

  For more information about the Verify attribute hints above,
  consult the Objective Sharpie documentation by running 'sharpie
  docs' or visiting the following URL:

    http://xmn.io/sharpie-docs

Submitting usage data to Xamarin...
  Submitted - thank you for helping to improve Objective Sharpie!

Done.
```

Všimnete si, že jsme předaný `-scope build/Headers` argument Sharpie cíle. Protože knihovny jazyka C a jazyka Objective-C musí `#import` nebo `#include` další hlavičkové soubory, které jsou podrobnosti implementace knihovny a chcete vytvořit vazbu, nikoli rozhraní API `-scope` argument určuje cíl Sharpie ignorovat jakékoli rozhraní API, která není definovaná v soubor někam `-scope` adresáře.

Vás bude `-scope` argument je často volitelná pro čistě implementované knihovny, ale není na explicitně poskytuje škodu.

Kromě toho jsme zadali `-c -Ibuild/headers`. Za prvé `-c` argument určuje cíl Sharpie zastavení interpretaci argumentů příkazového řádku a žádné následné argumenty _přímo do kompilátoru clang_. Proto `-Ibuild/Headers` je argumentem kompilátoru clang, který dává pokyn k vyhledání clang zahrnuje v části `build/Headers`, což je, kde záhlaví POP live. Bez tohoto argumentu by clang vědět, kam umístit soubory, které `POP.h` je `#import`ing. _Téměř všechny "problémy" s použitím cíle Sharpie vaří na tím, co se má předat clang_.

### <a name="completing-the-binding"></a>Dokončení vazby

Nyní cíle Sharpie vygenerovala `Binding/ApiDefinitions.cs` a `Binding/StructsAndEnums.cs` soubory.

Jedná se o základní první fáze cíle Sharpie na vazby a v několika případech může být vše, co potřebujete. Jak je uvedeno výše, ale, se vývojáři obvykle muset ručně upravit generované soubory po dokončení Sharpie cíle pro [opravte všechny problémy](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) , která nelze automaticky zpracovat nástrojem.

Po dokončení aktualizace se tyto dva soubory lze nyní přidat do projektu vazby v sadě Visual Studio pro Mac nebo být předána přímo `btouch` nebo `bmac` nástroje k vytvoření konečného vazby.

Důkladné popis proces vytváření vazby, najdete v tématu naše [kompletní názorný postup](~/ios/platform/binding-objective-c/walkthrough.md).

## <a name="related-links"></a>Související odkazy

- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)