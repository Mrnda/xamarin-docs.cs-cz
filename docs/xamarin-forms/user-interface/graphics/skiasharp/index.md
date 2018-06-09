---
title: Použití SkiaSharp v Xamarin.Forms
description: SkiaSharp je systém 2D grafiky pro rozhraní .NET a C# používá technologii open source Skia grafiky modulem, který je hojně používaná v produktech Google. Tato příručka vysvětluje, jak používat SkiaSharp pro 2D grafiky ve svých aplikacích Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: 272e70af83c8946d0c3eacadac9726487121ac0f
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243935"
---
# <a name="using-skiasharp-in-xamarinforms"></a>Použití SkiaSharp v Xamarin.Forms

_Použít SkiaSharp pro 2D grafiky ve svých aplikacích Xamarin.Forms_

SkiaSharp je systém 2D grafiky pro rozhraní .NET a C# používá technologii open source Skia grafiky modulem, který je hojně používaná v produktech Google. SkiaSharp ve svých aplikacích Xamarin.Forms slouží k vykreslení 2D vektorová grafika, rastrové obrázky a text. Najdete v článku [2D kreslení](~/graphics-games/skiasharp/index.md) Příručka pro další obecné informace o knihovně SkiaSharp a některých dalších kurzů.

Tato příručka předpokládá, že jste obeznámeni s Xamarin.Forms programování.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Webinář: SkiaSharp pro Xamarin.Forms**

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
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [SkiaSharp s Xamarin.Forms Webinář (video)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
