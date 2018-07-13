---
title: Jednoduché animace v Xamarin.Forms
description: Třída ViewExtensions poskytuje rozšiřující metody, které lze použít k vytvoření jednoduché animace. Tento článek popisuje vytvoření a rušení horizontálních oddílů pomocí třídy ViewExtensions animace.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: 124fc311d5e2c8c89353ba813df60f0bf1d0b34a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997068"
---
# <a name="simple-animations-in-xamarinforms"></a>Jednoduché animace v Xamarin.Forms

_Třída ViewExtensions poskytuje rozšiřující metody, které lze použít k vytvoření jednoduché animace. Tento článek popisuje vytvoření a rušení horizontálních oddílů pomocí třídy ViewExtensions animace._


[ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Třída poskytuje následující metody rozšíření, které lze použít k vytvoření jednoduché animace:

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animuje [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) a [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) vlastnosti [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`ScaleTo`](xref:Xamarin.Forms.VisualElement.Scale) Animuje [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) vlastnost [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) animovaný přírůstkové zvýšení nebo snížení se vztahuje [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) vlastnost [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animuje [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) vlastnost [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) animovaný přírůstkové zvýšení nebo snížení se vztahuje [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) vlastnost [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animuje [ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX) vlastnost [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animuje [ `RotationY` ](xref:Xamarin.Forms.VisualElement.RotationY) vlastnost [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animuje [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) vlastnost [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).

Ve výchozím nastavení bude trvat každou animaci 250 milisekund. Ale doba trvání pro každou animaci lze při vytváření animace.

[ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Také zahrnuje třídy [ `CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) metodu, která slouží k zrušení všech animací.

> [!NOTE]
> [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Poskytuje třídy [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)) – metoda rozšíření. Tato metoda je však určena rozložení používané pro animaci přechodů mezi stavy rozložení, které obsahují velikost a umístění změny. Proto je by měla sloužit pouze podle [ `Layout` ](xref:Xamarin.Forms.Layout) podtřídy.

Rozšiřující metody animace v [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) třídy jsou všechny asynchronní a zpět `Task<bool>` objektu. Vrácená hodnota je `false` Pokud dokončení animace a `true` Pokud animace se zrušila. Proto, animace by měl obvykle použít u metody `await` operátor, který umožňuje snadno zjistit po dokončení animace. Kromě toho pak bude možné vytvořit sekvenční animací pomocí metod následné animace provádění po dokončení předchozí metoda. Další informace najdete v tématu [složené animace](#compound).

Pokud se požaduje, aby animace dokončena na pozadí, pak bude `await` operátor může vynechat. V tomto scénáři se rozšiřující metody animace rychle vrátit po iniciaci svého animace s animací, ke kterým dochází na pozadí. Tato operace může být přijata výhod při vytváření složených animace. Další informace najdete v tématu [složené animace](#composite).

Další informace o `await` operátoru, naleznete v tématu [Přehled podpory asynchronních](~/cross-platform/platform/async.md).

## <a name="single-animations"></a>Jeden animace

Každá metoda rozšíření v [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) implementuje operaci jedné animace, která postupně se změní vlastnost z jedné hodnoty s jinou hodnotou po určitou dobu. Tato část popisuje jednotlivé operace animace.

### <a name="rotation"></a>Otočení

Následující příklad kódu ukazuje použití [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metody pro animaci [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) vlastnost [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Tento kód animuje [ `Image` ](xref:Xamarin.Forms.Image) instance otočením až 360 stupňů více než 2 sekundy (2000 MS). [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda získá aktuální [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) vlastnost hodnotu pro spuštění animace a potom otočí z této hodnoty na svůj první argument (360). Jakmile je animace dokončí, na obrázku [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) vlastnost nastavena na hodnotu 0. To zajistí, že `Rotation` vlastnost není po dokončení animace, která by jinak znemožňovaly další rotace zůstat na 360.

Na následujících snímcích obrazovky zobrazit otáčení probíhá na jednotlivých platformách:

![](simple-images/rotateto.png "Otočení animace")

### <a name="relative-rotation"></a>Relativní otočení

Následující příklad kódu ukazuje použití [ `RelRotateTo` ](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metodu postupně zvýšit nebo snížit [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) vlastnost [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RelRotateTo (360, 2000);
```

Tento kód animuje [ `Image` ](xref:Xamarin.Forms.Image) instance otočením 360 stupňů z jeho výchozí pozice více než 2 sekundy (2000 MS). [ `RelRotateTo` ](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda získá aktuální [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) vlastnost hodnotu pro spuštění animace a potom otočí z této hodnoty na hodnotu plus svůj první argument (360). Tím se zajistí, že každý animace bude vždycky 360 stupňů otočení od počáteční pozice. Proto pokud nové animace je vyvolána, když již probíhá animace, to se spustí od aktuální pozice a ukončit na pozici, která není přírůstek 360 stupňů.

Na následujících snímcích obrazovky zobrazit relativní otáčení probíhá na jednotlivých platformách:

![](simple-images/relrotateto.png "Relativní otočení animace")

### <a name="scaling"></a>Škálování

Následující příklad kódu ukazuje použití [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) metody pro animaci [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) vlastnost [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.ScaleTo (2, 2000);
```

Tento kód animuje [ `Image` ](xref:Xamarin.Forms.Image) instance vertikálním navýšením kapacity na dvojnásobek velikosti více než 2 sekundy (2000 MS). [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) Metoda získá aktuální [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) hodnota vlastnosti (výchozí hodnota 1) na začátku animace a pak možnost škálování z této hodnoty pro svůj první argument (2). To má za následek rozšiřuje velikost bitové kopie na dvojnásobek velikosti.

Na následujících snímcích obrazovky ukazují, probíhá operace škálování na jednotlivých platformách:

![](simple-images/scaleto.png "Škálování animace")

### <a name="relative-scaling"></a>Relativní škálování

Následující příklad kódu ukazuje použití [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metody pro animaci [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) vlastnost [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RelScaleTo (2, 2000);
```

Tento kód animuje [ `Image` ](xref:Xamarin.Forms.Image) instance vertikálním navýšením kapacity na dvojnásobek velikosti více než 2 sekundy (2000 MS). [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda získá aktuální [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) hodnotu vlastnosti na začátku animace a pak škáluje z této hodnoty na hodnotu plus svůj první argument (2). Tím se zajistí, že každý animace bude vždycky škálování 2 od počáteční pozice.

### <a name="scaling-and-rotation-with-anchors"></a>Změna měřítka a otočení pomocí kotev vztahů

[ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) a [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) nastavit střed změny velikosti nebo otočení pro vlastnosti [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) a [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) vlastnosti. Proto také ovlivnit jejich hodnoty [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) a [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) metody.

Vzhledem [ `Image` ](xref:Xamarin.Forms.Image) , který se nachází v centru rozložení, následující příklad kódu ukazuje otočení obrázku kolem středu rozložení tak, že nastavíte její [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) Vlastnost:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

Obměna [ `Image` ](xref:Xamarin.Forms.Image) instance kolem středu rozložení, [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) a [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) vlastnosti musí být nastavena na hodnoty, které jsou vzhledem k šířku a výšku `Image`. V tomto příkladu střed `Image` je definován jako v centru rozložení a proto výchozí `AnchorX` hodnota 0,5 nevyžaduje změny. Ale `AnchorY` předefinovalo se vlastnost na hodnotu z horní části `Image` do středového bodu rozložení. To zajistí, že `Image` provede úplné otočení 360 stupňů okolo středového bodu rozložení, jak je znázorněno na následujících snímcích obrazovky:

![](simple-images/rotate-anchors.png "Otočení animace s kotvy")

### <a name="translation"></a>Překlad

Následující příklad kódu ukazuje použití [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) metody pro animaci [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) a [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) vlastnosti [ `Image`](xref:Xamarin.Forms.Image):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Tento kód animuje [ `Image` ](xref:Xamarin.Forms.Image) instanci překladu je vodorovně a svisle více než jedna sekunda (1000 milisekund). [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda současně přeloží image 100 pixelů na levé straně a 100 pixelů nahoru. Je to proto, že první a druhý argument jsou obě záporná čísla. Poskytuje kladná čísla převedla na obrázku doprava a dolů.

Na následujících snímcích obrazovky zobrazit překlad probíhá na jednotlivých platformách:

![](simple-images/translateto.png "Překlad animace")

> [!NOTE]
> Pokud element je zpočátku vytvoří rozložení obrazovky a pak přeložit na obrazovku, po převodu vstupní rozložení prvku zůstane mimo obrazovku a uživatel nemůže pracovat s ním. Proto je doporučeno, zobrazení by měl být nastíněny v jeho poslední pozice, a pak potřebné převody provést.

### <a name="fading"></a>Pozvolného

Následující příklad kódu ukazuje použití [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metody pro animaci [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) vlastnost [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Tento kód animuje [ `Image` ](xref:Xamarin.Forms.Image) instanci pozvolného ve více než 4 sekundami (4000 milisekund). [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda získá aktuální [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) hodnotu vlastnosti na začátku animace a pak pomalu v z této hodnoty na svůj první argument (1).

Na následujících snímcích obrazovky zobrazit fade probíhá na jednotlivých platformách:

![](simple-images/fadeto.png "Animace pozvolného")

<a name="compound" />

## <a name="compound-animations"></a>Složené animace

Složené animace je kombinací sekvenční animace a je možné vytvořit s `await` operátoru, jak je ukázáno v následujícím příkladu kódu:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

V tomto příkladu [ `Image` ](xref:Xamarin.Forms.Image) přeložen více než 6 sekund (6000 milisekund). Překlad `Image` využívá pět animace s `await` operátor označující, že každou animaci provádí postupně. Proto následné animace metody spustit po dokončení předchozí metoda.

<a name="composite" />

## <a name="composite-animations"></a>Složený animace

Složený animace je tvořená kombinací animace, kde dva nebo více animace běžet současně. Složený animace mohou vytvořit kombinování nejočekávanějších a očekáváno animace, jak je ukázáno v následujícím příkladu kódu:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

V tomto příkladu [ `Image` ](xref:Xamarin.Forms.Image) je škálování a současně otočený o více než 4 sekundami (4000 milisekund). Škálování aplikace `Image` používá dva sekvenční animace, ke kterým dochází ve stejnou dobu jako otočení. [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda provede bez `await` operátor a vrátí hodnotu okamžitě, přičemž první [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) animace, pak od. `await` Operátor v prvním `ScaleTo` volání metody zpoždění druhý `ScaleTo` volání metody až do první `ScaleTo` volání metody byla dokončena. V tuto chvíli `RotateTo` animace se polovina stejně, jako je dokončení a `Image` bude otočený o 180 stupňů. Během poslední 2 sekundy (2000 MS) druhá `ScaleTo` animace a `RotateTo` animace, jak dokončit.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Spuštění více asynchronních metod současně

`static` `Task.WhenAny` a `Task.WhenAll` metody se používají jak souběžně spustit několik asynchronních metod a lze proto použít k vytvoření složeného animace. Obě metody vrací `Task` objektu a přijměte kolekci metod se každý návratový `Task` objektu. `Task.WhenAny` Metoda se dokončí po dokončení jakékoli metodě v jeho kolekce, jak je ukázáno v následujícím příkladu kódu:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

V tomto příkladu `Task.WhenAny` volání metody obsahuje dvě úlohy. První úkol otočí obrázek více než 4 sekundami (4000 milisekund) a druhá úloha se škáluje na obrázku více než 2 sekundy (2000 MS). Po dokončení druhý úkol `Task.WhenAny` volání metody dokončení. Ale i v případě, [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metoda pořád běží, druhý [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) metoda můžete začít.

`Task.WhenAll` Metoda se dokončí po dokončení všechny metody v jeho kolekce, jak je ukázáno v následujícím příkladu kódu:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

V tomto příkladu `Task.WhenAll` volání metody obsahuje tři úkoly, z nichž každý provádí více než 10 minut. Každý `Task` díky jiný počet 360 stupňové rotace – 307 rotace pro [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), 251 rotace pro [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))a 199 rotace pro [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)). Tyto hodnoty jsou prvočísel, proto zajistit, že otočení nejsou synchronizované a proto nebude mít za následek opakovaných vzorů.

Na následujících snímcích obrazovky zobrazit více otáčení probíhá na jednotlivých platformách:

![](simple-images/multiple-rotations.png "Složený animace")

## <a name="canceling-animations"></a>Ruší se animace

Aplikace můžete zrušit jednu nebo více animací pomocí volání `static` [ `ViewExtensions.CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) způsob, jak je ukázáno v následujícím příkladu kódu:

```csharp
ViewExtensions.CancelAnimations (image);
```

Okamžitě tato akce zruší všechny animace, které jsou aktuálně spuštěné [ `Image` ](xref:Xamarin.Forms.Image) instance.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali vytváření a zrušení animací [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) třídy. Tato třída poskytuje metody rozšíření, které lze použít k vytvoření jednoduché animace, které otočit, škálování, překlad a fade [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) instancí.


## <a name="related-links"></a>Související odkazy

- [Přehled podpory asynchronních operací](~/cross-platform/platform/async.md)
- [Základní animace (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
