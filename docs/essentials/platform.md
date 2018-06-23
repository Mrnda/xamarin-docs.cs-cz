---
title: Xamarin.Essentials platformy
description: Třída platformy umožňuje aplikacím spustit kód na hlavní prováděcí vlákno.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E0
author: charlespetzold
ms.author: chape
ms.date: 06/03/2018
ms.openlocfilehash: 82e248ee702e104dff98b342aec72179273fc34f
ms.sourcegitcommit: d9ecac62bcf743aff52408fbbd09c5509921a000
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/23/2018
ms.locfileid: "36329937"
---
# <a name="xamarinessentials-platform"></a>Xamarin.Essentials platformy

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Platformy** třída umožňuje aplikacím spustit kód na hlavní vlákno provádění a k určení, zda určitého bloku kódu je aktuálně spuštěné na hlavní vlákno.

## <a name="background"></a>Pozadí

Většina operačních systémů – včetně iOS, Android a univerzální platformu Windows – používá model jednoho vlákna pro kód zahrnující uživatelské rozhraní. Tento model je potřeba správně serializovat události uživatelského rozhraní, včetně stisknutí kláves a touch vstup. Tento přístup z více vláken se často říká _hlavního vlákna_ nebo _vlákna uživatelského rozhraní_ nebo _vlákna uživatelského rozhraní_. Nevýhodou tohoto modelu je, že veškerý kód, který přistupuje k prvky uživatelského rozhraní, musíte spustit na hlavní vlákno aplikace. 

Aplikace se někdy muset použít události, které volají obslužné rutiny události na sekundární podproces provádění. (Třídy Xamarin.Essentials [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md), [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), a [ `OrientationSensor` ](orientation-sensor.md) všechny může vrátit informace na sekundární vlákno při použití s vyšší rychlost.) Pokud obslužná rutina události potřebuje přístup k elementům uživatelského rozhraní, tento kód musí běžet na hlavní vlákno. **Platformy** třída umožňuje aplikaci spustit tento kód na hlavní vlákno.

## <a name="running-code-on-the-main-thread"></a>Spuštění kódu na hlavní vlákno

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Pokud chcete spustit kód na hlavní vlákno, zavolejte statickou `Platform.BeginInvokeOnMainThread` metoda. Argument je [ `Action` ](xref:System.Action) objekt, který je jednoduše metoda s bez argumentů a žádnou návratovou hodnotu:

```csharp
Platform.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Pokud je třeba volat tuto metodu v aplikaci Xamarin.Forms v rámci třídy, která je odvozena z `Element` (který zahrnuje všechny třídu odvozenou od `View` nebo `Page`), `Platform` název třídy musí být plně kvalifikovaný s Název oboru názvů:

```csharp
Xamarin.Essentials.Platform.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Je také možné definovat samostatnou metodu pro kód, který se musí spustit na hlavní vlákno:

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

Potom můžete spustit tuto metodu na hlavní vlákno v odkazem `BeginInvokeOnMainThread` metoda:

```csharp
Xamarin.Essentials.Platform.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms má metodu s názvem [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) který provádí stejnou funkci jako `Xamarin.Essentials.Platform.BeginInvokeOnMainThread(Action)`. I když můžete pomocí těchto metod v aplikaci Xamarin.Forms, zvažte, zda kód volání má jiné potřebu závislost na platformě Xamarin.Forms. Pokud ne, `Xamarin.Essentials.Platform.BeginInvokeOnMainThread(Action)` je pravděpodobně lepší volbou.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Určení, zda kód je spuštěn na hlavní vláken

`Platform` Třída rovněž umožňuje aplikaci k určení, zda je spuštěn určitého bloku kódu na hlavní vlákno. `IsMainThread` Vlastnost vrátí `true` Pokud kód volání vlastnost běží na hlavní vlákno. Program můžete spouštět různé kód pro hlavní vlákno nebo jiného vlákna tuto vlastnost:

```csharp
if (Xamarin.Essentials.Platform.IsMainThread)
{
    // Code to run if this is the main thread
}
else
{
    // Code to run if this is a secondary thread
}
```

Může vás zajímat, pokud byste měli zkontrolovat, zda kód je spuštěn na sekundární vlákno před voláním `BeginInvokeOnMainThread`, například tímto způsobem:

```csharp
if (Xamarin.Essentials.Platform.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    Xamarin.Essentials.Platform.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

Může předpokládat, že tato kontrola může zvýšit výkon, pokud blok kódu na hlavní vlákno je již spuštěna.

_Tato kontrola však není nutné._ Implementace platformy `BeginInvokeOnMainThread` sami zaškrtněte, pokud je volání na hlavní vlákno. Je velmi malé snížení výkonu při volání `BeginInvokeOnMainThread` při není nezbytně nutné.

## <a name="api"></a>rozhraní API

- [Zdrojový kód platformy](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Platform)
- [Dokumentace rozhraní API platformy](xref:Xamarin.Essentials.Platform)
