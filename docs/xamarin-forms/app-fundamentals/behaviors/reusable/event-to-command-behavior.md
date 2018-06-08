---
title: Znovu použitelné EventToCommandBehavior
description: Chování může být použito k přidružení příkazy s ovládacími prvky, které nebyly navrženy tak, aby komunikovali s příkazy. Tento článek ukazuje použití Xamarin.Forms chování k vyvolání příkazu, pokud aktivuje událost.
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: e89400c74c3d1afbf8954d0f88387c5967ebd534
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848217"
---
# <a name="reusable-eventtocommandbehavior"></a>Znovu použitelné EventToCommandBehavior

_Chování může být použito k přidružení příkazy s ovládacími prvky, které nebyly navrženy tak, aby komunikovali s příkazy. Tento článek ukazuje použití Xamarin.Forms chování k vyvolání příkazu, pokud aktivuje událost._

## <a name="overview"></a>Přehled

`EventToCommandBehavior` Třída je opakovaně použitelné vlastní chování Xamarin.Forms, který spouští příkaz v reakci na *žádné* spouštění událostí. Ve výchozím nastavení, argumenty událostí pro události se předá příkaz a může být volitelně pomocí převedeny [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) implementace.

Následující vlastnosti chování musí být nastavené použít toto chování:

- **EventName** – název události chování naslouchá.
- **Příkaz** – **ICommand** spouštění. Chování se očekává, že najít `ICommand` instance na [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) připojené ovládacího prvku, který může být dědí z nadřazeného prvku.

Následující vlastnosti volitelné chování lze také nastavit:

- **CommandParameter** – `object` , budou předány do příkazu.
- **Převaděč** – [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) implementace, které změní formát data argument události, jak se předávají mezi *zdroj* a *cíl*modul vazby.

## <a name="creating-the-behavior"></a>Vytváření chování

`EventToCommandBehavior` Třída odvozená z `BehaviorBase<T>` třídy, která naopak odvozen z [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) třída. Účelem `BehaviorBase<T>` třídy je poskytnout základní třídu pro všechny Xamarin.Forms chování, které vyžadují [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) chování být nastavena na připojené ovládacího prvku. Zajistíte tak, že chování navázat a provést `ICommand` určeným elementem `Command` vlastnost, když je zpracován chování.

`BehaviorBase<T>` Třída poskytuje přepisovatelným [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) metoda, která nastaví [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) chování a přepisovatelným [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)metoda, která vyčistí `BindingContext`. Kromě toho třída ukládá odkaz na připojené ovládacího prvku `AssociatedObject` vlastnost.

### <a name="implementing-bindable-properties"></a>Implementace vazbu vlastnosti

