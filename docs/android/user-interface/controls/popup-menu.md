---
title: Místní nabídka
description: Jak přidat místní nabídku, která je ukotven do konkrétního zobrazení.
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: d7cadde88e9ae7ee30815ee9323785038dbb1a39
ms.sourcegitcommit: ecdc031e9e26bbbf9572885531ee1f2e623203f5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393656"
---
# <a name="popup-menu"></a>Místní nabídka

[PopupMenu](https://developer.xamarin.com/api/type/Android.Widget.PopupMenu/) (také nazývané _nabídku_) je nabídka, která je ukotven na konkrétní zobrazení. V následujícím příkladu obsahuje jednu aktivitu tlačítko. Když uživatel klepne na tlačítko, zobrazí se tři položky místní nabídky:

[![Příklad aplikace s tlačítko a tři položky místní nabídky](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)


## <a name="creating-a-popup-menu"></a>Vytváří se místní nabídka

Prvním krokem je vytvoření souboru prostředků nabídek pro nabídky a umístit ji **prostředky/nabídka**. Například následující kód XML je kód pro tři položky nabídky zobrazí v předchozím snímku obrazovky **Resources/menu/popup_menu.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/item1"
          android:title="item 1" />
    <item android:id="@+id/item1"
          android:title="item 2" />
    <item android:id="@+id/item1"
          android:title="item 3" />
</menu>
```

Dále vytvořte instanci `PopupMenu` a ukotvit k jeho zobrazení. Při vytváření instance `PopupMenu`, předat konstruktoru odkaz na `Context` a také zobrazení, ke které se v nabídce připojena. Místní nabídka je ukotven v důsledku toho tato zobrazení během jeho vytváření.

V následujícím příkladu `PopupMenu` se vytvoří v obslužnou rutinu události kliknutí pro tlačítko (který se nazývá `showPopupMenu`). Toto tlačítko je také zobrazení, ke kterému `PopupMenu` je ukotven, jak je znázorněno v následujícím příkladu kódu:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
};
```

A konečně, musí být místní nabídka *zvýšeným* nabídce prostředku, který jste vytvořili dříve. V následujícím příkladu volání v nabídce [Inflate](https://developer.xamarin.com/api/member/Android.Views.LayoutInflater.Inflate/p/System.Int32/Android.Views.ViewGroup/) metoda je přidána a jeho [zobrazit](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Show%28%29/) metoda je volána k zobrazení:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```


## <a name="handling-menu-events"></a>Zpracování události nabídky

Když uživatel vybere položku nabídky [MenuItemClick](https://developer.xamarin.com/api/event/Android.Widget.PopupMenu.MenuItemClick/) klikněte na událost se vyvolá a nabídky se zruší. Klepnutím na libovolné místo mimo nabídky bude jednoduše zavřít. V obou případech, když v nabídce se zavře jeho [DismissEvent](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Dismiss%28%29/) , bude vyvolána. Následující kód přidává obslužné rutiny událostí pro obě `MenuItemClick` a `DismissEvent` události:

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
