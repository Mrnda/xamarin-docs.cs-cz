---
title: Vlastnosti umožňující vazbu
description: Tento článek obsahuje úvod do vlastnosti umožňující vazbu a ukazuje, jak vytvářet a využívat je.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 241579d51d1f0af84655f439bad3adb879404e91
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995385"
---
# <a name="bindable-properties"></a>Vlastnosti umožňující vazbu

_V Xamarin.Forms funkce common language runtime (CLR) vlastností rozšířit pomocí vlastnosti umožňující vazbu. Vlastnost s vazbou je speciální typ vlastnosti, kde hodnota vlastnosti je sledován pomocí funkce v systému vlastností Xamarin.Forms. Tento článek obsahuje úvod do vlastnosti umožňující vazbu a ukazuje, jak vytvářet a využívat je._

## <a name="overview"></a>Přehled

Vlastnosti umožňující vazbu rozšíření funkcí vlastnost CLR prostřednictvím vlastnosti se zálohování [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) typ místo zálohování vlastnost s polem. Účelem vlastnosti umožňující vazbu je poskytnout vlastnost systému, který podporuje datovou vazbu, styly, šablony a hodnoty nastavené pomocí vztahů nadřazenosti a podřízenosti. Vlastnosti umožňující vazbu kromě toho můžete zadat výchozí hodnoty, ověření hodnoty vlastností a zpětná volání, které sledují změny vlastností.

Vlastnosti by měla být implementována jako vlastnosti umožňující vazbu pro podporu jeden nebo více následujících funkcí:

- Funguje jako platný *cílové* vlastnost pro datovou vazbu.
- Nastavení vlastnosti prostřednictvím [styl](~/xamarin-forms/user-interface/styles/index.md).
- Poskytuje výchozí hodnotu vlastnosti, které se liší od výchozího nastavení pro typ vlastnosti.
- Ověření hodnoty vlastnosti.
- Monitorování změny vlastností.

