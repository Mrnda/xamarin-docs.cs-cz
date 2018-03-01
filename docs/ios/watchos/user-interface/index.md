---
title: "watchOS uživatelské rozhraní"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: 34063be728f8a13bbe5d68440d9aceba417c5627
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="watchos-user-interface"></a>watchOS uživatelské rozhraní

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) příklad ukazuje různé watchOS ovládací prvky. Storyboard aplikace se zobrazí zde (klikněte na tlačítko pro přiblížení):

[ ![](images/storyboard-sml.png "Ukázka watchOS rozložení")](images/storyboard.png)

Programová názvy všech ovládacích prvků je s předponou `WKInterface` (např. `WKInterfaceLabel`, `WKInterfaceButton`).


<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
        <strong>Ovládací prvek</strong>
      </th>
      <th>
        <strong>Popis</strong>
      </th>
      <th>
        <strong>snímek obrazovky</strong>
      </th>
    </thead>
    <tbody>
    <tr>
      <td valign="top">
Popisek </td>
      <td valign="top">
Použití <code>SetText</code> a další vlastnosti pro řízení vzhledu textu v ovládacím prvku popisek. <code>NSAttributedString</code> je také podporována.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/label.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Tlačítko </td>
      <td valign="top">
Vytvořte a nastavte vlastnosti ve scénáři. <kbd>CTRL + přetažení</kbd> přidat <code>Action</code> k implementaci obslužné rutiny pro když po kliknutí na.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/button.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
přepínače </td>
      <td valign="top">
Použití <code>SetOn</code> pro řízení stavu přepínače.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/switch.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Posuvník </td>
      <td valign="top">
Je možné, mnoha různými styly.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/slider.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Image </td>
      <td valign="top">
Použít <code>myImage.SetImage("MyWatchImage")</code> načíst Image na sledovat, nebo <code>WKInterfaceDevice.CurrentDevice.AddCachedImage</code> pro ukládání do mezipaměti je pro opakované použití v hodinek.
        <br />
        <a href="~/ios/watchos/user-interface/image.md">Obrázek ovládacího prvku dokumentace</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/image.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Oddělovač </td>
      <td valign="top">
Pomocí oddělovače vytvářet atraktivní sledovat uživatelská rozhraní.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/separator.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
mapy </td>
      <td valign="top">
Obrázku mapy staticky se zobrazí na sledovat ale můžete řídit mnoho aspektů její vzhled, včetně přidávání kódů PIN.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/map.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Film & InlineMove </td>
      <td valign="top">
Filmy, můžete buď otevřené samostatně nebo vložené <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/movie.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Skupina </td>
      <td valign="top">
Skupiny můžete použijte k usnadnění vytváření atraktivní sledovat uživatelská rozhraní.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/group.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Tabulka </td>
      <td valign="top">
Zjednodušené verzi tabulky v systému iOS.
Implementace <code>DidSelectRow</code> reakce na volbu uživatele (nebo použijte segue).
        <br />
        <a href="~/ios/watchos/user-interface/table.md">Tabulka dokumentace ovládací prvek</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TableDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/table.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Zařízení </td>
      <td valign="top">
        <code>WKInterfaceDevice.CurrentDevice</code> obsahuje vlastnosti, jako <code>ScreenBounds</code>, <code>ScreenScale</code>, a <code>PreferredContentSizeCategory</code>.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/device.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="~/ios/watchos/user-interface/menu.md">Nabídky</a>
      </td>
      <td valign="top">
Definování nabídky stiskněte vynutit ve scénáři a provádět akce pro každé tlačítko v kódu.
        <br />
        <a href="~/ios/watchos/user-interface/menu.md">Dokumentace nabídky ovládací prvek (Force Touch)</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/controller.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Zadávání textu </td>
      <td valign="top">
Použití <code>PresentTextInputController</code> a <code>WKTextInputMode</code> výčtu.
        <br />
        <a href="~/ios/watchos/user-interface/text-input.md">Text vstup dokumentace</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/textinput.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Digitální Crown </td>
      <td valign="top">
Digitální Crown lze použít k řízení výběr, nebo jeho otočení lze sledovat v kódu.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/digital-crown.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Gesta </td>
      <td valign="top">
Existují čtyři typy rozpoznávání gesta, která mohou být přidány do scény: klepněte na, prstem, Pan a LongPress.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs">Katalog kódu</a>
      </td>
      <td>
        <img src="Images/gestures.png" class="tableimg">
      </td>
    </tr>
    </tbody>
</table>



## <a name="related-links"></a>Související odkazy

- [WatchKitCatalog (ukázka)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Podívejte se na referenční dokumentace rozhraní API Kit](https://developer.xamarin.com/api/namespace/WatchKit/)
