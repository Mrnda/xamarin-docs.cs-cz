---
title: Podpora jazyka Objective-C
description: Tento dokument obsahuje popis podporu jazyka Objective-C v rozhraní .NET vložení. Popisuje automatické počítání odkazů, NSString, protokoly, NSObject protokol, výjimky a další.
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 95604133293f0fb2fe9b651fd7cb6b18f3994c84
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793852"
---
# <a name="objective-c-support"></a>Podpora jazyka Objective-C

## <a name="specific-features"></a>Konkrétní funkce

Generování jazyka Objective-C má několik speciálních funkcí, které jsou vhodné poznamenat.

### <a name="automatic-reference-counting"></a>Počítání automatické odkazů

Použití nástroje Automatické odkaz počítání (OBLOUK) je **požadované** volat generovaného vazby. Projekt pomocí knihovny založené na rozhraní .NET vložení musí být zkompilovány s `-fobjc-arc`.

### <a name="nsstring-support"></a>Podpora NSString

Rozhraní API, která vystavit `System.String` typy se převedou na `NSString`. To výrazně zjednodušuje Správa paměti než při plánování práce s `char*`.

### <a name="protocols-support"></a>Podpora protokolů

Spravovaná rozhraní se převedou na protokoly jazyka Objective-C, kde se všichni její členové `@required`.

### <a name="nsobject-protocol-support"></a>Podpora protokolu NSObject

Ve výchozím nastavení výchozí algoritmu hash a rovnosti .NET a runtime jazyka Objective-C považují za zaměňovat, protože sdílejí podobnou sémantiku jako.

Při přepsání spravovaného typu `Equals(Object)` nebo `GetHashCode`, obvykle znamená, že výchozí (.NET) chování nebyl dostatečný; z toho vyplývá, že výchozí chování jazyka Objective-C je pravděpodobně není dostatek buď.

V takových případech generátor přepsání [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) metoda a [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) v definovánu vlastnost [ `NSObject` protokol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). To umožňuje vlastní spravované implementace pro použití v kódu jazyka Objective-C transparentně.

### <a name="exceptions-support"></a>Podpora výjimek

Předávání `--nativeexception` jako argument pro `objcgen` bude převést spravované výjimky jazyka Objective-C výjimky, které můžete zachycení a zpracovat. 

### <a name="comparison"></a>Srovnání

Spravované typy, které implementují `IComparable` (nebo její obecné verzi `IComparable<T>`) vytvoří jazyka Objective-C popisný metody, které vracejí `NSComparisonResult` a přijímat `nil` argument. Díky tomu generovaného rozhraní API jazyka Objective-C vývojářům více popisný. Příklad:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Kategorie

Spravovaná rozšíření metody jsou převedeny do kategorií. Například následující metody rozšíření na `Collection`:

```csharp
public static class SomeExtensions {
    public static int CountNonNull (this Collection collection) { ... }
    public static int CountNull (this Collection collection) { ... }
}
```

by vytvořit kategorie jazyka Objective-C tohoto typu:

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

Pokud jeden spravovaný typ rozšiřuje několik typů, jsou generovány více kategorií jazyka Objective-C.

### <a name="subscripting"></a>Předplatné

Spravované indexované vlastnosti se převedou na objekt předplatné. Příklad:

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

by vytvořit jazyka Objective-C podobně jako:

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

který může být použit prostřednictvím subscripting syntaxe jazyka Objective-C:

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

V závislosti na typu indexer bude předplatné indexované nebo s klíčem vygenerována, kde je to vhodné.

