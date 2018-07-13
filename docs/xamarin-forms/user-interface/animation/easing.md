---
title: Funkce usnadnění v Xamarin.Forms
description: Xamarin.Forms obsahuje třídu Easing, která vám umožní určit funkce převodu, který řídí, jak urychlit animace, nebo zpomalit spuštěnými. Tento článek ukazuje, jak využívat předem definovaných funkcí usnadnění a jak vytvořit vlastní funkcí usnadnění.
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 1c75771173d94a18c7c1cc5100c64d45bdc32078
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998118"
---
# <a name="easing-functions-in-xamarinforms"></a>Funkce usnadnění v Xamarin.Forms

_Xamarin.Forms obsahuje třídu Easing, která vám umožní určit funkce převodu, který řídí, jak urychlit animace, nebo zpomalit spuštěnými. Tento článek ukazuje, jak využívat předem definovaných funkcí usnadnění a jak vytvořit vlastní funkcí usnadnění._


[ `Easing` ](xref:Xamarin.Forms.Easing) Třída definuje počet funkcí usnadnění, které mohou být spotřebovány animace:

- [ `BounceIn` ](xref:Xamarin.Forms.Easing.BounceIn) Funkce uvolnění nedoručitelných zpráv animace na začátku.
- [ `BounceOut` ](xref:Xamarin.Forms.Easing.BounceOut) Funkce uvolnění nedoručitelných zpráv animace na konci.
- [ `CubicIn` ](xref:Xamarin.Forms.Easing.CubicIn) Funkce uvolnění pomalu zrychluje animace.
- [ `CubicInOut` ](xref:Xamarin.Forms.Easing.CubicInOut) Funkce uvolnění zrychluje animace na začátku a zpomalí animace na konci.
- [ `CubicOut` ](xref:Xamarin.Forms.Easing.CubicOut) Funkce uvolnění rychle zpomalí animace.
- [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) Funkce uvolnění používá konstantní rychlosti a je výchozí funkce uvolnění.
- [ `SinIn` ](xref:Xamarin.Forms.Easing.SinIn) Funkce uvolnění plynule zrychluje animace.
- [ `SinInOut` ](xref:Xamarin.Forms.Easing.SinInOut) Funkce uvolnění plynule zrychluje animace na začátku a plynule zpomalí animace na konci.
- [ `SinOut` ](xref:Xamarin.Forms.Easing.SinOut) Funkce uvolnění plynule zpomalí animace.
- [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) Funkce uvolnění způsobí, že animace velmi rychle zrychlit ke konci.
- [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) Funkce uvolnění způsobí, že se ke konci rychle zpomalení animace.

`In` a `Out` přípony označení, pokud efekt poskytnuté funkci přechodu je patrné na začátku animace, na konci nebo obojí.

Kromě toho můžete vytvořit vlastní funkcí usnadnění. Další informace najdete v tématu [usnadnění funkce vlastního](#customeasing).

## <a name="consuming-an-easing-function"></a>Využívání uvolnění – funkce

Rozšiřující metody animace v [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) třídy povolit usnadnění funkce zadat jako parametr poslední metodu, jak je ukázáno v následujícím příkladu kódu:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Zadáním usnadnění funkce pro animaci rychlosti animace změní nelineárních a vytváří efekt poskytnuté funkci přechodu. Při vytváření animace vynechání usnadnění funkce způsobí, že animace, který chcete použít výchozí [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) funkci uvolnění navázat, který vytváří lineární rychlost.

Další informace o použití metod rozšíření animace v [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) najdete v tématu [jednoduché animace](~/xamarin-forms/user-interface/animation/simple.md). Funkce usnadnění můžete také využívat [ `Animation` ](xref:Xamarin.Forms.Animation) třídy. Další informace najdete v tématu [vlastní animace](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Vlastní funkce usnadnění

Existují tři hlavní přístupy k vytvoření vlastní funkce usnadnění:

1. Vytvoření metody, která přijímá `double` argument a vrátí `double` výsledek.
1. Vytvoření `Func<double, double>`.
1. Zadat jako argument funkci přechodu [ `Easing` ](xref:Xamarin.Forms.Easing) konstruktoru.

Ve všech třech případech by měl vlastní usnadnění funkce vrátí 0 pro argument 0 a 1 pro argument 1. Libovolná hodnota však mohou být vráceny hodnoty argumentů 0 až 1. Každý přístup teď probereme zase.

### <a name="custom-easing-method"></a>Vlastní funkce – metoda

Vlastní usnadnění funkce lze definovat jako metodu, která přijímá `double` argument a vrátí `double` povedou, jak je ukázáno v následujícím příkladu kódu:

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

`CustomEase` Metoda zkrátí hodnotu příchozí hodnoty 0, 0.2, 0.4, 0.6, 0,8 a 1. Proto [ `Image` ](xref:Xamarin.Forms.Image) instance je přeložen v samostatných přeskočí, spíše než plynule.

### <a name="custom-easing-func"></a>Vlastní funkce Func

Vlastní usnadnění funkce lze definovat také jako `Func<double, double>`, jak je ukázáno v následujícím příkladu kódu:

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

`CustomEase` `Func` Představuje funkci přechodu, který začíná rychle, může zpomalit a vrátí kurz a potom vrátí kurz znovu ke zrychlení rychle ke konci. Proto při celkové přesun [ `Image` ](xref:Xamarin.Forms.Image) instance je dolů, také dočasně obrátí kurzu uprostřed animace.

### <a name="custom-easing-constructor"></a>Vlastní funkce konstruktoru

Vlastní funkce usnadnění lze také definovat jako argument [ `Easing` ](xref:Xamarin.Forms.Easing) konstruktoru, jak je ukázáno v následujícím příkladu kódu:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Vlastní usnadnění funkce je zadaný jako argument funkce lambda k [ `Easing` ](xref:Xamarin.Forms.Easing) konstruktor a používá `Math.Cos` metodu pro vytvoření efektu pomalé přetažení, která je Tlumený podle `Math.Exp` metoda. Proto [ `Image` ](xref:Xamarin.Forms.Image) instance je přeložen tak, aby se zobrazuje v jeho konečné řady umístění přetažení.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali, jak využívat předem definovaných funkcí usnadnění a jak vytvořit vlastní funkcí usnadnění. Zahrnuje Xamarin.Forms [ `Easing` ](xref:Xamarin.Forms.Easing) třídu, která vám umožní určit přenos funkci, která určuje, jak urychlit animace nebo zpomalit běží.



## <a name="related-links"></a>Související odkazy

- [Přehled podpory asynchronních operací](~/cross-platform/platform/async.md)
- [Usnadnění funkce (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [Usnadnění](xref:Xamarin.Forms.Easing)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
