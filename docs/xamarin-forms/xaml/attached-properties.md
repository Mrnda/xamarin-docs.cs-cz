---
title: "Přidružené vlastnosti"
description: "– Přidružená vlastnost je zvláštním typem vazbu vlastnosti definované v jedné třídy ale připojené k jiné objekty a rozpoznat v jazyce XAML jako atribut, který obsahuje třídu a název vlastnosti, které jsou odděleny tečkou. Tento článek obsahuje úvod do přidružené vlastnosti a ukazuje, jak vytvářet a využívat je."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 7112812c843ccbcd6c24ea028deae3c09851b03d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="attached-properties"></a>Přidružené vlastnosti

_– Přidružená vlastnost je zvláštním typem vazbu vlastnosti definované v jedné třídy ale připojené k jiné objekty a rozpoznat v jazyce XAML jako atribut, který obsahuje třídu a název vlastnosti, které jsou odděleny tečkou. Tento článek obsahuje úvod do přidružené vlastnosti a ukazuje, jak vytvářet a využívat je._

## <a name="overview"></a>Přehled

Připojené vlastnosti povolit objekt přiřadit hodnotu pro vlastnost, která nedefinuje své vlastní třídy. Například podřízený, které můžete použít prvky připojené vlastnosti, které chcete informovat o tom, jak jsou uvedené v uživatelském rozhraní jejich nadřazený element. [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) Řízení umožňuje řádků a sloupců podřízených zadat nastavením `Grid.Row` a `Grid.Column` přidružené vlastnosti. `Grid.Row` a `Grid.Column` jsou přidružené vlastnosti, protože jsou nastavené u elementů, které jsou podřízené objekty `Grid`, spíš než na `Grid` sám sebe.

Vlastnosti vazbu by měla být implementována jako připojené vlastnosti v následujících scénářích:

- Pokud je potřeba mít vlastnost nastavení mechanismus k dispozici pro třídy jiné než definice třídy.
- Pokud třída představuje službu, která musí být snadno integrovat s jiné třídy.

