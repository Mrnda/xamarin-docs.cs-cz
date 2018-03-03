---
title: "Přidání kompatibility aplikace a podstatným návrhu"
description: "Pomocí těchto kroků převést stávající aplikace Xamarin.Forms Android používat kompatibility aplikace a materiálu návrhu"
ms.topic: article
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
ms.openlocfilehash: 3014db91ff87f0e73291595a17dd780b5e3cd3c2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="adding-appcompat-and-material-design"></a>Přidání kompatibility aplikace a podstatným návrhu

_Pomocí těchto kroků převést stávající aplikace Xamarin.Forms Android používat kompatibility aplikace a materiálu návrhu_

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>Přehled

Tyto pokyny popisují, jak k aktualizaci existující aplikace Xamarin.Forms Android v knihovně kompatibility aplikace a povolení materiálu návrhu v systému Android verze Xamarin.Forms aplikací.

### <a name="1-update-xamarinforms"></a>1. Aktualizace Xamarin.Forms

Zajistěte, aby že řešení používá Xamarin.Forms 2.0 nebo novější. V případě potřeby aktualizujte balíček Xamarin.Forms Nuget 2.0.

### <a name="2-check-android-version"></a>2. Zkontrolujte verzi systému Android

Zkontrolujte, zda cílový framework Android projektu Android 6.0 (Marshmallow). Zkontrolujte **projekt Android > Možnosti > sestavení > Obecné** je vybrané nastavení, které zajistí corrent framework:

 ![](appcompat-images/target-android-6-sml.png "Konfigurace sestavení obecné Android")

### <a name="3-add-new-themes-to-support-material-design"></a>3. Přidat nové motivy pro podporu materiálu návrhu

Vytvořte následující tři soubory v projektu Android a vložte následující obsah. Poskytuje Google [průvodci správným stylem](http://www.google.com/design/spec/style/color.html#color-color-palette) a [barev palety generátor](http://www.materialpalette.com/) si můžete vybrat režim alternativní barvu, která je zadán.

**Resources/values/colors.xml**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**Resources/values/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
  </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primaryDark</item>
    <item name="colorAccent">@color/accent</item>
    <item name="android:windowBackground">@color/window_background</item>
    <item name="windowActionModeOverlay">true</item>
  </style>
</resources>
```

Musí být součástí další styl **hodnoty v21** složku, kterou chcete použít konkrétní vlastnosti při spuštění na Android typu Lupa a novější.

**Resources/values-v21/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. Update AndroidManifest.xml

Pokud chcete zajistit nový motiv informace je motiv použít, nastavte v **AndroidManifest** souboru přidáním `android:theme="@style/MyTheme"` (stejně jako tomu bylo Ponechejte zbývající XML).

**Properties/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. Zadejte rozložení panelu nástrojů a na kartě

Vytvoření **Tabbar.axml** a **Toolbar.axml** soubory **prostředky, rozvržení** adresář a vložení v jejich obsah z níže:

**Resources/layout/Tabbar.axml**

```xml
<android.support.design.widget.TabLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/sliding_tabs"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:tabIndicatorColor="@android:color/white"
    app:tabGravity="fill"
    app:tabMode="fixed" />
```

Několik vlastností pro karty byly nastaveny včetně na kartě gravitace k `fill` a režim k `fixed`.
Pokud máte spoustu karty můžete chtít tento přepínač do posuvného - pročtěte Android [TabLayout dokumentace](http://developer.android.com/reference/android/support/design/widget/TabLayout.html) Další informace.

**Resources/layout/Toolbar.axml**

```xml
<android.support.v7.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
    app:layout_scrollFlags="scroll|enterAlways" />
```

V těchto souborech vytváříme konkrétní motiv pro panel nástrojů, který se může lišit pro vaši aplikaci.
Odkazovat [Hello nástrojů](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/) příspěvku na blogu Další informace.


### <a name="6-update-the-mainactivity"></a>6. Aktualizace `MainActivity`

V aplikacích pro existující Xamarin.Forms **MainActivity.cs** třída bude dědit ze `FormsApplicationActivity`. To je nutné nahradit `FormsAppCompatActivity` povolit nové funkce.

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

Nakonec "propojit nahoru" nové rozložení z kroku 5 v `OnCreate` metoda, jak je vidět tady:

```csharp
protected override void OnCreate(Bundle bundle)
{
  // set the layout resources first
  FormsAppCompatActivity.ToolbarResource = Resource.Layout.Toolbar;
  FormsAppCompatActivity.TabLayoutResource = Resource.Layout.Tabbar;

  // then call base.OnCreate and the Xamarin.Forms methods
  base.OnCreate(bundle);
  Forms.Init(this, bundle);
  LoadApplication(new App());
}
```
