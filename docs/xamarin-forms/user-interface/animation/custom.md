---
title: Vlastní animace v Xamarin.Forms
description: Tento článek ukazuje, jak se animace Xamarin.FOrms třída slouží k vytváření a zrušit animace, synchronizaci více animací a vytvořit vlastní animace, které animovat vlastnosti, které nejsou animované existující metody animace.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 519368031384e72a2d2e0a7c99053be44ea4cffc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995216"
---
# <a name="custom-animations-in-xamarinforms"></a>Vlastní animace v Xamarin.Forms

_Třída animace je základním pilířem pracovního všechny animace Xamarin.Forms s rozšiřující metody ve třídě ViewExtensions vytvoření jednoho nebo více objektů animace. Tento článek ukazuje, jak používat třídu animace k vytváření a zrušit animace, synchronizovat animací několik a vytvořit vlastní animace, které animovat vlastnosti, které nejsou animované existující metody animace._


Počet parametrů je nutné zadat při vytváření `Animation` objektu, včetně počáteční a koncové hodnoty animované, vlastnosti a zpětné volání, který změní hodnotu vlastnosti. `Animation` Objektu můžete také spravovat kolekci animací podřízený, které můžete spustit a synchronizovat. Další informace najdete v tématu [podřízené animace](#child).

Spuštění animace vytvořené pomocí [ `Animation` ](xref:Xamarin.Forms.Animation) třídu, která může nebo nemusí obsahovat podřízené animace, se dosahuje prostřednictvím volání [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) metody. Tato metoda určuje dobu trvání animace a dalších položek, zpětné volání, která určuje, zda se opakování animace.

## <a name="creating-an-animation"></a>Vytváří se animace

Při vytváření [ `Animation` ](xref:Xamarin.Forms.Animation) objekt obvykle minimálně tři parametry jsou povinné, jak je ukázáno v následujícím příkladu kódu:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Tento kód definuje animaci, která [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) vlastnost [ `Image` ](xref:Xamarin.Forms.Image) instanci z hodnoty 1 na hodnotu 2. Animovaný hodnotu, která se vypočítá Xamarin.Forms, je předaný zpětnému volání, zadaný jako první argument, ve kterém se používá ke změně hodnoty `Scale` vlastnost.

Spuštění animace pomocí volání [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) způsob, jak je ukázáno v následujícím příkladu kódu:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Všimněte si, že [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) metoda nevrací `Task` objektu. Oznámení místo toho jsou k dispozici prostřednictvím metody zpětného volání.

Následující argumenty se zadávají v `Commit` metody:

- První argument (*vlastníka*) identifikuje majitele animace. To může být vizuální prvek, na který se použije animaci nebo jiné vizuální prvek, jako jsou stránky.
- Druhý argument (*název*) identifikuje animace s názvem. Název je kombinovat s vlastníkem k jednoznačné identifikaci animace. Tato jedinečná identifikace pak lze zjistit, zda je spuštěn animace ([`AnimationIsRunning`](xref:Xamarin.Forms.AnimationExtensions.AnimationIsRunning(Xamarin.Forms.IAnimatable,System.String))), nebo zrušit jeho ([`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String))).
- Třetí argument (*míra*) označuje počet milisekund mezi každé volání metody zpětného volání, které jsou definovány v [ `Animation` ](xref:Xamarin.Forms.Animation) konstruktor
- Čtvrtý argument (*délka*) označuje dobu trvání animace v milisekundách.
- Pátý argument (*usnadnění*) definuje funkci uvolnění použít animace. Alternativně lze jako argument pro funkci přechodu [ `Animation` ](xref:Xamarin.Forms.Animation) konstruktoru. Další informace o usnadnění funkce najdete v tématu [usnadnění funkce](~/xamarin-forms/user-interface/animation/easing.md).
- Šestý argument (*dokončení*) je zpětné volání, která se spustí po dokončení animace. Toto zpětné volání přebírá dva argumenty, se první argument konečnou hodnotu, a druhý argument se `bool` , která je nastavena na `true` Pokud animace byla zrušena. Můžete také *dokončení* zpětného volání lze zadat jako argument [ `Animation` ](xref:Xamarin.Forms.Animation) konstruktoru. Avšak v případě jedné animace Pokud *dokončení* zpětná volání jsou určené v i `Animation` konstruktor a `Commit` metody pouze zpětného volání podle `Commit` metody se spustí.
- Sedmého argumentu (*opakujte*) je zpětné volání, která umožňuje animace bude opakovat. Je volána na konci animace a vrácení `true` označuje, že je potřeba zopakovat animace.

Celkový efekt je vytvořit animaci, která se zvyšuje [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) vlastnost [ `Image` ](xref:Xamarin.Forms.Image) od 1 do 2, více než 2 sekundy (2000 MS), pomocí [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) funkce uvolnění. Pokaždé, když dokončení animace, jeho `Scale` vlastnost nastavena na hodnotu 1 a animace se opakuje.

> [!NOTE]
> Souběžné animace, na kterých běží nezávisle na sobě lze sestavit tak, že vytvoříte `Animation` objekt pro každou animaci a následným voláním `Commit` metodu na každou animaci.

<a name="child" />

### <a name="child-animations"></a>Podřízené animace

[ `Animation` ](xref:Xamarin.Forms.Animation) Třída také podporuje podřízené animace, která zahrnuje vytvoření `Animation` další objekt `Animation` objekty jsou přidány. To umožňuje řadě animace spustit a synchronizovat. Následující příklad kódu ukazuje vytvoření a spuštění animace podřízené:

```csharp
var parentAnimation = new Animation ();
var scaleUpAnimation = new Animation (v => image.Scale = v, 1, 2, Easing.SpringIn);
var rotateAnimation = new Animation (v => image.Rotation = v, 0, 360);
var scaleDownAnimation = new Animation (v => image.Scale = v, 2, 1, Easing.SpringOut);

parentAnimation.Add (0, 0.5, scaleUpAnimation);
parentAnimation.Add (0, 1, rotateAnimation);
parentAnimation.Add (0.5, 1, scaleDownAnimation);

parentAnimation.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

Další možností v příkladu kódu lze zapsat, jako předvedenou v následujícím příkladu kódu:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

V obou příkladech kódu nadřazený [ `Animation` ](xref:Xamarin.Forms.Animation) je vytvořen objekt, ke kterému Další `Animation` objekty se pak přidají. První dva argumenty, které mají [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) metoda určit, kdy k zahájení a ukončení animace podřízené. Hodnoty argumentů musí být mezi 0 a 1 a představovat relativní období v rámci nadřazené animace zadanou podřízenou aktivitu animace bude aktivní. Proto se v tomto příkladu `scaleUpAnimation` aktivní v první polovině animace, `scaleDownAnimation` aktivní v druhé polovině animace a `rotateAnimation` bude aktivováno po celou dobu trvání.

Celkový efekt je, že animaci více než 4 sekundami (4000 milisekund). `scaleUpAnimation` Animuje [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) vlastnost z 1 na 2, více než 2 sekundy. `scaleDownAnimation` Následně animuje blednutí `Scale` vlastnost z 2 na 1, více než 2 sekundy. Během jsou obě škálování animace, `rotateAnimation` animuje [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) vlastnost od 0 do 360, více než 4 sekundami. Všimněte si, že animacemi také použít funkcí usnadnění. [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) Funkce uvolnění způsobí, že [ `Image` ](xref:Xamarin.Forms.Image) zpočátku zmenšení před získáním větší a [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) funkce uvolnění způsobí, že `Image` stane menší než skutečné velikosti ke konci dokončení animace.

Existuje několik rozdílů mezi [ `Animation` ](xref:Xamarin.Forms.Animation) objekt, který používá podřízené animace a ten, který není:

- Při použití podřízených animací *dokončení* zpětného volání na podřízené animace označuje po dokončení podřízené a *dokončení* zpětného volání předána `Commit` metoda označují, kdy celý animace byla dokončena.
- Při použití podřízených animace, vrací `true` z *opakujte* zpětné volání `Commit` metoda nezpůsobí opakování animace, ale animace bude dál běžet bez nové hodnoty.
- Při zahrnutí usnadnění funkce v `Commit` metoda a usnadnění funkce vrací hodnotu větší než 1, animace bude ukončena. Pokud usnadnění funkce vrátí hodnotu menší než 0, hodnota je omezen na hodnotu 0. Pokud chcete použít funkci přechodu, který vrací hodnotu menší než 0 nebo větší než 1, musí zadat v jednom podřízené animací, nikoli v `Commit` metody.

[ `Animation` ](xref:Xamarin.Forms.Animation) Třída také obsahuje [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)) metody, které lze použít k přidání podřízené animace s nadřazenou položkou `Animation` objektu. Však jejich *začít* a *Dokončit* hodnoty argumentů nejsou omezené na 0, 1, ale pouze část podřízené animace, který odpovídá rozsahu 0 až 1 bude aktivní. Například pokud `WithConcurrent` volání metody definuje podřízené animace, která cílí [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) vlastnost z 1 až 6, ale s *začít* a *Dokončit* hodnoty -2 a 3, *začít* odpovídá hodnota -2 `Scale` hodnotu 1 a *Dokončit* odpovídá hodnotu 3 `Scale` hodnotu 6. Vzhledem k tomu, že hodnoty mimo rozsah 0 až 1 přehrávat animace, žádná část `Scale` vlastnost bude animovat pouze ze 3 až 6.

## <a name="canceling-an-animation"></a>Ruší se animace

Aplikace můžete zrušit animace pomocí volání [ `AbortAnimation` ](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String)) rozšiřující metoda, jak je ukázáno v následujícím příkladu kódu:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Všimněte si, že animace jsou jedinečně identifikovaný kombinací animace vlastníka a název animace. Vlastníka a název zadán proto při spuštění animace musí být zadán pro zrušení animace. Proto se v příkladu kódu okamžitě zrušit animaci pojmenovanou `SimpleAnimation` , který je vlastněn stránky.

## <a name="creating-a-custom-animation"></a>Vytváří se vlastní animace

Příklady uvedené tady zatím osoby dokáží animace, které mají stejnou lze dosáhnout pomocí metod v [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) třídy. Ale výhodou [ `Animation` ](xref:Xamarin.Forms.Animation) třída je, že má přístup k metodu zpětného volání, která provádí při změně hodnoty animovaný. To umožňuje provádět všechny požadované animace zpětného volání. Například následující příklad kódu animuje [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) vlastnosti stránky nastavením na [ `Color` ](xref:Xamarin.Forms.Color) hodnotami vytvořenými nástrojem [ `Color.FromHsla` ](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))metodou hue hodnoty od 0 do 1:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Výsledný animace poskytuje vzhled přechodu pozadí stránky prostřednictvím barvy rainbow.

