---
title: Úvodní obrazovka
description: Aplikace pro Android trvá delší dobu spuštění, zejména při prvním spuštění aplikace na zařízení. Úvodní obrazovka se může zobrazovat počáteční registrace probíhá na uživatele nebo k označení brandingu.
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/14/2018
ms.openlocfilehash: 6200a04bb4d82174d36a48beab7c63709ac39187
ms.sourcegitcommit: c5bb1045b2f4607dafe3101ad1ea6ade23e44342
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/14/2018
---
# <a name="splash-screen"></a>Úvodní obrazovka

_Aplikace pro Android trvá delší dobu spuštění, zejména při prvním spuštění aplikace na zařízení. Úvodní obrazovka se může zobrazovat počáteční registrace probíhá na uživatele nebo k označení brandingu._


## <a name="overview"></a>Přehled

Aplikace pro Android chvíli o spuštění služby trvat, zejména při prvním spuštění aplikace na zařízení (někdy tento proces se označuje jako _studený start_). Úvodní obrazovka se může zobrazovat spuštění průběh uživateli, nebo se může zobrazit informace o konfiguraci identifikovat a podporovat aplikace.

Tato příručka popisuje jednoho technika, jak implementovat úvodní obrazovky v aplikaci pro Android. Zahrnuje následující kroky:

1.  Vytvoření drawable prostředku pro úvodní obrazovka.

2.  Definování nový motiv, který se zobrazí drawable prostředků.

3.  Přidává se nová aktivita k aplikaci, která se použije jako úvodní obrazovka definované motiv vytvořili v předchozím kroku.

