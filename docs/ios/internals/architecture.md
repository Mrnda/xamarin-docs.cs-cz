---
title: iOS architektura
description: Zkoumání Xamarin.iOS na nízké úrovni
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 85dc675a9b18b974f21532298e4d3028bdecd0b7
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="ios-architecture"></a>iOS architektura

Aplikace Xamarin.iOS spustit v prostředí Mono provádění a použít úplnou kompilace můžete některé z času (AOT) zkompilovat kód C# pro jazyk sestavení ARM. Toto spouští vedle sebe s [jazyka Objective-C Runtime](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/). Obě prostředí runtime běží nad jádro jako systému UNIX, konkrétně [XNU](https://en.wikipedia.org/wiki/XNU)a vystavit různé rozhraní API pro uživatelského kódu, což umožňuje vývojářům pro přístup k základním systému nativní nebo spravovaný.

Následující diagram ukazuje základní přehled této architektury:

[ ![](architecture-images/ios-arch-small.png "Tento diagram zobrazuje základní přehled o architektuře kompilace můžete některé z času (AOT)")](architecture-images/ios-arch.png#lightbox)

## <a name="native-and-managed-code-an-explanation"></a>Nativního a spravovaného kódu: vysvětlení

Při vývoji pro Xamarin podmínky *nativní a spravovaná* kódu se často používají. [Spravovaný kód](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/) je kód, který má jejího provádění spravuje [rozhraní .NET Framework Common Language Runtime](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx), nebo v případě pro Xamarin: Mono Runtime. Toto je takzvaný převodní jazyk.

Nativní kód je kód, který se spustí nativně na konkrétní platformu (například jazyka Objective-C nebo dokonce i AOT zkompilovat kód, na čip TPM ARM). Tato příručka popisuje, jak AOT zkompiluje spravovaného kódu do nativního kódu a vysvětluje, jak lze aplikaci Xamarin.iOS, provedení úplné použití společnosti Apple iOS rozhraní API prostřednictvím vazby, také přístup k. NET na BCL a sofistikované jazyk, například C#.


## <a name="aot"></a>AOT

Při kompilaci všechny platformy aplikace Xamarin kompilátoru Mono jazyka C# (nebo F #) se spustí a bude kompilace kódu C# a F # do Microsoft Intermediate Language (MSIL). Pokud používáte Xamarin.Android, Xamarin.Mac aplikace nebo i aplikace pro Xamarin.iOS v simulátoru, [rozhraní .NET Common Language Runtime (CLR)](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx) zkompiluje MSIL pomocí pouze v kompilátoru čas (JIT). Za běhu, které to kompiluje do nativního kódu, který můžete spustit na správnou architekturu pro vaši aplikaci.

Je však omezení zabezpečení v systému iOS, nastavte společností Apple, která zakáže spuštění dynamicky generovaném kódu na zařízení.
Aby se zajistilo, že jsme splňovat tyto protokoly zabezpečení, Xamarin.iOS místo toho používá kompilátoru můžete některé z času (AOT) pro kompilaci spravovaného kódu. To vytváří nativní aplikace pro iOS binární, volitelně s LLVM optimalizované pro zařízení, které lze nasadit na bázi ARM procesoru společnosti Apple. Dole je zobrazená hrubý diagram jak to zapadá společně:

[![](architecture-images/aot.png "Jak to zapadá společně hrubý diagram")](architecture-images/aot-large.png#lightbox)

Pomocí AOT má několik omezení, které jsou podrobně popsané na [omezení](~/ios/internals/limitations.md) průvodce. Také poskytuje řadu vylepšení přes JIT prostřednictvím snížení čas spuštění a různé optimalizace výkonu

Teď, když jsme jste prozkoumali jak zkompilovat kód ze zdroje do nativního kódu, se podíváme pod pokličkou zobrazíte jak Xamarin.iOS umožňuje psaní aplikací plně nativní aplikace pro iOS

## <a name="selectors"></a>Selektory

S Xamarinem, máme dvě samostatné ekosystémy, rozhraní .NET a společnosti Apple, která potřebujeme k přineste dohromady a zdá se, že jako efektivní, jako je možné, zajistit, že cílem je smooth uživatelské prostředí. Jsme viděli jste v části výše komunikace dva moduly runtime a je velmi dobře možná jste slyšeli o termín "vazby, což umožňuje nativní rozhraní API pro použití v Xamarin iOS. Vazby jsou vysvětlené podrobněji v našich [jazyka Objective-C vazby](~/cross-platform/macios/binding/overview.md) dokumentace, takže teď Podíváme se na tom, jak funguje iOS pod pokličkou.

Nejdřív musí existovat způsob, jak vystavit jazyka Objective-C jazyka C#, která se provádí pomocí selektorů. Selektor je zprávy, která je odeslána na objekt nebo třída. Pomocí jazyka Objective-C, to se provádí prostřednictvím [objc_msgSend](~/cross-platform/macios/binding/overview.md) funkce.
Další informace o používání selektorů, najdete v části [jazyka Objective-C selektory](~/ios/internals/objective-c-selectors.md) průvodce. Také musí existovat způsob, jak vystavit spravovaného kódu do jazyka Objective-C, což je složitější, vzhledem k tomu, že jazyka Objective-C neví nic o spravovaného kódu. Chcete-li se tomuto problému vyhnout, používáme *registrátorů*. Tyto jsou vysvětlené podrobněji v další části.

## <a name="registrars"></a>Registrátorů

Jak je uvedeno nahoře, je registrátora code této zpřístupňuje spravovaného kódu na cíl C. Dělá to tak, že vytvoříte seznam každý spravovaný třída odvozená z NSObject:

- Pro všechny třídy, které nejsou zabalení existující třídy jazyka Objective-C, vytvoří novou třídu jazyka Objective-C se členy jazyka Objective-C zrcadlení všechny spravované členy, kteří mají [`Export`] atributu.

- V implementacích pro každého člena Objective – C kód automaticky přidán do volání zrcadlené spravovaného člena.

Pseudo následující kód ukazuje příklad způsob provedení:

**C# (spravovaný kód)**

```csharp
 class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
```

**Cíl – C:**

```objectivec
@interface MyViewController : UIViewController { }

    -(void)myFunc;
@end

@implementation MyViewController {}

    -(void) myFunc
    {
        /* code to call the managed MyViewController.MyFunc method */
    }
@end

```

Spravovaný kód může obsahovat atributy, `[Register]` a `[Export]`, registrátora používající vědět, že objekt musí být vystaven na cíl C.
`[Register]` Atribut slouží k zadání názvu generované třídy jazyka Objective-C v případě, že název výchozí generovaný není vhodné. Všechny třídy odvozené od NSObject automaticky zaregistrovány v Objective-c
Požadované `[Export]` atribut obsahuje řetězec, který je selektor použít v generovaná třída jazyka Objective-C.

Existují dva typy registrátorů použít v Xamarin.iOS – dynamických a statických:


- **Dynamické registrátorů** – dynamické registrátora nepodporuje registraci všechny typy v sestavení za běhu. Dělá to pomocí funkce poskytované [Objective-C runtime rozhraní API](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/). Dynamické Registrátor má proto pomalejší spouštěcí, ale kratší čas sestavení. Toto je výchozí pro simulátoru iOS. Volání nativních funkcí (obvykle v jazyce C), trampolines, se používají jako metoda implementace při použití dynamické registrátorů. Se liší mezi různé architektury.

- **Statické registrátorů** – statické registrátora generuje kód jazyka Objective-C během vytváření sestavení, který je pak zkompiluje do statické knihovny a propojit do spustitelného souboru. To umožňuje rychlejší spouštění, ale během okamžiku sestavení trvá déle. Používá se ve výchozím nastavení pro zařízení sestavení. Statické registrátora lze také pomocí simulátoru iOS předáním `--registrar:static` jako `mtouch` atribut v projektu na sestavení možnosti, jak je uvedeno níže:

    [![](architecture-images/image1.png "Nastavení další mtouch argumenty")](architecture-images/image1.png#lightbox)

Další informace o jaké jsou specifikace iOS systém registrace typu používané Xamarin.iOS, najdete v části [typ registrátora](~/ios/internals/registrar.md) průvodce.

## <a name="application-launch"></a>Spuštění aplikace

Vstupní bod všech spustitelných souborů Xamarin.iOS poskytuje funkci s názvem `xamarin_main`, která inicializuje mono.

V závislosti na typu projektu následující provádí:

- Pro regulární aplikace pro iOS a tvOS se nazývá spravované metodu Main, poskytuje aplikaci Xamarin. To spravované hlavní metoda pak zavolá `UIApplication.Main`, což je vstupní bod pro cíl C. UIApplication.Main je vazba pro Objective-C `UIApplicationMain` metoda.
- Pro rozšíření, nativní funkce – `NSExtensionMain` nebo (`NSExtensionmain` pro rozšíření WatchOS) – poskytované společností Apple se nazývá knihovny. Protože tyto projektů knihovny tříd a není spustitelné projekty, nejsou žádné spravované metody Main provést.

Všechny toto pořadí spuštění zkompilován statickou knihovnu, která se pak propojí do konečné spustitelného souboru, aby vaše aplikace věděli, jak získat vypnout základů.

V tomto okamžiku má spuštění vaší aplikace, Mono běží, jsme jsou ve spravovaném kódu a víme, jak volání nativního kódu a zpětné volání. Další věcí, kterou je potřeba udělat, je ve skutečnosti začít přidávat ovládací prvky a vytvoření aplikace interaktivní.


## <a name="generator"></a>Generátor

Xamarin.iOS obsahuje definice pro každý jeden rozhraní API pro iOS. Můžete procházet některý z těchto na [úložiště github MaciOS](https://github.com/xamarin/xamarin-macios/tree/master/src). Tyto definice obsahovat rozhraní s atributy, a také všechny nezbytné metody a vlastnosti. Například následující kód je se používá k definování UIToolbar v UIKit [obor názvů](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327). Všimněte si, že se jedná o rozhraní s číslem metod a vlastností:

```csharp
[BaseType (typeof (UIView))]
public interface UIToolbar : UIBarPositioning {
    [Export ("initWithFrame:")]
    IntPtr Constructor (CGRect frame);

    [Export ("barStyle")]
    UIBarStyle BarStyle { get; set; }

    [Export ("items", ArgumentSemantic.Copy)][NullAllowed]
    UIBarButtonItem [] Items { get; set; }

    [Export ("translucent", ArgumentSemantic.Assign)]
    bool Translucent { [Bind ("isTranslucent")] get; set; }

    // done manually so we can keep this "in sync" with 'Items' property
    //[Export ("setItems:animated:")][PostGet ("Items")]
    //void SetItems (UIBarButtonItem [] items, bool animated);

    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage ([NullAllowed] UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);

    ...
}
```

Generátor, nazývá [ `btouch` ](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) v Xamarin.iOS, provede tyto definice soubory a používá nástroje rozhraní .NET pro [kompilována dočasné sestavení](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318). Toto dočasný sestavení však není funkční volat kód jazyka Objective-C. Generátor pak přečte dočasné sestavení a generuje kód C#, který lze použít za běhu.
Toto je jakých důvodů, například pokud přidáte do souboru definice .cs náhodných atribut, nezobrazí se v outputted kódu. Generátor neví o tom a proto `btouch` neví, jak hledat v dočasné sestavení výstup.

Po vytvoření Xamarin.iOS.dll mtouch se váže všechny součásti dohromady.

Na vysoké úrovni se toho dosahuje pomocí provádění následujících úloh:

-   Vytvořte strukturu sady aplikace.
-   Zkopírujte spravované sestavení.
-   Pokud je povoleno propojení, spusťte spravované linkeru k optimalizaci vašeho sestavení pomocí kopírování nepoužívaných částí se.
-   AOT kompilace.
-   Vytvoření nativní spustitelný soubor, který poskytuje řadu statické knihovny (jeden pro každé sestavení), které jsou propojeny do nativní spustitelný soubor, tak, aby nativní spustitelný soubor se skládá z kód spouštěče, kód registrátora (Pokud je nastavení static) a všechny výstupy z AOT kompilátoru


Podrobnější informace o linkeru a jak se používají, naleznete na [Linkeru](~/ios/deploy-test/linker.md) průvodce.

## <a name="summary"></a>Souhrn

Tato příručka prohlédli AOT kompilace aplikace Xamarin.iOS a prozkoumané Xamarin.iOS a jeho vztah k jazyka Objective-C podrobněji.

## <a name="related-links"></a>Související odkazy

- [Omezení](~/ios/internals/limitations.md)
- [Vytváření vazeb Objective-C](~/cross-platform/macios/binding/overview.md)
- [Selektory jazyka Objective-C](~/ios/internals/objective-c-selectors.md)
- [Typ registrátora](~/ios/internals/registrar.md)
- [Linker](~/ios/deploy-test/linker.md)
