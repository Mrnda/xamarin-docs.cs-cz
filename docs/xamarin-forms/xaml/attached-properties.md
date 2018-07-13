---
title: Přidružené vlastnosti
description: Tento článek obsahuje úvod do připojené vlastnosti a ukazuje, jak vytvářet a využívat je.
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 981e59fe3ba8c63d0f6c6a067ceb9f338a02da8f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997329"
---
# <a name="attached-properties"></a>Přidružené vlastnosti

_Připojená vlastnost je speciální typ s možností vazby vlastnosti definované v jedné třídy ale připojený k jiné objekty a rozpoznat v XAML jako atribut, který obsahuje třídu a název vlastnosti oddělené tečkou. Tento článek obsahuje úvod do připojené vlastnosti a ukazuje, jak vytvářet a využívat je._

## <a name="overview"></a>Přehled

Připojené vlastnosti povolit objektu přiřadí hodnotu pro vlastnost, která nedefinuje své vlastní třídy. Podřízené elementy můžete použít například připojených vlastností informovat jejich nadřazený element jak mají být uvedené v uživatelském rozhraní. [ `Grid` ](xref:Xamarin.Forms.Grid) Ovládací prvek umožňuje řádků a sloupců pro podřízenou položku zadat tak, že nastavíte `Grid.Row` a `Grid.Column` připojené vlastnosti. `Grid.Row` a `Grid.Column` jsou připojené vlastnosti, protože jsou nastavena pro prvky, které jsou podřízené `Grid`, spíše než na `Grid` samotný.

Vlastnosti umožňující vazbu by měla být implementována jako připojené vlastnosti v následujících scénářích:

- Když je potřeba mít vlastnost nastavení mechanismus k dispozici pro třídy jiné než definování třídy.
- Když třída představuje službu, která musí být snadno integrovat s jinými třídami.

