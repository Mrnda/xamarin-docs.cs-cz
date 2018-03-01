---
title: "2D kreslení"
description: "Křížové platformy 2D kreslení s SkiaSharp"
ms.topic: article
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: 03089e4760ebf19849cd4d34cafb7047d8915a4d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="2d-drawing"></a>2D kreslení

SkiaSharp poskytuje výkonné API jazyka C# pro provádění 2D grafiky. Používá technologii [Google Skia knihovny](http://skia.org), stejnou knihovnu, která pohání grafické zásobníky Google Chrome, Firefox a pro Android.

[ ![](images/ide-sml.png "SkiaSharp poskytuje výkonné C# rozhraní API pro provádění 2D grafiky")](images/ide.png)

SkiaSharp je přenosné knihovny a nemusí se dodává jako [balíček NuGet napříč platformami](https://www.nuget.org/packages/SkiaSharp)a podporuje tyto platformy ihned: systému macOS, Xamarin.Android, Xamarin.iOS a Windows Desktop.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[Úvod do SkiaSharp](~/graphics-games/skiasharp/introduction.md)

Přehled o klíčových konceptech SkiaSharp a ukázkový kód k vykreslení grafiky, text, rastrové obrázky a použít filtry bitové kopie.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[SkiaSharp kurzy pro Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Naučte se pracovat s křížové grafiky platformu, která vykreslit v Xamarin.Forms:

- [Základy kreslení](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Kreslení jednoduché kruhu.](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Integrace s Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Pixelů a jednotky nezávislé na zařízení](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Základní animace](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [Integrace textu a obrázků](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Bitmap – základy](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Řádky a cest](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Řádky a tahu CAP k vzdálené ploše](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Základy cesta](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [Typy výplně cesta](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Čáru lomených a čištění vzorce](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Tečky a pomlčky](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Malování prstem](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [Transformace přeložit](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [Transformace škálování](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [Otočit transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [Zkosení transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Maticové transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Manipulace dotykového ovládání](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Bez afinní transformace](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [3D otočení](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Cesty a křivek](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Tři způsoby, jak nakreslit oblouk](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Tři typy Bézierových křivek](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [Data SVG cesty](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Výstřižek se cesty a oblasti](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Cesta efekty](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Cesty a Text.](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Informace o cestě a – výčet](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[Poznámky pro konkrétní platformu](~/graphics-games/skiasharp/platform.md)

Tato stránka popisuje pokynů pro instalaci pro SkiaSharp na různých platformách, včetně iOS, Android, systému macOS a Windows.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[Dokumentaci k rozhraní API](https://developer.xamarin.com/api/namespace/SkiaSharp/)

Můžete procházet [dokumentaci k rozhraní API](https://developer.xamarin.com/api/namespace/SkiaSharp/) pro SkiaSharp na našem webu.

## <a name="work-in-progress"></a>Probíhající práce

SkiaSharp je probíhající práce, kterou jsme sdílení k naší komunitou. Když jsme vázaná důležité částí rozhraní API Skia množství práce zůstává provést. Používáme stabilní C API prezentované podle Skia a naše plán je chcete-li pokračovat, které přispívají naše práce C vazeb Skia k poskytování úplné pokrytí rozhraní API.

Abychom Průvodce naše vazby úsilí, nechejte prosím komentáře nebo návrhy jako problémy v úložišti GitHub [http://github.com/mono/SkiaSharp](http://github.com/mono/SkiaSharp).
