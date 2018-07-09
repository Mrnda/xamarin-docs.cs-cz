---
title: Vytvoření vazby knihovny iOS
description: Tento dokument popisuje, jak vytvořit vazby C# pro kód Objective-C, což umožňuje používání nativních knihoven a CocoaPods aplikace pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 75c8f9a2eea080c3da031366b314d94929080811
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855218"
---
# <a name="binding-ios-libraries"></a>Vytvoření vazby knihovny iOS

Další informace o vytvoření vazby knihovny Objective-C a CocoaPods pro Xamarin.iOS a Xamarin.Mac na následujících odkazech:

- [**Přehled** ](~/cross-platform/macios/binding/overview.md) -
  popisuje princip vazby.
- [**Vazba knihoven jazyka Objective-C** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  pokyny o tom, jak vytvořit vazbu knihovny Objective-C pro použití v projektech pro Xamarin.
- [**Zadejte definici referenční příručka** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  popisuje všechny atributy, které jsou k dispozici pro vazbu autoři Centrum umožňující prosazovat proces vytváření vazby.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Cíle Sharpie je nástroj příkazového řádku umožňující bootstrap prvním průchodu vazbu.
Funguje díky analýze na nativní knihovnu pro mapování veřejného rozhraní API do soubory hlaviček [vazby definice](~/cross-platform/macios/binding/objective-c-libraries.md) (Tento proces, který je jinak provést ručně). Cíle Sharpie nevytvoří vazbu samostatně, ale může pomoci, vám pomůžou začít!

Cíle 3.0 Sharpie zavádí možnost vytvořit vazbu Cocoapods přímo!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[Návod – vazby knihovnou Objective-C pro iOS](walkthrough.md)

Tato stránka obsahuje podrobný postup vytvoření vazby projektu aplikace iOS pomocí open source [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) Objective-C projektu jako příklad. **InfColorPicker** knihovna obsahuje opakovaně použitelné zobrazení kontroler, který uživateli umožní vybrat barvu podle jeho reprezentace HSB provádění přívětivější výběr barvy.
Cíle Sharpie se použije při proces vytváření vazby.

## <a name="xamarin-university-lightning-lecture"></a>Přednášky bleskově Xamarin University

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS vazby v jazyce C/C++ pomocí [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Související odkazy

- [Vytváření vazeb Objective-C](~/cross-platform/macios/binding/index.md)
- [Vazba Mac](~/mac/platform/binding.md)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)