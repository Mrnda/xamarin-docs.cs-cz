---
title: Zobrazování automaticky otevíraných oken
description: Xamarin.Forms poskytuje dva pop registrace jako prvky uživatelského rozhraní – upozornění a listu akce. Tento článek ukazuje, požádejte uživatele jednoduché otázky a provedou uživatele úkoly pomocí výstrahy a akce listu rozhraní API.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 156c2f9dca47a7755d4f810d7921a05662388ded
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996711"
---
# <a name="displaying-pop-ups"></a>Zobrazování automaticky otevíraných oken

_Xamarin.Forms poskytuje dva pop registrace jako prvky uživatelského rozhraní – upozornění a listu akce. Tento článek ukazuje, požádejte uživatele jednoduché otázky a provedou uživatele úkoly pomocí výstrahy a akce listu rozhraní API._

Zobrazení výstrahy nebo s dotazem, uživatel rozhodl je běžný úkol uživatelského rozhraní. Xamarin.Forms má dvě metody na [ `Page` ](xref:Xamarin.Forms.Page) třídou pro interakci s uživatelem přes automaticky otevírané okno: [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) a [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*). Jejich vykreslením s odpovídající nativní ovládací prvky na jednotlivých platformách.

## <a name="displaying-an-alert"></a>Zobrazení výstrahy

Všechny podporované Xamarin.Forms platformy mají modální automaticky otevírané okno pro upozornění uživatele nebo jednoduché otázky z nich. Chcete-li zobrazit tyto výstrahy v Xamarin.Forms, použijte [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) metoda na žádném [ `Page` ](xref:Xamarin.Forms.Page). Následující řádek kódu ukazuje jednoduchý zpráva uživateli:

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Dialogového okna výstrah s jedno tlačítko")

V tomto příkladu neshromažďuje informace od uživatele. Zobrazí upozornění modálně a když uživatel zavře pokračuje v práci s aplikací.

[ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) Metodu můžete také použije pro zachycení odpověď uživatele tak, že nabízí ten samý dvou tlačítek a vrácení `boolean`. Chcete-li získat odpověď z výstrahy, zadejte text pro obě tlačítka a `await` metodu. Když uživatele vybere jednu z možností odpověď se vrátí do vašeho kódu. Poznámka: `async` a `await` klíčových slovech ve níže uvedený ukázkový kód:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "upozornění dialogové okno s dvě tlačítka")](pop-ups-images/alert2.png#lightbox "upozornění dialogové okno s dvě tlačítka")

## <a name="guiding-users-through-tasks"></a>Průvodce uživatele prostřednictvím úkolů

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) je běžné prvek uživatelského rozhraní v Iosu. Xamarin.Forms [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*) metoda umožňuje zahrnout tohoto ovládacího prvku v aplikacích pro různé platformy, vykreslování nativní alternativy v Android a UPW.

Zobrazte seznam akce `await` [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*) v libovolném [ `Page` ](xref:Xamarin.Forms.Page), předejte zprávu a tlačítko popisky jako řetězce. Metoda vrátí řetězec popisku tlačítka, které bylo kliknuto uživatelem. Jednoduchý příklad můžete vidět tady:

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "Dialogové okno ActionSheet")

`destroy` Tlačítko vykreslením jinak než ostatní a můžete ho nechat `null` nebo zadané jako třetí parametr řetězce. V následujícím příkladu `destroy` tlačítka:

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "dialogové okno akce list s tlačítkem Destroy")](pop-ups-images/action2.png#lightbox "dialogové okno akce list s tlačítkem Destroy")

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali, požádejte uživatele jednoduché otázky a provedou uživatele úkoly pomocí výstrahy a akce listu rozhraní API. Xamarin.Forms má dvě metody na [ `Page` ](xref:Xamarin.Forms.Page) třídou pro interakci s uživatelem přes automaticky otevírané okno: [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) a [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*), a jsou obě vykreslen pomocí odpovídající nativní ovládací prvky na jednotlivých platformách.



## <a name="related-links"></a>Související odkazy

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
