---
title: macOS rozhraní API pro vývojáře Xamarin.Mac
description: Tento dokument popisuje, jak číst selektory Objective-C a tom, jak najít jejich odpovídající metody jazyka C# v aplikaci pro Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: 6dfaa3c7bf988228bfbacefe7c8e7268edc8117a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994309"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>macOS rozhraní API pro vývojáře Xamarin.Mac

## <a name="overview"></a>Přehled

Pro mnoho času vývoj s využitím Xamarin.Mac představit, čtení a zápis v jazyce C# bez obav, mnoho základní rozhraní API jazyka Objective-C. Ale někdy budete potřebujete od společnosti Apple, přečtěte si dokumentaci rozhraní API přeložit odpověď z přetečení zásobníku do řešení pro váš problém, nebo porovnat s existující vzorek.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Čtení dostatek Objective-C nebezpečné

V některých případech bude nutné načíst definici Objective-C nebo metoda volání a překladu, který ekvivalentní metodě jazyka C#. Pojďme podívejte se na definici funkce Objective-C a rozdělit části. Tuto metodu ( *selektor* v Objective-C) můžete najít na `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

Deklarace může být čteny zleva doprava:

- `-` Předponu znamená, že je metoda instance (nestatické). + znamená, že se jedná o metodu třídy (statické)
- `(BOOL)` je návratový typ (bool v jazyce C#)
- `canDragRowsWithIndexes` je první část názvu.
- `(NSIndexSet *)rowIndexes` je první parametr a má typ s ním. První parametr je ve formátu: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` je druhý parametr a jeho typu. Každý parametr po prvním má tento formát: `selectorPart:(Type) pararmName`
- Úplný název selektor tato zpráva je: `canDragRowsWithIndexes:atPoint:`. Poznámka: `:` na konci – je důležité.
- Skutečná vazba Xamarin.Mac C# je: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

Toto volání selektor může číst stejným způsobem jako:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- Instance `v` dochází k danému jeho `canDragRowsWithIndexes:atPoint` selektor volat se dvěma parametry `set` a `point`, předané.
- V jazyce C# volání metody vypadá například takto: `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>Vyhledání člen C# pro daný selektor

Teď, když najdete modulu pro výběr jazyka Objective-C, které potřebujete k vyvolání, dalším krokem, který mapuje na ekvivalentní člen C#. Můžete zkusit čtyři způsoby (pokračujte `NSTableView CanDragRows` příklad):

1. Pomocí seznamu dokončení automaticky rychle vyhledávat něco se stejným názvem. Protože víme, že to je instanci `NSTableView` můžete zadat:

    - `NSTableView x;`
    - `x.` [ctrl + MEZERNÍK, pokud se nezobrazí v seznamu).
    - `CanDrag` [enter]
    - Klikněte pravým tlačítkem na metodu, přejděte do deklarace otevřít prohlížeč sestavení, ve kterém můžete porovnat `Export` atribut dotyčný selektoru

2. Hledejte celá třída vazby. Protože víme, že to je instanci `NSTableView` můžete zadat:

    - `NSTableView x;`
    - Klikněte pravým tlačítkem na `NSTableView`, přejděte na deklaraci do prohlížeče sestavení
    - Vyhledejte dotyčný selektor

3. Můžete použít [online dokumentaci k rozhraní API Xamarin.Mac](https://docs.microsoft.com/dotnet/api/?view=xamarinmac-3.0) .

4. Miguel poskytuje přehled rozhraní API Xamarin.Mac "Rosetta kámen" [tady](http://tirania.org/tmp/rosetta.html) , která může prohledávat pro dané rozhraní API. Pokud vaše rozhraní API není prvcích AppKit nebo macOS konkrétní, možná bude existuje.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
