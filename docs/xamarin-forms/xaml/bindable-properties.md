---
title: Vazbu vlastnosti
description: V Xamarin.Forms je funkce společných vlastností language runtime (CLR) rozšířit pomocí vazbu vlastnosti. Vazbu vlastnosti je speciální typ vlastnosti, kde je vlastnost systémem Xamarin.Forms sledovat hodnotu vlastnosti. Tento článek obsahuje úvod do vlastnosti vazbu a ukazuje, jak vytvářet a využívat je.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 7e1d3c82036ef703014ae548a6719937e89d22f4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="bindable-properties"></a>Vazbu vlastnosti

_V Xamarin.Forms je funkce společných vlastností language runtime (CLR) rozšířit pomocí vazbu vlastnosti. Vazbu vlastnosti je speciální typ vlastnosti, kde je vlastnost systémem Xamarin.Forms sledovat hodnotu vlastnosti. Tento článek obsahuje úvod do vlastnosti vazbu a ukazuje, jak vytvářet a využívat je._

## <a name="overview"></a>Přehled

Vlastnosti vazbu rozšiřovat funkce vlastnost CLR prostřednictvím zálohování vlastnost s [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) typu, místo zálohování vlastnost s polem. Účelem vazbu vlastnosti je poskytnout vlastnost systém, který podporuje datovou vazbu, styly, šablony a hodnoty nastavit pomocí vztahů nadřazenosti a podřízenosti. Výchozí hodnoty, ověření hodnoty vlastností a zpětná volání, které sledují změny vlastností kromě toho můžete zadat vlastnosti vazbu.

Vlastnosti by měla být implementována jako vazbu vlastnosti, které chcete podporovat jeden nebo více z následujících funkcí:

- Funguje jako platný *cíl* vlastnost pro datovou vazbu.
- Nastavení vlastnosti prostřednictvím [styl](~/xamarin-forms/user-interface/styles/index.md).
- Poskytuje výchozí hodnotu vlastnosti, které se liší od výchozí hodnoty pro typ vlastnosti.
- Hodnota vlastnosti ověřování.
- Monitorování změny vlastností.

Příklady vazbu vlastnosti Xamarin.Forms [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/), [ `Button.BorderRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/), a [ `StackLayout.Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/). Každou vazbu vlastnost má odpovídající `public static readonly` vlastnost typu [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) který je zveřejněný u stejné třídy a který je identifikátor vazbu vlastnosti. Například odpovídající identifikátor vazbu vlastnosti pro `Label.Text` vlastnost je [ `Label.TextProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Label.TextProperty/).

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>Vytvoření a použití vazbu vlastnosti

Proces vytvoření vazbu vlastnosti vypadá takto:

1. Vytvoření [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instance s jedním z [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) přetížení metody.
1. Definování vlastnosti přistupující objekty pro [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instance.

Všimněte si, že všechny [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) musí být vytvořeny instance ve vlákně UI. To znamená, že pouze kód, který běží ve vlákně UI můžete získat nebo nastavit hodnotu vazbu vlastnosti. Ale `BindableProperty` instance je přístupný z jiných vláken zařazování na vlákna uživatelského rozhraní pomocí [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) metoda.

### <a name="creating-a-property"></a>Vytvoření vlastnosti

K vytvoření `BindableProperty` instance, obsahující třídy musí být odvozeny od [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) třídy. Ale `BindableObject` třída je vysoká. v hierarchii tříd, takže většina tříd použít pro uživatelské rozhraní funkce podpory vazbu vlastnosti.

Můžete vytvořit vazbu vlastnosti deklarace `public static readonly` vlastnost typu [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/). Vazbu vlastnost musí být nastavená na vrácená hodnota jednoho z [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) přetížení metody. Deklaraci by měla být v rámci textu [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) odvozené třídy, ale mimo všechny definice člen.

Minimálně identifikátor se musí zadat při vytváření [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/), společně s následujícími parametry:

- Název [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/).
- Typ vlastnosti.
- Typ vlastnící objekt.
- Výchozí hodnota pro vlastnost. Tím se zajistí, že vlastnost vždy vrátí hodnotu konkrétní výchozí hodnotu. Pokud je nastavení, a může lišit od výchozí hodnota pro typ vlastnosti. Výchozí hodnota bude obnovena, kdy [ `ClearValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.ClearValue/p/Xamarin.Forms.BindableProperty/) metoda je volána v vazbu vlastnosti.

