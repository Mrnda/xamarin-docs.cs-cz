---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 53f2ba8ff14a4338310e02244acdbfd7fa9bc13c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="gridlayout"></a>GridLayout

`GridLayout` Je nová `ViewGroup` podtřídami, který podporuje rozložení zobrazení v 2D mřížky, podobně jako tabulky HTML, jak je uvedeno níže:

 [![Oříznout GridLayout zobrazení čtyři buněk](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout` funguje s dvojrozměrném zobrazení hierarchie, kde podřízené zobrazení nastavit jejich umístění v mřížce zadáním řádky a sloupce, které by měl být ve. Tímto způsobem *GridLayout* je možné nastavit bez nutnosti, že všechny zprostředkující zobrazení poskytovat struktura tabulky, tak jak je vidět v řádky tabulky, které jsou používány TableLayout zobrazení v mřížce. Udržováním hierarchie ploché *GridLayout* je schopen více rychle rozložení jeho podřízené zobrazení. Podívejme se na příklad k objasnění, co tento koncept ve skutečnosti rozumí v kódu.


## <a name="creating-a-grid-layout"></a>Vytváření rozložení mřížky

Následující kód XML přidá několik `TextView` prvky *GridLayout*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip" />
</GridLayout>
```

Rozložení se upraví velikosti řádků a sloupců, aby buněk vejde na jejich obsah, které jsou popsány v následujícím diagramu:

 [![Diagram zobrazující dvě buňky na levé straně menší než na pravé straně rozložení](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

Výsledkem je následující uživatelské rozhraní při spuštění v aplikaci:

 [![Aplikace – snímek obrazovky GridLayoutDemo zobrazení čtyři buněk](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)



## <a name="specifying-orientation"></a>Zadání orientace

Všimněte si v souboru XML výše, každý `TextView` neurčuje sloupce či řádku. Pokud tyto nejsou zadána, `GridLayout` přiřadí každé podřízené zobrazení v pořadí, na základě orientaci. Umožňuje změnit například orientaci GridLayout z výchozí, což je vodorovná, svislá na takto:

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

Nyní `GridLayout` bude umístit buněk shora dolů v jednotlivých sloupcích, nikoli zleva doprava, jak je uvedeno níže:

 [![Diagram ilustrující, jak jsou buněk umístěn ve svislém orientace](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

Výsledkem je následující uživatelské rozhraní za běhu:

 [![Snímek obrazovky GridLayoutDemo s buňkami umístěný ve svislém orientace](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)



### <a name="specifying-explicit-position"></a>Určení explicitní pozice

Pokud chceme explicitně řízení pozice zobrazení podřízených `GridLayout`, jsme můžete nastavit jejich `layout_row` a `layout_column` atributy. Například následující kód XML způsobí rozložení ukazuje první snímek obrazovky (viz výše), bez ohledu na to orientaci.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="1" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="1"  />
</GridLayout>
```



### <a name="specifying-spacing"></a>Určení mezery

Máte několik možností, které zajistí mezery mezi podřízené zobrazení `GridLayout`. Můžeme použít `layout_margin` atribut pro nastavení v každé podřízené zobrazení okraje přímo, jak je uvedeno níže

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

Kromě toho v systému Android 4 názvem nové zobrazení pro obecné účely mezery `Space` je nyní k dispozici. Pokud chcete použít, jednoduše přidejte jej jako podřízené zobrazení.
Například následující kód XML přidá další řádek, abyste `GridLayout` nastavením jeho `rowcount` do 3 a přidá `Space` zobrazení, která poskytuje mezery mezi `TextViews`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="3"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"         
            android:layout_height="50dp" />    
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="1" />
</GridLayout>
```

Tato konfigurace XML vytvoří mezery v `GridLayout` jak je uvedeno níže:

 [![Snímek obrazovky GridLayoutDemo ilustrující větší buněk s mezery](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

Výhodou použití nové `Space` zobrazení je, že umožňuje mezery a nevyžaduje nám nastavit atributy u každé podřízené zobrazení.



### <a name="spanning-columns-and-rows"></a>Rozložení řádků a sloupců

`GridLayout` Také podporuje buněk, které jsou rozmístěny v několika sloupců a řádků. Řekněme například, přidáme jiný řádek obsahující tlačítko `GridLayout` jak je uvedeno níže:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="4"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"        
            android:layout_height="50dp" />   
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"        
            android:layout_row="2"        
            android:layout_column="1" />
     <Button
            android:id="@+id/myButton"
            android:text="@string/hello"        
            android:layout_row="3"
            android:layout_column="0" />
</GridLayout>
```

Výsledkem bude první sloupec `GridLayout` je roztažen tak, aby dokázala pojmout velikost tlačítko, jak vidíte zde:

[![Snímek obrazovky GridLayoutDemo s tlačítkem pokrývání uzlů pouze první sloupec](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

Chcete-li zabránit roztažení první sloupec, jsme nastavit tlačítko zahrnovat dva sloupce nastavením jeho columnspan takto:

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

To má za následek rozložení pro `TextViews` který je podobný rozložení jsme měli dříve, pomocí tlačítka Přidat do dolní části `GridLayout` jak je uvedeno níže:

 [![Snímek obrazovky GridLayoutDemo s tlačítkem pokrývání uzlů oba sloupce](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)


## <a name="related-links"></a>Související odkazy

- [GridLayoutDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/GridLayoutDemo/)
- [Představení Sandwichovy Zmrzlinová](http://www.android.com/about/ice-cream-sandwich/)
- [Platformu Android 4.0](http://developer.android.com/sdk/android-4.0.html)
