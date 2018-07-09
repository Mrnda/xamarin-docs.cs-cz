---
title: Vytváření vazeb s Sharpie cíle
description: Tato část obsahuje úvod do cíle Sharpie, nástroje příkazového řádku pro Xamarin umožňuje automatizovat proces vytváření vazby ke knihovnou Objective-C
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 53fcbbc408ae147405a3285d9391457051d6e16e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854790"
---
# <a name="creating-bindings-with-objective-sharpie"></a>Vytváření vazeb s Sharpie cíle

_Tato část obsahuje úvod do cíle Sharpie, nástroje příkazového řádku pro Xamarin umožňuje automatizovat proces vytváření vazby ke knihovnou Objective-C_

- [Přehled](#overview) & [historie](#history)
- [Začínáme](get-started.md)
- [Nástroje a příkazy](tools.md)
- [Funkce](platform/index.md)
- [Příklady](examples/index.md)
- [Kompletní postup](~/ios/platform/binding-objective-c/walkthrough.md)
- [Historie vydaných verzí](releases.md)

## <a name="overview"></a>Přehled

Cíle Sharpie je nástroj příkazového řádku umožňující bootstrap prvním průchodu vazbu.
Funguje díky analýze na nativní knihovnu pro mapování veřejného rozhraní API do soubory hlaviček [vazby definice](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) (procesu, která dříve byla ručně provést).

Cíle Sharpie používá Clang analýzy hlavičkové soubory, tedy jako přesné a co nejdůkladnější vazby. To může výrazně snížit čas a úsilí, potřebný k vytvoření vazby kvality.

> [!IMPORTANT]
> Cíle Sharpie je nástroj pro zkušené vývojáře Xamarin pomocí pokročilé znalosti jazyka Objective-C (a při rozšíření i pro C). Než se pokusíte vytvořit vazbu knihovnou Objective-C byste měli mít plnou znalosti o tom, a začít vytvářet nativní knihovnu pro příkazový řádek (a dobrý přehled o fungování nativní knihovny).

## <a name="history"></a>Historie

Jsme se vyvíjejí a interně pomocí Sharpie cíle v Xamarinu pro poslední tři roky. Jako důkaz k elektrické energie Sharpie cíle rozhraní API zavedené v Xamarin.iOS a Xamarin.Mac od iOS 8, Mac OS X 10.10, a zcela s cílem Sharpie byly zavedeny watchOS 2.0. Xamarin spoléhá na cíl Sharpie interně pro vytváření vlastní produkty.

Cíl Sharpie je však velmi pokročilý nástroj, který vyžaduje pokročilé znalosti jazyka Objective-C a C, jak používat clang kompilátoru na příkazovém řádku a obecně jak nativních knihoven jsou umístěny společně. Z důvodu tohoto panelu vysokou jsme popisovač s grafickým uživatelským rozhraním průvodce nastaví chybný očekávání, a v důsledku toho Sharpie cíl je momentálně dostupný jenom jako nástroj příkazového řádku.

## <a name="related-links"></a>Související odkazy

- [Cíle Sharpie stahování](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
- [Návod: Vytvoření vazby knihovnou Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)
- [Knihovny pro vytváření vazeb Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Podrobnosti o vazbě](~/cross-platform/macios/binding/overview.md)
- [Vazby typů referenční příručka](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin pro vývojáře v Objective-C](~/ios/get-started/objective-c-developers/index.md)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
