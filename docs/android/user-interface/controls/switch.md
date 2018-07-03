---
title: Přepínač
description: Tom, jak pomocí widgetu přepínače do aplikace Xamarin.Android
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/29/2018
ms.openlocfilehash: e3bcce48a675a9ba3d1d41f93babc7fcb26448c8
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403273"
---
# <a name="switch"></a>Přepínač

`Switch` Widgetu (viz dole) umožňuje uživatelům přepínat mezi dvěma stavy, například jako na nebo vypnuto. `Switch` Výchozí hodnota je OFF. Ve widgetu najdete níž v jeho ON a OFF stavů:

[![Snímky obrazovky widgetu přepínač v VYPÍNAT a ZAPÍNAT stavy](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>Vytváření přepínačů

Chcete-li vytvořit přepínač, stačí deklarovat `Switch` element v XML následujícím způsobem:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

To vytvoří základní přepínač, jak je znázorněno níže:

[![Snímek obrazovky s ukázkovou aplikaci, zobrazení přepínače ve stavu vypnuto](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>Změnou výchozích hodnot

Text, který ovládací prvek zobrazuje pro ON a OFF států a výchozí hodnota je možné konfigurovat. Například, chcete-li přepínač zapnuto ve výchozím nastavení a čtení č/Ano místo OFF/ON, můžete nastavíme `checked`, `textOn`, a `textOff` atributy v následující kód XML.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>Poskytuje název

`Switch` Widgetu podporuje také třeba popisek s textem tak, že nastavení `text` atribut následujícím způsobem:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Tento kód vytvoří za běhu na následujícím snímku obrazovky:

[![Snímek obrazovky s ukázkovou aplikaci s textem vodorovně před ovládacím prvku přepínač](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

Při `Switch`na změny hodnot, vyvolá `CheckedChange` událostí.
Například v následujícím kódu jsme zachycování této události a prezentovat `Toast` widgetů a zobrazí se zpráva na základě `isChecked` hodnotu `Switch`, který je předán do obslužné rutiny události v rámci `CompoundButton.CheckedChangeEventArg` argument.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>Související odkazy

- [SwitchDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/SwitchDemo/)
- [Kurz rozložení karty](~/android/user-interface/layouts/tab-layout/index.md)
