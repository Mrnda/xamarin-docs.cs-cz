---
title: Přehled integrace Java
description: Ekosystému Java obsahuje kolekci různých a enormním způsobem součástí. Mnoho z těchto součástí lze zkrátit čas potřebný k vyvíjet aplikace pro Android. Tento dokument se zavést a poskytují souhrnné informace o některých způsobech, vývojáři mohou použít tyto existující součásti Java k lepší využití vývoj aplikace Xamarin.Android.
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/18/2017
ms.openlocfilehash: dbaf17479ae077fced425df5ac31bdbbc4e06b64
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766943"
---
# <a name="java-integration-overview"></a>Přehled integrace Java

_Ekosystému Java obsahuje kolekci různých a enormním způsobem součástí. Mnoho z těchto součástí lze zkrátit čas potřebný k vyvíjet aplikace pro Android. Tento dokument se zavést a poskytují souhrnné informace o některých způsobech, vývojáři mohou použít tyto existující součásti Java k lepší využití vývoj aplikace Xamarin.Android._


## <a name="overview"></a>Přehled

Zadaný rozsah ekosystému Java, je velmi pravděpodobné, že byla v jazyce Java již programového žádné dané funkce vyžadované pro aplikace pro Xamarin.Android. Z toho důvodu je přitažlivé a zkuste to znovu použít tyto existující knihovny při vytváření aplikace pro Xamarin.Android. 

Existují tři možné způsoby, jak znovu použít knihovny Java v aplikaci Xamarin.Android: 

-   **Vytvoření vazby knihovny Java** &ndash; s touto technikou projektu Xamarin.Android slouží k vytváření obálek C# kolem typy jazyka Java. Aplikace pro Xamarin.Android můžete pak odkazovat vytvořené tento projekt obálky C# a pak použít `.jar` souboru. 

-   **Nativní rozhraní Java** &ndash; *Java nativní* *rozhraní* (JNI) je rozhraní, které umožňuje volání nebo kódem Java běžících v rámci JVM kód bez Java (například C++ nebo C#) . 

-   **Port kód** &ndash; tato metoda zahrnuje trvá Java zdrojový kód a jeho následnou konverzí na C#. Tento krok můžete provést ručně, nebo pomocí automatizovaného nástroje například Zostřit. 

Jádrem první dva postupy je *Java nativní rozhraní* (JNI). JNI je rozhraní, které umožňuje aplikacím není napsanou v jazyce Java pracovat s Java kód spuštěný ve virtuálním počítači Java. Xamarin.Android používá k vytvoření JNI *vazby* pro kód C#. 

První způsob je víc automatizovaný, deklarativní přístup s vazbou Java knihovny. Zahrnuje použití buď Visual Studio pro Mac nebo typu projektu sady Visual Studio, které poskytuje Xamarin.Android &ndash; vazby knihovna Java. K úspěšnému vytvoření těchto vazeb, knihovnu vazby Java může stále vyžadují že ruční úpravy, ale ne tolik, jako by čistý JNI přístup. V tématu [vazby knihovna Java](~/android/platform/binding-java-library/index.md) Další informace o knihovnách Java vazby. 

Druhý způsob spočívá pomocí JNI, funguje na mnohem nižší úrovni, ale můžete přinášejí jemnějšího ovládání a přístup k Java metody, které obvykle nebude dostupný přes knihovnu vazby Java. 

Třetí technika je výrazně liší od předchozí dva: portování kódu z prostředí Java do jazyka C#. Portování kódu z jednoho jazyka do druhého, může být velmi pracné proces, ale je možné snížit, že volá úsilí pomocí nástroje *Zostřit*. Zostřit je nástroj s otevřeným zdrojem, který je Java-na-C# převaděč. 



## <a name="summary"></a>Souhrn

Tento dokument poskytuje přehled některých různými způsoby, že lze opětovně použít knihovny z prostředí Java v aplikaci Xamarin.Android. Představil koncepty vazby a spravované obálky s možností a popsané možnosti pro přenos Java kódu jazyka C#. 


## <a name="related-links"></a>Související odkazy

- [Architektura](~/android/internals/architecture.md)
- [Vytvoření vazby knihovny Java](~/android/platform/binding-java-library/index.md)
- [Práce s JNI](~/android/platform/java-integration/working-with-jni.md)
- [Sharpen](https://github.com/slluis/sharpen)
- [Nativní rozhraní Java](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
