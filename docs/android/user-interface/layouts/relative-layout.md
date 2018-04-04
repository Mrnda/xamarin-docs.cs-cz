---
title: RelativeLayout
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 51b58c02ca2768481ffd8bf0e508e94fd408f465
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) je [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) který zobrazí podřízené [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) elementů v relativní umístění. Pozice [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) lze zadat jako relativně k prvků (například tak, aby levé straně z nebo pod daného elementu) nebo v umisťuje relativně k [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) oblast (například zarovnán dolů, doleva centra).

A [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) je velmi výkonný nástroj pro navrhování uživatelské rozhraní, protože můžete eliminovat vnořeného [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s. Pokud se přistihnete pomocí několik vnořené [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) skupiny, bude pravděpodobně možné je nahradit jedné [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/).

Spuštění nového projektu s názvem **HelloRelativeLayout**.

Otevřete **Resources/Layout/Main.axml** soubor a vložte následující:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:background="@android:drawable/editbox_background"
        android:layout_below="@id/label"/>
    <Button
        android:id="@+id/ok"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/entry"
        android:layout_alignParentRight="true"
        android:layout_marginLeft="10dip"
        android:text="OK" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toLeftOf="@id/ok"
        android:layout_alignTop="@id/ok"
        android:text="Cancel" />
</RelativeLayout>
```

Všimněte si těchto `android:layout_*` atributů, jako je například `layout_below`, `layout_alignParentRight`, a `layout_toLeftOf`.
Při použití [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), tyto atributy lze použít k popisu, jak chcete umístit každý [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/). Každé z nich těchto atributů definovat jiný druh relativní pozici. ID prostředku na stejné úrovni použití některých atributů [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) zadat relativní pozici. Například poslední [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) je definována leží na levé straně of a zarovnán s the-top-of [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) identifikovaný ID `ok` (což je předchozí [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

Všechny dostupné rozložení atributy jsou definovány v [ `RelativeLayout.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/).

Zajistěte, aby načíst toto rozložení v [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metoda:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/) Metoda načte soubor rozložení pro [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)zadaný podle ID prostředku &mdash; `Resource.Layout.Main` odkazuje **prostředky, rozvržení nebo Main.axml** rozložení souboru.

Spusťte aplikaci. Měli byste vidět následující rozložení:

[![Snímek obrazovky relativní rozložení TextView, EditText a dvě tlačítka](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>Prostředky

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*Úpravy, které jsou na základě práce vytvořen a sdílí projektu pro Android otevřít zdroje a používají podle podmínek, které jsou popsané v části této stránky jsou*
[*Creative Commons 2.5 porušení licence* ](http://creativecommons.org/licenses/by/2.5/).
