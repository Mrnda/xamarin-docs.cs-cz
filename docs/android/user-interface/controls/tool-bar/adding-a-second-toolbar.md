---
title: Přidání druhého panelu nástrojů
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: f2255ab5ac7e153266093877a1c52bfc08927a09
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766634"
---
# <a name="adding-a-second-toolbar"></a>Přidání druhého panelu nástrojů


## <a name="overview"></a>Přehled 

`Toolbar` Více než nahradit provést na panelu akcí &ndash; ho může být použita vícekrát v aktivitě, může být možné přizpůsobit pro umístění kdekoli na obrazovce a dá se rozložit částečné šířku obrazovky. Následující příklady ukazují, jak vytvořit druhý `Toolbar` a umístěte jej v dolní části obrazovky. To `Toolbar` implementuje **kopie**, **Vyjmout**, a **vložení** položky nabídky. 


## <a name="define-the-second-toolbar"></a>Definování druhý panelu nástrojů 

Upravte soubor rozložení **Main.axml** a nahraďte jeho obsah s následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:id="@+id/main_content"
        android:layout_below="@id/toolbar">
      <ImageView
          android:layout_width="fill_parent"
          android:layout_height="0dp"
          android:layout_weight="1" />
      <Toolbar
          android:id="@+id/edit_toolbar"
          android:minHeight="?android:attr/actionBarSize"
          android:background="?android:attr/colorAccent"
          android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
          android:layout_width="match_parent"
          android:layout_height="wrap_content" />
    </LinearLayout>
</RelativeLayout>
```

Tato konfigurace XML přidá druhý `Toolbar` k dolnímu okraji obrazovky s prázdnou `ImageView` naplnění středu obrazovky. Výška tohoto `Toolbar` je nastaven na výšku zobrazí panel akce: 

```xml
android:minHeight="?android:attr/actionBarSize"
```

Barva pozadí to `Toolbar` je nastaven na Barva zvýraznění, která budou vedle určené:

```xml
android:background="?android:attr/colorAccent
```

Všimněte si které tento `Toolbar` je založena na jiný motiv (**ThemeOverlay.Material.Dark.ActionBar**) než používaného `Toolbar` vytvořené v [nahrazení panelu akcí](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash;není vázána, vnitřní okno aktivity nebo motiv použitý v prvním `Toolbar`.

Upravit **Resources/values/styles.xml** a přidejte následující barva zvýraznění k definici stylu: 

```xml
<item name="android:colorAccent">#C7A935</item>
```

To dává tmavý oranžové barva spodním panelu nástrojů. Sestavení a spuštění aplikace zobrazí prázdné druhý panelu nástrojů v dolní části obrazovky: 

[![Snímek obrazovky aplikace s žlutý druhý panelu nástrojů v dolní části obrazovky](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)


 
## <a name="add-edit-menu-items"></a>Přidání položek nabídky Upravit 

Tato část vysvětluje postup přidání položky nabídky Upravit do dolní části `Toolbar`. 

Přidání položek nabídky do sekundárního `Toolbar`: 

1.  Přidání ikon nabídky do `mipmap-` složky projekt aplikace (v případě potřeby).

2.  Zadat obsah položek nabídky přidáním soubor prostředků další nabídky k **prostředky a nabídek**. 

3.  V rámci aktivity `OnCreate` metoda, Najít `Toolbar` (voláním `FindViewById`) a rozšířené `Toolbar`na nabídky.

4.  Implementace obslužná rutina kliknutí na v `OnCreate` pro nové položky nabídky. 

Následující části ukazují, tento proces podrobně: **Vyjmout**, **kopie**, a **vložení** nabídky položky budou přidány do dolní části `Toolbar`. 



### <a name="define-the-edit-menu-resource"></a>Definici zdroje nabídky Upravit

V **prostředky a nabídek** podadresáři, vytvořte nový soubor XML s názvem **edit_menus.xml** a nahraďte jeho obsah s následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

Tato konfigurace XML vytvoří **Vyjmout**, **kopie**, a **vložení** položky nabídky (pomocí ikony, které byly přidány do `mipmap-` složek v [nahrazení panelu akcí ](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)).



### <a name="inflate-the-menus"></a>Zvýšilo v nabídkách

Na konci `OnCreate` metoda v **MainActivity.cs**, přidejte následující řádky kódu: 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

Tento kód vyhledá `edit_toolbar` zobrazení definované v **Main.axml**, nastaví její název **úpravy**a zvýšení kapacity jeho položky nabídky (definované v **edit_menus.xml**). Definuje nabídce klikněte na tlačítko obslužná rutina, která se zobrazí informační zprávy k označení, které úpravy ikona byla stisknuté. 

Sestavte a spusťte aplikaci. Při spuštění aplikace, text a ikony přidané v předchozím kroku se zobrazí, jak je vidět tady: 

[![Diagram dolním panelu nástrojů s ikonami vyjmutí, kopírování a vložení](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

Klepnutím **Vyjmout** ikonu nabídky způsobí, že následující informační zprávy, který se má zobrazit: 

[![Snímek obrazovky informační označující, že byl stisknuté ikonu vyjmutí nabídky](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

Klepnutím položky nabídky na buď panelu nástrojů zobrazí výsledná informační zprávy: 

[![Snímky obrazovky informační zprávy pro uložit, kopírovat a vložit položky nabídky se stisknuté](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)



## <a name="the-up-button"></a>Tlačítka nahoru 

Většina aplikací pro Android spoléhají na **zpět** tlačítko pro navigaci aplikace; stisknutím **zpět** tlačítko přejde uživatele na předchozí obrazovku.
Ale můžete také zajistit **až** tlačítko, které usnadňuje uživatelům procházet "až" hlavní obrazovky aplikace. Když uživatel vybere **až** tlačítko uživatel přesune na vyšší úrovni v hierarchii aplikace &ndash; tedy aplikace POP uživatele zpět více aktivit v back zásobníku namísto operace zpět dříve navštívených Aktivita. 

Povolit **až** tlačítka na druhá aktivita, která používá `Toolbar` jako jeho panelu akcí volání `SetDisplayHomeAsUpEnabled` a `SetHomeButtonEnabled` metody v druhá aktivita `OnCreate` metoda:

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

[Podporu nástrojů v7](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/) příklad kódu ukazuje **až** tlačítko v akci. Tato ukázka (které používá knihovnu kompatibility aplikace, která je popsána dále) implementuje druhá aktivita, který používá panelu nástrojů **až** tlačítko hierarchické navigační zpět na předchozí aktivity. V tomto příkladu `DetailActivity` povolí tlačítko Domů **až** tlačítko tím, že provedete následující `SupportActionBar` volání metod: 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

Když uživatel přejde z `MainActivity` k `DetailActivity`, `DetailActivity` zobrazí **až** tlačítko (šipka doleva polohovací), jak je uvedené na snímku obrazovky:

[![Snímek obrazovky příklad levém šipka nahoru tlačítka na panelu nástrojů](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

Klepnutím to **až** tlačítko způsobí, že aplikace se vraťte do `MainActivity`. Ve složitější aplikaci s více úrovní hierarchie klepnutím na toto tlačítko by vrátit uživatele na další nejvyšší úroveň v aplikaci a nikoli na předchozí obrazovku. 



## <a name="related-links"></a>Související odkazy

- [Panel nástrojů typu Lupa (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [Kompatibilita aplikace panelu nástrojů (ukázka)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
