---
title: Vazba Objective-C
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: fef2826f536042dc9be830a4c0dc358658c359d9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="binding-objective-c"></a>Vazba Objective-C

Tato část obsahuje celou řadu dokumenty, které se týkají vytváření vazby na knihovny jazyka Objective-C, aby bylo možné volat z jazyka C# aplikace vytvořené s Xamarin.iOS nebo Xamarin.Mac.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Přehled](~/cross-platform/macios/binding/overview.md)

Tento dokument obsahuje některé z interních dat jak probíhá vazbu. Je dokument pokročilé určité technické informace.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Knihovny pro vytváření vazeb Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)

Tento dokument popisuje proces použít k vytvoření vazby C# jazyka Objective-C rozhraní API a jak jsou idioms v Objective-C mapované na idioms použít v rozhraní .NET.
Pokud vytváříte vazbu jenom rozhraní API jazyka C, měli byste použít standardní .NET mechanismus pro toto rozhraní P/Invoke.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Vazba definice referenční příručka](~/cross-platform/macios/binding/binding-types-reference.md)

Toto je referenční příručka, který popisuje všechny atributy, které jsou k dispozici pro vazbu autorům jednotka proces vytváření vazby.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Cíle Sharpie je nástroj příkazového řádku, který pomůže bootstrap první fáze vazby. Funguje díky analýze soubory hlaviček nativní knihovny pro mapování veřejné rozhraní API do [vazby definice](~/cross-platform/macios/binding/objective-c-libraries.md) (Tento proces, můžete to taky udělat ručně).

## <a name="ios"></a>iOS

[IOS vazby stránky](~/ios/platform/binding-objective-c/index.md) odkazy Zpět na tyto prostředky běžné vazby kromě do níže uvedených příkladech.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[Návod: Vytvoření vazby knihovna jazyka Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)

Tento článek obsahuje podrobný postup pro vytvoření vazby projektu pomocí open source [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) projekt jazyka Objective-C jako příklad. Knihovna InfColorPicker obsahuje opakovaně použitelné zobrazení kontroler, který umožňuje uživateli vybrat barvu podle jeho reprezentace HSB, provedení přívětivější výběr barev. Cíle Sharpie se použije jako pomůcku při proces vytváření vazby.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Ukázky vazby](https://github.com/mono/monotouch-bindings)

Kolekce vazby třetích stran, které se dají použít odkaz při vytváření nových projektů vazby.

## <a name="mac"></a>Mac

V minulosti [Mac vazby](~/mac/platform/binding.md) byl velmi ruční proces. V současné době není [ke stažení preview](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) Mac vazby projektu podpory pro budoucí verze sady Visual Studio for Mac.



## <a name="related-links"></a>Související odkazy

- [iOS vazby](~/ios/platform/binding-objective-c/index.md)
- [Vazba Mac](~/mac/platform/binding.md)