To [článku](http://nshipster.com/object-subscripting/) je skvělým Úvod do předplatné.

## <a name="main-differences-with-net"></a>Hlavní rozdíly s rozhraním .NET

### <a name="constructors-vs-initializers"></a>Inicializátory vs konstruktory

V Objective-C, můžete volat žádný inicializátoru prototypy libovolného nadřazenou třídu v řetězu dědičnosti Pokud je označen jako nedostupný (`NS_UNAVAILABLE`).

V jazyce C# je potřeba explicitně deklarovat členem konstruktor uvnitř třídy, což znamená, že nejsou zděděné konstruktory.

Ke zveřejnění správné reprezentace C# rozhraní API jazyka Objective-C `NS_UNAVAILABLE` se přidá do jakékoli inicializátoru, který se nenachází ve třídě podřízené od nadřazené třídy.

C# ROZHRANÍ API:

```csharp
public class Unique {
    public Unique () : this (1)
    {
    }

    public Unique (int id)
    {
    }
}

public class SuperUnique : Unique {
    public SuperUnique () : base (911)
    {
    }
}
```

Jazyka Objective-C prezentované rozhraní API:

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

Zde `initWithId:` byl označen jako nedostupný.

### <a name="operator"></a>Operátor

Jazyka Objective-C nepodporuje operátor přetížení stejně C#, takže operátory se převedou na selektory – třída:

```csharp
public static AllOperators operator + (AllOperators c1, AllOperators c2)
{
    return new AllOperators (c1.Value + c2.Value);
}
```

až

```objc
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

Ale některé jazyky .NET nepodporují operátor přetížení, takže je běžné, aby zahrnovala ["popisný"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) s názvem metoda kromě přetížení operátoru.

Pokud operátor verze a verze pro "Tisk" se nacházejí, jenom popisný verzi se budou generovat, jak bude generovat se stejným názvem jazyka Objective-C.

```csharp
public static AllOperatorsWithFriendly operator + (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
{
    return new AllOperatorsWithFriendly (c1.Value + c2.Value);
}

public static AllOperatorsWithFriendly Add (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
{
    return new AllOperatorsWithFriendly (c1.Value + c2.Value);
}
```

Změní na:

```objc
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Operátor rovnosti

V obecné operátoru `==` v jazyce C# se zpracovává jako obecné operátor uvedených výše.

Ale pokud je "popisný" operátor je rovno najde, oba operátor `==` a operátor `!=` bude přeskočena při generování.

### <a name="datetime-vs-nsdate"></a>Data a času vs NSDate

Z [ `NSDate` ](https://developer.apple.com/reference/foundation/nsdate?language=objc) dokumentaci:

> `NSDate` objekty zapouzdřují jediný bod v čase, nezávisle na všechny konkrétní calendrical systém nebo časové pásmo. Datum objekty jsou neměnné, reprezentující interval invariantní času relativně k datum absolutní odkaz (00: 00:00 UTC na 1. ledna 2001).

Z důvodu `NSDate` referenční datum, všechny převody mezi ho a `DateTime` je třeba provést ve standardu UTC.

#### <a name="datetime-to-nsdate"></a>Datum a čas k NSDate

Při převodu z `DateTime` k `NSDate`, `Kind` vlastnost `DateTime` je vzít v úvahu:

|Typ|Výsledky|
|---|---|
|`Utc`|Převod se provádí pomocí poskytnutého `DateTime` objektů, jako je.|
|`Local`|Výsledek volání `ToUniversalTime()` v zadaných `DateTime` objekt se používá pro převod.|
|`Unspecified`|Poskytnutého `DateTime` objektu předpokládá se, že UTC, takže stejné chování při `Kind` je `Utc`.|

Převod používá následující vzorec:

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

V tento vzorec: 

- `NSDateReferenceDateTicks` se počítá na základě `NSDate` referenční datum UTC 00:00:00 na 1. ledna 2001: 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) musí být definován [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Chcete-li vytvořit `NSDate` objekt, `TimeInterval` se používá s `NSDate` [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) selektor.

#### <a name="nsdate-to-datetime"></a>NSDate datum a čas

Převod z `NSDate` k `DateTime` používá následující vzorec:

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

V tento vzorec: 

- `NSDateReferenceDateTicks` se počítá na základě `NSDate` referenční datum UTC 00:00:00 na 1. ledna 2001: 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) musí být definován [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Po výpočtu `DateTimeTicks`, `DateTime` [konstruktor](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_) je vyvolána, nastavení jeho `kind` k `DateTimeKind.Utc`.

> [!NOTE]
> `NSDate` může být `nil`, ale `DateTime` je struktury v rozhraní .NET, který podle definice nelze `null`. Pokud zadáte název `nil` `NSDate`, bude možné přeložit na výchozí hodnoty `DateTime` hodnotu, která se mapuje na `DateTime.MinValue`.

`NSDate` podporuje vyšší maximální a minimální hodnotu nižší než `DateTime`. Při převodu z `NSDate` k `DateTime`, jsou tyto hodnoty vyšší a nižší změněna na `DateTime` [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) nebo [MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue), v uvedeném pořadí.
