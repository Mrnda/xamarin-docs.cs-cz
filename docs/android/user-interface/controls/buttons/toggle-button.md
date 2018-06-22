---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8323c456a97a033e19374a4bd3ea7468ecafa608
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762295"
---
# <a name="togglebutton"></a>ToggleButton

V této části vytvoříte určených pro při přepínání mezi dvěma stavy, pomocí tlačítka [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) pomůcky. Tato pomůcka je vynikající alternativa k přepínačů, pokud mají dva jednoduché stavy, které se vzájemně vylučují ("na" a "off", například). Android 4.0 (úroveň rozhraní API 14) byl zaveden alternativu k přepínací tlačítko říká [ `Switch` ](https://developer.xamarin.com/api/type/Android.Widget.Switch/).

Příklad **přepínací tlačítko** si můžete prohlédnout ve dvojici vlevo bitových kopií, při na pravé straně pár bitových kopií představuje příklad **přepínač**:

![Příklady přepínače a ToggleButtons v obou zapnout a vypnout stavy](toggle-button-images/togglebutton-switch.png)  

Řízení, které používá aplikace se stylu. Obě pomůcky jsou funkčně rovnocenné.

Otevřete **Resources/layout/Main.axml** souboru a přidejte [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) – element (uvnitř [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

Udělat něco při změně stavu, přidejte následující kód do konce [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) metoda:

```csharp
ToggleButton togglebutton = FindViewById<ToggleButton>(Resource.Id.togglebutton);

togglebutton.Click += (o, e) => {
    // Perform action on clicks
    if (togglebutton.Checked)
        Toast.MakeText(this, "Checked", ToastLength.Short).Show ();
    else
        Toast.MakeText(this, "Not checked", ToastLength.Short).Show ();
};
```

To zaznamená [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) element z rozložení a klikněte na událost, která definuje akce k provedení při kliknutí na tlačítko zpracovává. V tomto příkladu metoda zkontroluje nový stav na tlačítko, budou zobrazeny [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) zprávu, která ukazuje aktuální stav.

Všimněte si, že [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) změny mezi zaškrtnuto a nezaškrtnuto, vlastní stavu, který je právě požádejte obslužné rutiny.

Spusťte aplikaci.


**Tip:** Pokud potřebujete změnit stav (například při načítání uložené [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)), použijte [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) Metoda setter vlastnosti nebo [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/) metoda.


## <a name="related-links"></a>Související odkazy

- [ToggleButton](http://developer.android.com/reference/android/widget/ToggleButton.html)
- [Přepínač](http://developer.android.com/reference/android/widget/Switch.html)
