---
title: Podpora jazyka Objective-C
ms.topic: article
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: f6d19f0f6573b17dfb3feb6bf131686413d4e68f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="objective-c-support"></a>Podpora jazyka Objective-C

## <a name="specific-features"></a>Konkrétní funkce

Generování ObjC má několik, speciální funkce, které jsou vhodné poznamenat.

### <a name="automatic-reference-counting"></a>Počítání automatické odkazů

Použití nástroje Automatické odkaz počítání (OBLOUK) je **požadované** volat generovaného vazby. Projekt pomocí knihovny na základě embeddinator musí být zkompilovány s `-fobjc-arc`.

### <a name="nsstring-support"></a>Podpora NSString

Rozhraní API, která vystavit `System.String` typy se převedou na `NSString`. To výrazně zjednodušuje Správa paměti než při zpracování `char*`.

### <a name="protocols-support"></a>Podpora protokolů

Spravovaná rozhraní se převedou na ObjC protokoly, kde se všichni její členové `@required`.

### <a name="nsobject-protocol-support"></a>Podpora protokolu NSObject

Ve výchozím nastavení předpokládáme, výchozí algoritmu hash a rovnosti .net a ObjC runtime jsou dobře a zaměňovat sdílejí velmi podobné sémantiku.

Při přepsání spravovaného typu `Equals(Object)` nebo `GetHashCode` pak to obvykle znamená chování defaut (.NET) nebyla nejlepší. Budeme předpokládat, že buď výchozí chování jazyka Objective-C nebude.

V takovém případě generátor přepsání [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) metoda a [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) v definovánu vlastnost [ `NSObject` protokol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). To umožňuje vlastní spravované implementace pro použití v kódu ObjC transparentně.

### <a name="comparison"></a>Srovnání

Spravované typy, které implementují `IComparable` nebo je obecný verze `IComparable<T>` vytvoří ObjC popisný metody, které vrací `NSComparisonResult` a přijímat `nil` argument. Díky tomu generovaného rozhraní API přehlednější ObjC vývojářům, například

```csharp
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

```csharp
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;
@end
```

Při správě jednoho typu rozšiřuje několik typů, pak jsou generovány více kategorií jazyka Objective-C.

### <a name="subscripting"></a>Předplatné

Spravované indexované vlastnosti se převedou na objekt předplatné. Příklad:

```csharp
    public bool this[int index] {
        get { return c[index]; }
        set { c[index] = value; }
    }
```

by vytvořit jazyka Objective-C podobně jako:

```csharp
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

který může být použit prostřednictvím subscripting syntaxe jazyka Objective-C:

```csharp
    if ([intCollection [0] isEqual:@42])
        intCollection[0] = @13;
```

V závislosti na typu indexer bude předplatné indexované nebo s klíčem vygenerována, kde je to vhodné.

