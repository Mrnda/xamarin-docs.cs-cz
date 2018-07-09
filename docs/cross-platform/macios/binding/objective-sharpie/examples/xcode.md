---
title: Reálný příklad použití projektu Xcode
description: Tento dokument popisuje, jak použít projekt Xcode jako přímý vstup do cíle Sharpie, zjednodušuje proces vytváření vazby na C# kód Objective-C.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 05c55dc7cd20de2d216d1f267ea5a73631748a0a
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855244"
---
# <a name="real-world-example-using-an-xcode-project"></a>Reálný příklad použití projektu Xcode

**V tomto příkladu [POP knihovny ze sítě Facebook](https://github.com/facebook/pop).**

Nové ve verzi 3.0 podporuje cíl Sharpie projekty Xcode jako vstup. Tyto projekty zadejte správné hlavičkové soubory a příznaky kompilátoru potřebné ke kompilaci nativní knihovny a proto nutné vytvořit vazbu příliš. Cíle Sharpie vybere první _cílové_ a její výchozí konfiguraci projektu, pokud není nastaven na postupovat jinak.

Před Sharpie cíle se pokusí analyzovat soubory projektu a záhlaví, se musí vytvořit. Projekty mají často fáze sestavení, které se správně struktury soubory hlaviček pro externí spotřebu a integraci, takže je vhodné vždy úplný projekt sestavit před pokusem o vytvořte jeho vazbu.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

## <a name="related-links"></a>Související odkazy

- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)