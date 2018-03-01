---
title: "Související dokumentace"
description: "Odkazy na další dokumentaci pro vývojáře v systému macOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0a282c58-1c37-4f73-8440-85de2daf454a
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: 000061d8ed884f1eb248517c6131ec49134cb8a1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="related-documentation"></a>Související dokumentace

Kromě části Mac [developer.xamarin.com](~/mac/get-started/index.md) existují tři skvělé zdroje dokumentace, která může být také pomoc s dotazy Xamarin.Mac:

- [**Dokumentace Xamarin.iOS** ](~/ios/get-started/index.md) – pro mnoho rozhraní API (především mimo AppKit/UIKit) jsou jen malé rozdíly mezi verzemi iOS a systému macOS. V některých případech, kdy rozhraní API pro danou iOS obsahuje název `UIFoo`, podobně jako API s názvem `NSFoo` lze najít v systému macOS. Tyto příklady bude obvykle v jazyce C# již.

- **Společnosti Apple [Mac Dev Center](https://developer.apple.com/devcenter/mac/)**  – mnohokrát, například jaké rozhraní API volat v Objective-C lze převést na C# přehledné způsobem. V tématu [rozhraní API pro rozpoznávání Mac](~/mac/app-fundamentals/mac-apis.md) podrobnosti o tom, jak to udělat.

- [**Přetečení zásobníku** ](http://stackoverflow.com/) -skvělým zdrojem pro jednoduché jeden vypnout otázky, jako ["jak automaticky rozbalte všechny uzly v NSOutlineView"](http://stackoverflow.com/questions/519751/nsoutlineview-auto-expand-all-nodes). Tyto příklady často se bude v Objective-C a potřeba převést na C#, ale je podmnožinou odpovědi v jazyce C#.

## <a name="user-interface"></a>Uživatelské rozhraní

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac vývojář má přístup ke stejné uživatelské rozhraní prvky, které vývojář práce *jazyka Objective-C* a *Xcode* nepodporuje. Vzhledem k tomu, že Xamarin.Mac integruje přímo s Xcode, vývojáři použít na Xcode _rozhraní tvůrce_ vytvořit a udržovat uživatelského rozhraní aplikace (nebo je můžete také vytvořit přímo v kódu jazyka C#).

Příručky níže uvedené poskytnout podrobné informace o práci s systému macOS prvků v aplikaci Xamarin.Mac:

- [Windows](~/mac/user-interface/window.md)
- [Dialogová okna](~/mac/user-interface/dialog.md)
- [Výstrahy](~/mac/user-interface/alert.md)
- [Nabídky](~/mac/user-interface/menu.md)
- [Panely nástrojů](~/mac/user-interface/toolbar.md)
- [Zobrazení tabulek](~/mac/user-interface/table-view.md)
- [Zobrazení osnovy](~/mac/user-interface/outline-view.md)
- [Zdroj seznamy](~/mac/user-interface/source-list.md)