To [článku](http://nshipster.com/object-subscripting/) je skvělým Úvod do předplatné.

## <a name="main-differences-with-net"></a>Hlavní rozdíly s rozhraním .NET

### <a name="constructors-vs-initializers"></a>Konstruktory a inicializátory

V Objective-C můžete volat žádný inicializátoru prototypy libovolného nadřazenou třídu v řetězu dědičnosti není označena jako nedostupná (NS_UNAVAILABLE).

V jazyce C# je potřeba explicitně deklarovat členem konstruktor uvnitř třídy, to znamená, že nejsou zděděné konstruktory.

Ke zveřejnění správné reprezentace C# rozhraní API jazyka Objective-C, přidáme `NS_UNAVAILABLE` k žádné inicializátoru, který se nenachází ve třídě podřízené od nadřazené třídy.

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

```objectivec
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

Zde jsme uvidíte, že `initWithId:` byl označen jako nedostupný.

### <a name="operator"></a>Operátor

ObjC nepodporuje operátor přetížení stejně C#, takže operátory se převedou na selektory – třída:

```csharp
    public static AllOperators operator + (AllOperators c1, AllOperators c2)
    {
        return new AllOperators (c1.Value + c2.Value);
    }
```

až

```csharp
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

Ale některé jazyky .NET nepodporují operátor přetížení, takže je běžné, aby zahrnovala ["popisný"](https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx) s názvem metoda kromě přetížení operátoru.

Pokud operátor verze a verze pro "Tisk" se nacházejí, jenom popisný verzi se budou generovat, jak bude generovat se stejným názvem jazyka objective-c.

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

```csharp
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Operátor rovnosti

V obecné operátoru == v C# se zpracovává jako obecné operátor jak je uvedeno výše.

Ale pokud "popisný" operátor je rovno nenajde, oba operátor == a operátor! = bude přeskočena při generování.

### <a name="datetime-vs-nsdate"></a>DateTime vs NSDate

Z [na NSDate](https://developer.apple.com/reference/foundation/nsdate?language=objc) dokumentaci:

> NSDate objekty zapouzdřují jediný bod v čase, nezávisle na všechny konkrétní calendrical systém nebo časové pásmo. Datum objekty jsou neměnné, reprezentující interval invariantní času relativně k datum absolutní odkaz (00: 00:00 UTC na 1. ledna 2001).

Z důvodu `NSDate` referenční datum, všechny převody mezi ho a `DateTime` je třeba provést ve standardu UTC.

#### <a name="datetime-to-nsdate"></a>Datum a čas k NSDate

Při převodu z `DateTime` k `NSDate` data a času `Kind` vlastnost je vzít v úvahu.

<table>
<tr><th> Typ         </th><th> Výsledky                                                                                            </th></tr>
<!--tr><td> ------------ </td><td> -------------------------------------------------------------------------------------------------- </td></tr-->
<tr><td> Čas UTC          </td><td> Převod se provádí pomocí zadaného objektu data a času, jako je.                                  </td></tr>
<tr><td> místní        </td><td> Výsledek volání `ToUniversalTime ()` v zadané datum a čas objekt se používá pro převod. </td></tr>
<tr><td> Tento parametr  </td><td> Zadané datum a čas objektu předpokládá se, že UTC, takže stejné chování jako typ == Utc.                </td></tr>
</table>

Převod se provádí pomocí tohoto vzorce:

> [!NOTE]
> **TimeInterval** = DateTimeObjectTicks - NSDateReferenceDateTicks[dt] / [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx)

Jakmile TimeInterval používáme na NSDate [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) selektor k jeho vytvoření.

#### <a name="nsdate-to-datetime"></a>NSDate datum a čas

Přecházející z NSDate na typ DateTime předpokládáme, že jsme se získávání NSDate instanci, která je datum odkaz je **00:00:00 UTC na 1. ledna 2001** a použijte následující vzorec:

> [!NOTE]
> **DateTimeTicks** = NSDateTimeIntervalSinceReferenceDate * [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx) + NSDateReferenceDateTicks[dt]

Jakmile jsme vypočítat **DateTimeTicks** používáme následující data a času [konstruktor](https://msdn.microsoft.com/en-us/library/w0d47c9c(v=vs.110).aspx) nastavení jeho `kind` k `DateTimeKind.Utc`.

Existují některé aspekty, které je potřeba vědět, může být NSDate `nil` , ale na datum a čas je struktury v rozhraní .NET a podle definice nemůže být `null`. Pokud zadáte název `nil` NSDate jsme se převede ji na výchozí hodnotu DateTime, která se mapuje na `DateTime.MinValue`.

MinValue a MaxValue se také liší, NSDate může podporovat větší a dolní hranice než data a času je tak vždy, když poskytují vyšší nebo nižší hodnotu jsme ji nastaví na hodnotu data a času na [MaxValue](https://msdn.microsoft.com/en-us/library/system.datetime.maxvalue(v=vs.110).aspx) nebo [MinValue](https://msdn.microsoft.com/en-us/library/system.datetime.minvalue(v=vs.110).aspx) v uvedeném pořadí.

**DT**: NSDate referenční datum **00:00:00 UTC na 1. ledna 2001** => `new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;`
