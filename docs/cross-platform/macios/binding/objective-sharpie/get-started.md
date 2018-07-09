---
title: Začínáme s Sharpie cíle
description: Tento dokument obsahuje přehled Sharpie cíle, nástroj použitý k automatizaci vytváření vazby na C# kód Objective-C.
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: da8c51c4ba4df74afac950bbff867221e7307d6e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854776"
---
# <a name="getting-started-with-objective-sharpie"></a>Začínáme s Sharpie cíle

> [!IMPORTANT]
> Cíle Sharpie je nástroj pro zkušené vývojáře Xamarin pomocí pokročilé znalosti jazyka Objective-C (a při rozšíření i pro C). Než se pokusíte vytvořit vazbu knihovnou Objective-C byste měli mít plnou znalosti o tom, a začít vytvářet nativní knihovnu pro příkazový řádek (a dobrý přehled o fungování nativní knihovny).

<a name="installing" />

## <a name="installing-objective-sharpie"></a>Instalace Sharpie cíle

Cíle Sharpie aktuálně je samostatný nástroj příkazového řádku pro Mac OS X 10.10 a novější a je _nejsou plně podporované produkty Xamarin_. To by měla sloužit pouze pokročilé vývojáře které pomáhají při vytváření vazby projektu 3. straně knihovnou Objective-C.

Cíle Sharpie si můžete stáhnout jako standardní instalačního programu balíčků OS X.
Spusťte instalační program a postupujte podle všech na obrazovce zobrazí výzvu pomocí Průvodce instalací:

- **Aktuální verze: 3.4**
  - [Stáhněte si nejnovější vydaná verze](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [Fórum pro hlášení](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> Použití `sharpie update` příkaz k aktualizaci na nejnovější verzi.

## <a name="basic-walkthrough"></a>Základní Průvodce

Cíle Sharpie je nástroj příkazového řádku zadané pomocí Xamarin, která pomáhá při vytváření definice nezbytný pro vazbu 3. stran Objective-C knihovny jazyka C#.
I když se používá Sharpie cíle, Vývojář *bude* muset upravit generované soubory po dokončení Sharpie cíle na vyřešení problémů, které nelze zpracovat automaticky nástrojem.

Tam, kde je to možné, budou cíle Sharpie opatřit poznámkami rozhraní API, se kterým má některé pochybnosti o tom, jak správně vytvořit vazbu (mnoho konstrukce v nativním kódu jsou nejednoznačné).
Tyto poznámky se zobrazí jako [ `[Verify]` atributy](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md).

Výstup cíle Sharpie je pár souborů – [ `ApiDefinition.cs` a `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) –, který slouží k vytvoření vazby projektu, který se zkompiluje do knihovny můžete použít v aplikacích Xamarin.

> [!IMPORTANT]
> Cíle Sharpie součástí jedné **hlavní** pravidlo pro správné použití: musíte naprosto jí předat argumenty příkazového řádku kompilátoru správné clang aby se zajistilo správné analýze. Důvodem je, že cíl Sharpie parsování fáze je jednoduše nástroj [proti libtooling clang rozhraní API implementované](http://clang.llvm.org/docs/LibTooling.html).

To znamená, že cíl Sharpie má potenciál Clang (C/Objective-C/C++ kompilátor, který ve skutečnosti zkompiluje na nativní knihovnu, kterou by vazby) a všechny její interní znalosti soubory hlaviček pro vazbu.
Místo překladu analyzovanou klauzuli [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree) na kód objektu cíle Sharpie přeloží AST pro C# vazba "vygenerované uživatelské rozhraní" vhodný pro vstup do `bmac` a `btouch` nástroje Xamarin vazby.

Pokud cíl Sharpie chyby si při analýze, znamená to této clang došlo k chybě při jejich analýze fáze pokusu o vytvoření AST a potřebujete zjistit, proč.

**NOVÉ!** verze 3.0 pokusy řešení některých Tato složitost díky podpoře projekty Xcode přímo. Pokud nativní knihovnu má platný projekt Xcode, můžete vyhodnotit Sharpie cíle projektu pro zadaný cíl a konfiguraci k odvození nezbytné vstupní hlavičkové soubory a příznaky kompilátoru.

Pokud je k dispozici žádný projekt Xcode, musíte být více do hloubky projekt podle odvozuje se správné hlavičky vstupní soubory, cesty pro hledání souborů záhlaví a další příznaky kompilátoru nezbytné. Je důležité si uvědomit, že příznaky kompilátoru sloužící k sestavení nativní knihovnu jsou stejné, musí být předán Sharpie cíle. To je více ruční proces a ten, který vyžaduje hodně znalost kompilace nativního kódu v příkazovém řádku se sadou nástrojů Clang.

**NOVÉ!** verze 3.0 poskytuje také nástroj pro snadno vazby [CocoaPods](https://cocoapods.org) prostřednictvím `sharpie pod` příkazu.
Pokud je k dispozici jako CocoaPod knihovny, který vás zajímá, doporučujeme že začnete tím, že pokus o vytvoření vazby CocoaPod s cílem Sharpie (oproti došlo k pokusu o vytvoření vazby na zdroji přímo).

## <a name="related-links"></a>Související odkazy

- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)