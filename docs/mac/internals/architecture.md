---
title: Architektura Xamarin.Mac
description: Tato příručka prozkoumá Xamarin.Mac a jeho relace do jazyka Objective-C na nízké úrovni. Vysvětluje koncepty například kompilace, selektory, registrátorů, aplikace a generátor.
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: d6d7557fed5ea0ca0719dcbddbda316340645320
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30785553"
---
# <a name="xamarinmac-architecture"></a>Architektura Xamarin.Mac

_Tato příručka prozkoumá Xamarin.Mac a jeho relace do jazyka Objective-C na nízké úrovni. Vysvětluje koncepty například kompilace, selektory, registrátorů, aplikace a generátor._

## <a name="overview"></a>Přehled

Xamarin.Mac aplikace spustit v prostředí Mono provádění a použít na Xamarin kompilátoru zkompilovat dolů Intermediate Language (IL), který je pak stačí za běhu (JIT) zkompilovány do nativního kódu v době spuštění. Toto spouští vedle sebe s modulem Runtime jazyka Objective-C. Obě prostředí runtime běží nad jádra jako systému UNIX, konkrétně XNU a vystavit různé rozhraní API pro uživatelského kódu, což umožňuje vývojářům pro přístup k základním systému nativní nebo spravovaný.

Následující diagram ukazuje základní přehled této architektury:

