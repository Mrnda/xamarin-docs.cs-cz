---
title: "Začínáme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 01c390af08e59f3b10888a183df7fa6758c2609c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started"></a>Začínáme

<style type="text/css"> .Terminal blue {color: rgb(10,96,254);} .terminal zelená {barva: rgb(12,156,26);} .terminal purpurová {barva: rgb(152,12,103);} </style>


> [!IMPORTANT]
> **Upozornění:** cíle Sharpie je nástroj pro zkušeného Xamarin vývojářům pokročilou znalost jazyka Objective-C (a rozšíření, C). Před pokusem o vytvořit vazbu knihovna jazyka Objective-C byste měli mít solidní znalosti, jak vytvářet nativní knihovny na příkazovém řádku (a dostatečné povědomí o tom, jak funguje nativní knihovny).

<a name="installing" />

# <a name="installing-objective-sharpie"></a>Instalace cíle Sharpie

Aktuálně je samostatný nástroj příkazového řádku pro Mac OS X 10.10 a novějších cíle Sharpie a _není plně podporované produktu Xamarin_. Tato by měla použít jen pokročilé vývojáři 3rd straně knihovna jazyka Objective-C jako pomůcku při vytváření vazby projektu.

Cíle Sharpie si můžete stáhnout jako standardní instalační program balíčku OS X.
Spusťte instalační program a postupujte podle všech na obrazovce zobrazí výzvu z Průvodce instalací:

- **Aktuální verze: 3.4**
  - [Stáhněte si nejnovější verzi](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [Fórum oznámení](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> 💡 **Tip:** použít `sharpie update` příkaz k aktualizaci na nejnovější verzi.

# <a name="basic-walkthrough"></a>Základní postup

Cíle Sharpie je nástroj příkazového řádku zadaný pomocí Xamarin, který pomáhá při vytváření definice potřebné k vytvoření vazby. 3. stran jazyka Objective-C knihovny jazyka C#.
I když se používá Sharpie cíl, Vývojář *bude* muset upravit generované soubory po dokončení Sharpie cíl, chcete-li vyřešit potíže, které nelze zpracovat automaticky nástrojem.

Kde je to možné, bude cíl Sharpie opatřit poznámkami rozhraní API, se kterým má některé nejistých o tom, jak správně bind (mnoho konstrukce v nativním kódu jsou nejednoznačné).
Tyto poznámky se zobrazí jako [ `[Verify]` atributy](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md).

Výstup Sharpie cíl je pár soubory – [ `ApiDefinition.cs` a `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) -který lze použít k vytvoření vazby projektu, který zkompiluje do knihovny můžete použít v aplikacích pro Xamarin.

> [!IMPORTANT]
> Cíle Sharpie se dodává s jedním **hlavní** pravidlo pro správné použití: musí nezbytně předáváte jí argumenty příkazového řádku kompilátoru správné clang aby se zajistilo správné analýze. Důvodem je, že Sharpie cíl analýza fáze je jednoduše nástroj [implementována proti libtooling clang rozhraní API](http://clang.llvm.org/docs/LibTooling.html).

To znamená, že cíl Sharpie má potenciál Clang (kompilátor C/Objective-C nebo C++ ve skutečnosti kompilovaný nativní knihovna, kterou by vazby) a všechny jeho vnitřní znalosti soubory hlaviček pro vazbu.
Místo překladu analyzovanou klauzuli [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree) objekt kódu cíl Sharpie překládá AST pro C# vazba "vygenerované uživatelské rozhraní" vhodný pro vstup na `bmac` a `btouch` nástroje Xamarin vazby.

Pokud cíl Sharpie chyby se při analýze, znamená to, že se chyba během clang jeho analýza se první fáze pokusu o vytvoření AST a potřebujete zjistit, proč.

**NOVÉ!** verze 3.0 pokusů o zadání některé z této složitost vyřešit tak, že podpora projekty Xcode přímo. Pokud nativní knihovny má platný projekt Xcode, můžete vyhodnotit cíl Sharpie projekt pro zadaný cíl a konfiguraci odvodit nezbytné vstupní hlavičkových souborů a kompilátoru značky.

Pokud žádný projekt Xcode je k dispozici, musíte být známější s projekt tak, že deducing správný vstupní hlavičkových souborů, cesty pro hledání hlavičky souborů a nastavení další příznaky nezbytné kompilátoru. Je důležité si uvědomit, že kompilátoru příznaky použité pro sestavení nativní knihovny jsou stejné, který musí být předán Sharpie cíl. To je více ruční proces a ten, který nevyžaduje kousek znalost kompilace nativního kódu na příkazový řádek s nástrojů Clang.

**NOVÉ!** verze 3.0 také zavádí nástroj pro snadno vazby [CocoaPods](https://cocoapods.org) prostřednictvím `sharpie pod` příkaz.
Pokud je k dispozici jako CocoaPod knihovna, kterou byste chtěli, doporučujeme, abyste že začnete pokusu o vytvoření vazby CocoaPod s cílem Sharpie (oproti došlo k pokusu o vytvoření vazby na zdroji přímo).