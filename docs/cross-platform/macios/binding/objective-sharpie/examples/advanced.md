---
title: "Rozšířené (ruční) příklad reálného"
ms.topic: article
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 67bd1caf26c441e2a89def41ce3189b0dd67d7b1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="advanced-manual-real-world-example"></a>Rozšířené (ruční) příklad reálného


**Tento příklad používá [POP knihovny ze sítě Facebook](https://github.com/facebook/pop).**


Tato část popisuje pokročilejší přístup k vazby, kde budeme používat společnosti Apple `xcodebuild` nástroje k nejprve sestavení projektu POP a pak ručně odvodit vstup pro Sharpie cíl. To v podstatě popisuje, co je to cíl Sharpie pod pokličkou v předchozí části.

```csharp
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Vzhledem k tomu, že knihovna, POP, má projektu Xcode (`pop.xcodeproj`), můžeme jednoduše použít `xcodebuild` k sestavení POP. Tento proces může generovat zase soubory hlaviček, které Sharpie cíl může být nutné analyzovat. Z tohoto důvodu vytváření před vazba je důležité. Při sestavování prostřednictvím `xcodebuild` zkontrolujte předáte stejný identifikátor SDK a architektury který máte v úmyslu předat Sharpie cíl (a pamatujte si, že cíl Sharpie 3.0 můžete obvykle to pro vás!):

```csharp
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

Budou existovat velké množství informací výstup v konzole jako součást `xcodebuild`. Jak je zobrazen výše, uvidíte, že cíl "CpHeader" byla spuštěna ve kterém byly soubory hlaviček zkopírovány do výstupního adresáře sestavení. To je často případ a usnadňuje vazby: jako součást sestavení nativní knihovny, soubory hlaviček často zkopírují do "veřejně" použití umístění, které můžete provést analýzu snazší pro vazbu. V takovém případě víme, že jsou soubory hlaviček na POP v `build/Headers` adresáře.

Nyní připraveni vytvořit vazbu protokolu POP. Víme, že má být sestavení sady SDK `iphoneos8.1` s `arm64` architekturu a že se v záhlaví soubory záleží nám `build/Headers` pod POP git checkout. Pokud podíváme `build/Headers` adresář, uvidíme počet soubory hlaviček:

```csharp
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

Pokud se podíváme na `POP.h`, uvidíte je knihovny hlavní nejvyšší úrovně záhlaví souboru, který `#import`s další soubory. Z toho důvodu musíme pouze předat `POP.h` k Sharpie cíl a provede clang rest na pozadí:

```csharp
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

Si všimnete, že jsme předána `-scope build/Headers` argument Sharpie cíl. Protože C a jazyka Objective-C knihovny musí `#import` nebo `#include` další hlavičkové soubory, které jsou podrobnosti implementace knihovny a chcete vytvořit vazbu, není rozhraní API `-scope` argument určuje cíl Sharpie ignorovat jakéhokoli rozhraní API, které nejsou definované v soubor někde v rámci `-scope` adresáře.

Zjistíte, `-scope` argument je často volitelná implementovaná knihovnám této aplikace, ale neexistuje žádné škodu v explicitně poskytování ho.

Kromě toho jsme zadali `-c -Ibuild/headers`. Za prvé `-c` argument určuje cíl Sharpie zastavení interpretace argumenty příkazového řádku a předávat všechny následné argumenty _přímo do kompilátoru clang_. Proto `-Ibuild/Headers` je argumentem kompilátoru clang obsahující pokyn clang k vyhledání zahrnuje v části `build/Headers`, což je bydlišti hlavičky protokolu POP. Bez tento argument clang vědět, kde najít soubory, `POP.h` je `#import`ing. _Téměř všechny "problémy" s použitím Sharpie cíl povaří, aby se nad tím, co mají být předána do clang_.

###<a name="completing-the-binding"></a>Dokončení vazby

Cíle Sharpie má nyní generování `Binding/ApiDefinitions.cs` a `Binding/StructsAndEnums.cs` soubory.

Jedná se o základní první fáze Sharpie cíl v vazby a v několika případech může být všechny, které potřebujete. Jak jsme uvedli ale výše, vývojáři obvykle muset ručně upravit, generované soubory po dokončení Sharpie cíl pro [opravte všechny problémy](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) , nelze zpracovat automaticky nástrojem.

Po dokončení aktualizace se tyto dva soubory lze nyní přidat do projektu vazby v sadě Visual Studio pro Mac nebo předávané přímo na `btouch` nebo `bmac` nástroje k vytvoření konečné vazby.

Důkladné popis proces vytváření vazby, najdete v tématu naše [kompletní a podrobný postup pokyny](~/ios/platform/binding-objective-c/walkthrough.md).

