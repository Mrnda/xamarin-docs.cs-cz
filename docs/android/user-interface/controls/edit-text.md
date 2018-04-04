---
title: Upravit Text
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d6be8ae1587742a8c2a37b22b2da3701187dde2a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="edit-text"></a>Upravit Text

V této části vytvoříte textové pole pro vstup od uživatele, pomocí [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) pomůcky. Jakmile byl zadán text do pole, klíč "Enter" zobrazí text v informační zprávě.

Otevřete <code>Resources\layout\main.xml</code> souboru a přidejte [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) – element (uvnitř [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<pre><code class=" syntax brush-C#">&lt;EditText
    android:id="@+id/edittext"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"/&gt;</code></pre>
```

Aby dělala něco s text, který typy uživatelů, přidejte následující kód do konce [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/) metoda:

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
 e.Handled = false;
 if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) {
  Toast.MakeText (this, edittext.Text, ToastLength.Short).Show ();
  e.Handled = true;
         }
};
```

To zaznamená [ `EditText` ](https://developer.xamarin.com/api/type/Android.Widget.EditText/) element z rozložení a nastaví [ `KeyPress` ](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) obslužná rutina, která definuje akce prováděné při stisknutí klávesy při widgetu má právě fokus. V takovém případě je definována metodu naslouchat klávesy Enter (při stisknutí dolů) a pak překryvné [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) zprávu s text, který byl zadán. [ `Handled` ](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) Vlastnost by měla být vždy `true` Pokud událost byla zpracována, tak, aby událost není třídění (což by způsobilo návrat do textového pole).

Spusťte aplikaci.

*Části této stránky se změny na základě práce vytvořen a* [ *sdíleny projektu pro Android otevřít zdroj* ](http://code.google.com/policies.html) *a použít podle podmínek, které jsou popsané v* [ *Porušení licence licence creative Commons 2.5* ](http://creativecommons.org/licenses/by/2.5/) *. V tomto kurzu vychází z* [ *Android věcem formuláře kurzu* ](http://developer.android.com/resources/tutorials/views/hello-formstuff.html) *.*



## <a name="related-links"></a>Související odkazy

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