[![Příklad Xamarin logo úvodní obrazovky následuje obrazovky aplikace](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)


## <a name="requirements"></a>Požadavky

Tato příručka předpokládá, že cílem aplikace Android API úrovně 15 (Android 4.0.3) nebo vyšší. Aplikace musí mít také **Xamarin.Android.Support.v4** a **Xamarin.Android.Support.v7.AppCompat** přidán do projektu balíčky NuGet.

Všechny kód a kód XML v této příručce najdete v [SplashScreen](https://developer.xamarin.com/samples/monodroid/SplashScreen) ukázkový projekt této příručce.


## <a name="implementing-a-splash-screen"></a>Implementace úvodní obrazovky

Nejrychlejší způsob, jak vykreslit a zobrazí úvodní obrazovka je vytvořte vlastní motiv a použít ji na aktivitu, pro jehož úvodní obrazovka. Když aktivita vykresleno, načte motiv a drawable prostředků (odkazuje motiv) se vztahuje na pozadí aktivity. Tento přístup zabraňuje potřeba pro vytvoření rozložení souboru.

Úvodní obrazovka je implementovaná jako aktivity, která se zobrazí partnerské drawable, provede všechny inicializacích a spuštění žádné úlohy. Jakmile aplikace má připravili, úvodní obrazovka aktivity spustí hlavní aktivitu a samotné odebere back zásobník aplikací.


### <a name="creating-a-drawable-for-the-splash-screen"></a>Vytváření Drawable pro úvodní obrazovka

Úvodní obrazovka se zobrazí na pozadí na úvodní obrazovce aktivity drawable XML. Je nutné použít bitovou kopii rastrové obrázky (například PNG nebo JPG) pro bitovou kopii k zobrazení.

V této příručce použijeme [seznamu vrstvy](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) na střed obrázek úvodní obrazovky v aplikaci. Následující fragment kódu je příkladem `drawable` pomocí prostředků `layer-list`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

To `layer-list` bude center obrázek úvodní obrazovky **splash.png** na pozadí určeného `@color/splash_background` prostředků.
Umístěte tento soubor **prostředky/drawable** složky (například **Resources/drawable/splash_screen.xml**).

Po vytvoření drawable úvodní obrazovka, dalším krokem je vytvoření motiv pro úvodní obrazovka.


### <a name="implementing-a-theme"></a>Implementace motiv

Pokud chcete vytvořit vlastní motivy pro úvodní obrazovka aktivity, upravit (nebo přidejte) soubor **values/styles.xml** a vytvořte novou `style` element pro úvodní obrazovka. Ukázka **values/style.xml** souboru je uveden níže s `style` s názvem **MyTheme.Splash**:

```xml
<resources>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
  </style>

  <style name="MyTheme" parent="MyTheme.Base">
  </style>

  <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_screen</item>
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowFullscreen">true</item>
  </style>
</resources>
```

**MyTheme.Splash** je velmi spartan &ndash; deklaruje pozadí okna, explicitně odebere záhlaví z okna a deklaruje, že je přes celou obrazovku. Pokud chcete vytvořit úvodní obrazovky, který emuluje uživatelského rozhraní aplikace před aktivity zvýšení kapacity první rozložení, můžete použít `windowContentOverlay` místo `windowBackground` ve vašem definici stylu. V takovém případě musíte také upravit **splash_screen.xml** drawable tak, aby zobrazil emulaci uživatelské rozhraní.


### <a name="create-a-splash-activity"></a>Vytvořit úvodní aktivitu

Nyní potřebujeme novou aktivitu pro Android ke spuštění, který má naše obrázek úvodní a provede všechny spuštění úlohy. Následující kód je příkladem na dokončení úvodní obrazovky implementace:

```csharp
[Activity(Theme = "@style/MyTheme.Splash", MainLauncher = true, NoHistory = true)]
public class SplashActivity : AppCompatActivity
{
    static readonly string TAG = "X:" + typeof(SplashActivity).Name;

    public override void OnCreate(Bundle savedInstanceState, PersistableBundle persistentState)
    {
        base.OnCreate(savedInstanceState, persistentState);
        Log.Debug(TAG, "SplashActivity.OnCreate");
    }

    // Launches the startup task
    protected override void OnResume()
    {
        base.OnResume();
        Task startupWork = new Task(() => { SimulateStartup(); });
        startupWork.Start();
    }

    // Simulates background work that happens behind the splash screen
    async void SimulateStartup ()
    {
        Log.Debug(TAG, "Performing some startup work that takes a bit of time.");
        await Task.Delay (8000); // Simulate a bit of startup work.
        Log.Debug(TAG, "Startup work is finished - starting MainActivity.");
        StartActivity(new Intent(Application.Context, typeof (MainActivity)));
    }
}
```

`SplashActivity` používá explicitně motivu, který byl vytvořen v předchozí části, přepsání výchozí motiv aplikace.
Není nutné načíst rozložení v `OnCreate` jako motiv deklaruje drawable jako na pozadí.

Je důležité nastavit `NoHistory=true` atributů tak, aby aktivity je odebrán, zpět. Aby tlačítko Zpět na zrušení spuštění procesu, můžete také přepsat `OnBackPressed` a mějte ho nedělat nic:

```csharp
public override void OnBackPressed() { }
```

Spuštění pracovních probíhá asynchronně v `OnResume`. To je nezbytné, tak, aby pracovní spuštění zpomalit nebo zpoždění vzhled úvodní obrazovka. Po dokončení práce `SplashActivity` spustí `MainActivity` a uživatel může začít interakci s aplikací.

Tento nový `SplashActivity` je nastavená jako aktivita Spouštěč aplikace nastavením `MainLauncher` atribut `true`. Protože `SplashActivity` je teď Spouštěče aktivity, je nutné upravit `MainActivity.cs`a odeberte `MainLauncher` atribut z `MainActivity`:

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

## <a name="landscape-mode"></a>Režim na šířku

Úvodní obrazovka implementována v předchozích krocích zobrazí správně v režimu na výšku a šířku. V některých případech je ale potřeba mít samostatné úvodní obrazovky pro režimy na výšku a šířku (například pokud obrázek úvodní přes celou obrazovku).

Pro přidání úvodní obrazovka pro šířku, použijte následující kroky:

1. V **prostředky/drawable** složky, přidejte na šířku verzi obrázek úvodní obrazovky, který chcete použít. V tomto příkladu **splash_logo_land.png** je verze na šířku logo, které byl použit v uvedených příkladech (používá lettering bílé místo blue).

2. V **prostředky/drawable** složky, vytvořit na šířku verzi `layer-list` drawable, byla definována dříve (například **splash_screen_land.xml**). V tomto souboru nastaví cestu rastrového obrázku na verzi obrázek úvodní obrazovky na šířku. V následujícím příkladu **splash_screen_land.xml** používá **splash_logo_land.png**:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
      <item>
        <color android:color="@color/splash_background"/>
      </item>
      <item>
        <bitmap
            android:src="@drawable/splash_logo_land"
            android:tileMode="disabled"
            android:gravity="center"/>
      </item>
    </layer-list>
    ```

3.  Vytvořte **prostředky nebo hodnoty krajině** složky, pokud ještě neexistuje.

4.  Přidat soubory **colors.xml** a **style.xml** k **hodnoty krajině** (tyto můžete kopírovat a upravovat z existující **values/colors.xml**a **values/style.xml** soubory).

5.  Upravit **hodnoty krajině/style.xml** tak, aby používala na šířku verze drawable pro `windowBackground`. V tomto příkladu **splash_screen_land.xml** se používá:

    ```xml
    <resources>
      <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
      </style>
        <style name="MyTheme" parent="MyTheme.Base">
      </style>
      <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
        <item name="android:windowBackground">@drawable/splash_screen_land</item>
        <item name="android:windowNoTitle">true</item>  
        <item name="android:windowFullscreen">true</item>  
        <item name="android:windowContentOverlay">@null</item>  
        <item name="android:windowActionBar">true</item>  
      </style>
    </resources>
    ```

6.  Upravit **hodnoty krajině/colors.xml** konfigurace barvy, kterou chcete použít pro verzi úvodní obrazovky na šířku. V tomto příkladu je změnit barvu pozadí úvodní na modrou pro režim na šířku:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
      <color name="primary">#2196F3</color>
      <color name="primaryDark">#1976D2</color>
      <color name="accent">#FFC107</color>
      <color name="window_background">#F5F5F5</color>
      <color name="splash_background">#3498DB</color>
    </resources>
    ```

7.  Sestavte a spusťte aplikaci znovu. Otočení zařízení na šířku režimu, zatímco stále zobrazené úvodní obrazovce. Úvodní obrazovka se změní na verzi na šířku:

    [![Otočení úvodní obrazovky na šířku](splash-screen-images/landscape-splash-sml.png)](splash-screen-images/landscape-splash.png#lightbox)


Všimněte si, že použití úvodní obrazovky na šířku režimu vždy neposkytuje integrované prostředí. Ve výchozím nastavení Android spuštění aplikace v režimu na výšku a přejde se na šířku režimu i v případě, že zařízení je již v režimu na šířku. Výsledkem je pokud je aplikace spuštěná, když je zařízení v režimu na šířku, zařízení krátce zobrazí úvodní obrazovka na výšku a pak animuje otočení z na výšku, které se úvodní obrazovky na šířku. Bohužel nastane tato počáteční přechod na výšku do na šířku i v případě `ScreenOrientation = Android.Content.PM.ScreenOrientation.Landscape` je uveden v příznaky úvodní aktivity. Nejlepší způsob, jak toto omezení obejít je vytvoření obrázek jeden úvodní obrazovky, který vykreslí správně v režimech na výšku a šířku.


## <a name="summary"></a>Souhrn

Tato příručka popsané jeden způsob, jak implementovat úvodní obrazovky v aplikaci Xamarin.Android; Konkrétně použití vlastních motivu spuštění aktivity.


## <a name="related-links"></a>Související odkazy

- [SplashScreen (ukázka)](https://developer.xamarin.com/samples/monodroid/SplashScreen)
- [Drawable seznamu vrstev](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [ Vzory návrhu podstatným - spuštění obrazovky](https://www.google.com/design/spec/patterns/launch-screens.html)
