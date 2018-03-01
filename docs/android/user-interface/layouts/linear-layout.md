---
title: LinearLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 3cc5db39280c72f0de9dbdae07a49b56416c90a5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="linearlayout"></a>LinearLayout

[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) je [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) který zobrazí podřízené [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) elementy v lineární směru, buď vodorovně nebo svisle.

Byste měli být opatrní při používání přepsání [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).
Pokud začnete vnoření více [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s, možná budete chtít zvážit použití [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) místo.

Spuštění nového projektu s názvem **HelloLinearLayout**.

Otevřete **Resources/Layout/Main.axml** a vložte následující:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation=    "vertical"
    android:layout_width=    "fill_parent"
    android:layout_height=    "fill_parent"    >

  <LinearLayout
      android:orientation=    "horizontal"
      android:layout_width=    "fill_parent"
      android:layout_height=    "fill_parent"
      android:layout_weight=    "1"    >
      <TextView
          android:text=    "red"
          android:gravity=    "center_horizontal"
          android:background=    "#aa0000"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "green"
          android:gravity=    "center_horizontal"
          android:background=    "#00aa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "blue"
          android:gravity=    "center_horizontal"
          android:background=    "#0000aa"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "yellow"
          android:gravity=    "center_horizontal"
          android:background=    "#aaaa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
  </LinearLayout>
        
  <LinearLayout
    android:orientation=    "vertical"
    android:layout_width=    "fill_parent"
    android:layout_height=    "fill_parent"
    android:layout_weight=    "1"    >
    <TextView
        android:text=    "row one"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row two"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row three"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row four"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
       android:layout_weight=    "1"    />
  </LinearLayout>

</LinearLayout>
```

Pečlivě zkontrolujte tento XML. Je kořenová [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) , který definuje její orientace být svislé &ndash; všechny podřízené [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)s (který má dva) bude skládaný svisle. Je prvním podřízeným objektem jiného [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) používající vodorovné orientaci a druhý podřízená položka [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) používající vertikální orientace. Každá z těchto vnořené [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s obsahují několik [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) prvky, které jsou vzájemně orientované způsobem definované jejich nadřazené [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).

Nyní otevřete **HelloLinearLayout.cs** a ujistěte se, zda se načte **Resources/Layout/Main.axml** rozložení v [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metoda:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) Metoda načte soubor rozložení pro [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)zadaný podle ID prostředku &ndash; `Resources.Layout.Main` odkazuje **prostředky, rozvržení nebo Main.axml** rozložení souboru.

Spusťte aplikaci. Měli byste vidět následující:

[ ![Snímek obrazovky aplikace první LinearLayout vodorovně uspořádané druhé svisle](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png)

Všimněte si, jak definuje chování jednotlivých zobrazení v atributy XML. Zkuste stisknout různé hodnoty pro `android:layout_weight` najdete v části Jak nemovitosti obrazovky je distribuován na základě váhu jednotlivých prvků. Najdete v článku [běžně používané objekty rozložení](http://developer.android.com/guide/topics/ui/declaring-layout.html) dokumentu další informace o [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) obslužné rutiny `android:layout_weight` atribut.

<a name="References" />

## <a name="references"></a>Odkazy

-   [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Úpravy, které jsou na základě práce vytvořen a sdílí projektu pro Android otevřít zdroje a používají podle podmínek, které jsou popsané v části této stránky jsou*
[*Creative Commons 2.5 porušení licence* ](http://creativecommons.org/licenses/by/2.5/).

