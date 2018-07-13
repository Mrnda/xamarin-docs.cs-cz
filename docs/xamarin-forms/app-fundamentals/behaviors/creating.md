---
title: Xamarin.Forms Behaviors
description: Chování Xamarin.Forms vytvářejí odvozený od chování nebo chování<T> třídy. Tento článek ukazuje, jak vytvářet a využívat chování Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 7e057567ec0bb72e9bcc016d4a9fef3af78a3ea1
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998892"
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms Behaviors

_Chování Xamarin.Forms vytvářejí odvozený od chování nebo chování<T> třídy. Tento článek ukazuje, jak vytvářet a využívat chování Xamarin.Forms._

## <a name="overview"></a>Přehled

Proces vytvoření chování Xamarin.Forms vypadá takto:

1. Vytvořte třídu, která dědí z [ `Behavior` ](xref:Xamarin.Forms.Behavior) nebo [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) třídy, ve kterém `T` je typ ovládacího prvku, ke kterému by se měly používat chování.
1. Přepsat [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) metodu za účelem musí něco nastavovat.
1. Přepsat [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) možností, jak provést všechny potřebné vyčištění.
1. Implementace základní funkce chování.

Výsledkem je struktura je znázorněno v následujícím příkladu kódu:

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

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Metody se spustí ihned po chování je připojen k ovládacímu prvku. Tato metoda přijímá odkaz na ovládací prvek, ke kterému je připojený a je možné registrovat obslužné rutiny událostí nebo provádění jiných instalační program, který je potřeba pro podporu funkcí chování. Například je může přihlásit k odběru události v ovládacím prvku. Chování funkce by následně implementované v obslužné rutině události pro událost.

[ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Metoda je aktivována, když chování se odebere z ovládacího prvku. Tato metoda přijímá odkaz na ovládací prvek, ke kterému je připojený a slouží k provádění všechny potřebné vyčištění. Například může zrušit odběr události na ovládací prvek pro zabránění únikům paměti.

Chování může být potom používán připojením na [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) kolekce příslušný ovládací prvek.

## <a name="creating-a-xamarinforms-behavior"></a>Vytvoření chování Xamarin.Forms

Ukázkové aplikaci ukazuje `NumericValidationBehavior`, hodnota zadaného uživatelem, na kterém se dozvíte [ `Entry` ](xref:Xamarin.Forms.Entry) řídit červeně, pokud není `double`. Chování můžete vidět v následujícím příkladu kódu:

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

`NumericValidationBehavior` Je odvozen od [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) třídy, kde `T` je [ `Entry` ](xref:Xamarin.Forms.Entry). [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Metoda registruje obslužná rutina události [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) události, s [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) metodu zrušení registrace `TextChanged`nevracení událostí k zabránění paměti. Poskytuje základní funkce chování `OnEntryTextChanged` metodu, která analyzuje zadaná uživatelem do hodnota `Entry`a nastaví [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) vlastnost na hodnotu red, pokud hodnota není `double`.

> [!NOTE]
> Xamarin.Forms nenastaví `BindingContext` chování, protože se chování lze sdílet a použít pro více ovládacích prvků prostřednictvím stylů.

## <a name="consuming-a-xamarinforms-behavior"></a>Využívání chování Xamarin.Forms

Každý ovládací prvek Xamarin.Forms [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) kolekce, do kterého je jeden nebo více objektů přidat, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

Ekvivalent [ `Entry` ](xref:Xamarin.Forms.Entry) v jazyce C# je znázorněno v následujícím příkladu kódu:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

Za běhu chování odpoví na interakci ze strany ovládacího prvku, podle chování implementace. Na následujících snímcích obrazovky ukazují chování reakce na neplatný vstup:

[![](creating-images/screenshots-sml.png "Ukázková aplikace s Xamarin.Forms chování")](creating-images/screenshots.png#lightbox "ukázkovou aplikaci s chování Xamarin.Forms")

> [!NOTE]
> Chování, které jsou určeny pro konkrétní ovládací prvek typu (nebo nadřazené třídy, které můžete použít pro mnoho ovládacích prvků) a měly by být pouze přidány do ovládacího prvku kompatibilní. Výsledkem pokusu o připojení chování ovládacího prvku nekompatibilní bude k vyvolání výjimky.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Využívání chování Xamarin.Forms pomocí stylu

Chování mohou být spotřebovány explicitní nebo implicitní styl. Nicméně vytváření styl, který nastaví [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) vlastnost ovládacího prvku není možné, protože vlastnost je jen pro čtení. Řešením je přidání připojené vlastnosti do třídy chování, které řídí přidávání a odebírání chování. Probíhá takto:

1. Přidání připojené vlastnosti do třídy chování, která se dá používat k ovládání přidání nebo odebrání vlastnosti do ovládacího prvku do kterého bude chování připojené. Ujistěte se, že připojená vlastnost zaregistruje `propertyChanged` delegáta, který se spustí při změně hodnoty vlastnosti.
1. Vytvoření `static` metody getter a setter připojené vlastnosti.
1. Implementovat logiku `propertyChanged` delegáta k přidání a odebrání chování.

Následující příklad kódu ukazuje připojené vlastnosti, které řídí přidávání a odebírání `NumericValidationBehavior`:

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

`NumericValidationBehavior` Třída obsahuje připojené vlastnosti s názvem `AttachBehavior` s `static` metody getter a setter, který řídí přidání nebo odebrání vlastnosti do ovládacího prvku, k níž bude připojen. To připojené vlastnosti registrů `OnAttachBehaviorChanged` metodu, která se provede při změně hodnoty vlastnosti. Tato metoda přidá nebo odebere chování ovládacího prvku, v závislosti na hodnotě `AttachBehavior` přidružená vlastnost.

Následující příklad kódu ukazuje *explicitní* stylu pro `NumericValidationBehavior` , která používá `AttachBehavior` připojené vlastnosti a který můžete použít k [ `Entry` ](xref:Xamarin.Forms.Entry) ovládacích prvků:

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

[ `Style` ](xref:Xamarin.Forms.Style) Lze použít u [ `Entry` ](xref:Xamarin.Forms.Entry) ovládacího prvku tak, že nastavíte její [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnost `Style` instance pomocí `StaticResource` – rozšíření značek, jak je ukázáno v následujícím příkladu kódu:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Další informace o stylech najdete v tématu [styly](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> Když přidáte umožňujících vazbu vlastnosti na chování, které je nastavit nebo dotazovat v XAML, pokud vytvoříte chování, které mají stav nesmí sdílet mezi ovládacími prvky v `Style` v `ResourceDictionary`.

### <a name="removing-a-behavior-from-a-control"></a>Odebrání chování ovládacího prvku

[ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Metoda je aktivována, když se odebere z ovládacího prvku chování a umožňuje provádět všechny potřebné vyčištění, jako je například odpovězeno událost, aby se zabránilo nevrácení paměti. Ale chování se neodeberou implicitně z ovládacích prvků, není-li ovládacího prvku [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) kolekce se mění podle `Remove` nebo `Clear` metody. Následující příklad kódu ukazuje odstranění konkrétní chování pomocí ovládacího prvku `Behaviors` kolekce:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Alternativně ovládacího prvku [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) kolekci lze vymazat, jak je ukázáno v následujícím příkladu kódu:

```csharp
entry.Behaviors.Clear();
```

Navíc si všimněte, že chování se neodeberou implicitně z ovládacích prvků při stránky jsou odebrány z navigační zásobník. Místo toho je nutné explicitně odebrat před stránky odcházející z oboru.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak vytvářet a využívat chování Xamarin.Forms. Chování Xamarin.Forms vytvářejí odvozený od [ `Behavior` ](xref:Xamarin.Forms.Behavior) nebo [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) třídy.


## <a name="related-links"></a>Související odkazy

- [Chování Xamarin.Forms (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Chování Xamarin.Forms použitá stylem (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [Chování](xref:Xamarin.Forms.Behavior)
- [Chování<T>](xref:Xamarin.Forms.Behavior`1)
