---
title: "přepínače"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0cf1c4d749eb85a7e0f4c035e10e2e7a40e0c711
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="switch"></a>přepínače

`Switch` Pomůcky (zobrazené dole) umožňuje uživateli přepínat mezi dvěma stavy, například jako na nebo vypnout. `Switch` Výchozí hodnota je OFF. Widgetu jsou uvedeny níže v jeho ON a OFF stavy:

[![Snímky obrazovky se widget přepínače a ZAPNĚTE stavy](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>Vytváření přepínač

K vytvoření přepínače, jednoduše deklarovat `Switch` elementu v kódu XML následujícím způsobem:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

Tím se vytvoří základní přepínač jak je uvedeno níže:

[![Snímek obrazovky zobrazení přepínač ve stavu OFF ukázkovou aplikaci](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>Změna výchozí hodnoty

Text, který zobrazí ovládací prvek pro ON a OFF stavy a výchozí hodnota se dají konfigurovat. Chcete-li přepínač výchozí na hodnotu ON a čtení Ano-Ne místo OFF/ON, například můžete nastaví `checked`, `textOn`, a `textOff` atributy v následující soubor XML.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>Poskytuje název

`Switch` Pomůcka podporuje také včetně textový popisek nastavením `text` atribut následujícím způsobem:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Tato značka vytvoří za běhu na následujícím snímku obrazovky:

[![Snímek obrazovky ukázkové aplikace s textem vodorovně předcházející widgetu přepínače](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

Když `Switch`na změny hodnot, vyvolá `CheckedChange` událostí.
Například následující kód jsme zachycování této události a k dispozici `Toast` na základě pomůcky zprávou `isChecked` hodnotu `Switch`, který je předat do obslužné rutiny události v rámci `CompoundButton.CheckedChangeEventArg` argument.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>Související odkazy

- [SwitchDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/SwitchDemo/)
- [Karta rozložení kurzu](~/android/user-interface/layouts/tab-layout/index.md)
- [Představení Sandwichovy Zmrzlinová](http://www.android.com/about/ice-cream-sandwich/)
- [Platformu Android 4.0](http://developer.android.com/sdk/android-4.0.html)
