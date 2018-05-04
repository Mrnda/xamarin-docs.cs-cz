---
title: Android – ovládací prvky (widgety)
description: Stavební bloky pro vytvoření Xamarin.Android uživatelského rozhraní
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 28418c3b3fedcd24963008eb3b59ffa782d791f1
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="android-controls-widgets"></a>Android – ovládací prvky (widgety)

Xamarin.Android zpřístupní všechny nativní uživatelské rozhraní ovládací prvky (widgety), která je poskytovaný Androidem. Tyto ovládací prvky lze snadno přidat do aplikace Xamarin.Android pomocí návrháře Android nebo programově pomocí souborů XML rozložení. Bez ohledu na to, kterou metodu zvolit Xamarin.Android zpřístupní všechny vlastnosti objektu uživatelské rozhraní a metody v jazyce C#. Následující části zavést většiny běžných ovládacích prvků Android uživatelského rozhraní a popisují, jak začlenit do aplikace Xamarin.Android.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[Panel akcí](~/android/user-interface/controls/action-bar.md) 

`ActionBar` je panel nástrojů, který zobrazí název aktivity, navigace rozhraní a další interaktivní položky. Na panelu akcí obvykle se zobrazí v horní části okna aktivity.

![Příklad nadřízených členů.](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[Automatické dokončování](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` je element zobrazení upravovat text, který zobrazuje doplňování návrzích automaticky, zatímco je zadání uživatele. V rozevírací nabídce, ze kterého uživatel může vybrat položku, kterou chcete nahradit obsah textové pole s se zobrazí seznamu návrhů.

![Příklad automatické dokončování](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[Tlačítka](~/android/user-interface/controls/buttons/index.md)

Tlačítka jsou prvky uživatelského rozhraní, které uživatel klepnutím k provedení akce.

![Příklad tlačítka](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Kalendář](~/android/user-interface/controls/calendar.md)

`Calendar` Třída se používá pro převod konkrétní instanci v čas (milisekundu hodnotu, která je posun od epoch) hodnoty, jako například rok, měsíc, hodina, den v měsíci a datum příštího týdne.
`Calendar` podporuje širokou řadu možností interakce s kalendářní data, včetně možnost číst a zapisovat události, účastníci a připomenutí. Pomocí zprostředkovatele kalendáře v aplikaci se zobrazí data, která přidáte prostřednictvím rozhraní API v předdefinované kalendáře aplikace, která se dodává s Android.

![Příklad kalendáře](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` je součást uživatelského rozhraní, která uvede textových a obrázkových obsah v zobrazení, které se podobají karty. `CardView` je implementovaný jako `FrameLayout` pomůcky s zaoblenými hranami a stínu. Obvykle `CardView` používá se k předložení jednoho řádku položky v `ListView` nebo `GridView` zobrazení skupiny.

![Příklad zobrazení karet](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[Upravit Text](~/android/user-interface/controls/edit-text.md)

`EditText` je element uživatelského rozhraní, který se používá pro zadávání a úpravy textu.

![Příklad úpravy textu](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[Galerie](~/android/user-interface/controls/gallery.md)

`Gallery` je rozložení pomůcku, která se používá k zobrazení položek v seznamu vodorovně posouvání; aktuální výběr se umístí na střed zobrazení.

![Příklad galerie](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[Navigační panel](~/android/user-interface/controls/navigation-bar.md)

*Navigační panel* poskytuje ovládací prvky pro navigaci na zařízení, které neobsahují hardwaru tlačítka pro **Domů**, **zpět**, a **nabídky**.

![Příklad navigační panel](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[Výběry](~/android/user-interface/controls/pickers/index.md)

*Výběr* jsou prvky uživatelského rozhraní, které uživateli umožní vybrat datum nebo čas pomocí dialogová okna, které jsou k dispozici Android.

![Příklad výběru](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[Místní nabídka](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` slouží k zobrazení místní nabídky, které jsou připojeny ke konkrétní zobrazení.

![Příklad místní nabídky](images/popup-menu.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[Rotující indikátor průběhu](~/android/user-interface/controls/spinner.md)

`Spinner` je element uživatelského rozhraní, která poskytuje rychlý způsob, jak vybrat jednu hodnotu ze sady. Je simmilar do rozevíracího seznamu. 

![Příklad číselník](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[Přepínač](~/android/user-interface/controls/switch.md)

`Switch` je element uživatelského rozhraní, která umožňuje uživatelům přepínat mezi dvěma stavy, například jako na nebo vypnout. `Switch` Výchozí hodnota je OFF.

![Příklad přepínačů](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` je zobrazení, které používá accelerated hardwaru 2D vykreslování můžete povolit video nebo OpenGL obsahu datový proud, který se má zobrazit.

![Příklad zobrazení textury](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[ToolBar](~/android/user-interface/controls/tool-bar/index.md)

`Toolbar` Pomůcky (zavedená v systému Android 5.0 typu Lupa) lze považovat za generalizace rozhraní panelu Akce &ndash; je určena k nahrazení panelu akcí. `Toolbar` Lze použít kdekoli v aplikaci rozvržení a je mnohem víc přizpůsobitelné než zobrazí panel Akce.

![Příklad panelu nástrojů](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

`ViewPager` Je rozložení manager, která umožňuje uživatelům překlopit doleva a doprava prostřednictvím stránky data.

![Příklad ViewPager](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` je element uživatelského rozhraní, která umožňuje vytvořit vlastní okna pro zobrazení webové stránky (nebo si vytvořit úplný prohlížeče).

![Příklad webového zobrazení](images/web-view.png)

