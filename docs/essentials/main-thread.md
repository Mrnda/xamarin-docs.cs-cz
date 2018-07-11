---
title: 'Xamarin.Essentials: MainThread'
description: Třída MainThread umožňuje spouštět kód na hlavní prováděcího vlákna aplikace.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: charlespetzold
ms.author: chape
ms.date: 06/26/2018
ms.openlocfilehash: e07d36d3e9a5492e6e170b62dbacb36be44dbfa9
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831422"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**MainThread** třída umožňuje spouštět kód na hlavní vlákno provádění aplikace, a určete, jestli určitého bloku kódu je momentálně spuštěný v hlavním vlákně.

## <a name="background"></a>Pozadí

Většina operačních systémů, včetně iOS, Android a univerzální platformu Windows – používají model dělení na vlákna jedním pro kód zahrnující uživatelského rozhraní. Tento model je potřeba správně serializovat události uživatelského rozhraní, včetně stisknutí kláves a dotykovým ovládáním. Toto vlákno je často volá _hlavního vlákna_ nebo _vlákna uživatelského rozhraní_ nebo _vlákno uživatelského rozhraní_. Nevýhodou tohoto modelu je, že veškerý kód, který přistupuje k prvky uživatelského rozhraní, musíte spustit na hlavního vlákna aplikace. 

Někdy potřeba aplikace použijte události, které se volají obslužné rutiny události na sekundární vlákno provádění. (Třídy Xamarin.Essentials [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md), [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), a [ `OrientationSensor` ](orientation-sensor.md) všechny může vrátit informace na sekundární vlákno při použití s vyšší rychlost.) Pokud obslužná rutina události potřebuje pro přístup k prvkům uživatelského rozhraní, spuštěním tohoto kódu v hlavním vlákně. **MainThread** třída umožňuje, aby aplikace tento kód spustit na hlavním vlákně.

## <a name="running-code-on-the-main-thread"></a>Spouštění kódu v hlavním vlákně

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Chcete-li spustit kód na hlavním vlákně, zavolejte statickou `MainThread.BeginInvokeOnMainThread` metody. Argument je [ `Action` ](xref:System.Action) objekt, který je jednoduše metodou bez argumentů a žádnou návratovou hodnotu:

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Je také možné definovat zvláštní metodu pro kód, který musí být spuštěn v hlavním vlákně:

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

Poté lze tuto metodu v hlavním vlákně odkazem v `BeginInvokeOnMainThread` metody:

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms obsahuje metodu nazvanou [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) , který provede stejnou akci jako `MainThread.BeginInvokeOnMainThread(Action)`. Při použití některé z metod můžete v aplikaci Xamarin.Forms, zvažte, jestli má volající kód jiných potřebu závislost na Xamarin.Forms. V opačném případě `MainThread.BeginInvokeOnMainThread(Action)` je pravděpodobně vhodnější.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Určení, zda je kód spuštěn na hlavní vlákno

`MainThread` Třída rovněž umožňuje aplikaci k určení, zda je spuštěn určitého bloku kódu na hlavním vlákně. `IsMainThread` Vrátí vlastnost `true` Pokud kód volá vlastnost běží na hlavním vlákně. Program pro spuštění různých kódu pro hlavní vlákno nebo sekundární vlákno pomocí této vlastnosti:

```csharp
if (MainThread.IsMainThread)
{
    // Code to run if this is the main thread
}
else
{
    // Code to run if this is a secondary thread
}
```

Může vás zajímat, pokud by měl zkontrolovat, zda je kód spuštěn na sekundární vlákna před voláním `BeginInvokeOnMainThread`, například tímto způsobem:

```csharp
if (MainThread.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

Možná máte podezření, že tato kontrola může zvýšit výkon, pokud blok kódu už běží na hlavním vlákně.

_Tato kontrola však není nutné._ Implementace platformy `BeginInvokeOnMainThread` sami zaškrtněte, pokud jste volání provedli v hlavním vlákně. Je velmi málo snížení výkonu, pokud zavoláte `BeginInvokeOnMainThread` když to není nezbytně nutné.

## <a name="api"></a>rozhraní API

- [MainThread zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [Dokumentace k rozhraní API MainThread](xref:Xamarin.Essentials.MainThread)
