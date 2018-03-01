---
title: "Na panelu akcí nahrazení"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 91d5612991c2297418cf7003c499c1a1bbfc7558
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="replacing-the-action-bar"></a>Na panelu akcí nahrazení

<a name="overview" />

## <a name="overview"></a>Přehled

Mezi nejběžnější používá pro `Toolbar` je nahradit panelu akcí výchozí vlastní `Toolbar` (když je vytvořen nový projekt Android, se používá výchozí akce panel). Protože `Toolbar` poskytuje možnost přidání partnerské loga, názvu, položky nabídky, navigačních tlačítek a dokonce vlastní zobrazení do části panelu aplikace aktivity uživatelského rozhraní, nabízí významné upgrade na pruh výchozí akce.

Nahrazení aplikace výchozí akce panel s `Toolbar`: 

1.  Vytvořit nové vlastní motiv a upravit vlastnosti aplikace tak, aby používala nový motiv. 

2.  Zakažte `windowActionBar` atribut vlastní motiv a povolte `windowNoTitle` atribut.

3.  Definování rozložení pro `Toolbar`.

4.  Zahrnout `Toolbar` rozložení v rámci aktivity **Main.axml** rozložení souboru. 

5.  Přidání kódu do aktivity `OnCreate` metodu vyhledání `Toolbar` a volání `SetActionBar` k instalaci `ToolBar` jako na panelu akcí.

Následující části popisují tento proces podrobně. Vytvoření jednoduché aplikace a je nahrazený jeho panelu akcí s přizpůsobeným `Toolbar`. 


<a name="start_project" />

## <a name="start-an-app-project"></a>Spusťte projekt aplikace

Vytvořit nový projekt Android s názvem **ToolbarFun** (viz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) Další informace o vytvoření nového projektu Android). Po vytvoření tohoto projektu nastavit cíle a minimální úrovně rozhraní API systému Android **Android 5.0 (API úrovně 21 - typu Lupa)**. Další informace o úrovních verzi systému Android nastavení najdete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md). Při vytvořené a spuštění aplikace zobrazí na panelu akcí výchozí, jak je vidět na tomto snímku obrazovky: 

