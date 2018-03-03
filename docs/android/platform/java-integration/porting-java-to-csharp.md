---
title: "Portování Java jazyka C#"
description: "Třetí možnost pro používání Javy v aplikace pro Xamarin.Android je port zdrojový kód Java a C#."
ms.topic: article
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/05/2016
ms.openlocfilehash: bf881861eec0b28e59704253c3dab3f4e5dbae46
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="porting-java-to-c"></a>Portování Java jazyka C#

_Třetí možnost pro používání Javy v aplikace pro Xamarin.Android je port zdrojový kód Java a C#._

## <a name="overview"></a>Přehled

Tento přístup může být zájmu organizacím, které:

-  **Jsou přepínání technologie zásobníky z prostředí Java jazyka C#.**
-  **Musí zachovat C# a verzi Javy stejného produktu.**
-  **Chcete mít rozhraní .NET verze Oblíbené knihovny Java.**


Existují dva způsoby k portu Java kódu jazyka C#. První způsob je port kód ručně. To zahrnuje zkušený vývojáře, kteří pochopit .NET a Java a jsou obeznámeni s správné idioms pro jednotlivé jazyky. Tento přístup je nejvhodnější pro malé množství kódu, nebo pro organizace, které chcete úplně přesunout mimo Java do jazyka C#.

Druhý přenosem metody je zkuste a automatizovat proces pomocí převaděče kódu, například [Zostřit](https://github.com/mono/sharpen). [Zostřit](https://github.com/mono/sharpen) je otevřeným zdrojem převaděč z Versant, který se původně používal k portu kód pro *db4o* z prostředí Java jazyka C#. db4o je databáze objektově orientované Versant vyvinuté v jazyce Java a pak přesně do rozhraní .NET. Pomocí převaděče kód může být užitečné pro projekty, které musí existovat v obou jazycích a které vyžadují některé rozdíly mezi těmito dvěma.

Příklad při smysl automatizované kód převodní nástroj si můžete prohlédnout ve [ngit](https://github.com/mono/ngit) projektu.
Ngit je port projektu Java [jgit](http://eclipse.org/).
Jgit samotné je implementace Java [Git](http://git-scm.com/) zdrojového kódu systému správy. Pro generování kódu jazyka C# z prostředí Java, programátory ngit pomocí vlastní automatizované systému extrahovat kódu v jazyce Java z jgit, používat některé opravy pro přizpůsobení procesu převodu a potom spusťte Zostřit, který generuje kód C#. To umožňuje využívat průběžné, probíhající práce, která se provádí na jgit ngit projekt.

Je často netriviální množství práce spojené s zavedení spouštěcího programu nástroj pro převod automatizované kód, a to může být potřeba bariéry používat. V mnoha případech je jednodušší a usnadňují port Java jazyka C# ručně.



## <a name="related-links"></a>Související odkazy

- [Zostřit převodní nástroj](https://github.com/mono/sharpen)
