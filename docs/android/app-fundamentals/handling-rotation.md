---
title: Zpracování otočení
description: Toto téma popisuje, jak zpracovat změny orientace zařízení v Xamarin.Android. Vysvětluje způsob práce se systémem Android prostředků automaticky načíst prostředky pro orientaci určitého zařízení, jak programově zpracování orientaci změny.
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1cdc7928c45b99cdd8c8149b3ae9b06e790deeca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30768893"
---
# <a name="handling-rotation"></a>Zpracování otočení

_Toto téma popisuje, jak zpracovat změny orientace zařízení v Xamarin.Android. Vysvětluje způsob práce se systémem Android prostředků automaticky načíst prostředky pro orientaci určitého zařízení, jak programově zpracování orientaci změny._


## <a name="overview"></a>Přehled

Protože otáčejí snadno mobilní zařízení, je předdefinovaný otočení standardní funkce v mobilní operační systémy. Android poskytuje sofistikované rozhraní pro práci s otočení v aplikacích, zda uživatelské rozhraní je vytvořen deklarativně v kódu XML nebo z programu v kódu. Při zpracování automaticky deklarativní rozložení změny na otočený zařízení, aplikace využívat výhod úzkou integraci se systémem Android prostředků. Programová rozložení musí být zpracován změny ručně. To umožňuje lepší kontrolu za běhu, ale za cenu další práci pro vývojáře. Aplikace také můžete vyjádření výslovného nesouhlasu restartování aktivity a provést ruční Kontrola změn orientace.

Tato příručka prozkoumá v následujících tématech orientaci:

-   **Deklarativní otočení rozložení** &ndash; použití systému Android prostředků k vytvoření aplikace využívající technologii orientaci, včetně toho, jak načíst rozložení a drawables pro konkrétní orientace.

-   **Programová otočení rozložení** &ndash; přidání ovládacích prvků prostřednictvím kódu programu, jakož i způsob zpracování orientaci změny ručně.


## <a name="handling-rotation-declaratively-with-layouts"></a>Zpracování otočení deklarativně s rozložení

Zahrnutím souborů ve složkách, které následují zásady vytváření názvů Android automaticky načte příslušné soubory, když se změní orientaci.
To zahrnuje podporu pro:

-   *Rozložení prostředky* &ndash; určující soubory, které rozložení jsou zvětšený pro každý orientace.

-   *Drawable prostředky* &ndash; určující, které drawables jsou načtena pro každý orientace.


### <a name="layout-resources"></a>Rozložení prostředky

Ve výchozím nastavení, soubory XML Android (AXML) součástí **prostředky, rozvržení** složky se používá pro vykreslování zobrazení pro aktivitu. Tato složka prostředků slouží pro orientaci na výšku a šířku, pokud jsou k dispozici žádné další rozložení prostředky speciálně pro šířku. Vezměte v úvahu strukturu projektu vytvořen výchozí šablona projektu:

