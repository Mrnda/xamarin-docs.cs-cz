---
title: Práce s watchOS ikony v Xamarinu
description: Tento dokument popisuje různé ikony, které jsou nezbytné pro watchOS aplikace a jak nastavit řešení zahrnout tyto ikony.
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/26/2018
ms.openlocfilehash: e46ecc9d78ccc5dcfbe571c9ec5350fe6c391b7e
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39275921"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>Práce s watchOS ikony v Xamarinu

Apple Watch řešení vyžadují dvě sady ikon:

* Ikony aplikace iOS, které se zobrazí na Iphonu.
* Apple Watch ikony, které budou generovány v kruhu v nabídce sledování a na obrazovkách oznámení. Ikona aplikace watch se zobrazí také v [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) aplikace pro iOS.

## <a name="apple-watch-icons"></a>Apple Watch ikony

| | | |
|-|-|-|
|Ikona aplikace iOS|Zobrazí se na Iphonu a spustí nadřazená aplikace|![Ikona pro aplikace iOS](icons-images/icon-ios.png)|
|Podívejte se na ikoně aplikace|Zobrazí se na domovské obrazovce Apple Watch|![Ikona aplikace pro watchOS](icons-images/icon-home.png)|
||Zobrazí se v oznámeních sledování|![Ikona oznámení watchOS](icons-images/notification-icon.png)|
||Zobrazí se v [Apple Watch aplikace pro iOS](~/ios/watchos/app-fundamentals/settings.md)|![Ikona aplikace Watch pro iOS](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>Konfigurace řešení

Zajistěte, aby vaše aplikace pro iOS a aplikace watch zobrazit správný název a ikonu, postupujte podle těchto pokynů pro každý projekt:

### <a name="ios-app"></a>Aplikace pro iOS

Odkazovat [ikony aplikace pro iOS průvodce](~/ios/app-fundamentals/images-icons/app-icons.md) zajistit ikony aplikace iOS se správnou konfiguraci.

#### <a name="infoplist"></a>Info.plist

Řetězec, který se zobrazí vedle aplikace na hodinkách v [Apple Watch nastavení aplikace](~/ios/watchos/app-fundamentals/settings.md) je nakonfigurovaný v **souboru Info.plist aplikace iOS**.

Ujistěte se, že vaše **Info.plist** má `CFBundleName` klíče a hodnoty (Poznámka: Tento proces se liší na `CFBundleDisplayName`, může mít oba):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Aplikace pro Apple Watch

Jakmile vaše [nadřazená aplikace](~/ios/watchos/app-fundamentals/parent-app.md) nakonfiguroval jeho ikony, je třeba přidat katalog prostředků ikonu aplikace na aplikace watch.

1. Klikněte pravým tlačítkem na projekt aplikace pro sledování a vyberte **soubor > Přidat > Nový soubor... > iOS > Katalog Assetů** přidat katalog prostředků do projektu.

 ![](icons-images/newasset.png "Katalog prostředků přidejte do projektu")

2. Dvakrát klikněte na **AppIcon.appiconset/Contents.json** souboru

  ![](icons-images/xcassets-iconset-sml.png "Obsah AppIcon")

3. Přidejte všechny Image watchOS, jak je znázorněno na tomto snímku obrazovky:

  [![](icons-images/appicons-sml.png "Přidejte všechny Image watchOS, jak je znázorněno na tomto snímku obrazovky")](icons-images/appicons.png#lightbox)

  Odkazovat na [ikonu pokynů společnosti Apple](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/menu-icons/) pro požadované velikosti (dimenze jsou také uvedené na obrazovce). Mějte na paměti, že tyto ikony se automaticky přichycena k vykreslení v kruhu.

  Ikona seznam by měl vypadat přibližně takto:

  ![](icons-images/xcassets-complete-sml.png "Seznam ikon v Průzkumníku řešení")

4. Chcete-li katalog assetů je součástí aplikace, přidejte následující klíč a hodnota, která se **souboru Info.plist aplikace Watch**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcon.appiconset</string>
```

Můžete ověřit ikony jsou nakonfigurované správné kontrolou [Apple Watch nastavení aplikace](~/ios/watchos/app-fundamentals/settings.md) v simulátoru Iphonu nebo generování [oznámení](~/ios/watchos/platform/notifications.md) a potvrzení na ikonu se zobrazí na oznámení obrazovka.

> [!NOTE]
> Ikony nemůže mít alfa kanál (aplikace budou odmítnuty během odesílání App Store Pokud kanál alfa je k dispozici). Můžete zkontrolovat, jestli alfa kanál existuje a odebrat ho [použití náhledu aplikace v Mac OS X](~/ios/watchos/troubleshooting.md#noalpha).


## <a name="related-links"></a>Související odkazy

- [Ikona & obrázky watchOS průvodce společnosti Apple](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/)
