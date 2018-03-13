---
title: "Kompatibilita panelu nástrojů"
ms.topic: article
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: a17ad79d3f3b537332494fc368c878f2733d5db2
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="toolbar-compatibility"></a>Kompatibilita panelu nástrojů


## <a name="overview"></a>Přehled

Tato část vysvětluje, jak používat `Toolbar` ve verzích systému Android starší než typu Lupa Android 5.0. Pokud vaše aplikace nepodporuje verzi systému Android starší než Android 5.0, můžete tuto část přeskočit. 

Protože `Toolbar` je součástí knihovny podpory Android v7, je možné použít s zařízení se systémem Android 2.1 (API úrovně 7) a vyšší. Ale [kompatibility aplikace podporu knihovna pro Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet musí být nainstalován a kód upravit tak, aby používala `Toolbar` implementace uvedené v této knihovně. Tato část vysvětluje, jak nainstalovat tento NuGet a upravit **ToolbarFun** aplikace z [přidání druhého panelu nástrojů](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) tak, aby běžel na verzích systému Android starší než typu Lupa 5.0.

K úpravě aplikace pro použití kompatibility aplikace verze nástrojů: 

1.  Nastavte minimální a cílové Android verze pro aplikaci.

2.  Nainstalujte balíček NuGet kompatibility aplikace.

3.  Použijte motiv kompatibility aplikace místo předdefinovaného Android motivu.

4.  Upravit `MainActivity` tak, aby ho podtřídy `AppCompatActivity` místo `Activity`. 

Každý z těchto kroků je podrobně vysvětleny v následujících částech.



## <a name="set-the-minimum-and-target-android-version"></a>Nastavit minimální a cílové verzi systému Android

Cílová architektura aplikace musí být nastavena na 21 úroveň rozhraní API nebo větší nebo aplikace nebude správně nasadit. Pokud chybu, jako **nalezen žádný identifikátor prostředku pro atribut 'tileModeX' v balíčku, android,** se při nasazení aplikace, je to proto cílovém Frameworku, který není nastavený na **Android 5.0 (API úrovně 21 - typu Lupa)**  nebo vyšší. 

Nastavte cílové rozhraní úrovně do 21 úroveň rozhraní API nebo vyšší a nastavení úrovně projektu rozhraní API systému Android minimální Android verzi, která je pro podporu aplikace. Další informace o nastavení úrovně rozhraní API systému Android, najdete v části [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md). V `ToolbarFun` příklad, minimální verze Android nastavena na KitKat (4.4 úroveň rozhraní API). 


## <a name="install-the-appcompat-nuget-package"></a>Nainstalujte balíček NuGet kompatibility aplikace

Dál přidejte [kompatibility aplikace podporu knihovna pro Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) balíčku do projektu. V sadě Visual Studio, klikněte pravým tlačítkem na **odkazy** a vyberte **spravovat balíčky NuGet...** . Klikněte na tlačítko **Procházet** a vyhledejte **kompatibility podporu knihovna pro Android v7 aplikace**. Vyberte **Xamarin.Android.Support.v7.AppCompat** a klikněte na tlačítko **nainstalovat**: 

[![Snímek obrazovky kompatibility V7 aplikace balíčku vybraný v balíčcích spravovat balíčky NuGet](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

Při instalaci této NuGet několik dalších balíčcích NuGet nainstalují taky Pokud již nejsou přítomny (například **Xamarin.Android.Support.Animated.Vector.Drawable**, **Xamarin.Android.Support.v4**, a **Xamarin.Android.Support.Vector.Drawable**). Další informace o instalaci balíčků NuGet najdete v tématu [návod: včetně NuGet ve vašem projektu](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 


## <a name="use-an-appcompat-theme-and-toolbar"></a>Použijte motiv kompatibility aplikace a panelu nástrojů

Knihovna kompatibility aplikace obsahuje několik `Theme.AppCompat` motivy, které lze použít na libovolnou verzi systému Android nepodporuje knihovně kompatibility aplikace. `ToolbarFun` Motiv aplikace příklad je odvozený od `Theme.Material.Light.DarkActionBar`, která není k dispozici v systému Android verze starší než typu Lupa. Proto `ToolbarFun` musí být přizpůsobena použít protějšku kompatibility aplikace pro tento motiv `Theme.AppCompat.Light.DarkActionBar`. Navíc vzhledem k tomu `Toolbar` není k dispozici ve verzích systému Android starší než typu Lupa, jsme musí používat verzi kompatibility aplikace `Toolbar`. Proto musíte použít rozložení `android.support.v7.widget.Toolbar` místo `Toolbar`. 


### <a name="update-layouts"></a>Aktualizace rozložení

Upravit **Resources/layout/Main.axml** a nahraďte `Toolbar` element s následující kód XML: 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Upravit **Resources/layout/toolbar.xml** a nahraďte jeho obsah následující kód XML: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>
```

Všimněte si, že `?attr` hodnoty jsou už předponou `android:` (odvolat, který `?` zápis odkazuje na prostředek v aktuální motiv). Pokud `?android:attr` stále používaly tady by Android odkazovat hodnotu atributu ze probíhající platformy spíše než z knihovny kompatibility aplikace. Vzhledem k tomu, že v tomto příkladu `actionBarSize` definované knihovně kompatibility aplikace `android:` předpona je vyřazeno. Podobně `@android:style` se změní na `@style` tak, aby `android:theme` je atribut nastaven na motiv v knihovně kompatibility aplikace &ndash; `ThemeOverlay.AppCompat.Dark.ActionBar` tady je použita motiv místo `ThemeOverlay.Material.Dark.ActionBar`. 


### <a name="update-the-style"></a>Aktualizace styl

Upravit **Resources/values/styles.xml** a nahraďte jeho obsah následující kód XML: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="MyTheme.Base"> </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="windowNoTitle">true</item>
    <item name="windowActionBar">false</item>
    <item name="colorPrimary">#5A8622</item>
    <item name="colorAccent">#A88F2D</item>
  </style>
</resources>
```

Názvy položek a motivu nadřazené v tomto příkladu jsou již s předponou `android:` vzhledem k tomu, že používáme knihovně kompatibility aplikace. Navíc motiv nadřazené se změní na verzi kompatibility aplikace `Light.DarkActionBar`. 



### <a name="update-menus"></a>Aktualizace nabídky

K podpoře starší verze systému Android, knihovně kompatibility aplikace používá vlastních atributů, které zrcadlení atributy `android:` oboru názvů. Ale některé atributy (například `showAsAction` atribut použitý v `<menu>` značky) nejsou k dispozici v systému Android framework na starší zařízení &ndash; `showAsAction` byla zavedena v systému Android API 11, ale není k dispozici v systému Android API 7. Z tohoto důvodu musí použít vlastní obor názvů jako předpona všechny atributy definované knihovna podpory. V nabídce soubory prostředků, názvem oboru názvů `local` je definována pro prefixu `showAsAction` atribut. 

Upravit **Resources/menu/top_menus.xml** a nahraďte jeho obsah následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       local:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       local:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       local:showAsAction="never"
       android:title="Preferences" />
</menu>
```

`local` Obor názvů se přidá tento řádek:

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

`showAsAction` Atribut začíná to `local:` obor názvů místo `android:` 

```csharp
local:showAsAction="ifRoom"
```

Podobně upravit **Resources/menu/edit_menus.xml** a nahraďte jeho obsah následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

Jak tento obor názvů přepínač poskytují podporu pro `showAsAction` atribut na Android verze starší než 11 úroveň rozhraní API? Vlastní atribut `showAsAction` a všechny jeho možné hodnoty jsou zahrnuty v aplikaci při instalaci kompatibility aplikace NuGet. 


## <a name="subclass-appcompatactivity"></a>Podtřída AppCompatActivity

V posledním kroku převod je cílem upravit `MainActivity` tak, aby se podtřídou třídy `AppCompactActivity`. Upravit **MainActivity.cs** a přidejte následující `using` příkazy: 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

To deklaruje `Toolbar` jako verze kompatibility aplikace `Toolbar`. V dalším kroku změnit definici třídy `MainActivity`: 

```csharp
public class MainActivity : AppCompatActivity
```

Nastavte na panelu akcí na verzi kompatibility aplikace `Toolbar`, nahraďte volání `SetActionBar` s `SetSupportActionBar`. V tomto příkladu je název změní také k označení, že verze kompatibility aplikace `Toolbar` používá:

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

Nakonec změňte Android minimální úroveň na předběžné typu Lupa hodnotu, která má být podporován (například 19 rozhraní API). 

Sestavení aplikace a spusťte jej na předběžné typu Lupa zařízení nebo emulátoru systému Android. Následující snímek obrazovky ukazuje verze kompatibility aplikace **ToolbarFun** na Nexus 4 spuštěný KitKat (rozhraní API 19): 

[![Úplné snímek obrazovky aplikace spuštěné na zařízení KitKat, jsou uvedeny oba panely nástrojů](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

Když knihovna kompatibility aplikace se používá, motivy není nutné přepnout na základě Android verze &ndash; knihovně kompatibility aplikace umožňuje zajistit konzistentní prostředí napříč všechny podporované verze systému Android. 




## <a name="related-links"></a>Související odkazy

- [Panel nástrojů typu Lupa (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [Kompatibilita aplikace panelu nástrojů (ukázka)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
