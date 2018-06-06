---
title: Práce s tabulkami a buněk v Xamarin.iOS
description: Tento dokument obsahuje odkazy na různé příručky, které popisují, jak zobrazit data pomocí ovládacího prvku UITableView v aplikaci pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: ebdad2cc8e3083bee5acc127660b5641f42c731f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790013"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>Práce s tabulkami a buněk v Xamarin.iOS

Tato část představuje třídy používané k vytváření a zobrazování tabulky pak poskytuje příklady, jak je používat v Xamarin.iOS. Bude se vztahovat pomocí výchozí vzhled pro tabulky, přizpůsobení rozložení, implementace úpravy a použití Xamarin iOS Designer pro vizuální návrh tabulky. Někdy zobrazení je samozřejmě seznam řádků (například aplikace Hudba) a dalších akcí, kdy je obtížné rozpoznat ovládacího prvku tabulky (např. úpravy v aplikaci kontakty nebo konverzace v aplikaci zprávy).

Při práci v aplikacích pro různé platformy s Xamarin.Android ovládacího prvku UITableView je podobná ListView třídy v Android (a třída UITableViewSource je podobná třídy adaptéru na Android).

Tyto články bude trvat komplexní pohled na práci s tabulky, včetně:

-   **Tabulka částí** – Introducing a vysvětlením vizuální prvky `UITableView` ovládacího prvku. 
-   **Zobrazení dat v tabulkách** – který ukazuje, jak vytvořit a naplnit tabulku, používat různé styly tabulky a buněk a předešli problémům s paměti podle recyklace buňky objekty. 
-   **Rozšířené použití** – vytváření vlastní buněk a pomocí funkce úprav UITableView třídy. 
-   **Vytvoření tabulky vizuálně** – pomocí návrháře Xamarin pro iOS k vytváření rozhraní řízené tabulky s scénáře. 

## <a name="contents"></a>Obsah

 [Tabulka částí &amp; funkce](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Naplnění tabulky daty](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Přizpůsobení vzhledu tabulky](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Úprava](~/ios/user-interface/controls/tables/editing.md)
 
 [Řádek akce](~/ios/user-interface/controls/tables/row-action.md)

 [Vytváření tabulek v scénáře](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [Automatické nastavování výšky řádku](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>Související odkazy

- [WorkingWithTables (ukázka)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [Tabulky v scénářů (ukázka)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Úvod do scénářů](~/ios/user-interface/storyboards/index.md)
- [Scénáře recepturách zobrazení Tabulka](https://developer.xamarin.com/recipes/ios/general/storyboard/storyboard_a_tableview)
- [Úvod do MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [TableEditing ukázce na Githubu](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [TableParts ukázce na Githubu](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [TableAndCellStyles ukázce na Githubu](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [Odkaz na UITableView – třída](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [Odkaz na UITableViewCell – třída](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