[![Výchozí projekt šablony struktury](handling-rotation-images/00.png)](handling-rotation-images/00.png#lightbox)

Tento projekt vytvoříte jednoduchou **Main.axml** v soubor **prostředky, rozvržení** složky. Když aktivity `OnCreate` metoda je volána, je zvýšení kapacity zobrazení definované v **Main.axml,** který deklaruje tlačítka, jak je znázorněno v následující soubor XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:orientation="vertical"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
<Button  
  android:id="@+id/myButton"
  android:layout_width="fill_parent" 
  android:layout_height="wrap_content" 
  android:text="@string/hello"/>
</LinearLayout>
```

Pokud zařízení otočen na šířku, aktivity na `OnCreate` metoda je volána znovu a stejné **Main.axml** zvětšený souboru, jak ukazuje následující snímek obrazovky:

[![Stejné obrazovky, ale v orientaci na šířku](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png#lightbox)


#### <a name="orientation-specific-layouts"></a>Specifické pro orientaci rozložení

Kromě složce rozložení (což výchozí nastavení je na výšku a můžete také být explicitně s názvem *rozložení port* zahrnutím složku s názvem `layout-land`), aplikace můžete definovat zobrazení, musí v šířku bez žádný kód změny.

Předpokládejme, že **Main.axml** soubor obsahoval následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is portrait"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

Pokud do složky s názvem rozložení krajině, který obsahuje další **Main.axml** soubor je přidán do projektu, nafouknutí rozložení v šířku nyní způsobí Android načítání nově přidaný **Main.axml.** Zvažte šířku verzi **Main.axml** soubor, který obsahuje následující kód (pro jednoduchost, tato konfigurace XML je podobná výchozí verze na výšku kód, ale používá jiný řetězec v `TextView`):

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is landscape"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

Spuštění tohoto kódu a otáčení zařízení z výšky na šířku nové načítání XML, ukazuje, jak je uvedeno níže:

[![Snímky obrazovky na výšku a šířku tisk na výšku režimu](handling-rotation-images/02.png)](handling-rotation-images/02.png#lightbox)


### <a name="drawable-resources"></a>Drawable prostředky

Během otáčení Android pracuje s drawable prostředky podobně k prostředkům rozložení. V takovém případě systém získá drawables z **prostředky/drawable** a **prostředky/drawable krajině** složek, v uvedeném pořadí.

Předpokládejme například, že projekt zahrnuje obrázek s názvem Monkey.png v **prostředky/drawable** složku, kde drawable je na něj odkazovat z `ImageView` v kódu XML takto:

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

Další Předpokládejme, že jinou verzi **Monkey.png** je zahrnuta v části **prostředky/drawable krajině**. Jenom jako se soubory rozložení, když je zařízení otáčet, drawable změn pro danou orientaci, jak je uvedeno níže:

[![Jinou verzi Monkey.png ukazuje na výšku a šířku režimy](handling-rotation-images/03.png)](handling-rotation-images/03.png#lightbox)


## <a name="handling-rotation-programmatically"></a>Zpracování otočení prostřednictvím kódu programu

Někdy jsme definovali rozložení v kódu. Tato situace může nastat z různých důvodů, včetně technická omezení, developer předvoleb atd. Když jsme přidání ovládacích prvků prostřednictvím kódu programu, aplikace musí ručně účtu pro orientace zařízení, které se zpracovávají automaticky, když budeme používat prostředky XML.


### <a name="adding-controls-in-code"></a>Přidání ovládacích prvků v kódu

Aplikace potřebuje k přidávání ovládacích prvků prostřednictvím kódu programu, proveďte následující kroky:

-  Vytvořte rozložení.
-  Nastavit rozložení parametrů.
-  Vytvořte ovládací prvky.
-  Nastavit parametry rozložení ovládacího prvku.
-  Přidání ovládacích prvků do rozložení.
-  Nastavte rozložení jako zobrazení obsahu.

Představte si třeba uživatelské rozhraní, který se skládá z jedné `TextView` přidat do ovládacího prvku `RelativeLayout`, jak je znázorněno v následujícím kódu.

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
        
  // create TextView control
  var tv = new TextView (this);

  // set TextView's LayoutParameters
  tv.LayoutParameters = layoutParams;
  tv.Text = "Programmatic layout";

  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

Tento kód vytvoří instanci `RelativeLayout` třídy a nastaví její `LayoutParameters` vlastnost. `LayoutParams` Třída představuje způsob pro Android zapouzdřením, jak jsou opakovaně použitelné způsobem umístěný ovládací prvky. Po vytvoření instance rozložení ovládací prvky můžete vytvořit a přidat k němu. Ovládací prvky mít také `LayoutParameters`, například `TextView` v tomto příkladu. Po `TextView` je vytvořen, jejím přidáním do `RelativeLayout` a nastavení `RelativeLayout` jako výsledky zobrazení obsahu v zobrazení aplikace `TextView` znázorněné:

[![Tlačítko čítač přírůstek zobrazené v režimech na výšku a šířku](handling-rotation-images/04.png)](handling-rotation-images/04.png#lightbox)


### <a name="detecting-orientation-in-code"></a>Zjišťování orientaci v kódu

Pokud se aplikace pokusí načíst různé uživatelské rozhraní pro každou orientaci při `OnCreate` nazývá (proběhne pokaždé, když zařízení otočen), musí zjistit orientaci a pak můžete načíst kód požadované uživatelské rozhraní. Android má třídu s názvem `WindowManager`, který slouží k určení aktuální otáčení zařízení prostřednictvím `WindowManager.DefaultDisplay.Rotation` vlastnost, jak je uvedeno níže:

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
                        
  // get the initial orientation
  var surfaceOrientation = WindowManager.DefaultDisplay.Rotation;
  // create layout based upon orientation
  RelativeLayout.LayoutParams tvLayoutParams;
                
  if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
  } else {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    tvLayoutParams.LeftMargin = 100;
    tvLayoutParams.TopMargin = 100;
  }
                        
  // create TextView control
  var tv = new TextView (this);
  tv.LayoutParameters = tvLayoutParams;
  tv.Text = "Programmatic layout";
        
  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

Nastaví tento kód `TextView` být umístěné 100 pixelů z horní pravé obrazovce automaticky animace do nového rozložení, když otočen na šířku, jak je vidět tady:

[![Zobrazení stavu se zachová, i mezi režimy na výšku a šířku](handling-rotation-images/05.png)](handling-rotation-images/05.png#lightbox)


### <a name="preventing-activity-restart"></a>Brání restartování aktivity

Kromě všechno ve zpracování `OnCreate`, aplikace můžete také zabránit aktivity musí restartovat, když se změní orientaci nastavením `ConfigurationChanges` v `ActivityAttribute` následujícím způsobem:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```
Nyní když zařízení otočen, není restartovat aktivity. Ručně v tomto případě zvládnout Změna orientace, můžete přepsat aktivitu `OnConfigurationChanged` metoda a určení orientaci z `Configuration` objekt, který se předává v, stejně jako novou implementací aktivity níže:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
public class CodeLayoutActivity : Activity
{
  TextView _tv;
  RelativeLayout.LayoutParams _layoutParamsPortrait;
  RelativeLayout.LayoutParams _layoutParamsLandscape;
                
  protected override void OnCreate (Bundle bundle)
  {
    // create a layout
    // set layout parameters
    // get the initial orientation

    // create portrait and landscape layout for the TextView
    _layoutParamsPortrait = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
                
    _layoutParamsLandscape = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    _layoutParamsLandscape.LeftMargin = 100;
    _layoutParamsLandscape.TopMargin = 100;
                        
    _tv = new TextView (this);
                        
    if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
      _tv.LayoutParameters = _layoutParamsPortrait;
    } else {
      _tv.LayoutParameters = _layoutParamsLandscape;
    }
                        
    _tv.Text = "Programmatic layout";
    rl.AddView (_tv);
    SetContentView (rl);
  }
                
  public override void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
  {
    base.OnConfigurationChanged (newConfig);
                        
    if (newConfig.Orientation == Android.Content.Res.Orientation.Portrait) {
      _tv.LayoutParameters = _layoutParamsPortrait;
      _tv.Text = "Changed to portrait";
    } else if (newConfig.Orientation == Android.Content.Res.Orientation.Landscape) {
      _tv.LayoutParameters = _layoutParamsLandscape;
      _tv.Text = "Changed to landscape";
    }
  }
}
```

Zde `TextView's` rozložení parametry jsou inicializovány pro na šířku i na výšku. Třída proměnné uchovávat spolu s parametry, `TextView` samostatně, protože aktivity se znovu nevytvoří při změně orientace. Kód stále používá `surfaceOrientartion` v `OnCreate` nastavit počáteční rozložení pro `TextView`. Potom `OnConfigurationChanged` zpracovává všechny následné rozložení změny.

