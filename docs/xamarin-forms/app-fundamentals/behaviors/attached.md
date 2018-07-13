---
title: Připojená chování
description: Připojená chování jsou statické třídy s jeden nebo více připojené vlastnosti. Tento článek ukazuje, jak vytvářet a využívat připojená chování.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 2c9bd9ad4e7572b9eae6f0073da8a2c8f1e7c9fc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995343"
---
# <a name="attached-behaviors"></a>Připojená chování

_Připojená chování jsou statické třídy s jeden nebo více připojené vlastnosti. Tento článek ukazuje, jak vytvářet a využívat připojená chování._

## <a name="overview"></a>Přehled

Připojená vlastnost je zvláštní druh vázanou vlastnost. Jsou definovány v jedné třídy, ale připojené k jiné objekty, a jsou rozpoznatelné v XAML jako atributy, které obsahují třídy a názvu vlastnosti oddělené tečkou.

Můžete definovat připojené vlastnosti `propertyChanged` delegáta, který se spustí při změně hodnoty vlastnosti, například když je vlastnost nastavena na ovládacím prvku. Když `propertyChanged` spouští delegáta, je jí předán odkaz na ovládací prvek, na kterém je připojovaný a parametry, které obsahují staré a nové hodnoty pro vlastnost. Tento delegát je možné přidat nové funkce do ovládacího prvku, který je vlastnost připojen k manipulací odkaz, který je předán, následujícím způsobem:

1. `propertyChanged` Delegáta přetypování odkaz na ovládací prvek, který bude přijata jako [ `BindableObject` ](xref:Xamarin.Forms.BindableObject), pro typ ovládacího prvku, který je chování navržen k zvýšení.
1. `propertyChanged` Delegáta upraví vlastnosti ovládacího prvku, volání metod ovládací prvek nebo registrech obslužné rutiny událostí pro události, které jsou vystavené ovládacího prvku k implementaci základní chování funkce.

Problém s připojená chování je, že jsou definovány v `static` třídy s `static` vlastnosti a metody. To je těžké vytvořit připojená chování, které mají stav. Kromě toho chování Xamarin.Forms nahradil připojená chování jako upřednostňovaný způsob konstrukce chování. Další informace o chování Xamarin.Forms, naleznete v tématu [chování Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md) a [opakovaně použitelného chování](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

## <a name="creating-an-attached-behavior"></a>Vytváření připojená chování

Ukázkové aplikaci ukazuje `NumericValidationBehavior`, hodnota zadaného uživatelem, na kterém se dozvíte [ `Entry` ](xref:Xamarin.Forms.Entry) řídit červeně, pokud není `double`. Chování můžete vidět v následujícím příkladu kódu:

```csharp
public static class NumericValidationBehavior
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached (
            "AttachBehavior",
            typeof(bool),
            typeof(NumericValidationBehavior),
            false,
            propertyChanged:OnAttachBehaviorChanged);

    public static bool GetAttachBehavior (BindableObject view)
    {
        return (bool)view.GetValue (AttachBehaviorProperty);
    }

    public static void SetAttachBehavior (BindableObject view, bool value)
    {
        view.SetValue (AttachBehaviorProperty, value);
    }

    static void OnAttachBehaviorChanged (BindableObject view, object oldValue, object newValue)
    {
        var entry = view as Entry;
        if (entry == null) {
            return;
        }

        bool attachBehavior = (bool)newValue;
        if (attachBehavior) {
            entry.TextChanged += OnEntryTextChanged;
        } else {
            entry.TextChanged -= OnEntryTextChanged;
        }
    }

    static void OnEntryTextChanged (object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior` Třída obsahuje připojené vlastnosti s názvem `AttachBehavior` s `static` metody getter a setter, který řídí přidání nebo odebrání vlastnosti do ovládacího prvku, k níž bude připojen. To připojené vlastnosti registrů `OnAttachBehaviorChanged` metodu, která se provede při změně hodnoty vlastnosti. Tato metoda zaregistruje nebo zrušení zaregistruje obslužná rutina události [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) události v závislosti na hodnotě `AttachBehavior` přidružená vlastnost. Poskytuje základní funkce chování `OnEntryTextChanged` metodu, která analyzuje hodnoty zadané do [ `Entry` ](xref:Xamarin.Forms.Entry) podle uživatele a nastaví `TextColor` vlastnost na hodnotu red, pokud hodnota není `double`.

## <a name="consuming-an-attached-behavior"></a>Využívání připojená chování

`NumericValidationBehavior` Třídy mohou být spotřebovány přidání `AttachBehavior` nastavte na připojený [ `Entry` ](xref:Xamarin.Forms.Entry) řídit, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

Ekvivalent [ `Entry` ](xref:Xamarin.Forms.Entry) v jazyce C# je znázorněno v následujícím příkladu kódu:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

Za běhu chování odpoví na interakci ze strany ovládacího prvku, podle chování implementace. Na následujících snímcích obrazovky ukazují připojená chování reakce na neplatný vstup:

[![](attached-images/screenshots-sml.png "Ukázková aplikace s připojenými chování")](attached-images/screenshots.png#lightbox "ukázková aplikace s připojenými chování")

> [!NOTE]
> Připojená chování, které jsou určeny pro konkrétní ovládací prvek typu (nebo nadřazené třídy, které můžete použít pro mnoho ovládacích prvků) a měly by být pouze přidány do ovládacího prvku kompatibilní. Pokusu o připojení chování ovládacího prvku nekompatibilní bude mít za následek Neznámý chování a závisí na implementaci chování.

### <a name="removing-an-attached-behavior-from-a-control"></a>Připojená chování odebrání ovládacího prvku

`NumericValidationBehavior` Třídy lze odebrat pomocí ovládacího prvku tak, že nastavíte `AttachBehavior` připojené vlastnosti `false`, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

Ekvivalent [ `Entry` ](xref:Xamarin.Forms.Entry) v jazyce C# je znázorněno v následujícím příkladu kódu:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

V době běhu `OnAttachBehaviorChanged` metoda bude spuštěna při hodnotu `AttachBehavior` připojená vlastnost nastavená na `false`. `OnAttachBehaviorChanged` Metody se pak rušit registraci obslužné rutiny události pro [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) událostí, zajišťuje, že chování není spuštěn během interakce uživatele s ovládacím prvkem.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak vytvářet a využívat připojená chování. Připojená chování jsou `static` tříd pomocí jednoho nebo více připojené vlastnosti.


## <a name="related-links"></a>Související odkazy

- [Připojená chování (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
