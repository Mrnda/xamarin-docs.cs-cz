---
title: CocosSharp
description: Tento dokument obsahuje odkazy na různé články o vývoj her s CocosSharp.
ms.prod: xamarin
ms.assetid: 5E72869D-3541-408B-AB64-D34C777AFB79
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: 2772af61dfc60b7ecd0fd5f7ecdfe5701d2504f0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="cocossharp"></a>CocosSharp

_CocosSharp je knihovna pro vytváření 2D hry s použitím jazyka C# a F #. Je port .NET oblíbených Cocos2D stroje._

## <a name="introduction-to-cocossharp"></a>Úvod do CocosSharp

Modul 2D herní CocosSharp poskytuje technologie pro vytváření hry napříč platformami. Úplný seznam podporovaných platforem najdete v článku [CocosSharp wiki na Githubu](https://github.com/mono/CocosSharp/wiki).
Tyto příručky pomocí jazyka C# pro ukázky kódu, i když CocosSharp je také plně funkční a F #.

Poskytuje základní CocosSharp [MonoGame framework](http://www.monogame.net/), který je sám napříč platformami, hardwaru accelerated rozhraní API poskytuje grafiky, zvuk, herní stavu správy, vstup a obsahu kanálu pro import prostředky.
CocosSharp je efektivní abstraktní vrstvu vhodný pro 2D hry.
Větší hry kromě toho může provádět vlastní optimalizace mimo jejich základní knihovny se růstem složitosti hry. Jinými slovy poskytuje CocosSharp směs snadné použití a výkonu, umožňuje vývojářům rychle začít bez omezení herní velikost nebo složitost.

Toto praktické video ukazuje, jak vytvořit jednoduché CocosSharp napříč platformami herní:

> [!Video https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Developing-Cross-platform-2D-Games-in-C-and-CocosSharp/player]

## <a name="bouncinggamegraphics-gamescocossharpbouncing-gamemd"></a>[BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)

![BouncingGame](images/bouncing-game.png "BouncingGame")

Tato příručka popisuje BouncingGame, včetně toho, jak pracovat s herní obsah, různé vizuální prvky sloužící k vytvoření hry, přidání logiky herní a další.

## <a name="fruity-falls-gamegraphics-gamescocossharpfruity-fallsmd"></a>[Herní ovocný spadá](~/graphics-games/cocossharp/fruity-falls.md)

![Snímek obrazovky her ovocný spadá](images/fruity-falls.png "ovocný spadá her – snímek obrazovky")

Tato příručka popisuje hra Fruity spadá pokrývajících běžné CocosSharp a vývoj her koncepty například fyziky, správy obsahu, herní stavu a herní návrhu.  

## <a name="coin-time-gamegraphics-gamescocossharpcointimemd"></a>[Herní mince čas](~/graphics-games/cocossharp/cointime.md)

![Mince herní snímek čas](images/cointime.png "mince čas herní – snímek obrazovky")

Čas mince je úplné platformer herní pro iOS a Android. Cílem hry je pak oslovit dvířek ukončení a snižuje nepřátel a překážek a shromažďovat všechny mincí v úrovní.

## <a name="drawing-geometry-with-ccdrawnodegraphics-gamescocossharpccdrawnodemd"></a>[Kreslení geometrie pomocí CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md)

![Tvary s CCDrawNode](images/ccdrawnode.png "vykreslené s CCDrawNode tvarů")

CCDrawNode poskytuje metody pro kreslení primitivní objekty, například řádky, kružnice a trojúhelníčky.

## <a name="animating-with-ccactiongraphics-gamescocossharpccactionmd"></a>[Animace v CCAction](~/graphics-games/cocossharp/ccaction.md)

![Animace CCAction](images/ccaction.png "A CCAction animace")

`CCAction` je základní třída, která slouží k animace CocosSharp objektů. Tento průvodce obsahuje integrovanou `CCAction` implementace pro běžné úkoly jako umístění, škálování a otáčení. Vypadá také o tom, jak vytvořit vlastní implementace, která dědí z `CCAction`.

## <a name="using-tiled-with-cocossharpgraphics-gamescocossharptiledmd"></a>[Používání Tiled v CocosSharpu](~/graphics-games/cocossharp/tiled.md)

![Úroveň ve hře](images/tiled.png "úroveň ve hře")

Na dlaždicích je efektivní, flexibilní a mapování vyspělá aplikací pro vytvoření dlaždice ortogonální a Izometrické pro hry. CocosSharp poskytuje integraci pro vedle sebe na nativní formát souborů.

## <a name="entities-in-cocossharpgraphics-gamescocossharpentitiesmd"></a>[Entity v CocosSharpu](~/graphics-games/cocossharp/entities.md)

![Kosmické lodě z hru](images/entities.png "kosmické lodě z hry")

Vzor entity je efektivní způsob, jak uspořádat herní kódu. Ho zlepšuje čitelnost, usnadňuje kódu, které chcete zachovat a využívá funkce integrované nadřazený podřízený.

## <a name="handling-multiple-resolutions-in-cocossharpgraphics-gamescocossharpresolutionsmd"></a>[Zpracování více řešení v CocosSharp](~/graphics-games/cocossharp/resolutions.md)

![Mřížka představující rozlišení obrazovky](images/resolutions.png "mřížka představující rozlišení obrazovky")

Tato příručka ukazuje, jak pracovat s CocosSharp pro vývoj her, které zobrazí správně v zařízeních různých řešení.

## <a name="cocossharp-content-pipelinegraphics-gamescocossharpcontent-pipelineindexmd"></a>[Kanál obsahu CocosSharpu](~/graphics-games/cocossharp/content-pipeline/index.md)

![XNB](images/content-pipeline.png "XNB")

Obsahu kanály se často používají v vývoj her pro optimalizaci obsahu a naformátujte ho tak, aby jej můžete načíst na určitém hardwaru nebo s určitým vývoj her pro rozhraní.

## <a name="improving-frame-rate-with-ccspritesheetgraphics-gamescocossharpccspritesheetmd"></a>[Vylepšení obnovovací frekvence s CCSpriteSheet](~/graphics-games/cocossharp/ccspritesheet.md)

![Strom z CCSpriteSheet](images/ccspritesheet.png "stromu z CCSpriteSheet")

`CCSpriteSheet` poskytuje funkce pro kombinování a používání mnoho souborů bitové kopie v jedné texture. Snižuje počet texture zlepšit časů načítání herní a kmitočet snímků.

## <a name="texture-caching-using-cctexturecachegraphics-gamescocossharptexture-cachemd"></a>[Texture CCTextureCache pomocí ukládání do mezipaměti.](~/graphics-games/cocossharp/texture-cache.md)

![Reprezentace jak CocosSharp ukládá do mezipaměti bitové kopie](images/texture-cache.png "reprezentace jak CocosSharp ukládá do mezipaměti bitové kopie")

Na CocosSharp `CCTextureCache` třída poskytuje standardní způsob, jak uspořádat do mezipaměti a uvolnit obsah. 

## <a name="2d-math-with-cocossharpgraphics-gamescocossharpmathmd"></a>[2D matematické s CocosSharp](~/graphics-games/cocossharp/math.md)

![Obrázek se otáčet](images/math.png "bitovou kopii se otočen")

Tento průvodce pokrývá 2D Matematika pro vývoj her. Pomocí CocosSharp ukazují, jak provádět běžné úkoly vývoj her a vysvětluje výpočty za tyto úlohy.

## <a name="performance-and-visual-effects-with-ccrendertexturegraphics-gamescocossharpccrendertexturemd"></a>[Výkon a vizuální efekty s CCRenderTexture](~/graphics-games/cocossharp/ccrendertexture.md)

![Pohyblivý symbol z hru](images/ccrendertexture.png "pohyblivý symbol z hry")

`CCRenderTexture` Třída poskytuje funkce pro vykreslování více objektů CocosSharp do jednoho texture. Po vytvoření, `CCRenderTexture` instance lze použít k vykreslení grafiky efektivně a implementovat vizuálních efektů.