Další informace o vlastnosti vazbu najdete v tématu [vazbu vlastnosti](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="creating-and-consuming-an-attached-property"></a>Vytvoření a použití přidružená vlastnost

Proces vytvoření přidružená vlastnost vypadá takto:

1. Vytvoření [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instance s jedním z [ `CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) přetížení metody.
1. Zadejte `static` `Get` *PropertyName* a `Set` *PropertyName* metody jako přístupových objektů pro připojená vlastnost.

### <a name="creating-a-property"></a>Vytvoření vlastnosti

Při vytváření přidružená vlastnost pro použití v jiných typů, třídě, kde se má vytvořit vlastnost nemá k odvozování z [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/). Ale *cíl* vlastnost pro přistupující objekty by měla být nebo odvozena od, [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/).

– Přidružená vlastnost lze vytvořit pomocí deklarace `public static readonly` vlastnost typu [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/). Vazbu vlastnost musí být nastavená na vrácená hodnota jednoho z [ `BindableProperty.CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) přetížení metody. Deklaraci by měla být v textu vlastnícím třídy, ale mimo všechny definice člen.

Následující kód ukazuje příklad přidružená vlastnost:

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

Tím se vytvoří přidružená vlastnost s názvem `HasShadow`, typu `bool`. Vlastní vlastnost `ShadowEffect` třídy a má výchozí hodnotu `false`. Zásady vytváření názvů pro přidružené vlastnosti je, že identifikátor přidružená vlastnost musí shodovat název vlastnosti zadaný ve `CreateAttached` metoda s "Vlastnost" připojená k němu. Proto v předchozím příkladu je identifikátor přidružená vlastnost `HasShadowProperty`.

Další informace o vytváření vlastnosti vazbu, včetně parametrů, které lze zadat během vytváření, najdete v části [vytváření a použití vazbu vlastnosti](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property).

### <a name="creating-accessors"></a>Vytváření přístupových objektů

Statické `Get` *PropertyName* a `Set` *PropertyName* metody je vyžadován jako přistupující objekty pro připojená vlastnost, jinak bude systém vlastnost nelze použít – přidružená vlastnost. `Get` *PropertyName* přistupujícího objektu by měla odpovídat následující podpis:

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get` *PropertyName* přistupujícího objektu by měla vrátit hodnotu, která se nachází v odpovídající `BindableProperty` pole pro vlastnost připojené. Toho lze dosáhnout pomocí volání [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) metoda předávání v identifikátor vazbu vlastnosti, na kterém má být získána hodnota a pak výslednou hodnotu na požadovaný typ přetypování.

`Set` *PropertyName* přistupujícího objektu by měla odpovídat následující podpis:

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set` *PropertyName* přistupujícího objektu měli nastavit hodnotu odpovídající `BindableProperty` pole pro vlastnost připojené. Toho lze dosáhnout pomocí volání [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody předávání v identifikátor vazbu vlastnosti, na kterém chcete nastavit hodnota a hodnota k nastavení.

Pro obě přístupových objektů *cíl* objektu by měla být nebo odvozena od, [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/).

Následující příklad kódu ukazuje přistupující objekty pro `HasShadow` přidružená vlastnost:

```csharp
public static bool GetHasShadow (BindableObject view)
{
  return (bool)view.GetValue (HasShadowProperty);
}

public static void SetHasShadow (BindableObject view, bool value)
{
  view.SetValue (HasShadowProperty, value);
}
```

### <a name="consuming-an-attached-property"></a>Využívání přidružená vlastnost

Po vytvoření přidružená vlastnost, mohou být využívány z XAML nebo kódu. V jazyce XAML toho se dosáhne deklarace oboru názvů s předponou, s deklaraci oboru názvů, která udává název oboru názvů Common Language Runtime (CLR) a volitelně název sestavení. Další informace najdete v tématu [obory názvů jazyka XAML](~/xamarin-forms/xaml/namespaces.md).

Následující příklad kódu ukazuje oboru názvů jazyka XAML pro vlastní typ, který obsahuje přidružená vlastnost, která je definována v rámci stejného sestavení jako aplikační kód, který odkazuje na vlastní typ:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

Deklarace oboru názvů se pak použije, když nastavení přidružená vlastnost na určitý ovládací prvek, jako ukázáno v následujícím příkladu kódu XAML:

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

Ekvivalentní kódu C# je znázorněno v následujícím příkladu kódu:

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consuming-an-attached-property-with-a-style"></a>Využívání přidružená vlastnost s styl

Připojené vlastnosti můžete také přidat do ovládacího prvku ve stylu. Následující příklad ukazuje kód XAML *explicitní* styl, který používá `HasShadow` přidružená vlastnost, která může být použita na [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládací prvky:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Lze použít pro [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) nastavením jeho [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnost, která má `Style` pomocí `StaticResource`– rozšíření značek, jak je ukázáno v následujícím příkladu kódu:

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

Další informace o styly najdete v tématu [styly](~/xamarin-forms/user-interface/styles/index.md).

## <a name="advanced-scenarios"></a>Složitější scénáře

Při vytváření přidružená vlastnost, existují počet volitelné parametry, které můžete nastavit pro povolení přidružená vlastnost pokročilé scénáře. To zahrnuje detekce změn vlastnosti, ověřování hodnoty vlastností a vynucený hodnot vlastností. Další informace najdete v tématu [pokročilé scénáře](~/xamarin-forms/xaml/bindable-properties.md#advanced).

## <a name="summary"></a>Souhrn

Tento článek poskytuje úvod do přidružené vlastnosti a ukázal, jak lze vytvářet a využívat je. – Přidružená vlastnost je zvláštní druh vazbu vlastnosti, které jsou definované v jedné třídy ale připojené k ostatním objektům a rozpoznatelném v jazyce XAML jako atributy, které obsahují třídy a název vlastnosti odděleny tečkou.


## <a name="related-links"></a>Související odkazy

- [Vazbu vlastnosti](~/xamarin-forms/xaml/bindable-properties.md)
- [Obory názvů jazyka XAML](~/xamarin-forms/xaml/namespaces.md)
- [Efekt stínu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
