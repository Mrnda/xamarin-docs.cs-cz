---
title: Příklad reálného pomocí projektu Xcode
description: Tento dokument popisuje, jak používat projektu Xcode jako přímý vstup pro cíl Sharpie, zjednodušuje proces vytváření vazby C# na kód jazyka Objective-C.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 850c351f91c9a09a6654c876167e035f751e9504
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781114"
---
# <a name="real-world-example-using-an-xcode-project"></a>Příklad reálného pomocí projektu Xcode


**Tento příklad používá [POP knihovny ze sítě Facebook](https://github.com/facebook/pop).**

Nové ve verzi 3.0 podporuje cíl Sharpie projekty Xcode jako vstup. Tyto projekty zadejte správné hlavičkových souborů a kompilátoru příznaky nezbytné pro kompilaci nativní knihovny a proto potřeba vytvořit vazbu příliš. Cíle Sharpie vybere první _cíl_ a jeho výchozí konfigurace projektu není pokyn neurčí jinak.

Před Sharpie cíl se pokusí analyzovat soubory projektu a záhlaví, se musí vytvořit. Projekty mají často fáze sestavení, které bude správně struktury hlavičkových souborů pro externí spotřebu a integraci, takže je vhodné vždy úplný projekt sestavíte před pokusem o navázat jej.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