Následující kód ukazuje příklad vazbu vlastnosti s identifikátorem a hodnoty pro čtyři požadované parametry:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

Tím se vytvoří [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instanci s názvem `EventName`, typu `string`. Vlastní vlastnost `EventToCommandBehavior` třídy a má výchozí hodnotu `null`. Zásady vytváření názvů pro vazbu vlastnosti je, že identifikátor vazbu vlastnosti musí shodovat název vlastnosti zadaný ve `Create` metoda s "Vlastnost" připojená k němu. Proto v předchozím příkladu je identifikátor vazbu vlastnosti `EventNameProperty`.

Volitelně můžete při vytváření [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instance, následující parametry lze zadat:

- Režim vazby. Slouží k určení směru, ve kterém se rozšíří změn hodnot vlastností. Ve výchozím režimu vazby, změny se rozšíří z *zdroj* k *cíl*.
- Delegát ověření, která bude volána, když je hodnota vlastnosti nastavena. Další informace najdete v tématu [zpětná volání ověření](#validation).
- Změnila se vlastnost delegáta, který bude vyvolán při změně hodnoty vlastnosti. Další informace najdete v tématu [zjišťování změny vlastností](#propertychanges).
- Vlastnost Změna delegáta, který bude vyvolán při se změní hodnota vlastnosti. Tento delegát se stejným podpisem jako delegáta změněné vlastnosti.
- Coerce hodnota delegáta, který bude vyvolán při změně hodnoty vlastnosti. Další informace najdete v tématu [Coerce zpětná volání hodnotu](#coerce).
- A `Func` používané k chybě při inicializaci výchozí hodnotu vlastnosti. Další informace najdete v tématu [vytváření výchozí hodnotu s funkci Func](#defaultfunc).

### <a name="creating-accessors"></a>Vytváření přístupových objektů

Přístupové objekty vlastnosti vyžadovaných používat vlastnost syntaxe pro přístup k vazbu vlastnosti. `Get` Přistupujícího objektu by měla vrátit hodnotu, která se nachází v odpovídající vazbu vlastnosti. Toho lze dosáhnout pomocí volání [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) metoda předávání v identifikátor vazbu vlastnosti, na kterém má být získána hodnota a pak výsledek, který má požadovaný typ přetypování. `Set` Přistupujícího objektu měli nastavit hodnotu odpovídající vazbu vlastnosti. Toho lze dosáhnout pomocí volání [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody předávání v identifikátor vazbu vlastnosti, na kterém chcete nastavit hodnota a hodnota k nastavení.

Následující příklad kódu ukazuje přistupující objekty pro `EventName` vazbu vlastnosti:

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>Využívání vazbu vlastnosti

Po vytvoření vazbu vlastnosti mohou být využívány z XAML nebo kódu. V jazyce XAML toho se dosáhne deklarace oboru názvů s předponou, s deklaraci oboru názvů, která udává název oboru názvů CLR a volitelně název sestavení. Další informace najdete v tématu [obory názvů jazyka XAML](~/xamarin-forms/xaml/namespaces.md).

Následující příklad kódu ukazuje oboru názvů jazyka XAML pro vlastní typ, který obsahuje vazbu vlastnosti, která je definována v rámci stejného sestavení jako aplikační kód, který odkazuje na vlastní typ:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Deklarace oboru názvů se používá při nastavení `EventName` vazbu vlastnosti prokázaná v následujícím příkladu kódu XAML:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

Ekvivalentní kódu C# je znázorněno v následujícím příkladu kódu:

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  ...
});
```

<a name="advanced" />

## <a name="advanced-scenarios"></a>Složitější scénáře

Při vytváření [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instanci, je počet volitelné parametry, které můžete nastavit pro povolení rozšířených vlastností vazbu scénáře. Tato část popisuje tyto scénáře.

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>Detekce změn vlastností

A `static` metoda zpětného volání změny vlastnosti lze registrovat pomocí vazbu vlastnosti zadáním `propertyChanged` parametr pro [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) metoda. Metoda zpětného volání zadaný bude vyvolán při změně hodnoty vazbu vlastnosti.

Následující příklad kódu ukazuje jak `EventName` registry vazbu vlastnosti `OnEventNameChanged` jako metody zpětného volání vlastnost změnit metodu:

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

V metodě vlastnost změnit zpětného volání [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) parametr se používá k označení, které instance třídy vlastnícím ohlásil změnu a hodnoty dvou `object` parametry představují staré a nové hodnoty Vlastnost vazbu.

<a name="validation" />

### <a name="validation-callbacks"></a>Zpětná volání ověření

A `static` metoda zpětného volání ověření lze registrovat pomocí vazbu vlastnosti tak, že zadáte `validateValue` parametr pro [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) metoda. Metoda zpětného volání zadaný bude být volána, když je nastavena hodnota vazbu vlastnosti.

Následující příklad kódu ukazuje jak `Angle` registry vazbu vlastnosti `IsValidValue` metoda jako metody zpětného volání ověření:

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

Zpětná volání ověřování jsou k dispozici s hodnotou a by měla vrátit `true` Pokud hodnota je platná pro vlastnost, jinak `false`. Pokud vrátí zpětné volání pro ověření, bude vyvolána výjimka `false`, který má být zpracována vývojář. Typické použití metody zpětného volání ověření je omezíte hodnoty celá čísla nebo hodnoty Double-vazbu vlastnost nastavena. Například `IsValidValue` metoda kontroluje, zda hodnota vlastnosti `double` v rozsahu 0 až 360.

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>Coerce – hodnota zpětných volání

A `static` coerce – hodnota metody zpětného volání lze registrovat pomocí vazbu vlastnosti tak, že zadáte `coerceValue` parametr pro [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) metoda. Metoda zpětného volání zadaný bude vyvolán při změně hodnoty vazbu vlastnosti.

Coerce – hodnota, kterou zpětná volání slouží k vynucení opětovného hodnocení vazbu vlastnosti při změně hodnoty vlastnosti. Například hodnota zpětné volání coerce lze zajistit, že hodnota jednu vazbu vlastnosti není větší než hodnota jiné vazbu vlastnosti.

Následující příklad kódu ukazuje jak `Angle` registry vazbu vlastnosti `CoerceAngle` jako metody zpětného volání hodnotu coerce metodu:

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

`CoerceAngle` Metoda kontroluje hodnotu `MaximumAngle` vlastnost a pokud `Angle` je větší než hodnota vlastnosti, se převede hodnotu na `MaximumAngle` hodnotu vlastnosti.

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>Vytváření s funkci Func výchozí hodnotu.

A `Func` lze použít k chybě při inicializaci výchozí hodnota vlastnosti vazbu, jak je ukázáno v následujícím příkladu kódu:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator` Parametr je nastaven na `Func` , který spustí [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) metoda vrátí `double` představující pojmenované velikost písma, který se používá na [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) na nativní platformě.

## <a name="summary"></a>Souhrn

Tento článek poskytuje úvod do vlastnosti vazbu a ukázal, jak lze vytvářet a využívat je. Vazbu vlastnosti je speciální typ vlastnosti, kde je vlastnost systémem Xamarin.Forms sledovat hodnotu vlastnosti.


## <a name="related-links"></a>Související odkazy

- [Obory názvů jazyka XAML](~/xamarin-forms/xaml/namespaces.md)
- [Události k chování příkazu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Zpětné volání pro ověření (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [Coerce – hodnota zpětného volání (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
