---
title: Vazeb Objective-C
description: Tento dokument obsahuje odkazy na různá vodítka, které popisují, jak vytvořit vazby C# pro kód Objective-C, umožňuje vývojářům využívat předem připravená knihovny v aplikacích Xamarin.
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 3f1e1ce324e849c0c939d936eb6ee1470cf24a3b
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855153"
---
# <a name="binding-objective-c"></a>Vazeb Objective-C

Tato část obsahuje celou řadu dokumenty, které zahrnují vytváření vazeb Objective-C knihoven, takže může být volána z jazyka C# aplikace vytvořené pro Xamarin.iOS a Xamarin.Mac.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Přehled](~/cross-platform/macios/binding/overview.md)

Tento dokument obsahuje některé z interních dat jak provede vazbu. Jedná o pokročilé dokument určité technické informace.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Knihovny pro vytváření vazeb Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)

Tento dokument popisuje postup použitý k vytvoření vazby C# rozhraní API jazyka Objective-C a idiomy v jazyce Objective-C zpřístupněných idiomy použité v rozhraní .NET.
Pokud se vazba pouze rozhraní API jazyka C, používejte standardní mechanismus .NET v tomto případě rozhraní P/Invoke.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Vazba definice referenční příručka](~/cross-platform/macios/binding/binding-types-reference.md)

Toto je referenční příručku, která popisuje všechny atributy, které jsou k dispozici pro vazbu autoři Centrum umožňující prosazovat proces vytváření vazby.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Cíle Sharpie je nástroj příkazového řádku umožňující bootstrap prvním průchodu vazbu. Funguje díky analýze na nativní knihovnu pro mapování veřejného rozhraní API do soubory hlaviček [vazby definice](~/cross-platform/macios/binding/objective-c-libraries.md) (Tento proces, který je možné provést ručně).

## <a name="ios"></a>iOS

[Stránky vazby iOS](~/ios/platform/binding-objective-c/index.md) odkazuje zpět na tyto běžné prostředky vazby, kromě následující příklady.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[Návod: Vytvoření vazby knihovnou Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)

Tento článek obsahuje podrobný postup vytvoření vazby projektu pomocí open source [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective-C projektu jako příklad. Knihovna InfColorPicker obsahuje opakovaně použitelné zobrazení kontroler, který uživateli umožní vybrat barvu podle jeho reprezentace HSB provádění přívětivější výběr barvy. Cíle Sharpie se použije při proces vytváření vazby.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Ukázky vazby](https://github.com/mono/monotouch-bindings)

Kolekce vazby třetích stran, které lze použít odkaz při vytváření nové projekty vazby.

## <a name="mac"></a>Mac

V minulosti [Mac vazby](~/mac/platform/binding.md) bylo velmi ruční proces. Aktuálně nejsou k dispozici [ke stažení náhledu](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) Mac vazby projektu podpory pro budoucí verze sady Visual Studio pro Mac.



## <a name="related-links"></a>Související odkazy

- [iOS vazby](~/ios/platform/binding-objective-c/index.md)
- [Vazba Mac](~/mac/platform/binding.md)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
