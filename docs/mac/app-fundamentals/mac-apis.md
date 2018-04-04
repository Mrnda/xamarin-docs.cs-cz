---
title: Rozhraní API Macu
description: Tento dokument popisuje, jak číst selektory jazyka Objective-C a jak se mají najít jejich odpovídající metody C#.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: 0344fecb9a8d64a680bb11689f56cf074d952f4e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="mac-apis"></a>Rozhraní API Macu

## <a name="overview"></a>Přehled

Pro většinu doby vývoj s Xamarin.Mac vezměte v úvahu, čtení a zápis v jazyce C# bez mnohem problém na základní rozhraní API jazyka Objective-C. V některých případech můžete ale budete potřebovat od společnosti Apple, přečtěte si dokumentaci rozhraní API převede na odpověď od Stack Overflow řešení pro váš problém, nebo porovnat s existující vzorek.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Čtení dostatek Objective-C nebezpečné

Někdy je nezbytné pro čtení definici jazyka Objective-C nebo metoda volání a který převede ekvivalentní metodě C#. Pojďme si prohlédněte definici funkce jazyka Objective-C a rozdělení částí do jednoho. Tato metoda ( *selektor* v Objective-C) můžete najít na `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

Deklaraci můžete přečíst vlevo, vpravo:

- `-` Předponu znamená, že je metoda instance (nestatické). + znamená je metoda třídy (statické)
- `(BOOL)` je návratový typ (logická hodnota v jazyce C#)
- `canDragRowsWithIndexes` představuje první část názvu.
- `(NSIndexSet *)rowIndexes` je první param a s ním má typu. První parametr je ve formátu: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` je druhý param a jeho typu. Každý parametr po první je ve formátu: `selectorPart:(Type) pararmName`
- Úplný název selektoru tato zpráva je: `canDragRowsWithIndexes:atPoint:`. Poznámka: `:` na konci – je důležité.
- Skutečná vazba Xamarin.Mac C# je: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

Toto volání selektor může číst stejným způsobem jako:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- Instance `v` s jeho `canDragRowsWithIndexes:atPoint` selektor volána s dva parametry `set` a `point`, je předaná.
- V jazyce C# volání metody vypadat třeba takto: `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>Hledání člen C# pro danou selektor

Teď, když jste najde selektor jazyka Objective-C, které potřebujete k vyvolání, dalším krokem je, mapování na ekvivalentní člen C#. Existují čtyři přístupy, můžete zkusit (budete pokračovat `NSTableView CanDragRows` příklad):

1. Pomocí seznamu dokončení automaticky můžete rychle vyhledat něco se stejným názvem. Vzhledem k tomu, že jsme víte, že se o instanci `NSTableView` můžete zadat:

    - `NSTableView x;`
    - `x.` [ctrl + místa, pokud v seznamu nezobrazí.).
    - `CanDrag` [Zadejte]
    - Klikněte pravým tlačítkem na metodu, přejděte na deklaraci otevřete prohlížeč sestavení, kde můžete porovnat `Export` atribut modulu pro výběr v

2. Hledat celou třídu vazby. Vzhledem k tomu, že jsme víte, že se o instanci `NSTableView` můžete zadat:

    - `NSTableView x;`
    - Klikněte pravým tlačítkem na `NSTableView`, přejděte na deklaraci do prohlížeče sestavení
    - Vyhledejte modulu pro výběr v

3. Můžete použít [online dokumentaci rozhraní API Xamarin.Mac](https://developer.xamarin.com/api/root/monomac-lib/) .

4. Miguel poskytuje "Rosetta kamenem" zobrazení rozhraní API Xamarin.Mac [sem](http://tirania.org/tmp/rosetta.html) lze vyhledat prostřednictvím pro dané rozhraní API. Pokud vaše rozhraní API není AppKit nebo konkrétní systému macOS, bude pravděpodobně existuje.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
