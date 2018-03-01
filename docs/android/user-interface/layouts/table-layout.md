---
title: TableLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 80147de0f639b4b597e11b41a6854f550edd61a9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="tablelayout"></a>TableLayout

[`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) je [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) který zobrazí podřízené [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) elementů v řádků a sloupců.

Spuštění nového projektu s názvem **HelloTableLayout**.

Otevřete **Resources/Layout/Main.axml** soubor a vložte následující:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:stretchColumns="1">

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Open..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-O"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save As..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-Shift-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Import..."
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Export..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-E"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Quit"
            android:padding="3dip"/>
    </TableRow>
</TableLayout>
```

Všimněte si, jak to vypadá takto: struktura tabulky HTML. [ `TableLayout` ](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) Element je jako kód HTML `<table>` element; [ `TableRow` ](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) je stejná jako `<tr>` element; ale buněk, můžete použít libovolnou druh [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) element. V tomto příkladu [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) se používá pro každou buňku. Mezi některé řádky k dispozici je také základní [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/), který slouží k vykreslení na vodorovném řádku.

Zajistěte, aby vaše **HelloTableLayout** aktivity načte toto rozložení v [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metoda:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) Metoda načte soubor rozložení pro [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)zadaný podle ID prostředku &mdash; `Resource.Layout.Main` odkazuje **prostředky, rozvržení nebo Main.axml** rozložení souboru.

Spusťte aplikaci. Měli byste vidět následující:

[![Příklad snímek obrazovky aplikace TableLayout zobrazení více řádků tabulky](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png)


<a name="References" />

## <a name="references"></a>Odkazy

-   [`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) 

-   [`TableRow`](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) 

-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Úpravy, které jsou na základě práce vytvořen a sdílí projektu pro Android otevřít zdroje a používají podle podmínek, které jsou popsané v části této stránky jsou*
[*Creative Commons 2.5 porušení licence* ](http://creativecommons.org/licenses/by/2.5/).
