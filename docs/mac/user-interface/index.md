---
title: "Uživatelské rozhraní"
description: "Tento článek obsahuje odkazy na pokyny, které popisují různé ovládacích prvků uživatelského rozhraní systému macOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: a8cb9488849dafc2cd720ecf59d654009a9ad781
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="user-interface"></a>Uživatelské rozhraní

_Tento článek obsahuje odkazy na pokyny, které popisují různé ovládacích prvků uživatelského rozhraní systému macOS._

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup ke stejné uživatelské rozhraní prvky, které vývojář práce *jazyka Objective-C* a *Xcode* nepodporuje. Protože Xamarin.Mac integruje přímo s Xcode, můžete na Xcode _rozhraní tvůrce_ vytvořit a Udržovat uživatelská rozhraní (nebo je můžete také vytvořit přímo v kódu jazyka C#). 

Příručky níže uvedené poskytnout podrobné informace o práci s prvky uživatelského rozhraní systému macOS v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) části, jak se popisuje klíčové koncepty a techniky, které budeme používat v každé článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` příkazy používá k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

Tento článek se zabývá práci s Windows a panelů v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu systému Windows a panelů v Xcode a rozhraní tvůrci načítání Windows a panelů z `.storyboard` nebo `.xib` soubory pomocí systému Windows a reagovat na systém Windows v kódu jazyka C#.

## <a name="dialogsmacuser-interfacedialogmd"></a>[Dialogy](~/mac/user-interface/dialog.md)

Tento článek se zabývá práce se dialogová okna a modální okna v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu modální systému Windows v Xcode a rozhraní tvůrci práce Standardní dialogová okna, zobrazení a odpovídá na požadavky systému Windows v kódu jazyka C#.

## <a name="alertsmacuser-interfacealertmd"></a>[Upozornění](~/mac/user-interface/alert.md)

Tento článek se zabývá práce s výstrahami v aplikaci Xamarin.Mac. Vysvětluje vytváření a zobrazování výstrah z kódu jazyka C# a zpracování výstrah.

## <a name="menusmacuser-interfacemenumd"></a>[Nabídky](~/mac/user-interface/menu.md)

Nabídky se používají v různých částí uživatelského rozhraní pro aplikace systému Mac; z hlavní nabídky v horní části obrazovky, automaticky otevírané okno a kontextové nabídky, které může vyskytovat kdekoli v okně. Nabídky jsou nedílnou součástí aplikace systému Mac uživatelské prostředí. Tento článek se zabývá práce s nabídkami kakao v aplikaci Xamarin.Mac.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Standardní ovládací prvky](~/mac/user-interface/standard-controls.md)

Práce se standardní AppKit ovládacími prvky, jako je například tlačítek, popisky, textových polí, zaškrtněte políčka a Segmentovaným ovládacích prvků v aplikaci Xamarin.Mac. Tento průvodce popisuje jejich přidáním do návrh uživatelského rozhraní v Tvůrci rozhraní na Xcode, je vystavení kódu pomocí akce a výstupy a práce s ovládacími prvky AppKit v kódu jazyka C#.

 
## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Panely nástrojů](~/mac/user-interface/toolbar.md)

Tento článek se zabývá práci s panely nástrojů v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu panely nástrojů v Tvůrci Xcode a rozhraní, jak vystavit položky panelu nástrojů, které chcete kódu pomocí akce a výstupy, povolování a zakazování položky panelu nástrojů a nakonec neodpovídá na požadavky položky panelu nástrojů v kódu jazyka C#.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Zobrazení tabulek](~/mac/user-interface/table-view.md)

Tento článek se zabývá práci s zobrazení tabulek v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu zobrazení tabulek v Tvůrci Xcode a rozhraní, jak vystavit zobrazit položky, které mají kód pomocí akce a výstupy, vyplní zobrazení tabulek a nakonec zpracování tabulky zobrazit položky v kódu jazyka C#.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Zobrazení osnov](~/mac/user-interface/outline-view.md)

Tento článek se zabývá práci s zobrazení osnovy v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu zobrazení osnovy v Tvůrci Xcode a rozhraní, jak vystavit zobrazit položky osnova kódu pomocí akce a výstupy, vyplní Outline položky a nakonec neodpovídá na požadavky Outline zobrazit položky v kódu jazyka C#.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Seznamy zdrojů](~/mac/user-interface/source-list.md)

Tento článek se zabývá práce se uvádí zdroje v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu uvádí zdroje v Tvůrci Xcode a rozhraní, jak vystavit položky uvádí zdroje kódu pomocí akce a výstupy, vyplní položky seznamu zdroje a nakonec neodpovídá na požadavky položky seznamu zdroje v kódu jazyka C#.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Zobrazení kolekcí](~/mac/user-interface/collection-view.md)

Tento článek se zabývá práci s zobrazení kolekcí v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu zobrazení kolekcí v Tvůrci Xcode a rozhraní, jak vystavit zobrazení kolekce elementy kódu pomocí akce a výstupy, vyplní zobrazení kolekcí a nakonec neodpovídá na požadavky zobrazení kolekcí v kódu jazyka C#.

## <a name="creating-custom-user-controlsmacuser-interfacecustom-controlsmd"></a>[Vytváření vlastní uživatelské ovládací prvky](~/mac/user-interface/custom-controls.md)

Tento článek se zabývá vytváření vlastních ovládacích prvků uživatelského rozhraní (dědění ze `NSControl`), kreslení vlastní rozhraní pro ovládací prvek a vytvoření vlastní akce, které lze použít s Xcode na rozhraní tvůrce.

## <a name="mac-samples-gallery"></a>Galerie ukázek Mac

Doporučujeme také trvá podívejte se na [Mac ukázky Galerie](http://developer.xamarin.com/samples/mac/all/), obsahuje širokou řadu připravené k použití kód, který vám pomůže rychle získat projektu Xamarin.Mac vypnout základů.

## <a name="related-links"></a>Související odkazy

- [systému macOS Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
