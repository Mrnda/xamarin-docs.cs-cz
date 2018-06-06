---
title: watchOS ovládacích prvků uživatelského rozhraní v Xamarinu
description: Tento dokument popisuje různé ovládacích prvků, které jsou k dispozici pro použití v watchOS uživatelská rozhraní. Poskytuje popis popisky, tlačítka, přepínače, posuvníky, obrázky, oddělovačů, mapy a další.
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: b56cfed8f045d824996a004539533b27d66c8cb1
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791407"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>watchOS ovládacích prvků uživatelského rozhraní v Xamarinu

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) příklad ukazuje různé watchOS ovládací prvky. Storyboard aplikace se zobrazí zde (klikněte na tlačítko pro přiblížení):

[![](images/storyboard-sml.png "Ukázka watchOS rozložení")](images/storyboard.png#lightbox)

Programová názvy všech ovládacích prvků je s předponou `WKInterface` (např. `WKInterfaceLabel`, `WKInterfaceButton`).

|Ovládací prvek|Popis|snímek obrazovky|
|---|---|---|
|Popisek|Použití `SetText` a další vlastnosti pro řízení vzhledu textu v ovládacím prvku popisek. `NSAttributedString` je také podporována.<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Tlačítko|Vytvořte a nastavte vlastnosti ve scénáři. CTRL + přetažení přidat `Action` k implementaci obslužné rutiny pro když po kliknutí na.<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|přepínače|Použití `SetOn` pro řízení stavu přepínače.<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Posuvník|Je možné, mnoha různými styly.<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Image|Použít `myImage.SetImage("MyWatchImage")` načíst Image na sledovat, nebo `WKInterfaceDevice.CurrentDevice.AddCachedImage` pro ukládání do mezipaměti je pro opakované použití v hodinek.<br />[Obrázek ovládacího prvku dokumentace](~/ios/watchos/user-interface/image.md)<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Oddělovač|Pomocí oddělovače vytvářet atraktivní sledovat uživatelská rozhraní.<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|mapy|Obrázku mapy staticky se zobrazí na sledovat ale můžete řídit mnoho aspektů její vzhled, včetně přidávání kódů PIN.<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Film & InlineMove|Filmy, můžete buď otevřené samostatně nebo vložené<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Skupina|Skupiny můžete použijte k usnadnění vytváření atraktivní sledovat uživatelská rozhraní.<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|Tabulka|Zjednodušené verzi tabulky v systému iOS. Implementace `DidSelectRow` reakce na volbu uživatele (nebo použijte segue).<br />[Tabulka dokumentace ovládací prvek](~/ios/watchos/user-interface/table.md)<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Zařízení|`WKInterfaceDevice.CurrentDevice` obsahuje vlastnosti, jako `ScreenBounds`, `ScreenScale`, a `PreferredContentSizeCategory`.<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Nabídka](~/ios/watchos/user-interface/menu.md)|Definování nabídky stiskněte vynutit ve scénáři a provádět akce pro každé tlačítko v kódu.<br />[Dokumentace nabídky ovládací prvek (Force Touch)](~/ios/watchos/user-interface/menu.md)<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Zadávání textu|Použití `PresentTextInputController` a `WKTextInputMode` výčtu.<br />[Text vstup dokumentace](~/ios/watchos/user-interface/text-input.md)<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Digitální Crown|Digitální Crown lze použít k řízení výběr, nebo jeho otočení lze sledovat v kódu.<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Gesta|Existují čtyři typy rozpoznávání gesta, která mohou být přidány do scény: klepněte na, prstem, Pan a LongPress.<br />[Katalog kódu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## <a name="related-links"></a>Související odkazy

- [WatchKitCatalog (ukázka)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Podívejte se na referenční dokumentace rozhraní API Kit](https://developer.xamarin.com/api/namespace/WatchKit/)
