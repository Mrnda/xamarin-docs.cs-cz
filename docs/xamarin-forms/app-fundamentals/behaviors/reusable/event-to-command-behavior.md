---
title: Opakovaně použitelné EventToCommandBehavior
description: Chování slouží k přidružení příkazů s ovládacími prvky, které nejsou určeny k interakci s příkazy. Tento článek ukazuje použití chování Xamarin.Forms spuštění příkazu, když se aktivuje událost.
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 3151179b6ff6d26b74a87ded747310646b304603
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996318"
---
# <a name="reusable-eventtocommandbehavior"></a>Opakovaně použitelné EventToCommandBehavior

_Chování slouží k přidružení příkazů s ovládacími prvky, které nejsou určeny k interakci s příkazy. Tento článek ukazuje použití chování Xamarin.Forms spuštění příkazu, když se aktivuje událost._

## <a name="overview"></a>Přehled

`EventToCommandBehavior` Třída je opakovaně použitelné vlastní chování Xamarin.Forms, které spouští příkaz v reakci na *jakékoli* spouštění událostí. Ve výchozím nastavení, argumenty událostí pro události se předá příkazu a volitelně může být převedena [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) implementace.

Použít toto chování, musí být nastaveny následující vlastnosti chování:

- **EventName** – název události chování naslouchá.
- **Příkaz** – **rozhraní ICommand** má být proveden. Chování očekává `ICommand` instance na [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) připojeného ovládacího prvku, který může být zděděna z nadřazeného prvku.

Následující vlastnosti volitelné chování lze také nastavit:

- **CommandParameter** – `object` , který se předá příkazu.
- **Převaděč** – [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) implementace, která změní formát data argument události, jako je jí předán mezi *zdroj* a *cílové*nástrojem vazby.

## <a name="creating-the-behavior"></a>Vytvoření chování

`EventToCommandBehavior` Třída odvozena z `BehaviorBase<T>` třídu, která je dále odvozeno z [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) třídy. Účelem `BehaviorBase<T>` třídy je stanovit základní třídu pro Xamarin.Forms neovlivní žádné chování, které vyžadují [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) chování nastavit do připojeného ovládacího prvku. Tím se zajistí, že chování lze svázat a spusťte `ICommand` určená `Command` vlastnosti, když je zpracován chování.

`BehaviorBase<T>` Třída poskytuje přepisovatelným [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) metodu, která nastaví [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) chování a přepisovatelným [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))metodu, která vyčistí `BindingContext`. Kromě toho třídy ukládá odkaz na připojeného ovládacího prvku `AssociatedObject` vlastnost.

### <a name="implementing-bindable-properties"></a>Implementace vlastnosti umožňující vazbu

