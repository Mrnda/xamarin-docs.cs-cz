---
title: "Funkce usnadnění"
description: "Xamarin.Forms obsahuje třídu náběh a doběh, který umožňuje zadat přenosu funkce, pomocí které řídí, jak animací zrychlit nebo zpomalit jejich používáte. Tento článek ukazuje, jak používat předdefinované funkce usnadnění a postup vytvoření vlastní funkce usnadnění."
ms.topic: article
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: a57fd6e45d744d0e527c811649ce5299ebcd34d5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="easing-functions"></a>Funkce usnadnění

_Xamarin.Forms obsahuje třídu náběh a doběh, který umožňuje zadat přenosu funkce, pomocí které řídí, jak animací zrychlit nebo zpomalit jejich používáte. Tento článek ukazuje, jak používat předdefinované funkce usnadnění a postup vytvoření vlastní funkce usnadnění._


[ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) Třída definuje počet funkce usnadnění, které mohou být spotřebovávána animací:

- [ `BounceIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) Usnadnění funkce nedoručitelných zpráv animace na začátku.
- [ `BounceOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/) Usnadnění funkce nedoručitelných zpráv animace na konci.
- [ `CubicIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/) Funkce usnadnění pomalu zrychluje animace.
- [ `CubicInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/) Usnadnění funkce zrychluje animace na začátku a zpomaluje animace na konci.
- [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/) Usnadnění funkce rychle zpomaluje animace.
- [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) Usnadnění funkce používá konstantní rychlosti a je výchozí funkce usnadnění.
- [ `SinIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/) Funkce usnadnění plynule zrychluje animace.
- [ `SinInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/) Funkce usnadnění plynule zrychluje animace na začátku a plynule zpomaluje animace na konci.
- [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/) Funkce usnadnění plynule zpomaluje animace.
- [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) Usnadnění funkce způsobí, že animace velmi rychle urychlit ke konci.
- [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) Usnadnění funkce způsobí, že animace rychle zpomalení ke konci.

`In` a `Out` přípony znamenat, pokud platnost poskytované nejvýraznější funkce je patrné na začátku animace na konci nebo obojí.

Kromě toho můžete vytvořit vlastní funkce usnadnění. Další informace najdete v tématu [funkce usnadnění vlastní](#customeasing).

## <a name="consuming-an-easing-function"></a>Využívání nejvýraznější funkce

Metody rozšíření animace v [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) třída povolit nejvýraznější funkce zadat jako parametr poslední metodu, jak je ukázáno v následujícím příkladu kódu:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Rychlost animace zadáním nejvýraznější funkce pro animace se změní na jiný lineární a vytvoří poskytuje funkci nejvýraznější dopad. Při vytváření animace vynechání nejvýraznější funkce způsobí, že animace použijte výchozí [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) zmírnit funkci, která vytváří lineární rychlosti.

Další informace o metodách rozšíření animace v [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) třídy najdete v tématu [jednoduché animací](~/xamarin-forms/user-interface/animation/simple.md). Funkce usnadnění může také zpracovat [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) třídy. Další informace najdete v tématu [vlastní animace](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Vlastní funkce usnadnění

K vytvoření vlastní funkce nejvýraznější třemi způsoby:

1. Vytvoření metody, která přebírá `double` argument a vrátí `double` výsledek.
1. Vytvoření `Func<double, double>`.
1. Zadejte jako argument pro funkci nejvýraznější [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) konstruktor.

Ve všech třech případech by měl vrátit vlastní nejvýraznější funkce 0 pro argument 0 a 1 pro argument 1. Libovolná hodnota však mohou být vráceny mezi argument hodnoty 0 a 1. Každý přístup se teď popsané naopak.

### <a name="custom-easing-method"></a>Vlastní zmírnit – metoda

Vlastní nejvýraznější funkce může být definováno jako metody, která přijímá `double` argument a vrátí `double` výsledek, jak je ukázáno v následujícím příkladu kódu:

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

`CustomEase` Metoda zkrátí hodnota příchozí hodnoty 0, 0,2, 0.4, 0,6, 0,8 a 1. Proto [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance je přeložit v diskrétní přeskočí, nikoli bez problémů.

### <a name="custom-easing-func"></a>Vlastní zmírnit Func

Vlastní nejvýraznější funkce může být také definováno jako `Func<double, double>`, jak je znázorněno v následujícím příkladu kódu:

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

`CustomEase` `Func` Představuje nejvýraznější funkce, která se spouští vypnout rychlé a zpomaluje obrátí kurzu a potom zruší kurzu znovu k urychlení rychle ke konci. Proto při celkový pohyb [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance je směrem dolů, ho navíc dočasně obrátí kurzu uprostřed animace.

### <a name="custom-easing-constructor"></a>Vlastní zmírnit – konstruktor

Vlastní nejvýraznější funkce může být také definováno jako argument [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) konstruktoru, jak je ukázáno v následujícím příkladu kódu:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Vlastní nejvýraznější funkci je zadat jako argument lambda funkce k [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) konstruktor a používá `Math.Cos` metodu pro vytvoření efekt pomalé drop, který je omezován podle `Math.Exp` metoda. Proto [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance je přeložená tak, aby se odpojení k jeho poslední řady místo.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak využívat předem definovaná funkce usnadnění a postup vytvoření vlastní funkce usnadnění. Zahrnuje Xamarin.Forms [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) třídu, která umožňuje zadat přenos funkci, která určuje, jak zrychlit animací nebo zpomalit jejich používáte.



## <a name="related-links"></a>Související odkazy

- [Přehled podpory asynchronních operací](~/cross-platform/platform/async.md)
- [Funkce usnadnění (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [Usnadnění](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
