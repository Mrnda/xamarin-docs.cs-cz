---
title: "Práce s ikonami"
ms.topic: article
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 38a2e8b8cd5932bf96c1e0032a6f47627c3ea592
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-icons"></a>Práce s ikonami

Apple Watch řešení vyžadovat dvě sady ikon:

* Ikony aplikace iOS, které se zobrazí na iPhone.
* Apple Watch ikony, které bude vykreslen v kruh v nabídce sledovat a obrazovky oznámení. Ikona sledování aplikace se zobrazí také v [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) aplikace pro iOS.

## <a name="apple-watch-icons"></a>Ikony Apple Watch

<table align="center" border="1" cellpadding="1" cellspacing="1">
    <tr>
      <td valign="top">
        <b>Ikona aplikace iOS</b>
      </td>
      <td valign="top">
Zobrazí se na iPhone a spustí aplikace nadřazené </td>
      <td>
        <img src="icons-images/icon-ios.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top" rowspan="3">
        <b>Podívejte se na ikonu aplikace</b>
      </td>
      <td valign="top">
Zobrazí se na domovské obrazovce Apple Watch </td>
      <td>
        <img src="icons-images/icon-home.png" class="tableimg" />
      </td>
    </tr>
    <tr>
      <td valign="top">
Zobrazí se na sledovat oznámení </td>
      <td>
        <img src="icons-images/notification-icon.png" class="tableimg" />
      </td>
    </tr>
    <tr>
      <td valign="top">
Zobrazí se v <a href="~/ios/watchos/app-fundamentals/settings.md">Apple Watch aplikace pro iOS</a>
      </td>
      <td>
        <a href="icons-images/watch-app.png">
          <img src="icons-images/watch-app-sml.png" class="tableimg">
        </a>
      </td>
    </tr>
    <tbody>
</table>



## <a name="configuring-your-solution"></a>Konfigurace řešení

Aby vaše aplikace pro iOS a sledování aplikace zobrazují správný název a ikona, postupujte podle těchto pokynů pro každý projekt:

### <a name="ios-app"></a>iOS App

Odkazovat [ikony aplikace iOS průvodce](~/ios/app-fundamentals/images-icons/app-icons.md) zajistit ikony aplikace iOS jsou správně nakonfigurovány.

#### <a name="infoplist"></a>Info.plist

Řetězec, který se zobrazí vedle sledování aplikace v rámci [Apple Watch nastavení aplikace](~/ios/watchos/app-fundamentals/settings.md) je nakonfigurovaný v **Info.plist aplikace iOS**.

Potvrďte, že vaše **Info.plist** má `CFBundleName` klíč a hodnotu (Poznámka: Jedná se liší od `CFBundleDisplayName`, může mít oba):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch aplikace

Jednou vaše [nadřazená aplikace](~/ios/watchos/app-fundamentals/parent-app.md) nakonfiguroval jeho ikony, je nutné přidat katalog asset ikona aplikace pro sledování aplikace.

1. Klikněte pravým tlačítkem na projekt aplikace sledovat a vyberte **soubor > Přidat > Nový soubor... > iOS > Katalog Asset** katalog asset přidat do projektu.

 ![](icons-images/newasset.png "Katalog asset přidejte do projektu")

2. Dvakrát klikněte na **AppIcons.appiconset/Contents.json** souboru

  ![](icons-images/xcassets-iconset-sml.png "Obsah AppIcons")

3. Přidejte všechny watchOS bitové kopie, jak je vidět na tomto snímku obrazovky:

  [ ![](icons-images/appicons-sml.png "Přidejte všechny watchOS bitové kopie, jak je vidět na tomto snímku obrazovky")](icons-images/appicons.png)

  Odkazovat na [pokynů společnosti Apple ikonu](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html) pro požadované velikosti (dimenze jsou také uvedené na obrazovce). Mějte na paměti, že tyto ikony bude automaticky oříznut k vykreslení v kruh.

  Ikona seznamu by měl vypadat přibližně takto:

  ![](icons-images/xcassets-complete-sml.png "Ikona seznamu v Průzkumníku řešení")

4. Chcete-li v katalogu asset je součástí aplikace, přidat následující klíč a hodnotu **Info.plist aplikace sledovat**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcons.appiconset</string>
```

Můžete ověřit ikony jsou nakonfigurované správné kontrolou [Apple Watch nastavení aplikace](~/ios/watchos/app-fundamentals/settings.md) v iPhone simulátoru, nebo generování [oznámení](~/ios/watchos/platform/notifications.md) a potvrzení na ikonu se zobrazí na oznámení obrazovky.

> [!NOTE]
> **Poznámka:**: ikony nemůže mít kanálu alfa (aplikace budou odmítnuty během odesílání obchod Pokud alfa kanálu je k dispozici). Můžete zkontrolovat, zda alfa kanálu existuje a jeho odebrání [aplikaci Preview v systému Mac OS X](~/ios/watchos/troubleshooting.md#noalpha).


## <a name="related-links"></a>Související odkazy

- [Ikona & bitové kopie společnosti Apple Průvodce](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html)