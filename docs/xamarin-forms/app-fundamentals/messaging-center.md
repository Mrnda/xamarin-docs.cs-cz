---
title: Xamarin.Forms MessagingCenter
description: Tento článek vysvětluje, jak používat Xamarin.Forms MessagingCenter odesílat a přijímat zprávy a pokuste se snížit párování mezi třídy, jako je zobrazit modely.
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 49a7ecdad53c7820594f7ebc047ae6fbc5a9bc56
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209411"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

_Xamarin.Forms obsahuje jednoduché službou zasílání zpráv odesílat a přijímat zprávy._

<a name="Overview" />

## <a name="overview"></a>Přehled

Xamarin.Forms `MessagingCenter` umožňuje zobrazit modely a další součásti ke komunikaci s, aniž by museli znát něco o sobě navzájem kromě jednoduché kontrakt zprávy.

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>Jak funguje MessagingCenter

Existují dvě části `MessagingCenter`:

-  **Přihlášení k odběru** – přijímat zprávy pomocí určitých podpisu a provedení několika akcí při příjmu. Pro stejnou zprávu můžete být naslouchání více odběrateli.
-  **Odeslat** -publikování zprávy pro naslouchací procesy k provedení akce. Pokud máte předplatné žádné moduly pro naslouchání se ignoruje zprávy.


`MessagingService` Je statická třída s `Subscribe` a `Send` metody, které se používají v rámci řešení.

Zprávy mít řetězec `message` parametr, který se používá jako způsob, jak *adresu* zprávy. `Subscribe` a `Send` metody použijte obecné parametry k podrobnějšímu řízení jak doručování zpráv – dvě zprávy se stejným `message` text, ale jiné obecné argumenty typu nebude doručit do stejné odběratele.

Rozhraní API pro `MessagingCenter` je jednoduchý:

-  Přihlášení k odběru&lt;TSender > (objekt odběratele, řetězce zprávy akce&lt;TSender > zpětného volání, TSender zdroj = null)
-  Přihlášení k odběru&lt;TSender, TArgs > (objekt odběratele, řetězce zprávy akce&lt;TSender, TArgs > zpětného volání, TSender zdroj = null)
-  Odeslat&lt;TSender > (TSender odesílatele, řetězce zprávy)
-  Odeslat&lt;TSender, TArgs > (TSender odesílatele, řetězce zprávy TArgs argumentů)
-  Odhlášení&lt;TSender, TArgs > (objekt odběratele, řetězce zprávy)
-  Odhlášení&lt;TSender > (objekt odběratele, řetězce zprávy)


Tyto metody jsou vysvětleny níže.

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>Pomocí MessagingCenter

Zprávy mohou být odeslány v důsledku interakce uživatele (jako jsou například kliknutí na tlačítko), události systému (například ovládací prvky změny stavu) nebo jiné incident (např. asynchronní stahování dokončení). Odběratelé, kteří mohou naslouchání změnit vzhled uživatelského rozhraní, uložení dat nebo aktivovat jiné operace.

### <a name="simple-string-message"></a>Jednoduchým řetězcem zpráv

Nejjednodušší zpráva obsahuje řetězec v `message` parametr. A `Subscribe` metoda který *naslouchá* pro zprávu jednoduchým řetězcem jsou uvedeny níže - Všimněte si obecného typu zadání odesílatel musí být typu `MainPage`. Všechny třídy v řešení se mohou přihlásit k zprávu pomocí této syntaxe:

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

V `MainPage` třídy následujícím kódem *odešle* zprávy. `this` Parametr je instance `MainPage`.

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

Řetězec nemění – označuje *typ zprávy* a slouží k určení, které Odběratelé, kteří mají obdržet oznámení. Toto řazení zpráv slouží k označení, že některé k události došlo, jako je například "nahrávání dokončena", kdy je potřeba žádné další informace.

### <a name="passing-an-argument"></a>Předáním argumentu

Chcete-li předat argument se zprávou, zadejte argument typu v `Subscribe` obecné argumenty a v podpis akce.

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

K odeslání zprávy s argumentem, zahrnují obecný parametr typu a hodnoty argumentu v `Send` volání metody.

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

Tento jednoduchý příklad používá `string` lze předat argument ale libovolného objektu C#.

### <a name="unsubscribe"></a>Odhlásit

Objekt můžete zrušit odběr podpis zprávy tak, aby se dodávají žádné další zprávy. `Unsubscribe` Syntaxe využívající metody by měly odrážet podpis zprávy (takže třeba zahrnout parametr obecného typu pro argument zprávy).

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>Souhrn

MessagingCenter je jednoduchý způsob, jak snížit párování, zejména mezi modely zobrazení. Slouží k odesílání a příjem zpráv jednoduchý nebo předejte argument mezi třídami. Třídy by měl odhlášení odběru zpráv, které chtějí už přijímat.


## <a name="related-links"></a>Související odkazy

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Ukázky Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
