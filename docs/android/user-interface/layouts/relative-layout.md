---
title: Použití RelativeLayout v Xamarin.Android
description: Jak používat RelativeLayout do aplikace Xamarin.Android
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/29/2018
ms.openlocfilehash: af8d37775a798fc6019106a66df75843a951c108
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403413"
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) je [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) , který zobrazí podřízené [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) prvků v relativní pozice. Pozice [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) se dá nastavit jako relativní k elementů na stejné úrovni, (například tak, aby levé straně z nebo pod daného elementu) nebo v umístění vzhledem ke [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) oblast (například Zarovnání dolní, levý centra).

A [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) je velmi výkonný nástroj pro navrhování uživatelského rozhraní, protože může eliminovat vnořeného [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s. Pokud se několik použití vnořených [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) skupin, je možné nahradit pomocí jediného [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/).

Spuštění nového projektu s názvem **HelloRelativeLayout**.

Otevřít **Resources/Layout/Main.axml** soubor a vložte následující:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="match_parent"
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

Všimněte si, že každý z `android:layout_*` atributů, jako je například `layout_below`, `layout_alignParentRight`, a `layout_toLeftOf`.
Při použití [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), tyto atributy můžete použít k popisu, jak chcete umístit každý [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/). Jedna z těchto atributů definovat jiný druh relativní pozice. Některé atributy použijte ID prostředku na stejné úrovni [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) definovat vlastní relativní pozice. Například poslední [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) je definována leží na levé straně of a zarovnána s the horní of [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) identifikovaný ID `ok` (což je předchozí [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

Všechny dostupné rozložení atributy jsou definovány v [ `RelativeLayout.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/).

Ujistěte se, že načtete toto rozložení v [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metody:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/) Metoda načte soubor rozložení pro [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), podle ID prostředku &mdash; `Resource.Layout.Main` odkazuje **prostředky/rozložení / Main.axml** soubor rozložení.

Spusťte aplikaci. Měli byste vidět následující rozložení:

[![Snímek obrazovky s relativní rozložení s TextView EditText a dvě tlačítka](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>Prostředky

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*Části této stránky jsou změn založených na vytvořené a sdílené s Androidem otevřete zdrojový projekt a používán v souladu s podmínkami uvedenými v práci*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/).
