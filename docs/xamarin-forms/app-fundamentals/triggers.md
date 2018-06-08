---
title: Aktivační procedury
description: Reakce na změny uživatelského rozhraní s XAML
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: af5912736e2a2bd7d3347d4aa199faa3fdfe41c7
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846442"
---
# <a name="triggers"></a>Aktivační procedury

Aktivační události umožňují express akce deklarativně v jazyce XAML, které mění vzhled ovládací prvky založené na události nebo změny vlastností.

Můžete přiřadit aktivační událost přímo do ovládacího prvku, nebo ho přidat do slovník prostředků úrovni stránky nebo aplikace má být použita pro více ovládacích prvků.

Existují čtyři typy aktivační události:

* [Vlastnost aktivační událost](#property) -nastane, když vlastnost v ovládacím prvku je nastavená na určitou hodnotu.

* [Aktivační událost data](#data) – používá datové vazby k aktivační události na základě vlastností jiného ovládacího prvku.

* [Aktivační událost](#event) -nastane, když dojde k události v ovládacím prvku.

* [Aktivační událost více](#multi) -umožňuje více podmínky aktivace nastavit předtím, než dojde k akci.

<a name="property" />

## <a name="property-triggers"></a>Vlastnost aktivační události

Jednoduché aktivační událost může být vyjádřený výhradně v jazyce XAML, přidávání `Trigger` aktivuje element do ovládacího prvku kolekce.
Tento příklad ukazuje aktivační událost, která se změní `Entry` barva pozadí, když obdrží fokus:

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

Důležitou součástí deklarace aktivační události jsou:

* **TargetType** – typ ovládacího prvku, který má aktivační procedura se vztahuje na.

* **Vlastnost** -vlastnost u prvku, který je monitorován.

* **Hodnota** -hodnota případě monitorovaných vlastnosti, která způsobuje, aktivační událost aktivovat.

* **Metoda setter** -kolekce `Setter` elementy lze přidat, a pokud je splněna podmínka aktivace. Je nutné zadat `Property` a `Value` nastavit.

* **EnterActions a ExitActions** (není vidět) - jsou napsané v kódu a je možné použít kromě (nebo místo) `Setter` elementy. Jsou [popsané dál](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Použití aktivační událost pomocí stylu

Aktivační události lze také přidat do `Style` deklarace ovládacího prvku v stránky nebo aplikace v `ResourceDictionary`. Tento příklad deklaruje implicitní styl (ie. žádné `Key` nastavena) což znamená, že budou platit pro všechny `Entry` ovládací prvky na stránce.

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

Aktivační události data použít k monitorování další ovládací prvek způsobí datová vazba `Setter`s získat volána. Místo `Property` atribut v aktivační události vlastnost, nastavte `Binding` atribut monitorování pro zadanou hodnotu.

Následující příklad používá syntaxe vazby dat `{Binding Source={x:Reference entry}, Path=Text.Length}` tedy jak označujeme na další ovládací prvek vlastnosti. Když délka `entry` rovná nule, aktivační událost je aktivována. V této ukázce aktivační událost zakáže tlačítko při vstupu je prázdný.

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

Tip: při vyhodnocování `Path=Text.Length` vždy zadejte výchozí hodnotu pro vlastnost target (např. `Text=""`) protože jinak bude mít `null` a aktivační události nebude fungovat podle očekávání.

Kromě určení `Setter`s můžete zadat taky [ `EnterActions` a `ExitActions` ](#enterexit).

<a name="event" />

## <a name="event-triggers"></a>Aktivační události

`EventTrigger` Prvek vyžaduje pouze `Event` vlastnosti, jako například `"Clicked"` v následujícím příkladu.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Všimněte si, že neexistují žádné `Setter` elementů, ale místo odkazu na třídu definované `local:NumericValidationTriggerAction` což vyžaduje, aby `xmlns:local` deklarovat na stránce je XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Implementuje vlastní třídy `TriggerAction` což znamená, že by měl poskytovat přepsání pro `Invoke` metoda, která je volána, když dojde k aktivační události.

Na aktivační událost akce implementace proveďte následující kroky:

* Implementovat Obecné `TriggerAction<T>` třídě, obecný parametr odpovídající s typem aktivační událost se použijí pro ovládací prvek. Můžete například použít nadřazených tříd `VisualElement` k zápisu akce aktivační události, které pracovat s řadu ovládacích prvků, nebo zadat jako typ ovládacího prvku `Entry`.

* Přepsání `Invoke` metodu – to je volána, když se splní kritéria aktivační události.

* Volitelně vystavení vlastností, které lze nastavit v XAML při deklaraci aktivační události (například `Anchor`, `Scale`, a `Length` v tomto příkladu).

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

Vlastnosti vystavené prostřednictvím akce aktivace můžete nastavit v deklaraci XAML následujícím způsobem:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Buďte opatrní při sdílení aktivační události v `ResourceDictionary`, jedna instance bude sdílena mezi ovládací prvky, takže nějaký stav, který je nakonfigurovaný jednou budou platit pro všechny.

Všimněte si, že aktivační události nepodporují `EnterActions` a `ExitActions` [popsané dál](#enterexit).    

<a name="multi" />

## <a name="multi-triggers"></a>Více aktivačních událostí

A `MultiTrigger` bude vypadat podobně jako `Trigger` nebo `DataTrigger` s tím rozdílem, může být víc než jednu podmínku. Všechny podmínky musí být splněné před `Setter`s aktivaci.

Tady je příklad aktivační událost pro tlačítko s vazbou na dvou různých vstupy (`email` a `phone`):

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

`Conditions` Kolekce může obsahovat také `PropertyCondition` prvky, jako tento:

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>Vytváření aktivační události více "požadovat všechny"

Aktivační událost více pouze aktualizuje jeho řízení, pokud jsou splněny všechny podmínky. Testování pro "všechny délky pole jsou nula" (například přihlašovací stránku, kde musí být všechny vstupy dokončení) je složité, protože chcete, aby podmínku "kde Text.Length > 0", ale to není možné vyjádřit v jazyce XAML.

To lze provést pomocí `IValueConverter`. Převaděč kódu níže transformace `Text.Length` vazby do `bool` určující, zda pole je prázdné nebo není:


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

Pokud chcete použít tento převaděč v aktivační události více, přidejte je nejdříve do slovníku prostředků stránky (spolu s vlastní `xmlns:local` definici oboru názvů):

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

XAML jsou uvedeny níže. Vezměte na vědomí následující rozdíly proti v prvním příkladu více aktivační události:

* Tlačítko má `IsEnabled="false"` ve výchozím nastavení.
* Podmínky aktivace více převaděč slouží k povolení `Text.Length` hodnotu na hodnotu typu boolean.
* Když jsou všechny podmínky `true`, nastavovací metoda umožňuje na tlačítko `IsEnabled` vlastnost `true`.

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

Tyto snímky obrazovky ukazují rozdíl mezi dvěma více aktivační událost výše uvedených příkladech. V horní části obrazovky, vstup text jen v jedné `Entry` k povolení **Uložit** tlačítko.
V dolní části obrazovky **přihlášení** tlačítko zůstane neaktivní, dokud obě pole obsahovat data.


![](triggers-images/multi-requireall.png "Příklady multiTrigger")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions a ExitActions

Jiný způsob, jak implementovat změny, když dojde k aktivační události je přidáním `EnterActions` a `ExitActions` kolekce a zadání `TriggerAction<T>` implementace.

Můžete zadat *obě* `EnterActions` a `ExitActions` a také `Setter`s v aktivační události, ale mějte na paměti, `Setter`s se nazývají okamžitě (není čekají `EnterAction` nebo `ExitAction` k Dokončete). Případně můžete provádět všechno, co v kódu a nepoužívat `Setter`s na všechny.

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

Jako vždy, když třída odkazuje v jazyce XAML by měly deklarovat obor názvů, jako `xmlns:local` jak je vidět tady:

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

Poznámka: `EnterActions` a `ExitActions` ignorují na **aktivačních událostí**.



## <a name="related-links"></a>Související odkazy

- [Ukázka aktivační události](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Dokumentace rozhraní API Xamarin.Forms](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/)
