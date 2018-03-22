---
title: "Cíle Sharpie"
description: "Tato část obsahuje úvod do cíle Sharpie, nástroj pro příkazový řádek pro Xamarin umožňuje automatizovat proces vytváření vazby do knihovny jazyka Objective-C"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 08796d38301ba9c6ae15c3ddfe700b1cd2166ace
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="objective-sharpie"></a>Cíle Sharpie

_Tato část obsahuje úvod do cíle Sharpie, nástroj pro příkazový řádek pro Xamarin umožňuje automatizovat proces vytváření vazby do knihovny jazyka Objective-C_

- [Přehled](#overview) & [historie](#history)
- [Začínáme](get-started.md)
- [Nástroje a příkazy](tools.md)
- [Funkce](platform/index.md)
- [Příklady](examples/index.md)
- [Kompletní a podrobný postup](~/ios/platform/binding-objective-c/walkthrough.md)
- [Historie vydaných verzí](releases.md)

## <a name="overview"></a>Přehled

Cíle Sharpie je nástroj příkazového řádku, který pomůže bootstrap první fáze vazby.
Funguje díky analýze soubory hlaviček nativní knihovny pro mapování veřejné rozhraní API do [vazby definice](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) (Tento proces, která dříve byla ručně provést).

Cíle Sharpie používá Clang analýzy hlavičkových souborů, takže vazba je jako přesné a důkladné nejblíže. To může výrazně snížit čas a úsilí, potřebný k vytvoření vazby kvality.

> [!IMPORTANT]
> Cíle Sharpie je nástroj pro zkušeného Xamarin vývojářům pokročilou znalost jazyka Objective-C (a rozšíření, C). Před pokusem o vytvořit vazbu knihovna jazyka Objective-C byste měli mít solidní znalosti, jak vytvářet nativní knihovny na příkazovém řádku (a dostatečné povědomí o tom, jak funguje nativní knihovny).

## <a name="history"></a>Historie

Jsme byly vyvíjejí a interně pomocí Sharpie cíl na Xamarin pro poslední tři roky. Jako důkaz exponentem Sharpie cíl rozhraní API byla zavedená v Xamarin.iOS a Xamarin.Mac od sady iOS 8, Mac OS X 10.10, a nebyly zcela s cílem Sharpie připravili watchOS 2.0. Xamarin velice spoléhá na cíl Sharpie interně pro vytváření vlastní produkty.

Cíl Sharpie je však velmi pokročilý nástroj, který vyžaduje pokročilé znalosti jazyka Objective-C a C, jak používat kompilátoru clang na příkazovém řádku a jsou obecně jak nativní knihovny společně umístit. Z důvodu této vysoké panelu jsme popisovač s grafickým uživatelským rozhraním průvodce nastaví chybný očekávání, a jako takový Sharpie cíl je aktuálně k dispozici pouze jako nástroj pro příkazový řádek.

## <a name="related-links"></a>Související odkazy

- [Cíle Sharpie stahování](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
- [Návod: Vytvoření vazby knihovna jazyka Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)
- [Knihovny pro vytváření vazeb Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Podrobnosti o vazby](~/cross-platform/macios/binding/overview.md)
- [Vazby typů referenční příručka](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin pro vývojáře jazyka Objective-C](~/ios/get-started/objective-c-developers/index.md)
