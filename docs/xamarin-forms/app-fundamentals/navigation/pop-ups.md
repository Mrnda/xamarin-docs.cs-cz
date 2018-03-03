---
title: "Zobrazení automaticky otevíraná okna"
description: "Xamarin.Forms poskytuje dva pop množství jako elementy uživatelského rozhraní – výstrahu a akci listu. Tento článek ukazuje, požádejte uživatele jednoduchých dotazů a uživatele provádějí úlohy pomocí výstrah a akce listu rozhraní API."
ms.topic: article
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 957db750b852b40daf1556e8dc8f7ba18e022dba
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="displaying-pop-ups"></a>Zobrazení automaticky otevíraná okna

_Xamarin.Forms poskytuje dva pop množství jako elementy uživatelského rozhraní – výstrahu a akci listu. Tento článek ukazuje, požádejte uživatele jednoduchých dotazů a uživatele provádějí úlohy pomocí výstrah a akce listu rozhraní API._

Zobrazení výstrahy nebo požádat uživatele k provedení volby je běžné úkol uživatelského rozhraní. Xamarin.Forms má dvě metody na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) třídou pro interakci s uživatelem prostřednictvím automaticky otevíraného okna: [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) a [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/). Budou se vykreslují odpovídající nativní ovládací prvky na každou platformu.

## <a name="displaying-an-alert"></a>Zobrazení výstrahy

Všechny platformy podporované platformě Xamarin.Forms mít modální automaticky otevírané okno upozornění uživatele nebo se zeptejte jednoduché dotazy. Chcete-li zobrazit tyto výstrahy v Xamarin.Forms, použijte [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) metoda na žádném [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Následující řádek kódu ukazuje jednoduchý zpráva uživateli:

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Výstrahy dialogové okno s jedno tlačítko")

Tento příklad neshromažďuje informace od uživatele. Oznámení zobrazí modálně a jakmile se zavře uživatele pokračuje v interakci s aplikací.

[ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) Metoda můžete používá i k zachycení odpověď uživatele tak, že nabízí dvě tlačítka a vrácení `boolean`. Získání odezvy z výstrahy, zadejte text pro obě tlačítka a `await` metodu. Poté, co uživatel vybere jednu z možností odpověď se vrátí do vašeho kódu. Poznámka: `async` a `await` klíčová slova v ukázkovém kódu níže:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[ ![DisplayAlert](pop-ups-images/alert2-sml.png "výstrahy dialogové okno s dvě tlačítka")](pop-ups-images/alert2.png "výstrahy dialogové okno s dvě tlačítka")

## <a name="guiding-users-through-tasks"></a>Pokyny uživateli prostřednictvím úlohy

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) je běžné elementu uživatelského rozhraní v iOS. Platformě Xamarin.Forms [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/) metoda umožňuje zahrnout tento ovládací prvek v aplikacích pro různé platformy, vykreslování nativní alternativy v Android a Windows Phone.

Zobrazte seznam akci `await` [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/) v žádném [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), předávání zprávy a tlačítko popisky jako řetězce. Metoda vrátí řetězec popisku tlačítka, které bylo kliknuto uživatelem. Zde je uveden jednoduchý příklad:

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "Dialogové okno ActionSheet")

`destroy` Tlačítko vykreslením jinak než ostatní a může být ponecháno `null` nebo je zadána jako třetí parametr řetězce. Následující příklad používá `destroy` tlačítko:

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[ ![DisplayActionSheet](pop-ups-images/action2-sml.png "akce list dialogové okno s Destroy tlačítko")](pop-ups-images/action2.png "akce list dialogové okno s Destroy tlačítko")

## <a name="summary"></a>Souhrn

Tento článek ukázal, požádejte uživatele jednoduchých dotazů a uživatele provádějí úlohy pomocí výstrah a akce listu rozhraní API. Xamarin.Forms má dvě metody na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) třídou pro interakci s uživatelem prostřednictvím automaticky otevíraného okna: [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) a [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/), a jsou obě vykreslují odpovídající nativní ovládací prvky na každou platformu.



## <a name="related-links"></a>Související odkazy

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
