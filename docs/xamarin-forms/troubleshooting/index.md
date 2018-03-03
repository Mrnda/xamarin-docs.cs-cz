---
title: "Poradce při potížích"
description: "Běžné chybové stavy a způsob jejich řešení"
ms.topic: article
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 23ebefcbd6114b06c39740b3b56f87aeac0b9a00
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Poradce při potížích

_Běžné chybové stavy a způsob jejich řešení_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>Chyba: "nelze najít verzi Xamarin.Forms kompatibilní s..."

Tyto chyby se mohou objevit v **konzoly balíčků** okno při aktualizaci všech balíčků Nuget v řešení Xamarin.Forms nebo v projektu Xamarin.Forms Android aplikace:

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>Co způsobí, že tuto chybu?

Visual Studio pro Mac (nebo v sadě Visual Studio) může znamenat, že jsou aktualizace dostupné pro packge Xamarin.Forms Nuget *a všechny jeho závislé součásti*. V Xamarin studiu řešení na **balíčky** uzlu může vypadat takto (čísel verzí může být odlišné):

![](images/updates-available.png "Složka balíčky projekt pro Android")

Této chybě může dojít, pokud se pokus o aktualizaci _všechny_ balíčky.

Důvodem je, že s Android projekty nastaven na cíl nebo kompilace verze Android 6.0 (API 23) nebo níže, Xamarin.Forms má pevný závislost *konkrétní* verze Android podpůrných balíčků. I když může být k dispozici aktualizované verze těchto balíčků, není Xamarin.Forms nemusí být kompatibilní s nimi.

V takovém případě by měl aktualizovat _pouze_ **Xamarin.Forms** balíčku, tím totiž zajistíte, že zůstanou závislosti na kompatibilní verze. Další balíčky, které jste přidali do projektu může být i jednotlivě, dokud nezpůsobí Android, podporují balíčků aktualizace.


> [!NOTE]
> Pokud používáte Xamarin.Forms 2.3.4 nebo novější **a** verze cílového/kompilace projektu Android je nastavená na Android 7.0 (24 rozhraní API) nebo novější, potom použít pevný závislostí uvedených výše už a aktualizujete podpory balíčky nezávisle na platformě Xamarin.Forms balíčku.


### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>Oprava: Odeberte všechny balíčky a znovu přidat Xamarin.Forms

Pokud **Xamarin.Android.Support** balíčky byly aktualizovány na nekompatibilní verze, je nejjednodušší potíže:

1. Odstraňte ručně všechny balíčky Nuget v projektu pro Android, pak
2. Znovu přidat **Xamarin.Forms** balíčku.

To bude automaticky stáhnout *správné* verze balíčků.

Podívejte se na video tohoto procesu, najdete v tématu to [fóra post](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012).
