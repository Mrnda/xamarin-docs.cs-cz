---
title: Navigační panel
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: 476e73906e4fbe01847740f90a8da701768b29db
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762809"
---
# <a name="navigation-bar"></a>Navigační panel

Android 4 zavedená nová systému uživatelské rozhraní funkce s názvem *navigační panel*, který poskytuje ovládací prvky pro navigaci na zařízeních, která nezahrnuje hardwaru tlačítka pro **Domů**, **zpět** , a **nabídky**.
Následující snímek obrazovky ukazuje na navigačním panelu z Nexus Prime zařízení:

 [![Příklad zobrazí Android navigační panel](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

Jsou k dispozici několik nových příznaky, řídit viditelnost navigačním panelu a jeho ovládací prvky, a také viditelnost panelu systému, která byla zavedena v systému Android 3. Příznaky jsou definovány v `Android.View.View` třídy a jsou uvedeny níže:

-   `SystemUiFlagVisible` &ndash; Zviditelní navigačním panelu. 
-   `SystemUiFlagLowProfile` &ndash; Dims rozložení ovládacích prvků na navigačním panelu. 
-   `SystemUiFlagHideNavigation` &ndash; Skryje navigačním panelu. 


Tyto příznaky můžete použít pro všechna zobrazení v hierarchii zobrazení nastavením `SystemUiVisibility` vlastnost. Pokud máte více zobrazení tato vlastnost nastavena, systém je spojuje se operace nebo a použije je tak dlouho, dokud okno, ve kterém jsou nastavené příznaků uchovává fokus. Když odeberete zobrazení, budou také odebrány všechny příznaky, které se má nastavit.

Následující příklad ukazuje jednoduchou aplikaci, kde kliknete na některou z tlačítka změní `SystemUiVisibility`:

 [![Ukázka viditelný, nízká profil a skryté SystemUiVisibility snímky obrazovky](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

Chcete-li změnit kód `SystemUiVisibility` nastaví vlastnost na `TextView` z každé tlačítko klikněte na obslužnou rutinu události, jak je uvedeno níže:

```csharp
var tv = FindViewById<TextView> (Resource.Id.systemUiFlagTextView);
var lowProfileButton = FindViewById<Button>(Resource.Id.lowProfileButton);
var hideNavButton = FindViewById<Button> (Resource.Id.hideNavigation);
var visibleButton = FindViewById<Button> (Resource.Id.visibleButton);
           
lowProfileButton.Click += delegate {
    tv.SystemUiVisibility =
        (StatusBarVisibility)View.SystemUiFlagLowProfile;
};
           
hideNavButton.Click += delegate {
    tv.SystemUiVisibility =
       (StatusBarVisibility)View.SystemUiFlagHideNavigation;        
};
           
visibleButton.Click += delegate {
    tv.SystemUiVisibility = (StatusBarVisibility)View.SystemUiFlagVisible;
}
```

Navíc `SystemUiVisibility` změnit vyvolá `SystemUiVisibilityChange` událostí. Stejně jako nastavení `SystemUiVisibility` vlastnost, obslužné rutiny pro `SystemUiVisibilityChange` událost lze zaregistrovat pro všechna zobrazení v hierarchii. Například kód pod používá `TextView` instance k registraci pro událost:

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>Související odkazy

- [SystemUIVisibilityDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [Představení Sandwichovy Zmrzlinová](http://www.android.com/about/ice-cream-sandwich/)
- [Platformu Android 4.0](http://developer.android.com/sdk/android-4.0.html)
