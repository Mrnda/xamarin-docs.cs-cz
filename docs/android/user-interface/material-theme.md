---
title: "Podstatným motiv"
description: "Postup motiv aplikace Xamarin.Android pomocí materiálu motiv"
ms.topic: article
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 479abf7fef695be156d4447592bc59dceabe3f03
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="material-theme"></a>Podstatným motiv

*Podstatným motiv* je styl rozhraní uživatele, který určuje vzhled a chování zobrazení a aktivit od verze Android 5.0 (typu Lupa). Podstatným motiv je integrovaná do Android 5.0, aby byl použit podle uživatelského rozhraní systému a aplikací. Podstatným motiv není "motivu", v tom smyslu systémové vzhled možnost, která může dynamicky uživatel z nabídky nastavení. Místo toho materiálu motiv můžete představit jako sadu související integrovaných základní stylů, které můžete použít k přizpůsobení vzhledu a chování vaší aplikace.

Android poskytuje tři materiálu motiv charakteristikami:

-  `Theme.Material` &ndash; Tmavý verzi materiálu motiv; Toto je výchozí příchuť v systému Android 5.0.

-  `Theme.Material.Light` &ndash; Světla verze materiálu motivu.

-  `Theme.Material.Light.DarkActionBar` &ndash; Světla verze materiálu motivu, ale s panel tmavý akcí.

Příklady těchto typů materiálu motiv se zobrazí tady:

