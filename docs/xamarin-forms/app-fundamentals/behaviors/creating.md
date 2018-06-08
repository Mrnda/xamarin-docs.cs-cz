---
title: Xamarin.Forms Behaviors
description: Chování Xamarin.Forms jsou vytvořené pomocí odvozování z chování nebo chování<T> třídy. Tento článek ukazuje, jak vytvářet a využívat Xamarin.Forms chování.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 3a86e7713620eff90db995941eb35df7bc393a76
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848288"
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms Behaviors

_Chování Xamarin.Forms jsou vytvořené pomocí odvozování z chování nebo chování<T> třídy. Tento článek ukazuje, jak vytvářet a využívat Xamarin.Forms chování._

## <a name="overview"></a>Přehled

Proces vytvoření Xamarin.Forms chování je následující:

1. Vytvořte třídu, která dědí z [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) nebo [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) třídy, kde `T` je typ ovládacího prvku, ke kterému by se měly používat chování.
1. Přepsání [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) možností, jak provést všechny požadované instalace.
1. Přepsání [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) možností, jak provést jakékoli vyžaduje čištění.
1. Implementujte základní funkce služby chování.

Výsledkem je strukturu ukazuje následující příklad kódu:

```csharp
public class CustomBehavior : Behavior<View>
{
    protected override void OnAttachedTo (View bindable)
    {
        base.OnAttachedTo (bindable);
        // Perform setup
    }

    protected override void OnDetachingFrom (View bindable)
    {
        base.OnDetachingFrom (bindable);
        // Perform clean up
    }

    // Behavior implementation
}
```

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Metoda je aktivována, okamžitě po chování je připojen k ovládacímu prvku. Tato metoda přijímá odkaz na ovládací prvek, ke které je připojena a slouží k registraci obslužné rutiny událostí nebo provedení jiných instalace, které je nutné pro podporu funkcí chování. Například je může přihlásit k odběru události v ovládacím prvku. Chování funkce by pak provádějí v obslužné rutiny události pro událost.

