---
title: Vlastní animace
description: Třída animace je stavebním blokem všechny animace Xamarin.Forms, s rozšiřující metody ve třídě ViewExtensions vytváření jeden nebo více objektů animace. Tento článek ukazuje, jak používat třídu animace k vytváření a zrušit animací, synchronizovat více animací a vytvořit vlastní animace, které použije animaci vlastnosti, které nejsou animované existující metodami animace.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 302aa784baad9afb703f88dcfba56b68fd3c9105
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="custom-animations"></a>Vlastní animace

_Třída animace je stavebním blokem všechny animace Xamarin.Forms, s rozšiřující metody ve třídě ViewExtensions vytváření jeden nebo více objektů animace. Tento článek ukazuje, jak používat třídu animace k vytváření a zrušit animací, synchronizovat více animací a vytvořit vlastní animace, které použije animaci vlastnosti, které nejsou animované existující metodami animace._


Počet parametrů je nutné zadat při vytváření `Animation` objektu, včetně počáteční a koncové hodnoty vlastnosti se animovaný a zpětné volání, které změní hodnotu vlastnosti. `Animation` Objekt můžete také spravovat kolekci animací podřízené, které můžete spustit a synchronizovat. Další informace najdete v tématu [podřízené animací](#child).

Spuštění animace vytvořené pomocí [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) třídy, která může nebo nemusí obsahovat podřízené animací, se dosáhne voláním [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) metoda. Tato metoda určuje dobu trvání animace a kromě jiného zpětné volání, které řídí, zda opakováním animace.

## <a name="creating-an-animation"></a>Vytváření animace

Při vytváření [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) objektu obvykle minimálně tři parametry jsou povinné, jak je ukázáno v následujícím příkladu kódu:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Tento kód definuje animace z [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) vlastnost [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance z hodnoty 1 na hodnotu 2. Animovaný hodnotu, která se vypočítá Xamarin.Forms, je předaná funkci zpětného volání, zadaný jako první argument, kde se používá, chcete-li změnit hodnotu `Scale` vlastnost.

Animace je spuštěn s volání [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) metoda, jak je ukázáno v následujícím příkladu kódu:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Všimněte si, že [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) metoda nevrací `Task` objektu. Místo toho oznámení je zajišťována prostřednictvím metody zpětného volání.

Následující argumenty jsou určené v `Commit` metoda:

- První argument (*vlastníka*) identifikuje vlastník animace. To může být vizuální prvek, na kterém se používá animace nebo jiné vizuální prvek, například stránky.
- Druhý argument (*název*) identifikuje animace s názvem. Název spolu s vlastník k jednoznačné identifikaci animace. Tato jedinečnou identifikaci pak umožňuje určit, zda je animace spuštěna ([`AnimationIsRunning`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AnimationIsRunning/p/Xamarin.Forms.IAnimatable/System.String/)), nebo ji zrušte ([`AbortAnimation`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/)).
- Třetí argument (*míra*) označuje počet milisekund, po mezi každé volání metody zpětného volání, které jsou definované v [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) – konstruktor
- Poslední argument (*délka*) označuje trvání animace, v milisekundách.
- Pátý argument (*usnadnění*) definuje nejvýraznější funkce, které mají být použity v animace. Alternativně nejvýraznější funkce lze zadat jako argument pro [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) konstruktor. Další informace o usnadnění funkce najdete v tématu [funkce usnadnění](~/xamarin-forms/user-interface/animation/easing.md).
- Argumentem šesté (*dokončení*) je zpětné volání, které bude proveden po dokončení animace. Tato zpětného volání má dva argumenty, s prvním argumentem znamenající konečná hodnota a druhý argument se `bool` , je nastaven na `true` Pokud animace byla zrušena. Případně *dokončení* zpětného volání lze zadat jako argument k [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) konstruktor. Avšak v jednom animace Pokud *dokončení* zpětná volání, které jsou určené v obou `Animation` konstruktor a `Commit` metoda pouze zpětného volání zadaný v `Commit` metoda bude proveden.
- Argumentem sedmého (*opakujte*) je zpětné volání, které umožňuje animace zopakovat. Je volána na konci animace a vrátí `true` označuje, že je třeba opakovat animace.

Celkový efekt je vytvoření animace, která zvyšuje [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) vlastnost [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) z hodnoty 1 na 2, více než 2 sekund (v milisekundách 2000), pomocí [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) usnadnění funkce. Pokaždé, když dokončení animace jeho `Scale` vlastnost se resetují na 1 a animace se opakuje.

> [!NOTE]
> Souběžné animací, které spustit nezávisle na sobě navzájem konstruovat vytvořením `Animation` objekt pro každý animace a pak volání `Commit` metodu na každý animace.

<a name="child" />

### <a name="child-animations"></a>Podřízené animace

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Třída také podporuje podřízené animací, což zahrnuje vytvoření `Animation` objekt, který druhý `Animation` objekty jsou přidány. To umožňuje řadu animací ke spuštění a synchronizovány. Následující příklad kódu ukazuje vytvoření a spuštění animace podřízené:

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

Příklad kódu Alternativně lze zapsat více výstižně, prokázaná v následujícím příkladu kódu:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

V obou příkladech kódu nadřazený [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) je vytvořen objekt, pro který Další `Animation` objekty se pak přidají. První dva argumenty, které mají [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) metoda určete, kdy zahájení a ukončení animace podřízené. Argument hodnoty musí být mezi 0 a 1 a představuje relativní období v rámci nadřazené animace zadaný podřízený animace bude aktivní. Proto v tomto příkladu `scaleUpAnimation` aktivní pro první polovinu animace, `scaleDownAnimation` aktivní pro druhou polovinu animace a `rotateAnimation` bude aktivní, a to po celou dobu trvání.

Celkový efekt je, že animace k více než 4 sekundy (4000 v milisekundách). `scaleUpAnimation` Animuje [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) vlastnost z 1 na 2, více než 2 sekundy. `scaleDownAnimation` Pak animuje `Scale` vlastnost z 2 na 1, více než 2 sekundy. Během jsou obě animací škálování, `rotateAnimation` animuje [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) vlastnost od 0 do 360, více než 4 sekundy. Všimněte si, že škálování animací také používat funkce usnadnění. [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) Usnadnění funkce způsobí, že [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) původně zmenšení před získáním větší a [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) funkce usnadnění způsobí, že `Image` stane menší než jeho skutečná velikost na konci dokončení animace.

Existuje několik rozdílů mezi [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) objekt, který používá podřízené animace a ten, který nemá:

- Při použití animace podřízené *dokončení* zpětné volání pro podřízené animace označuje po dokončení podřízených a *dokončení* předaný zpětného volání `Commit` metoda určuje, kdy celý animace byla dokončena.
- Při použití animace podřízené, vrácení `true` z *opakovat* zpětné volání `Commit` metoda nezpůsobí opakování animace, ale animace bude nadále spouštět bez nové hodnoty.
- Při zahrnutí nejvýraznější funkce v `Commit` metoda a nejvýraznější funkce vrátí hodnotu větší než 1, animace bude ukončena. Pokud nejvýraznější funkce vrátí hodnotu menší než 0, hodnota se do něj na hodnotu 0. Pokud chcete používat nejvýraznější funkce, která vrátí hodnotu menší než 0 nebo větší než 1, musí zadaná v jedné z podřízených animací, nikoli v `Commit` metoda.

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Třída také obsahuje [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/) metody, které můžete použít k přidání podřízené animací s nadřazenou položkou `Animation` objektu. Však jejich *začít* a *Dokončit* argument hodnoty nejsou omezeny na 0, 1, ale jenom ta část animace podřízené, který odpovídá rozsahu 0 až 1 bude aktivní. Například pokud `WithConcurrent` volání metody, které definuje podřízené animace, která je cílena [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) vlastnost od 1 do 6, ale s *začít* a *Dokončit* hodnoty -2 a 3, *začít* hodnota -2 odpovídá `Scale` hodnotu 1 a *Dokončit* hodnota 3 odpovídá `Scale` hodnotu 6. Protože hodnoty mimo rozsah 0 a 1 hrát žádná část v animace, `Scale` vlastnost bude animovaný pouze od 3 do 6.

## <a name="canceling-an-animation"></a>Zrušení animace

Aplikace můžete zrušit animace pomocí volání [ `AbortAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/) rozšíření metoda, jak je ukázáno v následujícím příkladu kódu:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Všimněte si, že animace jsou jedinečně identifikovaný kombinací animace vlastníka a název animace. Proto vlastníka a název zadán při spuštění animace musí být zrušit animace. Proto příkladu kódu okamžitě zrušit animace s názvem `SimpleAnimation` , vlastní stránky.

## <a name="creating-a-custom-animation"></a>Vytvoření vlastní animace

Pokud zde uvedené příklady úspěšně demonstrovali animace, které mají stejnou lze dosáhnout pomocí metod v [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) třídy. Ale výhodou [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) je třída, má přístup k metoda zpětného volání, které se spustí při změně animovaný hodnoty. To umožňuje zpětného volání k implementaci všechny požadované animace. Například následující příklad kódu animuje [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) vlastnosti stránky nastavením na [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) hodnotami vytvořenými nástrojem [ `Color.FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/)metoda s hue hodnoty v rozsahu od 0 do 1:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Výsledný animace poskytuje vzhled přechodu pozadí stránky prostřednictvím barvy rainbow.

Další příklady vytváření složitých animace, včetně animace Bézierovu křivku, najdete v části [kapitoly 22](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) z [vytváření mobilních aplikací s Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="creating-a-custom-animation-extension-method"></a>Vytváření rozšíření metodu vlastní animace

Metody rozšíření v [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) třída animace vlastnost od aktuální hodnoty, která má zadanou hodnotou. Tím je těžké vytvořit, například `ColorTo` animace metoda, která slouží k animace barvu z jednu hodnotu, která má jiný, protože:

- Jediným [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) vlastnosti definované [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) třída je [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), což není vždy požadovanou `Color` vlastnost pro animaci.
- Často aktuální hodnota [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) vlastnost je [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), což není skutečné barvy a který nelze použít ve výpočtech interpolace.

Řešení tohoto problému je nemá `ColorTo` metoda cíle konkrétní [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) vlastnost. Místo toho je možné zapsat pomocí metody zpětného volání, který předává interpolované `Color` hodnota zpět k volajícímu. Kromě toho bude metoda trvat spuštění a ukončení `Color` argumenty.

`ColorTo` Může být implementována metoda jako metody rozšíření, která používá [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) metoda v [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) třída k poskytnutí funkcí. Důvodem je, že `Animate` metoda slouží k vlastností cíle, které nejsou typu `double`, jak je znázorněno v následujícím příkladu kódu:

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

[ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) Metoda vyžaduje, *transformace* argument, což je metoda zpětného volání. Vstup pro tento zpětné volání je vždy `double` rozsahu od 0 do 1. Proto `ColorTo` metoda definuje vlastní transformace `Func` který přijme `double` rozsahu od 0 do 1 a že vrátí [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) hodnotu odpovídající této hodnotě. `Color` Hodnota se vypočítá jako interpolace [ `R` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/), [ `G` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/), [ `B` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/), a [ `A` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/) hodnoty dvou zadaný `Color` argumenty. `Color` Metoda zpětného volání pro aplikaci na konkrétní vlastnost potom předána hodnota.

Tento přístup umožňuje `ColorTo` metodu pro všechny animace [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) vlastnosti, jak je ukázáno v následujícím příkladu kódu:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

V tomto příkladu kódu `ColorTo` metoda animuje [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) a [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) vlastnosti [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), `BackgroundColor`vlastnosti stránky a [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) vlastnost [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/).

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak používat [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) třída k vytvoření a zrušit animací, synchronizaci více animací a vytvořit vlastní animace, které použije animaci vlastnosti, které nejsou animované existující animace metody. `Animation` Třída je stavebním blokem všechny Xamarin.Forms animace.


## <a name="related-links"></a>Související odkazy

- [Vlastní animace (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [Animace](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)
- [AnimationExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)
