---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 85c505d03e7a763b24fb176b6a94c0fe43009b79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="checkbox"></a>CheckBox

V této části vytvoříte zaškrtávací políčko pro výběr položek, pomocí [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox) pomůcky. Při stisknutí políčka informační zpráva označí aktuální stav políčka.

Otevřete **Resources/layout/Main.axml** souboru a přidejte [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) – element (uvnitř [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout)):

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

Udělat něco při změně stavu, přidejte následující kód do konce [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) metoda:

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

To zaznamená [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) element z rozložení, pak klikněte na událost, která definuje akce prováděné při kliknutí na políčko zpracovává. Při kliknutí [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) vlastnost nazývá zjištění nového stavu zaškrtávacího políčka. Pokud byla ověřena, pak [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) zobrazí zpráva "Vybrané", jinak se zobrazí "Není vybrán". [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) Zpracovává vlastní změny stavu, takže potřebujete dotazování na aktuální stav.

Spusťte ji.

**Tip:** Pokud potřebujete změnit stav (například při načítání uložené [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference), použijte [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked) Metoda setter vlastnosti nebo [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle) metoda.

*Úpravy, které jsou na základě práce vytvořen a sdílí projektu pro Android otevřít zdroje a používají podle podmínek, které jsou popsané v části této stránky jsou*
[*Creative Commons 2.5 porušení licence* ](http://creativecommons.org/licenses/by/2.5/).
