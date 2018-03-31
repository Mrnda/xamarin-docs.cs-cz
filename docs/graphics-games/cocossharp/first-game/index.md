---
title: Úvod do vývoj her s CocosSharp
description: Tento vícedílné názorný postup ukazuje, jak vytvořit jednoduché 2D hry s použitím CocosSharp. Týká se běžných herní koncepty programování například grafiky, vstup a fyziky.
ms.topic: article
ms.prod: xamarin
ms.assetid: BCA99A61-A48D-4207-9A35-4C62F10C9AA5
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 5ab6f68aed791dd21516d663367ac5435e92d6cc
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/30/2018
---
# <a name="introduction-to-game-development-with-cocossharp"></a>Úvod do vývoj her s CocosSharp

_Tento vícedílné názorný postup ukazuje, jak vytvořit jednoduché 2D hry s použitím CocosSharp. Týká se běžných herní koncepty programování například grafiky, vstup a fyziky._

Modul 2D herní CocosSharp poskytuje technologie pro vytváření hry napříč platformami. Úplný seznam podporovaných platforem najdete v článku [CocosSharp wiki na Githubu](https://github.com/mono/CocosSharp/wiki). V tomto kurzu použije C# pro ukázky kódu, i když CocosSharp je také plně funkční a F #.

Poskytuje základní CocosSharp [MonoGame framework](http://www.monogame.net/), který je sám napříč platformami, hardwaru accelerated rozhraní API poskytuje grafiky, zvuk, herní stavu správy, vstup a obsahu kanálu pro import prostředky. CocosSharp je efektivní abstraktní vrstvu vhodný pro 2D hry. Větší hry kromě toho může provádět vlastní optimalizace mimo jejich základní knihovny se růstem složitosti hry. Jinými slovy poskytuje CocosSharp směs snadné použití a výkonu, umožňuje vývojářům rychle začít bez omezení herní velikost nebo složitost.

V první části tohoto návodu zaměřit se na nastavení prázdný projekt.  Druhá část popisuje zápis všech herní logiku. 

Na konci tohoto návodu jsme vytvořili jednoduchá hra, kde je cílem zobrazujícím Vysuňte desku vodorovně ke pokusí uchovat míč dostaly z obrazovky. Každý vrátit odesílateli zvýší skóre hráčů jeden bod.

![](images/image1.png "Každý vrátit odesílateli zvýší skóre hráčů o jeden bod.")

# <a name="walkthrough-parts"></a>Návod částí

* [Část 1 – Vytvoření CocosSharp projektu](~/graphics-games/cocossharp/first-game/part1.md)
* [Část 2 – implementace BouncingGame](~/graphics-games/cocossharp/first-game/part2.md)

## <a name="related-links"></a>Související odkazy

- [Herní obsah (ukázka)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
- [Dokončené projektu (ukázka)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [PCL CocosSharp na NuGet](http://www.nuget.org/packages/CocosSharp.PCL.Shared/)
- [Dokumentace CocosSharp rozhraní API](https://developer.xamarin.com/api/namespace/CocosSharp/)