[![Příklad snímky obrazovky tmavý motiv, motiv světlý a panelu akcí světlý motiv](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

Můžete odvozujete od materiálu motiv se vytvořte vlastní motiv přepsání některé nebo všechny atributy barvy. Například můžete vytvořit motiv, který je odvozen od `Theme.Material.Light`, ale přepíše barvu panelu aplikace tak, aby odpovídala barva vaší značkou. Můžete také styl jednotlivých zobrazení; Například můžete vytvořit styl [zobrazení karty aplikace](~/android/user-interface/controls/card-view.md) který má více zaokrouhlené rozích a používá tmavšího barvu pozadí.

Pro celou aplikaci můžete použít jeden motiv, nebo můžete použít různé motivy pro různé obrazovky (aktivity) v aplikaci. Ve výše uvedené snímky obrazovky například jenom jedna aplikace používá jiný motiv pro každou aktivitu k předvedení Vestavěná barevná schémata. Přepínací tlačítka Přepnout aplikace na různé aktivity a v důsledku toho zobrazit různé motivy.

Protože materiálu motiv je podporována pouze v systému Android 5.0 a novější, nemůžete použít ho (nebo vlastní motiv odvozených z motivu materiálů) pro motiv vaší aplikace pro spuštění v dřívějších verzích systému Android. Ale můžete nakonfigurovat vaše aplikace využívat materiálu motiv na zařízeních Android 5.0 a řádně vrátit zpět k starší motiv při spuštění ve starších verzích Android (najdete v článku [kompatibility](#compatibility) tohoto článku podrobnosti).


## <a name="requirements"></a>Požadavky

Používat nové funkce systému Android 5.0 materiálu motiv v aplikace založené na Xamarinu se vyžaduje následující text:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 nebo novější musí být nainstalovaná a nakonfigurovaná s Visual Studio nebo Visual Studio for Mac. 

-  **Sady SDK pro Android** &ndash; Android 5.0 (21 rozhraní API) nebo novější musí být nainstalován prostřednictvím Android SDK Manager.

-  **Java JDK 1.8** &ndash; JDK 1.7 lze použít, pokud je konkrétně zaměřen API úroveň 23 a starší. JDK 1.8 je k dispozici z [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Naučte se konfigurovat projekt aplikace Android 5.0, najdete v tématu [nastavení nahoru Android 5.0 projektu](~/android/platform/lollipop.md).


## <a name="using-the-built-in-themes"></a>Pomocí předdefinované motivy

Nejjednodušší způsob, jak používat motiv materiálu je konfigurace aplikace použít předdefinované motiv bez úprav. Pokud nechcete, aby explicitně nakonfigurovat motiv, vaše aplikace bude použita výchozí `Theme.Material` (tmavým motivem). Pokud vaše aplikace obsahuje jenom jedna aktivita, můžete nakonfigurovat motiv na úrovni aplikace. Pokud vaše aplikace obsahuje více aktivit, můžete nakonfigurovat motiv na úrovni aplikace tak, aby ji používá stejný motiv mezi všechny aktivity, nebo můžete přiřadit různé motivy různé aktivity. Následující části popisují postup konfigurace motivy na úrovni aplikace a na úrovni aktivity.


### <a name="theming-an-application"></a>Tvorba motivů aplikace

Ke konfiguraci celé aplikace pro použití příchuť materiálu motiv, nastavte `android:theme` atribut uzlu aplikace v **AndroidManifest.xml** na jednu z následujících akcí:

-  `@android:style/Theme.Material` &ndash; Tmavý motiv.

-  `@android:style/Theme.Material.Light` &ndash; Motiv světlý.

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; Motiv světlý panel tmavý akcí.

Následující příklad konfiguruje aplikaci *Moje aplikace* používat motiv světlý:

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

Alternativně můžete nastavit aplikaci `Theme` atribut **AssemblyInfo.cs** (nebo **Properties.cs**). Příklad:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

Když je aplikace motiv nastavená na `@android:style/Theme.Material.Light`, každá aktivita v *Moje aplikace* se zobrazí pomocí `Theme.Material.Light`.


### <a name="theming-an-activity"></a>Tvorba motivů aktivitu

Pro motiv aktivitu, můžete přidat `Theme` nastavení `[Activity]` atribut nad deklaraci vaše aktivity a přiřaďte `Theme` k příchuť materiálu motiv, který chcete použít. Následující příklad motivy aktivitu s `Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

Další aktivity v této aplikace bude používat výchozí `Theme.Material` tmavý barevné schéma (nebo, pokud nakonfigurované nastavení motiv aplikace).

<a name="customtheme" />

## <a name="using-custom-themes"></a>Použití vlastní motivy

Můžete vylepšit vaší značkou tak, že vytvoříte vlastní motiv styly aplikace s vaší značkou&rsquo;s barvy. Pokud chcete vytvořit vlastní motiv, definujete nový styl, který je odvozen z předdefinovaných příchuť materiálu motiv, přepsání barva atributy, které chcete změnit. Například můžete definovat vlastní motiv, který je odvozen od `Theme.Material.Light.DarkActionBar` a změní barvu pozadí obrazovky na béžová místo prázdné.

Podstatným motiv zveřejňuje následující atributy rozložení pro přizpůsobení:

-  `colorPrimary` &ndash; Barva panelu aplikace.

-  `colorPrimaryDark` &ndash; Barva stavového řádku a řádky kontextové aplikace; To je obvykle tmavý verze `colorPrimary`.

-  `colorAccent` &ndash; Barva ovládacích prvků uživatelského rozhraní, jako je například zaškrtávací políčka, přepínače a úpravy polí.

-  `windowBackground` &ndash; Barva pozadí.

-  `textColorPrimary` &ndash; Barva textu uživatelského rozhraní na panelu aplikace.

-  `statusBarColor` &ndash; Barva stavový řádek.

-  `navigationBarColor` &ndash; Barva navigačním panelu.

V následujícím diagramu jsou označeny tyto oblasti obrazovky:

[![Diagram atributy a jejich přidružené obrazovky oblasti](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

Ve výchozím nastavení `statusBarColor` je nastaven na hodnotu `colorPrimaryDark`. Můžete nastavit `statusBarColor` na plnou barvou, nebo ji nastavte na `@android:color/transparent` na průhledný stavový řádek. Na navigačním panelu můžete také provést transparentní nastavením `navigationBarColor` k `@android:color/transparent`.


### <a name="creating-a-custom-app-theme"></a>Vytváření vlastní aplikaci motiv

Vytvoření a úprava souborů v můžete vytvořit vlastní aplikaci motiv **prostředky** složce projektu aplikace. K úpravě stylu vaší aplikace pomocí vlastní motiv, použijte následující postup:

-   Vytvoření **colors.xml** souboru v **prostředky nebo hodnotami** &mdash; použít tento soubor definovat vaše vlastní motiv barvy. Například můžete vložte následující kód do **colors.xml** můžete začít:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   Upravte soubor příklad zadat názvy a barvu kódy pro barvu prostředky, které budete používat ve vaší vlastní motiv.

-   Vytvoření **prostředky nebo hodnoty v21** složky. V této složce vytvořte **styles.xml** souboru:

    [![Umístění styles.xml ve složce prostředky nebo hodnoty 21.xml](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    Všimněte si, že **prostředky nebo hodnoty v21** je specifická pro Android 5.0 &ndash; starší verze Android nebude číst soubory v této složce.

-   Přidat `resources` uzlu **styles.xml** a definovat `style` uzel s názvem vlastní motiv. Zde je ukázka, **styles.xml** soubor, který definuje *MyCustomTheme* (odvozený z integrovaného `Theme.Material.Light` styl motiv):

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

-   V tomto okamžiku aplikaci, která používá *MyCustomTheme* se zobrazí stock `Theme.Material.Light` motiv bez úpravy:

    [![Vlastní motiv vzhled před přizpůsobení](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

-   Přidat přizpůsobení barev **styles.xml** definováním barvy rozložení atributů, které chcete změnit. Například změnit barvu panelu aplikace na `my_blue` a změnit barvu ovládacích prvků uživatelského rozhraní pro `my_purple`, přidejte přepsání barva **styles.xml** , odkazovat na prostředky barva nakonfigurované v **colors.xml**:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Override the app bar color -->
        <item name="android:colorPrimary">@color/my_blue</item>
        <!-- Override the color of UI controls -->
        <item name="android:colorAccent">@color/my_purple</item>
    </style>
</resources>
```

Tyto změny zavedené aplikace, která používá *MyCustomTheme* se zobrazí barvu panelu aplikace v `my_blue` a ovládacích prvků uživatelského rozhraní v `my_purple`, ale použít `Theme.Material.Light` everywhere else barevném schématu:

[![Vlastní motiv vzhled po přizpůsobení](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

V tomto příkladu *MyCustomTheme* vypůjčí barvy z `Theme.Material.Light` pozadí barvu, stavový řádek a barvy textu, ale změní barvu panelu aplikace `my_blue` a nastaví barvu přepínače `my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>Vytvoření vlastního zobrazení styl

Android 5.0 také umožňuje styl jednotlivých zobrazení. Po vytvoření **colors.xml** a **styles.xml** (jak je popsáno v předchozí části), můžete přidat styl zobrazení pro **styles.xml**.
K formátování jednotlivých zobrazení, použijte následující postup:

-   Upravit **Resources/values-v21/styles.xml** a přidejte `style` uzel s názvem vašeho stylu vlastní zobrazení. Nastavit atributy vlastních barev pro zobrazení v rámci to `style` uzlu. Chcete-li například vytvořit vlastní [zobrazení karty aplikace](~/android/user-interface/controls/card-view.md) styl, který má více zaokrouhlené rozích a používá `my_blue` barvu pozadí karty přidat `style` uzlu **styles.xml** (uvnitř `resources`uzel) a nakonfigurujte pozadí barvy a rohu radius:

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   V rozložení, nastavte `style` atribut pro toto zobrazení tak, aby odpovídaly vlastní styl název, který jste vybrali v předchozím kroku. Příklad:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

Na následujícím snímku obrazovky představuje příklad, výchozí `CardView` (zobrazené na levé straně) jako ve srovnání s `CardView` , má stylem vlastní `CardView.MyBlue` motiv (zobrazené na pravé straně):

[![Příklady výchozí zobrazení karty aplikace a zobrazení karty vlastní aplikace](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

V tomto příkladu vlastní `CardView` se zobrazí s barvu pozadí `my_blue` a protokolu radius 18dp rohu.


## <a name="compatibility"></a>Kompatibilita

K úpravě stylu aplikace tak, aby ji používá motiv materiály v systému Android 5.0 ale automaticky vrátí u starší verze Android styl klesající kompatibilní, použijte následující postup:

-   Definovat vlastní motiv v **Resources/values-v21/styles.xml** která je odvozena od styl materiálu motivu. Příklad:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   Definovat vlastní motiv v **Resources/values/styles.xml** které pochází ze starší motiv, ale používá stejný název motivu, jak je uvedeno výše. Příklad:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   V **AndroidManifest.xml**, konfigurace aplikace s názvem vlastní motiv. 
    Příklad:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   Alternativně můžete styl konkrétní aktivitu pomocí vlastní motiv:

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

Pokud vaše motiv používá barev definovaných v **colors.xml** souboru, je nutné umístit tento soubor **prostředky nebo hodnotami** (místo **prostředky nebo hodnoty v21**) tak, aby obě verze vlastní motiv má přístup k definicím barev.

Když aplikace běží na zařízení se systémem Android 5.0, se bude používat zadaný v definici motiv **Resources/values-v21/styles.xml**. Při spuštění této aplikace na zařízení se systémem Android starší, ji budou automaticky vrátit zpět k definici motiv, který je zadaný v **Resources/values/styles.xml**.

Další informace o kompatibilitě motiv s Android starší verze najdete v tématu [alternativní prostředky](~/android/app-fundamentals/resources-in-android/alternate-resources.md).

## <a name="summary"></a>Souhrn

Tento článek se zavedl nový motiv materiálu uživatelské rozhraní styl zahrnuté v systému Android 5.0 (typu Lupa). Je popsané tři předdefinovaných typů motiv materiály, které můžete použít k úpravě stylu vaší aplikace, je popsané postupy vytvořte vlastní motiv pro branding vaší aplikace a poskytuje příklad toho, jak pro motiv jednotlivých zobrazení. Nakonec v tomto článku Vysvětlení najdete postup používání materiálu motiv v aplikaci při zachování klesající kompatibility se staršími verzemi systému Android.



## <a name="related-links"></a>Související odkazy

- [ThemeSwitcher (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [Úvod do typu Lupa](~/android/platform/lollipop.md)
- [CardView](~/android/user-interface/controls/card-view.md)
- [Alternativní prostředky](~/android/app-fundamentals/resources-in-android/alternate-resources.md)
- [Android L Developer Preview](http://developer.android.com/preview/index.html)
- [Podstatným návrhu](http://developer.android.com/preview/material/index.html)
- [Principy podstatným návrhu](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
- [Zachování kompatibility](http://developer.android.com/preview/material/compatibility.html)
