---
title: Používání Skiasharpu v Xamarin.Forms
description: Ve Skiasharpu je systém 2D grafika pro .NET a C# používá technologii stroj grafiky Skia open source, který je často používána v produktech Google. Tato příručka vysvětluje, jak používat ve Skiasharpu pro 2D grafika ve svých aplikacích Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: f7d97b798bf2a5a75af0731a665fe212491a6516
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615870"
---
# <a name="using-skiasharp-in-xamarinforms"></a>Používání Skiasharpu v Xamarin.Forms

_Použijte ve Skiasharpu pro 2D grafika ve svých aplikacích Xamarin.Forms_

Ve Skiasharpu je systém 2D grafika pro .NET a C# používá technologii stroj grafiky Skia open source, který je často používána v produktech Google. Ve Skiasharpu v aplikacích Xamarin.Forms slouží k vykreslení 2D vektorová grafika, rastrové obrázky a text. Zobrazit [2D kreslení](~/graphics-games/skiasharp/index.md) Průvodce pro další obecné informace o knihovně ve Skiasharpu a některých dalších kurzů.

Tato příručka předpokládá, že máte zkušenosti s Xamarin.Forms programování.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Webinář: SkiaSharp pro Xamarin.Forms**

## <a name="skiasharp-preliminaries"></a>Nezbytné úkony ve Skiasharpu

Ve Skiasharpu pro Xamarin.Forms je zabalený jako balíček NuGet. Po vytvoření řešení Xamarin.Forms v sadě Visual Studio nebo Visual Studio pro Mac, můžete použít Správce balíčků NuGet pro hledání **SkiaSharp.Views.Forms** balíček a přidejte ji do vašeho řešení. Pokud zaškrtnete **odkazy** části každého projektu po přidání ve Skiasharpu vidíte, že různé **ve Skiasharpu** knihovny jsou přidané do všech projektů v řešení.

Pokud cílíte na iOS aplikaci Xamarin.Forms, na stránce Vlastnosti projektu změnit minimální cíl nasazení na iOS 8.0.

Na libovolné stránce jazyka C#, která používá ve Skiasharpu budete chtít zahrnout `using` směrnice pro [ `SkiaSharp` ](https://developer.xamarin.com/api/namespace/SkiaSharp/) obor názvů, který zahrnuje všechny SkiaSharp třídy, struktury a výčty, které použijete v grafiky programování. Je také vhodné `using` směrnice pro [ `SkiaSharp.Views.Forms` ](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) obor názvů pro třídy, které jsou specifické pro Xamarin.Forms. To je mnohem menší oboru názvů, s nichž nejdůležitější je třída [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/). Tato třída je odvozena z Xamarin.Forms `View` třídy a je hostitelem vaší ve Skiasharpu grafického výstupu.

> [!IMPORTANT]
> `SkiaSharp.Views.Forms` Obor názvů také obsahuje `SKGLView` třídu odvozenou od `View` ale využívá OpenGL pro vykreslování grafiky. Pro účely jednoduchosti, tato příručka omezuje na `SKCanvasView`, ale pomocí `SKGLView` místo toho je velmi podobné.

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[Základy kreslení ve SkiaSharpu](basics/index.md)

Některé z nejjednodušší údajů grafiky, které můžete kreslit ve Skiasharpu jsou kruhy, elips a obdélníky. V zobrazení tyto hodnoty, se dozvíte o souřadnice SkiaSharp, velikostí a barev.

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[Čáry a cesty ve SkiaSharpu](paths/index.md)

Cesty grafiky je řada připojených přímo čar a křivek. Cesty můžete vytáhnout, naplní, nebo obojí. Toto téma zahrnuje mnoho aspektů Perokresba, včetně zakončení tahu a spojení a Čárkovaná a tečkované čáry, ale přestane nemá geometrie křivky.

## <a name="skiasharp-transformstransformsindexmd"></a>[Transformace SkiaSharp](transforms/index.md)

Transformace povolit grafických objektů rovnoměrně přeložit, škálování, otočit, nebo zešikmená. Tento článek také ukazuje, jak můžete použít standardní 3 3 transformační matice pro vytváření neafinní transformace a použití transformace na cesty.

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[Křivky a cesty ve SkiaSharpu](curves/index.md)

Zkoumání cesty pokračuje s přidáním křivek na objekty cesty a využití dalších funkcí výkonné cestu. Uvidíte, jak můžete zadat celou cestu v stručný textový řetězec, jak používat efekty cest a ponořte se do interní informace o cestě.

## <a name="skiasharp-bitmapsbitmapsindexmd"></a>[Rastrové obrázky SkiaSharp](bitmaps/index.md)

Rastrové obrázky mají Pravoúhlá pole bitů odpovídající pixelů zobrazovací zařízení. Tato série článků ukazuje, jak načíst, uložit, zobrazit, vytvořit, nakreslit, animace a přístup k bitů ve Skiasharpu rastrové obrázky.

## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Ve Skiasharpu s Xamarin.Forms Webinář (video)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