[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Metoda je aktivována, jestliže chování se odebere z ovládacího prvku. Tato metoda přijímá odkaz na ovládací prvek, ke které je připojena a slouží k provádění všech vyžaduje čištění. Například se může odhlásit z události v ovládacím prvku, aby se zabránilo nevracení paměti.

Chování může být potom používán připojením, aby [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) kolekce vhodný ovládací prvek.

## <a name="creating-a-xamarinforms-behavior"></a>Vytváření chování Xamarin.Forms

Představuje ukázkovou aplikaci `NumericValidationBehavior`, který označuje hodnota zadaná uživatelem do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řízení červeně, pokud není `double`. Chování je znázorněno v následujícím příkladu kódu:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    protected override void OnAttachedTo(Entry entry)
    {
        entry.TextChanged += OnEntryTextChanged;
        base.OnAttachedTo(entry);
    }

    protected override void OnDetachingFrom(Entry entry)
    {
        entry.TextChanged -= OnEntryTextChanged;
        base.OnDetachingFrom(entry);
    }

    void OnEntryTextChanged(object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior` Je odvozena z [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) třídy, kde `T` je [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Metoda registruje obslužné rutiny události pro [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) událostí, s [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) metoda zrušte registraci `TextChanged`nevracení událostí, aby se zabránilo paměti. Poskytuje základní funkce služby chování `OnEntryTextChanged` metoda, která analyzuje zadaná uživatelem do hodnota `Entry`a nastaví [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) vlastnosti tak, aby červená, pokud není hodnota `double`.

> [!NOTE]
> Xamarin.Forms nenastaví `BindingContext` chování, protože chování můžete sdílet a použity na více ovládacích prvků prostřednictvím stylů.

## <a name="consuming-a-xamarinforms-behavior"></a>Využívání chování Xamarin.Forms

Každý ovládací prvek Xamarin.Forms má [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) kolekce, do které můžete jeden nebo více chování přidány, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

Ekvivalent [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) v jazyce C# je znázorněno v následujícím příkladu kódu:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

V době běhu chování bude reagovat na interakci s ovládacím prvkem podle chování implementace. Tyto snímky obrazovky ukazují chování neodpovídá na požadavky neplatný vstup:

[![](creating-images/screenshots-sml.png "Ukázkové aplikace s Xamarin.Forms chování")](creating-images/screenshots.png#lightbox "ukázkové aplikace s chováním Xamarin.Forms")

> [!NOTE]
> Chování jsou určeny pro konkrétní ovládací prvek typu (nebo nadřazenou třídu, která můžete použít pro mnoho ovládací prvky) a musí být pouze přidaní do ovládacího prvku kompatibilní. Probíhá pokus o připojení chování do ovládacího prvku nekompatibilní způsobí výjimku hlášeny.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Využívání chování Xamarin.Forms s styl

Chování mohou být spotřebovávána explicitní nebo implicitní stylu. Ale vytváření stylu, které nastavuje [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) vlastností ovládacího prvku není možné, protože vlastnost je jen pro čtení. Řešení je přidání přidružená vlastnost k třídě chování, které řídí přidávání a odebírání chování. Proces je následující:

1. Přidejte přidružená vlastnost k třídě chování, který se použije k řízení přidání nebo odebrání chování do ovládacího prvku do které bude chování připojen. Ujistěte se, že přidružená vlastnost zaregistruje `propertyChanged` delegáta, který bude proveden při změně hodnoty vlastnosti.
1. Vytvoření `static` metody getter a setter pro vlastnost připojené.
1. Implementovat logiku v `propertyChanged` delegáta k přidání a odebrání chování.

Následující příklad kódu ukazuje přidružená vlastnost, která řídí přidávání a odebírání `NumericValidationBehavior`:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached ("AttachBehavior", typeof(bool), typeof(NumericValidationBehavior), false, propertyChanged: OnAttachBehaviorChanged);

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
            entry.Behaviors.Add (new NumericValidationBehavior ());
        } else {
            var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
            if (toRemove != null) {
                entry.Behaviors.Remove (toRemove);
            }
        }
    }
    ...
}
```

`NumericValidationBehavior` Třída obsahuje přidružená vlastnost s názvem `AttachBehavior` s `static` metody getter a setter, který řídí přidání nebo odebrání chování k ovládacímu prvku, k níž bude připojen. To připojené vlastnost registrů `OnAttachBehaviorChanged` metoda, která bude proveden při změně hodnoty vlastnosti. Tato metoda přidá nebo odebere požadované chování ovládacího prvku, v závislosti na hodnotě `AttachBehavior` přidružená vlastnost.

Následující příklad kódu ukazuje *explicitní* styl pro `NumericValidationBehavior` používající `AttachBehavior` připojené vlastnosti a který můžete použít k [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ovládací prvky:

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Lze použít pro [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ovládací prvek pro nastavení jeho [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnost, která má `Style` pomocí `StaticResource` – rozšíření značek, jak je ukázáno v následujícím příkladu kódu:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Další informace o styly najdete v tématu [styly](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> Přestože můžete přidat vazbu vlastnosti na chování, které je nastavit nebo dotaz v jazyce XAML, je-li vytvořit chování, které mají stav, neměl by se sdílet mezi ovládacími prvky v `Style` v `ResourceDictionary`.

### <a name="removing-a-behavior-from-a-control"></a>Odebrání chování z ovládacího prvku

[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Metoda je aktivována, jestliže chování se odebere z ovládacího prvku a slouží k provádění požadovaných vyčištění například Odhlašujete událost, aby se zabránilo nevrácenou pamětí. Ale chování neodeberou implicitně z ovládacích prvků Pokud ovládacího prvku [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) je upraveném kolekce `Remove` nebo `Clear` metoda. Následující příklad kódu ukazuje odebráním konkrétní chování ovládacího prvku `Behaviors` kolekce:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Alternativně ovládacího prvku [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) kolekce lze vymazat, jak je ukázáno v následujícím příkladu kódu:

```csharp
entry.Behaviors.Clear();
```

Všimněte si kromě toho, že chování neodeberou implicitně z ovládacích prvků při stránky jsou odebrány ze zásobníku navigace. Místo toho se musí explicitně před odebrat stránky přejdete mimo rozsah.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak vytvářet a využívat Xamarin.Forms chování. Chování Xamarin.Forms jsou vytvořené pomocí odvozování z [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) nebo [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) třídy.


## <a name="related-links"></a>Související odkazy

- [Chování Xamarin.Forms (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Chování Xamarin.Forms použitá stylem (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [Chování](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Chování<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