`EventToCommandBehavior` Třída definuje čtyři [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) při aktivuje událost definovaný instancí, které uživatel provést příkaz. Tyto vlastnosti jsou uvedeny v následující příklad kódu:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  public static readonly BindableProperty EventNameProperty =
    BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
  public static readonly BindableProperty CommandProperty =
    BindableProperty.Create ("Command", typeof(ICommand), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty CommandParameterProperty =
    BindableProperty.Create ("CommandParameter", typeof(object), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty InputConverterProperty =
    BindableProperty.Create ("Converter", typeof(IValueConverter), typeof(EventToCommandBehavior), null);

  public string EventName { ... }
  public ICommand Command { ... }
  public object CommandParameter { ... }
  public IValueConverter Converter { ...  }
  ...
}
```

Když `EventToCommandBehavior` třída zpracován, `Command` vlastnost by měla představovat data vázaná na `ICommand` mají být provedeny v reakci na spouštění událostí, která je definována v `EventName` vlastnost. Chování bude očekávali `ICommand` na [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) připojené ovládacího prvku.

Ve výchozím nastavení se předá argumenty událostí pro událost příkaz. Tato data mohou být volitelně převedeny předaných mezi *zdroj* a *cíl* stroj vazby, zadáním [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) implementaci jako `Converter` hodnotu vlastnosti. Alternativně můžete Předaný parametr k příkazu zadáním `CommandParameter` hodnotu vlastnosti.

### <a name="implementing-the-overrides"></a>Implementace přepsání

`EventToCommandBehavior` Třídy přepsání [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) a [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) metody `BehaviorBase<T>` třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  protected override void OnAttachedTo (View bindable)
  {
    base.OnAttachedTo (bindable);
    RegisterEvent (EventName);
  }

  protected override void OnDetachingFrom (View bindable)
  {
    DeregisterEvent (EventName);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Metoda provede instalaci pomocí volání `RegisterEvent` metodu předáním hodnoty `EventName` vlastnosti jako parametr. [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Metoda provádí vyčištění voláním `DeregisterEvent` metodu předáním hodnoty `EventName` vlastnosti jako parametr.

### <a name="implementing-the-behavior-functionality"></a>Implementace funkce chování

Účelem chování je možné provést příkaz definované `Command` v odezvě na spouštění událostí, který je definován `EventName` vlastnost. Základní funkce chování je znázorněno v následujícím příkladu kódu:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  void RegisterEvent (string name)
  {
    if (string.IsNullOrWhiteSpace (name)) {
      return;
    }

    EventInfo eventInfo = AssociatedObject.GetType ().GetRuntimeEvent (name);
    if (eventInfo == null) {
      throw new ArgumentException (string.Format ("EventToCommandBehavior: Can't register the '{0}' event.", EventName));
    }
    MethodInfo methodInfo = typeof(EventToCommandBehavior).GetTypeInfo ().GetDeclaredMethod ("OnEvent");
    eventHandler = methodInfo.CreateDelegate (eventInfo.EventHandlerType, this);
    eventInfo.AddEventHandler (AssociatedObject, eventHandler);
  }

  void OnEvent (object sender, object eventArgs)
  {
    if (Command == null) {
      return;
    }

    object resolvedParameter;
    if (CommandParameter != null) {
      resolvedParameter = CommandParameter;
    } else if (Converter != null) {
      resolvedParameter = Converter.Convert (eventArgs, typeof(object), null, null);
    } else {
      resolvedParameter = eventArgs;
    }        

    if (Command.CanExecute (resolvedParameter)) {
      Command.Execute (resolvedParameter);
    }
  }
  ...
}
```

`RegisterEvent` Metodu je provést v reakci na `EventToCommandBehavior` připojovaný ke ovládacího prvku a obdrží hodnotu `EventName` vlastnosti jako parametr. Metoda poté se pokusí vyhledat události definované v `EventName` vlastnost u prvku připojené. Za předpokladu, že událost může být umístěn, `OnEvent` metoda je zaregistrován jako metodu obslužné rutiny události.

`OnEvent` Metodu je provést v reakci na spouštění událostí, která je definována v `EventName` vlastnost. Za předpokladu, že `Command` odkazuje vlastnost platnou `ICommand`, metoda se pokusí načíst parametr předat `ICommand` následujícím způsobem:

- Pokud `CommandParameter` vlastnost definuje parametr, načítání.
- Jinak, pokud `Converter` definuje vlastnost [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) implementace převaděč se spustí a převede data události argumentů předaných mezi *zdroj* a *cíl* modul vazby.
- Jinak argumenty událostí se předpokládá, že jako parametr.

Data vázaná `ICommand` potom spuštěn, předávání v parametr k příkazu, za předpokladu, že [ `CanExecute` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Command.CanExecute/p/System.Object/) metoda vrátí `true`.

I když není tady, zobrazené `EventToCommandBehavior` také zahrnuje `DeregisterEvent` metoda, kterou spouští [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) metoda. `DeregisterEvent` Metoda se používá k vyhledání a zrušení registrace události definované v `EventName` vlastnost k vyčištění nevracení všechny potenciální paměti.

## <a name="consuming-the-behavior"></a>Využívání chování

`EventToCommandBehavior` Třídy je možné připojit k [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) kolekce ovládacího prvku, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<ListView ItemsSource="{Binding People}">
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}"
                                  Converter="{StaticResource SelectedItemConverter}" />
  </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

Ekvivalentní kódu C# je znázorněno v následujícím příkladu kódu:

```csharp
var listView = new ListView ();
listView.SetBinding (ItemsView<Cell>.ItemsSourceProperty, "People");
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
  Converter = new SelectedItemEventArgsToSelectedItemConverter ()
});

var selectedItemLabel = new Label ();
selectedItemLabel.SetBinding (Label.TextProperty, "SelectedItemText");
```

`Command` Vlastnost chování je data vázaná na `OutputAgeCommand` vlastnost přidružené ViewModel při `Converter` je nastavena na `SelectedItemConverter` instanci, která vrátí [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/)z [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) z [ `SelectedItemChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SelectedItemChangedEventArgs/).

V době běhu chování bude odpovídat interakci s ovládacím prvkem. Když je položka vybrána v [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) bude platit událostí, které budou spuštěny `OutputAgeCommand` v ViewModel. Naopak tím se aktualizuje ViewModel `SelectedItemText` vlastnost, [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) váže na, jak je vidět na následujících snímcích obrazovky:

[![](event-to-command-behavior-images/screenshots-sml.png "Ukázkové aplikace s EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "ukázkové aplikace s EventToCommandBehavior")

Výhodou použití toto chování k provedení příkazu, když událost se aktivuje, je, že příkazy mohou být související s ovládacími prvky, které nebyly navrženy tak, aby komunikovali s příkazy. Kromě toho tím odebere kód pro zpracování událostí kotle tabulky z soubory kódu.

## <a name="summary"></a>Souhrn

Tento článek ukázal pomocí Xamarin.Forms chování k vyvolání příkazu, pokud aktivuje událost. Chování může být použito k přidružení příkazy s ovládacími prvky, které nebyly navrženy tak, aby komunikovali s příkazy.


## <a name="related-links"></a>Související odkazy

- [Chování EventToCommand (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Chování](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Chování<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
