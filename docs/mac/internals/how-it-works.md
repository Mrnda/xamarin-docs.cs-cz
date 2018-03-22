---
title: Jak funguje Xamarin.Mac
description: "Tento dokument popisuje vnitřní strukturu a funkci Xamarin.Mac. Konkrétně se hledá v konstruktory, správa paměti před doba kompilace a registrátora."
ms.topic: article
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/25/2017
ms.openlocfilehash: a1dbff32b113bd1c3a6b2058a34c73977c59c9e5
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="how-xamarinmac-works"></a>Jak funguje Xamarin.Mac

Ve většině případů nebude nutné vývojář starat o interní "magic" z Xamarin.Mac, ale s hrubý povědomí o tom, jak funguje věcí pod pokličkou usnadní interpretace stávající dokumentaci s přehledu C# i ladění problémů při kterým dochází.

V Xamarin.Mac, aplikace přemosťuje dvě světů: je modul runtime jazyka Objective-C na základě obsahující instance nativních tříd (`NSString`, `NSApplication`atd) a je obsahující instance spravované třídy runtime jazyka C# (`System.String`, `HttpClient`atd). Mezi tyto dvě světů Xamarin.Mac vytvoří dvě způsob mostu, takže aplikace můžete volat metody (selektory) v Objective-C (například `NSApplication.Init`) a jazyka Objective-C můžete volat aplikace C# metody zpět (například metody delegáta aplikace). Obecně platí, jazyka Objective-C volání zpracovává transparentně prostřednictvím **P/vyvolá** a určitý kód runtime poskytuje Xamarin.

<a name="exposing-classes" />

## <a name="exposing-c-classes--methods-to-objective-c"></a>Vystavení třídy jazyka C# nebo metody jazyka Objective-C

Ale pro Objective-C zpětného volání do aplikace C# objekty, budou potřeba zpřístupnit způsobem, který můžete porozumět jazyka Objective-C. To se provádí prostřednictvím `Register` a `Export` atributy. Proveďte v následujícím příkladu:

```csharp
[Register ("MyClass")]
public class MyClass : NSObject
{
   [Export ("init")]
   public MyClass ()
   {
   }

   [Export ("run")]
   public void Run ()
   {
   }
}
```

V tomto příkladu se modul runtime jazyka Objective-C teď vědět o třídy s názvem `MyClass` s názvem selektory `init` a `run`.

Ve většině případů se jedná se podrobností implementace, které vývojář můžete ignorovat, protože většina zpětná volání aplikace obdrží bude buď prostřednictvím přepsané metody na `base` třídy (například `AppDelegate`, `Delegates`, `DataSources`) nebo na  **Akce** předaný do rozhraní API. Ve všech těchto případech `Export` atributy nejsou potřebné v kódu jazyka C#.

## <a name="constructor-runthrough"></a>Runthrough – konstruktor

V mnoha případech bude nutné vystavit aplikace C# třídy vytváření rozhraní API modulu runtime jazyka Objective-C, takže se dá vytvořit instance z míst, jako vývojář při volání v Storyboard nebo XIB soubory. Zde je pět nejčastějších konstruktory používat v aplikacích Xamarin.Mac:

```csharp
// Called when created from unmanaged code
public CustomView (IntPtr handle) : base (handle)
{
   Initialize ();
}

// Called when created directly from a XIB file
[Export ("initWithCoder:")]
public CustomView (NSCoder coder) : base (coder)
{
   Initialize ();
}

// Called from C# to instance NSView with a Frame (initWithFrame)
public CustomView (CGRect frame) : base (frame)
{
}

// Called from C# to instance NSView without setting the frame (init)
public CustomView () : base ()
{
}

// This is a special case constructor that you call on a derived class when the derived called has an [Export] constructor.
// For example, if you call init on NSString then you don’t want to call init on NSObject.
public CustomView () : base (NSObjectFlag.Empty)
{
}
```

Obecně platí, nechat Vývojář `IntPtr` a `NSCoder` konstruktory, které jsou generovány při vytváření některé typy, jako je například vlastní `NSViews` samostatně. Pokud Xamarin.Mac potřebuje volat jeden z těchto konstruktorů v odpovědi na požadavek modulu runtime jazyka Objective-C a odebrali jste ho, dojde k chybě aplikace do nativního kódu a může být obtížné přesně problém pochopit.

## <a name="memory-management-and-cycles"></a>Správa paměti a cyklů