[![Snímek obrazovky výchozí akce panelu](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png)


<a name="custom_theme" />

## <a name="create-a-custom-theme"></a>Vytvořte vlastní motiv

Otevřete **prostředky nebo hodnotami** adresář a vytvořit nový soubor s názvem **styles.xml**. Jeho obsah nahraďte následující kód XML: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="@android:style/Theme.Material.Light.DarkActionBar">
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowActionBar">false</item>
    <item name="android:colorPrimary">#5A8622</item>
  </style>
</resources>
```

Tato konfigurace XML definuje nový vlastní motiv názvem **MyTheme** založený na **Theme.Material.Light.DarkActionBar** motiv typu Lupa. `windowNoTitle` Je atribut nastaven na `true` ke skrytí záhlaví: 

```xml
<item name="android:windowNoTitle">true</item>
```

Zobrazení vlastních nástrojů výchozí `ActionBar` musí být zakázaná: 

```xml
<item name="android:windowActionBar">false</item>
```

Olive-green `colorPrimary` nastavení se používá pro barvu pozadí panelu nástrojů: 
 
```xml
<item name="android:colorPrimary">#5A8622</item>
```

Upravit **Properties/AndroidManifest.xml** a přidejte následující `android:theme` atribut `<application>` element tak, aby aplikace používá `MyTheme` vlastní motiv: 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

Další informace o použití vlastní motiv do aplikace najdete v tématu [pomocí vlastní motivy](~/android/user-interface/material-theme.md#customtheme). 


<a name="toolbar_layout" />

## <a name="define-a-toolbar-layout"></a>Definování rozložení panelu nástrojů

V **prostředky, rozvržení** adresáře, vytvořte nový soubor s názvem **toolbar.xml**. Jeho obsah nahraďte následující kód XML: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?android:attr/actionBarSize"
    android:background="?android:attr/colorPrimary"
    android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"/>
```

Tato konfigurace XML definuje vlastní `Toolbar` který nahrazuje panelu výchozí akce. Minimální výška `Toolbar` je nastaven na velikost panelu akcí, který ho nahrazuje: 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

Barva pozadí `Toolbar` je nastaven na barvu olive-green definované výše v **styles.xml**:

```csharp
android:background="?android:attr/colorPrimary"
```

Počínaje typu Lupa, `android:theme` atribut slouží k formátování jednotlivých zobrazení. `ThemeOverlay.Material` Motivy byla zavedená v typu Lupa umožňují překrytí výchozí `Theme.Material` motivy, přepsal odpovídající atributy, aby byly světlý nebo tmavý. V tomto příkladu `Toolbar` používá tmavým motivem tak, aby jeho obsah light v barev: 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

Toto nastavení se používá, takže položky nabídky rozdíl oproti tmavšího barvu pozadí.


<a name="include_layout" />

## <a name="include-the-toolbar-layout"></a>Zahrnout rozložení panelu nástrojů

Upravte soubor rozložení **Resources/layout/Main.axml** a nahraďte jeho obsah následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
    <Button
        android:id="@+id/MyButton"
        android:layout_below="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Hello World, Click Me!" />
</RelativeLayout>
```

Toto rozložení zahrnuje `Toolbar` definované v **toolbar.xml** a používá `RelativeLayout` určíte, že `Toolbar` má být umístěn na začátek uživatelského rozhraní (nahoře na tlačítko). 


<a name="activate_toolbar" />

## <a name="find-and-activate-the-toolbar"></a>Najít a aktivujte panelu nástrojů

Upravit **MainActivity.cs** a přidejte následující příkaz using:

```csharp
using Android.Views;
```

Navíc na konec přidejte následující řádky kódu `OnCreate` metoda:

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

Tento kód vyhledá `Toolbar` a volání `SetActionBar` tak, aby `Toolbar` bude trvat na výchozí akce panelu Vlastnosti. Název panelu nástrojů se změní na **Moje nástrojů**. Jak je vidět v tomto příkladu kódu `ToolBar` můžete přímo odkazovat jako panelu akcí. Zkompilování a spuštění této aplikace &ndash; vlastní `Toolbar` se zobrazí místo panelu výchozí akce: 

[![Snímek obrazovky vlastní panel nástrojů s zelenou barevné schéma](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png)

Všimněte si, že `Toolbar` je navržen tak, nezávisle na `Theme.Material.Light.DarkActionBar` motiv, který se použije pro zbývající aplikace. 


<a name="main_menus" />
 
## <a name="add-menu-items"></a>Přidání položek nabídky 

V této části jsou nabídky přidat do `Toolbar`. Horní pravé oblasti `ToolBar` je vyhrazená pro položky nabídky &ndash; každou položku nabídky (také nazývané *položky Akce*) můžete provádět akce v rámci aktuální aktivita nebo ji můžete provádět akce jménem celá aplikace. 

Chcete-li přidat nabídek `Toolbar`: 

1.  Přidat nabídku ikony (v případě potřeby) do `mipmap-` složky projekt aplikace. Google poskytuje sadu volné nabídky ikony na [podstatným ikony](https://design.google.com/icons/) stránky. 

2.  Zadat obsah položek nabídky přidáním nového souboru prostředků nabídky v části **prostředky a nabídek**. 

3.  Implementace `OnCreateOptionsMenu` metoda aktivity &ndash; tato metoda zvýšení kapacity položky nabídky. 

4.  Implementace `OnOptionsItemSelected` metoda aktivity &ndash; tato metoda provede akci, když je stisknuté položku nabídky. 

Následující části ukazují tento proces podrobně přidáním **upravit** a **Uložit** položky nabídky k vlastní `Toolbar`. 


<a name="menu_icons" />

### <a name="install-menu-icons"></a>Nainstalujte nabídky ikony

Budete pokračovat `ToolbarFun` aplikace příklad ikony nabídky přidat do projektu aplikace. Stáhněte si [nástrojů icons.zip](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons.zip?raw=true) a rozbalte ho. Zkopírujte obsah extrahované *mipmap -* složky do projektu *mipmap -* složek **ToolbarFun nebo prostředky** a zahrnout každý soubor ikony přidané do projektu.

<a name="menu_resource" />

### <a name="define-a-menu-resource"></a>Definici zdroje nabídky

Vytvořte novou **nabídky** podadresáři pod **prostředky**. V **nabídky** podadresáři, vytvořte nový soubor prostředků nabídky s názvem **top_menus.xml** a nahraďte jeho obsah následující kód XML: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       android:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       android:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       android:showAsAction="never"
       android:title="Preferences" />
</menu>
```

Tato konfigurace XML se vytvoří tři položky nabídky:

-   **Upravit** položky nabídky, která používá `ic_action_content_create.png` ikonu (tužky). 

-   A **Uložit** položky nabídky, která používá `ic_action_content_save.png` ikonu (disketu). 

-   A **Předvolby** položky nabídky, která nemá ikonu.

`showAsAction` Atributy **upravit** a **Uložit** položky nabídky jsou nastaveny na `ifRoom` &ndash; toto nastavení způsobí, že tyto položky nabídky se objeví v `Toolbar` pokud existuje dostatečný prostor pro místo zobrazení. **Předvolby** nastaví položku nabídky `showAsAction` k `never` &ndash; to způsobí, že **Předvolby** chcete zobrazit v nabídce *přetečení* nabídky (tři Svislé tečky). 

<a name="on_create_options_menu" />

### <a name="implement-oncreateoptionsmenu"></a>Implementace OnCreateOptionsMenu

Přidejte následující metodu do **MainActivity.cs**:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android volání `OnCreateOptionsMenu` metoda tak, aby aplikace může určit prostředků nabídky pro aktivitu. Tato metoda **top_menus.xml** je zvětšený prostředků do předaný `menu`. Tento kód způsobí, že nové **upravit**, **Uložit**, a **Předvolby** položek nabídky ve `Toolbar`. 


<a name="on_options_item_selected" />

### <a name="implement-onoptionsitemselected"></a>Implementace OnOptionsItemSelected

Přidejte následující metodu do **MainActivity.cs**:

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

Když uživatel klepne na položku nabídky, Android volá `OnOptionsItemSelected` metoda a předává v položce nabídky, která byla vybrána. V tomto příkladu implementace právě zobrazí informační zprávy k označení, které položky nabídky se stisknuté. 

Sestavení a spuštění `ToolbarFun` zobrazíte nové položky nabídky na panelu nástrojů. `Toolbar` Teď zobrazuje tři ikony nabídky, jak je vidět na tomto snímku obrazovky: 

[![Diagram ilustrující umístění úpravy, uložit a přetečení položky nabídky](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png)

Když uživatel odposlouchávání **upravit** položky nabídky, oznámení se zobrazí k označení, že `OnOptionsItemSelected` byla volána metoda: 

[![Snímek obrazovky informační zobrazí, když je stisknuté upravit položku](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png)

Když uživatel klepnutím nabídce přetečení **Předvolby** položky nabídky se zobrazí. Obvykle méně běžné akce musí být umístěny v nabídce přetečení &ndash; tento příklad používá v nabídce přetečení pro **Předvolby** protože není tak často používá jako **upravit** a  **Uložit**: 

[![Položka nabídky – snímek obrazovky předvolby, který se zobrazí v nabídce přetečení](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png)

Další informace o Android nabídky najdete v tématu Android Developer [nabídky](https://developer.android.com/guide/topics/ui/menus.html) tématu. 
 



## <a name="related-links"></a>Související odkazy

- [Panel nástrojů typu Lupa (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [Kompatibilita aplikace panelu nástrojů (ukázka)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