Další příklady vytvoření komplexní animace, včetně Bézierovu křivku animace, naleznete v tématu [kapitola 22](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) z [vytváření mobilních aplikací pomocí Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="creating-a-custom-animation-extension-method"></a>Vytvoření metody rozšíření vlastní animace

Metody rozšíření v [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) třídy animovat vlastnost její aktuální hodnota na zadanou hodnotu. To je těžké vytvořit, například `ColorTo` metoda animace, který můžete použít pro animaci barvu z jednu hodnotu do jiné, protože:

- Pouze [ `Color` ](xref:Xamarin.Forms.Color) vlastnosti definované [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) třída je [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor), který není vždy požadovaný `Color` vlastnost pro animaci.
- Často aktuální hodnotu [ `Color` ](xref:Xamarin.Forms.Color) vlastnost [ `Color.Default` ](xref:Xamarin.Forms.Color.Default), který není skutečný barvu a které nelze použít ve výpočtech interpolace.

Řešení tohoto problému nebudete chtít `ColorTo` metoda cílit na konkrétní [ `Color` ](xref:Xamarin.Forms.Color) vlastnost. Místo toho může být zapsán pomocí metody zpětného volání, které předává interpolovaný `Color` hodnotu zpět na volajícího. Kromě toho metoda bude trvat počáteční a ukončit `Color` argumenty.

`ColorTo` Metodu je možné implementovat jako metodu rozšíření, která používá [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) metodu [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions) třídy k zajištění jeho funkce. Je to proto, `Animate` metody slouží k vlastnosti cíle, které nejsou typu `double`, jak je ukázáno v následujícím příkladu kódu:

```csharp
public static class ViewExtensions
{
  public static Task<bool> ColorTo(this VisualElement self, Color fromColor, Color toColor, Action<Color> callback, uint length = 250, Easing easing = null)
  {
    Func<double, Color> transform = (t) =>
      Color.FromRgba(fromColor.R + t * (toColor.R - fromColor.R),
                     fromColor.G + t * (toColor.G - fromColor.G),
                     fromColor.B + t * (toColor.B - fromColor.B),
                     fromColor.A + t * (toColor.A - fromColor.A));
    return ColorAnimation(self, "ColorTo", transform, callback, length, easing);
  }

  public static void CancelAnimation(this VisualElement self)
  {
    self.AbortAnimation("ColorTo");
  }

  static Task<bool> ColorAnimation(VisualElement element, string name, Func<double, Color> transform, Action<Color> callback, uint length, Easing easing)
  {
    easing = easing ?? Easing.Linear;
    var taskCompletionSource = new TaskCompletionSource<bool>();

    element.Animate<Color>(name, transform, callback, 16, length, easing, (v, c) => taskCompletionSource.SetResult(c));
    return taskCompletionSource.Task;
  }
}
```

[ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) Vyžaduje metodu *transformace* argument, který je metoda zpětného volání. Vstupem do této zpětného volání je vždy `double` od 0 do 1. Proto `ColorTo` metoda definuje vlastní transformace `Func` , který přijme `double` od 0 do 1 a vrátí [ `Color` ](xref:Xamarin.Forms.Color) hodnota odpovídající této hodnotě. `Color` Hodnota je vypočítána pomocí interpolace [ `R` ](xref:Xamarin.Forms.Color.R), [ `G` ](xref:Xamarin.Forms.Color.G), [ `B` ](xref:Xamarin.Forms.Color.B), a [ `A` ](xref:Xamarin.Forms.Color.A) hodnoty dvou zadaný `Color` argumenty. `Color` Hodnota se pak předá metodě zpětného volání pro aplikaci na konkrétní vlastnost.

Tento přístup umožňuje `ColorTo` metody pro animaci žádné [ `Color` ](xref:Xamarin.Forms.Color) vlastnost, jak je ukázáno v následujícím příkladu kódu:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

V tomto příkladu kódu `ColorTo` animuje metoda [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) a [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) vlastnosti [ `Label` ](xref:Xamarin.Forms.Label), `BackgroundColor`vlastnosti stránky a [ `Color` ](xref:Xamarin.Forms.BoxView.Color) vlastnost [ `BoxView` ](xref:Xamarin.Forms.BoxView).

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak používat [ `Animation` ](xref:Xamarin.Forms.Animation) třída pro vytvoření a zrušit animace, synchronizaci více animací a vytvoření vlastních animací, které animovat vlastnosti, které nejsou animované existující animace metody. `Animation` Třída je stavebním blokem všech animace Xamarin.Forms.


## <a name="related-links"></a>Související odkazy

- [Vlastní animace (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [Animace](xref:Xamarin.Forms.Animation)
- [AnimationExtensions](xref:Xamarin.Forms.AnimationExtensions)