`EventToCommandBehavior` Třída definuje čtyři [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instancí, které jsou spouštěny uživatelem definovaný příkaz, když se aktivuje událost. Tyto vlastnosti jsou uvedeny v následujícím příkladu kódu:

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

Když `EventToCommandBehavior` spotřebování třídy `Command` vlastnost by měla představovat data vázaná `ICommand` mají být provedeny v reakci na události jeho spuštění, který je definován v `EventName` vlastnost. Chování bude chtít najít `ICommand` na [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) připojeného ovládacího prvku.

Ve výchozím nastavení se předá příkazu argumenty událostí pro událost. Tato data lze volitelně převést předaly mezi *zdroj* a *cílové* modulem vazbu tak, že určíte [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) implementace jako `Converter` hodnotu vlastnosti. Můžete také parametr může být předán příkazu tak, že zadáte `CommandParameter` hodnotu vlastnosti.

### <a name="implementing-the-overrides"></a>Implementace přepsání

`EventToCommandBehavior` Třídy přepsání [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) a [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) metody `BehaviorBase<T>` třídy, jak je znázorněno v následujícím příkladu kódu:

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

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Metoda provede instalaci pomocí volání `RegisterEvent` metodu předáním hodnoty `EventName` vlastnost jako parametr. [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Metoda provede vyčištění voláním `DeregisterEvent` metodu předáním hodnoty `EventName` vlastnost jako parametr.

### <a name="implementing-the-behavior-functionality"></a>Implementace chování funkce

Chování slouží k provedení příkazu definované `Command` v odezvě na událost vyvolanou, který je definován `EventName` vlastnost. Základní chování funkce můžete vidět v následujícím příkladu kódu:

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

`RegisterEvent` Provedení metody v reakci `EventToCommandBehavior` připojovaný ke ovládací prvek a přijímá hodnotu z `EventName` vlastnost jako parametr. Metoda následně se pokusí najít události definované v `EventName` vlastnost připojeného ovládacího prvku. Za předpokladu, že událost může být umístěna `OnEvent` metoda je zaregistrovaná a může být metoda obslužné rutiny události.

`OnEvent` Provedení metody v reakci na události jeho spuštění, který je definován v `EventName` vlastnost. Za předpokladu, že `Command` vlastnost odkazuje na platný `ICommand`, metoda se pokusí načíst parametr předat `ICommand` následujícím způsobem:

- Pokud `CommandParameter` vlastnost definuje parametr, je načten.
- Jinak, pokud `Converter` definuje vlastnost [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) implementace konvertoru spouští a převádí data argument události předaly mezi *zdroj* a *cílové* nástrojem vazby.
- Argumenty události jsou jinak, předpokládá se parametr.

Data vázaná `ICommand` pak provést, v parametru předá do příkazu za předpokladu, že [ `CanExecute` ](xref:Xamarin.Forms.Command.CanExecute(System.Object)) vrátí metoda `true`.

I když není zobrazený, `EventToCommandBehavior` zahrnuje také `DeregisterEvent` metodu, která provádí [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) – metoda. `DeregisterEvent` Metoda se používá k vyhledání a zrušení registrace události definované v `EventName` vlastnost k vyčištění paměti potenciální nevracení.

## <a name="consuming-the-behavior"></a>Využívání chování

`EventToCommandBehavior` Třídy lze připojit k [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) kolekce ovládacího prvku, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<ListView ItemsSource="{Binding People}">
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}"
                                  Converter="{StaticResource SelectedItemConverter}" />
  </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

Ekvivalentní kód jazyka C# můžete vidět v následujícím příkladu kódu:

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

`Command` Vlastnost chování je vázán na data `OutputAgeCommand` vlastnost přidružené ViewModel zatímco `Converter` je nastavena na `SelectedItemConverter` instance, která vrátí [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem)z [ `ListView` ](xref:Xamarin.Forms.ListView) z [ `SelectedItemChangedEventArgs` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs).

Za běhu chování odpoví na interakci ze strany ovládacího prvku. Při výběru položky v [ `ListView` ](xref:Xamarin.Forms.ListView), [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) se aktivuje událost, která se spustí `OutputAgeCommand` v ViewModel. Tím se pak aktualizuje ViewModel `SelectedItemText` vlastnost, která [ `Label` ](xref:Xamarin.Forms.Label) vytvoří vazbu na, jak je znázorněno na následujících snímcích obrazovky:

[![](event-to-command-behavior-images/screenshots-sml.png "Ukázková aplikace s EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "ukázkovou aplikaci s EventToCommandBehavior")

Výhodou použití tohoto chování k provedení příkazu, když se aktivuje událost je, že příkazy mohou být spojeny s prvky, které nejsou určeny k interakci s příkazy. Kromě toho tím kód pro zpracování událostí kotle talíře z soubory kódu na pozadí.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali pomocí chování Xamarin.Forms spuštění příkazu, když se aktivuje událost. Chování slouží k přidružení příkazů s ovládacími prvky, které nejsou určeny k interakci s příkazy.


## <a name="related-links"></a>Související odkazy

- [Chování EventToCommand (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Chování](xref:Xamarin.Forms.Behavior)
- [Chování<T>](xref:Xamarin.Forms.Behavior`1)