Správa paměti v Xamarin.Mac je velmi podobné Xamarin.iOS mnoha způsoby. Je také komplexní téma, jeden nad rámec tohoto dokumentu. Přečtěte si prosím [paměti a osvědčené postupy z hlediska výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md).

## <a name="ahead-of-time-compilation"></a>Můžete některé z doba kompilace

Obvykle při sestavení aplikací .NET nejde kompilovat dolů zkompilovaný kód, místo toho zkompilovat do zprostředkující vrstvy názvem IL kód, který získá _pouze za běhu_ (JIT) zkompilovat kód počítač při spuštění aplikace.

Doba, jakou trvá mono runtime JIT kompilace tento kód počítače může zpomalit spuštění aplikace Xamarin.Mac až 20 %, jako trvá, než pro potřeby zkompilovaný kód má být vygenerován.

JIT – kompilace kódu IL z důvodu omezení vynucená společností Apple pro iOS, není k dispozici pro Xamarin.iOS. V důsledku toho jsou všechny aplikace Xamarin.iOS úplné _Ahead čas_ (AOT) zkompilovat kódu počítače během cyklu sestavení.

Novinkou Xamarin.Mac je možnost AOT kód IL během cyklu sestavení aplikace, stejně jako můžete Xamarin.iOS. Používá Xamarin.Mac _hybridní_ AOT přístup, kompilovaný většinu potřebných zkompilovaný kód, ale umožňuje modulu runtime zkompilovat potřebné trampolines a flexibilitu na podporu Reflection.Emit i nadále (a jiné použití případů, které aktuálně fungovat na Xamarin.Mac).

Existují dvě hlavní oblasti, kde AOT může pomoct Xamarin.Mac aplikace:

- **Lepší "nativní" havárií protokoly** – Pokud Xamarin.Mac aplikace, dojde k chybě v nativním kódu, které je běžné situaci, kdy neplatné volání do rozhraní API kakao (například odeslání `null` do metody, která není přijmout) nativní havárií protokoly s JIT rámec se obtížně analyzovat. Vzhledem k tomu, že rámce JIT nemají informace o ladění, budou existovat více řádků s šestnáctkových odsazení a žádné potvrzením, co se děje. AOT generuje "skutečných" s názvem rámce a trasování je mnohem snazší ke čtení. To také znamená Xamarin.Mac aplikace budou používat lépe nativních nástrojů, jako **lldb** a **Instruments**.
- **Lépe spuštění doba** – pro velké aplikace Xamarin.Mac, s více spuštění podruhé, JIT kompilace všechny kódu může trvat dlouhou dobu. AOT funguje tohoto odběru.

### <a name="enabling-aot-compilation"></a>Povolení AOT kompilace

AOT je povolena v Xamarin.Mac dvojitým kliknutím **název projektu** v **Průzkumníku řešení**, navigační k **sestavení Mac** a přidání `--aot:[options]` k  **Další zhr argumenty:** pole (kde `[options]` je jedna nebo více možností řízení AOT typu, najdete v článku níže). Příklad:

![Přidání AOT do další mmp argumenty](how-it-works-images/aot01.png "přidání AOT další zhr argumenty")

> [!IMPORTANT]
> Povolení AOT kompilace výrazně zvyšuje čase vytvoření buildu, někdy až několik minut, ale může zlepšit časy spuštění aplikace v průměru o 20 %. V důsledku toho kompilace AOT by měla být povolena pouze v **verze** sestavení Xamarin.Mac aplikace.

### <a name="aot-compilation-options"></a>Možnosti AOT kompilace

Existuje několik různých možností, které lze provést při povolování kompilace AOT na Xamarin.Mac aplikace:

- `none` -Žádné AOT kompilace. Toto je výchozí nastavení.
- `all` -AOT zkompiluje všechna sestavení v MonoBundle.
- `core` -AOT zkompiluje `Xamarin.Mac`, `System` a `mscorlib` sestavení.
- `sdk` -AOT zkompiluje `Xamarin.Mac` a sestavení základní třídy knihovny (BCL).
- `|hybrid` -Přidání to na jednu z výše uvedených možností umožňuje hybridní AOT, která umožňuje IL odstraňování, ale bude vést nebude možné je kompilovat časy.
- `+` -Obsahuje jeden pro k AOT kompilace.
- `-` – Odebere jeden soubor AOT kompilace.

