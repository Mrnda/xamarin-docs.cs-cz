---
title: NSString
ms.topic: article
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 87cc1a95f250069310941e051dabe9ea588b4709
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="nsstring"></a>NSString

Návrh Xamarin.iOS a Xamarin.Mac volání pro rozhraní API používá ke zveřejnění nativní řetězce typu .NET, `string`, pro zacházení s řetězci v jazyce C# a jinými programovací jazyky rozhraní .NET a vystavit řetězec jako datový typ, který je zveřejněný prostřednictvím rozhraní API místo `NSString` datového typu.


To znamená, že vývojáři by neměl mít zachovat řetězce, které se má použít pro volání do Xamarin.iOS & Xamarin.Mac rozhraní API (Unified) ve zvláštním typem (`Foundation.NSString`), můžete dál používat na Mono `System.String` pro všechny operace a kdykoli rozhraní API v Xamarin.iOS nebo Xamarin.Mac vyžaduje řetězec, má na starosti naše rozhraní API vazby zařazování informace.

Například "text" vlastnost jazyka Objective-C na `UILabel` typu `NSString`, je deklarován takto:

```csharp
@property(nonatomic, copy) NSString *text
```

To je zpřístupněná Xamarin.iOS jako:

```csharp
class UILabel {
    public string Text { get; set; }
}
```

Pozadí v době, provádění tato vlastnost zařazuje řetězec jazyka C# do `NSString` a volání `objc_msgSend` metoda stejným způsobem, který by jazyka Objective-C.

Existuje několik rozhraní API jazyka Objective-C třetích stran, která není využívat `NSString`, ale místo toho využívají řetězec jazyka C ("*char*"). V takových případech můžete použít datový typ řetězec jazyka C#, ale je nutné použít [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) atribut k informování generátor vazby, který tento řetězec nesmí být zařazené jako `NSString`, ale místo toho jako řetězec C.

 <a name="Exceptions_to_the_Rule" />


## <a name="exceptions-to-the-rule"></a>Výjimky z pravidla

V Xamarin.iOS i Xamarin.Mac jsme provedli výjimku pro toto pravidlo. Rozhodnutí mezi při zveřejňujeme `string`s, a když jsme proveďte s výjimkou a vystavit `NSString`s, se provádí v případě `NSString` metoda může být způsobem porovnání ukazatelů místo obsahu porovnání.


Toto může nastat, když rozhraní API jazyka Objective-C používá veřejné `NSString` konstantní jako token, který představuje některá z akcí, namísto porovnávání skutečný obsah řetězce.


V takových případech `NSString` se zveřejňují rozhraní API, a neexistují výjimečných rozhraní API, které mají to. Také si všimněte, že jsou přístupné NSString vlastnosti některé třídy. Ty `NSString` vlastnosti jsou viditelné pro položky, jako jsou oznámení. Ty jsou vlastnosti obvykle vypadat například takto:

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

Oznámení jsou klíče, které se používají pro `NSNotification` třídy, pokud chcete zaregistrovat pro určitá událost se vysílání modulem runtime.

Klíče obvykle vypadat přibližně takto:

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

Jiné místo kde `NSString`s jsou zveřejněné v rozhraní API je jako tokeny, které se používají jako parametry pro určité rozhraní API v iOS nebo OS X, které provést `NSDictionary` objektů jako parametry. Slovník obvykle obsahuje `NSString` klíče. Xamarin.iOS, podle konvence názvy ty statické `NSString` vlastnosti přidáním názvu "Klíč".
