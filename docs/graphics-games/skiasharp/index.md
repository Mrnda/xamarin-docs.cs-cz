---
title: 2D kreslení ve Skiasharpu
description: Tento dokument obsahuje přehled multiplatformní 2D kreslení ve Skiasharpu. Odkazuje na různé pokyny, které popisují ve Skiasharpu a jeho různá rozhraní API.
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 0c8cbc14308c8c4131e5aaa2bcc0ddfa798af610
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130917"
---
# <a name="2d-drawing-with-skiasharp"></a>2D kreslení ve Skiasharpu

Ve Skiasharpu poskytuje výkonné API jazyka C# to 2D grafika. Používá technologii [Googlu Skia knihovny](http://skia.org), stejnou knihovnu, která je základem grafické zásobníky Google Chrome, Firefox a pro Android.

[![](images/ide-sml.png "Ve Skiasharpu poskytuje výkonné API jazyka C# to 2D grafika")](images/ide.png#lightbox)

Ve Skiasharpu je přenosné knihovny a snadno se dodává jako [balíček NuGet multiplatformní](https://www.nuget.org/packages/SkiaSharp)a podporuje tyto platformy předpřipravených: macOS, Windows Desktop, Xamarin.Android a Xamarin.iOS.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[Úvod do ve Skiasharpu](~/graphics-games/skiasharp/introduction.md)

Přehled o klíčových konceptech ve Skiasharpu a ukázkový kód pro vykreslení grafiky, text, bitmap a použít obraz filtry.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[Kurzy ve Skiasharpu v Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Zjistěte, jak pracovat s pro různé platformy grafiky, která vykreslit v Xamarin.Forms:

- [Základy kreslení](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Nakreslení jednoduchého kruhu](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Integrace se Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Pixely a jednotky nezávislé na zařízení](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Základní animace](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [Integrace textu a obrázků](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Bitmapa – základy](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Čáry a cesty](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Čáry a zakončení tahů](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Cesta – základy](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [Typy výplně cesty](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Lomené čáry a parametrické rovnice](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Tečky a spojovníky](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Malování prstem](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [Transformace translace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [Transformace měřítka](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [Transformace rotace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [Transformace zkosení](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Maticové transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Manipulace dotyků](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Non afinní transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [3D otáčení](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Křivky a cesty](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Tři způsoby, jak nakreslit oblouk](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Tři typy Bézierových křivek](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [Data cesty SVG](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Ořezy cestami a oblastmi](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Efekty cest](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Cesty a text](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Informace o cestě a výčet](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)
- [Rastrové obrázky](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/index.md)
  * [Zobrazení rastrových obrázků](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/displaying.md)
  * [Vytváření a na základě rastrových obrázků](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/drawing.md)
  * [Oříznutí rastrových obrázků](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/cropping.md)
  * [Segmentované zobrazení rastrových obrázků](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/segmented.md)
  * [Ukládání bitmap k souborům](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/saving.md)
  * [Přístup k rastrové bity pixelů](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/pixel-bits.md)
  * [Animace rastrových obrázků](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/animating.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[Poznámky pro konkrétní platformu](~/graphics-games/skiasharp/platform.md)

Tato stránka popisuje pokynů instalačního programu pro SkiaSharp na různých platformách, včetně iOS, Android, macOS a Windows.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[Dokumentace k rozhraní API](https://developer.xamarin.com/api/namespace/SkiaSharp/)

Můžete přejít [dokumentace k rozhraní API](https://developer.xamarin.com/api/namespace/SkiaSharp/) pro SkiaSharp na našem webu.

## <a name="work-in-progress"></a>Probíhající práce

Ve Skiasharpu je ve vývoji, které sdílíme s naší komunitou. Když jsme vázaná důležité části Skia rozhraní API mnoho práce je ještě třeba provést. Používáme stabilní rozhraní API jazyka C prezentované podle Skia a náš plán je nadále přispívání naší práci do vazby C Skia úplné pokrytí rozhraní API.

Nám Průvodce naše úsilí vazby, nechejte prosím nějaké komentáře nebo návrhy jako problémy v úložišti GitHub [ http://github.com/mono/SkiaSharp ](http://github.com/mono/SkiaSharp).
