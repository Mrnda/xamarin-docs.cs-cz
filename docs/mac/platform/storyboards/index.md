---
title: "Úvod do scénářů"
description: "Tento článek obsahuje úvod do práce s scénářů v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu uživatelském rozhraní aplikace pomocí scénářů a Tvůrce rozhraní pro Xcode."
ms.topic: article
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 21e35b056293e422b577b0ee8b51e8c43dbbf07d
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="introduction-to-storyboards"></a>Úvod do scénářů

_Tento článek obsahuje úvod do práce s scénářů v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu uživatelském rozhraní aplikace pomocí scénářů a Tvůrce rozhraní pro Xcode._

Scénářů umožňují vývoj uživatelského rozhraní pro aplikaci Xamarin.Mac, ne jenom zahrnuje definice oken a ovládacích prvků, ale obsahuje taky odkazy mezi různé windows (prostřednictvím segues) a zobrazit stavy.

[ ![](images/intro01.png "Ukázka uživatelského rozhraní v Xcode")](images/intro01.png)

Tento článek poskytne úvod k definování Xamarin.Mac aplikace uživatelské rozhraní pomocí scénářů.

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>Jaké jsou scénářů?

S použitím scénářů, všechny aplikace na Xamarin.Mac uživatelského rozhraní lze definovat v jednom umístění se všemi navigace mezi jeho jednotlivé elementy a uživatelského rozhraní. Scénářů pro Xamarin.Mac, fungují v velmi podobně jako ochrana scénářů pro Xamarin.iOS. Však obsahují jinou sadu _Segue typy_ kvůli idioms různých rozhraní.

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>Práce s scény

Jak jsme uvedli výše, scénáře definuje všechny uživatelské rozhraní pro danou aplikaci rozdělit do funkční přehled jeho _řadiče zobrazení_. V Tvůrci rozhraní Xcode na každý z těchto řadičů žije v jeho vlastní _scény_.

[ ![](images/intro02.png "Řadič příklad zobrazení")](images/intro02.png)

Každé scény reprezentuje daného zobrazení a zobrazení řadiče pár sadu řádků (nazývané Segues), které připojit každé scény v uživatelském rozhraní, což ukazuje jejich vztahů. Některé Segues definovat jak jeden řadič zobrazení obsahuje jeden nebo více podřízených zobrazení nebo zobrazení řadičů. Další Segues definovat přechodů mezi View Controller (například zobrazení popover nebo dialogové okno pole). 

[ ![](images/intro03.png "Ukázka segue")](images/intro03.png)

Je třeba mít na paměti je, že každý Segue představuje toku určitou formu dat mezi daného elementu uživatelském rozhraní aplikace.

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>Práce s řadiče zobrazení

Zobrazení řadičů definovat vztahy mezi daného zobrazení informací v rámci aplikace Mac a datový model, který poskytuje tyto informace. Každý nejvyšší úrovně scény ve scénáři, představuje jeden řadič zobrazení v kódu aplikace Xamarin.Mac.

[ ![](images/intro04.png "Příklad skluzu řadiče zobrazení")](images/intro04.png)

Tímto způsobem každého řadiče zobrazení je samostatná, opakovaně použitelný párování informace o vizuální reprezentace (zobrazení) a logiku pro řízení těchto informací a k dispozici.

V rámci dané scény, můžete provést všechny věcí, které by normálně byly zpracovány fyzická `.xib` soubory: 

 - Místní subviews a ovládací prvky (například tlačítka a polí).
 - Zadejte element pozic a automatického rozložení omezení.
 - Navázání akce a výstupy ke zveřejnění prvky uživatelského rozhraní ke kódu.

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>Práce s Segues

Jak jsme uvedli výše, zadejte Segues vztahy mezi všemi na pozadí, které definují rozhraní vaší aplikace. Pokud jste obeznámeni s práce v scénářů v iOS, víte, že Segues pro iOS obvykle definují přechodů mezi zobrazení celé obrazovky. To se liší od systému macOS, když Segues obvykle definují "omezení" (kde jeden scény je podřízená položka nadřazené scény).

