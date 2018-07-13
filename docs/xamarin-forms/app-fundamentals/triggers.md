---
title: Triggery Xamarin.Forms
description: Tento článek vysvětluje, jak reagovat na změny uživatelského rozhraní s XAML pomocí Xamarin.Forms aktivační události. Aktivační události umožňují express akce deklarativně v XAML, které se mění vzhled ovládacích prvků na základě události nebo změny vlastností.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 954a0967e034e0321964e12ca0725ae2a85e3bc6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995534"
---
# <a name="xamarinforms-triggers"></a>Triggery Xamarin.Forms

Aktivační události umožňují express akce deklarativně v XAML, které se mění vzhled ovládacích prvků na základě události nebo změny vlastností.

Můžete přiřadit aktivační události přímo do ovládacího prvku, nebo ho přidejte do slovníku prostředků na úrovni stránky nebo aplikace použít více ovládacích prvků.

Existují čtyři typy triggerů:

* [Aktivační procedura vlastností](#property) -nastane, pokud je vlastnost v ovládacím prvku nastavena na jednu konkrétní hodnotu.

* [Aktivační událost data](#data) - využívá datové vazby k aktivační události na základě vlastností jiný ovládací prvek.

* [Aktivační procedura událostí](#event) -nastane, pokud dojde k události na ovládacím prvku.

* [Aktivační událost s více](#multi) – umožňuje více podmínek aktivační události vyvolané před akcí nastavit.

<a name="property" />

## <a name="property-triggers"></a>Aktivační procedury vlastností

Jednoduché aktivační událost může být vyjádřena čistě v XAML, přidávání `Trigger` spustí element do ovládacího prvku kolekce.
Tento příklad ukazuje aktivační událost, která se mění `Entry` barva pozadí, když přijme zaměření:

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
             Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Důležité části deklarace aktivační události jsou:

* **TargetType** – typ ovládacího prvku, který se aktivační událost se vztahuje na.

* **Vlastnost** – vlastnost v ovládacím prvku, který je monitorován.

* **Hodnota** -hodnotu, pokud dojde k sledované vlastnosti, který způsobí, že aktivační událost aktivovat.

* **Metoda setter** – kolekce `Setter` elementy lze přidat, a pokud je splněna podmínka aktivace. Je nutné zadat `Property` a `Value` nastavení.

* **Funkce EnterActions a ExitActions** (není vidět) - jsou napsané v kódu a je možné kromě (nebo namísto něj) `Setter` elementy. Jsou [popisovaném](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Použití aktivační události pomocí stylu

Triggery se dají přidat i do `Style` deklarace na ovládací prvek, na stránce nebo aplikaci `ResourceDictionary`. V tomto příkladu deklaruje implicitní styl (tj. žádné `Key` nastavená) což znamená, že budou vztahovat na všechny `Entry` ovládacích prvků na stránce.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

<a name="data" />

## <a name="data-triggers"></a>Aktivační události dat

Aktivační události dat používat datové vazby k monitorování způsobí jiný ovládací prvek `Setter`s zavolána. Místo `Property` atribut aktivační události vlastnost, nastavte `Binding` atribut monitorování pro zadanou hodnotu.

Následující příklad používá syntaxe vazby dat `{Binding Source={x:Reference entry}, Path=Text.Length}` tedy jak označujeme vlastností ovládacího prvku. Když délka `entry` je nula, trigger se aktivuje. V této ukázce se aktivační událost zakáže tlačítko při vstupu je prázdný.

```xaml
<!-- the x:Name is referenced below in DataTrigger-->
<!-- tip: make sure to set the Text="" (or some other default) -->
<Entry x:Name="entry"
       Text=""
       Placeholder="required field" />

<Button x:Name="button" Text="Save"
        FontSize="Large"
        HorizontalOptions="Center">
    <Button.Triggers>
        <DataTrigger TargetType="Button"
                     Binding="{Binding Source={x:Reference entry},
                                       Path=Text.Length}"
                     Value="0">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </Button.Triggers>
</Button>
```

Tip: při vyhodnocování `Path=Text.Length` vždycky zadat výchozí hodnotu pro vlastnost target (např.) `Text=""`) vzhledem k tomu, v opačném případě bude `null` a aktivační události nebude fungovat podle očekávání.

Kromě zadání `Setter`s můžete také zadat [ `EnterActions` a `ExitActions` ](#enterexit).

<a name="event" />

## <a name="event-triggers"></a>Aktivační události

`EventTrigger` Element vyžaduje pouze `Event` vlastnosti, jako například `"Clicked"` v následujícím příkladu.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Všimněte si, že neexistují žádné `Setter` elementy, ale místo toho odkaz na třídu definované `local:NumericValidationTriggerAction` vyžadujícího `xmlns:local` deklarovat na stránce vaší XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Implementuje vlastní třídy `TriggerAction` což znamená, že by měly poskytnout přepsání `Invoke` metodu, která je volána pokaždé, když nastane aktivační událost.

By měla implementace akce aktivační události:

* Implementovat obecný `TriggerAction<T>` třídy s odpovídající typ ovládacího prvku se aktivační událost se použijí pro obecný parametr. Nadřazených tříd můžete použít například `VisualElement` pro zápis akce aktivační události, které pracuje s mnoha ovládacích prvků, nebo zadat typ ovládacího prvku, jako jsou `Entry`.

* Přepsat `Invoke` – to je volána metoda pokaždé, když se splní kritéria aktivační události.

* Volitelně můžete vystavit vlastnosti, které je možné nastavit v XAML při deklaraci aktivační událost (například `Anchor`, `Scale`, a `Length` v tomto příkladu).

```csharp
public class NumericValidationTriggerAction : TriggerAction<Entry>
{
    protected override void Invoke (Entry entry)
    {
        double result;
        bool isValid = Double.TryParse (entry.Text, out result);
        entry.TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Vlastností vystavovaných třídami aktivační událost lze nastavit v deklaraci XAML následujícím způsobem:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Buďte opatrní při sdílení aktivační události v `ResourceDictionary`, jedna instance bude sdílena mezi ovládací prvky tak jakýkoli stav, který je nakonfigurovaný jednou budou platit pro všechny.

Všimněte si, že aktivační události nepodporují `EnterActions` a `ExitActions` [popisovaném](#enterexit).    

<a name="multi" />

## <a name="multi-triggers"></a>Více aktivačních událostí

A `MultiTrigger` vypadá podobně jako `Trigger` nebo `DataTrigger` s tím rozdílem, může být více než jednu podmínku. Všechny podmínky musí být splněny, než `Setter`s spuštěná.

Tady je příklad aktivační události pro tlačítko s vazbou na dva různé vstupy (`email` a `phone`):

```xaml
<MultiTrigger TargetType="Button">
    <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference email},
                                   Path=Text.Length}"
                               Value="0" />
        <BindingCondition Binding="{Binding Source={x:Reference phone},
                                   Path=Text.Length}"
                               Value="0" />
    </MultiTrigger.Conditions>

  <Setter Property="IsEnabled" Value="False" />
    <!-- multiple Setter elements are allowed -->
</MultiTrigger>
```

`Conditions` Kolekce může také obsahovat `PropertyCondition` prvky tímto způsobem:

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>Vytváření aktivační události s více "požadovat vše"

Aktivační událost s více pouze aktualizuje ovládacího prvku, když jsou splněné všechny podmínky. Testování pro "všechny pole délky mají hodnotu nula" (jako je například přihlašovací stránku, kde všechny vstupy musí být úplný) je velmi obtížné, protože má podmínku "kde Text.Length > 0", ale to nelze vyjádřen v XAML.

To můžete udělat pomocí `IValueConverter`. Převaděč kódu níže transformace `Text.Length` vazby do `bool` , která označuje, zda je pole prázdné:


```csharp
public class MultiTriggerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        if ((int)value > 0) // length > 0 ?
            return true;            // some data has been entered
        else
            return false;            // input is empty
    }

    public object ConvertBack(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        throw new NotSupportedException ();
    }
}
```

Pokud chcete použít tento převaděč v triggeru s více, přidejte je nejdříve do slovníku prostředků na stránce (spolu s vlastní `xmlns:local` definice oboru názvů):

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

XAML najdete níž. Mějte na paměti následující rozdíly proti v prvním příkladu aktivační událost s více:

* Tlačítko má `IsEnabled="false"` ve výchozím nastavení.
* Podmínky aktivace více zapnout pomocí tohoto převaděče `Text.Length` hodnotu na hodnotu typu boolean.
* Když jsou všechny podmínky `true`, Metoda setter je tlačítka `IsEnabled` vlastnost `true`.

```xaml
<Entry x:Name="user" Text="" Placeholder="user name" />

