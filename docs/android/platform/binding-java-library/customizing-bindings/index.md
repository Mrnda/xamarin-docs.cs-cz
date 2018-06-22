---
title: Přizpůsobení vazby
description: Vazbu Xamarin.Android můžete přizpůsobit úpravou metadata, která řídí proces vytváření vazby. Tyto ruční úpravy jsou často potřebné pro řešení chyb při sestavení a pro úpravu výsledné rozhraní API, tak, aby byly víc konzistentní s C# nebo .NET. Tyto příručky popisují strukturu tato metadata, jak upravit metadata a jak JavaDoc obnovení pomocí pro názvy parametrů metody.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 09/25/2017
ms.openlocfilehash: bb4f3b24be2072cb8b33893899a23951ace63607
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763134"
---
# <a name="customizing-bindings"></a>Přizpůsobení vazby

_Vazbu Xamarin.Android můžete přizpůsobit úpravou metadata, která řídí proces vytváření vazby. Tyto ruční úpravy jsou často potřebné pro řešení chyb při sestavení a pro úpravu výsledné rozhraní API, tak, aby byly víc konzistentní s C# nebo .NET. Tyto příručky popisují strukturu tato metadata, jak upravit metadata a jak JavaDoc obnovení pomocí pro názvy parametrů metody._


## <a name="overview"></a>Přehled
 
Xamarin.Android automatizuje velkou část procesu vázání; ale v některých případech je ruční úpravy potřeba vyřešit následující problémy:

-   Řešení sestavení chyby způsobené chybějící typy, zkomolené typy, duplicitní názvy, třída viditelnost problémy a jiné situace, které nelze vyřešit pomocí nástroje Xamarin.Android. 

-   Změna mapování, která Xamarin.Android používá k vytvoření vazby rozhraní API systému Android pro různé typy v jazyku C# (například celá řada vývojářů přednost mapování Java `int` konstanty jazyka C# `enum` konstanty).

-   Odebrání nepoužívá typy, které nemusí být vázána. 

-   Přidání typů, které nemají žádné protějšku ve základního rozhraní API Java. 

Některé nebo všechny tyto změny můžete provést změnou metadata, která řídí proces vytváření vazby.


## <a name="guides"></a>Příručky

Následující příručky popisují metadata, která řídí proces vytváření vazby a popisují, jak upravit tato metadata vyřešit tyto problémy:

-   [Metadata vazby Java](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) poskytuje přehled metadata, která přejde do vazbu Java.
    Popisuje různé ruční kroky, které jsou někdy nutné k dokončení knihovnu vazby Java a vysvětluje, jak utvářejí rozhraní API vystavené vazbu ke přesněji postupujte podle pokynů pro návrh .NET.

-   [Názvy parametrů s Javadoc](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) vysvětluje, jak obnovit názvy parametrů v projektu Java vazby pomocí Javadoc vytvořeného z vázané projektu Java.


 

