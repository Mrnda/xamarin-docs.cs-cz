---
title: Opakovaně použitelné EffectBehavior
description: Chování jsou užitečné přístup pro přidání do ovládacího prvku, odebrání talíře kotle efekt kód ze souborů kódu pro zpracování efektu. Tento článek ukazuje efekt přidat do ovládacího prvku pomocí chování Xamarin.Forms.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 1ce7eda6f556041cbffc3793b00e8e2cba44d3d0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995778"
---
# <a name="reusable-effectbehavior"></a>Opakovaně použitelné EffectBehavior

_Chování jsou užitečné přístup pro přidání do ovládacího prvku, odebrání talíře kotle efekt kód ze souborů kódu pro zpracování efektu. Tento článek ukazuje efekt přidat do ovládacího prvku pomocí chování Xamarin.Forms._

## <a name="overview"></a>Přehled

`EffectBehavior` Třída je opakovaně použitelné vlastní chování Xamarin.Forms, které přidá [ `Effect` ](xref:Xamarin.Forms.Effect) instanci ovládacího prvku, když je připojen k ovládacímu prvku chování a odebere `Effect` instance je chování odpojit od ovládacího prvku.

Použít toto chování, musí být nastaveny následující vlastnosti chování:

- **Skupiny** – hodnota [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) atribut pro třídu vliv.
- **Název** – hodnota [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) atribut pro třídu vliv.

Další informace o efektů, naleznete v tématu [účinky](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="creating-the-behavior"></a>Vytvoření chování

`EffectBehavior` Třída odvozena z [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) třídy, kde `T` je [ `View` ](xref:Xamarin.Forms.View). To znamená, že `EffectBehavior` třídy lze připojit libovolný ovládací prvek Xamarin.Forms.

### <a name="implementing-bindable-properties"></a>Implementace vlastnosti umožňující vazbu

`EffectBehavior` Třída definuje dvě [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instance, které se používají k přidání [ `Effect` ](xref:Xamarin.Forms.Effect) do ovládacího prvku, když je chování připojen do ovládacího prvku. Tyto vlastnosti jsou uvedeny v následujícím příkladu kódu:

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

Když `EffectBehavior` spotřebování `Group` vlastnost musí být nastavena na hodnotu [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) atribut efektu. Kromě toho `Name` vlastnost musí být nastavena na hodnotu [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) atribut pro třídu vliv.

### <a name="implementing-the-overrides"></a>Implementace přepsání

`EffectBehavior` Třídy přepsání [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) a [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) metody [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) třídy, jak je znázorněno v následujícím kódu Příklad:

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

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Metoda provede instalaci pomocí volání `AddEffect` metodu připojeného ovládacího prvku jako parametr. [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Metoda provede vyčištění voláním `RemoveEffect` metodu připojeného ovládacího prvku jako parametr.

### <a name="implementing-the-behavior-functionality"></a>Implementace chování funkce

Chování slouží k přidání [ `Effect` ](xref:Xamarin.Forms.Effect) definované v `Group` a `Name` vlastnosti do ovládacího prvku, když je připojen k ovládacímu prvku chování a odebrat `Effect` chování, když je odpojit od ovládacího prvku. Základní chování funkce můžete vidět v následujícím příkladu kódu:

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

`AddEffect` Provedení metody v reakci `EffectBehavior` připojovaný ke ovládací prvek a přijímá jako parametr připojeného ovládacího prvku. Metoda pak přidá načtený efekt ovládacího prvku [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekce. `RemoveEffect` Provedení metody v reakci `EffectBehavior` budou tímto odpojeny z ovládacího prvku a přijímá jako parametr připojeného ovládacího prvku. Metoda pak taky odebere efekt z ovládacího prvku [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekce.

`GetEffect` Metoda používá [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) metodu pro načtení [ `Effect` ](xref:Xamarin.Forms.Effect). Účinek se nachází prostřednictvím zřetězení `Group` a `Name` hodnot vlastností. Pokud na platformě neposkytuje efekt, `Effect.Resolve` metoda vrátí non -`null` hodnotu.

## <a name="consuming-the-behavior"></a>Využívání chování

`EffectBehavior` Třídy lze připojit k [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) kolekce ovládacího prvku, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

Ekvivalentní kód jazyka C# můžete vidět v následujícím příkladu kódu:

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

`Group` a `Name` vlastnosti chování, které jsou nastaveny na hodnoty [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) a [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) atributy pro třídu efekt v každé specifické pro platformu projekt.

Za běhu, když chování je připojen k [ `Label` ](xref:Xamarin.Forms.Label) ovládací prvek, `Xamarin.LabelShadowEffect` budou přidány do ovládacího prvku [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekce. Výsledkem je stín přidávaný do text, zobrazený `Label` řídit, jak je znázorněno na následujících snímcích obrazovky:

![](effect-behavior-images/screenshots.png "Ukázkovou aplikaci pomocí EffectsBehavior")

Výhodou použití toto chování přidávat a odebírat účinky z ovládacích prvků je, že je kód pro zpracování efekt kotle talíře odebrat ze soubory kódu na pozadí.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali efekt přidat do ovládacího prvku pomocí chování. `EffectBehavior` Třída je opakovaně použitelné vlastní chování Xamarin.Forms, které přidá [ `Effect` ](xref:Xamarin.Forms.Effect) instanci ovládacího prvku, když je připojen k ovládacímu prvku chování a odebere `Effect` instance je chování odpojit od ovládacího prvku.


## <a name="related-links"></a>Související odkazy

- [Efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Efekt chování (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [Chování](xref:Xamarin.Forms.Behavior)
- [Chování<T>](xref:Xamarin.Forms.Behavior`1)