<Entry x:Name="pwd" Text="" Placeholder="password" />

<Button x:Name="loginButton" Text="Login"
        FontSize="Large"
        HorizontalOptions="Center"
        IsEnabled="false">
  <Button.Triggers>
    <MultiTrigger TargetType="Button">
      <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference user},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
        <BindingCondition Binding="{Binding Source={x:Reference pwd},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
      </MultiTrigger.Conditions>
      <Setter Property="IsEnabled" Value="True" />
    </MultiTrigger>
  </Button.Triggers>
</Button>
```

Tyto snímky obrazovky ukazují rozdíl mezi dvěma více aktivační událost výše uvedených příkladech. V horní části obrazovky zadejte text jen v jedné `Entry` je, aby bylo možné povolit **Uložit** tlačítko.
V dolní části obrazovky **přihlášení** tlačítko zůstane neaktivní, dokud obě pole obsahovat data.


![](triggers-images/multi-requireall.png "MultiTrigger příklady")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>Funkce EnterActions a ExitActions

Dalším způsobem, jak implementovat změny, když dojde k aktivační události je tak, že přidáte `EnterActions` a `ExitActions` kolekce a určení `TriggerAction<T>` implementace.

Můžete zadat *obě* `EnterActions` a `ExitActions` stejně jako `Setter`s aktivační události, ale mějte na paměti, která `Setter`s se nazývají okamžitě (jejich nechcete čekat `EnterAction` nebo `ExitAction` do dokončení). Případně můžete provádět vše, co v kódu a nepoužívat `Setter`s vůbec.

```xaml
<Entry Placeholder="enter job title">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="Entry.IsFocused" Value="True">
            <Trigger.EnterActions>
                <local:FadeTriggerAction StartsFrom="0"" />
            </Trigger.EnterActions>

            <Trigger.ExitActions>
                <local:FadeTriggerAction StartsFrom="1" />
            </Trigger.ExitActions>
                        <!-- You can use both Enter/Exit and Setter together if required -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Jako vždy, když třída odkazuje v XAML by měla deklarovat oboru názvů, jako `xmlns:local` jak je znázorněno zde:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

`FadeTriggerAction` Kód je uveden níže:

```csharp
public class FadeTriggerAction : TriggerAction<VisualElement>
{
    public FadeTriggerAction() {}

    public int StartsFrom { set; get; }

    protected override void Invoke (VisualElement visual)
    {
            visual.Animate("", new Animation( (d)=>{
                var val = StartsFrom==1 ? d : 1-d;
                visual.BackgroundColor = Color.FromRgb(1, val, 1);

            }),
            length:1000, // milliseconds
            easing: Easing.Linear);
    }
}
```

Poznámka: `EnterActions` a `ExitActions` se ignorují u **aktivačních procedur událostí**.



## <a name="related-links"></a>Související odkazy

- [Ukázka aktivační události](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Dokumentace k rozhraní Xamarin.Forms API](xref:Xamarin.Forms.TriggerAction`1)
