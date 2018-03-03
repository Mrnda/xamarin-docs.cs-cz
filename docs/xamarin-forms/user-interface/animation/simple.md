---
title: "Jednoduché animace"
description: "Třída ViewExtensions poskytuje rozšiřující metody, které lze použít k vytvoření jednoduché animace. Tento článek ukazuje vytvoření a zrušení animace pomocí třídy ViewExtensions."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: d1d91804c11d2e944bb618fadb3d659b512c5905
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="simple-animations"></a>Jednoduché animace

_Třída ViewExtensions poskytuje rozšiřující metody, které lze použít k vytvoření jednoduché animace. Tento článek ukazuje vytvoření a zrušení animace pomocí třídy ViewExtensions._


[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Třída poskytuje následující metody rozšíření, které lze použít k vytvoření jednoduché animací:

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animuje [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) a [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) vlastnosti [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`ScaleTo`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Animuje [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) vlastnost [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) platí animovaný přírůstkové zvýšení nebo snížení k [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) vlastnost [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animuje [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) vlastnost [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RelRotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) platí animovaný přírůstkové zvýšení nebo snížení k [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) vlastnost [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateXTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animuje [ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) vlastnost [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateYTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animuje [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) vlastnost [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`FadeTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animuje [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) vlastnost [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).

Ve výchozím nastavení bude každý animace trvat 250 milisekund. Doba trvání jednotlivých animací však lze zadat, při vytváření animace.

[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Třída také obsahuje [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) metoda, která slouží k zrušení všech animace.

> [!NOTE]
> [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Třída poskytuje [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/) metoda rozšíření. Tato metoda je však určena má být používána rozložení pro animaci přechodů mezi stavy rozložení, které obsahují velikosti a pozice změny. Proto je lze používat pouze pomocí [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) podtřídy.

Metody rozšíření animace v [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) třídy jsou všechny asynchronní a zpět `Task<bool>` objektu. Vrácená hodnota je `false` Pokud dokončení animace a `true` Pokud animace byla zrušena. Proto metody animace musí být použit obvykle s `await` operátor, který vám umožní snadno zjistit po dokončení animace. Kromě toho pak bude možné vytvořit sekvenční animace pomocí metod následné animace provádění po dokončení předchozí metoda. Další informace najdete v tématu [složené animací](#compound).

Pokud je požadavek umožníte animace dokončení na pozadí, pak se `await` operátor lze vynechat. V tomto scénáři se rozšiřující metody animace rychle vrátit po inicializaci animační, s animace, ke kterým dochází v pozadí. Tuto operaci můžete provedeny výhod, při vytváření složených animace. Další informace najdete v tématu [složené animací](#composite).

Další informace o `await` operátor, najdete v části [Přehled podpory asynchronní](~/cross-platform/platform/async.md).

## <a name="single-animations"></a>Jeden animace

Každá metoda rozšíření v [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) implementuje animace jednu operaci, která postupně změní vlastnost z jednu hodnotu na jinou hodnotu přes v časovém intervalu. Tato část prozkoumá každou operaci animace.

### <a name="rotation"></a>Otočení

Následující příklad kódu ukazuje, jak pomocí [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metoda pro animaci [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) vlastnost [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Tento kód animuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance otočením až 360 stupňů více než 2 sekund (v milisekundách 2000). [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metoda získá aktuální [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) vlastnost value pro spuštění animace a pak otočí z této hodnoty na svůj první argument (360). Animace po dokončení, obrázku [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) vlastnost nastaven na hodnotu 0. To zajistí, že `Rotation` vlastnost není zůstávají v 360 po animace se ukončí, která by zabránila další otočení.

Na následujících snímcích obrazovky zobrazit otočení probíhá na jednotlivých platformách:

![](simple-images/rotateto.png "Otočení animace")

### <a name="relative-rotation"></a>Relativní otočení

Následující příklad kódu ukazuje, jak pomocí [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metoda přírůstkově zvýšení nebo snížení [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) vlastnost [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelRotateTo (360, 2000);
```

Tento kód animuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance otočením 360 stupňů od počáteční pozice více než 2 sekund (v milisekundách 2000). [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metoda získá aktuální [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) vlastnost value pro spuštění animace a pak otočí z této hodnoty na hodnotu plus svůj první argument (360). To zajistí, že každý animace bude vždy rotaci 360 stupňů od počáteční pozice. Proto pokud je nový animace je vyvolána při animace již probíhá, ho začnou od aktuální pozice a může se stát, na pozici, který není přírůstek 360 stupňů.

Na následujících snímcích obrazovky zobrazit relativní otočení probíhá na jednotlivých platformách:

![](simple-images/relrotateto.png "Relativní otočení animace")

### <a name="scaling"></a>Změna měřítka

Následující příklad kódu ukazuje, jak pomocí [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) metoda pro animaci [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) vlastnost [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.ScaleTo (2, 2000);
```

Tento kód animuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instanci škálování na dvojnásobek velikosti více než 2 sekund (v milisekundách 2000). [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Metoda získá aktuální [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) hodnota vlastnosti (výchozí hodnota 1) pro spuštění animace a pak měřítka z této hodnoty na svůj první argument (2). To má za následek rozbaluje velikost bitové kopie na dvojnásobek velikosti.

Na následujících snímcích obrazovky zobrazit škálování probíhá na jednotlivých platformách:

![](simple-images/scaleto.png "Škálování animace")

### <a name="relative-scaling"></a>Relativní škálování

Následující příklad kódu ukazuje, jak pomocí [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metoda pro animaci [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) vlastnost [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelScaleTo (2, 2000);
```

Tento kód animuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instanci škálování na dvojnásobek velikosti více než 2 sekund (v milisekundách 2000). [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metoda získá aktuální [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) hodnota vlastnosti pro spuštění animace a pak měřítka z této hodnoty na hodnotu plus svůj první argument (2). To zajistí, že každý animace bude vždy škálování 2 od počáteční pozice.

### <a name="scaling-and-rotation-with-anchors"></a>Změna velikosti a oběh s kotvy

[ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) a [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) center škálování nebo otočení pro nastavit vlastnosti [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) a [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) vlastnosti. Proto jejich hodnoty ovlivní také [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) a [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) metody.

Vzhledem [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) , byla umístěna v centru rozložení, následující příklad kódu ukazuje, otáčení bitovou kopii kolem středu rozložení nastavením jeho [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) Vlastnost:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

Obměna [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instanci rozložení, kolem centru [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) a [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) vlastnosti musí být nastavena na hodnoty, které jsou vzhledem k šířce a výšce z `Image`. V tomto příkladu středu `Image` je definován v centru rozložení a tak výchozí `AnchorX` hodnota 0,5 nevyžaduje změny. Ale `AnchorY` vlastnost je předefinována být hodnota od začátku `Image` center bodem rozložení. To zajistí, že `Image` umožňuje rotaci úplné 360 stupňů kolem bodu center rozložení, jak je vidět na následujících snímcích obrazovky:

![](simple-images/rotate-anchors.png "Otočení animace s kotvy")

### <a name="translation"></a>Překlad

Následující příklad kódu ukazuje, jak pomocí [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) metoda pro animaci [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) a [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) vlastnosti [ `Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Tento kód animuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance a nakonec ho vodorovně a svisle více než 1 sekunda (1000 milisekund). [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metoda současně překládá image 100 pixelů na levé straně a 100 pixelů směrem nahoru. Je to proto, že první a druhý argument jsou obě záporná čísla. Poskytnutí kladná čísla by převede obrázek vpravo a dolů.

Na následujících snímcích obrazovky zobrazit překlad probíhá na jednotlivých platformách:

![](simple-images/translateto.png "Překlad animace")

> [!NOTE]
> **Poznámka:**: Pokud element původně nastíněny vypnout obrazovky a pak přeložit na obrazovku, po překladu element vstup rozložení zůstane mimo obrazovku a uživatel nemůže komunikovat s ním. Proto se doporučuje, že zobrazení by měl být nastíněny v konečné polohy, a pak potřebné překlady provést.

### <a name="fading"></a>Pozvolného vysouvání

Následující příklad kódu ukazuje, jak pomocí [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metoda pro animaci [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) vlastnost [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Tento kód animuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instanci pozvolného vysouvání ji ve více než 4 sekundy (4000 v milisekundách). [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metoda získá aktuální [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) hodnota vlastnosti pro spuštění animace a pak stmívačkami v z této hodnoty na svůj první argument (1).

Na následujících snímcích obrazovky zobrazit Objevování probíhá na jednotlivých platformách:

![](simple-images/fadeto.png "Pozvolného vysouvání animace")

<a name="compound" />

## <a name="compound-animations"></a>Složené animace

Složené animace je kombinací sekvenční animace a může být vytvořen pomocí `await` operátor, jak je ukázáno v následujícím příkladu kódu:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

V tomto příkladu [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) je přeložená víc než 6 sekund (6000 v milisekundách). Překlad `Image` používá pět animací s `await` operátor označující každý animace provádí postupně. Proto následné animace metody provést po dokončení předchozí metoda.

<a name="composite" />

## <a name="composite-animations"></a>Složené animace

Složené animace je kombinací animací, kde dvou nebo více animací běžet současně. Složené animací lze vytvořit pomocí kombinování očekávaná a očekáváno animací, jak je ukázáno v následujícím příkladu kódu:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

V tomto příkladu [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) je škálovat a současně otočený o více než 4 sekundy (4000 v milisekundách). Škálování `Image` používá dvě po sobě jdoucích animací, ke kterým dochází ve stejnou dobu jako otočení. [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metoda provede bez `await` operátor a vrátí okamžitě, přičemž první [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) pak od animace. `await` Operátor v prvním `ScaleTo` volání metody zpozdí druhý `ScaleTo` volání metody do prvního `ScaleTo` volání metody byla dokončena. V tomto okamžiku `RotateTo` animace se polovina způsobem dokončit a `Image` bude otočený o 180 stupňů. Během poslední 2 sekund (v milisekundách 2000) druhá `ScaleTo` animace a `RotateTo` animace obě dokončit.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Současné spuštění více asynchronních metod

`static` `Task.WhenAny` a `Task.WhenAll` metody se používají ke spuštění více asynchronních metod souběžně a proto je lze použít k vytváření složených animací. Obě metody vrátit `Task` objektu a přijměte kolekci metod aby se každý návratový `Task` objektu. `Task.WhenAny` Metoda se dokončí po libovolné metody v jeho kolekce dokončením provádění, jak je ukázáno v následujícím příkladu kódu:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

V tomto příkladu `Task.WhenAny` volání metody, které obsahuje dvě úlohy. První úlohou otočí obrázek více než 4 sekundy (4000 v milisekundách), a v druhé úloze změní velikost obrazu více než 2 sekund (v milisekundách 2000). Po dokončení v druhé úloze `Task.WhenAny` dokončení volání metody. Ale i když [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metoda je stále spuštěná, druhý [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) metoda můžete začít.

`Task.WhenAll` Metoda dokončení když jste dokončili všechny metody v jeho kolekce, jak je ukázáno v následujícím příkladu kódu:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

V tomto příkladu `Task.WhenAll` volání metody, které obsahuje úlohy, tři, z nichž každá spouští více než 10 minut. Každý `Task` díky jiný počet 360 stupňové otáčení – 307 otočení pro [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), 251 otočení pro [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)a 199 otočení pro [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/). Tyto hodnoty jsou prvočísel, proto zajistit, aby otočení nejsou synchronizované a proto nebude mít za následek opakovaných vzory.

Na následujících snímcích obrazovky zobrazit více otočení probíhá na jednotlivých platformách:

![](simple-images/multiple-rotations.png "Složené animace")

## <a name="canceling-animations"></a>Zrušení animace

Aplikace můžete zrušit jeden nebo více animace pomocí volání `static` [ `ViewExtensions.CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) metoda, jak je ukázáno v následujícím příkladu kódu:

```csharp
ViewExtensions.CancelAnimations (image);
```

Tato akce zruší okamžitě všechny animace, které jsou aktuálně spuštěné na [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance.

## <a name="summary"></a>Souhrn

Tento článek ukázal, vytváření a zrušení animace pomocí [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) třídy. Tato třída poskytuje rozšiřující metody, které lze použít k vytvoření jednoduché animace, které otočit, škálování, převede a vykreslit [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) instance.


## <a name="related-links"></a>Související odkazy

- [Přehled podpory asynchronních operací](~/cross-platform/platform/async.md)
- [Základní animace (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