Další informace o vlastnosti umožňující vazbu, naleznete v tématu [vlastnosti umožňující vazbu](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="creating-and-consuming-an-attached-property"></a>Vytváření a využívání připojené vlastnosti

Proces vytvoření připojené vlastnosti vypadá takto:

1. Vytvoření [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instance s jedním z [ `CreateAttached` ](xref:Xamarin.Forms.BindableProperty.CreateAttached*) přetížení metody.
1. Zadejte `static` `Get` *PropertyName* a `Set` *PropertyName* metody jako přístupové objekty pro připojené vlastnosti.

### <a name="creating-a-property"></a>Vytvoření vlastnosti

Při vytváření připojené vlastnosti pro použití na jiné typy, třídy, kde se vytvoří vlastnost nemá být odvozen od [ `BindableObject` ](xref:Xamarin.Forms.BindableObject). Ale *cílové* pro přistupující objekty vlastnosti by měla být nebo odvozovat, [ `BindableObject` ](xref:Xamarin.Forms.BindableObject).

Přidruženou vlastnost lze vytvořit deklarováním `public static readonly` vlastnost typu [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty). Vázanou vlastnost měli nastavit na hodnotu vrácené některého [ `BindableProperty.CreateAttached` ](xref:Xamarin.Forms.BindableProperty.CreateAttached(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) přetížení metody. Deklarace by měla být v těle vlastnící třídy, ale mimo všechny definice členů.

Následující kód ukazuje příklad připojené vlastnosti:

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

Tím se vytvoří připojené vlastnosti s názvem `HasShadow`, typu `bool`. Vlastní vlastnost `ShadowEffect` třídy a má výchozí hodnotu `false`. Zásady vytváření názvů pro připojené vlastnosti je, že identifikátor připojené vlastnosti musí odpovídat název vlastnosti zadaný v `CreateAttached` metodou "Vlastnosti" připojenou k němu. Proto v předchozím příkladu je připojená vlastnost identifikátor `HasShadowProperty`.

Další informace o vytvoření vlastnosti umožňující vazbu, včetně parametry, které se dá nastavit během vytváření, naleznete v tématu [vytváření a využívání vázanou vlastnost](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property).

### <a name="creating-accessors"></a>Vytváření přístupových objektů

Statické `Get` *PropertyName* a `Set` *PropertyName* metody, které budou sloužit jako přístupové objekty pro připojené vlastnosti, jinak bude systém vlastností nelze použít připojená vlastnost. `Get` *PropertyName* přistupující objekt by měl odpovídat následující podpis:

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get` *PropertyName* přístupového objektu by měla vrátit hodnotu, která se nachází v odpovídající `BindableProperty` pole pro připojené vlastnosti. Toho lze dosáhnout pomocí volání [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) metoda, předávání v identifikátoru vázanou vlastnost, na kterém má být získána hodnota a potom výslednou hodnotu na požadovaný typ přetypování.

`Set` *PropertyName* přistupující objekt by měl odpovídat následující podpis:

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set` *PropertyName* přistupující objekt měli nastavit hodnotu odpovídající `BindableProperty` pole pro připojené vlastnosti. Toho lze dosáhnout pomocí volání [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metodu identifikátor umožňujících vazbu vlastnosti na základě které chcete nastavit hodnotu a hodnotu nastavení.

Pro přistupující objekty jak *cílové* objektu by měla být nebo odvozovat, [ `BindableObject` ](xref:Xamarin.Forms.BindableObject).

Následující příklad kódu ukazuje přístupové objekty pro `HasShadow` přidružená vlastnost:

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

### <a name="consuming-an-attached-property"></a>Použití připojené vlastnosti

Po vytvoření připojené vlastnosti mohou být spotřebovány z XAML nebo kódu. V XAML tím se dosahuje deklarace oboru názvů s předponou, pomocí deklarace oboru názvů označující název oboru názvů Common Language Runtime (CLR) a volitelně název sestavení. Další informace najdete v tématu [obory názvů XAML](~/xamarin-forms/xaml/namespaces.md).

Následující příklad kódu ukazuje obor názvů XAML pro vlastní typ, který obsahuje připojené vlastnosti, která je definována v rámci stejného sestavení jako kód aplikace, který odkazuje na vlastní typ:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

Deklarace oboru názvů se pak použije, když nastavení připojená vlastnost na určitý ovládací prvek, jako ukazuje následující příklad kódu XAML:

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

Ekvivalentní kód jazyka C# můžete vidět v následujícím příkladu kódu:

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consuming-an-attached-property-with-a-style"></a>Použití se stylem připojené vlastnosti

Připojené vlastnosti můžete také přidat do ovládacího prvku ve stylu. Následující příklad ukazuje kód XAML *explicitní* styl, který se používá `HasShadow` připojené vlastnosti, který lze použít k [ `Label` ](xref:Xamarin.Forms.Label) ovládacích prvků:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

[ `Style` ](xref:Xamarin.Forms.Style) Lze použít [ `Label` ](xref:Xamarin.Forms.Label) nastavením jeho [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnost `Style` instance pomocí `StaticResource`– rozšíření značek, jak je ukázáno v následujícím příkladu kódu:

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

Další informace o stylech najdete v tématu [styly](~/xamarin-forms/user-interface/styles/index.md).

## <a name="advanced-scenarios"></a>Pokročilé scénáře

Při vytváření připojené vlastnosti, existuje mnoho nepovinných parametrů, které můžete nastavit pro povolení rozšířených připojených vlastností scénáře. To zahrnuje detekce změn vlastnosti, ověřování hodnoty vlastností a podřízenému hodnot vlastností. Další informace najdete v tématu [pokročilé scénáře](~/xamarin-forms/xaml/bindable-properties.md#advanced).

## <a name="summary"></a>Souhrn

Tento článek poskytuje úvod do připojené vlastnosti a ukázal, jak vytvářet a využívat je. Připojená vlastnost je speciální typ s možností vazby vlastnosti definované v jedné třídy ale připojených k jiným objektům a rozpoznat v XAML jako atributy, které obsahují třídy a názvu vlastnosti oddělené tečkou.


## <a name="related-links"></a>Související odkazy

- [Vlastnosti s podporou vazeb](~/xamarin-forms/xaml/bindable-properties.md)
- [Obory názvů jazyka XAML](~/xamarin-forms/xaml/namespaces.md)
- [Efektem stínování (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](xref:Xamarin.Forms.BindableProperty)
- [BindableObject](xref:Xamarin.Forms.BindableObject)
