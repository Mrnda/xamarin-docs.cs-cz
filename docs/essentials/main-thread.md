---
title: 'Xamarin.Essentials: MainThread'
description: Třída MainThread umožňuje aplikacím spustit kód na hlavní prováděcí vlákno.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: charlespetzold
ms.author: chape
ms.date: 06/26/2018
ms.openlocfilehash: e07d36d3e9a5492e6e170b62dbacb36be44dbfa9
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080372"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**MainThread** třída umožňuje aplikacím spustit kód na hlavní vlákno provádění a k určení, zda určitého bloku kódu je aktuálně spuštěné na hlavní vlákno.

## <a name="background"></a>Pozadí

Většina operačních systémů – včetně iOS, Android a univerzální platformu Windows – používá model jednoho vlákna pro kód zahrnující uživatelské rozhraní. Tento model je potřeba správně serializovat události uživatelského rozhraní, včetně stisknutí kláves a touch vstup. Tento přístup z více vláken se často říká _hlavního vlákna_ nebo _vlákna uživatelského rozhraní_ nebo _vlákna uživatelského rozhraní_. Nevýhodou tohoto modelu je, že veškerý kód, který přistupuje k prvky uživatelského rozhraní, musíte spustit na hlavní vlákno aplikace. 

Aplikace se někdy muset použít události, které volají obslužné rutiny události na sekundární podproces provádění. (Třídy Xamarin.Essentials [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md), [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), a [ `OrientationSensor` ](orientation-sensor.md) všechny může vrátit informace na sekundární vlákno při použití s vyšší rychlost.) Pokud obslužná rutina události potřebuje přístup k elementům uživatelského rozhraní, tento kód musí běžet na hlavní vlákno. **MainThread** třída umožňuje aplikaci spustit tento kód na hlavní vlákno.

## <a name="running-code-on-the-main-thread"></a>Spuštění kódu na hlavní vlákno

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Pokud chcete spustit kód na hlavní vlákno, zavolejte statickou `MainThread.BeginInvokeOnMainThread` metoda. Argument je [ `Action` ](xref:System.Action) objekt, který je jednoduše metoda s bez argumentů a žádnou návratovou hodnotu:

```csharp
MainThread.BeginInvokeOnMainThread(() =>
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
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms má metodu s názvem [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) který provádí stejnou funkci jako `MainThread.BeginInvokeOnMainThread(Action)`. I když můžete pomocí těchto metod v aplikaci Xamarin.Forms, zvažte, zda kód volání má jiné potřebu závislost na platformě Xamarin.Forms. Pokud ne, `MainThread.BeginInvokeOnMainThread(Action)` je pravděpodobně lepší volbou.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Určení, zda kód je spuštěn na hlavní vláken

`MainThread` Třída rovněž umožňuje aplikaci k určení, zda je spuštěn určitého bloku kódu na hlavní vlákno. `IsMainThread` Vlastnost vrátí `true` Pokud kód volání vlastnost běží na hlavní vlákno. Program můžete spouštět různé kód pro hlavní vlákno nebo jiného vlákna tuto vlastnost:

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

Může vás zajímat, pokud byste měli zkontrolovat, zda kód je spuštěn na sekundární vlákno před voláním `BeginInvokeOnMainThread`, například tímto způsobem:

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

Může předpokládat, že tato kontrola může zvýšit výkon, pokud blok kódu na hlavní vlákno je již spuštěna.

_Tato kontrola však není nutné._ Implementace platformy `BeginInvokeOnMainThread` sami zaškrtněte, pokud je volání na hlavní vlákno. Je velmi malé snížení výkonu při volání `BeginInvokeOnMainThread` při není nezbytně nutné.

## <a name="api"></a>rozhraní API

- [MainThread zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [Dokumentaci k rozhraní API MainThread](xref:Xamarin.Essentials.MainThread)
