---
title: Práce s tabulkami a buňky v Xamarin.iosu
description: Tento dokument obsahuje odkazy na různá vodítka, které popisují, jak zobrazit data s ovládacím prvkem UITableView v aplikace Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: ea5b6ba532d577bd503529065eef803acf3a7aa9
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241695"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>Práce s tabulkami a buňky v Xamarin.iosu

Tato část představuje třídy používané k vytvoření a zobrazení tabulek a poskytuje příklady, jak je používat v Xamarin.iOS. Bude se vztahovat pomocí výchozí vzhled pro tabulky, přizpůsobení rozložení implementace úpravy a použití Xamarin pro iOS Designer pro vizuální návrh tabulky. Zobrazení je v některých případech samozřejmě seznam řádků (jako je například aplikace Music) a jindy je obtížné rozpoznat ovládacího prvku tabulky (např. úpravy v aplikaci kontakty a konverzace v aplikaci zprávy).

Kteří pracují na aplikace pro různé platformy s Xamarin.Android ovládací prvek UITableView je podobný třídě ListView v Androidu (a UITableViewSource třída je podobně jako na třídy adaptér pro Android).

Tyto články provádí komplexní přehled práce s tabulkami, včetně:

-   **Tabulka částí** – Introducing a s vysvětlením vizuální prvky `UITableView` ovládacího prvku. 
-   **Zobrazení dat v tabulkách** – představením toho, jak vytvořit a vyplnit tabulku, použít různé styly pro tabulky a buňky a vyhnout se problémy s pamětí recyklací objekty buňky. 
-   **Rozšířené využití** – tvorba vlastní buňky a používání funkcí pro úpravy UITableView třídy. 
-   **Vytvoření tabulky vizuálně** – k vytvoření rozhraní řízené tabulky se Storyboardem pomocí návrháře Xamarin pro iOS. 

## <a name="contents"></a>Obsah

 [Tabulka části &amp; funkce](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Naplnění tabulky daty](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Přizpůsobení vzhledu tabulky](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Úprava](~/ios/user-interface/controls/tables/editing.md)
 
 [Akce s řádky](~/ios/user-interface/controls/tables/row-action.md)

 [Vytváření tabulek ve scénáři](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [Automatické nastavování výšky řádku](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>Související odkazy

- [WorkingWithTables (ukázka)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [Tabulky ve scénáři (ukázka)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Úvod do scénářů](~/ios/user-interface/storyboards/index.md)
- [Scénáře předpisu zobrazení Tabulka](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [Úvod do MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [TableEditing ukázka na Githubu](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [TableParts ukázka na Githubu](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [TableAndCellStyles ukázka na Githubu](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [Referenční třída UITableView](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [Referenční třída UITableViewCell](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
