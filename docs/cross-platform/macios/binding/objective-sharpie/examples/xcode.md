---
title: Příklad reálného pomocí projektu Xcode
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 4e30b07c10de439df8e1ec1706845150e8c54a41
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
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