Když jsme aplikaci spustit, načte Android změny uživatelského rozhraní, jak probíhá otáčení zařízení a nerestartuje aktivity.


## <a name="preventing-activity-restart-for-declarative-layouts"></a>Brání restartování aktivity pro deklarativní rozložení

Aktivita restartování způsobené otáčení zařízení je možné zabránit také když jsme definovali rozložení v kódu XML. Například můžeme můžete tuto metodu použijte, pokud chcete zabránit restartování aktivity (z důvodů výkonu možná) a není třeba načíst nové prostředky pro různé orientace.

K tomu, jsme postupujte stejným způsobem, který používáme programový rozložení. Jednoduše nastavit `ConfigurationChanges` v `ActivityAttribute`, jako jsme to udělali `CodeLayoutActivity` dříve. Kód, který potřebuje ke spuštění pro změnu orientace lze implementovat znovu `OnConfigurationChanged` metoda.


## <a name="maintaining-state-during-orientation-changes"></a>Zachování stavu během změny orientace

Zda zpracování otočení deklarativně nebo programově, by měla implementovat všechny aplikace pro Android se stejné techniky pro správu stavu, kdy se změní orientace zařízení. Správa stavu je důležité, protože systém se restartuje spuštěná aktivita při otočení zařízení se systémem Android. Android tomu snadno načíst alternativní prostředky, například rozložení a drawables, které byly navrženy speciálně pro konkrétní orientace. Po restartování, aktivity zruší všechny přechodného stavu, ve kterém můžou mít uložené v proměnné místní třídy. Proto pokud aktivita závisí na stavu, musí zachovat jeho stavu na úrovni aplikace. Aplikace je potřeba zpracovat ukládání a obnovení stavu všechny aplikace, která chce zachovat napříč změny orientace.

Další informace o zachování stavu v Android, najdete v části [životního cyklu aktivity](~/android/app-fundamentals/activity-lifecycle/index.md) průvodce.


## <a name="summary"></a>Souhrn

Tento článek popsané postupy pro práci s otočení používá integrované funkce pro Android. Nejprve vysvětlené použití systému Android prostředků k vytvoření aplikace využívající orientace. Potom prezentovaná přidání ovládacích prvků v kódu a také způsob zpracování orientaci změny ručně.



## <a name="related-links"></a>Související odkazy

- [Otočení ukázku (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/RotationDemo/)
- [Životní cyklus aktivity](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Zpracování změny v modulu Runtime](http://developer.android.com/guide/topics/resources/runtime-changes.html)
- [Změna orientace rychlé obrazovky](http://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)
