---
title: "Úvod do vývoj her s MonoGame"
description: "Tento vícedílné názorný postup ukazuje, jak vytvořit jednoduchou aplikaci 2D pomocí MonoGame.  Pokrývá běžné herní koncepty programování, jako je například grafiky, vstup her entity a fyziky."
ms.topic: article
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 8161920139413cd1b28adcebaf56e6d8265120ef
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-game-development-with-monogame"></a>Úvod do vývoj her s MonoGame

_Tento vícedílné názorný postup ukazuje, jak vytvořit jednoduchou aplikaci 2D pomocí MonoGame.  Pokrývá běžné herní koncepty programování, jako je například grafiky, vstup her entity a fyziky._

Tento článek popisuje technologie MonoGame rozhraní API pro vytvoření hry napříč platformami. Úplný seznam platforem, najdete v článku [MonoGame webu](http://www.monogame.net/). V tomto kurzu použije C# pro ukázky kódu, i když MonoGame je také plně funkční a F #.

MonoGame je napříč platformami, hardwaru accelerated rozhraní API poskytuje grafiky, zvuk, herní stavu správy, vstup a obsahu kanálu pro import prostředky. Na rozdíl od většiny herní moduly MonoGame neposkytuje ani žádné vzor nebo produktu project strukturu.  To znamená, že vývojáři jsou k uspořádání svůj kód jako jejich jako volné, také to znamená, že kousek instalační kód je potřeba při prvním spuštění nového projektu.

V první části tohoto návodu se zaměřuje na nastavení prázdný projekt. Poslední část popisuje zápis všechny naše herní logiku a obsahu – většina z nich bude pro různé platformy.

Na konci tohoto průvodce jsme vytvořili jednoduchá hra, kde přehrávač můžete řídit znakem animovaný s dotykovým ovládáním.  I když to není technicky úplné herní (protože má ne win nebo ztratit podmínky), ukazuje množství vývoj her koncepty a slouží jako základ pro mnoho typů her. 

Na obrázku je výsledek Tento názorný postup:

![](images/image1.gif "Aplikace, které budou vytvořeny v návodu")

# <a name="monogame-and-xna"></a>Monogame a XNA

Knihovna MonoGame slouží tak, aby napodoboval knihovna XNA společnosti Microsoft v syntaxi a funkce.  Všechny objekty MonoGame existovat pod oborem názvů Microsoft.Xna – povolení většinu XNA kódu pro použití v MonoGame bez úprav. 

Vývojáři, kteří znají XNA bude již obeznámeni s MonoGame pro syntaxi a vývojáři hledá další informace o práci s MonoGame budou moci odkazovat na existující online návody XNA, dokumentaci k rozhraní API a diskuzí.


# <a name="walkthrough-parts"></a>Návod částí

- [Část 1 – Vytvoření projektu MonoGame křížové platformy](~/graphics-games/monogame/introduction/part1.md)
- [Část 2 – implementace WalkingGame](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>Související odkazy

- [WalkingGame MonoGame projektu (ukázka)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
- [IOS XNB písem](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [Android XNB písem](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [MonoGame Android na NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [IOS MonoGame na NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [Dokumentace MonoGame rozhraní API](http://www.monogame.net/documentation/?page=main)
