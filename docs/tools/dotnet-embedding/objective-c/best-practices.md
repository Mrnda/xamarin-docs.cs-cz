---
title: Vložení osvědčené postupy pro Objective-C rozhraní .NET
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: e592a76e428d23881f1fe2dc5c7254999bece517
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="net-embedding-best-practices-for-objective-c"></a>Vložení .NET osvědčené postupy pro Objective-C

Toto je koncept a nemusí být synchronizovaná s funkcí podporován v současné době nástrojem. Věříme, že bude tento dokument momentální samostatně a nakonec odpovídat konečné nástroj, tedy budete doporučujeme dlouhodobé nejlepší přístupy - není okamžitý alternativní řešení.

Velká část tohoto dokumentu se týká také jiné podporované jazyky. Ale všechny za předpokladu, že jsou příklady v C# a Objective-c

## <a name="exposing-a-subset-of-the-managed-code"></a>Vystavení podmnožinu spravovaného kódu

Vygenerovaný nativní knihovny nebo framework obsahuje kód jazyka Objective-C pro každé spravované rozhraní API, který je zveřejněný volání. Další rozhraní API můžete surface (zveřejnit) pak větší nativního _pojidlem_ bude knihovny.

Může být vhodné vytvořit různé, menší sestavení vystavit pouze požadované rozhraní API pro nativní developer. Tento průčelí za vám také umožní větší kontrolu nad viditelnost, názvy, Chyba při kontrole... generovaného kódu.

## <a name="exposing-a-chunkier-api"></a>Vystavení chunkier rozhraní API

Existuje cena platit pro přechod z nativní na spravované (a pozadí). Jako takový je lepší vystavit _rozsekaně místo chatty_ rozhraní API pro nativní vývojáře, například

**Chatty**

```csharp
public class Person {
  public string FirstName { get; set; }
  public string LastName { get; set; }
}
```

```objc
// this requires 3 calls / transitions to initialize the instance
Person *p = [[Person alloc] init];
p.firstName = @"Sebastien";
p.lastName = @"Pouliot";
```

**Rozsekaně**

```csharp
public class Person {
  public Person (string firstName, string lastName) {}
}
```

```objc
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

Vzhledem k tomu, že je menší počet přechody bude lepší výkon. Také budete potřebovat méně kódu má být vygenerován, takže vznikne také menší nativní knihovny.

## <a name="naming"></a>Pojmenování

Pojmenování věcí je jedním z dva nejtěžší problémy v oblasti vědy, ostatní právě mezipaměti chyby zneplatnění a vypnout podle-1. Zpravidla vložení .NET může vás chrání před všechny ale názvy.

### <a name="types"></a>Typy

Jazyka Objective-C nepodporuje obory názvů. Obecně platí, jsou typy jejího předponou 2 (pro Apple) nebo 3 (pro 3. stran) znak předponou, jako je třeba `UIView` UIKit na zobrazení, který označuje rozhraní.

Typy .NET přeskočení oboru názvů není možné jako ho můžou představovat duplikovaná nebo matoucí, názvy. Díky tomu existující typy .NET velmi dlouhé, například

```csharp
namespace Xamarin.Xml.Configuration {
  public class Reader {}
}
```

se použije jako:

```objc
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

Můžete ale znovu vystavit typ jako:

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

Díky tomu další popisný jazyka Objective-C chcete použít, například:

```objc
id reader = [[XAMXmlConfigReader alloc] init];
```

### <a name="methods"></a>Metody

Nemusí být ideální pro rozhraní API jazyka Objective-C i dobrou názvy rozhraní .NET.

Zásady vytváření názvů v Objective-C se liší od rozhraní .NET (camelCase místo podrobnější pascalcase).
Přečtěte si prosím [kódování pokyny pro kakao](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF).

Z vývojář jazyka Objective-C hlediska, metodu s `Get` předponu znamená instance, tj. nevlastníte [získat pravidlo](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1).

Toto pravidlo pojmenování nemá žádná shoda v rozhraní .NET GC world; Metoda .NET s `Create` předpony budou chovat stejně jako v rozhraní .NET. Pro vývojáře jazyka Objective-C, znamená to však obvykle vlastníte vrácené instance, tj. [vytvořit pravidlo](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029).

## <a name="exceptions"></a>Výjimky

Je celkem běžné v rozhraní .NET použít výjimky hojně zprávy o chybách. Jsou to ale pomalé a není poměrně identické v Objective-c Pokud je to možné by jejich skrytí od vývojáře jazyka Objective-C.

Například .NET `Try` vzor bude mnohem jednodušší využívat z kódu jazyka Objective-C:

```csharp
public int Parse (string number)
{
  return Int32.Parse (number);
}
```

porovnání

```csharp
public bool TryParse (string number, out int value)
{
  return Int32.TryParse (number, out value);
}
```

### <a name="exceptions-inside-init"></a>Výjimky uvnitř `init*`

V rozhraní .NET konstruktor musí buď úspěšné a vrátit (_zpravidla_) platnou instanci nebo throw výjimku.

Naproti tomu jazyka Objective-C umožňuje `init*` vrátit `nil` když nelze vytvořit instanci. To je běžné, ale není obecné vzor používán v mnoha architektur společnosti Apple. V některých jiných případech `assert` se může vyskytnout (a ukončit aktuální proces).

Generátor podle stejné `return nil` vzor pro nevygeneruje `init*` metody. Pokud je vyvolána výjimka spravované, pak budou vytištěny (pomocí `NSLog`) a `nil` bude vrácen volajícímu.

## <a name="operators"></a>Operátory

Jazyka Objective-C neumožňuje operátory být přetíženy stejně C#, tak ty jsou převedeny na třídu selektorů.

["Popisný"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) pojmenované metody jsou generovány přednostně přetížení operátoru při nalezena a může vytvářet snadněji používat rozhraní API.

Třídy, které potlačí operátory `==` nebo `!=` by měly přepsat také standardní metody Equals (objekt).
