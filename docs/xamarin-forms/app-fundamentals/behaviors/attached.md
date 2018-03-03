---
title: "Připojené chování"
description: "Připojené chování jsou statické třídy s jeden nebo více přidružené vlastnosti. Tento článek ukazuje, jak vytvářet a využívat připojené chování."
ms.topic: article
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 9751b39987819428f93e09d4bfb6bee261604bb5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="attached-behaviors"></a>Připojené chování

_Připojené chování jsou statické třídy s jeden nebo více přidružené vlastnosti. Tento článek ukazuje, jak vytvářet a využívat připojené chování._

## <a name="overview"></a>Přehled

– Přidružená vlastnost je zvláštní druh vazbu vlastnosti. Jsou definované v jedné třídy, ale připojené k jiné objekty, a jsou rozpoznatelném v jazyce XAML jako atributy, které obsahují třídy a název vlastnosti odděleny tečkou.

– Přidružená vlastnost můžete definovat `propertyChanged` delegáta, který bude proveden při změně hodnoty vlastnosti, například když je vlastnost nastavena na ovládacího prvku. Když `propertyChanged` provede delegáta, úspěšně prošel odkaz na ovládací prvek, na kterém je připojovaný a parametry, které obsahují staré a nové hodnoty pro vlastnost. Tento delegát slouží k přidání nových funkcí do ovládacího prvku, který vlastnost je připojen k manipulací odkaz, který je předán, následujícím způsobem:

1. `propertyChanged` Delegáta vrhá odkaz ovládací prvek, který přijal jako [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)do typ ovládacího prvku toto chování je navržený tak, aby vylepšit.
1. `propertyChanged` Delegáta upraví vlastnosti ovládacího prvku, volání metod řízení nebo registry obslužné rutiny událostí pro události, které jsou vystavené ovládací prvek, k implementaci základní funkce chování.

Problém s připojené chování je, že jsou definovány v `static` třída, s `static` vlastnosti a metody. Díky tomu je obtížné vytvořit připojené chování, které mají stav. Kromě toho Xamarin.Forms chování nahradit připojené chování jako upřednostňovaný přístup k vytváření chování. Další informace o chování Xamarin.Forms najdete v tématu [Xamarin.Forms chování](~/xamarin-forms/app-fundamentals/behaviors/creating.md) a [opakovaně použitelného chování](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

## <a name="creating-an-attached-behavior"></a>Vytvoření připojených chování

Představuje ukázkovou aplikaci `NumericValidationBehavior`, který označuje hodnota zadaná uživatelem do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řízení červeně, pokud není `double`. Chování je znázorněno v následujícím příkladu kódu:

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

`NumericValidationBehavior` Třída obsahuje přidružená vlastnost s názvem `AttachBehavior` s `static` metody getter a setter, který řídí přidání nebo odebrání chování k ovládacímu prvku, k níž bude připojen. To připojené vlastnost registrů `OnAttachBehaviorChanged` metoda, která bude proveden při změně hodnoty vlastnosti. Tato metoda registruje nebo zrušte zaregistruje obslužné rutiny události pro [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) události v závislosti na hodnotě `AttachBehavior` přidružená vlastnost. Poskytuje základní funkce služby chování `OnEntryTextChanged` metoda, která analyzuje zadané hodnoty [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) uživatelem, a nastaví `TextColor` vlastnosti tak, aby červená, pokud není hodnota `double`.

## <a name="consuming-an-attached-behavior"></a>Využívání připojených chování

`NumericValidationBehavior` Třídy mohou být spotřebovávána přidání `AttachBehavior` připojené vlastnosti tak, aby [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řídit, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

Ekvivalent [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) v jazyce C# je znázorněno v následujícím příkladu kódu:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

V době běhu chování bude reagovat na interakci s ovládacím prvkem podle chování implementace. Tyto snímky obrazovky ukazují připojené chování neodpovídá na požadavky neplatný vstup:

[ ![](attached-images/screenshots-sml.png "Ukázkové aplikace s připojené chování")](attached-images/screenshots.png "ukázkové aplikace s připojené chování")

> [!NOTE]
> **Poznámka:**: připojené chování jsou určeny pro konkrétní ovládací prvek typu (nebo nadřazenou třídu, která můžete použít pro mnoho ovládací prvky) a musí být pouze přidaní do ovládacího prvku kompatibilní. Probíhá pokus o připojení chování do ovládacího prvku nekompatibilní výsledkem bude Neznámý chování a závisí na implementaci chování.

### <a name="removing-an-attached-behavior-from-a-control"></a>Odebrání připojené chování z ovládacího prvku

`NumericValidationBehavior` – Třída může být odebrán z ovládacího prvku nastavením `AttachBehavior` přidružená vlastnost k `false`, jak je znázorněno v následujícím příkladu kódu XAML:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

Ekvivalent [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) v jazyce C# je znázorněno v následujícím příkladu kódu:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

V době běhu `OnAttachBehaviorChanged` metoda bude spuštěna při hodnotu `AttachBehavior` je připojená vlastnost nastavená na `false`. `OnAttachBehaviorChanged` Metoda bude poté zrušte registraci obslužné rutiny události pro [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) událostí, zajistíte, že není chování provést, protože uživatel pracuje s ovládacím prvkem.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak vytvářet a využívat připojené chování. Jsou připojené chování `static` tříd pomocí jednoho nebo více přidružené vlastnosti.


## <a name="related-links"></a>Související odkazy

- [Připojené chování (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
