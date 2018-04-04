---
title: uživatelské rozhraní systému macOS
description: Tento článek obsahuje odkazy na pokyny, které popisují různé ovládacích prvků uživatelského rozhraní systému macOS.
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: d40faa29f2fe278377bf4eae42a032f3dc9086ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="macos-user-interface"></a>uživatelské rozhraní systému macOS

_Tento článek obsahuje odkazy na pokyny, které popisují různé ovládacích prvků uživatelského rozhraní systému macOS._

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup pro téhož uživatele rozhraní určuje, které vývojář pracující *jazyka Objective-C* a *Xcode* nepodporuje. Protože Xamarin.Mac integruje přímo s Xcode, můžete na Xcode _rozhraní tvůrce_ vytvořit a Udržovat uživatelská rozhraní (nebo je můžete také vytvořit přímo v kódu jazyka C#).

Příručky níže uvedené poskytnout podrobné informace o práci s prvky uživatelského rozhraní systému macOS v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) části, jak se popisuje klíčové koncepty a techniky, které budeme používat v každé článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky, jak vysvětluje `Register` a `Export` Atributy použité k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

Tento článek se zabývá práci s windows a panely v Xamarin.Mac aplikace. Vysvětluje vytváření a údržbu systému windows a panely v Xcode a rozhraní tvůrce, načítání windows a panelů z .storyboard nebo .xib souborů, pomocí systému windows a odpovídá na požadavky systému windows v kódu jazyka C#.

## <a name="dialogsmacuser-interfacedialogmd"></a>[Dialogy](~/mac/user-interface/dialog.md)

Tento článek se zabývá práce se dialogová okna a modální okna v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu modální systému windows v Xcode a rozhraní tvůrce, práce s standardní dialogová okna, zobrazení a odpovídá na požadavky systému windows v kódu jazyka C#.

## <a name="alertsmacuser-interfacealertmd"></a>[Upozornění](~/mac/user-interface/alert.md)

Tento článek se zabývá práce s výstrahami v aplikaci Xamarin.Mac. Vysvětluje vytváření a zobrazování výstrah z kódu jazyka C# a zpracování výstrah.

## <a name="menusmacuser-interfacemenumd"></a>[Nabídky](~/mac/user-interface/menu.md)

Nabídky se používají v různých částí uživatelského rozhraní pro aplikace systému Mac; z hlavní nabídky v horní části obrazovky místní nabídky a kontextové nabídky, které může vyskytovat kdekoli v okně. Nabídky jsou nedílnou součástí aplikace systému Mac uživatelské prostředí. Tento článek se zabývá práce s nabídkami kakao v aplikaci Xamarin.Mac.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Standardní ovládací prvky](~/mac/user-interface/standard-controls.md)

Práce s standardní ovládací prvky AppKit např. tlačítka, popisky, textových polí, zaškrtněte políčka a segmentovaným ovládací prvky v aplikaci Xamarin.Mac. Tento průvodce popisuje jejich přidáním do návrh uživatelského rozhraní v Tvůrci rozhraní na Xcode, je vystavení kódu pomocí akce a výstupy a práce s ovládacími prvky AppKit v kódu jazyka C#.

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Panely nástrojů](~/mac/user-interface/toolbar.md)

Tento článek se zabývá práci s panely nástrojů v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu panely nástrojů v Xcode a Tvůrce rozhraní, jak vystavit položky panelu nástrojů, které chcete kódu pomocí akce a výstupy, povolení a zakázání položek panelu nástrojů a nakonec neodpovídá na požadavky položek panelu nástrojů v kódu jazyka C#.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Zobrazení tabulek](~/mac/user-interface/table-view.md)

Tento článek se zabývá práci s zobrazení tabulek v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu zobrazení tabulek v Xcode a Tvůrce rozhraní, jak vystavit tabulka zobrazení položky kódu pomocí akce a výstupy, vyplní zobrazení tabulek a odpovídá na tabulku zobrazit položky v kódu jazyka C#.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Zobrazení osnovy](~/mac/user-interface/outline-view.md)

Tento článek se zabývá práci s zobrazení osnovy v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu zobrazení osnovy v Xcode a Tvůrce rozhraní, jak vystavit zobrazení osnovy položky kódu pomocí akce a výstupy, vyplní zobrazení osnovy a odpovídá na požadavky popisují zobrazit položky v kódu jazyka C#.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Zdroj seznamy](~/mac/user-interface/source-list.md)

Tento článek se zabývá práce se seznamy zdroje v Xamarin.Mac aplikace. Vysvětluje vytváření a údržbu seznamy zdroje v Xcode a Tvůrce rozhraní, jak vystavit položky seznamu zdrojového kódu pomocí akce a výstupy, naplnění seznamů zdroje a odpovídá na požadavky položky seznamu zdroje v kódu jazyka C#.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Zobrazení kolekce](~/mac/user-interface/collection-view.md)

Tento článek se zabývá práci s zobrazení kolekcí v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu zobrazení kolekcí v Xcode a Tvůrce rozhraní, jak vystavit zobrazení kolekce položek kódu pomocí akce a výstupy, vyplní zobrazení kolekcí a odpovídá na požadavky zobrazení kolekcí v kódu jazyka C#.

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[Vytváření vlastních ovládacích prvků](~/mac/user-interface/custom-controls.md)

Tento článek se zabývá vytváření vlastní uživatelské rozhraní ovládacích prvků (dědění ze `NSControl`), kreslení vlastní rozhraní pro ovládací prvek a vytváření vlastních akcí, které lze použít s Xcode na rozhraní tvůrce.

## <a name="mac-samples-gallery"></a>Galerie ukázek Mac

Doporučujeme také trvá podívejte se na [Mac ukázky Galerie](https://developer.xamarin.com/samples/mac/all/). Obsahuje širokou řadu připravené k použití kód, který vám pomůže rychle získat projektu Xamarin.Mac vypnout základů.

## <a name="related-links"></a>Související odkazy

- [systému macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
