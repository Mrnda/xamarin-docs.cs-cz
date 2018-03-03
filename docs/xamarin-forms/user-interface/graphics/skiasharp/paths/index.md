---
title: "SkiaSharp čar a cesty"
description: "Kreslení čar a grafiky cesty pomocí SkiaSharp"
ms.topic: article
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: b94091afc459866d072bd3c4adc3947f6be258b1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="skiasharp-lines-and-paths"></a>SkiaSharp čar a cesty

_Kreslení čar a grafiky cesty pomocí SkiaSharp_

[Předchozí části](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) ukázán, který SkiaSharp `SKCanvas` třída obsahuje několik metod pro kreslení obdélníků, tři tečky a zaokrouhlené obdélníky. Tento oddíl a pozdějších částech zahrnují různé třídy připojen k vytváření a vykreslování *grafické cesty*.

Cesty grafiky je nejobecnější přístup pro kreslení čar a křivek v SkiaSharp. Tato část popisuje používání `SKPath` objekt Kreslení úseček a použít kolekci jen nepatrnou přímky (nazývá *lomenou čáru*) kreslení křivek, které můžete definovat matematicky. Další části se věnuje různým druhům křivek nepodporuje `SKPath`.

Všechny programy ukázka v této části se zobrazí v části **čar a cesty** na domovské stránce [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) programu a v [ **Cesty** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Paths) složce řešení.

## <a name="lines-and-stroke-capslinesmd"></a>[Čáry a zakončení tahů](lines.md)

Další informace o použití SkiaSharp kreslení čar pomocí různých tahu CAP k vzdálené ploše.

## <a name="path-basicspathsmd"></a>[Cesta – základy](paths.md)

Prozkoumejte objekt SkiaSharp SKPath pro spojení čar a křivek.

## <a name="the-path-fill-typesfill-typesmd"></a>[Typy výplně cesty](fill-types.md)

Zjistit možné s typy výplně SkiaSharp cesty jiný důsledky.

## <a name="polylines-and-parametric-equationspolylinesmd"></a>[Lomené čáry a parametrické rovnice](polylines.md)

SkiaSharp použijte k vykreslení kterýkoli řádek, který můžete definovat s čištění vzorce.

## <a name="dots-and-dashesdotsmd"></a>[Tečky a čárky](dots.md)

Hlavní rozbor všech nástroje kreslení čar desítkovém a přerušovanou v SkiaSharp.

## <a name="finger-paintingfinger-paintmd"></a>[Malování prstem](finger-paint.md)

Pomocí prstů k vyplnění na plátně.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
