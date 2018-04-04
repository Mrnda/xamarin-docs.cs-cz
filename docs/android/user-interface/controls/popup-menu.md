---
title: Místní nabídky
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/18/2017
ms.openlocfilehash: e7fad84133ca712c531ab0d12a67db78103c7cdd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="popup-menu"></a>Místní nabídky

`PopupMenu` Třída přidává podporu pro zobrazení místní nabídky, které jsou připojeny ke konkrétní zobrazení. Následující obrázek znázorňuje místní nabídky pomocí tlačítka předloží druhý zvýrazněná stejně, jako je vybraná položka:

 [![Příklad PopopMenu s třemi tři položky](popup-menu-images/20-popupmenu.png)](popup-menu-images/20-popupmenu.png#lightbox)

Android 4 přidat několik nových funkcí, které `PopupMenu` která usnadňují trochu pracovat, konkrétně:

-   **Inflate** &ndash; The zvýšilo metoda je nyní k dispozici přímo na třídě PopupMenu.
-   **DismissEvent** &ndash; PopupMenu třída má teď DismissEvent.

Podívejme se na tato vylepšení. V tomto příkladu máme jediné aktivity, která obsahuje tlačítko. Když uživatel klikne na tlačítko, se zobrazí místní nabídky, jak je uvedeno níže:

 [![Příklad aplikaci spuštěnou v emulátoru s tlačítko a 3 položky místní nabídky](popup-menu-images/06-popupmenu.png)](popup-menu-images/06-popupmenu.png#lightbox)


## <a name="creating-a-popup-menu"></a>Vytváření místní nabídky

Když vytvoříme instanci `PopupMenu`, musíme předat odkaz na jeho konstruktoru `Context`, a také zobrazení, ke kterému je připojena v nabídce. V tomto případě vytvoříme `PopupMenu` obslužné rutiny události kliknutí pro naše tlačítko, která má název `showPopupMenu`.
Toto tlačítko je také zobrazit, na kterou jsme budete připojení `PopupMenu`, jak je znázorněno v následujícím kódu:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
}
```

V systému Android 3, vyžaduje nejprve získat odkaz na kód, který zvýšilo nabídky v prostředek XML `MenuInflator`a pak zavolají jeho `Inflate` metoda s ID prostředku XML, která obsahovala v nabídce a instance nabídky zvýšilo do. Tento postup pořád funguje v systému Android 4 a vyšší verze jako kód níže znázorňuje:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.MenuInflater.Inflate (Resource.Menu.popup_menu, menu.Menu);
};
```

Od verze Android 4 však můžete nyní volat `Inflate` přímo na instanci systému `PopupMenu`. Díky tomu kód přesnější, jak je vidět tady:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```

Ve výše uvedeném kódu, po nafouknutí nabídce jednoduše říkáme `menu.Show` zobrazíte na obrazovce.


## <a name="handling-menu-events"></a>Zpracování událostí nabídky

Když uživatel vybere položku nabídky `MenuItemClick` , bude vyvolána událost a v nabídce se se zavře. Klepnutím kamkoli mimo nabídce bude jednoduše zprávu zavřete. V obou případech od verze Android 4, když je v nabídce odmítnuta jeho `DismissEvent` , bude vyvolána. Následující kód přidá obslužné rutiny události pro oba `MenuItemClick` a `DismissEvent` události:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);

    menu.MenuItemClick += (s1, arg1) => {
        Console.WriteLine ("{0} selected", arg1.Item.TitleFormatted);
    };

    menu.DismissEvent += (s2, arg2) => {
        Console.WriteLine ("menu dismissed");
    };
            menu.Show ();
};
```



## <a name="related-links"></a>Související odkazy

- [PopupMenuDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/)
- [Představení Sandwichovy Zmrzlinová](http://www.android.com/about/ice-cream-sandwich/)
- [Platformu Android 4.0](http://developer.android.com/sdk/android-4.0.html)
