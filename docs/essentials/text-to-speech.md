---
title: Převod textu na řeč Xamarin.Essentials
description: Třída TextToSpeech umožňuje aplikace využívat předdefinovaných v převod textu na řeč moduly mluvit back text ze zařízení a také dotazu dostupné jazyky, které mohou podporovat modul.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b2c9ed50c48aee6343a20ddb28c49e1bd05d2153
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-text-to-speech"></a>Převod textu na řeč Xamarin.Essentials

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**TextToSpeech** třída umožňuje aplikaci využívat předdefinovaných v převod textu na řeč moduly mluvit back text ze zařízení a také dotazu dostupné jazyky, které mohou podporovat modul.

## <a name="using-text-to-speech"></a>Mluvené slovo

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Převod textu na řeč funkce funguje tak, že volání `SpeakAsync` metoda s textem a volitelné parametry a vrátí po dokončení utterance. 

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

Tato metoda přebírá volitelné CancellationToken zastavit utterance po jeho spuštění. 
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

Převod textu na řeč, bude automaticky fronty řeči požadavky ze stejného podprocesu. 

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

### <a name="speech-settings"></a>Rozpoznávání řeči nastavení

Pro větší kontrolu nad jak oznamována zvukovému záznamu zpět s `SpeakSettings` umožňuje nastavení svazek, výšky a národní prostředí.

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

### <a name="speech-locales"></a>Rozpoznávání řeči národní prostředí

Každá platforma nabízí národní prostředí mluvit pozadí textu v několika jazycích a akcenty. Každá platforma má jiné kódy a způsobů určení toho, které je důvod, proč Essentials poskytuje napříč platformami `Locale` třídy a vytvářet dotazy pomocí `GetLocalesAsync`.

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

- Fronty utterance není zaručeno, pokud volána napříč více vláken.
- Přehrávání zvuku pozadí není oficiálně podporován.

## <a name="api"></a>rozhraní API

- [TextToSpeech zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [Dokumentaci k rozhraní API TextToSpeech](xref:Xamarin.Essentials.TextToSpeech)
