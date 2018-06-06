---
title: Historie verzí cíle Sharpie
description: Tento dokument popisuje historii Sharpie cíl, nástroj, který slouží k automatizaci vytváření vazby C# na kód jazyka Objective-C.
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: a173df769068a3834dfdc314675254af1f3594ed
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781728"
---
# <a name="objective-sharpie-release-history"></a>Historie verzí cíle Sharpie

## <a name="34-october-11-2017"></a>3.4 (11 říjen 2017)

[Stáhnout v3.4.0](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Podpora pro Xcode 9: systému iOS 11, macOS 10.13, tvOS 11 a watchOS 4
* Nyní nutné opravit problémy s SIMD a tgmath
* Telemetrie byla úplně odebrána

## <a name="33-august-3-2016"></a>3.3 (3 srpna 2016)

[Stáhnout v3.3.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Podpora pro Xcode 8 Beta 4, iOS 10, systému macOS 10.12, tvOS 10 a watchOS 3.
* Aktualizovat na nejnovější Clang hlavní sestavení (2016-08-02)
* [Zachovat možnosti odesílání telemetrie](https://twitter.com/Symbiatch/status/760373403878559744) z `sharpie pod bind` k `sharpie bind`.

## <a name="32-june-14-2016"></a>3.2 (14. června 2016)

[Stáhnout v3.2.3](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Podpora pro Xcode 8 Beta 1, iOS 10, systému macOS 10.12, tvOS 10 a watchOS 3.

## <a name="31-may-31-2016"></a>3.1 (31. května 2016)

[Stáhnout v3.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* Podpora pro CocoaPods 1.0
* Zvýšit spolehlivost vazby CocoaPods podle první budovy nativní `.framework` a následným vytvoření vazby,
* Zkopírujte CocoaPods `.framework` a vazbu definice do `Binding` adresáře usnadňují integraci s projekty Xamarin.iOS a Xamarin.Mac vazby

## <a name="30-october-5-2015"></a>3.0 (5. října 2015)

[Stáhnout v3.0.8](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Podpora pro nové jazykové funkce jazyka Objective-C, včetně lightweight obecné typy a možnost použití hodnoty Null, protože byla zavedená v Xcode 7
* Podporu pro nejnovější iOS a Mac sady SDK.
* Projektu Xcode vytváření a analýzu podpory! Nyní předat úplné projektu Xcode Sharpie cíl a provede je nejlepší a pokuste se zjistit správné věc udělat (například `sharpie bind Project.xcodeproj -sdk ios`).
* [CocoaPods](https://cocoapods.org) podporu! Teď můžete nakonfigurovat, sestavení a vytvořit vazbu CocoaPods přímo z Sharpie cíl (například `sharpie pod init ios AFNetworking && sharpie pod bind`).
* Při vytváření vazby architektury, moduly Clang bude-li povolena rozhraní podporuje, což vede k nejvíce správné analýze rozhraní, protože je definována strukturu rozhraní jeho `module.modulemap`.
* Pro projekty Xcode, kteří vytvářejí framework produktu analyzovat produktu místo zprostředkující produktu cíle jako jiný framework cíle v projektu Xcode mohou mít i nadále mnohoznačnosti, které nelze vyřešit automaticky.

## <a name="216-march-17-2015"></a>2.1.6 (17 března 2015)

[Stáhnout v2.1.6](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* Opravené binární operátor výraz vazby: na levé straně výraz byl nesprávně si místo, se pravém (například `1 << 0` byl nesprávně spojen jako `0 << 1`). Díky Adam Kemp pro vašeho povšimnutí to!
* Byl opraven problém s `NSInteger` a `NSUInteger` je vázán jako `int` a `uint` místo `nint` a `nuint` na i386; `-DNS_BUILD_32_LIKE_64` je nyní předaný Clang aby analýza `objc/NSObjCRuntime.h` fungovat podle očekávání na i386.
* Architektura výchozí pro Mac OS X SDK (například `-sdk macosx10.10`) je x86_64 místo i386, takže `-arch` lze vynechat, pokud přepsání výchozího se požaduje.

## <a name="210-march-15-2015"></a>2.1.0 (15. března 2015)

[Stáhnout v2.1.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxc #27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): Zkontrolujte `using ObjCRuntime;` při vytváří `ArgumentSemantic` se používá.
* [bxc #27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): Zkontrolujte `using System.Runtime.InteropServices;` při vytváří `DllImport` se používá.
* [bxc #27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): výchozí `DllImport` k načítání symboly z `__Internal`.
* [bxc #27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): přeskočit deklarovaný dopředného deklarace jazyka Objective-C kontejneru.
* [bxc #27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): vytvoření vazby protokolu typů s jeden kvalifikace jako konkrétní rozhraní (`id<Foo>` jako `Foo` místo `Foundation.NSObject<Foo>`).
* [bxc #28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): vytvoření vazby `UInt32`, `UInt64`, a `Int64` literály jako `Int32` odpojení `u` nebo `uL` přípony při hodnoty můžete bezpečně začlenit do `Int32`.
* [bxc #28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): Opravte výčtu název mapping při původní nativní název začíná `k` předponu.
* `sizeof` Výrazy jazyka C s typem argument není přiřazena k primitivní typ C# budou vyhodnoceny v Clang a vázán jako celé číslo literálu, aby nedošlo ke generování neplatný C#.
* Opravte syntaxi jazyka Objective-C vlastností s typem je blok (Objective-C – kód se zobrazí v komentářích výše vázané deklarace).
* Vazby zkažená typy jako názvy jejich původní typů (`int[]` decays k `int*` během sémantického analýzy v Clang, ale navázat jej jako původní jako zapsána `int[]` místo).

Děkujeme vám, že velmi velká pro Dave Dunkin reporting mnoho opravených v této verzi bodu.

## <a name="200-march-9-2015"></a>2.0.0: 9. března 2015

[Stáhnout v2.0.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

Cíle Sharpie 2.0 je hlavní verzí, který poskytuje lepší ovladač na základě Clang a analyzátor a nový na základě NRefactory vazby modul. Tyto komponenty vylepšené poskytují pro _mnohem_ lepší vazby, zejména kolem typ vazby. Byly provedeny mnoho dalších vylepšení, které jsou interní Sharpie cíl, který poskytne řadu funkcí viditelné pro uživatele v budoucích verzích.

Cíle Sharpie 2.0 je založena na Clang 3.6.1.

### <a name="type-binding-improvements"></a>Typ vazby vylepšení

* Bloky jazyka Objective-C jsou nyní podporovány. To zahrnuje anonymní nebo vložené bloky a bloky s názvem prostřednictvím `typedef`. Anonymní bloky bude vázán jako `System.Action` nebo `System.Func` deleguje, zatímco pojmenovaných bloků bude vázán jako silně pojmenovaných `delegate` typy.

* Je lepší pojmenování heuristiky pro anonymní výčty, která okamžitě předchází `typedef` řešení integrální typu builtin například `long` nebo `int`.

* Ukazatele C je nyní vázána jako C# `unsafe` ukazatele místo `System.IntPtr`. To vede k více přehlednost ve vazbě pro když chcete zapnout ukazatel parametry do `out` nebo `ref` parametry. Není možné vždy odvození, zda má být parametr `out` nebo `ref`, takže ukazatele se uchovávají v vazby umožňující snadnější auditování.

* Výjimkou z výše vazby ukazatel je vyskytne 2 pořadí ukazatel na objekt jazyka Objective-C jako parametr. V těchto případech je konvence převládá a parametr bude vázán jako `out` (například `NSError **error` → `out NSError error`).

### <a name="verify-attribute"></a>Ověření atributu

Často zjistíte, že vazby vyprodukované cíl Sharpie nyní přidáme s `[Verify]` atribut. Tyto atributy znamenat, že byste měli _ověřte_ , cíl Sharpie nebyla správnou věc tak, že porovnáte vazba s původní deklaraci jazyka C nebo Objective-C (která bude k dispozici v komentář nad deklaraci vázané).

Ověření _doporučená_ pro všechny vázané deklarace, ale je pravděpodobně _požadované_ pro opatřen poznámkou deklarace `[Verify]` atribut. Je to proto, že v mnoha situacích, není dostatek metadata v původní nativní zdrojový kód, který odvodit, jak nejlépe vytvořit vazbu. Budete muset referenční dokumentace nebo komentáře kódu uvnitř soubory hlaviček aby nejlepší rozhodnutí vazby.

Najdete v článku [Ověřte atributy](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) dokumentace pro další podrobnosti.

### <a name="other-notable-improvements"></a>Další vylepšení upozorňují na důležité

* `using` příkazy jsou nyní generování na základě typů vázána. Například pokud `NSURL` byla vázaná `using Foundation;` příkaz vygeneruje také.

* `struct` a `union` deklarace bude nyní vázán, pomocí `[FieldOffset]` zdvihy pro sjednocení.

* Hodnoty výčtu s konstantní výraz inicializátory bude nyní správně vázán; úplné výrazu je přeložen na C#.

* Nyní jsou svázané Variadická metody a bloky.

* Rozhraní jsou nyní podporovány prostřednictvím `-framework` možnost. V dokumentaci na [vazby nativní rozhraní](http://developer.xamarin.com/guides/ios/advanced_topics/binding_objective-c/objective_sharpie/#frameworks) další podrobnosti.

* Jazyka Objective-C zdrojový kód bude automaticky nalezeny nyní, který by měl eliminovat potřeba předat `-ObjC` nebo `-xobjective-c` do Clang ručně.

* Clang modul využití (`@import`) je nyní zjištěn automaticky, který by měl eliminovat potřeba předat `-fmodules` do Clang pro knihovny, které používají nové podpoře modulu v Clang ručně.

* Xamarin unifikované API je nyní výchozí vazby cíl; použít `-classic` možnost cílit 32-bit pouze klasické rozhraní API.

### <a name="notable-bug-fixes"></a>Významné opravy chyb

* Opravte `instancetype` vazby při použití v kategorii jazyka Objective-C
* Plně název kategorie jazyka Objective-C
* Předpony jazyka Objective-C protokoly s `I` (například `INSCopying` místo `NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35: 21 prosince 2014

[Stáhnout v1.1.35](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

Menšími opravami chyb.

## <a name="111-december-15-2014"></a>1.1.1: 15 prosince 2014

[Stáhnout verze 1.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 se první hlavní verzi po 1,5 roku interní použití a vývoj v Xamarinu následující počáteční verze preview Sharpie cíl v duben 2013. Tato verze je první, obecně považovat za stabilní a dá se použít pro celou řadu nativní knihovny, poskytuje funkci nový Clang back-end.

