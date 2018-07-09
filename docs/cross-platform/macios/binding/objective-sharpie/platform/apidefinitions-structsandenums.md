---
title: ApiDefinitions & StructsAndEnums soubory
description: Tento dokument popisuje ApiDefinitions.cs a StructsAndEnums.cs soubory, které generuje Sharpie cíle. Tyto soubory jsou pak použita pro přístup kód Objective-C z jazyka C#.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: df8d4508db14116a5b36e893f161ac891d58dc46
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855179"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions & StructsAndEnums soubory

Cíl Sharpie proběhla úspěšně, generuje `Binding/ApiDefinitions.cs` a `Binding/StructsAndEnums.cs` soubory.
Tyto dva soubory jsou přidány do projektu vazby v sadě Visual Studio pro Mac nebo předána přímo `btouch` nebo `bmac` nástroje k vytvoření konečného vazby.

V *některé* případy tyto vygenerované soubory mohou být všechno, co potřebujete, ale častější vývojář bude muset ručně upravit tyto generovaných souborů Chcete-li vyřešit potíže, které nelze zpracovat automaticky pomocí nástroje (například tyto označené příznakem s [ `Verify` atribut](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Další kroky patří:

- **Úprava názvů**: někdy budete chtít upravit názvy metod a tříd tak, aby odpovídaly pokyny k návrhu architektury .NET.
- **Metody nebo vlastnosti**: heuristické metody používané cíle Sharpie někdy vybere metodu převeden na vlastnost. V tomto okamžiku může rozhodnout, jestli to je zamýšlené chování nebo ne.
- **Připojení událostí**: můžete propojit tříd pomocí tříd delegáta a automaticky generovat události pro ty.
- **Připojení oznámení**: není možné extrahovat kontraktem rozhraní API oznámení ze souborů hlaviček pure, bude nutné postoupí do dokumentace k rozhraní API. Pokud chcete oznámení silného typu, je potřeba aktualizovat výsledek.
- **Kurátorování rozhraní API**: V tuto chvíli můžete poskytnout dodatečné konstruktory, přidejte metody (aby bylo možné inicializovat na konstrukce syntaxi jazyka C#), přetížení operátoru a implementovat vlastní rozhraní v souboru další definice.

Najdete v článku [vazba rozhraní API](~/cross-platform/macios/binding/objective-c-libraries.md) popis naleznete v tématu jak tyto soubory umístit do proces vytváření vazby, jak je znázorněno v následujícím diagramu:

![](apidefinitions-structsandenums-images/binding-flowchart.png "Proces vytváření vazby se zobrazí v tomto diagramu")

Odkazovat [vázaném odkazu typy](~/cross-platform/macios/binding/binding-types-reference.md) Další informace o obsahu těchto souborů.

## <a name="related-links"></a>Související odkazy

- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
