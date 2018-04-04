---
title: ApiDefinitions & StructsAndEnums soubory
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 366a50d67e114084808fbd41f5e70157e508eb4e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions & StructsAndEnums soubory

Cíl Sharpie proběhla úspěšně, vygeneruje `Binding/ApiDefinitions.cs` a `Binding/StructsAndEnums.cs` soubory.
Tyto dva soubory se přidají do projektu vazby v sadě Visual Studio pro Mac nebo přímo na předán `btouch` nebo `bmac` nástroje k vytvoření konečné vazby.

V *některé* případů tyto soubory generované může být vše, co potřebujete, ale častější vývojář bude nutné upravit tyto ručně generované soubory a opravte všechny problémy, které nelze automaticky zpracovávat nástroj (například těchto označení příznakem s [ `Verify` atribut](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Mezi další kroky patří:

- **Úprava názvů**: někdy budete chtít nastavit názvy metod a třídy tak, aby odpovídaly pokynů pro návrh rozhraní .NET Framework.
- **Metody nebo vlastnosti**: heuristiky používané cíl Sharpie někdy bude vyberte metodu pro být převedena na vlastnost. V tomto okamžiku může rozhodnout, jestli se jedná o záměrné chování nebo ne.
- **Připojení událostí**: můžete propojit váš tříd pomocí tříd delegáta a automaticky generovat události pro ty.
- **Propojte oznámení**: není možné extrahovat kontrakt API oznámení z čisté hlavičkových souborů, to bude vyžadovat výlet dokumentaci rozhraní API. Pokud chcete silného typu oznámení, musíte aktualizovat výsledek.
- **Správa funkce rozhraní API**: V této chvíli můžete poskytnout další konstruktory, přidejte metody (aby bylo možné inicializovat na vytváření syntaxe jazyka C#), přetížení operátoru a implementace vlastní rozhraní v souboru další definice.

Najdete v článku [vazby rozhraní API](~/cross-platform/macios/binding/objective-c-libraries.md) popis najdete v tom, jak tyto soubory začlenit do procesu vázání, jak je znázorněno v následujícím diagramu:

![](apidefinitions-structsandenums-images/binding-flowchart.png "Na této obrázku je znázorněn proces vytváření vazby")

Odkazovat [vazby typy odkaz](~/cross-platform/macios/binding/binding-types-reference.md) Další informace o obsah těchto souborů.

