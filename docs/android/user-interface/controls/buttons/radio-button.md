---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 1267491f2d9b7519f76651df059722420fa8e1eb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="radiobutton"></a>RadioButton

V této části vytvoříte dvě vzájemně se vylučující přepínací tlačítka (povolení jeden zakáže dalších), pomocí [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) a [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) pomůcky. Při stisknutí tlačítka buď přepínačů, zobrazí se informační zpráva.


Otevřete **Resources/layout/Main.axml** souboru a přidejte dva [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s, vnořených v [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) (uvnitř [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<RadioGroup
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"
  android:orientation="vertical">
  <RadioButton android:id="@+id/radio_red"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Red" />
  <RadioButton android:id="@+id/radio_blue"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Blue" />
</RadioGroup>
```

Je důležité, který [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s jsou seskupeny podle [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) element tak, aby více než jeden lze vybrat současně. Tato logika prováděno automaticky v systému Android. Pokud jeden [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) v rámci vybrána skupina, všechny ostatní jsou automaticky odznačené.

Něco udělat při každém [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) je vybrána, potřebujeme zápis obslužné rutiny události:

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

Nejprve odesílatele, je předaná vložena do přepínač.
Pak [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) zpráva zobrazí text tlačítka vybraného přepínače.

Nyní, v dolní části [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) metoda, přidejte následující:

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

To zaznamená všechny [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s z rozložení a přidá handlerto nově vytvořený událostí každý.

Spusťte aplikaci.

**Tip:** Pokud potřebujete změnit stav (například při načítání uložené [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)), použijte [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) Metoda setter vlastnosti nebo [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/) metoda.

*Úpravy, které jsou na základě práce vytvořen a sdílí projektu pro Android otevřít zdroje a používají podle podmínek, které jsou popsané v části této stránky jsou*
[*Creative Commons 2.5 porušení licence* ](http://creativecommons.org/licenses/by/2.5/). 
