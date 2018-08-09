---
title: Transformace SkiaSharp
description: Tento článek zkoumá transformací pro zobrazování grafického ve Skiasharpu v Xamarin.Forms aplikací a ukazuje to se vzorovým kódem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 89aa29d5bf03b1d6f9668ef2aee6ce0c1a277cc5
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615857"
---
# <a name="skiasharp-transforms"></a>Transformace SkiaSharp

_Další informace o transformacích pro zobrazování grafického ve Skiasharpu_

Ve Skiasharpu podporuje tradiční grafiky transformace, které jsou implementovány jako metody [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) objektu. Matematický, změnit transformace souřadnice a velikostí, které zadáte v `SKCanvas` kreslení funkce jako grafické objekty jsou vykreslovány. Transformace mají často vhodné pro vykreslení grafiky opakovaných nebo pro animaci. Některé techniky &mdash; například otáčení rastrové obrázky a text &mdash; nejsou možné bez použití transformací.

Transformace SkiaSharp podporují tyto operace:

- *Přeložit* souřadnice posun z jednoho umístění do jiného
- *Škálování* chcete zvýšit nebo snížit souřadnice a velikosti
- *Otočit* obměna souřadnice z bodu
- *Zkosení* posunout můžete koordinuje vodorovně nebo svisle tak, aby se z něj rovnoběžník obdélník

Toto jsou známé jako *nastavená na affine* transformace. Afinní transformace vždy zachovat paralelní řádky a nikdy nezpůsobí souřadnice nebo velikost, aby se stal nekonečné. Čtverec se nikdy transformuje na nic jiného než se z něj rovnoběžník a kruh se nikdy transformuje na nic jiného než elipsu.

Také podporuje neafinní transformace SkiaSharp (také nazývané *projective* nebo *perspektivy* transformuje) založené na standardní 3 3 transformační matice. Neafinní transformace umožňuje čtverec a tím se transformuje na jakékoli konvexní čtyřúhelník (čtyřsměrná elementu figure s všechny vnitřní úhly menší než 180stupňový rozsah s orientací). Non afinní transformace může způsobit souřadnice nebo velikosti k nekonečné, ale jsou důležité pro 3D efekty.

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>Rozdíly ve Skiasharpu a transformace Xamarin.Forms

Xamarin.Forms také podporuje transformace, které jsou podobné těm v SkiaSharp. Xamarin.Forms [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) třída definuje následující vlastnosti transformace:

- `TranslationX` a `TranslationY`
- `Scale`
- `Rotation`, `RotationX`, a `RotationY`

`RotationX` a `RotationY` perspektivy transformace, které vytvářejí quasi 3D efekty jsou vlastnosti.

Existuje několik zásadní rozdíly mezi transformace SkiaSharp a Xamarin.Forms transformace:

První rozdíl spočívá v tom, že transformace SkiaSharp platí pro celou `SKCanvas` objektu transformace Xamarin.Forms, které se použijí na osobu `VisualElement` vy. (Můžete použít Xamarin.Forms transformace `SKCanvasView` objektu samotného, protože `SKCanvasView` je odvozena z `VisualElement`, ale v rámci, která `SKCanvasView`, transformace SkiaSkarp použít.)

Transformace SkiaSharp jsou relativní vzhledem k levého horního rohu `SKCanvas` Xamarin.Forms transformace jsou relativní vzhledem k levého horního rohu `VisualElement` na které se použijí. Tento rozdíl je důležité při použití měřítka a otočení transformuje, protože tyto transformace jsou vždy relativní k určité místo.

Opravdu velký rozdíl je, že transformace SKiaSharp *metody* Xamarin.Forms transformace jsou *vlastnosti*. Toto je sémantické rozdíl nad rámec syntaktické rozdíly: transformace SkiaSharp provádět operace, zatímco Xamarin.Forms transformace nastavení stavu. Transformace SkiaSharp použít následně vykresleného grafických objektů, ale ne grafických objektů, které jsou zpracovány před použitím transformace. Naproti tomu transformace Xamarin.Forms platí pro dříve vykreslovaného elementu, jakmile je vlastnost nastavena. Transformace SkiaSharp jsou kumulativní, protože volání těchto metod; Xamarin.Forms transformace jsou nahrazena když je vlastnost nastavena s jinou hodnotou.

Všechny ukázky programů v této části se zobrazí pod nadpisem **transformuje** na domovské stránce [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program a [ **Transformuje** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms) složky řešení.

## <a name="the-translate-transformtranslatemd"></a>[Transformace translace](translate.md)

Další informace o použití transformace translace posunutí ve Skiasharpu grafiky.

## <a name="the-scale-transformscalemd"></a>[Transformace měřítka](scale.md)

Objevte transformace měřítka ve Skiasharpu škálování objektů různých velikostí.

## <a name="the-rotate-transformrotatemd"></a>[Transformace rotace](rotate.md)

Prozkoumejte dopady a možnosti s transformace rotace ve Skiasharpu animace.

## <a name="the-skew-transformskewmd"></a>[Transformace zkosení](skew.md)

Podívejte se, jak můžete vytvořit nakloněné grafických objektů v SkiaSharp transformace zkosení.

## <a name="matrix-transformsmatrixmd"></a>[Maticové transformace](matrix.md)

Ponořte se hlouběji do transformace SkiaSharp s univerzální transformační matice.

## <a name="touch-manipulationstouchmd"></a>[Manipulace dotyků](touch.md)

Použití matice transformuje provádět manipulace dotykového ovládání pro přetažení, měřítko a otočení.

## <a name="non-affine-transformsnon-affinemd"></a>[Neafinní transformace](non-affine.md)

Nad rámec oridinary s účinky neafinní transformace.

## <a name="3d-rotation3d-rotationmd"></a>[3D otáčení](3d-rotation.md)

Pomocí neafinní transformace otočení 2D objekty v 3D prostoru.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API ve Skiasharpu](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
