---
title: Transformace SkiaSharp
description: V tomto článku jsou zde popsány transformací pro zobrazení grafiky SkiaSharp v aplikacích Xamarin.Forms a to ukazuje s ukázkový kód.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: d8e9c26df9286cec94562b5d3d7b7721cc3f3f3d
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244945"
---
# <a name="skiasharp-transforms"></a>Transformace SkiaSharp

_Další informace o transformací pro zobrazení SkiaSharp grafiky_

SkiaSharp podporuje tradiční grafiky transformace, které jsou implementovány jako metody [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) objektu. Matematický, transformací alter souřadnice a velikostí, které zadáte v `SKCanvas` kreslení funkce, jako jsou vykreslovány grafické objekty. Transformace jsou často vhodné pro kreslení opakovaných grafiky nebo animace. Některé techniky &mdash; například otáčení rastrové obrázky nebo text &mdash; nejsou možné bez použití transformací.

Transformace SkiaSharp podporují následující operace:

- *Převede* k posunutí souřadnice z jednoho umístění do druhého
- *Škálování* zvýšení nebo snížení souřadnice a velikosti
- *Otočit* otočení souřadnice z bodu
- *Zkreslit* se posunou koordinuje vodorovně nebo svisle tak, aby obdélníku stalo rovnoběžník

Toto jsou známé jako *afinní* transformace. Afinní transformace vždy zachovat paralelní řádky a nikdy způsobit souřadnice nebo velikost k nekonečné. Čtverce nikdy převede na jakoukoli jinou hodnotu než rovnoběžník a kruh nikdy převede na jakoukoli jinou hodnotu než elipsy.

SkiaSharp podporuje i jiné afinní transformace (také nazývané *projective* nebo *perspektivy* transformuje) podle standardní 3 3 transformační matice. Afinní transformace umožňuje čtverce ke zpracování na všechny konvexní čtyřúhelník (okolo obrázek se všech vnitřní úhly menší než 180 stupňů). Bez afinní transformace může způsobit souřadnice nebo velikosti k nekonečné, ale jsou důležité pro 3D efekty.

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>Rozdíly mezi SkiaSharp a transformace Xamarin.Forms

Xamarin.Forms také podporuje transformace, které jsou podobné jako v SkiaSharp. Platformě Xamarin.Forms [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) třída definuje následující vlastnosti transformace:

- `TranslationX` A `TranslationY`
- `Scale`
- `Rotation`, `RotationX`, a `RotationY`

`RotationX` a `RotationY` vlastnosti jsou perspektivy transformace, které se vytvořit quasi 3D účinky.

Existuje několik klíčové rozdíly mezi SkiaSharp transformací a transformace Xamarin.Forms:

První rozdíl je, že jsou SkiaSharp transformací použitá pro celou `SKCanvas` objektu během transformací Xamarin.Forms se použijí pro jednotlivé `VisualElement` odvozené konfigurace. (Můžete použít Xamarin.Forms transformace na `SKCanvasView` objekt samostatně, protože `SKCanvasView` odvozená z `VisualElement`, ale v rámci které `SKCanvasView`, použít transformace SkiaSkarp.)

Transformace SkiaSharp jsou od levého horního rohu `SKCanvas` Xamarin.Forms transformace jsou relativní vzhledem k levém horním rohu `VisualElement` u kterých se použije. Tento rozdíl je důležité, při použití, změny velikosti a oběh transformuje, protože tyto soubory jsou vždy vzhledem k konkrétní bodu.

Opravdu velký rozdíl je, že SKiaSharp transformací *metody* jsou transformace Xamarin.Forms *vlastnosti*. To je rozdíl sémantického nad rámec syntaktické rozdíly: transformace SkiaSharp provedení určité operace, při Xamarin.Forms transformací sady stavu. Transformace SkiaSharp použít následně vykresleného grafických objektů, ale není grafických objektů, které jsou vykreslovány před použitím pro transformaci. Naproti tomu se Xamarin.Forms transformace vztahuje na dříve vykreslovaného elementu co nejrychleji, pokud je vlastnost nastavena. SkiaSharp transformace jsou kumulativní, jako jsou metody říká; Transformace Xamarin.Forms jsou nahrazeny, když je vlastnost nastavena s jinou hodnotou.

Všechny programy ukázka v této části se zobrazí v části **transformuje** na domovské stránce [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programu a v [ **Transformuje** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms) složce řešení.

## <a name="the-translate-transformtranslatemd"></a>[Transformace translace](translate.md)

Další informace o použití transformace přeložit se posunou SkiaSharp grafiky.

## <a name="the-scale-transformscalemd"></a>[Transformace měřítka](scale.md)

Zjistit transformace škálování SkiaSharp pro škálování objekty, které se různé velikosti.

## <a name="the-rotate-transformrotatemd"></a>[Transformace rotace](rotate.md)

Prozkoumejte dopady a animací možné pomocí SkiaSharp rotační transformace.

## <a name="the-skew-transformskewmd"></a>[Transformace zkosení](skew.md)

V tématu, jak můžete vytvořit nakloněné grafické objekty v SkiaSharp zkosení transformace.

## <a name="matrix-transformsmatrixmd"></a>[Maticové transformace](matrix.md)

Podrobně hlubší SkiaSharp transformací s univerzální transformační matice.

## <a name="touch-manipulationstouchmd"></a>[Manipulace dotyků](touch.md)

Použití matice transformuje implementovat manipulace dotykového ovládání pro přetahování, změny velikosti a oběh.

## <a name="non-affine-transformsnon-affinemd"></a>[Neafinní transformace](non-affine.md)

Překročila oridinary s důsledky-afinní transformace.

## <a name="3d-rotation3d-rotationmd"></a>[3D otáčení](3d-rotation.md)

Otočit 2D objektů v 3D prostoru pomocí-afinní transformace.


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
