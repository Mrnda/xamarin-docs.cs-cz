---
title: "Použití SkiaSharp v Xamarin.Forms"
description: "Použít SkiaSharp pro 2D grafiky ve svých aplikacích Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: a49c442fcce31fb6b853359ddfafc9a43d0a2114
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="using-skiasharp-in-xamarinforms"></a>Použití SkiaSharp v Xamarin.Forms

_Použít SkiaSharp pro 2D grafiky ve svých aplikacích Xamarin.Forms_

SkiaSharp je systém 2D grafiky pro rozhraní .NET a C# používá technologii open source Skia grafiky modulem, který je hojně používaná v produktech Google. SkiaSharp ve svých aplikacích Xamarin.Forms slouží k vykreslení 2D vektorová grafika, rastrové obrázky a text. Najdete v článku [2D kreslení](~/graphics-games/skiasharp/index.md) Příručka pro další obecné informace o knihovně SkiaSharp a některých dalších kurzů.

Tato příručka předpokládá, že jste obeznámeni s Xamarin.Forms programování.

## <a name="webinar"></a>Webinar

[![](images/skiasharpwebinarscreen.png "SkiaSharp Xamarin.Forms webinář")](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)  
[Podívejte se na webinář "SkiaSharp pro Xamarin.Forms"](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)

## <a name="skiasharp-preliminaries"></a>Nezbytné úkony SkiaSharp

SkiaSharp pro Xamarin.Forms je zabalené jako balíčku NuGet. Po vytvoření řešení Xamarin.Forms v sadě Visual Studio nebo Visual Studio pro Mac, můžete použít Správce balíčků NuGet pro vyhledávání **SkiaSharp.Views.Forms** balíček a přidejte ji do vašeho řešení. Pokud zaškrtnete **odkazy** části každého projektu přidané SkiaSharp můžete uvidíte, že různé **SkiaSharp** knihovny jsou přidané do všechny projekty v řešení.

Pokud vaše aplikace Xamarin.Forms zaměřuje iOS, na stránce vlastností projektu můžete změnit cíl minimální nasazení na iOS 8.0.

Na libovolné stránce C# se používá SkiaSharp budete chcete zahrnout `using` direktivy pro [ `SkiaSharp` ](https://developer.xamarin.com/api/namespace/SkiaSharp/) názvů, který zahrnuje všechny SkiaSharp tříd, struktur a výčty, které budete používat v grafiky programování. Také budete muset `using` direktivy pro [ `SkiaSharp.Views.Forms` ](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) obor názvů pro specifické pro Xamarin.Forms třídy. To je mnohem menší obor názvů, s nejdůležitější se třída [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/). Tato třída odvozená z platformě Xamarin.Forms `View` třídy a hostitelem SkiaSharp grafiky výstupu.

> [!IMPORTANT]
> `SkiaSharp.Views.Forms` Obor názvů obsahuje taky `SKGLView` třídu odvozenou od `View` ale používá OpenGL pro vykreslování grafiky. Pro účely jednoduchost, tato příručka používat pouze k `SKCanvasView`, ale pomocí `SKGLView` místo toho je velmi podobné.

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[Základy kreslení ve SkiaSharpu](basics/index.md)

Některé nejjednodušší grafiky údajů, které použijete při kreslení SkiaSharp jsou kružnice, elipsy a obdélníky. V zobrazení tyto údaje, se dozvíte o SkiaSharp souřadnice, velikosti a barvy.

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[Čáry a cesty ve SkiaSharpu](paths/index.md)

Cesty grafiky je řadu připojené přímých čar a křivek. Cesty mohou vytáhnout, vyplněno, nebo obojí. Toto téma zahrnuje mnoho aspektů kreslení čáry, včetně tahu je ukončená a spojení a přerušovanou a čáry s koncovými body, ale přestane souborem geometrie křivky.

## <a name="skiasharp-transformstransformsindexmd"></a>[Transformace SkiaSharp](transforms/index.md)

Transformace povolit grafických objektů jednotně přeložit, škálování, otáčet, nebo nesouměrně rozdělí. Tento článek také ukazuje, jak můžete použít standardní 3 3 transformační matice pro vytváření-afinní transformace a použití transformace na cesty.

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[Křivky a cesty ve SkiaSharpu](curves/index.md)

Zkoumání cest pokračuje přidávání křivek na cestu objekty a zneužitím další funkce, výkonné cestu. Uvidíte, jak můžete zadat celou cestu v stručným textového řetězce, jak použít cestu důsledky a jak a dostanete se do interní informace o cestě.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [SkiaSharp s Xamarin.Forms Webinář (video)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