[![Diagram zobrazující základní přehled architektury](architecture-images/mac-arch.png "Diagram znázorňující základní přehled architektury")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>Nativního a spravovaného kódu

Při vývoji pro Xamarin, podmínky *nativní* a *spravované* kódu se často používají. Spravovaný kód je kód, který má jejího provádění spravovaného modulu .NET Framework CLR, nebo v případě pro Xamarin: Mono Runtime.

Nativní kód je kód, který se spustí nativně na konkrétní platformu (například jazyka Objective-C nebo dokonce i AOT zkompilovat kód, na čip TPM ARM). Tato příručka prozkoumá jak kompiluje do nativního kódu spravovaného kódu a vysvětluje, jak lze aplikaci Xamarin.Mac, provedení úplné použití společnosti Apple Mac rozhraní API prostřednictvím vazby, přitom má také přístup k. NET na BCL a sofistikované jazyk, například C#.

## <a name="requirements"></a>Požadavky

Toto je požadováno pro vývoj aplikací systému macOS s Xamarin.Mac:

- Systému Mac spuštěné macOS Sierra (10.12) nebo vyšší.
- Nejnovější verzi Xcode (instalovat z [obchod](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- Nejnovější verzi Xamarin.Mac a Visual Studio pro Mac

Spuštěné aplikace systému Mac, které jsou vytvořené pomocí Xamarin.Mac mají následující požadavky:

- Mac, který se systémem Mac OS X 10,7 nebo větší.

## <a name="compilation"></a>Kompilace

Při kompilaci všechny platformy aplikace Xamarin kompilátoru Mono jazyka C# (nebo F #) se spustí a bude kompilována Microsoft Intermediate Language (MSIL nebo IL) kódu C# a F #. Xamarin.Mac pak použije *těsně čas (JIT)* kompilátoru za běhu kompilace nativního kódu, umožňuje provádění na správnou architekturu podle potřeby.

Tím se liší od Xamarin.iOS, která používá AOT kompilace. Při použití kompilátoru AOT, všechna sestavení a všechny metody v nich kompilovány v čase vytvoření buildu. S JIT probíhá kompilace na vyžádání pouze pro metody, které jsou spouštěny.

U aplikací Xamarin.Mac Mono obvykle integrovaný do sady prostředků aplikace (a označovat jako **Embedded Mono**). Pokud používáte klasické rozhraní API Xamarin.Mac, může aplikace místo toho použijte **systému Mono**, ale to není podporována v unifikované API. Systém Mono odkazuje na Mono, která byla nainstalována v operačním systému. Při spuštění aplikace Xamarin.Mac aplikace bude používat toto.

## <a name="selectors"></a>Selektory

S Xamarinem, máme dvě samostatné ekosystémy, rozhraní .NET a společnosti Apple, která potřebujeme k přineste dohromady a zdá se, že jako efektivní, jako je možné, zajistit, že cílem je smooth uživatelské prostředí. Jsme viděli jste v části výše komunikace dva moduly runtime a je velmi dobře možná jste slyšeli o termín "vazby, což umožňuje nativních rozhraní API Mac mají být použity v Xamarin. Vazby jsou vysvětlené podrobněji v [jazyka Objective-C vazby dokumentaci](~/mac/platform/binding.md), takže prozatím se podíváme, jak funguje Xamarin.Mac pod pokličkou.

Nejdřív musí existovat způsob, jak vystavit jazyka Objective-C jazyka C#, která se provádí pomocí selektorů. Selektor je zprávy, která je odeslána na objekt nebo třída. Pomocí jazyka Objective-C, to se provádí prostřednictvím [objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html) funkce. Další informace o používání selektorů, najdete v části iOS [jazyka Objective-C selektory](~/ios/internals/objective-c-selectors.md) průvodce. Také musí existovat způsob, jak vystavit spravovaného kódu do jazyka Objective-C, což je složitější, vzhledem k tomu, že jazyka Objective-C neví nic o spravovaného kódu. Chcete-li se tomuto problému vyhnout, používáme [registrátora](~/mac/internals/registrar.md). To vysvětlené podrobněji v další části.

## <a name="registrar"></a>Registrátor

Jak je uvedeno nahoře, je registrátora code této zpřístupňuje spravovaného kódu na cíl C. Dělá to tak, že vytvoříte seznam každý spravovaný třída odvozená z NSObject:

- Pro všechny třídy, které nejsou zabalení existující třídy jazyka Objective-C, vytvoří novou třídu jazyka Objective-C se členy jazyka Objective-C zrcadlení všechny spravované členy, kteří mají `[Export]` atribut.
- V implementacích pro každého člena Objective – C kód automaticky přidán do volání zrcadlené spravovaného člena.

Pseudo následující kód ukazuje příklad způsob provedení:

**C# (spravovaný kód):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**Objective-C (nativní kód):**

```objc
@interface MyViewController : UIViewController
 - (void)myFunc;
@end 

@implementation MyViewController
- (void)myFunc {
    // Code to call the managed C# MyFunc method in MyViewController
}
@end
```

Spravovaný kód může obsahovat atributy, `[Register]` a `[Export]`, registrátora používající vědět, že objekt musí být vystaven na cíl C. Atribut [registrace] slouží k určení názvu generované třídy jazyka Objective-C v případě, že název výchozí generovaný není vhodné. Všechny třídy odvozené od NSObject automaticky zaregistrovány v Objective-c Požadovaný atribut [Export] obsahuje řetězec, který je selektor použít v generovaná třída jazyka Objective-C.

Existují dva typy registrátorů použít v Xamarin.Mac – dynamických a statických:

- Dynamické registrátorů – Toto je výchozí registrátora pro všechny Xamarin.Mac sestavení. Dynamické registrátora nepodporuje registraci všechny typy v sestavení za běhu. Dělá to pomocí funkce poskytované Objective-C runtime rozhraní API. Dynamické Registrátor má proto pomalejší spouštěcí, ale kratší čas sestavení. Volání nativních funkcí (obvykle v jazyce C), trampolines, se používají jako metoda implementace při použití dynamické registrátorů. Se liší mezi různé architektury.
- Statické registrátorů – statické registrátora generuje kód jazyka Objective-C během vytváření sestavení, který je pak zkompiluje do statické knihovny a propojit do spustitelného souboru. To umožňuje rychlejší spouštění, ale během okamžiku sestavení trvá déle.

## <a name="application-launch"></a>Spuštění aplikace

Logika spuštění Xamarin.Mac se budou lišit v závislosti na tom, zda vložených nebo se používá systém Mono. Zobrazení kódu a kroky pro spuštění aplikace Xamarin.Mac, najdete v tématu [spuštění záhlaví](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) souboru v úložišti veřejné xamarin macios.

## <a name="generator"></a>Generátor

Xamarin.Mac obsahuje definice pro každý Mac rozhraní API. Můžete procházet některý z těchto na [úložiště github MaciOS](https://github.com/xamarin/xamarin-macios/tree/master/src). Tyto definice obsahovat rozhraní s atributy, a také všechny nezbytné metody a vlastnosti. Například následující kód je se používá k definování NSBox v [obor názvů AppKit](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526). Všimněte si, že se jedná o rozhraní s číslem metod a vlastností:

```csharp
[BaseType (typeof (NSView))]
public interface NSBox {

        …

        [Export ("borderRect")]
        CGRect BorderRect { get; }

        [Export ("titleRect")]
        CGRect TitleRect { get; }

        [Export ("titleCell")]
        NSObject TitleCell { get; }

        [Export ("sizeToFit")]
        void SizeToFit ();

        [Export ("contentViewMargins")]
        CGSize ContentViewMargins { get; set; }

        [Export ("setFrameFromContentFrame:")]
        void SetFrameFromContentFrame (CGRect contentFrame);

        …

}
```

Generátor, názvem `bmac` v Xamarin.Mac, provede tyto definice soubory a kompilována dočasné sestavení pomocí nástroje rozhraní .NET. Toto dočasný sestavení však není funkční volat kód jazyka Objective-C. Generátor pak přečte dočasné sestavení a generuje kód C#, který lze použít za běhu. Toto je jakých důvodů, například pokud přidáte do souboru definice .cs náhodných atribut, nezobrazí se v outputted kódu. Generátor neví a proto `bmac` neví, jak hledat v dočasné sestavení výstup.

Po vytvoření Xamarin.Mac.dll, Balíčkovač, `mmp`, budou všechny součásti sady společně.

Na vysoké úrovni se toho dosahuje pomocí provádění následujících úloh:

- Vytvořte strukturu sady aplikace.
- Zkopírujte spravované sestavení.
- Pokud je povoleno propojení, spusťte spravované linkeru k optimalizaci vašeho sestavení odebráním nepoužívaných částí.
- Vytvoření aplikace Spouštěče propojení v kód Spouštěče věnovala spolu s kódem registar Pokud ve statickém režimu.

Toto je pak spustit jako součást uživatel tento odkaz Xamarin.Mac.dll a spustí proces, kompilovaný kód uživatele do sestavení sestavení `mmp` Chcete-li balíček

Podrobnější informace o linkeru a jak se používají, naleznete na webu iOS [Linkeru](~/ios/deploy-test/linker.md) průvodce.

## <a name="summary"></a>Souhrn

Tato příručka zvážení kompilace Xamarin.Mac aplikací a prozkoumané Xamarin.Mac a jeho relace na cíl C.
