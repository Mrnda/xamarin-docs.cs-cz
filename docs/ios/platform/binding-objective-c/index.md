---
title: Vazba iOS knihovny
description: "Jak provádět iOS nativní knihovny (a CocoaPods) dostupné v aplikacích pro Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3afe1a03299e600502d49b1db039af4c6642e131
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
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



## <a name="related-links"></a>Související odkazy

- [Vytváření vazeb Objective-C](~/cross-platform/macios/binding/index.md)
- [Vazba Mac](~/mac/platform/binding.md)