Příkladem vlastnosti umožňující vazbu Xamarin.Forms [ `Label.Text` ](xref:Xamarin.Forms.Label.Text), [ `Button.BorderRadius` ](xref:Xamarin.Forms.Button.BorderRadius), a [ `StackLayout.Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation). Každou vlastnost podporující vazby má odpovídající `public static readonly` vlastnost typu [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) , které je zveřejněné ve stejné třídě, a to je identifikátor vlastnost podporující vazby. Například odpovídající identifikátor vázanou vlastnost pro `Label.Text` vlastnost [ `Label.TextProperty` ](xref:Xamarin.Forms.Label.TextProperty).

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>Vytváření a využívání vázanou vlastnost

Proces vytvoření vázanou vlastnost vypadá takto:

1. Vytvoření [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instance s jedním z [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create*) přetížení metody.
1. Definujte přístupové objekty vlastnosti pro [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instance.

Všimněte si, že všechny [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instancí musí být vytvořen na vlákně UI. To znamená, že pouze kód, který běží na vlákně UI můžete získat nebo nastavit hodnotu vázanou vlastnost. Ale `BindableProperty` instance je přístupný z jiných vláken zařazování na vlákno uživatelského rozhraní se [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) metody.

### <a name="creating-a-property"></a>Vytvoření vlastnosti

Chcete-li vytvořit `BindableProperty` instance, obsažené třídy musí být odvozen od [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) třídy. Ale `BindableObject` třídy je vysoká. v hierarchii tříd, takže většina tříd použitý pro uživatelské rozhraní funkce podpory umožňujících vazbu vlastnosti.

Vlastnost s vazbou lze vytvořit deklarováním `public static readonly` vlastnost typu [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty). Vázanou vlastnost měli nastavit na hodnotu vrácené některého [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) přetížení metody. Deklarace by měla být v těle [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) odvozené třídy, ale mimo všechny definice členů.

Minimálně musí být identifikátor zadali při vytváření [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty), společně s následujícími parametry:

- Název [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty).
- Typ vlastnosti.
- Typ vlastnící objekt.
- Výchozí hodnota pro vlastnost. Tím se zajistí, že vlastnost vždy vrátí hodnotu konkrétní výchozí, pokud není nastavena a může se lišit od výchozí hodnoty pro typ vlastnosti. Výchozí hodnota bude obnovena, kdy [ `ClearValue` ](xref:Xamarin.Forms.BindableObject.ClearValue(Xamarin.Forms.BindableProperty)) metoda je volána na vlastnost s vazbou.

Následující kód ukazuje příklad s možností vazby vlastnosti s identifikátorem a hodnoty pro čtyři požadované parametry:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

Tím se vytvoří [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instanci s názvem `EventName`, typu `string`. Vlastní vlastnost `EventToCommandBehavior` třídy a má výchozí hodnotu `null`. Zásady vytváření názvů pro vlastnosti umožňující vazbu je, že identifikátor vázanou vlastnost musí odpovídat název vlastnosti zadaný v `Create` metodou "Vlastnosti" připojenou k němu. Proto v předchozím příkladu je identifikátor vázanou vlastnost `EventNameProperty`.

Volitelně můžete při vytváření [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instance následující parametry lze zadat:

- Režim vazeb. Slouží k určení směru, ve kterém bude šířit změně hodnoty vlastnosti. Ve výchozím režimu vazby, se změny rozšíří z *zdroj* k *cílové*.
- Ověření delegáta, který bude vyvolán při nastavení hodnoty vlastnosti. Další informace najdete v tématu [zpětná volání ověření](#validation).
- Delegát, který bude vyvolán při změně hodnoty vlastnosti došlo ke změně vlastnosti. Další informace najdete v tématu [zjištění změny vlastností](#propertychanges).
- Vlastnost Změna delegáta, který se vyvolá, když se změní hodnota vlastnosti. Tento delegát má stejný podpis jako delegát změněné vlastnosti.
- Coerce hodnotu delegáta, který bude vyvolán při změně hodnoty vlastnosti. Další informace najdete v tématu [vynucení zpětných volání hodnoty](#coerce).
- A `Func` , který slouží k inicializaci výchozí hodnotu vlastnosti. Další informace najdete v tématu [vytváření výchozí hodnotu s funkci Func](#defaultfunc).

### <a name="creating-accessors"></a>Vytváření přístupových objektů

Přistupující objekty vlastnosti je potřeba použít syntaxe vlastnosti pro přístup k vázanou vlastnost. `Get` Přístupového objektu by měla vrátit hodnotu, která se nachází v odpovídající vlastnost podporující vazby. Toho lze dosáhnout pomocí volání [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) metoda předávání v identifikátoru vázanou vlastnost, na kterém má být získána hodnota a potom výsledek na požadovaný typ přetypování. `Set` Přistupující objekt měli nastavit hodnotu vlastnosti odpovídající vazbu. Toho lze dosáhnout pomocí volání [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metodu identifikátor umožňujících vazbu vlastnosti na základě které chcete nastavit hodnotu a hodnotu nastavení.

Následující příklad kódu ukazuje přístupové objekty pro `EventName` vázanou vlastnost:

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>Využívání vázanou vlastnost

Po vytvoření vázanou vlastnost může být upotřebena z XAML nebo kódu. V XAML tím se dosahuje deklarace oboru názvů s předponou, pomocí deklarace oboru názvů označující název oboru názvů CLR a volitelně název sestavení. Další informace najdete v tématu [obory názvů XAML](~/xamarin-forms/xaml/namespaces.md).

Následující příklad kódu ukazuje obor názvů XAML pro vlastní typ, který obsahuje vazbu vlastnosti, která je definována v rámci stejného sestavení jako kód aplikace, který odkazuje na vlastní typ:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Deklarace oboru názvů se používá při nastavení `EventName` vlastnost podporující vazby, jako předvedenou v následujícím příkladu kódu XAML:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

Ekvivalentní kód jazyka C# můžete vidět v následujícím příkladu kódu:

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  ...
});
```

<a name="advanced" />

## <a name="advanced-scenarios"></a>Pokročilé scénáře

Při vytváření [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instance, existuje mnoho nepovinných parametrů, které můžete nastavit pro povolení rozšířených vlastností umožňujících vazbu scénáře. Tato část popisuje tyto scénáře.

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>Zjišťují se změny vlastnosti

A `static` metoda změny vlastnosti zpětného volání lze dokument zaregistrovat u vázanou vlastnost tak, že zadáte `propertyChanged` parametr pro [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) metody. Zadaná metoda zpětného volání bude vyvolán při změně hodnoty vlastnosti umožňující vazbu.

Následující příklad kódu ukazuje jak `EventName` vázanou vlastnost registrů `OnEventNameChanged` metody jako metody zpětného volání změny vlastnosti:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create (
    "EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
...

static void OnEventNameChanged (BindableObject bindable, object oldValue, object newValue)
{
  // Property changed implementation goes here
}
```

V metodě změny vlastnosti zpětného volání [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) parametr se používá k označení, která instance třídy vlastnící ohlásil změnu a hodnoty ze dvou `object` parametry představují staré a nové hodnoty Vlastnost podporující vazby.

<a name="validation" />

### <a name="validation-callbacks"></a>Zpětná volání ověření

A `static` ověření metody zpětného volání lze dokument zaregistrovat u vázanou vlastnost tak, že zadáte `validateValue` parametr pro [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) metody. Zadaná metoda zpětného volání bude vyvolán při nastavenou vlastnost podporující vazby.

Následující příklad kódu ukazuje jak `Angle` vázanou vlastnost registrů `IsValidValue` metody jako metody zpětného volání ověření:

```csharp
public static readonly BindableProperty AngleProperty =
  BindableProperty.Create ("Angle", typeof(double), typeof(HomePage), 0.0, validateValue: IsValidValue);
...

static bool IsValidValue (BindableObject view, object value)
{
  double result;
  bool isDouble = double.TryParse (value.ToString (), out result);
  return (result >= 0 && result <= 360);
}
```

Zpětná volání ověřování jsou k dispozici s hodnotou a by měl vrátit `true` Pokud hodnota je platná pro vlastnost, v opačném případě `false`. Bude vyvolána výjimka, pokud vrací zpětné volání pro ověření `false`, který má být zpracována vývojáře. Typické použití ověřovací metodu zpětného volání je omezení hodnoty celých čísel nebo hodnot datového typu Double, pokud je nastavena vlastnost podporující vazby. Například `IsValidValue` metoda zkontroluje, že hodnota vlastnosti je `double` v rozsahu 0 až 360.

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>Vynucení zpětných volání hodnoty

A `static` vynucení hodnotu metody zpětného volání lze dokument zaregistrovat u vázanou vlastnost tak, že zadáte `coerceValue` parametr pro [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) metoda. Zadaná metoda zpětného volání bude vyvolán při změně hodnoty vlastnosti umožňující vazbu.

Převeďte hodnotu, použitý zpětná volání k vynucení přehodnocení vázanou vlastnost při změně hodnoty vlastnosti. Například zpětné volání hodnotu coerce lze zajistit, že hodnota jednu vlastnost podporující vazby není větší než hodnota jinou vlastnost s vazbou.

Následující příklad kódu ukazuje jak `Angle` vázanou vlastnost registrů `CoerceAngle` metody jako metody zpětného volání hodnotu coerce:

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle) {
    input = homePage.MaximumAngle;
  }

  return input;
}
```

`CoerceAngle` Metoda zkontroluje hodnotu vlastnosti `MaximumAngle` vlastnost a pokud `Angle` je větší než hodnota vlastnosti, převede hodnotu na `MaximumAngle` hodnotu vlastnosti.

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>Vytváří se výchozí hodnota s funkci Func

A `Func` slouží k inicializaci výchozí hodnoty vlastnosti umožňující vazbu, jak je ukázáno v následujícím příkladu kódu:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator` Parametr je nastaven na `Func` , která vyvolává [ `Device.GetNamedSize` ](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) metodu pro návrat `double` , která představuje pojmenované velikost písma, která se používá na [ `Label` ](xref:Xamarin.Forms.Label) na nativní platformě.

## <a name="summary"></a>Souhrn

Tento článek poskytuje úvod do vlastnosti umožňující vazbu a ukázal, jak vytvářet a využívat je. Vlastnost s vazbou je speciální typ vlastnosti, kde hodnota vlastnosti je sledován pomocí funkce v systému vlastností Xamarin.Forms.


## <a name="related-links"></a>Související odkazy

- [Obory názvů jazyka XAML](~/xamarin-forms/xaml/namespaces.md)
- [Událost k chování příkazu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Zpětné volání pro ověření (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [Vynucení zpětného volání hodnotu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](xref:Xamarin.Forms.BindableProperty)
- [BindableObject](xref:Xamarin.Forms.BindableObject)
