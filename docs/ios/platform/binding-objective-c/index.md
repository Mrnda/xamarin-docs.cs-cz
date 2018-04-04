---
title: Vazba iOS knihovny
description: Jak provádět iOS nativní knihovny (a CocoaPods) dostupné v aplikacích pro Xamarin.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 268900c7ab7b317b0b20f4c1ead2360fd6f9bbf0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="binding-ios-libraries"></a>Vazba iOS knihovny

_Jak provádět iOS nativní knihovny (a CocoaPods) dostupné v aplikacích pro Xamarin._

Další informace o vazba knihoven jazyka Objective-C a CocoaPods pro Xamarin.iOS a Xamarin.Mac na následujících odkazech:

- [**Přehled** ](~/cross-platform/macios/binding/overview.md) -
  popisuje, jak funguje vazby.
- [**Vazba knihoven jazyka Objective-C** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  pokyny, jak vytvořit vazbu knihovny jazyka Objective-C pro použití v projektech Xamarin.
- [**Zadejte definici referenční příručka** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  popisuje všechny atributy, které jsou k dispozici pro vazbu autorům jednotka proces vytváření vazby.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Cíle Sharpie je nástroj příkazového řádku, který pomůže bootstrap první fáze vazby.
Funguje díky analýze soubory hlaviček nativní knihovny pro mapování veřejné rozhraní API do [vazby definice](~/cross-platform/macios/binding/objective-c-libraries.md) (Tento proces, který je jinak provést ručně). Cíle Sharpie nevytvoří vazbu sám o sobě, ale může pomoct vám pomůžou začít!

Cíle Sharpie 3.0 zavedla možnost vazby Cocoapods přímo!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[Návod - vazby iOS knihovna jazyka Objective-C](walkthrough.md)

Tato stránka obsahuje podrobný postup vytváření projektu iOS vazbu pomocí open source [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) projekt jazyka Objective-C jako příklad. **InfColorPicker** knihovna obsahuje opakovaně použitelné zobrazení kontroler, který umožňuje uživateli vybrat barvu podle jeho reprezentace HSB, provedení přívětivější výběr barev.
Cíle Sharpie se použije jako pomůcku při proces vytváření vazby.

## <a name="xamarin-university-lightning-lecture"></a>Xamarin univerzity Lightning Přednáškový

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS vazby v jazyce C/C++ pomocí [univerzity Xamarin](https://university.xamarin.com/)**

## <a name="related-links"></a>Související odkazy

- [Vytváření vazeb Objective-C](~/cross-platform/macios/binding/index.md)
- [Vazba Mac](~/mac/platform/binding.md)
