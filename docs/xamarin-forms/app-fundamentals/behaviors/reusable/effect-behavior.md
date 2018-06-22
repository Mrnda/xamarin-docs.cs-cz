---
title: Znovu použitelné EffectBehavior
description: Chování jsou užitečný postup pro přidání do ovládacího prvku, odebrání kotle desku vliv kód z soubory kódu pro zpracování vliv. Tento článek ukazuje použití Xamarin.Forms chování pro přidání do ovládacího prvku vliv.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: a1612d1e87f0e05c859babd93fd03ac9a5736b47
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30785052"
---
# <a name="reusable-effectbehavior"></a>Znovu použitelné EffectBehavior

_Chování jsou užitečný postup pro přidání do ovládacího prvku, odebrání kotle desku vliv kód z soubory kódu pro zpracování vliv. Tento článek ukazuje použití Xamarin.Forms chování pro přidání do ovládacího prvku vliv._

## <a name="overview"></a>Přehled

`EffectBehavior` Třída je opakovaně použitelné vlastní chování Xamarin.Forms, která přidává [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) instance do ovládacího prvku, když je připojen k řízení chování a odebere `Effect` instance po chování odpojit od řízení.

Následující vlastnosti chování musí být nastavené použít toto chování:

- **Skupiny** – hodnota [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) atribut pro třídu vliv.
- **Název** – hodnota [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) atribut pro třídu vliv.

Další informace o důsledky najdete v tématu [důsledky](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="creating-the-behavior"></a>Vytváření chování

`EffectBehavior` Třída odvozená z [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) třídy, kde `T` je [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/). To znamená, že `EffectBehavior` třídy je možné připojit k libovolný ovládací prvek Xamarin.Forms.

### <a name="implementing-bindable-properties"></a>Implementace vazbu vlastnosti

`EffectBehavior` Třída definuje dvě [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instance, které se používají pro přidání [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) do ovládacího prvku po připojení k řízení chování. Tyto vlastnosti jsou uvedeny v následující příklad kódu:

```csharp
public class EffectBehavior : Behavior<View>
{
  public static readonly BindableProperty GroupProperty =
    BindableProperty.Create ("Group", typeof(string), typeof(EffectBehavior), null);
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(EffectBehavior), null);

  public string Group {
    get { return (string)GetValue (GroupProperty); }
    set { SetValue (GroupProperty, value); }
  }

  public string Name {
    get { return(string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }
  ...
}
```

Když `EffectBehavior` zpracován, `Group` vlastnost musí být nastavená na hodnotu [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) atribut pro účinek. Kromě toho `Name` vlastnost musí být nastavená na hodnotu [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) atribut pro třídu vliv.

### <a name="implementing-the-overrides"></a>Implementace přepsání

`EffectBehavior` Třídy přepsání [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) a [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) metody [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) třídy, jak je znázorněno v následujícím kódu Příklad:

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  protected override void OnAttachedTo (BindableObject bindable)
  {
    base.OnAttachedTo (bindable);
    AddEffect (bindable as View);
  }

  protected override void OnDetachingFrom (BindableObject bindable)
  {
    RemoveEffect (bindable as View);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Metoda provede instalaci pomocí volání `AddEffect` metodu předáním v ovládacím prvku připojené jako parametr. [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Metoda provádí vyčištění voláním `RemoveEffect` metodu předáním v ovládacím prvku připojené jako parametr.

### <a name="implementing-the-behavior-functionality"></a>Implementace funkce chování

Účelem chování je přidat [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) definované v `Group` a `Name` vlastnosti do ovládacího prvku, když je připojen k řízení chování a odebrat `Effect` po chování odpojit od řízení. Základní funkce chování je znázorněno v následujícím příkladu kódu:

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  void AddEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Add (GetEffect ());
    }
  }

  void RemoveEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Remove (GetEffect ());
    }
  }

  Effect GetEffect ()
  {
    if (!string.IsNullOrWhiteSpace (Group) && !string.IsNullOrWhiteSpace (Name)) {
      return Effect.Resolve (string.Format ("{0}.{1}", Group, Name));
    }
    return null;
  }
}
```

`AddEffect` Metodu je provést v reakci `EffectBehavior` připojovaný ke ovládacího prvku a přijímá ovládacího prvku připojené jako parametr. Metoda načtené účinek pak přidá do ovládacího prvku [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekce. `RemoveEffect` Metodu je provést v reakci `EffectBehavior` se odpojit z ovládacího prvku a přijímá ovládacího prvku připojené jako parametr. Metoda pak Odebere účinek z ovládacího prvku [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekce.

`GetEffect` Metoda používá [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) metoda pro načtení [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/). Účinek nachází prostřednictvím zřetězení `Group` a `Name` hodnot vlastností. Pokud na platformě neposkytuje platit, `Effect.Resolve` metoda vrátí jinou hodnotu než`null` hodnotu.

## <a name="consuming-the-behavior"></a>Využívání chování

`EffectBehavior` Třídy je možné připojit k [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) kolekce ovládacího prvku, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

Ekvivalentní kódu C# je znázorněno v následujícím příkladu kódu:

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};
label.Behaviors.Add (new EffectBehavior {
  Group = "Xamarin",
  Name = "LabelShadowEffect"
});
```

`Group` a `Name` vlastnosti chování jsou nastaveny na hodnoty [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) a [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) atributy pro třídu vliv v každé specifické pro platformu projekt.

Za běhu, když chování je připojen k [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládací prvek, `Xamarin.LabelShadowEffect` budou přidány do ovládacího prvku [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekce. Výsledkem je stínové, který se přidává do textu zobrazovaného `Label` řídit, jak je vidět na následujících snímcích obrazovky:

![](effect-behavior-images/screenshots.png "Ukázkovou aplikaci s EffectsBehavior")

Výhodou použití toto chování přidávat a odebírat důsledky z ovládacích prvků je, že kód pro zpracování vliv kotle tabulky může být odebrána z soubory kódu.

## <a name="summary"></a>Souhrn

Tento článek ukázal, pokud chcete přidat efekt do ovládacího prvku pomocí chování. `EffectBehavior` Třída je opakovaně použitelné vlastní chování Xamarin.Forms, která přidává [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) instance do ovládacího prvku, když je připojen k řízení chování a odebere `Effect` instance po chování odpojit od řízení.


## <a name="related-links"></a>Související odkazy

- [Efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Vliv chování (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [Chování](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Chování<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
