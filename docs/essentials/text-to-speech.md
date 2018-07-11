---
title: 'Xamarin.Essentials: převod textu na řeč'
description: Třída TextToSpeech v Xamarin.Essentials umožňuje využívat v převod textu na řeč moduly mluvit back text ze zařízení a také pro dotaz dostupné jazyky, které podporují modul integrované aplikace.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 9383411074bc43af1034138aadbb6ac5494c2c01
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815658"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials: převod textu na řeč

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**TextToSpeech** třída umožňuje aplikaci využívat integrovaná v převod textu na řeč moduly mluvit back text ze zařízení a také pro dotaz dostupné jazyky, které podporují modul.

## <a name="using-text-to-speech"></a>Pomocí převod textu na řeč

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Funkce pro převod textu na řeč funguje tak, že volání `SpeakAsync` metoda s textem a volitelné parametry a vrátí po dokončení zpracování utterance. 

```csharp
public async Task SpeakNowDefaultSettings()
{
    await TextToSpeech.SpeakAsync("Hello World");

    // This method will block until utterance finishes.
}

public void SpeakNowDefaultSettings2()
{
    TextToSpeech.SpeakAsync("Hello World").ContinueWith((t) => 
    {
        // Logic that will run after utterance finishes.

    }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

Tato metoda přijímá volitelný CancellationToken zastavit utterance po jeho spuštění. 
```csharp
CancellationTokenSource cts;
public async Task SpeakNowDefaultSettings()
{
    cts = new CancellationTokenSource();
    await TextToSpeech.SpeakAsync("Hello World", cancelToken: cts.Token);

    // This method will block until utterance finishes.
}

public void CancelSpeech()
{
    if (cts?.IsCancellationRequested ?? false)
        return;

    cts.Cancel();
}
```

Převod textu na řeč automaticky fronty hlasové požadavky ze stejného podprocesu. 

```csharp
bool isBusy = false;
public void SpeakMultiple()
{
    isBusy = true;
    Task.Run(async () =>
    {
        await TextToSpeech.SpeakAsync("Hello World 1");
        await TextToSpeech.SpeakAsync("Hello World 2");
        await TextToSpeech.SpeakAsync("Hello World 3");
        isBusy = false;
    });

    // or you can query multiple without a Task:
    Task.WhenAll(
        TextToSpeech.SpeakAsync("Hello World 1"),
        TextToSpeech.SpeakAsync("Hello World 2"),
        TextToSpeech.SpeakAsync("Hello World 3"))
        .ContinueWith((t) => { isBusy = false; }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

### <a name="speech-settings"></a>Nastavení řeči

Větší kontrolu nad tím, jak je slyšet zvuk zpět s `SpeakSettings` , který umožňuje nastavení svazku, výšku a národní prostředí.

```csharp
public async Task SpeakNow()
{
    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

Tady jsou podporované hodnoty pro tyto parametry:

| Parametr | Minimální | Maximum |
| --- | :---: | :---: |
| Výška | 0 | 2.0 |
| Svazek | 0 | 1.0 |

### <a name="speech-locales"></a>Národní prostředí řeči

Každá platforma nabízí národní prostředí mluvit back text v několika jazycích a zvýraznění. Každá platforma má jiné kódy a způsoby pro určení toho, což je důvod, proč Essentials poskytuje napříč platformami `Locale` třídy a způsob, jak dotazovat je pomocí `GetLocalesAsync`.

```csharp
public async Task SpeakNow()
{
    var locales = await TextToSpeech.GetLocalesAsync();

    // Grab the first locale
    var locale = locales.FirstOrDefault();

    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0,
            Locale = locale
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

## <a name="limitations"></a>Omezení

- Fronty utterance není zaručeno, pokud volá napříč více vlákny.
- Přehrávání zvuku na pozadí není oficiálně podporován.

## <a name="api"></a>rozhraní API

- [TextToSpeech zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [Dokumentace k rozhraní API TextToSpeech](xref:Xamarin.Essentials.TextToSpeech)