Například `--aot:all,-MyAssembly.dll` by povolit kompilace AOT na všechna sestavení v MonoBundle _s výjimkou_ `MyAssembly.dll` a `--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll` by povolit hybridní, zahrnují kód AOT `MyOtherAssembly.dll` s výjimkou `mscorlib.dll`.

## <a name="partial-static-registrar"></a>Částečné statické registrátora

Při vývoji aplikace Xamarin.Mac, se může stát důležité dodržení vývoj termínů minimalizovat čas mezi dokončení změnu a jeho otestováním. Strategie například základy modularization z kódu a testování částí může pomoct snížit dobu kompilace, jak se snížit počet pokusů, že aplikace bude vyžadovat nákladné úplné opětovné sestavení.

Kromě toho a nové Xamarin.Mac, _částečné statické registrátora_ (jak pioneered podle Xamarin.iOS) může výrazně snížit dobu spuštění aplikace Xamarin.Mac ve **ladění** konfigurace. Pochopení, jak můžete pomocí částečné statické registrátora zmenšené se téměř zlepšování 5 x spuštění ladění bude trvat bit pozadí na novinky registrátora, co je rozdíl mezi statických a dynamických a jaké jsou tato verze "částečné static".

### <a name="about-the-registrar"></a>O registrátora

Pod pokličkou žádné Xamarin.Mac aplikace je rozhraní kakao od Applu a modul runtime jazyka Objective-C. Vytváření most mezi tato "nativní world" a "spravované world" jazyka C# zodpovídá primární Xamarin.Mac. Součástí této úlohy se zpracovává souborem registrátora, který je proveden uvnitř `NSApplication.Init ()` metoda. Toto je jedním z důvodů, která vyžaduje jakékoli použití rozhraní API kakao v Xamarin.Mac `NSApplication.Init` k první volání.

Úloha vašeho registrátora je informovat modul runtime jazyka Objective-C existenci aplikace C# třídy, které jsou odvozeny od třídy, jako `NSApplicationDelegate`, `NSView`, `NSWindow`, a `NSObject`. To vyžaduje kontrolu všech typů v aplikaci k určení toho, co je registrace a přizpůsobitelné prvky na každý typ sestavy.

Tato kontrola se dá buď **dynamicky**, při spuštění aplikace s reflexí, nebo **staticky**, jako čas krok sestavení. Při výběru typu registrace vývojář potřeba vzít na vědomí následující:

- Statické registrace může výrazně snížit dobu spuštění, ale může sestavení dobu výrazně zpomalit (obvykle více než double ladění sestavení čas). To bude výchozí hodnotou pro **verze** konfigurace sestavení.
- Dynamickou registraci zpozdí tento pracovní dokud aplikace spustit a přeskočí generování kódu, ale tento další pracovní můžete vytvořit znatelné pozastavení (nejméně dvou sekund), ve spuštění aplikace. Toto je patrné zejména v sestavení pro konfiguraci ladění, což výchozí nastavení pro dynamickou registraci a jehož reflexe je pomalejší.

Částečné statický registrační poprvé v bodu 8.13 Xamarin.iOS, poskytuje vývojář nejlepší z obou možností. Informace o registraci všech prvků předem computing připravila `Xamarin.Mac.dll` a přesouvání tyto informace s Xamarin.Mac v statické knihovny (který stačí propojení v čase vytvoření buildu), Microsoft odebrala ve většině případů reflexe dynamické Registrátor při není, které mají vliv čase vytvoření buildu.

### <a name="enabling-the-partial-static-registrar"></a>Povolení částečné statické registrátora

Částečné statické registrátora je povolena v Xamarin.Mac dvojitým kliknutím **název projektu** v **Průzkumníku řešení**, navigační k **Mac sestavení** a přidání `--registrar:static` k **zhr další argumenty:** pole. Příklad:

![Přidání částečné statické registrátora do další zhr argumenty](how-it-works-images/psr01.png "přidání částečné statické registrátora k další zhr argumentů")

## <a name="additional-resources"></a>Další zdroje

Zde jsou některé podrobnější vysvětlení interně fungování věcí:

- [Selektory jazyka Objective-C](~/ios/internals/objective-c-selectors.md)
- [Registrátor](~/ios/internals/registrar.md)
- [Xamarin unifikované API pro iOS a OS X](~/cross-platform/macios/unified/index.md)
- [Základy Theading](~/ios/app-fundamentals/threading.md)
- [Delegáti, protokoly a události](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [o `newrefcount`](~/ios/internals/newrefcount.md)