V systému macOS zpravidla skupiny jejich zobrazení společně v rámci stejného časového období používání prvky uživatelského rozhraní, jako je například rozdělení zobrazení a karty většiny aplikací. Na rozdíl od iOS, kde zobrazení musí být přešla zapnout a vypnout obrazovku, zobrazení kvůli omezené fyzického místa.

S ohledem na systému macOS tendence směrem členství ve skupině, existují situace, kdy _Segues prezentace_ používají, jako je například modální okna, náhledů a Popovers.

Při použití Segues prezentace, můžete přepsat `PrepareForSegue` metoda nadřazené řadič zobrazení pro prezentaci k chybě při inicializaci a proměnné a poskytují řadiče zobrazení se zobrazí žádná data.

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>Návrh a doby běhu

V době návrhu (když rozložení se uživatelské rozhraní v Tvůrci Xcode na rozhraní), každý prvek uživatelském rozhraní aplikace je rozdělena do základní položky:

- **Scény** – které se skládají ze:
    - **Zobrazit řadiče** -, definovat vztahy mezi zobrazení a data, která je podporují.
    - **Zobrazení a dílčích zobrazení** -skutečné prvky, které tvoří uživatelské rozhraní.
    - **Členství ve skupině Segues** – které definují vztahy podřízenosti mezi scény.
- **Prezentace Segues** – které definují jednotlivé prezentační režim. 

Definováním jednotlivých prvků tímto způsobem umožňuje pro každý prvek opožděného načítání jenom dle potřeby za běhu. V systému macOS, celý proces byla navržena k umožnění vývojáři vytvořit komplexní, flexibilní uživatelského rozhraní, které vyžadují minimálně úplné zálohování kód pro jejich fungování, všechny při se co nejvíce jako efektivní s systémové prostředky.

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>Scénáře rychlý Start

V [Storyboard rychlý Start](~/mac/platform/storyboards/quickstart.md) průvodce, vytvoříme jednoduchou aplikaci Xamarin.Mac, která představuje klíčové koncepty práce scénářů pro vytvoření uživatelského rozhraní. Ukázková aplikace se skládá z oddělení zobrazení obsahující _oblast obsahu_ a _Inspector oblasti_ a nabídne jednoduché předvolby dialogového okna. Budeme ke svázání všechny prvky uživatelského rozhraní společně používat Segues.

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>Práce s scénářů

Tato část obsahuje podrobný podrobnosti [práce s scénářů](~/mac/platform/storyboards/indepth.md) v Xamarin.Mac aplikaci. Můžeme provést hlubší pohled na scény a jak se skládají řadiče zobrazení a zobrazení. Potom provedeme podívejte se na tom, jak jsou svázané scény společně s Segues. Nakonec provedeme vypadá a pracuje s vlastní typy Segue. 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Příklad komplexní scénáře

Příklad komplexní příklad práci s scénářů v aplikaci Xamarin.Mac, najdete v tématu [SourceWriter ukázkovou aplikaci](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter je editor jednoduché zdrojového kódu, který poskytuje podporu pro doplňování kódu a zvýraznění syntaxe jednoduché.

Kód SourceWriter má plně komentář, pokud je k dispozici odkazy být zadané a z klíčové technologie nebo metody na příslušné informace v dokumentaci k Xamarin.Mac příručky.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek trvá rychlý přehled práce s scénářů v aplikaci Xamarin.Mac. Jsme viděli, jak vytvořit novou aplikaci pomocí scénářů a jak definovat uživatelské rozhraní. Také jsme viděli postup přecházet mezi různé windows a zobrazení stavů pomocí segues.


## <a name="related-links"></a>Související odkazy

- [Hello, Mac (ukázka)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Práce s Windows](~/mac/user-interface/window.md)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Úvod do systému Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
