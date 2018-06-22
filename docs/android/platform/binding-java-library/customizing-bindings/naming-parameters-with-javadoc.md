---
title: Názvy parametrů s Javadoc
description: Tento článek vysvětluje, jak obnovit názvy parametrů v vazby projektu Java pomocí Javadoc vygenerovat z projektu Java.
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/20/2017
ms.openlocfilehash: 7517e46c5b66123dc4e12fb5562c59f569f249aa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766891"
---
# <a name="naming-parameters-with-javadoc"></a>Názvy parametrů s Javadoc

_Tento článek vysvětluje, jak obnovit názvy parametrů v vazby projektu Java pomocí Javadoc vygenerovat z projektu Java._


## <a name="overview"></a>Přehled

Při vytváření vazby existující knihovnu Java, dojde ke ztrátě některých metadata o vázané rozhraní API. Na konkrétní názvy parametrů pro metody. Názvy parametrů se zobrazí jako `p0`, `p1`atd. Důvodem je, že jazyce Java `.class` soubory nezachováte názvy parametrů, které byly používány ve zdrojovém kódu Java. 

Vazba projektu Xamarin.Android Java můžete zadat názvy parametrů, pokud má přístup k Javadoc HTML z původní knihovna. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Integrace Javadoc HTML do vazby projektu Java

Integrace Javadoc HTML do projektu Java vazba je ruční proces skládající se z následujících kroků: 

1.  Stáhněte si Javadoc knihovny
2.  Upravit `.csproj` souboru a přidejte `<JavaDocPaths>` vlastnost:
3.  Vyčistěte a projekt znovu sestavte

Až bude vše Hotovo, původní názvy parametrů Java by měla existovat v rozhraní API svázaná s vazby projektu Java. 


> [!NOTE]
> Není značnou část odchylky JavaDoc výstup. Na. Sada vazby JAR nepodporuje každých jeden možné Permutace a v důsledku toho nemusí být některé parametr pojmenovaný správně.


## <a name="summary"></a>Souhrn

V tomto článku jak zahrnutých v projektu Java vazby použít Javadoc k poskytnutí názvy parametrů význam pro vázané rozhraní API. 

