---
title: Ve Skiasharpu čáry a cesty
description: Tento článek vysvětluje, jak používat ve Skiasharpu kreslení čar a grafické cesty v aplikacích Xamarin.Forms a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 9febfabb7b44b1ec09abda4b352691b37565cb48
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615132"
---
# <a name="skiasharp-lines-and-paths"></a>Ve Skiasharpu čáry a cesty

_Použít k vykreslení čar a grafické cesty ve Skiasharpu_

[Předchozí části](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) ukázala, že ve Skiasharpu `SKCanvas` třída obsahuje několik metod pro kreslení obdélníků, symbol tří teček a zaoblené obdélníky. Tato část a v dalších částech zahrnují různé třídy související s vytvořením a vykreslování *grafické cesty*.

Cesty grafiky je nejobecnější přístup k vykreslení čar a křivek v SkiaSharp. Tato část se věnuje `SKPath` objektů pro kreslení rovné čáry a použít kolekci malý rovné čáry (volá *lomenou čáru*) pro kreslení křivek, které můžete definovat matematicky. Další části se popisuje různé druhy křivky podporuje `SKPath`.

Všechny ukázky programů v této části se zobrazí pod nadpisem **čáry a cesty** na domovské stránce [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program a [ **Cesty** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths) složky řešení.

## <a name="lines-and-stroke-capslinesmd"></a>[Čáry a zakončení tahů](lines.md)

Další informace o použití ve Skiasharpu kreslení čar pomocí různých zakončení tahů.

## <a name="path-basicspathsmd"></a>[Cesta – základy](paths.md)

Prozkoumejte objektu ve Skiasharpu SKPath kombinování čar a křivek.

## <a name="the-path-fill-typesfill-typesmd"></a>[Typy výplně cesty](fill-types.md)

Seznamte se s typy výplně cesty ve Skiasharpu různé účinky.

## <a name="polylines-and-parametric-equationspolylinesmd"></a>[Lomené čáry a parametrické rovnice](polylines.md)

Ve Skiasharpu použijte k vykreslení libovolný řádek, který definujete pomocí parametrické rovnice.

## <a name="dots-and-dashesdotsmd"></a>[Tečky a čárky](dots.md)

Hlavní složitými rozhraními z kreslení ve Skiasharpu tečkovaná a přerušované čáry.

## <a name="finger-paintingfinger-paintmd"></a>[Malování prstem](finger-paint.md)

Pomocí prstů malovat na plátně.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
